---
title: Usar políticas de restrição de software para ajudar a proteger seu computador contra um vírus de email
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4c691255683eb37eecdbeaa55c094b7ce5c4e26d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357655"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Usar políticas de restrição de software para ajudar a proteger seu computador contra um vírus de email

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece informações sobre como definir políticas de controle de aplicativo usando diretivas de restrição de software (SRP) para ajudar a proteger seu computador contra vírus de email a partir do Windows Server 2008 e do Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Eles são integrados ao Microsoft Active Directory Domain Services e Política de Grupo, mas também podem ser configurados em computadores autônomos. Para obter um ponto de partida para o SRP, consulte as [diretivas de restrição de software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e do Windows 7, o Windows AppLocker pode ser usado em vez de ou em conjunto com o SRP para uma parte da sua estratégia de controle de aplicativo. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurar o SRP para ajudar a proteger contra um vírus de email

1.  Examine as práticas recomendadas para diretivas de restrição de software para entender como o SRP funciona.

    -   [Práticas recomendadas](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Como funcionam as diretivas de restrição de software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Abra Políticas de Restrição de Software.

    -   [Para seu computador local](administer-software-restriction-policies.md#BKMK_1)

    -   [Para um domínio, site ou unidade organizacional, e você está em um servidor membro ou em uma estação de trabalho que tenha ingressado em um domínio](administer-software-restriction-policies.md#BKMK_2)

3.  Se você não tiver definido políticas de restrição de software anteriormente, crie novas diretivas de restrição de software.

    -   [Para criar novas políticas de restrição de software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Crie uma regra de caminho para a pasta que seu programa de email usa para executar anexos de email e, em seguida, defina o nível de segurança como não **permitido**.

    -   [Trabalhando com regras de caminho](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Especifique os tipos de arquivo aos quais a regra se aplica.

    -   [Para adicionar ou excluir um tipo de arquivo designado](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modifique as configurações de política para que elas se apliquem aos usuários e aos grupos desejados:

    -   Especifique os usuários ou grupos aos quais você não deseja que as configurações de política do objeto de Política de Grupo (GPO) sejam aplicadas.

    -   Exclua os administradores locais das diretivas de restrição de software de uma configuração de política específica no Política de Grupo e ainda tenha o restante dos Política de Grupo se aplicar aos administradores.

        -   [Para impedir que as diretivas de restrição de software sejam aplicadas a administradores locais](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Teste a política.


