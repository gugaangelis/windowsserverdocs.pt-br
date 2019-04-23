---
title: Considerações sobre conta de usuário
description: Fornece a conta de usuário, nome de usuário e considerações sobre a senha do MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 00fb5e83921ba0b8ad86a6f75bdfd7bf16419b73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850787"
---
# <a name="user-account-considerations"></a>Considerações sobre conta de usuário
Este tópico descreve problemas que você, como um usuário administrativo, deve considerar ao criar e gerenciar contas de usuário. Gerencie contas de usuário na guia usuários no Gerenciador do MultiPoint. Para obter mais informações, consulte o tópico [Gerenciar contas de usuário](Manage-User-Accounts.md).  
  
## <a name="user-account-types"></a>Tipos de conta de usuário  
Uma conta de usuário é uma coleção de informações que comunicam ao MultiPoint Services quais arquivos e pastas um usuário pode acessar, quais alterações ele pode fazer no sistema MultiPoint Services e as preferências de cada usuário, como tela de fundo da área de trabalho. Cada pessoa acessa sua própria conta de usuário usando um nome de usuário exclusivo e uma senha. MultiPoint Services dá suporte a três tipos de contas de usuário:  
  
-   **Contas de usuário administrativo** são para aqueles que usará o MultiPoint Manager para usar e gerenciar o sistema MultiPoint Services. Para obter mais informações, consulte [Criar uma conta de usuário administrativo](Create-an-Administrative-User-Account.md).  
  
-   **Contas de usuário padrão** são para aqueles que acessarão regularmente estações, mas não gerenciarão o sistema. Normalmente, você deve criar contas de usuário padrão para a maioria dos usuários de sistema MultiPoint Services. Para obter mais informações, consulte [criar uma conta de usuário padrão](Create-a-Standard-User-Account.md).  
  
-   **Contas de usuário do MultiPoint Dashboard** são para aqueles que usam o MultiPoint Dashboard para gerenciar sessões do usuário padrão e que podem fazer logon de qualquer estação. Para obter mais informações, consulte [Criar uma conta de usuário do MultiPoint Dashboard](Create-a-MultiPoint-Dashboard-User-Account.md).  
  
## <a name="user-name-and-password-considerations"></a>Considerações sobre nome de usuário e senha  
Usuários administrativos podem executar tarefas que afetam todos os outros usuários do sistema MultiPoint Services, como instalar software ou alterar as configurações de segurança. Por esse motivo, os usuários administrativos devem ter nomes de usuário exclusivos e senhas que só eles saibam.  
  
Uma consideração importante de contas de usuário é que cada conta de usuário é alocada a uma única biblioteca **Documentos** no Windows Explorer que inclui a pasta **Meus documentos**. Se os usuários padrão em seu sistema MultiPoint Services armazenarem documentos particulares em sua biblioteca **Documentos** no Windows Explorer, eles também deverão fazer logon no sistema MultiPoint Services usando um nome de usuário exclusivo e uma senha que só eles saibam. Para obter mais informações sobre como armazenar documentos no Windows Explorer, consulte o tópico [Gerenciar arquivos do usuário](Manage-User-Files.md).  
  
> [!TIP]  
> Para aumentar a segurança do sistema, as senhas de todos os usuários devem ser fortes. Uma senha forte é aquele que não pode ser difícil de ser adivinhada ou quebrada pelo menos oito caracteres, não contém todo ou parte do nome da conta do usuário e contém pelo menos três das quatro categorias de caracteres a seguir: maiusculas, letras minúsculas caracteres, números e símbolos encontrados em um teclado (como!, @, #).  
  
## <a name="see-also"></a>Consulte também  
[Criar uma conta de usuário administrativo](Create-an-Administrative-User-Account.md)  
[Criar uma conta de usuário padrão](Create-a-Standard-User-Account.md)  
[Gerenciar arquivos do usuário](Manage-User-Files.md)
[gerenciar contas de usuário](Manage-User-Accounts.md)