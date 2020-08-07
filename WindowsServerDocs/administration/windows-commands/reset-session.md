---
title: reset session
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 2f910dfc1c13b0e8555078acfb4e7ad830049592
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883664"
---
# <a name="reset-session"></a>reset session

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você redefina (exclua) uma sessão em um servidor Host da Sessão da Área de Trabalho Remota (host de sessão da área de trabalho remota).


> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|\<SessionName>|Especifica o nome da sessão que você deseja redefinir. Para determinar o nome da sessão, use o comando **Query Session** .|
|\<SessionID>|Especifica a ID da sessão a ser redefinida.|
|/server:\<ServerName>|Especifica o servidor de terminal que contém a sessão que você deseja redefinir. Caso contrário, o servidor host da Sessão RD atual será usado.|
|/v|Exibe informações sobre as ações que estão sendo executadas.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você sempre pode redefinir suas próprias sessões, mas deve ter permissão de acesso controle total para redefinir a sessão de outro usuário.
-   Lembre-se de que redefinir a sessão de um usuário sem aviso o usuário pode resultar na perda de dados na sessão.
-   Você deve redefinir uma sessão somente quando ela não está funcionando corretamente ou parece ter parado de responder.
-   O parâmetro **/Server** será necessário apenas se você usar **Redefinir sessão** de um servidor remoto.

## <a name="examples"></a>Exemplos
- Para redefinir a sessão de RDP designada-TCP # 6, digite:
  ```
  reset session rdp-tcp#6
  ```
- Para redefinir a sessão que usa a ID de sessão 3, digite:
  ```
  reset session 3
  ```

## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Referência de comando de serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
