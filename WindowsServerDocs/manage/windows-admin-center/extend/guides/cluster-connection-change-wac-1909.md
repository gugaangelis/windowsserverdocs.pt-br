---
title: Alterações de tipo de conexão de cluster no centro de administração do Windows v1909
description: Alterações de tipo de conexão de cluster no centro de administração do Windows v1909
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 10/01/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 5324f782ea3c02ed24968d4b3ef58ab8b6ac9d32
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319353"
---
# <a name="cluster-connection-type-changes-in-windows-admin-center-v1909"></a>Alterações de tipo de conexão de cluster no centro de administração do Windows v1909

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

> [!IMPORTANT]
> Este documento descreve as alterações exigidas pelos desenvolvedores de extensão do centro de administração do Windows que estão desenvolvendo ferramentas do centro de administração do Windows para cluster de failover e soluções de HCI (cluster hiperconvergente). Esta é uma alteração obrigatória necessária para que sua extensão seja compatível com a versão de visualização do centro de administração do Windows v1909 e futuras versões de GA.

No centro de administração do Windows v1909, nós unificamos os dois tipos de conexão de cluster diferentes (cluster de failover e conexões hiperconvergentes de cluster) em um único tipo de conexão de cluster. Os usuários não precisarão mais identificar a configuração do cluster antecipadamente para decidir a qual tipo de conexão adicionar o cluster, nem adicionar o cluster duas vezes como tipos de conexão diferentes para obter acesso aos diferentes conjuntos de ferramentas. Os clusters agora podem ser adicionados como um "cluster do Windows Server" e as ferramentas apropriadas serão carregadas, principalmente com base no fato de o Espaços de Armazenamento Diretos estar habilitado ou não.

Como isso exigia uma alteração na definição do tipo de conexão e como as ferramentas relacionadas ao cluster decidem quando serem carregadas, as extensões que fornecem ferramentas para clusters (ou ambos os clusters de HCI ou não de HCI) exigirão as alterações na implementação conforme descrito acima.

## <a name="manifestjson---solutionsids-and-connectiontypes"></a>Manifest. JSON-solutionsIds e ConnectionType

Anteriormente, para fazer sua ferramenta ser exibida para um cluster de failover ou tipo de conexão de cluster de HCI, você usaria uma das seguintes definições no arquivo de ```manifest.json```.

Para clusters de failover:
``` json
    {
        "entryPointType": "tool",
        "name": "MyToolName",
        "urlName": "MyToolUrl",
        "displayName": "MyToolDisplayName",
        "description": "MyToolDescription",
        "icon": "MyToolIcon",
        "path": "MyToolPath",
        "iframeId": "MyToolIframeId",
        "requirements": [
        {
            "solutionIds": [
                "msft.sme.failover-cluster!failover-cluster"
            ],
            "connectionTypes": [
                "msft.sme.connection-type.cluster"
            ]
        }
        ]
    }
```

Para os clusters de HCI, a seção realçada acima teria sido substituída pelo seguinte:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    }
    ]
```

No centro de administração do Windows 1909 e posteriores, os dois SolutionId e ConnectionType foram substituídos pelo seguinte:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ]
    }
    ]
```

Esse é o único tipo de SolutionId e ConnectionName relacionados ao cluster com suporte de Now on. Se sua ferramenta for definida apenas com esse tipo SolutionId e ConnectionType, ela será carregada para qualquer conexão de cluster de failover, independentemente de ser um cluster de HCI ou não. Se você quiser limitar sua ferramenta para estar disponível apenas para clusters de HCI ou clusters não com HCI, será necessário usar também as novas propriedades de inventário descritas na seção a seguir.

## <a name="manifestjson--inventory-properties"></a>Manifest. JSON – Propriedades de inventário
Ao se conectar a um servidor ou cluster, o centro de administração do Windows consultará um conjunto de propriedades de inventário que você pode usar para criar condições para determinar quando a ferramenta deve estar disponível ou não (consulte a seção "Propriedades do inventário" no controle do documento de [visibilidade de sua ferramenta](dynamic-tool-display.md) para obter mais informações). No centro de administração do Windows v1909, adicionamos duas novas propriedades a essa lista que podem ser usadas para determinar se um cluster é um cluster hiperconvergente ou não. 

### <a name="iss2denabled"></a>isS2dEnabled
Tecnicamente, um cluster hiperconvergente é definido como um cluster de failover com o Espaços de Armazenamento Diretos (S2D) habilitado. Se você quiser que sua ferramenta esteja disponível somente para clusters hiperconvergentes, ou seja, quando o S2D estiver habilitado, adicione a seguinte condição de inventário:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
        ]
    }
    ]
```
> [!TIP] 
> Se você quiser que sua ferramenta esteja disponível somente quando o S2D não estiver habilitado, altere o valor de "operador" para "não".

### <a name="isbritannicaenabled"></a>isBritannicaEnabled
Além disso, se você for dependente do recurso de cluster de gerenciamento do SDDC e usar o modelo de objeto do SDDC, poderá verificar a seguinte condição:
``` json
    "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                },
                "isBritannicaEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
    ]
```

## <a name="backward-compatibility-to-support-previous-versions-of-windows-admin-center"></a>Compatibilidade com versões anteriores do centro de administração do Windows
Para garantir que sua extensão continue a trabalhar com versões mais antigas do centro de administração do Windows, como a versão GA do v1904, a definição de SolutionId e ConnectionName anterior pode ser usada junto com a nova definição. Consulte o exemplo abaixo para exibir sua ferramenta somente para clusters de HCI em todas as versões do centro de administração do Windows.
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    },
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC",
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster",
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
            {
                "inventory": {
                    "isS2dEnabled": {
                        "type": "boolean",
                        "operator": "is"
                    }
                }
            }
        ]
    }
    ]
```

## <a name="known-issue-appcontextserviceactiveconnectionishyperconvergedclusterisfailovercluster-is-not-set-properly-in-windows-admin-center-v1909"></a>Problema conhecido: AppContextService. activeConnection. isHyperConvergedCluster/isFailoverCluster não está definido corretamente no centro de administração do Windows v1909
Uma regressão das alterações recentes é que as propriedades de ```AppContextService.activeConnection.isHyperConvergedCluster/isFailoverCluster``` não estão definidas corretamente no centro de administração do Windows v1909 e serão sempre falsas. Isso será corrigido na próxima versão, v1910, mas também será preterido e não estará mais disponível na seguinte versão de GA em 2020. No futuro, você pode substituir isso pelo código abaixo e usar ```this.connectHCI```.
```
    import { ClusterInventoryCache } from '@msft-sme/core';

    constructor(
        ...
        private appContextService: AppContextService,
        ...
    ) {
        ...
        this.clusterInventoryCache = new ClusterInventoryCache(
            this.appContextService,
            <SharedCacheOptions>{ expiration: Constants.sharedCacheExpiration }
        );

         this.isBritannicEnabled().subscribe(result => this.connectHCI = result);
    }

    private isBritannicEnabled(): Observable<boolean> {
        return this.clusterInventoryCache.query(this.getClusterInventoryQueryParameters())
        .pipe(
            map(inventory => {
                return inventory.instance.isBritannicaEnabled;
        }));
    }

    private getClusterInventoryQueryParameters(): ClusterInventoryParams {
        const nodeName = this.appContextService.activeConnection.validNodeName;
        const endpoint = this.appContextService.authorizationManager.getJeaEndpoint(nodeName);
        const options = { powerShellEndpoint: endpoint };
        return {
            name: nodeName,
            requestOptions: options
        };
    }
```