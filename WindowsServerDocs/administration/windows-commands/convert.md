---
title: convert
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9fcb7b2190c3359b78145a2ef79a28e30639a89c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873157"
---
# <a name="convert"></a>convert



Converte arquivos (FAT) de tabela de alocação e volumes FAT32 ao sistema de arquivos NTFS, deixando intactos os diretórios e arquivos existentes. Volumes convertidos para o sistema de arquivos NTFS não podem ser convertidos para FAT ou FAT32.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Volume>|Especifica a letra da unidade (seguida por dois-pontos), ponto de montagem ou nome do volume para converter em NTFS.|
|/fs:ntfs|Obrigatório. Converte o volume NTFS.|
|/v|Execuções **converter** em modo detalhado, que exibe todas as mensagens durante o processo de conversão.|
|/cvtarea:\<FileName>|Especifica que o arquivo de MFT (tabela mestra) e outros arquivos de metadados NTFS são gravados em um arquivo de espaço reservado existente e contíguos. Esse arquivo deve estar no diretório raiz do sistema de arquivos a ser convertido. Usar o **/cvtarea** parâmetro pode resultar em um sistema de arquivos menos fragmentado após a conversão. Para obter melhores resultados, o tamanho desse arquivo deve ser 1 KB multiplicado pelo número de arquivos e diretórios no sistema de arquivos, embora o **converter** utilitário aceita arquivos de qualquer tamanho.</br>Importante: Você deve criar o arquivo de espaço reservado usando o **fsutil arquivo createnew** comando antes da execução **converter**. **Converter** não cria esse arquivo para você. **Converter** substituirá esse arquivo com metadados do NTFS. Após a conversão, qualquer espaço não utilizado nesse arquivo é liberado.|
|/NoSecurity|Especifica que as configurações de segurança nos arquivos convertidos e diretórios permitem o acesso por todos os usuários.|
|/x|Desmonta o volume, se necessário, antes de ser convertido. Os identificadores abertos para o volume deixarão de ser válidos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se **converter** não é possível bloquear a unidade (por exemplo, a unidade é o volume do sistema ou a unidade atual), você terá a opção de converter a unidade na próxima vez que você reiniciar o computador. Se você não puder reiniciar o computador imediatamente para concluir a conversão, planeje um tempo para reiniciar o computador e permitir que o tempo extra para concluir o processo de conversão.
-   Para volumes convertido FAT ou FAT32 em NTFS:

    Devido ao uso de disco existente, o MFT é criado em um local diferente do que em um volume originalmente formatado com NTFS, portanto, desempenho do volume pode não ser tão bom quanto em volumes originalmente formatados com NTFS. Para um desempenho ideal, considere recriar esses volumes e formatá-los com o sistema de arquivos NTFS.

    Conversão de volume do FAT ou FAT32 em NTFS deixa os arquivos intactos, mas o volume pode não ter alguns benefícios de desempenho em comparação comparados volumes inicialmente formatados com NTFS. Por exemplo, o MFT pode se tornar fragmentado em volumes convertidos. Além disso, nos volumes de inicialização convertidos **converter** aplica-se a mesma segurança padrão que é aplicada durante a instalação do Windows.

## <a name="BKMK_examples"></a>Exemplos

Para converter o volume na unidade E para NTFS e exibir todas as mensagens durante o processo de conversão, digite:
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)