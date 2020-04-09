---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Delegando administração usando objetos da UO
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af13896c07c10710be6e087be5d31dbd4aec698f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822709"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delegando administração usando objetos da UO

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar UOs (unidades organizacionais) para delegar a administração de objetos, como usuários ou computadores, dentro da UO a um indivíduo ou grupo designado. Para delegar a administração usando uma UO, coloque a pessoa ou o grupo ao qual você está delegando direitos administrativos em um grupo, coloque o conjunto de objetos a serem controlados em uma UO e, em seguida, delegue tarefas administrativas para a UO para esse grupo.  
  
O Active Directory Domain Services (AD DS) permite que você controle as tarefas administrativas que podem ser delegadas em um nível muito detalhado. Por exemplo, você pode atribuir um grupo para ter controle total de todos os objetos em uma UO; Atribua outro grupo aos direitos somente para criar, excluir e gerenciar contas de usuário na UO; em seguida, atribua um terceiro grupo à direita apenas para redefinir senhas de conta de usuário. Você pode tornar essas permissões herdáveis para que elas se apliquem a quaisquer UOs colocadas em subárvores da UO original.  
  
As UOs e os contêineres padrão são criados durante a instalação do AD DS e são controlados pelos administradores de serviço. É melhor se os administradores de serviço continuarem a controlar esses contêineres. Se você precisar delegar controle sobre objetos no diretório, crie UOs adicionais e coloque os objetos nessas UOs. Delegue o controle sobre essas UOs aos administradores de dados apropriados. Isso possibilita delegar o controle sobre objetos no diretório sem alterar o controle padrão fornecido aos administradores de serviço.  
  
O proprietário da floresta determina o nível de autoridade delegado a um proprietário da UO. Isso pode variar da capacidade de criar e manipular objetos dentro da UO para ter permissão apenas para controlar um único atributo de um único tipo de objeto na UO. Conceder a um usuário a capacidade de criar um objeto na UO concede implicitamente a esse usuário a capacidade de manipular qualquer atributo de qualquer objeto que o usuário cria. Além disso, se o objeto criado for um contêiner, o usuário terá implicitamente a capacidade de criar e manipular todos os objetos que são colocados no contêiner.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Como delegar a administração de contêineres padrão e UOs](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Como delegar a administração das UOs de conta e de recurso](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


