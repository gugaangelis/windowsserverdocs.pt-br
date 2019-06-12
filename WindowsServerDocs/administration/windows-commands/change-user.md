---
title: change user
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bedef9f996554a3b5745b47f646204646fdfa9a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434416"
---
# <a name="change-user"></a>change user

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o modo de instalação para o servidor Host da sessão da área de trabalho remota (Host de sessão rd).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> ## <a name="syntax"></a>Sintaxe
> ```
> change user {/execute | /install | /query}
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> | Parâmetro |                                                                                                 Descrição                                                                                                  |
> |-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | /execute  |                                                                Permite o mapeamento de arquivo. ini no diretório base. Essa é a configuração padrão.                                                                 |
> | /install  | Desabilita o mapeamento de arquivo. ini no diretório base. Todos os arquivos. ini são lidos e gravados no diretório do sistema. Você deve desabilitar o mapeamento de arquivo. ini ao instalar aplicativos em um servidor de Host de sessão de área de trabalho remota. |
> |  /Query   |                                                                             Exibe a configuração atual para o mapeamento de arquivo. ini.                                                                              |
> |    /?     |                                                                                     Exibe a ajuda no prompt de comando.                                                                                     |
> 
> ## <a name="remarks"></a>Comentários
> - Use **alteração de usuário /install** antes de instalar um aplicativo para criar arquivos. ini para o aplicativo no diretório do sistema. Esses arquivos são usados como a origem quando os arquivos. ini de específicas do usuário são criados. Depois de instalar o aplicativo, use **Alterar usuário / executar** para reverter para o mapeamento de arquivo. ini padrão.
> - Na primeira vez que você executar o aplicativo, ele pesquisa o diretório base para seus arquivos. ini. Se os arquivos. ini não são encontrados no diretório base, mas são encontrados no diretório do sistema, o Remote Desktop Services copia os arquivos. ini para o diretório inicial, garantindo que cada usuário tenha uma cópia única dos arquivos. ini do aplicativo. Quaisquer novos arquivos. ini são criados no diretório base.
> - Cada usuário deve ter uma cópia única dos arquivos. ini para um aplicativo. Isso evita que instâncias onde usuários diferentes podem ter configurações de aplicativo incompatível (por exemplo, os diretórios padrão diferente ou resoluções de tela).
> - Quando o sistema está no modo de instalação (ou seja, **alteração de usuário /install**), várias coisas ocorrem. Todas as entradas do registro que são criadas são sombreadas sob **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentversion\Terminal Server\Install**, em ambos os **\SOFTWARE** subchave ou o **\MACHINE** subchave. Subchaves adicionado ao **HKEY_CURrenT_USER** são copiadas sob a **\SOFTWARE** subchave e subchaves adicionados ao **HKEY_LOCAL_MACHINE** são copiadas sob a **\ MÁQUINA** subchave. Se o aplicativo consulta o diretório do Windows por meio de chamadas do sistema, como GetWindowsdirectory, o servidor de Host de sessão de área de trabalho remota retorna o diretório systemroot. Se qualquer entrada do arquivo. ini é adicionada por meio de chamadas do sistema, como a WritePrivateProfileString, eles serão adicionados aos arquivos. ini no diretório raiz do sistema.
> - Quando o sistema retorna ao modo de execução (ou seja, **Alterar usuário /execute**), e o aplicativo tenta ler uma entrada de registro sob **HKEY_CURrenT_USER** que não existe, serviços de área de trabalho remota verifica se existe uma cópia da chave de **\Terminal Server\Install** subchave. Se isso acontecer, as subchaves são copiadas para o local apropriado em **HKEY_CURrenT_USER**. Se o aplicativo tenta ler de um arquivo. ini que não existe, o Remote Desktop Services procura esse arquivo. ini na raiz do sistema. Se o arquivo. ini estiver na raiz do sistema, ele é copiado para o subdiretório \Windows da pasta base do usuário. Se o aplicativo consulta o diretório do Windows, o servidor de Host de sessão de área de trabalho remota retorna o subdiretório \Windows da pasta base do usuário.
> - Quando você faz logon, os serviços de área de trabalho remota verifica se os seus arquivos. ini do sistema são mais recentes que os arquivos. ini em seu computador. Se a versão do sistema for mais recente, seu arquivo. ini é substituído ou mesclado com a versão mais recente. Isso depende se o bit INISYNC, 0x40, é definida para esse arquivo. ini. A versão anterior do arquivo. ini é renomeada como inifile. Se os valores de registro do sistema sob a **\Terminal Server\Install** subchave são mais recentes que sua versão sob **HKEY_CURrenT_USER**, sua versão das subchaves é excluído e substituído por novas sub-chaves partir **\Terminal Server\Install**.
>   ## <a name="BKMK_examples"></a>Exemplos
> - Para desabilitar o mapeamento de arquivo. ini no diretório inicial, digite:
>   ```
>   change user /install
>   ```
> - Para habilitar o mapeamento de arquivo. ini no diretório inicial, digite:
>   ```
>   change user /execute
>   ```
> - Para exibir a configuração atual para o mapeamento de arquivo. ini, digite:
>   ```
>   change user /query
>   ```
>   #### <a name="additional-references"></a>Referências adicionais
>   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   [alterar](change.md)
>   [Remote Desktop Services &#40;serviços de Terminal&#41; referência de comandos](remote-desktop-services-terminal-services-command-reference.md)
