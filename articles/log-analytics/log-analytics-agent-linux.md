---
title: Conectar computadores Linux ao OMS (Operations Management Suite) | Microsoft Docs
description: Este artigo descreve como conectar computadores Windows hospedados na infraestrutura local ao OMS usando o MMA (Microsoft Monitoring Agent).
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 3c556f3d9e81caae574ec093b6f2ce15651c4485
ms.contentlocale: pt-br
ms.lasthandoff: 05/11/2017

---

# <a name="connect-your-linux-computers-to-operations-management-suite-oms"></a>Conectar computadores Linux ao OMS (Operations Management Suite)

Com o OMS, você pode coletar e agir sobre os dados gerados em computadores Linux e soluções de contêiner como o Docker, residindo em seu data center local como servidores físicos ou máquinas virtuais, máquinas virtuais em um serviço hospedado em nuvem como AWS (Amazon Web Services) ou Microsoft Azure. Você também pode usar soluções de gerenciamento disponíveis no OMS, como Controle de Alterações, para identificar alterações de configuração e Gerenciamento de Atualizações para gerenciar atualizações de software para gerenciar proativamente o ciclo de vida das VMs Linux.

O Agente do OMS para Linux se comunica com o serviço OMS pela porta TCP 443 e se o computador se conectar a um firewall ou servidor proxy para se comunicar pela Internet, examine o artigo sobre [Definir as configurações de proxy e firewall](log-analytics-proxy-firewall.md) para entender quais alterações de configuração precisarão ser aplicadas.  Se você estiver monitorando o computador com o System Center 2016 – Operations Manager ou Operations Manager 2012 R2, ele poderá ser multihomed com o serviço OMS para coletar dados e encaminhe para o serviço e ainda ser monitorado pelo Operations Manager.  Os computadores Linux monitorados por um grupo de gerenciamento do Operations Manager integrado ao OMS não recebem a configuração de fontes de dados e encaminham os dados coletados pelo grupo de gerenciamento.  

Se suas políticas de segurança não permitem que computadores em sua rede se conectem à Internet, o agente pode ser configurado para se conectar ao Gateway do OMS para receber informações de configuração e enviar os dados coletados dependendo da solução habilitada. Para obter mais informações e etapas sobre como configurar o Agente do OMS para Linux para se comunicar através de um Gateway do OMS ao serviço OMS, consulte [Conectar computadores ao OMS usando o Gateway do OMS](log-analytics-oms-gateway.md).  

O diagrama a seguir ilustra a conexão entre os computadores Linux gerenciados por agente e o OMS, incluindo a direção e as portas.

