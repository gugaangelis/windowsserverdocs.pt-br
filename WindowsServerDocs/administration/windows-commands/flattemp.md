---
title: flattemp
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a291c102d70ff9166a7bb0261e506792a49dc18
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844569"
---
# <a name="flattemp"></a>flattemp

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita ou desabilita pastas temporárias simples.
para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
flattemp {/query | /enable | /disable}
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Query|Consulta a configuração atual.|
|/Enable|Habilita pastas temporárias simples. Os usuários irão compartilhar a pasta temporária, a menos que a pasta temporária resida na pasta base do usuário.|
|/Disable|Desabilita as pastas temporárias simples. Cada pasta temporária do usuário residirá em uma pasta separada (determinada pela ID de sessão do usuário).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O comando **flattemp** só está disponível quando você instalou o serviço de função Terminal Server em um computador que executa o windows Server 2008 ou o serviço de função de host da Sessão RD em um computador que executa o windows Server 2008 R2.
-   Você deve ter credenciais administrativas para executar **flattemp**.
-   Depois que cada usuário tiver uma pasta temporária exclusiva, use **flattemp/Enable** para habilitar pastas temporárias simples.
-   O método padrão para criar pastas temporárias para vários usuários (geralmente apontados pelas variáveis de ambiente TEMP e TMP) é criar subpastas na pasta **\temp** , usando o LogonId como o nome da subpasta. Por exemplo, se a variável de ambiente TEMP aponta para C:\Temp, a pasta temporária atribuída ao usuário LogonId 4 é C:\Temp\4. Usando **flattemp**, você pode apontar diretamente para a pasta \temp e impedir que as subpastas sejam formadas. Isso é útil quando você deseja que as pastas temporárias do usuário estejam contidas em pastas base, seja em uma unidade local do servidor host da sessão da área de trabalho remota ou em uma unidade de rede compartilhada. Você deve usar o comando **flattemp/Enable** somente quando cada usuário tiver uma pasta temporária separada.
-   Você poderá encontrar erros de aplicativo se a pasta temporária do usuário estiver em uma unidade de rede. Isso ocorre quando a unidade de rede compartilhada torna-se momentaneamente inacessível na rede. Como os arquivos temporários do aplicativo são inacessíveis ou estão fora de sincronização, ele responde como se o disco fosse interrompido. Não é recomendável mover a pasta temporária para uma unidade de rede. O padrão é manter as pastas temporárias no disco rígido local. Se você tiver um comportamento inesperado ou erros de corrupção de disco com determinados aplicativos, estabilize sua rede ou mova as pastas temporárias de volta para o disco rígido local.
-   Se você desabilitar o uso de pastas temporárias separadas por sessão, as configurações de **flattemp** serão ignoradas. Essa opção é definida na ferramenta de configuração do Serviços de Área de Trabalho Remota.

## <a name="examples"></a><a name=BKMK_examples></a>Disso
-   Para exibir a configuração atual de pastas temporárias simples, digite:
    ```
    flattemp /query
    ```
-   Para habilitar pastas temporárias simples, digite:
    ```
    flattemp /enable
    ```
-   Para desabilitar pastas temporárias simples, digite:
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
