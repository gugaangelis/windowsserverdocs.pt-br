---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: "Visão geral dos serviços de domínio do Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b502017315d2b8b6b3d6ddfdad40c6d0f913e6c3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="active-directory-domain-services-overview"></a>Visão geral dos serviços de domínio do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Um diretório é uma estrutura hierárquica que armazena informações sobre objetos na rede. Um serviço de diretório, como os serviços de domínio do Active Directory (AD DS), fornece os métodos para armazenar dados de diretório e tornar esses dados disponíveis para administradores e usuários de rede. Por exemplo, o AD DS armazena informações sobre contas de usuário, como nomes, senhas, números de telefone e assim por diante e permite que outros usuários autorizados na mesma rede acessar essas informações.

Active Directory armazena informações sobre objetos na rede e facilita a essas informações para administradores e usuários encontrar e usar. Active Directory usa um repositório de dados estruturados como base para uma organização lógica e hierárquica de informações de diretório.

Esse repositório de dados, também conhecido como o diretório contém informações sobre objetos do Active Directory. Esses objetos normalmente incluem recursos compartilhados como servidores, volumes, impressoras e contas de usuário e computador da rede. Para saber mais sobre o repositório de dados do Active Directory, consulte [repositório de dados do diretório](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).

Segurança é integrada com o Active Directory por meio de autenticação de logon e controle de acesso a objetos no diretório. Com um único logon de rede, os administradores podem gerenciar dados de diretório e organização em suas redes e os usuários da rede autorizados podem acessar recursos em qualquer lugar na rede. Com base em política de administração facilita o gerenciamento da rede ainda mais complexo. Para obter mais informações sobre segurança do Active Directory, consulte Visão geral de segurança.

Active Directory também inclui:
* Um conjunto de regras, **o esquema**, que define as classes de objetos e atributos contidos no diretório, as restrições e limites em instâncias desses objetos e o formato dos seus nomes. Para obter mais informações sobre o esquema, consulte esquema.


* A **catálogo global** que contém informações sobre cada objeto no diretório. Isso permite que os usuários e administradores para encontrar informações de diretório independentemente de qual domínio no diretório realmente contém os dados. Para obter mais informações sobre o catálogo global, consulte a função do catálogo global.


* A **mecanismo de consulta e índice**, para que os objetos e suas propriedades podem ser publicadas e encontradas por usuários da rede ou aplicativos. Para obter mais informações sobre como consultar o diretório, consulte localizar informações de diretório.


* A **serviço de replicação** que distribui dados do diretório em uma rede. Todos os controladores de domínio em um domínio participam da replicação e contêm uma cópia completa de todas as informações de diretório para seu domínio. Qualquer alteração nos dados de diretório é replicada para todos os controladores de domínio no domínio. Para obter mais informações sobre a replicação do Active Directory, consulte Visão geral de replicação.

## <a name="understanding-active-directory"></a>Noções básicas sobre o Active Directory
 Esta seção fornece links para conceitos básicos do Active Directory:
 
* [Estrutura do Active Directory e tecnologias de armazenamento](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [Funções de controlador de domínios](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* Esquema do Active Directory 
* [Noções básicas sobre relações de confiança](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx) 
* [Tecnologias de replicação do Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Pesquisa do Active Directory e tecnologias de publicação](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* Interoperando com DNS e política de grupo 
* [Noções básicas sobre esquema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Para obter uma lista detalhada dos conceitos do Active Directory, consulte [Noções básicas sobre Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx). 


