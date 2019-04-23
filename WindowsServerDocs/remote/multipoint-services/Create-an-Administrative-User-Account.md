---
title: Criar uma conta de usuário administrativo
description: Criar uma conta com privilégios administrativos no MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce4c5a9-3dec-412f-910b-54a252f8f209
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: bf460107e57de5e19f8eaa311e497e9d984680e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879757"
---
# <a name="create-an-administrative-user-account"></a>Criar uma conta de usuário administrativo
Crie *contas de usuário administrativo* para as pessoas que gerenciarão o sistema MultiPoint Services. Para ver quem tem acesso administrativo, no Gerenciador do MultiPoint, clique o **usuários** guia. Contas de usuário administrativo são exibidas na coluna Tipo de conta como **Administrador**. *Os usuários administrativos* têm acesso a todas as tarefas do MultiPoint Manager que alteram as configurações de área de trabalho e do sistema, como:  
  
-   Criação de contas  
  
-   Adição e remoção de programas  
  
-   Gerenciamento de *áreas de trabalho* e hardware  
  
-   Encerramento de outras sessões do *usuário*  
  
Usuários administrativos podem executar tarefas que afetam todos os outros usuários do sistema MultiPoint Services, como instalar software ou alterar as configurações de segurança. Por esse motivo, os usuários administrativos devem ter nomes de usuário exclusivos e senhas que só eles saibam.  
  
Para obter mais informações sobre problemas que você, como usuário administrativo, deve considerar ao criar e gerenciar contas de usuário, consulte o tópico [Considerações sobre a conta de usuário](User-Account-Considerations.md).  
  
> [!NOTE]  
> Talvez seja conveniente criar uma *conta de usuário padrão* para usar ao executar tarefas no sistema MultiPoint Services que não estão relacionadas ao gerenciamento do sistema MultiPoint Services. Você deve apenas fazer logon em sua conta de usuário administrativo quando precisar executar tarefas de gerenciamento do sistema.  
  
#### <a name="to-create-an-administrative-user-account"></a>Para criar uma conta de usuário administrativo  
  
1.  No Gerenciador do MultiPoint, clique o **usuários** guia.  
  
2.  Em **Tarefas de usuários**, clique em **Adicionar conta de usuário**. O assistente **Adicionar conta de usuário** é aberto.  
  
3.  No campo **Conta de usuário**, digite um nome de logon para o usuário. Normalmente, o nome de usuário de logon é o nome e o sobrenome escritos juntos sem espaço ou a primeira inicial e o sobrenome escritos juntos sem espaço.  
  
4.  No campo **Nome completo**, digite o nome de usuário em qualquer formato que você preferir, como nome, nome completo ou apelido.  
  
5.  No campo **Senha**, digite uma senha para o usuário. A senha deve ser conhecida apenas por você e o usuário, e você deve armazenar essas informações em um local seguro. A senha só pode ser alterada por um usuário administrativo.  
  
6.  No campo **Confirmar senha**, digite a senha novamente e, em seguida, clique em **Avançar**.  
  
7.  No nível da página de acesso, selecione **Usuário administrativo** e, em seguida, clique em **Avançar**.  
  
8.  O MultiPoint Services verificará todas as informações e exibirá uma mensagem quando a conta tiver sido configurada. Quando você vir o texto **Uma nova conta de usuário foi criada com êxito**, clique em **Concluir**.  
