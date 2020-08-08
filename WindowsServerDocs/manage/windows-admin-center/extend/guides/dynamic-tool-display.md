---
title: Controlar a visibilidade da sua ferramenta em uma solução
description: Controlar a visibilidade da sua ferramenta em uma solução SDK do centro de administração do Windows (projeto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: df939bb1a87c9ded77431661dcabd7faf607bb6e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944999"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Controlar a visibilidade da sua ferramenta em uma solução #

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Pode haver ocasiões em que você deseja excluir (ou ocultar) sua extensão ou ferramenta da lista de ferramentas disponíveis. Por exemplo, se sua ferramenta se destina apenas ao Windows Server 2016 (não versões mais antigas), talvez você não queira que um usuário se conecte a um servidor Windows Server 2012 R2 para ver sua ferramenta. (Imagine a experiência do usuário-eles clicam nele, aguardam a ferramenta ser carregada, apenas para obter uma mensagem informando que seus recursos não estão disponíveis para sua conexão.) Você pode definir quando mostrar (ou ocultar) o recurso no manifest.jsda ferramenta no arquivo.

## <a name="options-for-deciding-when-to-show-a-tool"></a>Opções para decidir quando mostrar uma ferramenta ##

Há três opções diferentes que você pode usar para determinar se sua ferramenta deve ser exibida e estar disponível para um servidor ou conexão de cluster específico.

* localhost
* inventário (uma matriz de propriedades)
* Script

### <a name="localhost"></a>LocalHost ###

A propriedade localHost do objeto Conditions contém um valor booliano que pode ser avaliado para inferir se o nó de conexão é localHost (o mesmo computador em que o centro de administração do Windows está instalado) ou não. Ao passar um valor para a propriedade, você indica quando (a condição) exibir a ferramenta. Por exemplo, se você quiser que a ferramenta seja exibida apenas se o usuário estiver na verdade conectando-se ao host local, configure-o da seguinte maneira:

``` json
"conditions": [
{
    "localhost": true
}]
```

Como alternativa, se você quiser que sua ferramenta seja exibida apenas quando o nó de conexão *não for* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Veja como as definições de configuração parecem mostrar apenas uma ferramenta quando o nó de conexão não é localhost:

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

O SDK inclui um conjunto de propriedades de inventário previamente organizado que você pode usar para criar condições para determinar quando sua ferramenta deve estar disponível ou não. Há nove propriedades diferentes na matriz "Inventory":

| Nome da propriedade | Tipo de valor esperado |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | número |
| operatingSystemVersion | version_string (por exemplo: "10,1. *") |
| productType | número |
| clusterFqdn | string |
| isHyperVRoleInstalled | booleano |
| isHyperVPowershellInstalled | booleano |
| isManagementToolsAvailable | booleano |
| isWmfInstalled | booleano |

Cada objeto na matriz de inventário deve estar de acordo com a seguinte estrutura JSON:

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
| gt | maior que |
| ge | maior que ou igual a |
| lt | menor que |
| le | menor que ou igual a |
| eq | igual a |
| ne | diferente de |
| is | verificando se um valor é true |
| not | verificando se um valor é false |
| contains | o item existe em uma cadeia de caracteres |
| notContains | o item não existe em uma cadeia de caracteres |

#### <a name="data-types"></a>Tipos de dados ####

Opções disponíveis para a propriedade ' type ':

| Type | DESCRIÇÃO |
| ---- | ----------- |
| version | um número de versão (por exemplo: 10,1. *) |
| número | um valor numérico |
| string | um valor de cadeia de caracteres |
| booleano | true ou false |

#### <a name="value-types"></a>Tipos de valor ####

A propriedade ' value ' aceita estes tipos:

* cadeia de caracteres
* número
* booleano

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

### <a name="script"></a>script ###

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
A propriedade State é o valor importante que controlará a decisão de mostrar ou ocultar sua extensão na lista de ferramentas.  Os valores permitidos são:

| Valor | Descrição |
| ---- | ----------- |
| Disponível | A extensão deve ser exibida na lista de ferramentas. |
| NotSupported | A extensão não deve ser exibida na lista de ferramentas. |
| NotConfigured | Esse é um valor de espaço reservado para o trabalho futuro que solicitará ao usuário uma configuração adicional antes que a ferramenta seja disponibilizada.  Atualmente, esse valor resultará na exibição da ferramenta e será o equivalente funcional a ' disponível '. |

Por exemplo, se quisermos que uma ferramenta seja carregada somente se o servidor remoto tiver o BitLocker instalado, o script terá esta aparência:

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

Uma configuração de ponto de entrada usando a opção script tem esta aparência:

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

## <a name="supporting-multiple-requirement-sets"></a>Oferecendo suporte a vários conjuntos de requisitos ##

Você pode usar mais de um conjunto de requisitos para determinar quando exibir sua ferramenta definindo vários blocos de "requisitos".

Por exemplo, para exibir sua ferramenta se "cenário A" ou "cenário B" for verdadeiro, defina dois blocos de requisitos; Se for verdadeiro (ou seja, todas as condições em um bloco de requisitos forem atendidas), a ferramenta será exibida.

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

## <a name="supporting-condition-ranges"></a>Intervalos de condição de suporte ##

Você também pode definir um intervalo de condições definindo vários blocos "Conditions" com a mesma propriedade, mas com operadores diferentes.

Quando a mesma propriedade é definida com operadores diferentes, a ferramenta é exibida contanto que o valor esteja entre as duas condições.

Por exemplo, essa ferramenta é exibida contanto que o sistema operacional seja uma versão entre 6.3.0 e 10.0.0:

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
