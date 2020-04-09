---
title: winrs
description: O tópico de comandos do Windows para o WinRS, que permite que você gerencie e execute programas remotamente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b714f851c981611bbe4d4f26a7b7eed2db7903
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829139"
---
# <a name="winrs"></a>winrs

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O gerenciamento remoto do Windows permite que você gerencie e execute programas remotamente.   
## <a name="syntax"></a>Sintaxe  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
#### <a name="parameters"></a>Parâmetros  

|           Parâmetro            |                                                                                                                                                                                    Descrição                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Remote: ponto de extremidade de\<>       |                                                                                          Especifica o ponto de extremidade de destino usando um nome NetBIOS ou a conexão padrão:<p>-   <url>: [\<Transport >://]\<destino > [:\<porta >]<p>Se não for especificado, **/r: localhost** será usado.                                                                                          |
|          /unencrypted          | Especifica que as mensagens para o shell remoto não serão criptografadas. Isso é útil para solucionar problemas ou quando o tráfego de rede já está criptografado usando **IPSec**ou quando a segurança física é imposta.<p>Por padrão, as mensagens são criptografadas usando chaves Kerberos ou NTLM.<p>Essa opção de linha de comando é ignorada quando o transporte HTTPS é selecionado. |
|     /username:\<nome de usuário >      |                                                                                Especifica o nome de usuário na linha de comando.<p>Se não for especificado, a ferramenta usará a autenticação Negotiate ou solicitará o nome.<p>Se **/username** for especificado, **/password** também deverá ser especificado.                                                                                 |
|     /Password:\<de senha >      |                                                                           Especifica a senha na linha de comando.<p>Se **/password** não for especificado, mas **/username** for, a ferramenta solicitará a senha.<p>Se **/password** for especificado, **/username** também deverá ser especificado.                                                                            |
|      /timeout:\<segundos >       |                                                                                                                                                                             Esta opção foi preterida.                                                                                                                                                                             |
|       /diretório: caminho de\<>       |                                                                                            Especifica o diretório inicial para o shell remoto.<p>Se não for especificado, o shell remoto será iniciado no diretório base do usuário definido pela variável de ambiente **% USERPROFILE%** .                                                                                             |
| /Environment: cadeia de caracteres de\<> =<value> |                                                                          Especifica uma única variável de ambiente a ser definida quando o Shell é iniciado, o que permite a alteração do ambiente padrão para o Shell.<p>Várias ocorrências dessa opção devem ser usadas para especificar várias variáveis de ambiente.                                                                          |
|            /noecho             |                                                                                                    Especifica que o eco deve ser desabilitado. Isso pode ser necessário para garantir que as respostas do usuário para prompts remotos não sejam exibidas localmente.<p>Por padrão, o eco está ativado.                                                                                                    |
|           /noprofile           |                                              Especifica que o perfil do usuário não deve ser carregado.<p>Por padrão, o servidor tentará carregar o perfil do usuário.<p>Se o usuário remoto não for um administrador local no sistema de destino, essa opção será necessária (o padrão resultará em erro).                                               |
|         /allowdelegate         |                                                                                                                  Especifica que as credenciais do usuário podem ser usadas para acessar um compartilhamento remoto, por exemplo, encontradas em um computador diferente do ponto de extremidade de destino.                                                                                                                   |
|          /Compression          |                                                                           Ative a compactação.  As instalações mais antigas em máquinas remotas podem não dar suporte à compactação, portanto, estão desativadas por padrão.<p>A configuração padrão é off, já que as instalações mais antigas em máquinas remotas podem não dar suporte à compactação.                                                                           |
|            /usessl             |                                                                                                               Use uma conexão SSL ao usar um ponto de extremidade remoto.  Especificando isso em vez do HTTPS de transporte **:** usará a porta padrão **WinRM** default.                                                                                                                |
|               /?               |                                                                                                                                                                        Exibe a ajuda no prompt de comando.                                                                                                                                                                        |

## <a name="remarks"></a>Comentários  
-   Todas as opções de linha de comando aceitam forma abreviada ou longa. Por exemplo, **/r** e **/Remote** são válidos.  
-   Para encerrar o comando **/Remote** , o usuário pode digitar **Ctrl-C** ou **Ctrl-Break**, que será enviado para o shell remoto. O segundo **Ctrl-C** forçará o encerramento de **winrs. exe**.  
-   Para gerenciar os shells remotos ativos ou a configuração do WinRS, use a ferramenta WinRM.  O alias de URI para gerenciar shells ativos é **shell/cmd**.  O alias do URI para a configuração do WinRS é **WinRM/config/WinRS**.  

## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  

