---
title: Criar contas de usuário local
description: Saiba mais sobr thte três tipos de contas de usuário nos serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: 874d9cbdb56c59693cb5843a898b9ebd6893d674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405105"
---
# <a name="create-local-user-accounts"></a>Criar contas de usuário local
Três níveis de contas de usuário local podem ser criados no usando o Gerenciador do MultiPoint: Contas de usuário padrão; Usuários do painel do MultiPoint, que têm direitos administrativos limitados; e contas de usuário administrativo completas.  
  
Use o procedimento a seguir para criar uma conta de usuário local em um servidor MultiPoint. Se o seu ambiente incluir vários MultiPoint Servers e você quiser que o usuário seja capaz de fazer logon em qualquer estação em qualquer servidor, você precisará criar uma conta de usuário local em cada um dos servidores. Essa instalação tem algumas limitações. Em um ambiente de domínio, você também pode permitir que os usuários usem suas contas de domínio. Para obter uma visão geral das suas opções, consulte [planejar contas de usuário para o ambiente do Windows MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).  
   
1.  Faça logon no servidor como administrador e abra o Gerenciador do MultiPoint.  
  
2.  Clique na guia **usuários** e, em seguida, clique em **adicionar conta de usuário**.  
  
    O assistente para adicionar conta de usuário é aberto.  
  
3.  Insira um nome de conta e uma senha para a nova conta de usuário e clique em **Avançar**.  
  
4.  Selecione o tipo de conta de usuário que você deseja criar:  
  
    -   **Usuário padrão** – pode fazer logon em uma estação e executar tarefas do usuário, mas não tem acesso ao MultiPoint Manager ou ao painel do MultiPoint Server e não pode desligar o sistema.  
  
    -   **Usuário do painel do MultiPoint** -tem direitos administrativos limitados. Um usuário do painel pode abrir o painel e executar tarefas como fazer o logoff de usuários do sistema ou desligar o computador do MultiPoint Server, mas o usuário não tem acesso ao Gerenciador do MultiPoint.  
  
    -   **Usuário administrativo** Tem direitos administrativos completos no MultiPoint Server. Por exemplo, um usuário administrativo pode executar o MultiPoint Manager, adicionar e excluir usuários, modificar configurações do sistema e atualizar drivers.  
  
5.  Clique em **Avançar**e em **concluir** para criar a conta de usuário.