---
title: Grupos de servidores RADIUS remotos
description: Este tópico fornece uma visão geral dos grupos de servidores RADIUS remotos do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8275c3e8902ed78d77d01a2ff5d769d3e99abf97
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316172"
---
# <a name="remote-radius-server-groups"></a>Grupos de servidores RADIUS remotos

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Ao configurar o NPS (servidor de diretivas de rede) como um proxy de serviço RADIUS (RADIUS), você usa o NPS para encaminhar solicitações de conexão para servidores RADIUS que são capazes de processar as solicitações de conexão porque elas podem executar autenticação e autorização no domínio em que a conta de usuário ou computador está localizada. Por exemplo, se você quiser encaminhar solicitações de conexão para um ou mais servidores RADIUS em domínios não confiáveis, poderá configurar o NPS como um proxy RADIUS para encaminhar as solicitações para os servidores remotos RADIUS no domínio não confiável.

>[!NOTE]
>Os grupos de servidores RADIUS remotos não estão relacionados a grupos do Windows e separados deles.

Para configurar o NPS como um proxy RADIUS, você deve criar uma política de solicitação de conexão que contenha todas as informações necessárias para que o NPS avalie quais mensagens devem ser encaminhadas e para onde enviar as mensagens.

Quando você configura um grupo de servidores remotos RADIUS no NPS e configura uma política de solicitação de conexão com o grupo, você está designando o local onde o NPS é para encaminhar solicitações de conexão.

## <a name="configuring-radius-servers-for-a-group"></a>Configurando servidores RADIUS para um grupo

Um grupo de servidores RADIUS remotos é um grupo nomeado que contém um ou mais servidores RADIUS. Se você configurar mais de um servidor, poderá especificar as configurações de balanceamento de carga para determinar a ordem na qual os servidores são usados pelo proxy ou distribuir o fluxo de mensagens RADIUS em todos os servidores do grupo para evitar sobrecarregar um ou mais servidores com muitas solicitações de conexão.

Cada servidor do grupo tem as seguintes configurações.

- **Nome ou endereço**. Cada membro do grupo deve ter um nome exclusivo dentro do grupo. O nome pode ser um endereço IP ou um nome que possa ser resolvido para seu endereço IP.

- **Autenticação e contabilidade**. Você pode encaminhar solicitações de autenticação, solicitações de contabilização ou ambas para cada membro do grupo de servidores RADIUS remoto.

- **Balanceamento de carga**. Uma configuração de prioridade é usada para indicar qual membro do grupo é o servidor primário (a prioridade é definida como 1). Para membros do grupo que têm a mesma prioridade, uma configuração de peso é usada para calcular a frequência com que as mensagens RADIUS são enviadas para cada servidor. Você pode usar configurações adicionais para configurar a maneira como o NPS detecta quando um membro do grupo se torna indisponível pela primeira vez e quando ele fica disponível depois de ser determinado como indisponível.

Depois de configurar um grupo de servidores remotos RADIUS, você pode especificar o grupo nas configurações de estatísticas e autenticação de uma política de solicitação de conexão. Por isso, você pode configurar um grupo de servidores remotos RADIUS primeiro. Em seguida, você pode configurar a política de solicitação de conexão para usar o grupo de servidores RADIUS remotos recentemente configurados. Como alternativa, você pode usar o assistente de nova política de solicitação de conexão para criar um novo grupo de servidores remotos RADIUS enquanto estiver criando a política de solicitação de conexão.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
