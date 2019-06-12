---
title: tscon
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8cac59b2f5524df5a82e9c83424fd781f0ef7c8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440930"
---
# <a name="tscon"></a>tscon

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se a outra sessão em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).  
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).  

> [!NOTE]  
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.  

## <a name="syntax"></a>Sintaxe  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Parâmetros  

|Parâmetro|Descrição|  
|-------|--------|  
|\<SessionID>|Especifica a ID da sessão para o qual você deseja se conectar. Se você usar opcional **/dest:** <*SessionName*> parâmetro, isso é a ID da sessão para o qual você deseja se conectar.|  
|\<SessionName>|Especifica o nome da sessão à qual você deseja se conectar.|  
|/dest:\<SessionName>|Especifica o nome da sessão atual. Esta sessão será desconectada quando você se conectar à nova sessão.|  
|/password:\<pw>|Especifica a senha do usuário que possui a sessão à qual você deseja se conectar. Esta senha é necessária quando o usuário conectado não possui a sessão.|  
|/Password: *|solicita a senha do usuário que possui a sessão à qual você deseja se conectar.|  
|/v|Exibe informações sobre as ações que está sendo executada.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
-   Você deve ter permissão de acesso de controle total ou a permissão de acesso especial para se conectar a outra sessão de conexão.  
-   O **/dest:** <*SessionName*> parâmetro permite que você se conecte a sessão de outro usuário a uma sessão diferente.  
-   Se você não especificar uma senha no <*senha*> parâmetro e a sessão de destino pertence a um usuário diferente do atual, **tscon** falhar.  
-   Você não pode se conectar à sessão de console.  

## <a name="BKMK_examples"></a>Exemplos  
- Para se conectar à sessão 12 no servidor de Host de sessão de área de trabalho remota atual e desconectar a sessão atual, digite:  
  ```  
  tscon 12  
  ```  
- Para conectar-se à sessão 23 no servidor de Host de sessão de área de trabalho remota atual, usando a senha minha_senha e desconectar a sessão atual, digite:  
  ```  
  tscon 23 /password:mypass  
  ```  
- Para conectar-se a sessão denominada TERM03 à sessão denominada TERM05 e desconectar a sessão TERM05, se ele estiver conectado, digite:  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  #### <a name="additional-references"></a>Referências adicionais  
  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  [Serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)  
