---
title: "Preparar destino (Físico para Azure) | Microsoft Docs"
description: "Este artigo descreve como preparar seu ambiente do Azure para iniciar a replicação de servidores físicos executando Windows ou Linux no Azure."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.translationtype: Human Translation
ms.sourcegitcommit: b7f26df96ddedb579cf5d9d20fee6b2599e762e0
ms.openlocfilehash: 9dd4e9007da6cf276f976e2f3f25305286830544
ms.contentlocale: pt-br
ms.lasthandoff: 02/13/2017

---

# <a name="prepare-target-vmware-to-azure"></a>Preparar destino (VMware para Azure)
> [!div class="op_single_selector"]
> * [Máquinas Virtuais VMware](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Servidores físicos](./site-recovery-prepare-target-physical-to-azure.md)

Este artigo descreve como preparar seu ambiente do Azure para iniciar a replicação de servidores físicos (x64) executando Windows ou Linux no Azure.

## <a name="prerequisites"></a>Pré-requisitos

O artigo pressupõe o seguinte:
- Você criou um Cofre dos Serviços de Recuperação para proteger seus servidores físicos. Você pode criar um Cofre dos Serviços de Recuperação no [Portal do Azure](http://portal.azure.com "Portal do Azure").
- Você [configurou seu ambiente local](./site-recovery-set-up-physical-to-azure.md) para replicar servidores físicos no Azure.

## <a name="prepare-target"></a>Preparar o destino

Depois de concluir a **Etapa 1: Selecionar meta de proteção** e a **Etapa 2: Preparar origem**, você segue para a **Etapa 3: Destino**

![Preparar o destino](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Assinatura:** no menu suspenso, escolha a assinatura na qual você quer replicar seus servidores físicos.
2. **Modelo de Implantação:** escolha o modelo de implantação (Clássico ou Resource Manager)

Com base no modelo de implantação escolhido, uma validação é executada para garantir que você tenha pelo menos uma conta de armazenamento compatível e a rede virtual na assinatura de destino na qual replicar e fazer failover dos servidores físicos.

Depois que as validações são concluídas com êxito, clique em OK de modo a passar para a próxima etapa.

Caso não tenha uma rede virtual ou conta de armazenamento do Resource Manager compatível, ou queira adicionar mais, você pode fazer isso clicando nos botões **+ Conta de Armazenamento** ou **+ Rede** na parte superior da folha.

## <a name="next-steps"></a>Próximas etapas
[Definir configurações de replicação](./site-recovery-setup-replication-settings-vmware.md).

