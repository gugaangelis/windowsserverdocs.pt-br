---
title: bootcfg addsw
description: O tópico de comandos do Windows para **Bootcfg addsw** -adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2dd727c839babe1ae4f7743285844f35cf5bf76e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380185"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parâmetros

|         Termo         |                                                                                                            Definição                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                        |
| /u <Domain> @ no__t-1 @ no__t-2  |               Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> @ no__t-2 @ no__t-3. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.               |
|    /p <Password>     |                                                                      Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                       |
|   /mm <MaximumRAM>   |                                          Especifica a quantidade máxima de RAM, em megabytes, que o sistema operacional pode usar. O valor deve ser igual ou maior que 32 megabytes.                                          |
|         /bv          |                                    Adiciona a opção **/basevideo** ao <OSEntryLineNum> especificado, direcionando o sistema operacional para usar o modo VGA padrão para o driver de vídeo instalado.                                     |
|         /so          |                                      Adiciona a opção **/SOS** ao *núm_linha_entrada_sist_op*especificado, direcionando o sistema operacional para exibir nomes de driver de dispositivo enquanto eles estão sendo carregados.                                      |
|         /ng          |                                         Adiciona a opção **/noguiboot** ao <OSEntryLineNum> especificado, desabilitando a barra de progresso que aparece antes do prompt de logon Ctrl + Alt + Del.                                          |
| /ID <OSEntryLineNum> | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
|          /?          |                                                                                               Exibe a ajuda no prompt de comando.                                                                                               |

## <a name="BKMK_examples"></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/addsw** :
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
