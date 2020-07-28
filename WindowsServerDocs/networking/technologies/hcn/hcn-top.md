---
title: API de serviço HCN (rede de computação de host) para VMs e contêineres
description: A API de serviço HCN (rede de computação de host) é uma API Win32 voltada para o público que fornece acesso em nível de plataforma para gerenciar redes virtuais, pontos de extremidade de rede virtual e políticas associadas. Juntos, isso fornece conectividade e segurança para VMs (máquinas virtuais) e contêineres em execução em um host do Windows.
ms.author: jmesser
author: jmesser81
ms.prod: windows-server
ms.date: 11/05/2018
ms.openlocfilehash: 6e4d665ba431331fbf1f41a0ac4774e58693a5e2
ms.sourcegitcommit: 717222e9efceb5964872dbf97034cad60f3c48df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87295040"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>API de serviço HCN (rede de computação de host) para VMs e contêineres

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019

A API de serviço HCN (rede de computação de host) é uma API Win32 voltada para o público que fornece acesso em nível de plataforma para gerenciar redes virtuais, pontos de extremidade de rede virtual e políticas associadas. Juntos, isso fornece conectividade e segurança para VMs (máquinas virtuais) e contêineres em execução em um host do Windows. 

Os desenvolvedores usam a API de serviço HCN para gerenciar a rede para VMs e contêineres em seus fluxos de trabalho de aplicativo. A API HCN foi projetada para fornecer a melhor experiência para os desenvolvedores. Os usuários finais não interagem diretamente com essas APIs.  

## <a name="features-of-the-hcn-service-api"></a>Recursos da API do serviço HCN
-    Implementada como C API hospedada pelo serviço de rede do host (HNS) no Oncore/VM.

-    Fornece a capacidade de criar, modificar, excluir e enumerar objetos HCN, como redes, pontos de extremidade, namespaces e políticas. As operações são executadas em identificadores para os objetos (por exemplo, um identificador de rede) e internamente esses identificadores são implementados usando identificadores de contexto RPC.

-    Baseado em esquema. A maioria das funções da API definem parâmetros de entrada e saída como cadeias de caracteres que contêm os argumentos da chamada de função como documentos JSON. Os documentos JSON são baseados em esquemas com controle de versão e fortemente tipados, esses esquemas fazem parte da documentação pública. 

-    Uma API de assinatura/retorno de chamada é fornecida para permitir que os clientes se registrem para notificações de eventos de todo o serviço, como as exclusões e criações de rede.

-    A API do HCN funciona na ponte de desktop (também conhecido como Centennial) aplicativos em execução nos serviços do sistema. A API verifica a ACL recuperando o token do usuário do chamador.

>[!TIP]
>A API do serviço HCN tem suporte em tarefas em segundo plano e em janelas que não são de primeiro plano. 

## <a name="terminology-host-vs-compute"></a>Terminologia: host versus computação
O serviço de computação do host permite que os chamadores criem e gerenciem máquinas virtuais e contêineres em um único computador físico. Ele é nomeado para seguir a terminologia do setor. 

- O **host** é amplamente usado no setor de virtualização para se referir ao sistema operacional que fornece recursos virtualizados.

- A **computação** é usada para se referir a métodos de virtualização mais amplos do que apenas máquinas virtuais. O serviço de rede de computação do host permite que os chamadores criem e gerenciem a rede para máquinas virtuais e contêineres em um único computador físico.

## <a name="schema-based-configuration-documents"></a>Documentos de configuração baseados em esquema
Os documentos de configuração baseados em esquemas bem definidos são um padrão industrial estabelecido no espaço de virtualização. A maioria das soluções de virtualização, como Docker e kubernetes, fornece APIs baseadas em documentos de configuração. Várias iniciativas do setor, com a participação da Microsoft, orientam um ecossistema para definir e validar esses esquemas, como [openapi](https://www.openapis.org/).  Essas iniciativas também orientam a padronização de definições de esquema específicas para os esquemas usados para contêineres, como o [OCI (Open container Initiative)](https://www.opencontainers.org/).

O idioma usado para criar documentos de configuração é [JSON](https://tools.ietf.org/html/rfc8259), que você usa em combinação com:
-    Definições de esquema que definem um modelo de objeto para o documento
-    Validação de se um documento JSON está em conformidade com um esquema
-    Conversão automatizada de documentos JSON de e para representações nativas desses esquemas nas linguagens de programação usadas pelos chamadores das APIs 

As definições de esquema usadas com frequência são [openapi](https://www.openapis.org/) e [esquema JSON](http://json-schema.org/), que permite especificar as definições detalhadas das propriedades em um documento, por exemplo:
-    O conjunto válido de valores para uma propriedade, como 0-100 para uma propriedade que representa uma porcentagem.
-    A definição de enumerações, que são representadas como um conjunto de cadeias de caracteres válidas para uma propriedade.
-    Uma expressão regular para o formato esperado de uma cadeia de caracteres. 

Como parte da documentação das APIs do HCN, estamos planejando publicar o esquema de nossos documentos JSON como uma especificação OpenAPI. Com base nessa especificação, representações específicas de idioma do esquema podem permitir o uso seguro de tipo dos objetos de esquema na linguagem de programação usada pelo cliente. 

### <a name="example"></a>Exemplo 

Veja a seguir um exemplo desse fluxo de trabalho para o objeto que representa um controlador SCSI no documento de configuração de uma VM. 

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
>As anotações [NewIn ("2.0") fazem parte do suporte de controle de versão para as definições de esquema.

A partir desta definição interna, geramos as especificações de OpenAPI para o esquema:

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

Você pode usar ferramentas, como o [Swagger](https://swagger.io/), para gerar representações específicas de idioma da linguagem de programação de esquema usada por um cliente. O Swagger dá suporte a uma variedade de linguagens, como C#, go, JavaScript e Python.

- [Exemplo de código C# gerado](example-c-sharp.md) para o objeto de sub-rede & o IPAM de nível superior.

- [Exemplo de código go gerado](example-go.md) para o objeto de sub-rede & de nível superior do IPAM. Go é usado pelo Docker e kubernetes, que são dois dos consumidores das APIs de serviço de rede de computação do host. O Go tem suporte interno para empacotamento de tipos Go de e para documentos JSON.

Além da geração de código e da validação, você pode usar ferramentas para simplificar o trabalho com documentos JSON, ou seja, [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Objetos de nível superior definidos no HCN. Arquivo schemas. Mars
Conforme mencionado acima, você pode encontrar o esquema do documento para documentos usados pelas APIs HCN em um conjunto de arquivos. Mars em: onecore/VM/DV/net/HNS/Schema/Mars/esquema

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

- Saiba mais sobre os [cenários comuns de HCN](hcn-scenarios.md).

- Saiba mais sobre os [identificadores de contexto RPC para HCN](hcn-declaration-handles.md).

- Saiba mais sobre os [esquemas de documento JSON HCN](hcn-json-document-schemas.md).
