---
title: Grupos de servidores RADIUS remotos
description: Este tópico fornece uma visão geral de rede política de servidor RADIUS grupos de servidores remotos no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9912927a7b75e4c9f04aa3d24eb7ed46c73a7dd2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855257"
---
# <a name="remote-radius-server-groups"></a>Grupos de servidores RADIUS remotos

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando você configura o servidor de diretivas de rede (NPS) como um proxy RADIUS Remote Authentication Dial-In usuário Service (), você usar o NPS para encaminhar solicitações de conexão para servidores RADIUS que são capazes de processar as solicitações de conexão, pois eles podem executar autenticação e autorização no domínio onde se encontra a conta de usuário ou computador. Por exemplo, se você quiser encaminhar solicitações de conexão para um ou mais servidores RADIUS em domínios não confiáveis, você pode configurar o NPS como um proxy RADIUS para encaminhar as solicitações para os servidores RADIUS remotos no domínio não confiável.

>[!NOTE]
>Grupos de servidores remotos RADIUS são relacionada aos e separado de grupos do Windows.

Para configurar o NPS como um proxy RADIUS, você deve criar uma diretiva de solicitação de conexão que contém todas as informações necessárias para o NPS para avaliar quais mensagens para encaminhar e para onde enviar as mensagens.

Quando você configura um grupo de servidores remotos RADIUS no NPS e configurar uma diretiva de solicitação de conexão com o grupo, você está designando o local onde o NPS encaminhar solicitações de conexão.

## <a name="configuring-radius-servers-for-a-group"></a>Configurando servidores RADIUS de um grupo

Um grupo de servidores remotos RADIUS é um grupo nomeado que contém um ou mais servidores RADIUS. Se você configurar mais de um servidor, você pode especificar configurações para determinar a ordem na qual os servidores são usados pelo proxy ou para distribuir o fluxo de mensagens RADIUS entre todos os servidores no grupo para evitar a sobrecarga de um ou mais servidores de balanceamento de carga com muitas solicitações de conexão.

Cada servidor no grupo tem as seguintes configurações.

- **Nome ou endereço**. Cada membro do grupo deve ter um nome exclusivo dentro do grupo. O nome pode ser um endereço IP ou um nome que pode ser resolvido para seu endereço IP.

- **Autenticação e estatísticas**. Você pode encaminhar solicitações de autenticação, as solicitações de contabilidade ou ambos para cada membro do grupo de servidor RADIUS remoto.

- **O balanceamento de carga**. Uma configuração de prioridade é usada para indicar qual membro do grupo é o servidor primário (a prioridade é definida como 1). Para membros do grupo que têm a mesma prioridade, uma configuração de peso é usada para calcular a frequência com que as mensagens RADIUS sejam enviadas para cada servidor. Você pode usar configurações adicionais para configurar o modo em que o NPS detecta quando um membro do grupo se torna indisponível pela primeira vez e quando ele fica disponível depois que for determinado ficar indisponível.

Depois de configurar um grupo de servidores remotos RADIUS, você pode especificar o grupo na autenticação e as configurações de contabilização de uma diretiva de solicitação de conexão. Por isso, você pode configurar um grupo de servidores remotos RADIUS primeiro. Em seguida, você pode configurar a diretiva de solicitação de conexão para usar o grupo de servidores remotos RADIUS recém-configurado. Como alternativa, você pode usar o Assistente de nova diretiva de solicitação de Conexão para criar um novo grupo de servidores RADIUS remoto, enquanto você está criando a diretiva de solicitação de conexão.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
