---
title: bootcfg rmsw
description: O tópico de comandos do Windows para Bootcfg rmsw, que remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956732f396e0fa353a8acd55953e46605a5c4200
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848439"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                      Descrição                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                   |
| /u <Domain>\\<User>  |          Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.          |
|    /p <Password>     |                                                                 Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                  |
|         /mm          |           Remove a opção/maxmem e seu valor máximo de memória associado da <OSEntryLineNum>especificada. A opção/maxmem especifica a quantidade máxima de RAM que o sistema operacional pode usar.            |
|         /bv          |                     Remove a opção/basevideo do <OSEntryLineNum>especificado. A opção/basevideo instrui o sistema operacional a usar o modo VGA padrão para o driver de vídeo instalado.                     |
|         /so          |                         Remove a opção/SOS da <OSEntryLineNum>especificada. A opção/SOS direciona o sistema operacional para exibir nomes de driver de dispositivo enquanto eles estão sendo carregados.                          |
|         /ng          |                         Remove a opção/noguiboot da <OSEntryLineNum>especificada. A opção/noguiboot desabilita a barra de progresso que aparece antes do prompt de logon CTRL + ALT + del.                          |
| /ID <OSEntryLineNum> | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini do qual as opções de carregamento do sistema operacional são removidas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
|          /?          |                                                                                          Exibe a ajuda no prompt de comando.                                                                                          |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/rmsw**:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
