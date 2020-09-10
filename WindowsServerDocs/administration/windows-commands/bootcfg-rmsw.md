---
title: bootcfg rmsw
description: Artigo de referência para o comando Bootcfg rmsw, que remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.
ms.topic: reference
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9dabe87fe9e56933de1f522728934a7c1dc946f6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630122"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe

```
bootcfg /rmsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /mm | Remove a opção/maxmem e seu valor máximo de memória associado do especificado `<osentrylinenum>` . A opção/maxmem especifica a quantidade máxima de RAM que o sistema operacional pode usar. |
| /bv | Remove a opção/basevideo do especificado `<osentrylinenum>` . A opção/basevideo instrui o sistema operacional a usar o modo VGA padrão para o driver de vídeo instalado. |
| /so | Remove a opção/SOS do especificado `<osentrylinenum>` . A opção/SOS direciona o sistema operacional para exibir nomes de driver de dispositivo enquanto eles estão sendo carregados. |
| /ng | Remove a opção/noguiboot do especificado `<osentrylinenum>` . A opção/noguiboot desabilita a barra de progresso que aparece antes do prompt de logon CTRL + ALT + DEL. |
| `/id <osentrylinenum>` | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo de Boot.ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/rmsw** :

```
bootcfg /rmsw /mm 64 /id 2
bootcfg /rmsw /so /id 3
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /rmsw /ng /id 2
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
