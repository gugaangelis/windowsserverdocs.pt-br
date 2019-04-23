---
title: Determinar a lista de permissão-restrição e inventário de aplicativos para políticas de restrição de Software
description: Segurança do Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847287"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinar a lista de permissão-restrição e inventário de aplicativos para políticas de restrição de Software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para profissionais de TI fornece diretrizes sobre como criar um permitir e negar lista para aplicativos que serão gerenciados por políticas de restrição de Software (SRP) a partir do Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Elas são integradas ao Microsoft Active Directory Domain Services e a diretiva de grupo, mas também podem ser configuradas em computadores autônomos. Para um ponto de partida para política SRP, consulte o [diretivas de restrição de Software](software-restriction-policies.md).

Começando com o Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com as SRP por uma parte de sua estratégia de controle de aplicativo.

Para obter informações sobre como realizar tarefas específicas usando o SRP, consulte o seguinte:

-   [Trabalhar com regras de políticas de restrição de Software](work-with-software-restriction-policies-rules.md)

-   [Usar políticas de restrição de Software para ajudar a proteger seu computador contra um vírus de Email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Qual regra padrão para escolher: Permitir ou negar
Políticas de restrição de software podem ser implantadas em um dos dois modos são a base da sua regra padrão: Lista de permissões ou lista de Negações. Você pode criar uma política que identifica cada aplicativo que pode ser executado em seu ambiente; a regra padrão na diretiva é restrito bloquear todos os aplicativos que você explicitamente não permitir a execução. Ou você pode criar uma política que identifica todos os aplicativos que não serão executadas. a regra padrão é irrestrito e restringe apenas os aplicativos listados explicitamente por você.

> [!IMPORTANT]
> O modo de lista de negação pode ser uma estratégia de manutenção de alto para a sua organização sobre o controle de aplicativo. Criar e manter uma lista em evolução que proíbe a todos os malwares e outros aplicativos problemáticos seria demorado e suscetível a erros.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Criar um inventário de seus aplicativos para a lista de permissões
Para usar efetivamente a regra padrão permitir, você precisa determinar exatamente quais são os aplicativos em sua organização. Existem ferramentas criadas para produzir um inventário de aplicativos, como o coletor de inventário no Microsoft Application Compatibility Toolkit. Mas o SRP tem um recurso de registro em log avançado para ajudá-lo a entender exatamente quais aplicativos estão sendo executados em seu ambiente.

##### <a name="to-discover-which-applications-to-allow"></a>Para descobrir quais aplicativos permitir

1.  Em um ambiente de teste, implante a diretiva de restrição de Software com a regra padrão definida como Irrestrito e remova todas as regras adicionais. Se você habilitar o SRP sem forçá-lo para restringir os aplicativos, SPR será capaz de monitorar quais aplicativos estão sendo executados.

2.  Crie o seguinte valor de registro para habilitar o recurso de registro em log avançado e defina o caminho para onde o arquivo de log deve ser gravado.

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    Valor de cadeia de caracteres: *Caminho de NameLogFile para NameLogFile*

    Porque o SRP é avaliar todos os aplicativos quando eles são executados, uma entrada é gravada para o arquivo de log *NameLogFile* cada vez que o aplicativo é executado.

3.  Avaliar o arquivo de log

    Estados de cada entrada de log:

    -   o chamador da diretiva de restrição de software e o processo de identificação do processo de chamada

    -   o destino avaliado

    -   a regra de política SRP foi encontrada quando o aplicativo foi executado

    -   um identificador para a regra de política SRP.

    Um exemplo da saída gravada em um arquivo de log:

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe como regra usingpath irrestrito, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** todos os aplicativos e o código associado que SRP verifica e definido para bloquear serão observados no log arquivo, que você pode usar para determinar quais executáveis devem ser considerados para sua lista de permitidos.


