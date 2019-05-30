---
title: winrs
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c54a747f4dde1113fa735c1408f48dbfaf2e74dc
ms.sourcegitcommit: 39ab8041d166e6817a95417d6aa30bc7abeeef54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66260266"
---
# <a name="winrs"></a>winrs

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerenciamento remoto do Windows permite que você gerenciar e executar programas remotamente.   
## <a name="syntax"></a>Sintaxe  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/remote:\<endpoint>|Especifica o ponto de extremidade de destino usando um nome NetBIOS ou a conexão padrão:<br /><br />-   <url>: [\<transport>://]\<target>[:\<port>]<br /><br />Se não for especificado, **/r:localhost** é usado.|  
|/unencrypted|Especifica que as mensagens para o shell remoto não serão criptografadas. Isso é útil para solução de problemas ou quando o tráfego de rede já está criptografado com **ipsec**, ou quando a segurança física é imposta.<br /><br />Por padrão, as mensagens são criptografadas usando chaves de Kerberos ou NTLM.<br /><br />Essa opção de linha de comando é ignorada quando o transporte HTTPS é selecionada.|  
|/username:\<username>|Especifica o nome de usuário na linha de comando.<br /><br />Se não for especificado, a ferramenta usará a autenticação Negotiate ou o prompt para o nome.<br /><br />Se **/username** for especificado, **/password** também deve ser especificado.|  
|/password:\<password>|Especifica a senha na linha de comando.<br /><br />Se **/password** não for especificado, mas **/username** é, a ferramenta solicitará a senha.<br /><br />Se **/password** for especificado, **/username** também deve ser especificado.|  
|/timeout:\<seconds>|Essa opção é preterida.|  
|/directory:\<path>|Especifica o diretório inicial para o shell remoto.<br /><br />Se não for especificado, o shell remoto começará no diretório base do usuário, definido pela variável de ambiente **% USERPROFILE %** .|  
|/Environment:\<cadeia de caracteres > =<value>|Especifica uma variável de ambiente único a ser definido quando inicia do shell, o que permite alterar o ambiente padrão do shell.<br /><br />Várias ocorrências dessa opção devem ser usadas para especificar diversas variáveis de ambiente.|  
|/noecho|Especifica que o eco deve ser desabilitado. Isso pode ser necessário para garantir respostas do usuário aos prompts remotos não são exibidas localmente.<br /><br />Por padrão o eco for "on".|  
|/noprofile|Especifica que o perfil do usuário não deve ser carregado.<br /><br />Por padrão, o servidor tentará carregar o perfil do usuário.<br /><br />Se o usuário remoto não é um administrador local no sistema de destino e, em seguida, essa opção será necessária (o padrão resultará em erro).|  
|/allowdelegate|Especifica que as credenciais do usuário podem ser usadas para acessar um compartilhamento remoto, por exemplo, encontrado em um computador diferente que o ponto de extremidade de destino.|  
|/compression|Ative a compactação.  Instalações antigas em máquinas remotas não podem dar suporte a compactação para que ele está desativado por padrão.<br /><br />Configuração padrão é off, uma vez que as instalações antigas em máquinas remotas podem não dar suporte a compactação.|  
|/usessl|Use uma conexão SSL ao usar um ponto de extremidade remoto.  Especificar isso, em vez de transporte **https:** usará o padrão **WinRM** porta padrão.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
-   Todas as opções de linha de comando aceitam curtas ou longas. Por exemplo, ambos **/r** e **/remota** são válidos.  
-   Para encerrar a **/remota** de comando, o usuário pode digitar **Ctrl-C** ou **Ctrl-break**, que será enviado para o shell remoto. A segunda **Ctrl-C** vai forçar o encerramento da **winrs.exe**.  
-   Para gerenciar o Active Directory shells remotos ou winrs configuração, use a ferramenta de WinRM.  O URI é o alias para gerenciar os shells do Active Directory **shell/cmd**.  É o URI de alias para a configuração winrs **winrm/config/winrs**.  

## <a name="BKMK_Examples"></a>Exemplos  
```  
winrs /r:https://contoso.com command  
```  
```  
winrs /r:contoso.com /usessl command  
```  
```  
winrs /r:myserver command  
```  
```  
winrs /r:http://127.0.0.1 command  
```  
```  
winrs /r:http://169.51.2.101:80 /unencrypted command  
```  
```  
winrs /r:https://[::FFFF:129.144.52.38] command  
```  
```  
winrs /r:http://[1080:0:0:0:8:800:200C:417A]:80 command  
```  
```  
winrs /r:https://contoso.com /t:600 /u:administrator /p:$%fgh7 ipconfig  
```  
```  
winrs /r:myserver /env:path=^%path^%;c:\tools /env:TEMP=d:\temp config.cmd  
```  
```  
winrs /r:myserver netdom join myserver /domain:testdomain /userd:johns /passwordd:$%fgh789  
```  
```  
winrs /r:myserver /ad /u:administrator /p:$%fgh7 dir \\anotherserver\share  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
