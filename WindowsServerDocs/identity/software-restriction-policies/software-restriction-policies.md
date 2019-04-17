---
title: "Políticas de restrição de software"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies"></a>Políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para o profissional de TI descreve as políticas de restrição de Software (SRP) no Windows Server 2012 e Windows 8 e fornece links para informações técnicas sobre o Windows Server 2003 a partir do SRP.

Para obter os procedimentos e dicas de solução de problemas, consulte [administrar políticas de restrição de Software](administer-software-restriction-policies.md) e [Solucionar políticas de restrição de Software](troubleshoot-software-restriction-policies.md).

## <a name="BKMK_OVER"></a>Descrição de políticas de restrição de software
Políticas de restrição de software (SRP) é um recurso baseado em política de grupo que identifica os programas de software executados em computadores em um domínio e controla a capacidade desses programas para execução. Políticas de restrição de software fazem parte da estratégia de gerenciamento para ajudar as empresas aumentando a confiabilidade, a integridade e a capacidade de gerenciamento de seus computadores e da segurança da Microsoft.

Você também pode usar políticas de restrição de software para criar uma configuração altamente restrita para computadores, em que você permitir que apenas especificamente identificados aplicativos sejam executados. Políticas de restrição de software são integradas ao Microsoft Active Directory e política de grupo. Você também pode criar políticas de restrição de software em computadores autônomos. Políticas de restrição de software são políticas de confiança, que são regulamentos definidos por um administrador para restringir scripts e outro código que não é totalmente confiável seja executado.

Você pode definir essas políticas através da extensão de políticas de restrição de Software do Editor de política de Grupo Local ou o snap-in de políticas de segurança Local para Console de gerenciamento Microsoft (MMC).

Para obter informações detalhadas sobre o SRP, consulte o [visão geral técnica Software para políticas de restrição](software-restriction-policies-technical-overview.md).

## <a name="BKMK_APP"></a>Aplicativos práticos
Os administradores podem usar políticas de restrição de software para as seguintes tarefas:

-   Definir o que é o código confiável

-   Criar uma política de grupo flexível para controlar a scripts, arquivos executáveis e controles ActiveX

Políticas de restrição de software são impostas pelo sistema operacional e por aplicativos (como aplicativos de scripts) que estão em conformidade com políticas de restrição de software.

Especificamente, os administradores podem usar políticas de restrição de software para as seguintes finalidades:

-   Especificar qual software (arquivos executáveis) pode ser executado em clientes

-   Impedir que os usuários executem programas específicos em computadores compartilhados

-   Especificar quem pode adicionar editores confiáveis aos clientes

-   Definir o escopo de políticas de restrição de software (especificar se políticas afetam todos os usuários ou um subconjunto de usuários nos clientes)

-   Impedir que arquivos executáveis em execução no computador local, unidade organizacional (UO), site ou domínio. Isso seria adequado em casos quando você não estiver usando políticas de restrição de software para resolver problemas potenciais com usuários mal-intencionados.

## <a name="BKMK_NEW"></a>Funcionalidades novas e alteradas
Não há nenhuma alteração na funcionalidade de políticas de restrição de Software.

## <a name="BKMK_DEP"></a>Funcionalidade removida ou preterida
Não há nenhuma funcionalidade removida ou preterida para políticas de restrição de Software.

## <a name="BKMK_SOFT"></a>Requisitos de software
A extensão de políticas de restrição de Software para Editor de política de Grupo Local pode ser acessada por meio do MMC.

Os seguintes recursos são necessários para criar e manter políticas de restrição de software no computador local:

-   Editor de política de grupo local

-   Do Windows Installer

-   Authenticode e WinVerifyTrust

Se o seu design chama para implantação de domínio dessas políticas, além da lista acima, os seguintes recursos são necessários:

-   Serviços de domínio do Active Directory

-   Política de grupo

## <a name="BKMK_INSTALL"></a>Informações do Gerenciador do servidor
Políticas de restrição de software é uma extensão do Editor de política de Grupo Local e não é instalada por meio do Gerenciador de servidores, adicionar funções e recursos.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir fornece links para recursos relevantes em Noções básicas e uso SRP.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Bloqueio do aplicativo com políticas de restrição de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Planejamento**|[Visão geral do técnica de políticas de restrição do software](software-restriction-policies-technical-overview.md) (Windows Server 2012)<br /><br />[Referência técnica de políticas de restrição de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Implantação**|Não há recursos disponíveis.|
|**Operações**|[Administrar políticas de restrição de Software](administer-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Ajuda do produto de políticas de restrição de software](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Solução de problemas**|[Solucionar problemas de políticas de restrição de Software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Solução de problemas de políticas de restrição de software](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Segurança**|[Políticas de ameaças e contramedidas de restrição de Software](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<br /><br />[Políticas de ameaças e contramedidas de restrição de Software](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Ferramentas e configurações**|[Políticas de restrição de software ferramentas e configurações](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Recursos da comunidade**|[Bloqueio do aplicativo com políticas de restrição de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



