---
title: regini
description: Saiba como modificar o registro do prompt de comando ou usando um script.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 292dfe5755a10a91f2b8bcffaa6412ccda6c6f8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867627"
---
# <a name="regini"></a>regini

Modifica o registro da linha de comando ou um script e aplica as alterações que foram predefinidas em um ou mais arquivos de texto. Você pode criar, modificar ou excluir chaves do registro, além de modificar as permissões nas chaves do registro.

Para obter detalhes sobre o formato e conteúdo do arquivo de script de texto Regini.exe usa para fazer alterações no registro, consulte [como alterar permissões ou valores de registro de uma linha de comando ou um script](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Sintaxe

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|-m \< \\ \\ComputerName >|Especifica o nome do computador remoto com um registro que deve ser modificado. Use o formato  **\\ \\ComputerName**.|
|---------------------|-|
|-h \<ArquivoDe Sessão hiveroot >|Especifica o hive do Registro local para modificar. Você deve especificar o nome do arquivo de hive e a raiz do hive no formato **hiveroot arquivode sessão**.|
|-i \<n>|Especifica o nível de recuo para usar para indicar a estrutura de árvore das chaves do registro na saída do comando. O **Regdmp.exe** ferramenta (que obtém um registro de permissões atual da chave em formato binário) usa o recuo em múltiplos de quatro, portanto, o valor padrão é **4**.|
|-o \<outputwidth>|Especifica a largura da saída do comando, em caracteres. Se a saída será exibida na janela de comando, o valor padrão é a largura da janela. Se a saída é direcionada para um arquivo, o valor padrão é **240** caracteres.|
|-b|Especifica que **Regini.exe** saída é compatível com versões anteriores com versões anteriores do **Regini.exe**. Consulte a seção de comentários para obter detalhes.|
|textfiles|Especifica o nome de um ou mais arquivos de texto que contêm dados do registro. Qualquer número de arquivos de texto ANSI ou Unicode pode ser listado.|

## <a name="remarks"></a>Comentários

As diretrizes a seguir se aplicam principalmente para o conteúdo dos arquivos de texto que contêm dados de registro que você aplicar usando **Regini.exe**.
-   Use o ponto e vírgula como um caractere de comentário de final de linha. Ele deve ser o primeiro caractere não em branco em uma linha.
-   Use a barra invertida para indicar a continuação de uma linha. O comando irá ignorar todos os caracteres de barra invertida até (mas não incluir) o primeiro caractere não em branco da próxima linha. Se você incluir mais de um espaço antes da barra invertida, ele será substituído por um único espaço.
-   Use caracteres de disco rígido-guia para controlar o recuo. Este recuo indica a estrutura de árvore das chaves do registro; No entanto, esses caracteres são convertidos em um único espaço, independentemente de sua posição.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)