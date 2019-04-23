---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Delegando administração das UOs de conta e de recurso
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70900b44de85774e8e595f9691885d67682fa753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834257"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delegando administração das UOs de conta e de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conta OUs (unidades organizacionais) contêm os objetos de usuário, grupo e computador. UOs de recursos contém recursos e as contas que são responsáveis por gerenciar esses recursos. O proprietário da floresta é responsável por criar uma estrutura de UO para gerenciar esses objetos e recursos e para delegar o controle dessa estrutura para o proprietário de UO.  
  
## <a name="delegating-administration-of-account-ous"></a>Delegando a administração da conta de UOs  
Delegar uma estrutura de UO de conta para os administradores de dados caso eles precisem criar e modificar objetos de usuário, grupo e computador. A estrutura de UO de conta é uma subárvore de UOs para cada tipo de conta deve ser controlado independentemente. Por exemplo, o proprietário da UO pode delegar o controle específico para vários administradores de dados sobre UOs filho em uma UO de conta para usuários, computadores, grupos e contas de serviço.  
  
A ilustração a seguir mostra um exemplo de uma estrutura de UO de conta.  
  
![delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
A tabela a seguir lista e descreve as UOs filho possíveis que você pode criar em uma conta de estrutura de UO.  
  
|OU|Finalidade|  
|------|-----------|  
|Usuários|Contém contas de usuário para a equipe não administrativas.|  
|Contas de serviço|Alguns serviços que exigem acesso aos recursos de rede são executados como contas de usuário. Essa UO é criada para contas de usuário de serviço separado de contas de usuário contidas na UO de usuários. Além disso, colocar os diferentes tipos de contas de usuário em UOs separadas permite que você gerenciá-los de acordo com suas necessidades administrativas específicas.|  
|Computadores|Contém contas dos computadores que não sejam controladores de domínio.|  
|Grupos|Contém grupos de todos os tipos, exceto para os grupos administrativos, que são gerenciados separadamente.|  
|Admins|Contém contas de usuário e grupo para administradores de dados na conta de estrutura de UO permitir que eles sejam gerenciados separadamente dos usuários regulares. Habilite a auditoria para essa UO para que você possa acompanhar alterações aos grupos e usuários administrativos.|  
  
A ilustração a seguir mostra um exemplo de um design de grupo administrativo para uma conta de estrutura de UO.  
  
![delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Grupos que gerencia a UO filha recebem controle completo somente sobre a classe específica de objetos que eles são responsáveis por gerenciar.  
  
Os tipos de grupos que podem ser usados para delegar o controle dentro de uma estrutura de UO se baseiam em que as contas estão localizadas em relação a estrutura de UO que deve ser gerenciado. Se as contas de usuário administrador e a estrutura de UO existirem em um único domínio, os grupos que você cria para usar para a delegação devem ser grupos globais. Se sua organização tiver um departamento que gerencia suas próprias contas de usuário e existe em mais de uma região geográfica, você pode ter um grupo de administradores de dados que são responsáveis por gerenciar UOs em mais de um domínio de conta. Se as contas dos administradores todos os dados existem em um único domínio e ter estruturas de UO em vários domínios para o qual você precisará delegar o controle, tornar esses membros de contas administrativas de grupos globais e delegar o controle das estruturas de UO em cada domínio a esses grupos globais. Se as contas de administradores de dados ao qual você delega o controle de uma estrutura de UO provém de vários domínios, você deve usar um grupo universal. Grupos universais podem conter usuários de domínios diferentes e, portanto, pode ser usados para delegar o controle em vários domínios.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delegar a administração de UOs de recursos  
UOs de recursos são usadas para gerenciar o acesso aos recursos. O proprietário do recurso UO cria contas de computador para servidores que ingressaram no domínio que incluem recursos como compartilhamentos de arquivos, bancos de dados e impressoras. O proprietário do recurso UO também cria grupos para controlar o acesso a esses recursos.  
  
A ilustração a seguir mostra os dois locais possíveis para o recurso de UO.  
  
![delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
O recurso pode ser localizada na raiz do domínio ou como um filho de UO da conta correspondente UO em sua hierarquia administrativa de UO. UOs de recursos não têm todas as OUs filho padrão. Computadores e grupos são colocados diretamente no recurso de UO.  
  
O proprietário do recurso OU possui os objetos na UO mas não possuir o contêiner de unidade Organizacional em si. Os proprietários do recurso OU gerenciar apenas o computador e objetos de grupo; eles não é possível criar outras classes de objetos na UO e eles não podem criar UOs filho.  
  
> [!NOTE]  
> O criador ou proprietário de um objeto tem a capacidade de definir a lista de controle de acesso (ACL) no objeto, independentemente das permissões que são herdadas do contêiner pai. Se um proprietário de UO do recurso pode redefinir a ACL em uma unidade Organizacional, o proprietário pode criar qualquer classe de objeto na UO, incluindo usuários. Por esse motivo, os proprietários de UO de recursos não são permitidos para criar unidades organizacionais.  
  
Para cada recurso UO no domínio, crie um grupo global para representar os administradores de dados que são responsáveis por gerenciar o conteúdo da UO. Esse grupo tem controle total sobre os objetos de computador e grupo na UO mas não sobre o próprio contêiner de unidade Organizacional.  
  
A ilustração a seguir mostra o design de grupo administrativo para um recurso de UO.  
  
![delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Colocar as contas de computador em um recurso de UO dá o controle de proprietário OU sobre os objetos de conta, mas não torna o proprietário OU administrador dos computadores. Em um domínio do Active Directory, o grupo Admins. do domínio é, por padrão, colocado no grupo de administradores local em todos os computadores. Ou seja, os administradores de serviço tem controle sobre esses computadores. Se os proprietários de UO de recursos exigem controle administrativo sobre os computadores em suas OUs, o proprietário da floresta pode aplicar uma política de grupo grupos restritos para tornar o proprietário do recurso OU um membro do grupo Administradores nos computadores na UO.  
  


