---
title: Usar políticas de restrição de software para ajudar a proteger seu computador contra um vírus de email
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 41b4c2399a86ef96d34b62295eda4a1ce9300609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850667"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Usar políticas de restrição de software para ajudar a proteger seu computador contra um vírus de email

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece informações sobre como definir o controle de aplicativo políticas usando políticas de restrição de Software (SRP) para ajudar a proteger seu computador contra o início de vírus de email com o Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Elas são integradas ao Microsoft Active Directory Domain Services e a diretiva de grupo, mas também podem ser configuradas em computadores autônomos. Para um ponto de partida para política SRP, consulte o [diretivas de restrição de Software](software-restriction-policies.md).

Começando com o Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com as SRP por uma parte de sua estratégia de controle de aplicativo. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurar o SRP para ajudar a proteger contra vírus de email

1.  Examine as práticas recomendadas para políticas de restrição de software entender como funciona o SRP.

    -   [Práticas recomendadas](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Como funcionam as políticas de restrição de Software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Abra Políticas de Restrição de Software.

    -   [Para o computador local](administer-software-restriction-policies.md#BKMK_1)

    -   [Para um domínio, site, ou unidade organizacional e você está em um servidor membro ou estação de trabalho que tenha ingressada em um domínio](administer-software-restriction-policies.md#BKMK_2)

3.  Se você não definiu anteriormente as políticas de restrição de software, crie novas políticas de restrição de software.

    -   [Para criar novas políticas de restrição de software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Criar uma regra de caminho para a pasta que o programa de email usa para executar anexos de email e, em seguida, defina a nível de segurança para **não permitido**.

    -   [Trabalhando com regras de caminho](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Especifica os tipos de arquivo para o qual a regra se aplica.

    -   [Para adicionar ou excluir um tipo de arquivo designado](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modifica configurações de política para que elas se aplicam aos usuários e grupos que você precisa:

    -   Especificar usuários ou grupos aos quais você não deseja que o grupo de política do GPO (objeto) para aplicar as configurações de diretiva.

    -   Excluir local de administradores de diretivas de restrição de software de uma configuração de política específica na diretiva de grupo e ainda terá o restante da diretiva de grupo que se aplicam aos administradores.

        -   [Para impedir que as políticas de restrição de software aplicadas a administradores locais](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Teste a política.


