---
title: Criar contas de usuário local
description: Saiba mais Processing thte três tipos de contas de usuário do MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: e2e5a6c79e6dcd603d19ca868df1d11fce2bf746
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823667"
---
# <a name="create-local-user-accounts"></a>Criar contas de usuário local
Três níveis de contas de usuário locais podem ser criados usando o MultiPoint Manager: Contas de usuário padrão; Usuários do multiPoint Dashboard, que têm direitos administrativos; e as contas de usuário administrativo completas.  
  
Use o procedimento a seguir para criar uma conta de usuário local em um servidor do MultiPoint. Se seu ambiente incluir vários servidores MultiPoint e você deseja que o usuário seja capaz de fazer logon em qualquer estação em qualquer servidor, você precisa criar uma conta de usuário local em cada um dos seus servidores. Se a configuração tem algumas limitações. Em um ambiente de domínio, você também pode permitir que os usuários usem suas contas de domínio. Para obter uma visão geral das suas opções, consulte [plano de contas de usuário para o seu ambiente Windows MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).  
   
1.  Faça logon no servidor como administrador e abra o MultiPoint Manager.  
  
2.  Clique o **os usuários** guia e, em seguida, clique em **adicionar conta de usuário**.  
  
    Abre o Assistente para adicionar conta de usuário.  
  
3.  Insira um nome de conta e senha para a nova conta de usuário e, em seguida, clique em **próxima**.  
  
4.  Selecione o tipo de conta de usuário que você deseja criar:  
  
    -   **Usuário padrão** - faça logon uma estação e executar tarefas de usuário, mas não tem acesso ao MultiPoint Manager ou o servidor do MultiPoint Dashboard e não é possível desligar o sistema.  
  
    -   **Usuário do multiPoint Dashboard** -tem direitos administrativos limitados. Um usuário do Dashboard pode abrir o painel e executar tarefas como registro em log os usuários do sistema ou desligar o computador do MultiPoint Server, mas o usuário não tem acesso ao Gerenciador do MultiPoint.  
  
    -   **Usuário administrativo** tem direitos administrativos completos no MultiPoint Server. Por exemplo, um usuário administrativo pode executar o Gerenciador do MultiPoint, adicionar e excluir usuários, modificar configurações do sistema e atualizar drivers.  
  
5.  Clique em **próxima**e, em seguida, clique em **concluir** para criar a conta de usuário.