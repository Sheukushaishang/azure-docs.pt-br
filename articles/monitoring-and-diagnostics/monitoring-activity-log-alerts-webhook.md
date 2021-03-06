# <a name="webhooks-for-azure-activity-log-alerts"></a>Webhook para alertas de Log de atividades do Azure
Como parte da definição de um grupo de ação será possível configurar pontos de extremidade de Webhook para receber notificações de alertas do Log de atividades. Os webhooks permitem rotear uma notificação de alerta do Azure para outros sistemas para pós-processamento ou notificações personalizadas. Este artigo mostra a aparência do conteúdo para o HTTP POST para um webhook.

Para obter informações sobre a instalação e o esquema para alertas de Log de atividades do Azure, [consulte esta página em vez disso](monitoring-activity-log-alerts.md).

Para obter informações sobre a instalação e o esquema para grupos de ação, [consulte esta página em vez disso](monitoring-action-groups.md)

## <a name="authenticating-the-webhook"></a>Autenticação do webhook
O webhook pode autenticar usando a autorização baseada em token – o URI do webhook é salvo com uma ID de token, por exemplo,`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Esquema de conteúdo
O conteúdo JSON contida na operação POST difere com base no campo de data.context.activityLog.eventSource do conteúdo.

###<a name="common"></a>Comum
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a>Administrativo
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                },
            }
        },
        "properties": {}
    }
}
```

Para obter detalhes de esquema específico sobre alertas de log de atividades de notificação do serviço de integridade [clique aqui](monitoring-service-notifications.md) Para obter detalhes de esquema específico em todos os outras alertas do Log de atividades [clique aqui](monitoring-overview-activity-logs.md)

| Nome do elemento | Descrição |
| --- | --- |
| status |Usado para alertas de métrica. Sempre definido como "ativado" para alertas do Log de Atividades. |
| context |Contexto do evento. |
| resourceProviderName |O provedor de recursos do recurso afetado. |
| conditionType |Sempre "Evento". |
| name |Nome da regra de alerta. |
| ID |ID do recurso do alerta. |
| Descrição |Descrição do alerta conforme definido durante a criação do alerta. |
| subscriptionId |ID de assinatura do Azure. |
| timestamp |Hora quando o evento foi gerado pelo serviço do Azure que processou a solicitação. |
| resourceId |ID de recurso do recurso afetado. |
| resourceGroupName |Nome do grupo de recursos do recurso afetado |
| propriedades |Conjunto de pares `<Key, Value>` (ou seja, `Dictionary<String, String>`) que inclui detalhes sobre o evento. |
| evento |Elemento que contém metadados sobre o evento. |
| autorização |Propriedades RBAC do evento. Essas propriedades geralmente incluem a "ação", "função" e "escopo". |
| categoria |Categoria do evento. Os valores com suporte são: Administrativo, Alerta, Segurança, ServiceHealth, Recomendação. |
| chamador |Endereço de email do usuário que realizou a operação, declaração UPN ou declaração SPN com base na disponibilidade. Pode ser nulo para determinadas chamadas do sistema. |
| correlationId |Geralmente um GUID no formato de cadeia de caracteres. Eventos com correlationId pertencem à mesma ação maior e geralmente compartilham uma correlationId. |
| eventDescription |Descrição de texto estático do evento. |
| eventDataId |Identificador exclusivo do evento. |
| eventSource |Nome do serviço ou infraestrutura do Azure que gerou o evento. |
| httpRequest |A solicitação geralmente inclui o "clientRequestId", "clientIpAddress" e "método" HTTP (por exemplo, PUT). |
| level |Um dos seguintes valores: “Crítico”, “Erro”, “Aviso”, “Informativo” e “Detalhado”. |
| operationId |Geralmente um GUID compartilhado entre os eventos correspondentes a uma única operação. |
| operationName |Nome da operação. |
| propriedades |Propriedades do evento. |
| status |Cadeia de caracteres. Status da operação. Os valores comuns incluem: "Iniciado", "Em Andamento", "Êxito", "Falha", "Ativo", "Resolvido". |
| subStatus |Geralmente inclui o código de status HTTP da chamada REST correspondente. Também pode incluir outras cadeias de caracteres que descrevam um substatus. Os valores de substatus comuns incluem: OK (Código de Status HTTP: 200), Criado (Código de Status HTTP: 201), Aceito (Código de Status HTTP: 202), Sem Conteúdo (Código de Status HTTP: 204), Solicitação Incorreta (Código de Status HTTP: 400), Não Encontrado (Código de Status HTTP: 404), Conflito (Código de Status HTTP: 409), Erro Interno do Servidor (Código de Status HTTP: 500), Serviço Indisponível (Código de Status HTTP: 503), Tempo Limite do Gateway (Código de Status HTTP: 504) |

## <a name="next-steps"></a>Próximas etapas
* [Leia mais sobre o Log de Atividades](monitoring-overview-activity-logs.md)
* [Exemplos de scripts da Automação do Azure (Runbooks) em alertas do Azure](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Usar aplicativo lógico para enviar um SMS por meio de Twilio de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Este exemplo serve para alertas de métrica, mas pode ser modificado para funcionar com um alerta do Log de Atividades.
* [Usar aplicativo lógico para enviar uma mensagem do Slack de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Este exemplo serve para alertas de métrica, mas pode ser modificado para funcionar com um alerta do Log de Atividades.
* [Usar aplicativo lógico para enviar uma mensagem a uma Fila do Azure de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Este exemplo serve para alertas de métrica, mas pode ser modificado para funcionar com um alerta do Log de Atividades.
