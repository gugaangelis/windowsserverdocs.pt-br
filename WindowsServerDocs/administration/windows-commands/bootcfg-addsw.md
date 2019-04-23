---
title: bootcfg addsw
description: Tópico de comandos do Windows para **addsw bootcfg** -adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 768e9c5bcf8a5d272927d013ff4accf5c3237219
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862867"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificado.

## <a name="syntax"></a>Sintaxe
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parâmetros
|Termo|Definição|
|----|-------|
|/s <computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u <Domain>\\<User>|Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.|
|/p <Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/mm <MaximumRAM>|Especifica a quantidade máxima de RAM, em megabytes, que o sistema operacional pode usar. O valor deve ser igual ou maior que 32 Megabytes.|
|/bv|Adiciona o **/basevideo** opção especificado <OSEntryLineNum>, instruindo o sistema operacional para usar o modo para o driver de vídeo instalado.|
|/so|Adiciona o **/sos** opção especificado *NúmLinhaEntradaSistOp*, instruindo o sistema operacional para exibir os nomes de driver de dispositivo enquanto estão sendo carregados.|
|/ng|Adiciona o **/noguiboot** opção especificado <OSEntryLineNum>, desabilitando a barra de progresso aparece antes do CTRL + ALT + del logon prompt.|
|/id <OSEntryLineNum>|Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini para o qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /addsw** comando:
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
