---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: "Delegar a administração de conta UOs e UOs de recursos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56e69bffdf8ecc0f37f9f5159ae6bce7fcdc49f8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delegar a administração de conta UOs e UOs de recursos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Unidades de conta organizacionais (UOs) contêm objetos de usuário, grupo e computador. UOs de recursos contêm recursos e as contas que são responsáveis pelo gerenciamento desses recursos. O proprietário floresta é responsável por criar uma estrutura de UO para gerenciar esses objetos e recursos e delegando o controle de estrutura para o proprietário de UO.  
  
## <a name="delegating-administration-of-account-ous"></a>Delegar a administração de conta UOs  
Delegar uma estrutura de UO conta aos administradores de dados se eles precisam criar e modificar objetos de usuário, grupo e computador. A estrutura de UO de conta é uma subárvore de UOs para cada tipo de conta deve ser controlada de forma independente. Por exemplo, o proprietário de UO pode delegar controle específico a vários administradores de dados sobre UOs filho em uma conta OU para usuários, computadores, grupos e contas de serviço.  
  
A ilustração a seguir mostra um exemplo de uma estrutura de UO de conta.  
  
![Delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
A tabela a seguir lista e descreve as UOs filho possível que você pode criar em uma estrutura de UO de conta.  
  
|UO|Finalidade|  
|------|-----------|  
|Usuários|Contém contas de usuário para a equipe de não administrativa.|  
|Contas de serviço|Alguns serviços que exigem acesso aos recursos de rede são executados como contas de usuário. Essa UO é criado para contas de usuário separada do serviço de contas de usuário contidas na UO os usuários. Além disso, colocar os diferentes tipos de contas de usuários em UOs separadas permite que você gerenciá-los de acordo com suas necessidades administrativas específicas.|  
|Computadores|Contém contas para computadores que não sejam controladores de domínio.|  
|Grupos|Contém grupos de todos os tipos com exceção de grupos administrativos, que são gerenciados separadamente.|  
|Administradores|Contém contas de usuário e grupo de administradores de dados na conta de estrutura de UO permitir que eles sejam gerenciados separadamente de usuários regulares. Habilite a auditoria para essa UO para que você pode controlar as alterações de grupos e usuários administrativos.|  
  
A ilustração a seguir mostra um exemplo de um design de grupo administrativo de uma estrutura de UO de conta.  
  
![Delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Grupos que gerenciam as UOs filho são concedidos controle total apenas sobre a classe específica de objetos que eles são responsáveis pelo gerenciamento.  
  
Os tipos de grupos que você usa para delegar controle dentro de uma estrutura de UO se baseiam em que as contas estão localizadas em relação a estrutura de UO que deve ser gerenciado. Se as contas de usuário administrador e a estrutura de UO existirem dentro de um único domínio, os grupos que você cria para usar para delegação devem ser grupos globais. Se sua organização tiver um departamento que gerencia suas próprias contas de usuário e existe em mais de uma região geográfica, você pode ter um grupo de administradores de dados que são responsáveis pelo gerenciamento de conta UOs em mais de um domínio. Se as contas dos administradores todos os dados existem em um único domínio e você tiver estruturas de UO em vários domínios ao qual você precisa fazer delegar controle, tornar esses membros de contas de administrador de grupos globais e delegar o controle das estruturas de UO em cada domínio para esses grupos globais. Se as contas de administradores de dados ao qual você delega o controle de uma estrutura de UO vêm de vários domínios, você deve usar um grupo universal. Grupos universais podem conter os usuários de domínios diferentes e, portanto, eles podem ser usados para delegar controle em vários domínios.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delegar a administração de UOs de recursos  
UOs de recursos são usados para gerenciar o acesso aos recursos. O proprietário do recurso UO cria contas de computador para servidores que ingressam no domínio que incluem recursos como compartilhamentos de arquivos, bancos de dados e impressoras. Além disso, o proprietário do recurso UO cria grupos para controlar o acesso a esses recursos.  
  
A ilustração a seguir mostra os dois locais possíveis para o recurso UO.  
  
![Delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
O recurso de UO pode ser localizado na raiz do domínio ou como um filho UO da conta correspondente unidade Organizacional na hierarquia administrativa UO. UOs de recursos não têm todas as OUs filho padrão. Computadores e os grupos são colocados diretamente no recurso UO.  
  
O proprietário do recurso UO possui objetos na unidade Organizacional, mas não possui o contêiner de UO em si. Os proprietários do recurso OU gerenciar somente computador e objetos de grupo; eles não podem criar outras classes de objetos dentro de unidade Organizacional e eles não podem criar UOs filho.  
  
> [!NOTE]  
> O criador ou o proprietário de um objeto tem a capacidade de definir a lista de controle de acesso (ACL) no objeto independentemente das permissões que são herdadas de contêiner pai. Se um proprietário do recurso UO pode redefinir a ACL em uma unidade Organizacional, esse proprietário pode criar qualquer classe de objeto na UO, incluindo os usuários. Por esse motivo, os proprietários de UO de recursos não são permitidos para criar unidades organizacionais.  
  
Para cada recurso OU no domínio, crie um grupo global para representar os administradores de dados que serão responsáveis por gerenciar o conteúdo da unidade Organizacional. Esse grupo tem controle total sobre os objetos de grupo e o computador na UO, mas não sobre o contêiner de UO em si.  
  
A ilustração a seguir mostra o design de grupo administrativo de um recurso de UO.  
  
![Delegação de administração](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Colocar as contas de computador em um recurso UO dá o controle do proprietário de UO sobre os objetos de conta, mas não faz o proprietário OU um administrador dos computadores. Em um domínio do Active Directory, o grupo Admins. do domínio é, por padrão, colocado no grupo local Administradores em todos os computadores. Ou seja, os administradores de serviço têm controle sobre esses computadores. Se os proprietários de UO de recursos exigem controle administrativo sobre os computadores em suas unidades organizacionais, o proprietário da floresta pode aplicar uma política de grupo grupos restritos para tornar o proprietário do recurso OU um membro do grupo Administradores nos computadores nessa UO.  
  


