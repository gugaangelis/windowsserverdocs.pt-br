---
title: Determinar a lista de permissão-restrição e inventário de aplicativos para políticas de restrição de Software
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c0fdb5c1d7c4b03610a173c6cd0575d39646a7d0
ms.sourcegitcommit: af1cf89632d62a94943d3ad9f6b5234b88499278
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81524901"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinar a lista de permissão-restrição e inventário de aplicativos para políticas de restrição de Software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para o profissional de ti fornece orientação sobre como criar uma lista de permissões e negações para que os aplicativos sejam gerenciados por políticas de restrição de software (SRP) a partir do Windows Server 2008 e do Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Eles são integrados ao Microsoft Active Directory Domain Services e Política de Grupo, mas também podem ser configurados em computadores autônomos. Para obter um ponto de partida para o SRP, consulte as [diretivas de restrição de software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e do Windows 7, o Windows AppLocker pode ser usado em vez de ou em conjunto com o SRP para uma parte da sua estratégia de controle de aplicativo.

Para obter informações sobre como realizar tarefas específicas usando o SRP, consulte o seguinte:

-   [Trabalhar com regras de políticas de restrição de software](work-with-software-restriction-policies-rules.md)

-   [Use as políticas de restrição de software para ajudar a proteger seu computador contra um vírus de email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Qual regra padrão escolher: permitir ou negar
As diretivas de restrição de software podem ser implantadas em um dos dois modos que são a base da sua regra padrão: lista de permissões ou lista de negações. Você pode criar uma política que identifica cada aplicativo que tem permissão para ser executado em seu ambiente; a regra padrão em sua política é restrita e bloqueará todos os aplicativos que você não permitir explicitamente que sejam executados. Ou você pode criar uma política que identifica todos os aplicativos que não podem ser executados; a regra padrão é irrestrita e restringe apenas os aplicativos que você listou explicitamente.

> [!IMPORTANT]
> O modo de lista de negações pode ser uma estratégia de alta manutenção para sua organização em relação ao controle do aplicativo. Criar e manter uma lista em evolução que proíba todos os malwares e outros aplicativos problemáticos seria um tempo demorado e suscetível a erros.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Criar um inventário de seus aplicativos para a lista de permissões
Para usar efetivamente a regra permitir padrão, você precisa determinar exatamente quais aplicativos são necessários em sua organização. Há ferramentas projetadas para produzir um inventário de aplicativos, como o coletor de inventário no kit de ferramentas de compatibilidade de aplicativos da Microsoft. Mas o SRP tem um recurso de registro em log avançado para ajudá-lo a entender exatamente quais aplicativos estão sendo executados em seu ambiente.

##### <a name="to-discover-which-applications-to-allow"></a>Para descobrir quais aplicativos permitir

1.  Em um ambiente de teste, implante a política de restrição de software com a regra padrão definida como irrestrito e remova as regras adicionais. Se você habilitar o SRP sem forçá-lo a restringir qualquer aplicativo, o SPR poderá monitorar quais aplicativos estão sendo executados.

2.  Crie o seguinte valor de registro para habilitar o recurso de registro em log avançado e defina o caminho para onde o arquivo de log deve ser gravado.

    **"HKEY_LOCAL_MACHINE \SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers"**

    Valor da cadeia *de caracteres: LogFileName caminho para LogFileName*

    Como o SRP está avaliando todos os aplicativos quando eles são executados, uma entrada é gravada no arquivo de log *NameLogFile* cada vez que o aplicativo é executado.

3.  Avaliar o arquivo de log

    Cada entrada de log declara:

    -   o chamador da política de restrição de software e a ID do processo (PID) do processo de chamada

    -   o destino que está sendo avaliado

    -   a regra SRP que foi encontrada quando o aplicativo foi executado

    -   um identificador para a regra de SRP.

    Um exemplo da saída gravada em um arquivo de log:

**Explorer. exe (PID = 4728) identifiedC: \ Windows\system32\onenote.exe como regra usingpath irrestrita, GUID = {320bd852-aa7c-4674-82c5-9a80321670a3}**    Todos os aplicativos e o código associado que o SRP verifica e definem para bloquear serão indicados no arquivo de log, que você pode usar para determinar quais executáveis devem ser considerados para sua lista de permissões.

