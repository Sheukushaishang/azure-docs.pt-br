---
title: Criar uma VM de uma VM gerenciada no Azure | Microsoft Docs
description: "Crie uma máquina virtual do Windows a partir de uma imagem de VM gerenciada generalizada usando o Azure PowerShell no modelo de implantação do Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/7/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: e308b75581ca5372a4de168895f2eeb140a69a78
ms.contentlocale: pt-br
ms.lasthandoff: 05/17/2017


---
# <a name="create-a-vm-from-a-generalized-managed-vm-image"></a>Criar uma VM a partir de uma imagem de VM gerenciada e generalizada

Você pode criar várias VMs de uma imagem de VM gerenciada no Azure. Uma imagem de VM gerenciada contém as informações necessárias para criar uma VM, incluindo o sistema operacional e os discos de dados. Os VHDs que formam a imagem, incluindo os discos do sistema operacional e quaisquer discos de dados, são armazenados como discos gerenciados. 

Uma VM generalizada teve todas suas informações de conta pessoal removidas usando [Sysprep](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Você pode criar uma VM generalizada executando o Sysprep em uma VM local e então [carregando o VHD no Azure](upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), ou executando o Sysprep em uma VM do Azure existente e então [capturando uma imagem da VM](capture-image-resource.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="prerequisites"></a>Pré-requisitos

Você já precisa ter [criado uma imagem de VM gerenciada](capture-image-resource.md) a ser usada para criação da nova VM. 

Verifique se você tem a versão mais recente do módulo AzureRM.Compute do PowerShell. Execute o comando a seguir para instalá-lo.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Para saber mais, confira [Azure PowerShell Versioning](/powershell/azure/overview) (Controle de versão do Azure PowerShell).



## <a name="collect-information-about-the-image"></a>Coletar informações sobre a imagem

Primeiramente, precisamos coletar informações básicas sobre a imagem e criar uma variável para ela. Este exemplo usa uma imagem de VM gerenciada chamada **myImage**, que está no grupo de recursos **myResourceGroup** no local **Centro-Oeste dos EUA**. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Criar uma rede virtual
Crie a vNet e a sub-rede da [rede virtual](../../virtual-network/virtual-networks-overview.md).

1. Crie a sub-rede. Este exemplo cria uma sub-rede chamada **mySubnet** com um prefixo de endereço de **10.0.0.0/24**.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Crie a rede virtual. Este exemplo cria uma rede virtual chamada **myVnet** com o prefixo de endereço de **10.0.0.0/16**.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Criar um endereço IP público e um adaptador de rede

Para habilitar a comunicação com a máquina virtual na rede virtual, são necessários um [endereço IP público](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) e um adaptador de rede.

1. Criar um endereço IP público. Este exemplo cria um endereço IP público chamado **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Crie a NIC. Este exemplo cria uma NIC chamada **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Criar o grupo de segurança de rede e uma regra RDP

Para fazer logon em sua VM usando RDP, é preciso ter uma NSG (regra de segurança de rede) que permita acesso de RDP na porta 3389. 

Este exemplo cria um NSG chamado **myNsg** que contém uma regra chamada **myRdpRule** que permite tráfego RDP pela porta 3389. Para obter mais informações sobre NSGs, consulte [Abrir portas para uma VM no Azure usando PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Criar uma variável para a rede virtual

Crie uma variável para a rede virtual concluída. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>Obter as credenciais para a VM

O cmdlet a seguir abrirá uma janela na qual você vai inserir um novo nome de usuário e senha para usar como a conta de administrador local para acessar remotamente a VM. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>Definir variáveis para o nome da VM, o nome do computador e o tamanho da VM

1. Crie variáveis para o nome da VM e do computador. Este exemplo define o nome da VM como **myVM** e o nome do computador como **myComputer**.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Defina o tamanho da máquina virtual. Este exemplo cria a VM de tamanho **Standard_DS1_v2**. Veja a documentação sobre os [tamanhos de VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) para obter mais informações.

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Adicione o nome e o tamanho da VM à configuração da VM.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>Definir a imagem da VM como imagem de origem para a nova VM

Defina a imagem de origem usando a ID da imagem da VM gerenciada.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>Defina a configuração do SO e adicione a NIC.

Insira o tipo de armazenamento (PremiumLRS ou StandardLRS) e o tamanho do disco do SO. Este exemplo define o tipo de conta como **PremiumLRS**, o tamanho do disco como **128 GB** e o cache do disco como **ReadWrite**.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Criar a VM

Crie a nova VM usando a configuração que criamos e armazenamos na variável **$vm**.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>Verificar se a VM foi criada
Ao concluir, você deverá ver a VM recém-criada no [portal do Azure](https://portal.azure.com) em **Navegar** > **Máquinas virtuais** ou usando os seguintes comandos do PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Próximas etapas
Para gerenciar sua nova máquina virtual com o Azure PowerShell, consulte [Criar e gerenciar VMs do Windows com o módulo do Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


