---
title: memória do WinSAT
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cdae81694a916905f36cdd9e941015e3ce5f15c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440087"
---
# <a name="winsat-mem"></a>memória do WinSAT



Copia de largura de banda de memória de sistema a testes de forma refletiva do buffer de memória para memória grande, pois são usados no processamento de multimídia.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
winsat mem <parameters>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-up|Forçar o teste com apenas um segmento de memória. O padrão é executar um thread por físico da CPU ou núcleos.|
|-rn|Especifique que os threads da avaliação devem ser executado com prioridade normal. O padrão é executado com prioridade 15.|
|-nc|Especifica que a avaliação deve alocar memória e marcá-lo como não armazenada em cache. Isso significa que os caches do processador serão ignorados para operações de cópia. O padrão é executar no espaço em cache.|
|-do \<n>|Especifique a distância, em bytes, entre o final do buffer de origem e o início do buffer de destino. O padrão é 64 bytes. O deslocamento de destino permitido máximo é de 16MB. Especificar um deslocamento de destino inválida resultará em erro.</br>Observação: Zero é um valor válido para  **\<n >** , mas os números negativos não são.|
|-mint \<n>|Especifica o mínimo de tempo de execução em segundos para a avaliação. O padrão é 2.0. O valor mínimo é 1.0. O valor máximo é 30,0.</br>Observação: Especificando um **-mint** valor maior que o **- maxt** valor quando os dois parâmetros são usados em combinação resultará em um erro.|
|-maxt \<n>|Especifique o máximo de tempo de execução em segundos para a avaliação. O padrão é 5.0. O valor mínimo é 1.0. O valor máximo é 30,0. Se usado em combinação com o **-mint** parâmetro, a avaliação começará a fazer verificações periódicas de estatísticas de seus resultados após o período de tempo especificado na **-mint**. Se as verificações de estatísticas de passe, então a avaliação será concluído antes do período de tempo especificado na **maxt -** tiver decorrido. Se a avaliação é executada para o período de tempo especificado na **maxt -** sem que satisfazem as verificações de estatísticas e, em seguida, a avaliação será concluir naquele momento e retornar os resultados coletados.|
|-buffersize \<n >|Especifique o tamanho do buffer que o teste de cópia de memória deve usar. Duas vezes essa quantidade será alocada por CPU, que determina a quantidade de dados copiados de um buffer para outro. O padrão é 16MB. Esse valor é arredondado para o limite mais próximo de 4 KB. O valor máximo é 32MB. O valor mínimo é de 4 KB. Especificar um tamanho de buffer inválida resultará em erro.|
|-v|Envie a saída detalhada para STDOUT, incluindo informações de status e o progresso. Os erros também serão gravados na janela de comando.|
|-xml \<file name>|Salve a saída da avaliação como o arquivo XML especificado. Se o arquivo especificado existir, ele será substituído.|
|-idiskinfo|Salvar informações sobre volumes físicos e os discos lógicos como parte do  **\<caixa >** seção na saída XML.|
|-iguid|Crie um identificador global exclusivo (GUID) no arquivo de saída XML.|
|-Observe "texto de Observação"|Adicione o texto de anotação para o  **\<Observação >** seção no arquivo de saída XML.|
|-icn|Inclua o nome do computador local no arquivo de saída XML.|
|-eef|Enumere as informações de sistema adicionais no arquivo de saída XML.|

## <a name="BKMK_examples"></a>Exemplos

- O exemplo a seguir executa a avaliação por um mínimo de 4 segundos e não mais do que 12 segundos, usando um tamanho de buffer de 32MB e salvar os resultados em formato XML no arquivo **memtest.xml**.  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>Comentários

-   A associação no grupo Administradores local, ou equivalente, é o mínimo necessário para usar **winsat**. O comando deve ser executado em uma janela de prompt de comando com privilégios elevados.
-   Para abrir uma janela de prompt de comando com privilégios elevados, clique em **inicie**, clique em **Acessórios**, clique com botão direito **Prompt de comando**e clique em **executar como administrador**.

#### <a name="additional-references"></a>Referências adicionais

