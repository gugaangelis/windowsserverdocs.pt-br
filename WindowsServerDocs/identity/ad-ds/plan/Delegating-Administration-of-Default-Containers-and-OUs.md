---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Delegando administração de contêineres padrão e UOs
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d482854fd82b4bf0d0e61315d36e6222470ca55
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830887"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delegando administração de contêineres padrão e UOs

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada domínio do Active Directory contém um conjunto padrão de contêineres e UOs (unidades organizacionais) que são criadas durante a instalação dos serviços de domínio Active Directory (AD DS). Entre eles estão os seguintes:  
  
-   Contêiner de domínio, que serve como o contêiner raiz para a hierarquia  
  
-   Contêiner interno, que contém o padrão de contas de administrador de serviço  
  
-   Contêiner de usuários, que é o local padrão para novas contas de usuário e grupos criados no domínio  
  
-   Contêiner de computadores, que é o local padrão para novas contas de computador criadas no domínio  
  
-   OU de controladores de domínio, que é o local padrão para as contas de computador para contas de computador de controladores de domínio  
  
O proprietário da floresta controla esses contêineres padrão e UOs.  
  
## <a name="domain-container"></a>Contêiner de domínio  
O contêiner de domínio é o contêiner raiz da hierarquia de um domínio. Alterações nas políticas ou a lista de controle de acesso (ACL) neste contêiner potencialmente podem ter impacto de todo o domínio. Não é delegar o controle desse contêiner; ela deve ser controlada pelos administradores de serviço.  
  
## <a name="users-and-computers-containers"></a>Usuários e computadores contêineres  
Quando você executa uma atualização in-loco de domínio do Windows Server 2003 para o Windows Server 2008, os usuários e computadores existentes são colocados automaticamente em usuários e os contêineres de computadores. Se você estiver criando um novo contêineres de domínio, os usuários e computadores do Active Directory são os locais padrão para todas as novas contas de usuário e contas de computador não seja de controlador de domínio no domínio.  
  
> [!IMPORTANT]  
> Se você precisar delegar o controle sobre os usuários ou computadores, não modifique as configurações padrão, os usuários e computadores contêineres. Em vez disso, criar novas UOs (conforme necessário) e mover os objetos de computador e usuário de seus contêineres padrão e em novas UOs. Delegar o controle sobre as novas UOs, conforme necessário. É recomendável que você não modifique que controla os contêineres padrão.  
  
Além disso, você não pode aplicar configurações de diretiva de grupo para os computadores e usuários padrão contêineres. Para aplicar a diretiva de grupo para usuários e computadores, crie novas UOs e mover os objetos de usuário e computador para essas UOs. Aplique as configurações de diretiva de grupo para as novas UOs.  
  
Opcionalmente, você pode redirecionar a criação de objetos que são colocados em contêineres padrão a ser colocado em contêineres de sua escolha.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Well-Known usuários e grupos e contas internas  
Por padrão, vários usuários conhecidos e grupos e contas internas são criadas em um novo domínio. É recomendável que o gerenciamento dessas contas permanece sob o controle dos administradores de serviço. Não é delegar o gerenciamento dessas contas para uma pessoa que não seja um administrador de serviço. A tabela a seguir lista os bem conhecidos que os usuários e grupos e contas internas que devem permanecer sob o controle dos administradores de serviço.  
  
|Well-Known usuários e grupos|Contas internas|  
|--------------------------------|----------------------|  
|Editores de certificados<br /><br />Controladores de Domínio<br /><br />Proprietários criadores de política de grupo<br /><br />KRBTGT<br /><br />Convidados do Domínio<br /><br />Administrador<br /><br />Administradores do domínio<br /><br />Administradores de esquema (domínio raiz da floresta apenas)<br /><br />Administradores de empresa (domínio raiz da floresta apenas)<br /><br />Usuários do Domínio|Administrador<br /><br />Convidado<br /><br />Convidados<br /><br />Opers. de contas<br /><br />Administradores<br /><br />Operadores de cópia<br /><br />Criadores de confiança de floresta de entrada<br /><br />Operadores de Impressão<br /><br />Acesso Compatível com Versões Anteriores ao Windows 2000<br /><br />Opers. de servidores<br /><br />Usuários|  
  
## <a name="domain-controller-ou"></a>Unidade Organizacional do controlador de domínio  
Quando os controladores de domínio são adicionados ao domínio, os objetos de computador são adicionados automaticamente à UO de controlador de domínio. Essa UO tem um conjunto padrão de políticas aplicadas a ele. Para garantir que essas políticas são aplicadas uniformemente em todos os controladores de domínio, é recomendável que você não pode mover os objetos de computador dos controladores de domínio fora essa UO. Falha ao aplicar as políticas padrão pode causar um controlador de domínio não funcionar corretamente.  
  
Por padrão, os administradores de serviço controlam essa UO. Não é delegar o controle dessa UO para indivíduos que não sejam administradores de serviço.  
  


