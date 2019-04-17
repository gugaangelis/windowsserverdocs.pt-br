---
title: "Determinar lista permitir negação e inventário de aplicativos para políticas de restrição de Software"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 60e78912284715649938567d66ffb90b9890b1b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinar lista permitir negação e inventário de aplicativos para políticas de restrição de Software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para o profissional de TI fornece orientações sobre como criar um permitir e negar a lista de aplicativos podem ser gerenciados pelo início de políticas de restrição de Software (SRP) com o Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
Políticas de restrição de software (SRP) é um recurso baseado em política de grupo que identifica os programas de software executados em computadores em um domínio e controla a capacidade desses programas para execução. Você pode usar políticas de restrição de software para criar uma configuração altamente restrita para computadores, em que você permitir que apenas especificamente identificados aplicativos sejam executados. Esses são integrados ao Microsoft Active Directory Domain Services e política de grupo, mas também podem ser configuradas em computadores autônomos. Para um ponto de partida para SRP, consulte o [políticas de restrição de Software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com SRP para uma parte da sua estratégia de controle do aplicativo.

Para obter informações sobre como realizar tarefas específicas usando SRP, consulte o seguinte:

-   [Trabalhar com regras de políticas de restrição de Software](work-with-software-restriction-policies-rules.md)

-   [Usar políticas de restrição de Software para ajudar a proteger seu computador contra vírus de Email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Qual regra padrão para escolher: permitir ou negar
Políticas de restrição de software podem ser implantadas em um dos dois modos são a base da regra padrão: lista de permitir ou negar. Você pode criar uma política que identifica cada aplicativo tem permissão para executar em seu ambiente. a regra padrão na sua política é restrito bloquear todos os aplicativos que você explicitamente não permitir para ser executado. Ou você pode criar uma política que identifica todos os aplicativos que não serão executadas. a regra padrão é irrestrito e restringe apenas os aplicativos que você listou explicitamente.

> [!IMPORTANT]
> O modo de lista de negação pode ser uma estratégia de alta manutenção para sua organização em relação ao controle do aplicativo. Criar e manter uma lista em evolução que proíbe todo o malware e outros aplicativos problemáticos seria demorado e suscetível a erros.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Criar um inventário dos seus aplicativos para a lista de permissões
Para usar efetivamente a regra padrão de permitir, você precisa determinar exatamente quais aplicativos são necessários em sua organização. Existem ferramentas projetadas para produzir um inventário dos aplicativos, como o coletor de inventário em Microsoft Application Compatibility Toolkit. Mas SRP tem um recurso de registro em log avançado para ajudar a compreender exatamente quais aplicativos estão sendo executados em seu ambiente.

##### <a name="to-discover-which-applications-to-allow"></a>Para descobrir quais aplicativos para permitir que

1.  Em um ambiente de teste, implantar política de restrição de Software com o conjunto de regras padrão como Irrestrito e remova quaisquer regras adicionais. Se você habilitar SRP sem forçar para restringir todos os aplicativos, SPR será capaz de monitorar quais aplicativos estão sendo executados.

2.  Crie o seguinte valor do registro para habilitar o recurso de registro em log avançado e definir o caminho para onde o arquivo de log deve ser gravado.

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    Valor de cadeia de caracteres: *NameLogFile caminho NameLogFile*

    Porque SRP é avaliar todos os aplicativos quando eles são executados, uma entrada é escrita para o arquivo de log *NameLogFile* toda vez que o aplicativo é executado.

3.  Avaliar o arquivo de log

    Cada entrada de log estados:

    -   O chamador da política de restrição de software e o ID (PID) do processo de chamada de processo

    -   O destino avaliado

    -   A regra SRP que ocorreu quando o aplicativo foi executado

    -   Um identificador para a regra SRP.

    Um exemplo da saída gravado em um arquivo de log:

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe como irrestrita usingpath regra, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** todos os aplicativos e o código associado que SRP verifica e configurado para bloqueio serão registradas no arquivo de log, você pode usar para determinar quais arquivos executáveis devem ser considerados para a sua lista de permitidos.


