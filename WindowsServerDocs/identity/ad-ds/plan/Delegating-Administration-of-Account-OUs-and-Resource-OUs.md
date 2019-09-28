---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Delegando administração das UOs de conta e de recurso
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 63fda63d5a34404563bab44ee54ba2e22d852782
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402697"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delegando administração das UOs de conta e de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As UOs (unidades organizacionais) da conta contêm objetos de usuário, grupo e computador. As UOs de recurso contêm recursos e as contas que são responsáveis por gerenciar esses recursos. O proprietário da floresta é responsável por criar uma estrutura de UO para gerenciar esses objetos e recursos e delegar o controle dessa estrutura ao proprietário da UO.  
  
## <a name="delegating-administration-of-account-ous"></a>Delegando a administração de UOs de conta  
Delegue uma estrutura de UO de conta para administradores de dados se precisar criar e modificar objetos de usuário, grupo e computador. A estrutura da UO da conta é uma subárvore de UOs para cada tipo de conta que deve ser controlada de forma independente. Por exemplo, o proprietário da UO pode delegar controle específico a vários administradores de dados em unidades organizacionais filhas em uma UO de conta para usuários, computadores, grupos e contas de serviço.  
  
A ilustração a seguir mostra um exemplo de uma estrutura de UO de conta.  
  
![Delegando a administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
A tabela a seguir lista e descreve as possíveis UOs filhas que você pode criar em uma estrutura de UO de conta.  
  
|OU|Finalidade|  
|------|-----------|  
|Usuários|Contém contas de usuário para funcionários não administrativos.|  
|Contas de serviço|Alguns serviços que exigem acesso a recursos de rede são executados como contas de usuário. Essa UO é criada para separar contas de usuário de serviço das contas de usuário contidas na UO users. Além disso, colocar os diferentes tipos de contas de usuário em UOs separadas permite que você gerencie-as de acordo com seus requisitos administrativos específicos.|  
|Computadores|Contém contas para computadores que não sejam controladores de domínio.|  
|Grupos|Contém grupos de todos os tipos, exceto grupos administrativos, que são gerenciados separadamente.|  
|Admins|Contém contas de usuário e grupo para administradores de dados na estrutura da UO da conta para permitir que eles sejam gerenciados separadamente de usuários comuns. Habilite a auditoria para essa UO para que você possa controlar as alterações feitas em usuários e grupos administrativos.|  
  
A ilustração a seguir mostra um exemplo de um design de grupo administrativo para uma estrutura de UO de conta.  
  
![Delegando a administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Os grupos que gerenciam as UOs filhas recebem controle total apenas sobre a classe específica de objetos que eles são responsáveis por gerenciar.  
  
Os tipos de grupos que você usa para delegar controle dentro de uma estrutura de UO são baseados em onde as contas estão localizadas em relação à estrutura da UO a ser gerenciada. Se as contas de usuário administrador e a estrutura da UO existirem em um único domínio, os grupos que você criar para serem usados para delegação deverão ser grupos globais. Se sua organização tiver um departamento que gerencia suas próprias contas de usuário e existir em mais de uma região geográfica, você poderá ter um grupo de administradores de dados que são responsáveis pelo gerenciamento de UOs de conta em mais de um domínio. Se as contas dos administradores de dados existirem em um único domínio e você tiver estruturas de UO em vários domínios para os quais você precisa delegar o controle, torne essas contas administrativas membros de grupos globais e delegue o controle das estruturas de UO em cada domínio para esses grupos globais. Se as contas de administradores de dados para as quais você delega o controle de uma estrutura de UO são provenientes de vários domínios, você deve usar um grupo universal. Os grupos universais podem conter usuários de diferentes domínios e, portanto, podem ser usados para delegar o controle em vários domínios.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delegando a administração de UOs de recursos  
As UOs de recurso são usadas para gerenciar o acesso aos recursos. O proprietário da UO do recurso cria contas de computador para servidores que ingressaram no domínio que incluem recursos como compartilhamentos de arquivos, bancos de dados e impressoras. O proprietário da UO do recurso também cria grupos para controlar o acesso a esses recursos.  
  
A ilustração a seguir mostra os dois locais possíveis para a UO de recurso.  
  
![Delegando a administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
A UO de recursos pode estar localizada na raiz do domínio ou como uma UO filho da UO da conta correspondente na hierarquia administrativa da UO. As UOs de recurso não têm nenhuma UO filho padrão. Os computadores e grupos são colocados diretamente na UO do recurso.  
  
O proprietário da UO do recurso possui os objetos dentro da UO, mas não possui o próprio contêiner da UO. Os proprietários de UO de recursos gerenciam somente objetos de computador e grupo; Eles não podem criar outras classes de objetos dentro da UO e não podem criar UOs filhas.  
  
> [!NOTE]  
> O criador ou proprietário de um objeto tem a capacidade de definir a ACL (lista de controle de acesso) no objeto, independentemente das permissões que são herdadas do contêiner pai. Se um proprietário da UO do recurso puder redefinir a ACL em uma UO, esse proprietário poderá criar qualquer classe de objeto na UO, incluindo os usuários. Por esse motivo, os proprietários da UO de recursos não têm permissão para criar UOs.  
  
Para cada UO de recurso no domínio, crie um grupo global para representar os administradores de dados que são responsáveis pelo gerenciamento do conteúdo da UO. Esse grupo tem controle total sobre os objetos de computador e grupo na UO, mas não sobre o próprio contêiner da UO.  
  
A ilustração a seguir mostra o design do grupo administrativo para uma UO de recurso.  
  
![Delegando a administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Colocar as contas de computador em uma UO de recurso fornece o controle de proprietário da UO sobre os objetos de conta, mas não torna o proprietário da UO um administrador dos computadores. Em um domínio Active Directory, o grupo Admins. do domínio é, por padrão, colocado no grupo Administradores local em todos os computadores. Ou seja, os administradores de serviço têm controle sobre esses computadores. Se os proprietários da UO do recurso exigirem controle administrativo sobre os computadores em suas UOs, o proprietário da floresta poderá aplicar grupos restritos Política de Grupo para tornar o proprietário da UO do recurso um membro do grupo Administradores nos computadores dessa UO.  
  


