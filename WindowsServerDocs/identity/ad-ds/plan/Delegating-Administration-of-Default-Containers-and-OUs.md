---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Delegando administração de contêineres padrão e UOs
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1bce2ce312b3105d3c347da1e491601bf15364e7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947684"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delegando administração de contêineres padrão e UOs

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada domínio de Active Directory contém um conjunto padrão de contêineres e UOs (unidades organizacionais) que são criados durante a instalação do Active Directory Domain Services (AD DS). Entre elas estão as seguintes:

-   Contêiner de domínio, que serve como o contêiner raiz para a hierarquia

-   Contêiner interno, que contém as contas de administrador de serviço padrão

-   Contêiner de usuários, que é o local padrão para novas contas de usuário e grupos criados no domínio

-   Contêiner de computadores, que é o local padrão para novas contas de computador criadas no domínio

-   Controlador de domínio OU, que é o local padrão para as contas de computador para controladores de domínio contas de computador

O proprietário da floresta controla esses contêineres e UOs padrão.

## <a name="domain-container"></a>Contêiner de domínio
O contêiner de domínio é o contêiner raiz da hierarquia de um domínio. As alterações nas políticas ou na ACL (lista de controle de acesso) desse contêiner podem potencialmente ter impacto em todo o domínio. Não delegar controle desse contêiner; Ele deve ser controlado pelos administradores de serviço.

## <a name="users-and-computers-containers"></a>Contêineres de usuários e computadores
Quando você executa uma atualização de domínio in-loco do Windows Server 2003 para o Windows Server 2008, os usuários e computadores existentes são colocados automaticamente nos contêineres usuários e computadores. Se você estiver criando um novo domínio de Active Directory, os contêineres usuários e computadores serão os locais padrão para todas as novas contas de usuário e contas de computador não controlador de domínio no domínio.

> [!IMPORTANT]
> Se você precisar delegar controle sobre usuários ou computadores, não modifique as configurações padrão nos contêineres usuários e computadores. Em vez disso, crie novas UOs (conforme necessário) e mova os objetos de usuário e computador de seus contêineres padrão e para as novas UOs. Delegue o controle sobre as novas UOs, conforme necessário. É recomendável que você não modifique quem controla os contêineres padrão.

Além disso, você não pode aplicar Política de Grupo configurações aos contêineres usuários e computadores padrão. Para aplicar Política de Grupo a usuários e computadores, crie novas UOs e mova os objetos de usuário e computador para essas UOs. Aplique as configurações de Política de Grupo às novas UOs.

Opcionalmente, você pode redirecionar a criação de objetos que são colocados nos contêineres padrão a serem colocados em contêineres de sua escolha.

## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Usuários e grupos bem conhecidos e contas internas
Por padrão, vários usuários e grupos bem conhecidos e contas internas são criados em um novo domínio. É recomendável que o gerenciamento dessas contas permaneça sob o controle dos administradores de serviço. Não delegar o gerenciamento dessas contas a um indivíduo que não seja um administrador de serviços. A tabela a seguir lista os usuários e grupos conhecidos e contas internas que precisam permanecer sob o controle dos administradores de serviço.

|Usuários e grupos bem conhecidos|Contas internas|
|--------------------------------|----------------------|
|Editores de Certificados<p>Controladores de Domínio<p>Proprietários criadores de política de grupo<p>KRBTGT<p>Convidados do Domínio<p>Administrador<p>Administradores de Domínio<p>Administradores de esquema (somente domínio raiz da floresta)<p>Administradores corporativos (somente domínio raiz da floresta)<p>Usuários do Domínio|Administrador<p>Convidado<p>Convidados<p>Opers. de contas<p>Administradores<p>Operadores de cópia<p>Criadores de confiança de floresta de entrada<p>Operadores de Impressão<p>Acesso compatível com versões anteriores ao Windows 2000<p>Opers. de servidores<p>Usuários|

## <a name="domain-controller-ou"></a>UO do controlador de domínio
Quando os controladores de domínio são adicionados ao domínio, seus objetos de computador são adicionados automaticamente à UO do controlador de domínio. Essa UO tem um conjunto padrão de políticas aplicadas a ela. Para garantir que essas políticas sejam aplicadas uniformemente a todos os controladores de domínio, recomendamos que você não mova os objetos de computador dos controladores de domínio dessa UO. A falha ao aplicar as políticas padrão pode fazer com que um controlador de domínio falhe para funcionar corretamente.

Por padrão, os administradores de serviço controlam essa UO. Não Delegue o controle dessa UO a indivíduos diferentes dos administradores de serviço.



