---
title: Software Restriction Policies
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 44f917beaa7b1e13171d2c8ade6f0172b450350d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953013"
---
# <a name="software-restriction-policies"></a>Software Restriction Policies

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para o profissional de ti descreve as políticas de restrição de software (SRP) no Windows Server 2012 e no Windows 8 e fornece links para informações técnicas sobre o SRP a partir do Windows Server 2003.

Para obter procedimentos e dicas de solução de problemas, consulte [administrar políticas de restrição](administer-software-restriction-policies.md) de software e [solucionar problemas de diretivas de restrição de software](troubleshoot-software-restriction-policies.md).

## <a name="software-restriction-policies-description"></a><a name="BKMK_OVER"></a>Descrição de políticas de restrição de software
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. As políticas de restrição de software são parte da estratégia de segurança e gerenciamento da Microsoft para ajudar empresas a aumentar a confiabilidade, integridade e gerenciabilidade de seus computadores.

Você também pode usar as políticas de restrição de software para criar uma configuração altamente restrita para computadores em que você permite apenas aplicativos especialmente identificados a serem executados. As políticas de restrição de software são integradas com o Microsoft Active Directory e a Política de Grupo. Você também pode criar políticas de restrição de software em computadores autônomos. As políticas de restrição de software são políticas de confiança, que são normas definidas por um administrador para restringir scripts e outro código que não seja totalmente confiável de ser executado.

Você pode definir essas políticas por meio da extensão das Políticas de Restrição de Software do Editor de Política de Grupo ou do snap-in Políticas de Segurança Local para o MMC (Console de Gerenciamento Microsoft).

Para obter informações mais detalhadas sobre a política SRP, consulte a [Visão geral técnica das políticas de restrição de software](software-restriction-policies-technical-overview.md).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
Os Administradores podem usar as políticas de restrição de software para as seguintes tarefas:

-   Definir o que é código confiável

-   Criar uma Política de Grupo flexível para scripts de regulação, arquivos executáveis e controles ActiveX

As políticas de restrição de software são aplicadas pelo sistema operacional e por aplicativos (como aplicativos de script) que atendem às políticas de restrição de software.

Especificamente, os administradores podem usar as políticas de restrição de software para as seguintes finalidades:

-   Especificar quais software (arquivos executáveis) podem ser executados em clientes

-   Evitar que usuários executem programas específicos em computadores compartilhados

-   Especificar quem pode adicionar editores confiáveis aos computadores

-   Definir o escopo das políticas de restrição de software (especificar se as políticas afetam todos os usuários ou um subconjunto de usuários em clientes)

-   Evitar que arquivos executáveis sejam executados no computador local, na unidade organizacional (OU), no site, ou no domínio. Isso seria apropriado em casos em que você não estiver usando políticas de restrição de software para resolver possíveis problemas com usuários mal-intencionados.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidade nova e alterada
Não há alterações na funcionalidade para Políticas de Restrição de Software.

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>Funcionalidades removidas ou reprovadas
Não há funcionalidade removida ou preterida para Políticas de Restrição de Software.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
A extensão das Políticas de Restrição de Software para o Editor de Política de Grupo pode ser acessada por meio do MMC.

Os seguintes recursos são necessários para criar e manter as políticas de restrição de software no computador local:

-   Editor de Política de Grupo Local

-   Windows Installer

-   Authenticode e WinVerifyTrust

Se seu projeto solicitar a implantação do domínio dessas políticas, além da lista anterior, os seguintes recursos são necessários:

-   Active Directory Domain Services

-   Política de Grupo

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Informações sobre o Gerenciador do Servidor
As Políticas de Restrição de Software são uma extensão do Editor de Política de Grupo e não são instaladas por meio do Gerenciador do Servidor, Adicionar Funções e Recursos.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir fornece links para os recursos relevantes para compreensão e uso de SRP.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Bloqueio de Aplicativo com Políticas de Restrição de Software](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
|**Planejamento**|[Visão geral técnica das políticas de restrição de software](software-restriction-policies-technical-overview.md) (Windows Server 2012)<p>[Referência técnica de políticas de restrição de software](/previous-versions/windows/it-pro/windows-server-2003/cc728085(v=ws.10)) (Windows Server 2003)|
|**Implantação**|Nenhum recurso disponível.|
|**Operações**|[Administrar políticas de restrição de software](administer-software-restriction-policies.md) (Windows Server 2012)<p>[Ajuda do produto das diretivas de restrição de software](/previous-versions/windows/it-pro/windows-server-2003/cc779607(v=ws.10)) (Windows Server 2003)|
|**Solução de problemas**|[Solucionar problemas de diretivas de restrição de software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<p>[Solução de problemas de diretivas de restrição de software](/previous-versions/windows/it-pro/windows-server-2003/cc737011(v=ws.10)) (Windows Server 2003)|
|**Segurança**|[Ameaças e contramedidas para políticas de restrição de software](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349795(v=ws.10)) (Windows Server 2008)<p>[Ameaças e contramedidas para políticas de restrição de software](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125926(v=ws.10)) (Windows Server 2008 R2)|
|**Ferramentas e configurações**|[Ferramentas e configurações de diretivas de restrição de software](/previous-versions/windows/it-pro/windows-server-2003/cc782454(v=ws.10)) (Windows Server 2003)|
|**Recursos da comunidade**|[Bloqueio de Aplicativo com Políticas de Restrição de Software](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
