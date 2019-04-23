---
title: Políticas de rede
description: Este tópico fornece uma visão geral das políticas de rede para o servidor de políticas de rede no Windows Server 2016 e inclui links para diretrizes adicionais sobre o NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ab80bb6cf26578430b76806405a65a0f596ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848947"
---
# <a name="network-policies"></a>Políticas de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter uma visão geral das políticas de rede no NPS.

>[!NOTE]
>Além deste tópico, a seguinte documentação de política de rede está disponível.
> - [Permissão de acesso](nps-np-access.md)
> - [Configurar políticas de rede](nps-np-configure.md)

Diretivas de rede são conjuntos de condições, restrições e configurações que permitem que você determine quem está autorizado a conectar-se à rede e as circunstâncias sob as quais eles podem ou não podem se conectar.

No processamento das solicitações de conexão como um servidor RADIUS, o NPS executa a autenticação e a autorização da solicitação de conexão. Durante o processo de autenticação, o NPS verifica a identidade do usuário ou computador que está se conectando à rede. Durante o processo de autorização, o NPS determina se o usuário ou computador tem permissão para acessar a rede.

Para essas determinações, o NPS usa políticas de rede que estão configuradas no console do NPS. O NPS também examina as propriedades de discagem da conta de usuário no Active Directory&reg; serviços de domínio \(AD DS\) para executar a autorização.

## <a name="network-policies---an-ordered-set-of-rules"></a>Políticas de rede – um conjunto ordenado de regras

Políticas de rede podem ser exibidas como regras. Cada regra tem um conjunto de condições e configurações. O NPS compara as condições da regra para as propriedades das solicitações de conexão. Se ocorrer uma correspondência entre a regra e a solicitação de conexão, as configurações definidas na regra são aplicadas para a conexão.

Quando várias diretivas de rede são configuradas no NPS, eles são um conjunto ordenado de regras. O NPS compara cada solicitação de conexão com a primeira regra na lista, em seguida, o segundo e assim por diante, até que uma correspondência for encontrada.

Cada política de rede tem um **estado da política** configuração que permite que você habilitar ou desabilitar a política. Quando você desabilita uma diretiva de rede, o NPS não avalia a política quando a autorização de solicitações de conexão.

>[!NOTE]
>Se você quiser que o NPS para avaliar uma política de rede ao executar a autorização para solicitações de conexão, você deve configurar o **estado da política** configuração selecionando a política habilitada a caixa de seleção.

## <a name="network-policy-properties"></a>Propriedades da política de rede

Há quatro categorias de propriedades para cada diretiva de rede:

### <a name="overview"></a>Visão geral

 Essas propriedades permitem que você especifique se a política está habilitada, se a política concede ou nega acesso e se um método de conexão de rede específico, ou o tipo de servidor de acesso de rede (NAS), é necessário para solicitações de conexão. Propriedades de visão geral também permitem que você especifique se as propriedades de discagem das contas de usuário no AD DS são ignoradas. Se você selecionar essa opção, somente as configurações na diretiva de rede são usadas pelo NPS para determinar se a conexão está autorizada.


### <a name="conditions"></a>Condições

 Essas propriedades permitem que você especifique as condições que a solicitação de conexão deve ter para coincidir com a política de rede; Se as condições configuradas na política de com a solicitação de conexão, o NPS aplica as configurações designadas na diretiva de rede para a conexão. Por exemplo, se você especificar o endereço IPv4 de NAS como uma condição da diretiva de rede e o NPS recebe uma solicitação de conexão de NAS que tem o endereço IP especificado, a condição na política corresponde a solicitação de conexão. 


### <a name="constraints"></a>Restrições

 Restrições são parâmetros adicionais da diretiva de rede que são necessárias para comparar a solicitação de conexão. Se uma restrição não é atendida pela solicitação de conexão, o NPS rejeita a solicitação automaticamente. Ao contrário da resposta do NPS para condições sem correspondência na política de rede, se uma restrição não for atendida, o NPS nega a solicitação de conexão sem avaliação de políticas de rede adicional.

### <a name="settings"></a>Configurações

 Essas propriedades permitem que você especifique as configurações de NPS aplica-se a solicitação de conexão se todas as condições de política de rede para a política são correspondidas.

Quando você adiciona uma nova política de rede usando o console do NPS, você deve usar o Assistente de nova diretiva de rede. Depois de criar uma política de rede usando o assistente, você pode personalizar a política clicando duas vezes a política no console do NPS para obter as propriedades de política.

Para obter exemplos de sintaxe de correspondência para especificar atributos de política de rede, consulte [usar expressões regulares no NPS](nps-crp-reg-expressions.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
