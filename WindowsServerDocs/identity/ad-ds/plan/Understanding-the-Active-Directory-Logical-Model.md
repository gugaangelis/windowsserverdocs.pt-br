---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: "Noções básicas sobre o modelo lógico do Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f8cdc4789d1b3008f3b53104e5517d4ef1e65b9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="understanding-the-active-directory-logical-model"></a>Noções básicas sobre o modelo lógico do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Projetando a estrutura lógica para os serviços de domínio do Active Directory (AD DS) envolve definindo as relações entre os contêineres em seu diretório. Esses relacionamentos podem ser baseados em requisitos administrativos, como delegação de autoridade, ou eles podem ser definidos dos requisitos operacionais, como a necessidade de controlar a replicação.  
  
Antes de criar a estrutura lógica do Active Directory, é importante entender o modelo lógico do Active Directory. AD DS é um banco de dados distribuído que armazena e gerencia informações sobre recursos de rede, bem como dados específicos do aplicativo de aplicativos habilitados para diretórios. AD DS permite que os administradores organizar os elementos de uma rede (por exemplo, os usuários, computadores e dispositivos) em uma estrutura hierárquica de contenção. O contêiner de nível superior é floresta. Nas florestas são domínios e em domínios são unidades organizacionais (UOs). Isso é chamado de modelo lógico porque ele é independente dos aspectos físicos da implantação, como o número de controladores de domínio necessárias em cada domínio e topologia de rede.  
  
## <a name="active-directory-forest"></a>Floresta do Active Directory  
Uma floresta é uma coleção de um ou mais domínios do Active Directory que compartilham uma estrutura lógica comum, esquema de diretório (definições de classe e atributo), a configuração de diretório (informações de site e replicação) e catálogo global (recursos de pesquisa de toda a floresta). Domínios na mesma floresta serão vinculados automaticamente relações de confiança transitiva bidirecional.  
  
## <a name="active-directory-domain"></a>Domínio do Active Directory  
Um domínio é uma partição em uma floresta do Active Directory. Dados de particionamento permite que as organizações replicar dados apenas para onde são necessários. Dessa forma, o diretório pode dimensionar globalmente em uma rede que tem largura de banda disponível limitada. Além disso, o domínio dá suporte a um número de outro core funções relacionadas à administração, incluindo:  
  
-   Identidade do usuário de toda a rede. Os domínios permitem que identidades do usuário ser criados uma vez e referenciados em qualquer computador que tenha ingressado em floresta em que o domínio está localizado. Controladores de domínio que compõem a um domínio são usados para armazenar contas de usuário e as credenciais do usuário (como senhas ou certificados) com segurança.  
  
-   Autenticação. Controladores de domínio fornecem serviços de autenticação para os usuários e fornecem dados de autorização adicionais, como associações de grupo do usuário, que podem ser usados para controlar o acesso aos recursos na rede.  
  
-   Relações de confiança. Domínios podem estender serviços de autenticação para os usuários em domínios fora de seu próprio domínio por meio de relações de confiança.  
  
-   Replicação. O domínio define uma partição de diretório que contém dados suficientes para fornecer serviços de domínio e, em seguida, replica, entre os controladores de domínio. Dessa forma, todos os controladores de domínio são pares em um domínio em são gerenciados como uma unidade.  
  
## <a name="active-directory-organizational-units"></a>Unidades organizacionais do Active Directory  
UOs podem ser usados para formar uma hierarquia de contêineres dentro de um domínio. UOs são usados para objetos de grupo para fins administrativos, como a aplicação de política de grupo ou delegação de autoridade. Controle (ao longo de uma unidade Organizacional e os objetos dentro dela) é determinado pelas listas de controle acesso (ACLs) na unidade Organizacional e em objetos na UO. Para facilitar o gerenciamento de um grande número de objetos, o AD DS dá suporte ao conceito de delegação de autoridade. Por meio de delegação, os proprietários podem transferir total ou limitado controle administrativo sobre objetos para outros usuários ou grupos. A delegação é importante porque ele ajuda a distribuir o gerenciamento de um grande número de objetos em um número de pessoas que são confiáveis para executar tarefas de gerenciamento.  
  


