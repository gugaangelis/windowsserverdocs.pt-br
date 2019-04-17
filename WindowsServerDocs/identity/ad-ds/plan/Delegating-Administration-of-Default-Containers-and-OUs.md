---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: "Delegar a administração de contêineres padrão e UOs"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2504208210c03193451d19478f3bc8c98ec98f23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delegar a administração de contêineres padrão e UOs

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada domínio do Active Directory contém um conjunto padrão de contêineres e unidades organizacionais (UOs) que são criadas durante a instalação dos serviços de domínio do Active Directory (AD DS). Isso inclui o seguinte:  
  
-   Contêiner de domínio, o que serve como o contêiner de raiz para a hierarquia  
  
-   Contêiner interno, que contém o padrão de contas de administrador de serviço  
  
-   Contêiner de usuários, o que é o local padrão para novas contas de usuário e os grupos criados no domínio  
  
-   Contêiner de computadores, o que é o local padrão para novas contas de computador criadas no domínio  
  
-   UO de controladores de domínio, que é o local padrão para as contas de computador para contas de computador de controladores de domínio  
  
O proprietário da floresta controla esses contêineres padrão e unidades organizacionais.  
  
## <a name="domain-container"></a>Contêiner de domínio  
O contêiner de domínio é o contêiner de raiz da hierarquia de um domínio. Alterações feitas as políticas ou a lista de controle de acesso (ACL) nesse contêiner potencialmente podem ter impacto de todo o domínio. Não delegar controle desse contêiner; ele deve ser controlado pelos administradores de serviço.  
  
## <a name="users-and-computers-containers"></a>Usuários e computadores contêineres  
Quando você realizar uma atualização in-loco domínio do Windows Server 2003 para o Windows Server 2008, os usuários e computadores existentes são colocados automaticamente para os usuários e os contêineres de computadores. Se você estiver criando um novo contêineres de domínio, os usuários e computadores do Active Directory são os locais padrão para todas as novas contas de usuário e contas de computador não controlador de domínio no domínio.  
  
> [!IMPORTANT]  
> Se você precisar delegar controle sobre os usuários ou computadores, não modifique as configurações padrão nos computadores e usuários contêineres. Em vez disso, crie novas OUs (conforme necessário) e mover os objetos de usuário e o computador de seus contêineres padrão para as UOs novo. Delegar controle sobre as UOs novo, conforme necessário. Recomendamos que você não modifique quem controla os contêineres padrão.  
  
Além disso, você não pode aplicar configurações de política de grupo para o padrão que os usuários e computadores contêineres. Para aplicar a política de grupo para usuários e computadores, crie novas UOs e mova os objetos de usuário e computador nessas unidades organizacionais. Aplique as configurações de política de grupo para as UOs de novo.  
  
Opcionalmente, você pode redirecionar a criação de objetos que são colocados nos contêineres padrão deve ser colocado em contêineres de sua escolha.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Conhecidos usuários e grupos e contas internas  
Por padrão, vários usuários bem conhecidos e grupos e contas internas são criadas em um novo domínio. Recomendamos que o gerenciamento dessas contas permaneça sob o controle de administradores de serviços. Gerenciamento dessas contas não delegar para uma pessoa que não é um administrador de serviço. A tabela a seguir lista os usuários bem conhecidos e grupos e contas internas que devem permanecer sob o controle de administradores de serviços.  
  
|Grupos e usuários conhecidos|Contas internas|  
|--------------------------------|----------------------|  
|Editores de certificados<br /><br />Controladores de domínio<br /><br />Grupo Proprietários criadores de política<br /><br />KRBTGT<br /><br />Convidados do domínio<br /><br />Administrador<br /><br />Administradores de domínio<br /><br />Administradores de esquema (domínio raiz da floresta somente)<br /><br />Administradores corporativos (domínio raiz da floresta somente)<br /><br />Usuários de domínio|Administrador<br /><br />Convidado<br /><br />Convidados<br /><br />Operadores de conta<br /><br />Administradores<br /><br />Operadores de backup<br /><br />Criadores de confiança de floresta de entrada<br /><br />Operadores de impressão<br /><br />Acesso de compatível com versões anteriores ao Windows 2000<br /><br />Operadores de servidor<br /><br />Usuários|  
  
## <a name="domain-controller-ou"></a>OU controlador de domínio  
Quando controladores de domínio são adicionados ao domínio, seus objetos de computador são adicionados automaticamente à UO de controlador de domínio. Essa UO tem um conjunto padrão de políticas aplicadas a ela. Para garantir que essas políticas são aplicadas de maneira uniforme para todos os controladores de domínio, recomendamos que você não pode mover os objetos de computador dos controladores de domínio sem essa UO. Falha ao aplicar as políticas padrão pode causar um controlador de domínio não funcionar corretamente.  
  
Por padrão, os administradores de serviços controlam essa UO. Não delegar controle dessa ou a indivíduos que não sejam administradores de serviços.  
  


