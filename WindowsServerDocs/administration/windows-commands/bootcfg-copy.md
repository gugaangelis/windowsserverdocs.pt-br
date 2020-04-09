---
title: bootcfg copy
description: O tópico de comandos do Windows para Bootcfg Copy, que faz uma cópia de uma entrada de inicialização existente, à qual você pode adicionar opções de linha de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5194418a07aece4f15a84c3eccbc044431a865b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848679"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz uma cópia de uma entrada de inicialização existente, à qual você pode adicionar opções de linha de comando.

## <a name="syntax"></a>Sintaxe
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                             Descrição                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User>  | Executa o comando com as permissões de conta do usuário especificado por <User>ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
|    /p <Password>     |                                                        Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                        |
|   /d <Description>   |                                                                    Especifica a descrição para a nova entrada do sistema operacional.                                                                    |
| /ID <OSEntryLineNum> |         Especifica o número da linha da entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini a ser copiado. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.         |
|          /?          |                                                                                Exibe a ajuda no prompt de comando.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/Copy** para copiar a entrada de inicialização 1 e inserir o \ABC Server\\ como a descrição:
```
bootcfg /copy /d \ABC Server\ /id 1
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
