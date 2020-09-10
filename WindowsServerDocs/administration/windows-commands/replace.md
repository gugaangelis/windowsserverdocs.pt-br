---
title: substituir
description: Artigo de referência para o comando Replace, que pode substituir os novos arquivos existentes ou adicionados a um diretório.
ms.topic: reference
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 44ece657b87b61bc9be6333644d05b8201061014
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626980"
---
# <a name="replace"></a>substituir

Substituir os arquivos existentes em um diretório. Se usado com a opção **/a** , esse comando adiciona novos arquivos a um diretório em vez de substituir os arquivos existentes.

## <a name="syntax"></a>Sintaxe

```
replace [<drive1>:][<path1>]<filename> [<drive2>:][<path2>] [/a] [/p] [/r] [/w]
replace [<drive1>:][<path1>]<filename> [<drive2>:][<path2>] [/p] [/r] [/s] [/w] [/u]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `[<drive1>:][<path1>]<filename>` | Especifica o local e o nome do arquivo de origem ou conjunto de arquivos. A opção *filename* é necessária e pode incluir caracteres curinga (**&#42;** e **?**). |
| `[<drive2>:][<path2>]` | Especifica o local do arquivo de destino. Não é possível especificar um nome de arquivo para os arquivos que você substituir. Se você não especificar uma unidade ou um caminho, esse comando usará a unidade e o diretório atuais como destino. |
| /a | Adiciona novos arquivos ao diretório de destino em vez de substituir os arquivos existentes. Você não pode usar essa opção de linha de comando com a opção de linha de comando **/s** ou **/u** . |
| /p | Solicita a confirmação antes de substituir um arquivo de destino ou adicionar um arquivo de origem. |
| /r | Substitui arquivos somente leitura e não protegidos. Se você tentar substituir um arquivo somente leitura, mas não especificar **/r**, um erro resultará e interromperá a operação de substituição. |
| /w | Aguarda a inserção de um disco antes que a pesquisa de arquivos de origem comece. Se você não especificar **/w**, esse comando começará a substituir ou a adicionar arquivos imediatamente depois que você pressionar Enter. |
| /s | Pesquisa todos os subdiretórios no diretório de destino e substitui os arquivos correspondentes. Você não pode usar **/s** com a opção de linha de comando **/a** . O comando não pesquisa subdiretórios que são especificados em *caminho1*. |
| /u | Substitui somente os arquivos no diretório de destino que são mais antigos do que aqueles no diretório de origem. Você não pode usar **/u** com a opção de linha de comando **/a** . |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Como esse comando adiciona ou substitui arquivos, os nomes de arquivo aparecem na tela. Depois que esse comando é feito, uma linha de resumo é exibida em um dos seguintes formatos:

  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```

- Se você estiver usando disquetes e precisar alternar discos ao executar esse comando, poderá especificar a opção de linha de comando **/w** para que esse comando espere que você alterne os discos.

- Você não pode usar esse comando para atualizar arquivos ocultos ou arquivos do sistema.

- A tabela a seguir mostra cada código de saída e uma breve descrição de seu significado:

  | Código de saída | Descrição |
  |--|--|
  | 0 | Esse comando substituiu ou adicionou os arquivos com êxito. |
  | 1 | Este comando encontrou uma versão incorreta do MS-DOS. |
  | 2 | Este comando não pôde localizar os arquivos de origem. |
  | 3 | Este comando não pôde localizar o caminho de origem ou de destino. |
  | 5 | O usuário não tem acesso aos arquivos que você deseja substituir. |
  | 8 | Memória do sistema insuficiente para executar o comando. |
  | 11 | O usuário usou a sintaxe incorreta na linha de comando. |

> [!NOTE]
> Você pode usar o parâmetro ERRORLEVEL na linha de comando **If** em um programa em lotes para processar os códigos de saída retornados por esse comando.

## <a name="examples"></a>Exemplos

Para atualizar todas as versões de um arquivo chamado *phones. CLI* (que aparecem em vários diretórios na unidade C:), com a versão mais recente do arquivo *phones. CLI* de um disquete na unidade a:, digite:

```
replace a:\phones.cli c:\ /s
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
