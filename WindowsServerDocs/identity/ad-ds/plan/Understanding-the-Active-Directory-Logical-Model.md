---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Noções básicas do modelo lógico do Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e909d4e9c1fb26aa0f7cb97a7dc06192db5cc21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834267"
---
# <a name="understanding-the-active-directory-logical-model"></a>Noções básicas do modelo lógico do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criando a estrutura lógica para os serviços de domínio do Active Directory (AD DS) envolve a definição de relações entre os contêineres em seu diretório. Essas relações podem ser com base em requisitos administrativos, como a delegação de autoridade, ou eles podem ser definidos por requisitos operacionais, como a necessidade de controlar a replicação.  
  
Antes de criar a estrutura lógica do Active Directory, é importante entender o modelo lógico do Active Directory. O AD DS é um banco de dados distribuído que armazena e gerencia informações sobre recursos de rede, bem como dados específicos do aplicativo de aplicativos habilitados por diretório. AD DS permite aos administradores organizar elementos de uma rede (como usuários, computadores e dispositivos) em uma estrutura de confinamento hierárquica. O contêiner de nível superior é a floresta. Em florestas são domínios e dentro dos domínios são unidades organizacionais (OUs). Isso é chamado de modelo lógico porque ele é independente dos aspectos da implantação, como o número de controladores de domínio necessários dentro de cada domínio e topologia de rede físicos.  
  
## <a name="active-directory-forest"></a>Floresta do Active Directory  
Uma floresta é uma coleção de um ou mais domínios do Active Directory que compartilham uma estrutura lógica comum, o esquema de diretório (definições de classe e atributo), a configuração de diretório (informações do site e replicação) e catálogo global (pesquisa de floresta recursos). Domínios na mesma floresta são automaticamente vinculados por relações de confiança transitivas e bidirecionais.  
  
## <a name="active-directory-domain"></a>Domínio do Active Directory  
Um domínio é uma partição em uma floresta do Active Directory. O particionamento de dados permite que as organizações repliquem dados somente para onde for necessário. Dessa forma, o diretório pode ser dimensionado globalmente em uma rede que limitou a largura de banda disponível. Além disso, o domínio dá suporte a um número de núcleo de outro funções relacionadas à administração, incluindo:  
  
-   Identidade de usuário em toda a rede. Os domínios permitem que identidades de usuário a ser criada uma vez e referenciada em qualquer computador que ingressou na floresta em que o domínio está localizado. Controladores de domínio que constituem um domínio são usados para armazenar contas de usuário e as credenciais do usuário (como senhas ou certificados) com segurança.  
  
-   Autenticação. Controladores de domínio fornecem serviços de autenticação para usuários e fornecem dados de autorização adicional, como associações de grupo de usuário, que podem ser usados para controlar o acesso aos recursos da rede.  
  
-   Relações de confiança. Domínios podem estender os serviços de autenticação para usuários em domínios fora de sua própria floresta por meio de relações de confiança.  
  
-   Replicação. O domínio define uma partição de diretório que contém dados suficientes para fornecer serviços de domínio e, depois, replica-lo entre os controladores de domínio. Dessa forma, todos os controladores de domínio são pares em um domínio em são gerenciados como uma unidade.  
  
## <a name="active-directory-organizational-units"></a>Unidades organizacionais do Active Directory  
Unidades organizacionais podem ser usados para formar uma hierarquia de contêineres dentro de um domínio. As UOs são usados para objetos de grupo para fins administrativos, como a aplicação de diretiva de grupo ou delegação de autoridade. Controle (mais de uma unidade Organizacional e os objetos dentro dele) é determinado pelas controle listas de acesso (ACLs) na unidade Organizacional e nos objetos na UO. Para facilitar o gerenciamento de um grande número de objetos, o AD DS oferece suporte ao conceito de delegação de autoridade. Por meio de delegação, os proprietários podem transferir controle administrativo completo ou limitado de objetos para outros usuários ou grupos. A delegação é importante pois ajuda a distribuir o gerenciamento de um grande número de objetos em um número de pessoas que são confiáveis para executar tarefas de gerenciamento.  
  


