---
title: Hospedar a API do serviço de rede de computação (HCN) para VMs e contêineres
description: Host de API do serviço de rede de computação (HCN) é uma API do Win32 para o público que fornece acesso de nível de plataforma para gerenciar as redes virtuais, pontos de extremidade de rede virtual e as políticas associadas. Juntos isso fornece a conectividade e segurança para máquinas virtuais (VMs) e contêineres em execução em um host do Windows.
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 50af0dab69633aa6e07ded68e9246aa0315377f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844977"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>Hospedar a API do serviço de rede de computação (HCN) para VMs e contêineres

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Host de API do serviço de rede de computação (HCN) é uma API do Win32 para o público que fornece acesso de nível de plataforma para gerenciar as redes virtuais, pontos de extremidade de rede virtual e as políticas associadas. Juntos isso fornece a conectividade e segurança para máquinas virtuais (VMs) e contêineres em execução em um host do Windows. 

Os desenvolvedores usam a API do serviço HCN para gerenciar a rede para VMs e contêineres em seus fluxos de trabalho do aplicativo. A API HCN foi projetada para fornecer a melhor experiência para os desenvolvedores. Os usuários finais não interagem diretamente com essas APIs.  

## <a name="features-of-the-hcn-service-api"></a>Recursos da API do serviço HCN
-   Implementado como C API hospedada pelo Host de rede HNS (serviço) na VM/OnCore.

-   Fornece a capacidade de criar, modificar, excluir e enumerar objetos HCN como redes, pontos de extremidade, namespaces e as políticas. Operações são realizadas em identificadores de objetos (por exemplo, um identificador de rede) e internamente esses identificadores são implementados usando identificadores de contexto RPC.

-   Com base no esquema. A maioria das funções da API de definir a entrada e parâmetros, como cadeias de caracteres que contém os argumentos da chamada de função como documentos JSON de saída. Os documentos JSON se baseiam nos esquemas de controle de versão e fortemente tipados, esses esquemas são parte da documentação do pública. 

-   Um assinatura/retorno de chamada de API é fornecido para permitir que os clientes para se registrar para notificações de eventos de todo o serviço como uma rede de criações e exclusões.

-   API de HCN funciona na ponte de Desktop (também conhecido como Aplicativos de centennial) em execução nos serviços do sistema. A API verifica a ACL, recuperar o token de usuário do chamador.

>[!TIP]
>Há suporte para a API do serviço HCN tarefas em segundo plano e o windows não em primeiro plano. 

## <a name="terminology-host-vs-compute"></a>Terminologia: Host vs. Computação
O serviço de computação do host permite que os chamadores criar e gerenciar máquinas virtuais e contêineres em um único computador físico. Ele é chamado para seguir a terminologia do setor. 

- **Host** é amplamente usado no setor de virtualização para referir-se ao sistema operacional que fornece recursos virtualizados.

- **Computação** é usado para se referir a métodos de virtualização que são mais amplos do que apenas máquinas de virtuais. Serviço de rede do host de computação permite que os chamadores criar e gerenciar a rede para máquinas virtuais e o contêiner em um único computador físico.

