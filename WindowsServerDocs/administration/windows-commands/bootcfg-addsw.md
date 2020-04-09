---
title: bootcfg addsw
description: O tópico de comandos do Windows para Bootcfg addsw, que adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ae5175dfc3b068276f6ab95d6823699c96b2b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848709"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Parâmetros

|         Termo         |                                                                                                            Definição                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                        |
| /u <Domain>\\<User>  |               Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.               |
|    /p <Password>     |                                                                      Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                       |
|   /mm <MaximumRAM>   |                                          Especifica a quantidade máxima de RAM, em megabytes, que o sistema operacional pode usar. O valor deve ser igual ou maior que 32 megabytes.                                          |
|         /bv          |                                    Adiciona a opção **/basevideo** ao <OSEntryLineNum>especificado, direcionando o sistema operacional para usar o modo VGA padrão para o driver de vídeo instalado.                                     |
|         /so          |                                      Adiciona a opção **/SOS** ao *núm_linha_entrada_sist_op*especificado, direcionando o sistema operacional para exibir nomes de driver de dispositivo enquanto eles estão sendo carregados.                                      |
|         /ng          |                                         Adiciona a opção **/noguiboot** ao <OSEntryLineNum>especificado, desabilitando a barra de progresso que aparece antes do prompt de logon Ctrl + Alt + Del.                                          |
| /ID <OSEntryLineNum> | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
|          /?          |                                                                                               Exibe a ajuda no prompt de comando.                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/addsw** :
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
