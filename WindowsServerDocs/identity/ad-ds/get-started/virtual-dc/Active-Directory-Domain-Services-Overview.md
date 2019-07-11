---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Visão geral do Active Directory Domain Services
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ed8a22881cd20633e6fcd61b146f3b0aad7a757b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792277"
---
# <a name="active-directory-domain-services-overview"></a>Visão geral do Active Directory Domain Services

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Um diretório é uma estrutura hierárquica que armazena informações sobre objetos na rede. Um serviço de diretório, como os serviços de domínio do Active Directory (AD DS), fornece os métodos para armazenar dados de diretório e disponibilizá-los para os administradores e usuários da rede. Por exemplo, o AD DS armazena informações sobre contas de usuário, como nomes, senhas, números de telefone e assim por diante e permite que outros usuários autorizados na mesma rede acessem essas informações.

Active Directory armazena informações sobre objetos na rede e facilita para os administradores e usuários a localizar e usar essas informações. Active Directory usa um armazenamento de dados estruturados como base para uma organização lógica e hierárquica de informações de diretório.

Esse armazenamento de dados, também conhecido como o diretório contém informações sobre objetos do Active Directory. Normalmente, esses objetos incluem recursos compartilhados, como servidores, volumes, impressoras e contas de usuário e computador da rede. Para obter mais informações sobre o armazenamento de dados do Active Directory, consulte [armazenamento de dados do diretório](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).

A segurança é integrada ao Active Directory por meio de autenticação de logon e controle de acesso a objetos no diretório. Com um único logon de rede, os administradores podem gerenciar dados de diretório e organização por toda a sua rede e usuários de rede autorizados podem acessar recursos em qualquer lugar na rede. A administração baseada em política facilita igualmente o gerenciamento de redes mais complexas. Para obter mais informações sobre segurança do Active Directory, consulte [visão geral de segurança](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

O Active Directory também inclui:
* Um conjunto de regras, **o esquema**, que define as classes de objetos e atributos contidos no diretório, as restrições e limites nas instâncias desses objetos e o formato de seus nomes. Para obter mais informações sobre o esquema, consulte o esquema.


* Um **catálogo global** que contém informações sobre todos os objetos no diretório. Isso permite que os usuários e administradores para localizar informações de diretório independentemente de qual domínio no diretório realmente contém os dados. Para obter mais informações sobre o catálogo global, consulte a função do catálogo global.


* Um **mecanismo de consulta e índice**, de modo que os objetos e suas propriedades podem ser publicadas e encontradas por usuários da rede ou aplicativos. Para obter mais informações sobre como consultar o diretório, consulte localizar informações de diretório.


* Um **serviço de replicação** que distribui dados do diretório em uma rede. Todos os controladores de domínio em um domínio participam da replicação e contêm uma cópia completa de todas as informações de diretório para seu domínio. Qualquer alteração nos dados do diretório é replicada em todos os controladores de domínio. Para obter mais informações sobre replicação do Active Directory, consulte Visão geral da replicação.

## <a name="understanding-active-directory"></a>Noções básicas sobre o Active Directory
 Esta seção fornece links para os principais conceitos do Active Directory:
 
* [Estrutura do Active Directory e tecnologias de armazenamento](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [Funções de controlador de domínios](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Esquema do Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771796(v=ws.10))
* [Noções básicas sobre relações de confiança](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771568(v=ws.10)) 
* [Tecnologias de replicação do Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Pesquisa do Active Directory e as tecnologias de publicação](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* [Interoperação com DNS e diretiva de grupo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197486(v=ws.10))
* [Noções básicas sobre esquema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Para obter uma lista detalhada de conceitos do Active Directory, consulte [Noções básicas sobre Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx). 


