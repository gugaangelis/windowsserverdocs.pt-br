---
title: Visão geral técnica de autenticação do Windows
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 286d3e41-434f-4703-9320-706d06ebda51
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c2ac43da06d6df177523b389eac90f02e6e26b93
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989972"
---
# <a name="windows-authentication-technical-overview"></a>Visão geral técnica de autenticação do Windows

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de ti fornece links para tópicos sobre a visão geral técnica da autenticação do Windows. A autenticação do Windows é o processo para provar a autenticidade de um usuário ou serviço que tenta acessar o Windows.

Esta coleção de tópicos descreve a arquitetura de autenticação do Windows e seus componentes.

Para salvar ou imprimir páginas desta biblioteca digitalmente, clique em **Exportar** (no canto superior direito da página) e siga as instruções.

-   [Diferenças na autenticação do Windows entre os sistemas operacionais Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169017(v=ws.10))

    Descreve as diferenças significativas na arquitetura e nos processos de autenticação.

-   [Conceitos de autenticação do Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169018(v=ws.10))

    Descreve os conceitos nos quais a autenticação do Windows é baseada.

-   [Cenários de autenticação de logon do Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169020(v=ws.10))

    Resume os vários cenários de logon.

-   [Arquitetura de autenticação do Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169024(v=ws.10))

    Descreve as diferenças significativas na arquitetura e nos processos de autenticação dos sistemas operacionais Windows.

    -   [Arquitetura de Interface SSPI](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169026(v=ws.10))

        Descreve a arquitetura SSPI.

    -   [Processos de credenciais na autenticação do Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169014(v=ws.10))

        Descreve os diferentes processos de gerenciamento de credenciais.

-   [Políticas de grupo usadas na autenticação do Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169021(v=ws.10))

    Descreve o uso e o impacto das políticas de grupo no processo de autenticação.

## <a name="what-is-not-covered"></a>O que não é abordado
Esta coleção de tópicos não aborda os procedimentos para projetar, implementar ou monitorar suas tecnologias de autenticação em um ambiente Windows.

-   Para obter informações de design sobre estratégias de autorização do Windows, consulte [criando uma estratégia de autorização de recursos](/previous-versions/windows/it-pro/windows-server-2003/cc783368(v=ws.10)).

-   Para obter informações de design sobre estratégias de autenticação do Windows, consulte [criando uma estratégia de autenticação](/previous-versions/windows/it-pro/windows-server-2003/cc758124(v=ws.10)).

-   Para obter informações de design sobre estratégias de implementação de infraestrutura de chave pública do Windows, consulte [projetando uma infraestrutura de chave pública](/previous-versions/windows/it-pro/windows-server-2003/cc773138(v=ws.10)).

-   Para configurar e monitorar a segurança, incluindo a autenticação do em seu ambiente do Windows, consulte:

    -   [Guia de segurança do Windows XP](https://www.microsoft.com/download/details.aspx?id=962)

    -   [Linha de base de segurança do Windows Vista](/previous-versions/tn-archive/dd450978(v=technet.10))

    -   [Linha de base de segurança do Windows Server 2003](/previous-versions/tn-archive/cc163140(v=technet.10)) e o [Guia de ameaças e contramedidas](/previous-versions/tn-archive/dd162275(v=technet.10))

    -   [Guia de Segurança do Windows Server 2008](https://www.microsoft.com/download/details.aspx?id=17606)

    -   [Linha de base de segurança do Windows Server 2008 R2](/previous-versions/tn-archive/gg236605(v=technet.10))

-   Para obter informações sobre auditoria de eventos de logon e autenticação no Windows, consulte [auditoria de eventos de segurança](/previous-versions/windows/it-pro/windows-server-2003/cc776394(v=ws.10)).