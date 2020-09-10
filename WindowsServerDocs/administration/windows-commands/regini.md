---
title: regini
description: Artigo de referência para o comando Regini, que modifica o registro da linha de comando ou de um script e aplica as alterações que foram predefinidas em um ou mais arquivos de texto.
ms.topic: reference
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 78c56a68392d066047123dc77bafc3d1b01de127
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627471"
---
# <a name="regini"></a>regini

Modifica o registro da linha de comando ou de um script e aplica as alterações que foram predefinidas em um ou mais arquivos de texto. Você pode criar, modificar ou excluir chaves do registro, além de modificar as permissões nas chaves do registro.

Para obter detalhes sobre o formato e o conteúdo do arquivo de script de texto que regini.exe usa para fazer alterações no registro, consulte [como alterar valores ou permissões de registro de uma linha de comando ou de um script](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Sintaxe

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputwidth][-b] textfiles...
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -m `<\\computername>` | Especifica o nome do computador remoto com um registro que deve ser modificado. Use o formato ** \\ ComputerName**. |
| -h `<hivefile hiveroot>` | Especifica o hive do Registro local a ser modificado. Você deve especificar o nome do arquivo do hive e a raiz do hive no formato **hivefile hiveroot**. |
| -i `<n>` | Especifica o nível de recuo a ser usado para indicar a estrutura de árvore das chaves do registro na saída do comando. A ferramenta de **regdmp.exe** (que obtém as permissões atuais de uma chave do registro no formato binário) usa o recuo em múltiplos de quatro, portanto, o valor padrão é **4**. |
| -o `<outputwidth>` | Especifica a largura da saída do comando, em caracteres. Se a saída for exibida na janela de comando, o valor padrão será a largura da janela. Se a saída for direcionada para um arquivo, o valor padrão será de **240** caracteres. |
| -b | Especifica que **regini.exe** saída é compatível com versões anteriores do **regini.exe**. |
| textfiles | Especifica o nome de um ou mais arquivos de texto que contêm dados de registro. Qualquer número de arquivos de texto ANSI ou Unicode pode ser listado. |

#### <a name="remarks"></a>Comentários

As diretrizes a seguir se aplicam principalmente ao conteúdo dos arquivos de texto que contêm dados de registro que você aplica usando **regini.exe**.

- Use o ponto e vírgula como um caractere de comentário de fim de linha. Ele deve ser o primeiro caractere que não seja em branco em uma linha.

- Use a barra invertida para indicar a continuação de uma linha. O comando irá ignorar todos os caracteres da barra invertida até (mas não incluindo) o primeiro caractere não em branco da linha seguinte. Se você incluir mais de um espaço antes da barra invertida, ele será substituído por um único espaço.

- Use caracteres de guia rígido para controlar o recuo. Esse recuo indica a estrutura de árvore das chaves do registro; no entanto, esses caracteres são convertidos em um único espaço, independentemente de sua posição.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