## <a name="schema-based-configuration-documents"></a>Documentos de configuração com base em esquema
Documentos de configuração com base em esquemas bem definidas é um padrão da indústria estabelecido no espaço de virtualização. A maioria das soluções de virtualização, como Docker e Kubernetes, fornecem que APIs com base em documentos de configuração. Unidade de várias iniciativas de mercado, com a participação da Microsoft, um ecossistema para definir e validar esses esquemas, como [OpenAPI](https://www.openapis.org/).  A padronização de definições de esquema específico para os esquemas usados para contêineres, como também levam a essas iniciativas [abrir contêiner OCI (iniciativa)](https://www.opencontainers.org/).

É o idioma usado para a criação de documentos de configuração [JSON](https://tools.ietf.org/html/rfc8259), que você usa em combinação com:
-   Definições de esquema que definem um modelo de objeto para o documento
-   Validação de se um documento JSON está em conformidade com um esquema
-   Conversão de documentos JSON e para representações nativas desses esquemas nas linguagens de programação usadas por chamadores das APIs de automatizada 

Definições de esquema usados com frequência são [OpenAPI](https://www.openapis.org/) e [esquema JSON](http://json-schema.org/), que permite especificar as definições detalhadas das propriedades em um documento, por exemplo:
-   O conjunto válido de valores para uma propriedade, como 0 e 100 para uma propriedade que representa uma porcentagem.
-   A definição de enumerações, que são representados como um conjunto de cadeias de caracteres válidas para uma propriedade.
-   Uma expressão regular para o formato esperado de uma cadeia de caracteres. 

Como parte de documentar as APIs HCN, estamos planejando publicar o esquema de nossos documentos JSON como uma especificação de OpenAPI. Com base nessa especificação, representações de linguagem específica do esquema podem permitir uso fortemente tipada dos objetos de esquema na linguagem de programação usada pelo cliente. 

### <a name="example"></a>Exemplo 

O exemplo a seguir é um exemplo desse fluxo de trabalho para o objeto que representa um controlador SCSI no documento de configuração de uma VM. 

No código-fonte do Windows, podemos definir esquemas usando arquivos .mars: onecore/vm/dv/net/hns/schema/mars/Schema/HCN.Schema.Network.mars

```
enum IpamType
{
    [NewIn("2.0")] Static,
    [NewIn("2.0")] Dhcp,
};

class Ipam
{
    // Type : dhcp
    [NewIn("2.0"),OmitEmpty] IpamType   Type;
    [NewIn("2.0"),OmitEmpty] Subnet     Subnets[];
};

class Subnet : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string         IpAddressPrefix;
    [NewIn("2.0"),OmitEmpty] SubnetPolicy   Policies[];
    [NewIn("2.0"),OmitEmpty] Route          Routes[];
};


enum SubnetPolicyType
{
    [NewIn("2.0")] VLAN
};

class SubnetPolicy
{
    [NewIn("2.0"),OmitEmpty] SubnetPolicyType                 Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Common.PolicySettings Data;
};

class PolicySettings
{
    [NewIn("2.0"),OmitEmpty]  string      Name;
};

class VlanPolicy : HCN.Schema.Common.PolicySettings 
{
    [NewIn("2.0")] uint32 IsolationId;
};

class Route 
{
    [NewIn("2.0"),OmitEmpty] string NextHop;
    [NewIn("2.0"),OmitEmpty] string DestinationPrefix;
    [NewIn("2.0"),OmitEmpty] uint16 Metric;
};

```

>[!TIP]
>O [NewIn("2.0") anotações fazem parte do suporte do controle de versão para as definições de esquema.

Dessa definição interna, podemos gerar as especificações de OpenAPI para o esquema:

```
{ 
    "swagger" : "2.0", 
    "info" : { 
       "version" : "2.1", 
       "title" : "HCN API" 
    },
    "definitions": {        
        "Ipam": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "Static",
                        "Dhcp"
                    ],
                    "description": " Type : dhcp"
                },
                "Subnets": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Subnet"
                    }
                }
            }
        },
        "Subnet": {
            "type": "object",
            "properties": {
                "ID": {
                    "type": "string",
                    "pattern": "^[0-9A-Fa-f]{8}-([0-9A-Fa-f]{4}-){3}[0-9A-Fa-f]{12}$"
                },                
                "IpAddressPrefix": {
                    "type": "string"
                },
                "Policies": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/SubnetPolicy"
                    }
                },
                "Routes": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Route"
                    }
                }
            }
        },
        "SubnetPolicy": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "VLAN",
                        "VSID"
                    ]
                },
                "Data": {
                    "$ref": "#/definitions/PolicySettings"
                }
            }
        },
        "PolicySettings": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                }
            }
        },                      
        "VlanPolicy": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                },
                "IsolationId": {
                    "type": "integer",
                    "format": "uint32"
                }
            }
        },
        "Route": {
            "type": "object",
            "properties": {
                "NextHop": {
                    "type": "string"
                },
                "DestinationPrefix": {
                    "type": "string"
                },
                "Metric": {
                    "type": "integer",
                    "format": "uint16"
                }
            }
        }        
    }
} 
```

Você pode usar ferramentas, tais como [Swagger](https://swagger.io/), para gerar representações de linguagem específica do esquema de linguagem de programação usada por um cliente. Swagger dá suporte a uma variedade de linguagens, como C#, Go, Javascript e Python).

- [Gerado do exemplo de C# código](example-c-sharp.md) para o IPAM de nível superior e a sub-rede do objeto.

- [Exemplo de código gerado do Go](example-go.md) para o IPAM de nível superior e a sub-rede do objeto. Go é usado com o Docker e Kubernetes, que são dois dos consumidores das APIs de serviço de rede de computação do Host. Go tem suporte interno de marshaling de tipos de ir para e de documentos JSON.

Além de geração de código e validação, você pode usar ferramentas para simplificar o trabalho com documentos JSON — ou seja, [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Objetos de nível superior definidos no HCN. Arquivo schemas.MARS
Conforme mencionado acima, você pode encontrar o esquema do documento para documentos usados pelas APIs em um conjunto de arquivos .mars sob HCN: onecore/vm/dv/net/hns / / mars/de esquema

Os objetos de nível superior são:
- [HostComputeNetwork](hcn-scenarios.md#scenario-hcn)
- [HostComputeEndpoint](hcn-scenarios.md#scenario-hcn-endpoint)
- [HostComputeNamespace](hcn-scenarios.md#scenario-hcn-namespace)
- [HostComputeLoadBalancer](hcn-scenarios.md#scenario-hcn-load-balancer)

```
class HostComputeNetwork : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkMode          Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkPolicy        Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.MacPool              MacPool;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                  Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Ipam                 Ipams[];
};

class HostComputeEndpoint : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string                                     HostComputeNetwork;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.EndpointPolicy Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.IpConfig       IpConfigurations[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                     Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Route                   Routes[];
    [NewIn("2.0"),OmitEmpty] string                                     MacAddress;
};

class HostComputeNamespace : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] uint32                                    NamespaceId;
    [NewIn("2.0"),OmitEmpty] Guid                                      NamespaceGuid;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceType        Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceResource    Resources[];
};

class HostComputeLoadBalancer : HCN.Schema.Common.Base
{
    [NewIn("2.0"), OmitEmpty] string                                               HostComputeEndpoints[]; 
    [NewIn("2.0"), OmitEmpty] string                                               VirtualIPs[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.Network.Endpoint.Policy.PortMappingPolicy PortMappings[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.LoadBalancer.LoadBalancerPolicy           Policies[];
};
```

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [cenários comuns de HCN](hcn-scenarios.md).

- Saiba mais sobre o [cuida de contexto RPC para HCN](hcn-declaration-handles.md).

- Saiba mais sobre o [esquemas de documentos JSON HCN](hcn-json-document-schemas.md).