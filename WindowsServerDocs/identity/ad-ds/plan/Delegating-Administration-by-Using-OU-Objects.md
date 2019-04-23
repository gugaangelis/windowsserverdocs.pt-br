---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Delegando administração usando objetos da UO
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39c4278270e4ab4fba9ff1062d2aa043d203a74b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886937"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delegando administração usando objetos da UO

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar unidades organizacionais (OUs) para delegar a administração de objetos, como usuários ou computadores, dentro da OU a uma pessoa designada ou grupo. Para delegar a administração por meio de uma unidade Organizacional, coloque o indivíduo ou grupo ao qual você está delegando direitos administrativos em um grupo, coloque o conjunto de objetos a ser controlado em uma unidade Organizacional e, em seguida, delegar tarefas administrativas para a UO para esse grupo.  
  
Os serviços de domínio do Active Directory (AD DS) permite que você controle as tarefas administrativas que podem ser delegadas em um nível muito detalhado. Por exemplo, você pode atribuir um grupo para ter controle total sobre todos os objetos em uma UO; atribuir os direitos somente para criar, excluir e gerenciar contas de usuário na UO; de outro grupo e, em seguida, atribuir um terceiro grupo o direito de apenas redefinir senhas de contas de usuário. Você pode fazer essas permissões herdáveis para que elas se aplicam a todas as organizacionais que são colocadas em subárvores da UO original.  
  
Padrão de UOs e contêineres são criados durante a instalação do AD DS e são controlados por administradores de serviço. É melhor se os administradores de serviço continuam a controlar esses contêineres. Se você precisar delegar o controle sobre objetos no diretório, criar UOs adicionais e coloque os objetos nessas UOs. Delegar o controle sobre esses UOs para os administradores de dados apropriado. Isso torna possível delegar o controle sobre os objetos no diretório sem alterar o controle padrão fornecido para os administradores de serviço.  
  
O proprietário da floresta determina o nível de autoridade delegada para um proprietário de UO. Isso pode variar desde a capacidade de criar e manipular objetos na UO para apenas terem permissão para controlar um único atributo de um único tipo de objeto na UO. Conceder a um usuário a capacidade de criar um objeto na UO implicitamente concede ao usuário a capacidade de manipular qualquer atributo de qualquer objeto que o usuário cria. Além disso, se o objeto criado é um contêiner, o usuário implicitamente tem a capacidade de criar e manipular todos os objetos que são colocados no contêiner.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Delegar a administração de contêineres padrão e UOs](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delegar a administração de conta de UOs e UOs de recursos](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


