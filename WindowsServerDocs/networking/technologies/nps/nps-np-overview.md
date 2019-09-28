---
title: Políticas de rede
description: Este tópico fornece uma visão geral das políticas de rede para o servidor de políticas de rede no Windows Server 2016 e inclui links para diretrizes adicionais sobre o NPS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c241191f54080ed92a1f1a274c0b2aff0b8c564c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405344"
---
# <a name="network-policies"></a>Políticas de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para obter uma visão geral das políticas de rede no NPS.

>[!NOTE]
>Além deste tópico, a seguinte documentação de política de rede está disponível.
> - [Permissão de acesso](nps-np-access.md)
> - [Configurar políticas de rede](nps-np-configure.md)

As políticas de rede são conjuntos de condições, restrições e configurações que permitem designar quem está autorizado a se conectar à rede e as circunstâncias sob as quais eles podem ou não se conectar.

No processamento das solicitações de conexão como um servidor RADIUS, o NPS executa a autenticação e a autorização da solicitação de conexão. Durante o processo de autenticação, o NPS verifica a identidade do usuário ou computador que está se conectando à rede. Durante o processo de autorização, o NPS determina se o usuário ou o computador tem permissão para acessar a rede.

Para fazer essas determinações, o NPS usa políticas de rede que são configuradas no console do NPS. O NPS também examina as propriedades de discagem da conta de usuário em Active Directory @ no__t-0 Domain Services \(AD DS @ no__t-2 para executar a autorização.

## <a name="network-policies---an-ordered-set-of-rules"></a>Políticas de rede-um conjunto ordenado de regras

As políticas de rede podem ser exibidas como regras. Cada regra tem um conjunto de condições e configurações. O NPS compara as condições da regra com as propriedades de solicitações de conexão. Se ocorrer uma correspondência entre a regra e a solicitação de conexão, as configurações definidas na regra serão aplicadas à conexão.

Quando várias políticas de rede são configuradas no NPS, elas são um conjunto ordenado de regras. O NPS verifica cada solicitação de conexão em relação à primeira regra na lista, depois o segundo, e assim por diante, até que uma correspondência seja encontrada.

Cada política de rede tem uma configuração de **estado de política** que permite habilitar ou desabilitar a política. Quando você desabilita uma política de rede, o NPS não avalia a política ao autorizar solicitações de conexão.

>[!NOTE]
>Se desejar que o NPS avalie uma política de rede ao executar a autorização para solicitações de conexão, você deverá configurar a configuração de **estado da política** marcando a caixa de seleção política habilitada.

## <a name="network-policy-properties"></a>Propriedades da política de rede

Há quatro categorias de propriedades para cada política de rede:

### <a name="overview"></a>Visão geral

 Essas propriedades permitem que você especifique se a política está habilitada, se a política concede ou nega acesso, e se um método de conexão de rede específico ou o tipo de servidor de acesso à rede (NAS) é necessário para solicitações de conexão. As propriedades de visão geral também permitem que você especifique se as propriedades de discagem das contas de usuário no AD DS são ignoradas. Se você selecionar essa opção, somente as configurações na política de rede serão usadas pelo NPS para determinar se a conexão está autorizada.


### <a name="conditions"></a>Condições

 Essas propriedades permitem que você especifique as condições que a solicitação de conexão deve ter para corresponder à política de rede; Se as condições configuradas na política corresponderem à solicitação de conexão, o NPS aplicará as configurações designadas na política de rede para a conexão. Por exemplo, se você especificar o endereço IPv4 do NAS como uma condição da diretiva de rede e o NPS receber uma solicitação de conexão de um NAS que tem o endereço IP especificado, a condição na política corresponderá à solicitação de conexão. 


### <a name="constraints"></a>Restrições

 Restrições são parâmetros adicionais da política de rede que são necessários para corresponder à solicitação de conexão. Se uma restrição não for correspondida pela solicitação de conexão, o NPS rejeitará automaticamente a solicitação. Ao contrário da resposta do NPS a condições não correspondentes na diretiva de rede, se uma restrição não for correspondida, o NPS negará a solicitação de conexão sem avaliar diretivas de rede adicionais.

### <a name="settings"></a>Configurações

 Essas propriedades permitem que você especifique as configurações que o NPS aplica à solicitação de conexão se todas as condições da política de rede para a política forem correspondidas.

Ao adicionar uma nova política de rede usando o console do NPS, você deve usar o assistente de nova política de rede. Depois de criar uma política de rede usando o assistente, você pode personalizar a política clicando duas vezes na política no console do NPS para obter as propriedades da política.

Para obter exemplos de sintaxe de correspondência de padrões para especificar atributos de política de rede, consulte [usar expressões regulares no NPS](nps-crp-reg-expressions.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
