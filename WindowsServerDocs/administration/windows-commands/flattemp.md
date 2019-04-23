---
title: flattemp
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fc14a6fe1a355f7c20c130fba3fb1f17e49b6f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872927"
---
# <a name="flattemp"></a>flattemp

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita ou desabilita as pastas temporárias simples.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Query|Consulta a configuração atual.|
|/enable|Habilita simples pastas temporárias. Os usuários compartilharão a pasta temporária, a menos que a pasta temporária reside na pasta base usuário s.|
|/disable|Desabilita simples pastas temporárias. Cada pasta temporária do usuário s reside em uma pasta separada (determinada pelo usuário s ID de sessão).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O **flattemp** comando só está disponível quando você instalou o serviço de função do Terminal Server em um computador executando o Windows Server 2008 ou a serviço de função Host de sessão da área de trabalho remota em um computador executando o Windows Server 2008 R2.
-   Você deve ter credenciais administrativas para executar **flattemp**.
-   Depois de cada usuário tem uma pasta temporária exclusiva, use **flattemp /enable** para habilitar as pastas temporárias simples.
-   O método padrão para a criação de pastas temporárias para vários usuários (normalmente, apontadas para as variáveis de ambiente TEMP e TMP) é criar subpastas na **\Temp** pasta, usando a identificação de logon como o nome da subpasta. Por exemplo, se a variável de ambiente TEMP aponta para C:\Temp, a pasta temporária atribuída para a identificação de logon do usuário 4 é c:\Temp\4. Usando o **flattemp**, você pode apontar diretamente para a pasta \Temp e impedir a formação de subpastas. Isso é útil quando você deseja que as pastas temporárias de usuário estejam contidas em pastas base, seja em uma unidade local do servidor de Host de sessão da área de trabalho remota ou em uma unidade de rede compartilhada. Você deve usar o **flattemp /enable** comando somente quando cada usuário tem uma pasta temporária separada.
-   Se a pasta temporária do usuário está em uma unidade de rede, você pode encontrar erros de aplicativo. Isso ocorre quando a unidade de rede compartilhada torna-se momentaneamente inacessível na rede. Como os arquivos temporários do aplicativo estão inacessíveis ou fora de sincronização, ele responde como se o disco foi interrompido. Não é recomendável mover a pasta temporária para uma unidade de rede. O padrão é manter pastas temporárias no disco rígido local. Se você enfrentar um comportamento inesperado ou erros de corrupção de disco com determinados aplicativos, estabilize sua rede ou mover as pastas temporárias de volta para o disco rígido local.
-   Se você desativar usando pastas temporárias separadas por sessão, **flattemp** configurações serão ignoradas. Essa opção é definida na ferramenta de configuração de serviços de área de trabalho remota.

## <a name="BKMK_examples"></a>Exemplos
-   Para exibir a configuração atual de pastas temporárias simples, digite:
    ```
    flattemp /query
    ```
-   Para habilitar as pastas temporárias simples, digite:
    ```
    flattemp /enable
    ```
-   Para desabilitar pastas temporárias simples, digite:
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
