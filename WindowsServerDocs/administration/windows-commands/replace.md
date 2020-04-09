---
title: replace
description: Saiba como usar o comando Replace para substituir arquivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d44e4f8383a77582177f4d9b161210207ce46e63
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835899"
---
# <a name="replace"></a>replace



Substitui arquivos. Se usado com a opção **/a** , **replace** adiciona novos arquivos a um diretório em vez de substituir os arquivos existentes.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<unidade1 >:] [\<caminho1 >]\<nome do arquivo >|Especifica o local e o nome do arquivo de origem ou conjunto de arquivos. O *nome de arquivo* é necessário e pode incluir caracteres **&#42;** curinga (e **?** ).|
|[\<unidade2 >:] [\<Caminho2 >]|Especifica o local do arquivo de destino. Não é possível especificar um nome de arquivo para os arquivos que você substituir. Se você não especificar uma unidade ou um caminho, **replace** usará a unidade e o diretório atuais como destino.|
|/a|Adiciona novos arquivos ao diretório de destino em vez de substituir os arquivos existentes. Você não pode usar essa opção de linha de comando com a opção de linha de comando **/s** ou **/u** .|
|/p|Solicita a confirmação antes de substituir um arquivo de destino ou adicionar um arquivo de origem.|
|/r|Substitui arquivos somente leitura e não protegidos. Se você tentar substituir um arquivo somente leitura, mas não especificar **/r**, um erro resultará e interromperá a operação de substituição.|
|/w|Aguarda a inserção de um disco antes que a pesquisa de arquivos de origem comece. Se você não especificar **/w**, **replace** começará a substituir ou a adicionar arquivos imediatamente depois que você pressionar Enter.|
|/s|Pesquisa todos os subdiretórios no diretório de destino e substitui os arquivos correspondentes. Você não pode usar **/s** com a opção de linha de comando **/a** . O comando **replace** não pesquisa subdiretórios que são especificados em *caminho1*.|
|/u|Substitui somente os arquivos no diretório de destino que são mais antigos do que aqueles no diretório de origem. Você não pode usar **/u** com a opção de linha de comando **/a** .|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Como **replace** adiciona ou substitui arquivos, os nomes de arquivo são exibidos na tela. Após a conclusão da **substituição** , uma linha de resumo é exibida em um dos seguintes formatos:  
  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```  
- Se você estiver usando disquetes e precisar alternar discos durante a operação de **substituição** , você poderá especificar a opção de linha de comando **/w** para que **replace** espere que você alterne os discos.
- Você não pode usar **replace** para atualizar arquivos ocultos ou arquivos do sistema.
- A tabela a seguir mostra cada código de saída e uma breve descrição de seu significado:  
  |Código de Saída|Descrição|
  |---------|-----------|
  |0|O comando **replace** substituiu ou adicionou os arquivos com êxito.|
  |1|O comando **replace** encontrou uma versão incorreta do MS-dos.|
  |2|O comando **replace** não pôde localizar os arquivos de origem.|
  |3|O comando **replace** não pôde localizar o caminho de origem ou de destino.|
  |5|O usuário não tem acesso aos arquivos que você deseja substituir.|
  |8|Memória do sistema insuficiente para executar o comando.|
  |11|O usuário usou a sintaxe incorreta na linha de comando.|

> [!NOTE]
> Você pode usar o parâmetro ERRORLEVEL na linha de comando **If** em um programa em lotes para processar códigos de saída que são retornados por **replace**.

## <a name="examples"></a><a name="BKMK_examples"></a>Disso

Para atualizar todas as versões de um arquivo chamado phones. CLI (que aparece em vários diretórios na unidade C), com a versão mais recente do arquivo phones. CLI de um disquete na unidade A, digite:

`replace a:\phones.cli c:\ /s`

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)