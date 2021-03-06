# Visão geral
## [O que é o Resource Manager?](resource-group-overview.md)
## [Serviços com suporte](resource-manager-supported-services.md)
## [Resource Manager e a implantação clássica](resource-manager-deployment-model.md)
## [Governança de assinatura](resource-manager-subscription-governance.md)
## [Aplicativos gerenciados](managed-application-overview.md)

# Introdução
## [Exportar modelo](resource-manager-export-template.md)
## [Crie seu primeiro modelo](resource-manager-create-first-template.md)
## [Visual Studio com o Gerenciador de Recursos](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Exemplos
## PowerShell
### [Implantar modelo](resource-manager-samples-powershell-deploy.md)

## CLI do Azure
### [Implantar modelo](resource-manager-samples-cli-deploy.md)

# Como
## Criar modelos
### [Práticas recomendadas para modelos](resource-manager-template-best-practices.md)
### [Seções do modelo](resource-group-authoring-templates.md)
### [Vincular a outros modelos](resource-group-linked-templates.md)
### [Definir dependência entre recursos](resource-group-define-dependencies.md)
### [Criar múltiplas instâncias](resource-group-create-multiple.md)
### [Definir local](resource-manager-template-location.md)
### [Atribuir marcas](resource-manager-template-tags.md)
### [Definir o nome e tipo do recurso filho](resource-manager-template-child-resource.md)
### [Atualizar recurso](resource-manager-update.md)
### [Usar objetos de parâmetros](resource-manager-objects-as-parameters.md)
### [Compartilhar estado entre modelos vinculados](best-practices-resource-manager-state.md)
### [Padrões para projetar modelos](best-practices-resource-manager-design-templates.md)

## Implantar
### PowerShell
#### [Implantar modelo](resource-group-template-deploy.md)
#### [Implantar modelo particular com o token SAS](resource-manager-powershell-sas-token.md)
#### [Exportar modelo e reimplantar](resource-manager-export-template-powershell.md)
### CLI do Azure
#### [Implantar modelo](resource-group-template-deploy-cli.md)
#### [Implantar modelo particular com o token SAS](resource-manager-cli-sas-token.md)
#### [Exportar modelo e reimplantar](resource-manager-export-template-cli.md)
### [Portal](resource-group-template-deploy-portal.md)
### [API REST](resource-group-template-deploy-rest.md)
### [Implantação entre grupos de recursos](resource-manager-cross-resource-group-deployment.md)
### [Integração contínua com o Visual Studio Team Services](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Transmitir valores seguros durante a implantação](resource-manager-keyvault-parameter.md)

## Gerenciar
### [PowerShell](powershell-azure-resource-manager.md)
### [CLI do Azure](xplat-cli-azure-resource-manager.md)
### [Portal](resource-group-portal.md)
### [API REST](resource-manager-rest-api.md)
### [Usar marcações para organizar recursos](resource-group-using-tags.md)
### [Mover recursos para um novo grupo ou assinatura](resource-group-move-resources.md)
### [Exemplos de governança](resource-manager-subscription-examples.md)

## Controlar Acesso
### Criar uma entidade de serviço
#### [PowerShell](resource-group-authenticate-service-principal.md)
#### [CLI 2.0 do Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [CLI 1.0 do Azure](resource-group-authenticate-service-principal-cli.md)
#### [Portal](resource-group-create-service-principal-portal.md)
### [API de autenticação para assinaturas de acesso](resource-manager-api-authentication.md)
### [Bloquear recursos](resource-group-lock-resources.md)

## Definir políticas de recursos
### [O que são políticas de recursos?](resource-manager-policy.md)
### [Atribuição de política do portal](resource-manager-policy-portal.md)
### [Atribuição de política de script](resource-manager-policy-create-assign.md)
### [Políticas de marca do recurso](resource-manager-policy-tags.md)
### [Políticas de armazenamento](resource-manager-policy-storage.md)
### [Políticas de VM do Linux](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Políticas de VM do Windows](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

## Usar aplicativos gerenciados
### [Publicar o aplicativo gerenciado](managed-application-publishing.md)
### [Consumir um aplicativo gerenciado](managed-application-consumption.md)
### [Criar definições de interface do usuário](managed-application-createuidefinition-overview.md)

## Audit
### [Exibir logs de atividade](resource-group-audit.md)
### [Exibir operações de implantação](resource-manager-deployment-operations.md)

## Solucionar problemas
### [Erros de implantação comuns](resource-manager-common-deployment-errors.md)

# Referência
## [Formato do modelo](/azure/templates/)
## [Funções de modelo](resource-group-template-functions.md)
### [Funções de objeto e matriz](resource-group-template-functions-array.md)
### [Funções de comparação](resource-group-template-functions-comparison.md)
### [Funções de implantação](resource-group-template-functions-deployment.md)
### [Funções numéricas](resource-group-template-functions-numeric.md)
### [Funções de recurso](resource-group-template-functions-resource.md)
### [Funções de cadeia de caracteres](resource-group-template-functions-string.md)
## [Funções de definição de interface do usuário](managed-application-createuidefinition-functions.md)
## [Elementos de definição de interface do usuário](managed-application-createuidefinition-elements.md)
### [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
### [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
### [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
### [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
### [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
### [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
### [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
### [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
### [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
### [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
### [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
### [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
### [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [CLI do Azure](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# Recursos
## [Solicitações de limitação](resource-manager-request-limits.md)
## [Rastrear operações assíncronas](resource-manager-async-operations.md)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
## [Atualizações de serviço](https://azure.microsoft.com/updates/?product=azure-resource-manager)
