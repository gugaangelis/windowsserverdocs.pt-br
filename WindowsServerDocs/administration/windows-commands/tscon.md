---
title: tscon
description: Tópico de comandos do Windows para tscon, que se conecta a outra sessão em um servidor Host da Sessão da Área de Trabalho Remota (host de Sessão RD).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1666e3fd47fab89b63efccef43489d15930cd6de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832529"
---
# <a name="tscon"></a>tscon

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se a outra sessão em um servidor Host da Sessão da Área de Trabalho Remota.  

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).  

> [!NOTE]  
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.  

## <a name="syntax"></a>Sintaxe  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
### <a name="parameters"></a>Parâmetros  

|Parâmetro|Descrição|  
|-------|--------|  
|\<SessionID >|Especifica a ID da sessão à qual você deseja se conectar. Se você usar o parâmetro opcional **/dest:** <*SessionName*>, essa será a ID da sessão à qual você deseja se conectar.|  
|\<SessionName >|Especifica o nome da sessão à qual você deseja se conectar.|  
|/DEST:\<SessionName >|Especifica o nome da sessão atual. Esta sessão será desconectada quando você se conectar à nova sessão.|  
|/Password:\<PW >|Especifica a senha do usuário que possui a sessão à qual você deseja se conectar. Essa senha é necessária quando o usuário que está se conectando não possui a sessão.|  
|/Password: *|solicita a senha do usuário que possui a sessão à qual você deseja se conectar.|  
|/v|Exibe informações sobre as ações que estão sendo executadas.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
-   Você deve ter permissão de acesso controle total ou conectar permissão de acesso especial para se conectar a outra sessão.  
-   O parâmetro **/dest:** <*SessionName*> permite que você conecte a sessão de outro usuário a uma sessão diferente.  
-   Se você não especificar uma senha no parâmetro <*senha*> e a sessão de destino pertencer a um usuário diferente do atual, o **tscon** falhará.  
-   Você não pode se conectar à sessão do console.  

## <a name="examples"></a><a name=BKMK_examples></a>Disso  
- Para se conectar à sessão 12 no servidor de host da Sessão RD atual e desconectar a sessão atual, digite:  
  ```  
  tscon 12  
  ```  
- Para se conectar à sessão 23 no servidor host da Sessão RD atual, usando a senha mypass e desconectar a sessão atual, digite:  
  ```  
  tscon 23 /password:mypass  
  ```  
- Para conectar a sessão chamada TERM03 à sessão chamada TERM05 e desconectar a sessão TERM05, se ela estiver conectada, digite:  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
  - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)  
