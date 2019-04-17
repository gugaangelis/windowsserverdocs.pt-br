---
title: Políticas de rede
description: Este tópico fornece uma visão geral das políticas de rede para o servidor de política de rede no Windows Server 2016 e inclui links para orientação adicional sobre NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7e1604f4839bd955e5ea10d9eafea5ef0c978977
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policies"></a>Políticas de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para obter uma visão geral das políticas de rede no NPS.

>[!NOTE]
>Além de neste tópico, a seguinte documentação de política de rede está disponível.
> - [Permissão de acesso](nps-np-access.md)
> - [Configurar as políticas de rede](nps-np-configure.md)

Políticas de rede são conjuntos de condições, restrições e configurações que permitem que você designar quem tem autorização para se conectar à rede e as condições sob as quais podem ou não consegue se conectar.

Ao processar solicitações de conexão como um servidor remoto Authentication Dial-In User Service (RADIUS), o NPS realiza autenticação e autorização da solicitação de conexão. Durante o processo de autenticação, o NPS verifica a identidade do usuário ou computador que está conectando-se à rede. Durante o processo de autorização, o NPS determina se o usuário ou o computador tem permissão para acessar a rede.

Para tornar esses determinações, o NPS usa políticas de rede que estão configuradas no console do NPS. NPS também examina as propriedades de discagem da conta de usuário no Active Directory&reg; \(AD DS\) serviços de domínio para executar a autorização.

## <a name="network-policies---an-ordered-set-of-rules"></a>Políticas de rede - um conjunto de regras ordenado

Políticas de rede podem ser exibidas como regras. Cada regra tem um conjunto de condições e configurações. NPS compara as condições da regra para as propriedades das solicitações de conexão. Se ocorrer uma correspondência entre a regra e a solicitação de conexão, as configurações definidas na regra serão aplicadas para a conexão.

Quando várias políticas de rede são configuradas no NPS, eles são um conjunto ordenado de regras. NPS verifica cada solicitação de conexão com a primeira regra na lista, em seguida, o segundo e assim por diante, até que uma correspondência é encontrada.

Cada política de rede tem um **política estado** configuração que permite que você habilite ou desabilite a política. Ao desabilitar uma política de rede, o NPS não avalia a política quando autorizar solicitações de conexão.

>[!NOTE]
>Se você quiser NPS avaliar uma política de rede ao executar a autorização para solicitações de conexão, você deve configurar o **política estado** configuração selecionando a política habilitada caixa de seleção.

## <a name="network-policy-properties"></a>Propriedades de política de rede

Há quatro categorias de propriedades para cada política de rede:

### <a name="overview"></a>Visão geral

 Essas propriedades permitem que você especifique se a política está habilitada, se a diretiva concede ou nega o acesso e se um método de conexão de rede específica ou tipo de servidor de acesso de rede (NAS), é necessário para solicitações de conexão. Propriedades de visão geral também permitem que você especifique se as propriedades de discagem rápida de contas de usuário no AD DS são ignoradas. Se você selecionar essa opção, somente as configurações na política de rede são usadas pelo NPS para determinar se a conexão está autorizado.


### <a name="conditions"></a>Condições

 Essas propriedades permitem que você especifique as condições que a solicitação de conexão deve ter para corresponder a política de rede; Se as condições configuradas na diretiva corresponderem a solicitação de conexão, o NPS aplica as configurações designadas na política de rede para a conexão. Por exemplo, se você especificar o endereço IPv4 NAS como uma condição da diretiva de rede e NPS recebe uma solicitação de conexão de NAS com o endereço IP especificado, a condição na política corresponde a solicitação de conexão. 


### <a name="constraints"></a>Restrições

 Restrições são parâmetros adicionais da diretiva de rede que são necessárias para corresponder a solicitação de conexão. Se uma restrição não for atendida pela solicitação de conexão, o NPS rejeita automaticamente a solicitação. Ao contrário da resposta do NPS às condições incomparáveis na política de rede, se uma restrição não for atendida, o NPS nega a solicitação de conexão sem avaliar as políticas de rede adicionais.

### <a name="settings"></a>Configurações

 Essas propriedades permitem que você especifique as configurações que NPS aplica-se a solicitação de conexão se todas as condições de política de rede para a política forem atendidas.

Quando você adiciona uma nova política de rede usando o console NPS, você deve usar o Assistente de nova diretiva de rede. Depois que tiver criado uma política de rede usando o assistente, você pode personalizar a política clicando duas vezes a política no console do NPS para obter as propriedades de política.

Para obter exemplos de sintaxe de padrões coincidentes para especificar atributos de política de rede, consulte [Use expressões regulares no NPS](nps-crp-reg-expressions.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
