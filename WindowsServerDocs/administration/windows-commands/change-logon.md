---
title: change logon
description: Artigo de referência para o comando Change login, que habilita ou desabilita os logons de sessões de cliente ou exibe o status de logon atual.
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1196ec063ce6ab6d2ede64d58d22608e2e76c48
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880272"
---
# <a name="change-logon"></a>change logon

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita ou desabilita os logons de sessões de cliente ou exibe o status de logon atual. Esse utilitário é útil para manutenção do sistema. Você deve ser um administrador para executar este comando.

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /Query | Exibe o status de logon atual, se habilitado ou desabilitado. |
| /Enable | Habilita logons de sessões de cliente, mas não do console do. |
| /Disable | Desabilita logons subsequentes de sessões de cliente, mas não do console do. Não afeta usuários conectados no momento. |
| /drain | Desabilita logons de novas sessões de cliente, mas permite reconexões com sessões existentes. |
| /drainuntilrestart | Desabilita logons de novas sessões de cliente até que o computador seja reiniciado, mas permite reconexões com sessões existentes. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os logons são habilitados novamente quando o sistema é reiniciado.

- Se você estiver conectado ao servidor de Host da Sessão da Área de Trabalho Remota de uma sessão de cliente e, em seguida, desabilitar logons e fazer logoff antes de reabilitar os logons, você não poderá se reconectar à sua sessão. Para reabilitar logons de sessões de cliente, faça logon no console do.

### <a name="examples"></a>Exemplos

- Para exibir o status de logon atual, digite:

  ```
  change logon /query
  ```

- Para habilitar logons de sessões de cliente, digite:

  ```
  change logon /enable
  ```

- Para desabilitar os logons do cliente, digite:

  ```
  change logon /disable
  ```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [alterar comando](change.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
