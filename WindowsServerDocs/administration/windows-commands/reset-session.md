---
title: reset session
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5a0991c76ba890bb94b0dcf258df6207ed228e72
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441788"
---
# <a name="reset-session"></a>reset session

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você redefina (excluir) uma sessão em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).  
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).  

> [!NOTE]  
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.  

## <a name="syntax"></a>Sintaxe  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>Parâmetros  

|Parâmetro|Descrição|  
|-------|--------|  
|\<SessionName>|Especifica o nome da sessão que você deseja redefinir. Para determinar o nome da sessão, use o **sessão de consulta** comando.|  
|\<SessionID>|Especifica a ID da sessão para redefinir.|  
|/server:\<ServerName>|Especifica o servidor de terminal que contém a sessão que você deseja redefinir. Caso contrário, o servidor de Host de sessão de área de trabalho remota atual é usado.|  
|/v|Exibe informações sobre as ações que está sendo executada.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
-   Você sempre poderá redefinir suas próprias sessões, mas você deve ter permissão de acesso de controle total para redefinir a sessão de outro usuário.  
-   Lembre-se de que a redefinição de uma sessão de usuário sem avisar o usuário pode resultar em perda de dados da sessão.  
-   Você deve redefinir uma sessão apenas quando ele ou problemas parece ter parado de responder.  
-   O **/server** parâmetro é necessário apenas se você usar **redefinir sessão** de um servidor remoto.  

## <a name="BKMK_examples"></a>Exemplos  
- Para redefinir a sessão designada como rdp-tcp #6, digite:  
  ```  
  reset session rdp-tcp#6  
  ```  
- Para redefinir a sessão que usa a identificação 3, digite:  
  ```  
  reset session 3  
  ```  

#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)  
