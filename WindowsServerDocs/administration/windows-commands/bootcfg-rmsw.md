---
title: bootcfg rmsw
description: O tópico de comandos do Windows para **Bootcfg rmsw** – remove opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43629f2e13bb6269a43d592fa0907637135aea71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379860"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                      Descrição                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                   |
| /u <Domain> @ no__t-1 @ no__t-2  |          Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> @ no__t-2 @ no__t-3. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.          |
|    /p <Password>     |                                                                 Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                  |
|         /mm          |           Remove a opção/maxmem e seu valor máximo associado da memória do <OSEntryLineNum> especificado. A opção/maxmem especifica a quantidade máxima de RAM que o sistema operacional pode usar.            |
|         /bv          |                     Remove a opção/basevideo do <OSEntryLineNum> especificado. A opção/basevideo instrui o sistema operacional a usar o modo VGA padrão para o driver de vídeo instalado.                     |
|         /so          |                         Remove a opção/SOS do <OSEntryLineNum> especificado. A opção/SOS direciona o sistema operacional para exibir nomes de driver de dispositivo enquanto eles estão sendo carregados.                          |
|         /ng          |                         Remove a opção/noguiboot do <OSEntryLineNum> especificado. A opção/noguiboot desabilita a barra de progresso que aparece antes do prompt de logon CTRL + ALT + del.                          |
| /ID <OSEntryLineNum> | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini do qual as opções de carregamento do sistema operacional são removidas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
|          /?          |                                                                                          Exibe a ajuda no prompt de comando.                                                                                          |

## <a name="BKMK_examples"></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/rmsw**:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
