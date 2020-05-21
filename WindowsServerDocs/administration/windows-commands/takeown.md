---
title: takeown
description: Saiba como obter acesso a um arquivo, tornando-se o proprietário do arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da43b13f0333f3a12a8763db4cad31e12283bb8b
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436191"
---
# <a name="takeown"></a>takeown

Permite que um administrador recupere o acesso a um arquivo que foi negado anteriormente, transformando-o no proprietário do arquivo.



## <a name="syntax"></a>Sintaxe

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<> do computador|Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O valor padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando.|
|/u [ \< domínio>\]<User name>|Executa o script com as permissões da conta de usuário especificada. O valor padrão é permissões do sistema.|
|/p [ \< senha>]|Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .|
|/f \< nome do arquivo>|Especifica o nome do arquivo ou padrão de nome de diretório. Você pode usar o caractere curinga * ao especificar o padrão. Você também pode usar a sintaxe *ShareName* \* filename *.|
|/a|Concede a propriedade ao grupo de administradores em vez do usuário atual.|
|/r|Executa uma operação recursiva em todos os arquivos no diretório e nos subdiretórios especificados.|
|/d {Y \| N}|Suprime o prompt de confirmação exibido quando o usuário atual não tem a permissão "Listar pasta" em um diretório especificado e, em vez disso, usa o valor padrão especificado. Os valores válidos para a opção **/d** são os seguintes:</br>-Y: apropriar-se do diretório.</br>-N: ignorar o diretório.</br>Observe que você deve usar essa opção em conjunto com a opção **/r** .|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Esse comando é normalmente usado em arquivos em lotes.
-   Se o parâmetro **/a** não for especificado, a propriedade do arquivo será dada ao usuário que está conectado ao computador no momento.
-   Padrões mistos usando (**?** e **&#42;**) não são suportados pelo comando **takeown** .
-   Depois de excluir o bloqueio com **takeown**, talvez seja necessário usar o Windows Explorer ou o comando **cacls** para conceder permissões completas aos arquivos e diretórios antes de poder excluí-los. Para obter mais informações sobre **cacls**, consulte "referências adicionais" no final deste tópico.

## <a name="examples"></a><a name="BKMK_examples"></a>Exemplos

Para apropriar-se de um arquivo chamado renomeado, digite:
```
takeown /f lostfile
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)