---
title: convert
description: O tópico de comandos do Windows para converter, que converte FAT (tabela de alocação de arquivos) e volumes FAT32 no sistema de arquivos NTFS, deixando os arquivos e diretórios existentes intactos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fb2981d6cd5a54737700b64b28f7a8a52de72b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847169"
---
# <a name="convert"></a>convert

Converte os volumes FAT (tabela de alocação de arquivo) e FAT32 no sistema de arquivos NTFS, deixando os arquivos e diretórios existentes intactos. Os volumes convertidos no sistema de arquivos NTFS não podem ser convertidos de volta para FAT ou FAT32.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<volume >|Especifica a letra da unidade (seguida por dois-pontos), o ponto de montagem ou o nome do volume a ser convertido em NTFS.|
|/FS: NTFS|Obrigatório. Converte o volume em NTFS.|
|/v|Executa a **conversão** em modo detalhado, que exibe todas as mensagens durante o processo de conversão.|
|/CvtArea: nome de > do\<|Especifica que a tabela de arquivos mestre (MFT) e outros arquivos de metadados NTFS são gravados em um arquivo de espaço reservado contíguo existente. Esse arquivo deve estar no diretório raiz do sistema de arquivos a ser convertido. O uso do parâmetro **/Cvtarea** pode resultar em um sistema de arquivos menos fragmentado após a conversão. Para obter melhores resultados, o tamanho desse arquivo deve ser 1 KB multiplicado pelo número de arquivos e diretórios no sistema de arquivos, embora o utilitário de **conversão** aceite arquivos de qualquer tamanho.</br>Importante: você deve criar o arquivo de espaço reservado usando o comando **fsutil file createnew** antes de executar **Convert**. **Converter** não cria esse arquivo para você. **Convert** substitui esse arquivo por metadados NTFS. Após a conversão, qualquer espaço não utilizado nesse arquivo será liberado.|
|/nosecurity|Especifica que as configurações de segurança nos arquivos e diretórios convertidos permitem o acesso por todos os usuários.|
|/x|Desmonta o volume, se necessário, antes de ser convertido. Os identificadores abertos para o volume deixarão de ser válidos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se **converter** não puder bloquear a unidade (por exemplo, a unidade é o volume do sistema ou a unidade atual), você terá a opção de converter a unidade na próxima vez que reiniciar o computador. Se você não puder reiniciar o computador imediatamente para concluir a conversão, planeje um tempo para reiniciar o computador e permita o tempo extra para que o processo de conversão seja concluído.
-   Para volumes convertidos de FAT ou FAT32 para NTFS:

    Devido ao uso do disco existente, o MFT é criado em um local diferente do que em um volume originalmente formatado com NTFS, portanto, o desempenho do volume pode não ser tão bom quanto em volumes originalmente formatados com NTFS. Para obter um desempenho ideal, considere recriar esses volumes e formatá-los com o sistema de arquivos NTFS.

    A conversão de volume de FAT ou FAT32 para NTFS deixa os arquivos intactos, mas o volume pode não ter alguns benefícios de desempenho em comparação com os volumes inicialmente formatados com NTFS. Por exemplo, o MFT pode ficar fragmentado em volumes convertidos. Além disso, nos volumes de inicialização convertidos, **Convert** aplica a mesma segurança padrão que é aplicada durante a instalação do Windows.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para converter o volume na unidade E em NTFS e exibir todas as mensagens durante o processo de conversão, digite:
```
convert e: /fs:ntfs /v
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)