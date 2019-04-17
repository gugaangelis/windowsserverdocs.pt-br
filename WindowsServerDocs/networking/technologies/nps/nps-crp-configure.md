---
title: Configurar as políticas de solicitação de Conexão
description: Este tópico fornece informações sobre como configurar as políticas de solicitação de Conexão no servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9677e147bdaea4de71a054cd6c52d81126e005d1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-connection-request-policies"></a>Configurar as políticas de solicitação de Conexão

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para criar e configurar as políticas de solicitação de conexão designar se o servidor NPS local processa as solicitações de conexão ou encaminha para o servidor RADIUS remoto para processamento.

As políticas de solicitação de Conexão são conjuntos de configurações que permitem aos administradores de rede designar quais servidores remotos Authentication Dial-In User Service (RADIUS) fazer a autenticação e autorização de solicitações de conexão que o servidor que executa o servidor de política de rede \(NPS\) recebe de clientes RADIUS e condições.

A política de solicitação de conexão padrão usa NPS como um servidor RADIUS e processa todas as solicitações de autenticação localmente.

Para configurar um servidor NPS para atuar como um proxy RADIUS e solicitações de conexão direta para outros servidores NPS ou RADIUS, você deve configurar um grupo de servidores remotos RADIUS além de adicionar uma nova política de solicitação de conexão que especifica condições e configurações que as solicitações de conexão devem corresponder.

Você pode criar um novo grupo de servidores remotos RADIUS enquanto você estiver criando uma nova política de solicitação de conexão com o Assistente de nova diretiva de solicitação de Conexão.

Se não quiser que o servidor NPS para atuar como uma conexão de servidor e o processo de RAIO solicita localmente, você pode excluir a política de solicitação de conexão padrão.

Se você deseja que o servidor NPS para atuar como um servidor RADIUS, processar solicitações de conexão localmente e como um proxy RADIUS, encaminhar algumas solicitações de conexão para um grupo de servidores remotos RADIUS, adicione uma nova política usando o procedimento a seguir e, em seguida, verifique se a política de solicitação de conexão padrão é a última diretiva processada, colocando-o último na lista de políticas.

## <a name="add-a-connection-request-policy"></a>Adicione uma diretiva de solicitação de Conexão

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-add-a-new-connection-request-policy"></a>Para adicionar uma nova política de solicitação de conexão 

1. No Gerenciador do servidor, clique em **ferramentas**e clique em **NPS** para abrir o console NPS. 
2. Na árvore de console, clique duas vezes em **políticas**.
3. Clique com botão direito **diretivas de solicitação de Conexão**e clique em **nova diretiva de solicitação de Conexão**.
4. Use o novo Assistente de diretiva de solicitação de Conexão para configurar sua conexão diretiva de solicitação e, se não configurado anteriormente, um grupo de servidores remotos RADIUS.


Para obter mais informações sobre o gerenciamento de NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).

