---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: "Delegar a administração usando objetos de UO"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d0b4765304d8b302fc174c191af2c8e87a25304
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delegar a administração usando objetos de UO

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar unidades organizacionais (UOs) para delegar a administração de objetos, como os usuários ou computadores, dentro OU para um indivíduo designado ou grupo. Para delegar a administração usando uma unidade Organizacional, coloque o indivíduo ou grupo ao qual você está delegando direitos administrativos em um grupo, coloque o conjunto de objetos para ser controlados em uma unidade Organizacional e, em seguida, delegar tarefas administrativas para a unidade Organizacional para esse grupo.  
  
Os serviços de domínio do Active Directory (AD DS) permite que você controle as tarefas administrativas que podem ser recebidas em um nível muito detalhado. Por exemplo, você pode atribuir um grupo ter controle total de todos os objetos em uma UO; atribuir outro grupo os direitos somente para criar, excluir e gerenciar contas de usuário na UO; e, em seguida, atribua um terceiro grupo o direito somente para redefinir as senhas de conta de usuário. Você pode fazer essas permissões herdadas para que eles se aplicam a qualquer UOs são colocados em subárvores da UO original.  
  
UOs de padrão e contêineres são criados durante a instalação do AD DS e são controladas por administradores de serviços. É melhor se continuam administradores de serviços controlar esses contêineres. Se você precisar delegar controle sobre objetos no diretório, crie UOs adicionais e coloque os objetos nessas UOs. Delegar controle sobre esses UOs aos administradores de dados adequado. Isso torna possível delegar controle sobre objetos no diretório sem alterar o controle padrão fornecido para os administradores de serviços.  
  
O proprietário da floresta determina o nível de autoridade seja delegada a um proprietário de UO. Isso pode variar desde a capacidade de criar e manipular objetos dentro da OU pode somente para controlar um único atributo de um único tipo de objeto na UO. Concessão de um usuário a capacidade de criar um objeto na UO implicitamente concede a esse usuário a capacidade de manipular qualquer atributo de qualquer objeto que o usuário cria. Além disso, se o objeto que é criado é um contêiner, o usuário implicitamente tem a capacidade de criar e manipular todos os objetos que são colocados no contêiner.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Delegar a administração de contêineres padrão e UOs](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delegar a administração de conta UOs e UOs de recursos](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


