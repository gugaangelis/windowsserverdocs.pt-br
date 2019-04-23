---
title: Configurar políticas de solicitação de conexão
description: Este tópico fornece informações sobre como configurar políticas de solicitação de Conexão no servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f80f9fb8be0c44cfb5685e5b9cc489282e4961d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884507"
---
# <a name="configure-connection-request-policies"></a>Configurar políticas de solicitação de conexão

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para criar e configurar as diretivas de solicitação de conexão que designa se o NPS local processa as solicitações de conexão ou encaminha ao servidor RADIUS remoto para processamento.

Diretivas de solicitação de Conexão são conjuntos de condições e configurações que permitem que os administradores de rede designar quais servidores RADIUS Remote Authentication Dial-In usuário Service () executam a autenticação e solicitações de autorização de conexão que o servidor que executa o servidor de políticas de rede \(NPS\) recebe de clientes RADIUS.

A diretiva de solicitação de conexão padrão usa o NPS como servidor RADIUS e processa todas as solicitações de autenticação localmente.

Para configurar um servidor que executa o NPS para atuar como um proxy RADIUS e solicitações de conexão direta a outros servidores RADIUS ou NPS, você deve configurar um grupo de servidores remotos RADIUS, além de adicionar uma nova diretiva de solicitação de conexão que especifica as configurações e condições que as solicitações de conexão devem corresponder.

Enquanto você estiver criando uma nova diretiva de solicitação de conexão com o Assistente de nova diretiva de solicitação de Conexão, você pode criar um novo grupo de servidores RADIUS remoto.

Se você não quiser que o NPS para atuar como uma conexão de servidor e dos processos RADIUS solicitações localmente, você pode excluir a política de solicitação de conexão padrão.

Se você quiser que o NPS para atuar como um servidor RADIUS, processamento de solicitações de conexão localmente e como um proxy RADIUS, encaminhando algumas solicitações de conexão para um grupo de servidores remotos RADIUS, adicione uma nova política usando o procedimento a seguir e, em seguida, verifique se que o padrão diretiva de solicitação de conexão é a última diretiva processada, colocando-o pela última vez na lista de políticas.

## <a name="add-a-connection-request-policy"></a>Adicione uma diretiva de solicitação de Conexão

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-add-a-new-connection-request-policy"></a>Para adicionar uma nova diretiva de solicitação de conexão 

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server** para abrir o console do NPS. 
2. Na árvore de console, clique duas vezes **políticas**.
3. Clique com botão direito **diretivas de solicitação de Conexão**e, em seguida, clique em **nova diretiva de solicitação de Conexão**.
4. Use o novo Assistente de diretiva de solicitação de Conexão para configurar sua conexão solicitam a política e, se não configurado anteriormente, um grupo de servidores RADIUS remotos.


Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

