---
title: conjunto de Telnet
description: Tópico de comandos do Windows para telnet definido, que define opções.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8fefc18bde39713c3864361118ff4440ca12b0d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833159"
---
# <a name="telnet-set"></a>Telnet: definir

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define opções.   

## <a name="syntax"></a>Sintaxe  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
#### <a name="parameters"></a>Parâmetros  

|                    Parâmetro                     |                                                                                                                                              Descrição                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 Envia **Backspace** como uma **exclusão**.                                                                                                                                  |
|                       CRLF                       |                                                                                                        Envia CR & LF (0x0D, 0x 0A) quando a tecla **Enter** é pressionada. Conhecido como modo de nova linha.                                                                                                        |
|                     delasbs                      |                                                                                                                                 Envia **delete** como um **Backspace**.                                                                                                                                  |
|                <Character> de escape                | Define o caractere de escape usado para entrar no prompt do cliente Telnet. O caractere de escape pode ser um único caractere ou pode ser uma combinação da tecla **Ctrl** mais um caractere. Para definir uma combinação de chave de controle, mantenha pressionada a tecla **Ctrl** enquanto digita o caractere que você deseja atribuir. |
|                    localecho                     |                                                                                                                                         Ativa o eco local.                                                                                                                                          |
|                <FileName> de arquivo de log                |                                                                                               Registra a sessão Telnet atual no arquivo local. O registro em log inicia automaticamente quando você define essa opção.                                                                                               |
|                     log                      |                                                                                                                  Ativa o registro em log. Se nenhum arquivo de log estiver definido, uma mensagem de erro será exibida.                                                                                                                   |
|           Mode {tela &#124; do console}           |                                                                                                                                       Define o modo de operação.                                                                                                                                        |
|                       NTLM                       |                                                                                                                                     Ativa a autenticação NTLM.                                                                                                                                     |
| termo {ANSI &#124; vt100 &#124; vt52 &#124; VTNT} |                                                                                                                                        Define o tipo de terminal.                                                                                                                                        |
|                        ?                         |                                                                                                                                    Exibe a ajuda para este comando.                                                                                                                                    |

## <a name="remarks"></a>Comentários  
1. Você pode usar o comando de **desdefinição** para desativar uma opção que foi definida anteriormente.  
2. Em versões do telnet que não estão em inglês, o <option> de **códigoset** está disponível. O conjunto de **código** <option> define o código atual definido como uma opção, que pode ser qualquer um dos seguintes: **Shift JIS**, **EUC japonês**, **JIS kanji**, **JIS kanji (78)** , **Dec kanji**, **NEC kanji**. Você deve definir o mesmo conjunto de código no computador remoto.  
   ## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
   Defina o arquivo de log e comece a fazer logon no arquivo local tnlog. txt  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>Referências adicionais  
3. - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
