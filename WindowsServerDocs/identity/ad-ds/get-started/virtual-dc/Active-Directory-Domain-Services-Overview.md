---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Visão geral dos serviços de domínio Active Directory
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 8087a6c7698952793546d6266a6a112bb810ac70
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88940326"
---
# <a name="active-directory-domain-services-overview"></a>Visão geral dos serviços de domínio Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Um diretório é uma estrutura hierárquica que armazena informações sobre objetos na rede. Um serviço de diretório, como o Active Directory Domain Services (AD DS), fornece os métodos para armazenar dados de diretório e disponibilizar esses dados para usuários e administradores de rede. Por exemplo, AD DS armazena informações sobre contas de usuário, como nomes, senhas, números de telefone e assim por diante, e permite que outros usuários autorizados na mesma rede acessem essas informações.

O Active Directory armazena informações sobre objetos na rede e torna essas informações fáceis de serem encontradas e usadas por administradores e usuários. O Active Directory usa um armazenamento de dados estruturado como base para uma organização lógica e hierárquica de informações de diretório.

Esse armazenamento de dados, também conhecido como o diretório, contém informações sobre objetos Active Directory. Esses objetos normalmente incluem recursos compartilhados, como servidores, volumes, impressoras e contas de usuário e computador de rede. Para obter mais informações sobre o armazenamento de dados de Active Directory, consulte [Directory Data Store](/previous-versions/windows/it-pro/windows-server-2003/cc736627(v=ws.10)).

A segurança é integrada com Active Directory por meio de autenticação de logon e controle de acesso a objetos no diretório. Com um único logon de rede, os administradores podem gerenciar dados de diretório e organização em toda a rede, e os usuários de rede autorizados podem acessar recursos em qualquer lugar da rede. A administração baseada em política facilita igualmente o gerenciamento de redes mais complexas. Para obter mais informações sobre Active Directory segurança, consulte [visão geral de segurança](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

O Active Directory também inclui:
* Um conjunto de regras, **o esquema**, que define as classes de objetos e atributos contidos no diretório, as restrições e limites de instâncias desses objetos e o formato de seus nomes. Para obter mais informações sobre o esquema, consulte esquema.


* Um **catálogo global** que contém informações sobre cada objeto no diretório. Isso permite que os usuários e administradores encontrem informações de diretório, independentemente de qual domínio no diretório realmente contenha os dados. Para obter mais informações sobre o catálogo global, consulte a função do catálogo global.


* Um **mecanismo de consulta e índice**, para que os objetos e suas propriedades possam ser publicados e encontrados por usuários ou aplicativos de rede. Para obter mais informações sobre como consultar o diretório, consulte Localizando informações de diretório.


* Um **serviço de replicação** que distribui dados de diretório em uma rede. Todos os controladores de domínio em um domínio participam da replicação e contêm uma cópia completa de todas as informações de diretório para seu domínio. Qualquer alteração nos dados do diretório é replicada em todos os controladores de domínio. Para obter mais informações sobre replicação de Active Directory, consulte Visão geral da replicação.

## <a name="understanding-active-directory"></a>Noções básicas sobre Active Directory
 Esta seção fornece links para os principais conceitos de Active Directory:

* [Active Directory a estrutura e as tecnologias de armazenamento](/previous-versions/windows/it-pro/windows-server-2003/cc759186(v=ws.10))
* [Funções do controlador de domínio](/previous-versions/windows/it-pro/windows-server-2003/cc786438(v=ws.10))
* [Esquema de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771796(v=ws.10))
* [Noções básicas sobre relações de confiança](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771568(v=ws.10))
* [Tecnologias de replicação Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc776877(v=ws.10))
* [Active Directory tecnologias de pesquisa e publicação](/previous-versions/windows/it-pro/windows-server-2003/cc775686(v=ws.10))
* [Interoperação com DNS e Política de Grupo](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd197486(v=ws.10))
* [Compreendendo o esquema](/previous-versions/windows/it-pro/windows-server-2003/cc759402(v=ws.10))

Para obter uma lista detalhada dos conceitos de Active Directory, consulte [noções básicas sobre Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc781408(v=ws.10)).
