---
title: conjunto de Telnet
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b68ce0ee87d80b25cf13db5bebc6c407a9fe091f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441024"
---
# <a name="telnet-set"></a>telnet: set

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define as opções.   
## <a name="syntax"></a>Sintaxe  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>Parâmetros  

|                    Parâmetro                     |                                                                                                                                              Descrição                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 Envia **Backspace** como um **excluir**.                                                                                                                                  |
|                       CRLF                       |                                                                                                        Envia a CR e LF (0x0D, 0A x 0) quando o **Enter** tecla é pressionada. Conhecido como o novo modo de linha.                                                                                                        |
|                     delasbs                      |                                                                                                                                 Envia **exclua** como um **Backspace**.                                                                                                                                  |
|                escape <Character>                | Define o caractere de escape usado para inserir o prompt de cliente telnet. O caractere de escape pode ser um único caractere, ou ele pode ser uma combinação de **CTRL** chave além de um caractere. Para definir uma combinação de teclas de controle, mantenha pressionada a **CTRL** pressionada enquanto você digita o caractere que você deseja atribuir. |
|                    eco local                     |                                                                                                                                         Ativa o eco local.                                                                                                                                          |
|                logfile <FileName>                |                                                                                               Registra a sessão de telnet atual para o arquivo local. O log começa automaticamente quando você define esta opção.                                                                                               |
|                     logging                      |                                                                                                                  Ativa o registro em log. Se nenhum arquivo de log for definido, uma mensagem de erro é exibida.                                                                                                                   |
|           modo de {console &#124; tela}           |                                                                                                                                       Define o modo de operação.                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     Ativa a autenticação NTLM.                                                                                                                                     |
| term {ansi &#124; vt100 &#124; vt52 &#124; vtnt} |                                                                                                                                        Define o tipo de terminal.                                                                                                                                        |
|                        ?                         |                                                                                                                                    Exibe a Ajuda para este comando.                                                                                                                                    |

## <a name="remarks"></a>Comentários  
1. Você pode usar o **unset** comando para desativar uma opção definida anteriormente.  
2. Em versões diferentes do inglês do telnet, o **conjunto de código** <option> está disponível. **Conjunto de código** <option> define o código atual definido como uma opção, que pode ser qualquer um dos seguintes: **shift JIS**, **EUC japonês**, **JIS Kanji**, **JIS Kanji (78)** , **DEC Kanji**, **NEC Kanji**. Você deve definir o mesmo código definido no computador remoto.  
   ## <a name="BKMK_Examples"></a>Exemplos  
   Definir o arquivo de log e iniciar o registro em log para o arquivo local tnlog.txt  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>Referências adicionais  
3. [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
