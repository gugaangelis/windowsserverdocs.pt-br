---
title: Políticas de restrição de software
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ab44013b947d33adc12c54b527415bf16c46a4c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875817"
---
# <a name="software-restriction-policies"></a>Políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para profissionais de TI descreve as políticas de restrição de Software (SRP) no Windows Server 2012 e Windows 8 e fornece links para informações técnicas sobre políticas SRP a partir do Windows Server 2003.

Para procedimentos e dicas de solução de problemas, consulte [administrar políticas de restrição de Software](administer-software-restriction-policies.md) e [solucionar problemas de políticas de restrição de Software](troubleshoot-software-restriction-policies.md).

## <a name="BKMK_OVER"></a>Descrição de diretivas de restrição de software
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. As políticas de restrição de software são parte da estratégia de segurança e gerenciamento da Microsoft para ajudar empresas a aumentar a confiabilidade, integridade e gerenciabilidade de seus computadores.

Você também pode usar as políticas de restrição de software para criar uma configuração altamente restrita para computadores em que você permite apenas aplicativos especialmente identificados a serem executados. As políticas de restrição de software são integradas com o Microsoft Active Directory e a Política de Grupo. Você também pode criar políticas de restrição de software em computadores autônomos. As políticas de restrição de software são políticas de confiança, que são normas definidas por um administrador para restringir scripts e outro código que não seja totalmente confiável de ser executado.

Você pode definir essas políticas por meio da extensão das Políticas de Restrição de Software do Editor de Política de Grupo ou do snap-in Políticas de Segurança Local para o MMC (Console de Gerenciamento Microsoft).

Para obter informações detalhadas sobre o SRP, consulte o [Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md).

## <a name="BKMK_APP"></a>Aplicativos práticos
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

## <a name="BKMK_NEW"></a>Funcionalidades novas e alteradas
Não há alterações na funcionalidade para Políticas de Restrição de Software.

## <a name="BKMK_DEP"></a>Funcionalidade removida ou preterida
Não há funcionalidade removida ou preterida para Políticas de Restrição de Software.

## <a name="BKMK_SOFT"></a>Requisitos de software
A extensão das Políticas de Restrição de Software para o Editor de Política de Grupo pode ser acessada por meio do MMC.

Os seguintes recursos são necessários para criar e manter as políticas de restrição de software no computador local:

-   Editor de Política de Grupo Local

-   Windows Installer

-   Authenticode e WinVerifyTrust

Se seu projeto solicitar a implantação do domínio dessas políticas, além da lista anterior, os seguintes recursos são necessários:

-   Active Directory Domain Services

-   Política de Grupo

## <a name="BKMK_INSTALL"></a>Informações do Gerenciador do servidor
As Políticas de Restrição de Software são uma extensão do Editor de Política de Grupo e não são instaladas por meio do Gerenciador do Servidor, Adicionar Funções e Recursos.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir fornece links para os recursos relevantes para compreensão e uso de SRP.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Bloqueio de aplicativo com políticas de restrição de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Planejamento**|[Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md) (Windows Server 2012)<br /><br />[Referência técnica de políticas de restrição de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Implantação**|Nenhum recurso disponível.|
|**Operações**|[Administrar políticas de restrição de Software](administer-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Ajuda do produto de políticas de restrição de software](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Solução de problemas**|[Solucionar problemas de políticas de restrição de Software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Solução de problemas de políticas de restrição de software](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Segurança**|[Ameaças e contramedidas para políticas de restrição de software](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<br /><br />[Ameaças e contramedidas para políticas de restrição de software](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Ferramentas e configurações**|[Ferramentas e configurações de políticas de restrição de software](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Recursos da comunidade**|[Bloqueio de aplicativo com políticas de restrição de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



