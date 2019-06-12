---
title: bootcfg rmsw
description: Tópico de comandos do Windows para **rmsw bootcfg** - remove operacional opções de carregamento do sistema para uma entrada de sistema operacional especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f3d873cffbdb386b5f4f564801a4f4b815c6987a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434684"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificado.

## <a name="syntax"></a>Sintaxe
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                      Descrição                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                   |
| /u <Domain>\\<User>  |          Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.          |
|    /p <Password>     |                                                                 Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                                  |
|         /mm          |           Remove a opção /maxmem e seu valor máximo de memória associado especificado <OSEntryLineNum>. A opção /maxmem Especifica a quantidade máxima de RAM que o sistema operacional pode usar.            |
|         /bv          |                     Remove a opção /basevideo especificado <OSEntryLineNum>. A opção /basevideo direciona o sistema operacional para usar o modo para o driver de vídeo instalado.                     |
|         /so          |                         Remove a opção /sos especificado <OSEntryLineNum>. A opção instrui o sistema operacional para exibir os nomes de driver de dispositivo enquanto estão sendo carregados.                          |
|         /ng          |                         Remove a opção /noguiboot especificado <OSEntryLineNum>. Essa opção desabilita a barra de progresso aparece antes do CTRL + ALT + del prompt de logon.                          |
| /id <OSEntryLineNum> | Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini do qual as opções de carregamento do sistema operacional são removidas. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1. |
|          /?          |                                                                                          Exibe a ajuda no prompt de comando.                                                                                          |

## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /rmsw**comando:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
