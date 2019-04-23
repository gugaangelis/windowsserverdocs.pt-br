---
title: sort
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 77116469-4790-4442-8a21-9fa73b65ef9f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d38beef74156d9d57b947883c542c2c7e971e00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854747"
---
# <a name="sort"></a>sort



Lê a entrada, classifica os dados e grava os resultados na tela, em um arquivo ou para outro dispositivo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/r|Inverte a ordem de classificação (ou seja, as classificações de Z a e de 9 a 0).|
|/+\<N>|Especifica o número de posição do caractere em que **classificação** começará a cada comparação. *N* pode ser qualquer inteiro válido.|
|/m \<Kilobytes>|Especifica a quantidade de memória principal a ser usada para a classificação em quilobytes (KB).|
|/l \<Locale>|Substitui a ordem de classificação de caracteres que são definidas pela localidade padrão do sistema (ou seja, o idioma e país/região selecionado durante a instalação).|
|/Rec \<caracteres >|Especifica o número máximo de caracteres em um registro ou uma linha do arquivo de entrada (o valor padrão é 4.096 e o máximo é de 65.535).|
|[\<Drive1>:][\<Path1>]\<FileName1>|Especifica o arquivo a ser classificado. Se nenhum nome de arquivo for especificado, a entrada padrão é classificada. Especificar o arquivo de entrada é mais rápido que redirecionar o mesmo arquivo de entrada padrão.|
|/t [\<Drive2>:][\<Path2>]|Especifica o caminho do diretório para armazenar o **classificação** comando de trabalho do armazenamento se os dados não couberem na memória principal. Por padrão, o diretório temporário do sistema é usado.|
|/o [\<Drive3>:][\<Path3>]\<FileName3>|Especifica o arquivo em que a entrada classificada deve ser armazenado. Se não for especificado, os dados são gravados na saída padrão. Especificando o arquivo de saída é mais rápido do que o redirecionamento de saída padrão para o mesmo arquivo.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando o **/+** opção de linha de comando

    Por padrão, as comparações são iniciadas no primeiro caractere de cada linha. O **/+** opção de linha de comando inicia comparações no caractere especificado por *N*. Por exemplo, `/+3` indica que cada comparação deve iniciar no terceiro caractere de cada linha. Linhas com menos de *N* caracteres de agrupamento antes de outras linhas.
-   Usando o **/m** opção de linha de comando

    A memória usada é sempre um mínimo de 160 KB. Se o tamanho da memória for especificado, a quantidade exata especificada é usada para a classificação (deve ser pelo menos 160 KB), independentemente da quantidade de memória principal está disponível.

    O tamanho de máximo de memória padrão quando nenhum tamanho for especificado é 90 por cento da memória principal disponível se a entrada e saída são arquivos ou 45% de memória principal, caso contrário. A configuração padrão oferece normalmente o melhor desempenho.
-   Usando o **/l** opção de linha de comando

    Atualmente, a única alternativa para a localidade padrão é a localidade "C", que é mais rápida do que a classificação do idioma nativo (classifica caracteres de acordo com a codificação binária).
-   Usando símbolos de redirecionamento com o **classificação** comando

    Você pode usar o símbolo de pipe (**|**) para direcionar dados de entrada para o **classificação** comando de outro comando ou para direcionar a saída classificada para outro comando. Você pode especificar arquivos de entrada e saídos usando símbolos de redirecionamento (**<** ou **>**). Ele pode ser mais rápido e mais eficiente (especialmente com arquivos grandes) especificar o arquivo de entrada diretamente (conforme definido pela *arquivo1* na sintaxe do comando) e, em seguida, especifique o arquivo de saída usando o **/o** parâmetro.
-   Diferenciação de maiusculas

    O **classificação** comando não faz distinção entre letras maiusculas e minúsculas.
-   Limites de tamanho do arquivo

    O **classificação** comando não tem limite de tamanho do arquivo.
-   Sequência de agrupamento

    O programa de classificação usa a tabela de sequência de agrupamento que corresponde às configurações de código e página de código de país/região. Caracteres maiores do que o código de ASCII 127 são classificados com base nas informações do arquivo Country ou em um arquivo alternativo especificado pela **país** comando em seu arquivo config.
-   Uso de memória

    Se a classificação couber dentro do tamanho máximo de memória (conforme definido por padrão, ou conforme especificado pelo **/m** parâmetro), a classificação é executada em uma única passagem. Caso contrário, a classificação é executada em duas passagens de classificação e mesclagem separadas, e as quantidades de memória usada para ambos os passos são iguais. Quando duas passagens são executadas, os dados parcialmente classificados são armazenados em um arquivo temporário no disco. Se não houver memória suficiente para executar a classificação em duas passagens, um erro de tempo de execução será emitido. Se o **/m** opção de linha de comando é usada para especificar mais memória do que realmente disponível, degradação de desempenho ou um erro de tempo de execução pode ocorrer.

## <a name="BKMK_examples"></a>Exemplos

**Classificar um arquivo**

Para classificar e exibir as linhas na ordem inversa em um arquivo chamado despesas. txt, digite:

`sort /r expenses.txt`

**Classificar a saída de um comando**

Para pesquisar um arquivo grande denominado Maladir txt para o texto "Jones" e para classificar os resultados da pesquisa, use a barra vertical (|) para direcionar a saída de um **encontrar** comando para o **classificação** comando, da seguinte maneira:

`find "Jones" maillist.txt | sort`

O comando produz uma lista classificada de linhas que contêm o texto especificado.

**Classificar entrada de teclado**

Para classificar a entrada do teclado e exibir os resultados em ordem alfabética na tela, primeiro você poderá usar o **classificação** comando sem parâmetros, da seguinte maneira:

`sort`

Em seguida, digite o texto que você deseja que sejam classificados e pressione ENTER no final de cada linha. Quando terminar de digitar o texto, pressione CTRL + Z e, em seguida, pressione ENTER. O **classificação** comando exibe o texto digitado, classificado em ordem alfabética.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)