![diagrama Comunicação do agente direto com o OMS](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Requisitos do sistema
Antes de começar, examine os detalhes a seguir para verificar se você atende aos pré-requisitos.

### <a name="supported-linux-operating-systems"></a>Sistemas operacionais Linux com suporte
As seguintes distribuições Linux têm suporte oficialmente.  No entanto, o Agente do OMS para Linux também pode ser executado em outras distribuições não listadas.

* Amazon Linux 2012.09 --> 2015.09 (x86/x64)
* CentOS Linux 5, 6 e 7 (x86/x64)
* Oracle Linux 5, 6 e 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 e 7 (x86/x64)
* Debian GNU/Linux 6, 7 e 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 e 12 (x86/x64)

### <a name="package-requirements"></a>Requisitos do pacote

 **Pacote necessário**     | **Descrição**     | **Versão mínima**
--------------------- | --------------------- | -------------------
Glibc |    Biblioteca GNU C    | 2.5-12
Openssl    | Bibliotecas OpenSSL | 0.9.8e ou 1.0
Curl | cliente Web cURL | 7.15.5
Python-ctypes | |
PAM | Módulos de autenticação conectáveis     |

> [!NOTE]
>  Rsyslog ou syslog-ng são necessários para coletar mensagens de syslog. O daemon syslog padrão na versão 5 do Red Hat Enterprise Linux, CentOS e na versão Oracle Linux (sysklog) não tem suporte para a coleta de eventos de syslog. Para coletar dados de syslog nessa versão dessas distribuições, o daemon rsyslog deve ser instalado e configurado para substituir sysklog.

O agente é composto por vários pacotes. O arquivo de versão contém os seguintes pacotes, disponíveis por meio da execução do pacote do shell com `--extract`:

**Pacote** | **Versão** | **Descrição**
----------- | ----------- | --------------
omsagent | 1.3.4 | O Agente do Operations Management Suite para Linux
omsconfig | 1.1.1 | Agente de configuração para o Agente do OMS
omi | 1.2.0 | OMI (Open Management Infrastructure) – um servidor CIM leve
scx | 1.6.3 | Provedores de CIM OMI para métricas de desempenho do sistema operacional
apache-cimprov | 1.0.1 | Provedor de monitoramento de desempenho do Servidor HTTP Apache para OMI. Instalado se o Servidor HTTP Apache for detectado.
mysql-cimprov | 1.0.1 | Provedor de monitoramento de desempenho do Servidor MySQL para OMI. Instalado se o servidor MySQL/MariaDB for detectado.
docker-cimprov | 1.0.0 | Provedor do Docker para OMI. Instalado se o Docker for detectado.

### <a name="compatibility-with-system-center-operations-manager"></a>Compatibilidade com o System Center Operations Manager
O Agente do OMS para Linux compartilha arquivos binários de agente com o agente do System Center Operations Manager. A instalação do Agente do OMS para Linux em um sistema atualmente gerenciado pelo Operations Manager, atualiza os pacotes OMI e SCX no computador para uma versão mais recente. Nesta versão, o OMS e o System Center 2016 – agentes do Operations Manager/Operations Manager 2012 R2 para Linux são compatíveis.

> [!NOTE]
> O System Center 2012 SP1 e versões anteriores atualmente não são compatíveis com o Agente do OMS para Linux ou não têm suporte.<br>
> Se o Agente do OMS para Linux for instalado em um computador que não é monitorado pelo Operations Manager no momento e você desejar monitorar o computador com o Operations Manager, será necessário modificar a [configuração do OMI](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) antes de descobrir o computador. **Esta etapa não *será* necessária se o agente do Operations Manager for instalado antes do Agente do OMS para Linux.**

### <a name="system-configuration-changes"></a>Alterações de configuração do sistema
Após a instalação dos pacotes do Agente do OMS para Linux, serão aplicadas as seguintes alterações de configuração adicionais em todo o sistema. Esses artefatos serão removidos quando o pacote omsagent for desinstalado.

* Um usuário sem privilégios chamado: `omsagent` é criado. O daemon omsagent é executado como essa conta.
* Um arquivo "include" sudoers é criado em /etc/sudoers.d/omsagent. Isso autoriza o omsagent a reiniciar os daemons syslog e omsagent. Se o sudo incluir diretivas sem suporte na versão instalada do sudo, essas entradas serão gravadas em /etc/sudoers.
* A configuração de syslog é modificada para encaminhar um subconjunto de eventos para o agente. Para saber mais, confira a seção **Configuração da Coleta de Dados** abaixo

### <a name="upgrade-from-a-previous-release"></a>Atualizar de uma versão anterior
A atualização de versões anteriores à 1.0.0-47 tem suporte nesta versão. Executar a instalação com o comando `--upgrade` atualizará todos os componentes do agente para a versão mais recente.

## <a name="install-the-oms-agent-for-linux"></a>Instalar o Agente do OMS para Linux
O Agente do OMS para Linux é fornecido em um pacote de script de shell de extração automática instalável. Este pacote contém pacotes Debian e RPM para cada um dos componentes do agente e pode ser instalado diretamente ou extraído para recuperar os pacotes individuais. Um pacote é fornecido para arquiteturas x64 e outro para arquiteturas x86.

### <a name="installing-the-agent"></a>Instalando o agente

1. Transfira o pacote apropriado (x86 ou x64) para seu computador Linux usando scp/sftp.
2. Instale o pacote usando o argumento `--install` ou `--upgrade`.

    > [!NOTE]
    > Use o argumento `--upgrade` se houver qualquer pacote existente instalado, como quando o agente do System Center Operations Manager para Linux já está instalado. Para se conectar ao Operations Management Suite durante a instalação, forneça os parâmetros `-w <WorkspaceID>` e `-s <Shared Key>`.

### <a name="bundle-command-line-arguments"></a>Argumentos de linha de comando do pacote
```
Options:
  --extract              Extract contents and exit.
  --force                Force upgrade (override version checks).
  --install              Install the package from the system.
  --purge                Uninstall the package and remove all related data.
  --remove               Uninstall the package from the system.
  --restart-deps         Reconfigure and restart dependent service
  --source-references    Show source code reference hashes.
  --upgrade              Upgrade the package in the system.
  --version              Version of this shell bundle.
  --version-check        Check versions already installed to see if upgradable.
  --debug                use shell debug mode.

  -w id, --id id         Use workspace ID <id> for automatic onboarding.
  -s key, --shared key   Use <key> as the shared key for automatic onboarding.
  -d dmn, --domain dmn   Use <dmn> as the OMS domain for onboarding. Optional.
                         default: opinsights.azure.com
                         ex: opinsights.azure.us (for FairFax)
  -p conf, --proxy conf  Use <conf> as the proxy configuration.
                         ex: -p [protocol://][user:password@]proxyhost[:port]
  -a id, --azure-resource id Use Azure Resource ID <id>.
  -m marker, --multi-homing-marker marker
                         Onboard as a multi-homing(Non-Primary) workspace.

  -? | --help            shows this usage text.
```

#### <a name="to-install-and-onboard-directly"></a>Para instalar e integrar diretamente
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a>Para instalar e integrar para um espaço de trabalho no Nuvem do Governo dos EUA
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

#### <a name="to-install-the-agent-packages-and-onboard-at-a-later-time"></a>Para instalar os pacotes do agente e integrar posteriormente
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade
```

#### <a name="to-extract-the-agent-packages-from-the-bundle-without-installing"></a>Para extrair os pacotes do agente do pacote sem instalar
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --extract
```

## <a name="configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a>Configurando o agente para uso com um servidor proxy HTTP ou Gateway do OMS
O Agente do OMS para Linux dá suporte para a comunicação por meio de um servidor proxy HTTP, HTTPS ou Gateway do OMS para o serviço OMS.  Há suporte para a autenticação anônima e básica (nome de usuário/senha).  

### <a name="proxy-configuration"></a>Configuração de Proxy
O valor de configuração de proxy tem a seguinte sintaxe:

`[protocol://][user:password@]proxyhost[:port]`

Propriedade|Descrição
-|-
Protocolo|http ou https
usuário|Nome de usuário opcional para autenticação de proxy
Senha|Senha opcional para autenticação de proxy
proxyhost|Endereço ou FQDN do servidor proxy/Gateway do OMS
porta|Número da porta opcional para o servidor proxy/OMS do Gateway

Por exemplo: `http://user01:password@proxy01.contoso.com:8080`

O servidor proxy pode ser especificado durante a instalação ou modificando o arquivo de configuração proxy.conf após a instalação.   

### <a name="specify-proxy-configuration-during-installation"></a>Especificar a configuração de proxy durante a instalação
O argumento `-p` ou `--proxy` para o pacote de instalação omsagent especifica a configuração de proxy a ser usada.

```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a>Definir a configuração de proxy em um arquivo
A configuração de proxy pode ser definida no arquivo: `/etc/opt/microsoft/omsagent/proxy.conf` esse arquivo pode ser criado ou editado diretamente, mas deve ser legível pelo usuário omsagent. Por exemplo:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a>Removendo a configuração de proxy
Para remover uma configuração de proxy definida anteriormente e reverter para a conectividade direta, remova o arquivo proxy.conf:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```
## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a>Habilitar o Agente do OMS para Linux para reportar para o System Center Operations Manager
Execute as seguintes etapas para configurar o Agente do OMS para Linux para reportar para um grupo de gerenciamento do System Center Operations Manager.  

1. Edite o arquivo `/etc/opt/omi/conf/omiserver.conf`
2. Verifique se a linha que começa com **httpsport=** define a porta 1270. Como: `httpsport=1270`
3. Reinicie o servidor OMI: `sudo /opt/omi/bin/service_control restart`

## <a name="onboarding-with-operations-management-suite"></a>Integração com o Operations Management Suite
Se uma ID e chave do espaço de trabalho não foram fornecidas durante a instalação do pacote, o agente deve ser subsequentemente registrado com o Operations Manager Suite.

### <a name="onboarding-using-the-command-line"></a>Integração usando a linha de comando
Execute o comando omsadmin.sh fornecendo a ID e a chave do espaço de trabalho. Este comando deve ser executado como raiz (com elevação sudo):
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>Integração usando um arquivo
1.    Crie o arquivo `/etc/omsagent-onboard.conf`. O arquivo deve ser legível e gravável para a raiz.
`sudo vi /etc/omsagent-onboard.conf`
2.    Insira as seguintes linhas no arquivo com a ID e a Chave Compartilhada do Espaço de Trabalho:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  

3.    Execute o comando a seguir para integrar ao OMS: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.    O arquivo será excluído na integração bem-sucedida

## <a name="manage-omsagent-daemon"></a>Gerenciar o daemon omsagent
Começando com a versão 1.3.0-1, registramos o daemon omsagent para cada espaço de trabalho integrado. O nome do daemon é *omsagent-\<workspace-id>*.  Você pode usar o comando `/opt/microsoft/omsagent/bin/service_control` para operar o daemon.

```
sudo sh /opt/microsoft/omsagent/bin/service_control start|stop|restart|enable|disable [<workspace id>]
```

Essa ID do espaço de trabalho é um parâmetro opcional. Se for especificada, ela funcionará apenas no daemon específico do espaço de trabalho.  Caso contrário, ela funcionará em todos os daemons.


## <a name="agent-logs"></a>Logs do Agente
Os logs do Agente do OMS para Linux podem ser encontrados em: `/var/opt/microsoft/omsagent/<workspace id>/log/` Os logs do programa omsconfig (configuração do agente) pode ser encontrado em: `/var/opt/microsoft/omsconfig/log/` Os Logs dos componentes OMI e SCX (que fornecem dados de métrica de desempenho) podem ser encontrados em: `/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Configuração de rotação de log##
A configuração de rotação de log para omsagent pode ser encontrada em: `/etc/logrotate.d/omsagent-<workspace id>`

As configurações padrão são:
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a>Desinstalar o Agente do OMS para Linux
Os pacotes de agente podem ser desinstalados usando dpkg ou rpm ou executando o arquivo. sh do pacote com o argumento `--remove`.  Além disso, se desejar remover completamente todos os elementos do Agente do OMS para Linux, você poderá executar o arquivo. sh do pacote com o argumento `--purge`.

### <a name="debian--ubuntu"></a>Debian e Ubuntu
```
> sudo dpkg -P omsconfig
> sudo dpkg -P omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

### <a name="centos-oracle-linux-rhel-and-sles"></a>CentOS, Oracle Linux, RHEL e SLES
```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```
## <a name="troubleshooting"></a>Solucionar problemas

### <a name="issue-unable-to-connect-through-proxy-to-oms"></a>Problema: não é possível se conectar por meio do proxy ao OMS

#### <a name="probable-causes"></a>Causas prováveis
* O proxy especificado durante a integração estava incorreto
* Os Pontos de Extremidade do Serviço OMS não estão na lista de permissões do seu datacenter

#### <a name="resolutions"></a>Resoluções
1. Reintegre ao Serviço OMS com o Agente do OMS para Linux usando o seguinte comando com a opção `-v` habilitada. Isso permite a saída detalhada do agente que está se conectando por meio do proxy ao Serviço OMS.
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Consulte a seção [Configurando o agente para uso com um servidor proxy HTTP (#configuring the-agent-for-use-with-a-http-proxy-server)] para verificar se você configurou adequadamente o agente para se comunicar pelo servidor proxy.    
* Verifique uma segunda vez se os seguintes pontos de extremidade do Serviço OMS estão na lista de permissões:

    |Recurso de agente| Portas |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Porta 443|   
    |*.oms.opinsights.azure.com | Porta 443|   
    |ods.systemcenteradvisor.com | Porta 443|   
    |*.blob.core.windows.net/ | Porta 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problema: você recebe um erro 403 quando tentar integrar

#### <a name="probable-causes"></a>Causas prováveis
* A data e a hora estão incorretas no servidor Linux
* A ID e a Chave do Espaço de Trabalho usadas estão incorretas

#### <a name="resolution"></a>Resolução

1. Verifique a hora no servidor Linux com o comando date. Se a hora for +/-15 minutos da hora atual, a integração falhará. Para corrigir esse problema, atualize a data e/ou o fuso horário do servidor Linux.
Novo! Agora, a versão mais recente do Agente do OMS para Linux notifica se a diferença de horário está causando falhas integração. Reintegre usando as instruções de ID e Chave do Espaço de Trabalho corretas

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problema: Você vê um erro 404 e 500 no arquivo de log logo após a integração
Isso é um problema conhecido que ocorre durante o primeiro upload de dados do Linux em um espaço de trabalho do OMS. Isso não afeta os dados sendo enviados ou a experiência do serviço.

### <a name="issue--you-are-not-seeing-any-data-in-the-oms-portal"></a>Problema: Você não está vendo nenhum dado no portal do OMS

#### <a name="probable-causes"></a>Causas prováveis

- Falha na integração com o Serviço OMS
- A conexão com o Serviço OMS está bloqueada
- Os dados do Agente do OMS para Linux têm o backup realizado

#### <a name="resolutions"></a>Resoluções
1. Verifique se a integração do Serviço OMS foi bem-sucedida verificando se os seguintes arquivos existem: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reintegração usando as instruções de linha de comando `omsadmin.sh`
3. Se estiver usando um proxy, consulte as etapas de resolução de proxy fornecidas anteriormente.
4. Em alguns casos, quando o Agente do OMS para Linux não pode se comunicar com o Serviço OMS, os dados no agente são enfileirados até todo o tamanho do buffer, que é 50 MB. O Agente do OMS para Linux deve ser reiniciado executando o seguinte comando `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.
> [!NOTE]
> Esse problema foi corrigido nas versões 1.1.0-28 e posteriores do Agente.

