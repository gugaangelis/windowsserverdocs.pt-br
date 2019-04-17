---
title: Controla a visibilidade da sua ferramenta em uma solução
description: Controla a visibilidade da sua ferramenta em uma solução de SDK do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080963"
---
# Controla a visibilidade da sua ferramenta em uma solução #

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Pode haver momentos em que você deseja excluir (ou ocultar) sua extensão ou ferramenta na lista ferramentas disponíveis. Por exemplo, se a ferramenta for direcionado somente Windows Server 2016 (versões mais antigas não), não convém um usuário que se conecta a um servidor Windows Server 2012 R2 para ver sua ferramenta todo o tempo. (Imagine a experiência do usuário, eles clique nele, aguarde até que a ferramenta para carregar, apenas para receber uma mensagem de que seus recursos não estão disponíveis para sua conexão.) Você pode definir quando o recurso no arquivo de manifest.json da ferramenta Mostrar (ou ocultar).

## Opções para decidir quando mostrar uma ferramenta ##

Há três opções diferentes, que você pode usar para determinar se a ferramenta deve ser exibido e estará disponível para um servidor específico ou conexão do cluster.

* localhost
* inventário (uma matriz de propriedades)
* script

### LocalHost ###

A propriedade localHost do objeto condições contém um valor booleano que pode ser avaliado para inferir se o nó de conexão for localHost (o mesmo computador que o Windows Admin Center está instalado no) ou não. Passando um valor para a propriedade, você indicar quando (a condição) para exibir a ferramenta. Por exemplo, se você deseja apenas a ferramenta para exibir se o usuário na verdade está se conectando ao host local, configurá-lo como este:

``` json
"conditions": [
{
    "localhost": true
}]
```

Como alternativa, se você quiser apenas a ferramenta para exibir quando a conexão nó *não é* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Veja as definições de configuração aparecem para mostrar apenas uma ferramenta quando o nó de conexão não é localhost:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true
        }
        ]
    }
    ]
}
```

### Propriedades de inventário ###

O SDK inclui um conjunto administrado previamente de propriedades de inventário que você pode usar para criar condições para determinar quando a ferramenta deve estar disponível ou não. Há nove propriedades diferentes na matriz 'estoque':

| Nome da propriedade | Tipo de valor esperado |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | number |
| operatingSystemVersion | version_string (por exemplo: "10.1. *") |
| productType | number |
| clusterFqdn | string |
| isHyperVRoleInstalled | booliano |
| isHyperVPowershellInstalled | booliano |
| isManagementToolsAvailable | booliano |
| isWmfInstalled | booliano |

Cada objeto na matriz estoque deve estar de acordo com a seguinte estrutura de json:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### Valores de operador ####

| Operador | Descrição |
| -------- | ----------- |
| gt | maior |
| GE | maior ou igual a |
| lt | menor que |
| Le | menor ou igual a |
| EQ | igual a |
| ne | não é igual a |
|  está  | verificar se um valor for true |
| não | Verificando se um valor é false |
| contém | existe um item em uma cadeia de caracteres |
| notContains | item não existe em uma cadeia de caracteres |

#### Tipos de dados ####

Opções disponíveis para a propriedade 'type':

| Tipo | Descrição |
| ---- | ----------- |
| version | um número de versão (ex: 10.1. *) |
| number | um valor numérico |
| string | um valor de cadeia de caracteres |
| booliano | True ou false |

#### Tipos de valor ####

A propriedade 'value' aceita estes tipos:

* string
* number
* booliano

Um conjunto de condições de inventário formada corretamente tem esta aparência:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "gt",
                "value": "6.3"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "8"
            }
            }
        }
        ]
    }
    ]
}
```

### Script ###

Por fim, você pode executar um script PowerShell personalizado para identificar a disponibilidade e o estado do nó. Todos os scripts devem retornar um objeto com a seguinte estrutura:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
A propriedade de estado é o valor importante que controla a decisão para mostrar ou ocultar sua extensão na lista de ferramentas.  Os valores permitidos são:
| Valor | Descrição |
| ---- | ----------- |
| Disponível | A extensão deve ser exibida na lista de ferramentas. |
| NotSupported | A extensão não deve ser exibida na lista de ferramentas. |
| Não-configuradas | Este é um valor de espaço reservado para o trabalho futuro que solicitará ao usuário para configuração adicional antes da ferramenta é disponibilizada.  Atualmente, esse valor resultará na ferramenta que está sendo exibida e é o equivalente funcional para 'Disponível'. |

Por exemplo, se quisermos uma ferramenta para carregar somente se o servidor remoto tiver instalado o BitLocker, o script semelhante à seguinte:

``` ps
$response = @{
    State = 'NotSupported';
    Message = 'Not executed';
    Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}

if (Get-Module -ListAvailable -Name servermanager) {
    Import-module servermanager; 
    $isInstalled = (Get-WindowsFeature -name bitlocker).Installed;
    $isGood = $isInstalled;
}

if($isGood) {
    $response.State = 'Available';
    $response.Message = 'Everything should work.';
}

$response
```

Uma configuração de ponto de entrada usando a opção de script tem esta aparência:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true,
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "eq",
                "value": "10.0.*"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "4"
            }
            },
            "script": "$response = @{ State = 'NotSupported'; Message = 'Not executed'; Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' }, @{Name='Prop2'; Value = 12345678; Type='number'; }; }; if (Get-Module -ListAvailable -Name servermanager) { Import-module servermanager; $isInstalled = (Get-WindowsFeature -name bitlocker).Installed; $isGood = $isInstalled; }; if($isGood) { $response.State = 'Available'; $response.Message = 'Everything should work.'; }; $response"
        }
        ]
    }
    ]
}
```

## Suporte a vários conjuntos de requisito ##

Você pode usar mais de um conjunto de requisitos para determinar quando exibir a ferramenta definindo vários blocos de "requisitos".

Por exemplo, para exibir a ferramenta se "cenário A" ou "cenário B" for verdadeiro, definir dois blocos de requisitos; Se uma for verdadeira (ou seja, todas as condições dentro de um bloco de requisitos são atendidas), a ferramenta é exibida.

``` json
"entryPoints": [
{
    "requirements": [
    {
        "solutionIds": [
            …"scenario A"…
        ],
        "connectionTypes": [
            …"scenario A"…
        ],
        "conditions": [
            …"scenario A"…
        ]
    },
    {
        "solutionIds": [
            …"scenario B"…
        ],
        "connectionTypes": [
            …"scenario B"…
        ],
        "conditions": [
            …"scenario B"…
        ]
    }
    ]
}

```

## Intervalos de condição de suporte ##

Você também pode definir uma variedade de condições definindo vários blocos de "condições" com a mesma propriedade, mas com operadores diferentes.

Quando a mesma propriedade é definida com operadores diferentes, a ferramenta é exibida como o valor é entre as duas condições.

Por exemplo, essa ferramenta é exibida como o sistema operacional é uma versão entre 6.3.0 e 10.0.0:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
             "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
             "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "gt",
                    "value": "6.3.0"
                },
            }
        },
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "lt",
                    "value": "10.0.0"
                }
            }
        }
        ]
    }
    ]
}

```
