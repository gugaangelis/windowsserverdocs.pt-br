---
title: Grupos de servidores RADIUS remotos
description: Este tópico fornece uma visão geral de rede política servidor RADIUS grupos de servidores remotos no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f27b5e501f110a038264cd54d75c8b8f9566a64
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="remote-radius-server-groups"></a>Grupos de servidores RADIUS remotos

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Quando você configurar o servidor de política de rede (NPS) como um proxy de autenticação discagem usuário serviço RADIUS (Remote), você usa NPS para encaminhar as solicitações de conexão para servidores RADIUS que são capazes de processar as solicitações de conexão, porque eles podem executar a autenticação e autorização no domínio em que a conta de usuário ou computador está localizada. Por exemplo, se você quiser encaminhar solicitações de conexão para um ou mais servidores RADIUS de domínios não confiáveis, você pode configurar o NPS como um proxy RADIUS para encaminhar as solicitações para os servidores RADIUS remotos no domínio não confiável.

>[!NOTE]
>Grupos de servidores remotos RADIUS são relacionada à e separados dos grupos do Windows.

Para configurar o NPS como um proxy RADIUS, você deve criar uma política de solicitação de conexão que contém todas as informações necessárias para NPS avaliar quais mensagens para encaminhar e onde enviar as mensagens.

Quando você configura um grupo de servidores remotos RADIUS de NPS e configurar uma política de solicitação de conexão com o grupo, são designar o local onde o NPS encaminhar as solicitações de conexão.

## <a name="configuring-radius-servers-for-a-group"></a>Configurando servidores RADIUS de um grupo

Um grupo de servidores remotos RADIUS é um grupo nomeado que contém um ou mais servidores RADIUS. Se você definir mais de um servidor, você pode especificar configurações para determinar a ordem em que os servidores são usados pelo proxy ou para distribuir o fluxo de mensagens RADIUS em todos os servidores do grupo para impedir a sobrecarga de um ou mais servidores com muitas solicitações de conexão de balanceamento de carga.

Cada servidor no grupo tem as seguintes configurações.

- **Nome ou endereço**. Cada membro do grupo deve ter um nome exclusivo dentro do grupo. O nome pode ser um endereço IP ou um nome que pode ser resolvido para seu endereço IP.

- **Autenticação e estatísticas**. Você pode encaminhar solicitações de autenticação, solicitações de contabilização ou ambas para cada membro do grupo de servidores RADIUS remotos.

- **Balanceamento de carga**. Uma configuração de prioridade é usada para indicar qual membro do grupo é o servidor primário (a prioridade é definida como 1). Para membros do grupo que têm a mesma prioridade, uma configuração de peso é usada para calcular a frequência RADIUS mensagens são enviadas para cada servidor. Você pode usar configurações adicionais para configurar o modo em que o servidor NPS detecta quando um membro do grupo se torna indisponível pela primeira vez e quando ele fica disponível depois que ela tiver sido determinada não esteja disponível.

Depois que você tiver configurado um grupo de servidores remotos RADIUS, você pode especificar o grupo na autenticação e configurações de estatísticas de uma política de solicitação de conexão. Por isso, você pode configurar um grupo de servidores remotos RADIUS primeiro. Em seguida, você pode configurar a política de solicitação de conexão para usar o grupo de servidores remotos RADIUS recentemente configurado. Como alternativa, você pode usar o Assistente de nova diretiva de solicitação de Conexão para criar um novo grupo de servidores remotos RADIUS enquanto você estiver criando a diretiva de solicitação de conexão.

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
