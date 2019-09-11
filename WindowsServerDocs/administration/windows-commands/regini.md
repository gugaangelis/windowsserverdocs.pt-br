---
title: regini
description: Saiba como modificar o registro no prompt de comando ou usando um script.
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
ms.openlocfilehash: 80f8f4212d2054fc54ce33993a1cef8a1501c6d5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868875"
---
# <a name="regini"></a>regini

Modifica o registro da linha de comando ou de um script e aplica as alterações que foram predefinidas em um ou mais arquivos de texto. Você pode criar, modificar ou excluir chaves do registro, além de modificar as permissões nas chaves do registro.

Para obter detalhes sobre o formato e o conteúdo do arquivo de script de texto que o Regini. exe usa para fazer alterações no registro, consulte [como alterar valores de registro ou permissões de uma linha de comando ou um script](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Sintaxe

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |

|-m \< \\ ComputerName\\>|Especifica o nome do computador remoto com um registro que deve ser modificado. Use o  **\\ formato\\ComputerName**.|
|---------------------|-|
|-h \<hivefile hiveroot >|Especifica o hive do Registro local a ser modificado. Você deve especificar o nome do arquivo do hive e a raiz do hive no formato **hivefile hiveroot**.|
|-i \<n >|Especifica o nível de recuo a ser usado para indicar a estrutura de árvore das chaves do registro na saída do comando. A ferramenta **Regdmp. exe** (que obtém as permissões atuais de uma chave do registro no formato binário) usa o recuo em múltiplos de quatro, portanto, o valor padrão é **4**.|
|-o \<outputwidth >|Especifica a largura da saída do comando, em caracteres. Se a saída for exibida na janela de comando, o valor padrão será a largura da janela. Se a saída for direcionada para um arquivo, o valor padrão será de **240** caracteres.|
|-b|Especifica que a saída de **Regini. exe** é compatível com versões anteriores do **Regini. exe**. Consulte a seção comentários para obter detalhes.|
|textfiles|Especifica o nome de um ou mais arquivos de texto que contêm dados de registro. Qualquer número de arquivos de texto ANSI ou Unicode pode ser listado.|

## <a name="remarks"></a>Comentários

As diretrizes a seguir se aplicam principalmente ao conteúdo dos arquivos de texto que contêm dados de registro que você aplica usando **Regini. exe**.
-   Use o ponto e vírgula como um caractere de comentário de fim de linha. Ele deve ser o primeiro caractere que não seja em branco em uma linha.
-   Use a barra invertida para indicar a continuação de uma linha. O comando irá ignorar todos os caracteres da barra invertida até (mas não incluindo) o primeiro caractere não em branco da linha seguinte. Se você incluir mais de um espaço antes da barra invertida, ele será substituído por um único espaço.
-   Use caracteres de guia rígido para controlar o recuo. Esse recuo indica a estrutura de árvore das chaves do registro; no entanto, esses caracteres são convertidos em um único espaço, independentemente de sua posição.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)