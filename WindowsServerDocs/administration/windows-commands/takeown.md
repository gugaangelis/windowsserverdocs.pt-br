---
title: takeown
description: Saiba como obter acesso a um arquivo, tornando-se o proprietário do arquivo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b5a4874edf9fa4406d4643e686fed2b725699dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854357"
---
# <a name="takeown"></a>takeown

Permite que um administrador recupere o acesso a um arquivo que foi negado anteriormente, transformando-o no proprietário do arquivo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<Computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O valor padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificadas no comando.|
|/u [\<Domain>\]<User name>|Executa o script com as permissões da conta de usuário especificada. O valor padrão é permissões do sistema.|
|/p [\<Password>]|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/f \<nome do arquivo >|Especifica o nome de arquivo ou o nome do diretório padrão. Você pode usar o caractere curinga * ao especificar o padrão. Você também pode usar a sintaxe *ShareName*\*nome de arquivo *.|
|/a|Fornece a propriedade para o grupo de administradores, em vez do usuário atual.|
|/r|Executa uma operação recursiva em todos os arquivos no diretório especificado e subdiretórios.|
|/d {Y \| N}|Suprime o prompt de confirmação é exibido quando o usuário atual não tem a permissão "Listar pasta" em um diretório especificado e, em vez disso, usa o valor padrão especificado. Os valores válidos para o **/d** opção são da seguinte maneira:</br>-   Y: Apropriar-se do diretório.</br>-   N: Ignore o diretório.</br>Observe que você deve usar essa opção em conjunto com o **/r** opção.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Normalmente, esse comando é usado em arquivos em lotes.
-   Se o **/a** parâmetro não for especificado, a propriedade do arquivo é fornecida para o usuário que está conectado no momento no computador.
-   Padrões mistos usando (**?** e **&#42;**) não são suportados pelo **takeown** comando.
-   Depois de excluir o bloqueio com **takeown**, talvez você precise usar o Windows Explorer ou o **cacls** comando a fim de obter permissões completas para os arquivos e diretórios antes de excluí-los. Para obter mais informações sobre **cacls**, consulte "Referências adicionais" no final deste tópico.

## <a name="BKMK_examples"></a>Exemplos

Para apropriar-se de um arquivo denominado Lostfile, digite:
```
takeown /f lostfile
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)