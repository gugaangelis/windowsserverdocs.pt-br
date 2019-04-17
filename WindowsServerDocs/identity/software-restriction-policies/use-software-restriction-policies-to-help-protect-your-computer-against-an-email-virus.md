---
title: "Usar políticas de restrição de Software para ajudar a proteger seu computador contra vírus de Email"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Usar políticas de restrição de Software para ajudar a proteger seu computador contra vírus de Email

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece informações sobre como configurar o controle de aplicativo políticas usando políticas de restrição de Software (SRP) para ajudar a proteger seu computador contra o início de vírus de email com o Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
Políticas de restrição de software (SRP) é um recurso baseado em política de grupo que identifica os programas de software executados em computadores em um domínio e controla a capacidade desses programas para execução. Você pode usar políticas de restrição de software para criar uma configuração altamente restrita para computadores, em que você permitir que apenas especificamente identificados aplicativos sejam executados. Esses são integrados ao Microsoft Active Directory Domain Services e política de grupo, mas também podem ser configuradas em computadores autônomos. Para um ponto de partida para SRP, consulte o [políticas de restrição de Software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com SRP para uma parte da sua estratégia de controle do aplicativo. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurar o SRP para ajudar a proteger contra vírus de email

1.  Examine as práticas recomendadas para políticas de restrição de software entender como funciona o SRP.

    -   [Práticas recomendadas](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Como funcionam as políticas de restrição de Software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Abra as políticas de restrição de Software.

    -   [Para o computador local](administer-software-restriction-policies.md#BKMK_1)

    -   [Para um domínio, site ou unidade organizacional e você está em um servidor membro ou estação de trabalho que tenha ingressada em um domínio](administer-software-restriction-policies.md#BKMK_2)

3.  Se você não tiver definido anteriormente políticas de restrição de software, crie novas políticas de restrição de software.

    -   [Para criar novas políticas de restrição de software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Criar uma regra de caminho para a pasta que o programa de email usa para executar anexos de email e, em seguida, defina a nível de segurança para **não permitido**.

    -   [Trabalhando com regras de caminho](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Especifica os tipos de arquivo ao qual a regra se aplica.

    -   [Para adicionar ou excluir um tipo de arquivo designados](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modificar configurações de política para que eles se aplicam aos usuários e grupos que você deseja:

    -   Especifique os usuários ou grupos aos quais você não fará o grupo do objeto de diretiva (GPO) as configurações de política para aplicar.

    -   Excluir as políticas de restrição de software de uma configuração de política específica na política de grupo local Administradores e ainda ter o resto da política de grupo se aplicam aos administradores.

        -   [Para impedir que as políticas de restrição de software aplicadas a administradores locais](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Teste a política.


