---
title: change user
description: Artigo de referência do comando Change User, que altera o modo de instalação para o servidor de Host da Sessão da Área de Trabalho Remota.
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca78126095b11ca7bf900d10c253cd7ad19fcc12
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892895"
---
# <a name="change-user"></a>change user

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o modo de instalação para o servidor de Host da Sessão da Área de Trabalho Remota.

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
change user {/execute | /install | /query}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /Execute | Habilita o mapeamento de arquivo. ini para o diretório base. Essa é a configuração padrão. |
| /install | Desabilita o mapeamento de arquivo. ini para o diretório base. Todos os arquivos. ini são lidos e gravados no diretório do sistema. Você deve desabilitar o mapeamento de arquivo. ini ao instalar aplicativos em um servidor de Host da Sessão da Área de Trabalho Remota. |
| /Query | Exibe a configuração atual do mapeamento de arquivo. ini. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Use **Change User/install** antes de instalar um aplicativo para criar arquivos. ini para o aplicativo no diretório do sistema. Esses arquivos são usados como a origem quando arquivos. ini específicos do usuário são criados. Depois de instalar o aplicativo, use **Change User/execute** para reverter para o mapeamento de arquivo. ini padrão.

- Na primeira vez que você executar o aplicativo, ele pesquisará o diretório base em busca de seus arquivos. ini. Se os arquivos. ini não forem encontrados no diretório base, mas forem encontrados no diretório do sistema, Serviços de Área de Trabalho Remota copiará os arquivos. ini para o diretório base, garantindo que cada usuário tenha uma cópia exclusiva dos arquivos. ini do aplicativo. Todos os novos arquivos. ini são criados no diretório base.

- Cada usuário deve ter uma cópia exclusiva dos arquivos. ini de um aplicativo. Isso impede instâncias em que diferentes usuários podem ter configurações de aplicativo incompatíveis (por exemplo, diretórios padrão ou resoluções de tela diferentes).

- Quando o sistema estiver executando **Change User/install**, várias coisas ocorrerão. Todas as entradas de registro criadas são sombreadas em **HKEY_LOCAL_MACHINE \Software\microsoft\windows NT\Currentversion\Terminal Server\Install**, na subchave **\Software** ou na subchave **\MACHINE** . As subchaves adicionadas aos **HKEY_CURRENT_USER** são copiadas na subchave **\Software** e as subchaves adicionadas a **HKEY_LOCAL_MACHINE** são copiadas na subchave **\MACHINE** . Se o aplicativo consultar o diretório do Windows usando chamadas do sistema, como GetWindowsdirectory, o servidor host da sessão da área de trabalho remota retornará o diretório systemroot. Se qualquer entrada de arquivo. ini for adicionada usando chamadas do sistema, como WritePrivateProfileString, elas serão adicionadas aos arquivos. ini no diretório systemroot.

- Quando o sistema retorna para **alterar User/execute**e o aplicativo tenta ler uma entrada de registro em **HKEY_CURRENT_USER** que não existe, serviços de área de trabalho remota verifica se existe uma cópia da chave na subchave **\Terminal Server\Install** . Se tiver, as subchaves serão copiadas para o local apropriado em **HKEY_CURRENT_USER**. Se o aplicativo tentar ler um arquivo. ini que não existe, Serviços de Área de Trabalho Remota pesquisará esse arquivo. ini na raiz do sistema. Se o arquivo. ini estiver na raiz do sistema, ele será copiado para o subdiretório \Windows do diretório base do usuário. Se o aplicativo consultar o diretório do Windows, o servidor host da Sessão RD retornará o subdiretório \Windows do diretório base do usuário.

- Quando você faz logon, o Serviços de Área de Trabalho Remota verifica se seus arquivos System. ini são mais novos do que os arquivos. ini em seu computador. Se a versão do sistema for mais recente, seu arquivo. ini será substituído ou mesclado com a versão mais recente. Isso depende se o bit INISYNC, 0x40, está definido para esse arquivo. ini. A versão anterior do arquivo. ini foi renomeada como inifile. CTX. Se os valores do registro do sistema na subchave **\Terminal Server\Install** forem mais recentes do que a sua versão em **HKEY_CURRENT_USER**, sua versão das subchaves será excluída e substituída pelas novas subchaves de **\Terminal Server\Install**.

## <a name="examples"></a>Exemplos

- Para desabilitar o mapeamento de arquivo. ini no diretório base, digite:

  ```
  change user /install
  ```

- Para habilitar o mapeamento de arquivo. ini no diretório base, digite:

  ```
  change user /execute
  ```

- Para exibir a configuração atual do mapeamento de arquivo. ini, digite:

  ```
  change user /query
  ```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [alterar comando](change.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
