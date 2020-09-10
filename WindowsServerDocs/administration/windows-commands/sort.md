---
title: sort
description: Artigo de referência para classificação, que lê entrada, classifica dados e grava os resultados na tela, em um arquivo ou em outro dispositivo.
ms.topic: reference
ms.assetid: 77116469-4790-4442-8a21-9fa73b65ef9f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c0809f6a44ee25507f944ce2882305c44215b8f1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640916"
---
# <a name="sort"></a>sort

Lê a entrada, classifica os dados e grava os resultados na tela, em um arquivo ou em outro dispositivo.



## <a name="syntax"></a>Sintaxe

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/r|Reverte a ordem de classificação (ou seja, classifica de Z a A e de 9 a 0).|
|/+\<N>|Especifica o número da posição do caractere em que a **classificação** começará cada comparação. *N* pode ser qualquer número inteiro válido.|
|opção \<Kilobytes>|Especifica a quantidade de memória principal a ser usada para a classificação em kilobytes (KB).|
|/l \<Locale>|Substitui a ordem de classificação dos caracteres que são definidos pela localidade padrão do sistema (ou seja, o idioma e o país/região selecionados durante a instalação).|
|/rec \<Characters>|Especifica o número máximo de caracteres em um registro ou uma linha do arquivo de entrada (o valor padrão é 4.096 e o máximo é 65.535).|
|[\<Drive1>:][\<Path1>]\<FileName1>|Especifica o arquivo a ser classificado. Se nenhum nome de arquivo for especificado, a entrada padrão será classificada. Especificar o arquivo de entrada é mais rápido do que redirecionar o mesmo arquivo como entrada padrão.|
|/t [ \<Drive2> :] [ \<Path2> ]|Especifica o caminho do diretório para manter o armazenamento de trabalho do comando de **classificação** se os dados não couberem na memória principal. Por padrão, o diretório temporário do sistema é usado.|
|/o [ \<Drive3> :] [ \<Path3> ]\<FileName3>|Especifica o arquivo onde a entrada classificada deve ser armazenada. Se não for especificado, os dados serão gravados na saída padrão. Especificar o arquivo de saída é mais rápido do que redirecionar a saída padrão para o mesmo arquivo.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando a **/+** opção de linha de comando

    Por padrão, as comparações começam no primeiro caractere de cada linha. A **/+** opção de linha de comando inicia as comparações no caractere especificado por *N*. Por exemplo, `/+3` indica que cada comparação deve começar no terceiro caractere de cada linha. Linhas com menos de *N* caracteres se agrupam antes de outras linhas.
-   Usando a opção de linha de comando **/m**

    A memória usada é sempre um mínimo de 160 KB. Se o tamanho da memória for especificado, o valor especificado exato será usado para a classificação (deve ser pelo menos 160 KB), independentemente da quantidade de memória principal disponível.

    O tamanho máximo de memória padrão quando nenhum tamanho é especificado é de 90% da memória principal disponível se a entrada e a saída forem arquivos ou 45% da memória principal, caso contrário. A configuração padrão geralmente oferece o melhor desempenho.
-   Usando a opção de linha de comando **/l**

    Atualmente, a única alternativa para a localidade padrão é a localidade C, que é mais rápida do que a classificação de idioma natural (ela classifica caracteres de acordo com suas codificações binárias).
-   Usando símbolos de redirecionamento com o comando **Sort**

    Você pode usar o símbolo de pipe ( **|** ) para direcionar dados de entrada para o comando de **classificação** de outro comando ou para direcionar a saída classificada para outro comando. Você pode especificar os arquivos de entrada e saída usando símbolos de redirecionamento ( **<** ou **>** ). Ele pode ser mais rápido e mais eficiente (especialmente com arquivos grandes) para especificar o arquivo de entrada diretamente (conforme definido por *arquivo1* na sintaxe do comando) e, em seguida, especificar o arquivo de saída usando o parâmetro **/o** .
-   Diferenciar maiúsculas de minúsculas

    O comando **Sort** não faz distinção entre letras maiúsculas e minúsculas.
-   Limites no tamanho do arquivo

    O comando **Sort** não tem limite no tamanho do arquivo.
-   Sequência de agrupamento

    O programa de classificação usa a tabela de sequência de agrupamento que corresponde ao código do país/região e às configurações da página de código. Caracteres maiores que o código ASCII 127 são classificados com base nas informações do arquivo de Country.sys ou em um arquivo alternativo especificado pelo comando **Country** no seu arquivo config. NT.
-   Uso de memória

    Se a classificação couber no tamanho máximo da memória (conforme definido por padrão ou conforme especificado pelo parâmetro **/m** ), a classificação será executada em uma única passagem. Caso contrário, a classificação é executada em duas passagens de classificação e mesclagem separadas, e as quantidades de memória usadas para ambas as passagens são iguais. Quando duas passagens são executadas, os dados parcialmente classificados são armazenados em um arquivo temporário no disco. Se não houver memória suficiente para executar a classificação em duas passagens, um erro de tempo de execução será emitido. Se a opção de linha de comando **/m** for usada para especificar mais memória do que está verdadeiramente disponível, a degradação do desempenho ou um erro em tempo de execução poderá ocorrer.

## <a name="examples"></a>Exemplos

**Classificando um arquivo**

Para classificar e exibir em ordem inversa, as linhas em um arquivo chamado Expenses.txt, digite:

`sort /r expenses.txt`

**Classificando a saída de um comando**

Para pesquisar um arquivo grande chamado Maillist.txt para o texto Jones e classificar os resultados da pesquisa, use o pipe (|) para direcionar a saída de um comando **Find** para o comando **Sort** , da seguinte maneira:

`find Jones maillist.txt | sort`

O comando produz uma lista classificada de linhas que contêm o texto especificado.

**Classificando entrada de teclado**

Para classificar a entrada do teclado e exibir os resultados em ordem alfabética na tela, você pode primeiro usar o comando **Sort** sem parâmetros, da seguinte maneira:

`sort`

Em seguida, digite o texto que você deseja classificar e pressione ENTER no final de cada linha. Quando terminar de digitar o texto, pressione CTRL + Z e pressione ENTER. O comando **classificar** exibe o texto digitado, classificado alfabeticamente.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)