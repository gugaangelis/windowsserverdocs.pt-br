---
title: bootcfg raw
description: Tópico de comandos do Windows para Bootcfg RAW, que adiciona opções de carregamento do sistema operacional, especificadas como uma cadeia de caracteres, a uma entrada do sistema operacional na seção do sistema operacional do arquivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd5137187c5ba1dc1b410d728f1c2930ddcbc3cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848509"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada do sistema operacional na seção **[Operating Systems]** do arquivo boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                            Definição                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                         |
| /u <Domain> \\<User>  |               Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.                |
|     /p <Password>     |                                                                       Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                       |
| <OSLoadOptionsString> | Especifica as opções de carregamento do sistema operacional a serem adicionadas à entrada do sistema operacional. Essas opções de carregamento substituirão todas as opções de carregamento existentes associadas à entrada do sistema operacional. Nenhuma validação de <OSLoadOptions> é feita. |
| /ID <OSEntryLineNum>  |                       Especifica o número da linha da entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini a ser atualizado. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.                       |
|          /a           |                                                       Especifica que as opções de sistema operacional que estão sendo adicionadas devem ser acrescentadas a qualquer opção de sistema operacional existente.                                                        |
|          /?           |                                                                                               Exibe a ajuda no prompt de comando.                                                                                                |

##### <a name="remarks"></a>Comentários
- **Bootcfg RAW** é usado para adicionar texto ao final de uma entrada do sistema operacional, substituindo todas as opções de entrada do sistema operacional existente. Esse texto deve conter opções de carregamento de sistema operacional válidas, como **/debug**, **/fastdetect**, **/NODEBUG**, **/baudrate**, **/crashdebug**e **/SOS**. Por exemplo, o comando a seguir adiciona **/debug/fastdetect** ao final da primeira entrada do sistema operacional, substituindo todas as opções de entrada do sistema operacional anterior:
  ```
  bootcfg /raw /debug /fastdetect /id 1
  ```
  ## <a name="examples"></a><a name=BKMK_examples></a>Disso
  Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/RAW** :
  ```
  bootcfg /raw /debug /sos /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
  ```
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
