---
title: Controlar a visibilidade da sua ferramenta em uma solução
description: Controlar a visibilidade da sua ferramenta em uma solução Windows Admin Center SDK (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 3cce07ba5b3d2cc89f1363bbb2af5acd048c0466
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445945"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Controlar a visibilidade da sua ferramenta em uma solução #

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Pode haver ocasiões quando você deseja excluir (ou ocultar) sua extensão ou a ferramenta da lista de ferramentas disponíveis. Por exemplo, se sua ferramenta se destina apenas Windows Server 2016 (versões mais antigas não), não convém um usuário que se conecta a um servidor Windows Server 2012 R2 para ver sua ferramenta em todos os. (Imagine a experiência do usuário – eles clicar nela, aguarde até que a ferramenta para carregar, apenas para receber uma mensagem de que seus recursos não estão disponíveis para sua conexão.) Você pode definir quando exibir (ou ocultar) seu recurso no arquivo manifest. JSON da ferramenta.

## <a name="options-for-deciding-when-to-show-a-tool"></a>Opções para decidir quando mostrar uma ferramenta ##

Há três opções diferentes, que você pode usar para determinar se sua ferramenta deve ser exibidos e estarão disponíveis para um servidor específico ou a conexão do cluster.

* localhost
* inventário (uma matriz de propriedades)
* script

### <a name="localhost"></a>LocalHost ###

A propriedade de localHost do objeto condições contém um valor booliano que pode ser avaliado para inferir se o nó está se conectando é localHost (o computador mesmo que Windows Admin Center está instalado) ou não. Ao passar um valor para a propriedade, indicam quando (a condição) para exibir a ferramenta. Por exemplo, se você quiser apenas a ferramenta a ser exibido se o usuário na verdade está se conectando ao host local, configurá-lo assim:

``` json
"conditions": [
{
    "localhost": true
}]
```

Como alternativa, se você quiser apenas sua ferramenta a ser exibida quando o nó está se conectando *não é* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Eis aqui as definições de configuração como a aparência para mostrar apenas uma ferramenta quando o nó está se conectando não é localhost:

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

### <a name="inventory-properties"></a>Propriedades de inventário ###

O SDK inclui um conjunto estruturado previamente de propriedades de inventário que você pode usar para criar condições para determinar quando a ferramenta deve estar disponível ou não. Há nove propriedades diferentes na matriz 'inventário':

| Nome da propriedade | Tipo de valor esperado |
| ------------- | ------------------- |
| computerManufacturer | cadeia de caracteres |
| operatingSystemSKU | number |
| operatingSystemVersion | version_string (eg: "10.1.*") |
| productType | number |
| clusterFqdn | cadeia de caracteres |
| isHyperVRoleInstalled | boolean |
| isHyperVPowershellInstalled | boolean |
| isManagementToolsAvailable | boolean |
| isWmfInstalled | boolean |

Cada objeto na matriz de inventário deve estar de acordo com a seguinte estrutura json:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>Valores de operador ####

| Operador | Descrição |
| -------- | ----------- |
| gt | Maior que |
| ge | Maior que ou igual a |
| lt | Menor que |
| le | Menor ou igual a |
| eq | Igual a |
| ne | Não é igual a |
| está | Verificando se um valor é true |
| not | Verificando se um valor é false |
| Contém | existe um item em uma cadeia de caracteres |
| notContains | item não existe em uma cadeia de caracteres |

#### <a name="data-types"></a>Tipos de dados ####

Opções disponíveis para a propriedade 'type':

| Tipo | Descrição |
| ---- | ----------- |
| version | um número de versão (por exemplo: 10.1.*) |
| number | um valor numérico |
| cadeia de caracteres | um valor de cadeia de caracteres |
| boolean | VERDADEIRO ou falso |

#### <a name="value-types"></a>Tipos de valor ####

A propriedade 'value' aceita esses tipos:

* cadeia de caracteres
* number
* boolean

Um conjunto de condições de inventário corretamente formado tem esta aparência:

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

### <a name="script"></a>Script ###

Por fim, você pode executar um script do PowerShell personalizado para identificar a disponibilidade e o estado do nó. Todos os scripts devem retornar um objeto com a seguinte estrutura:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
A propriedade de estado é o valor importante que controlará a decisão para mostrar ou ocultar sua extensão na lista de ferramentas.  Os valores permitidos são:

| Valor | Descrição |
| ---- | ----------- |
| Disponível | A extensão deve ser exibida na lista de ferramentas. |
| NotSupported | A extensão não deve ser exibida na lista de ferramentas. |
| NotConfigured | Este é um valor de espaço reservado para futuros trabalhos que solicitará ao usuário para a configuração adicional antes que a ferramenta está disponível.  Atualmente, esse valor resultará na ferramenta que está sendo exibida e é o equivalente funcional para 'Disponível'. |

Por exemplo, se quisermos que uma ferramenta para carregar apenas se o servidor remoto tiver o BitLocker instalado, o script fica assim:

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

## <a name="supporting-multiple-requirement-sets"></a>Suporte a vários conjuntos de requisito ##

Você pode usar mais de um conjunto de requisitos para determinar quando exibir sua ferramenta definindo vários blocos de "requisitos".

Por exemplo, para exibir sua ferramenta se "cenário A" ou "cenário B" é true, definir dois blocos de requisitos; Se uma for verdadeira (ou seja, todas as condições dentro de um bloco de requisitos forem atendidas), a ferramenta é exibida.

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

## <a name="supporting-condition-ranges"></a>Suporte a intervalos de condição ##

Você também pode definir uma variedade de condições definindo vários blocos de "condições" com a mesma propriedade, mas com diferentes operadores.

Quando a mesma propriedade é definida com operadores diferentes, a ferramenta é exibida como o valor está entre as duas condições.

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
