---
title: WinSAT mem
description: Tópico de referência para WinSAT Mem, que testa a largura de banda de memória do sistema de uma maneira que reflete a memória grande para cópias de buffer de memória, como são usadas no processamento de multimídia.
ms.prod: windows-server
ms.technology: manage-windows-commands
winms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 158757d4449b288469b52d2d62e7cfb53d57a7d2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720667"
---
# <a name="winsat-mem"></a>WinSAT mem



Testa a largura de banda da memória do sistema de maneira refletida de memória grande para cópias de buffer de memória, como são usadas no processamento de multimídia.



## <a name="syntax"></a>Sintaxe

```
winsat mem <parameters>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-up|Forçar o teste de memória com apenas um thread. O padrão é executar um thread por CPU física ou núcleo.|
|-RN|Especifique que os threads da avaliação devem ser executados em prioridade normal. O padrão é executar na prioridade 15.|
|-NC|Especifique que a avaliação deve alocar memória e sinalizá-la como não armazenado em cache. Isso significa que os caches do processador serão ignorados para operações de cópia. O padrão é executar no espaço em cache.|
|-Faça \<n>|Especifique a distância, em bytes, entre o fim do buffer de origem e o início do buffer de destino. O padrão é 64 bytes. O deslocamento máximo permitido de destino é de 16 MB. A especificação de um deslocamento de destino inválido resultará em um erro.</br>Observação: zero é um valor válido para ** \<n>**, mas números negativos não são.|
|-menta \<n>|Especifique o tempo mínimo de execução em segundos para a avaliação. O padrão é 2,0. O valor mínimo é 1,0. O valor máximo é 30,0.</br>Observação: a especificação de um valor **-menta** maior que o valor **-maxT** quando os dois parâmetros são usados em combinação resultará em um erro.|
|-maxT \<n>|Especifique o tempo de execução máximo em segundos para a avaliação. O padrão é 5,0. O valor mínimo é 1,0. O valor máximo é 30,0. Se usado em combinação com o parâmetro **-menta** , a avaliação começará a fazer verificações estatísticas periódicas de seus resultados após o período de tempo especificado em **-menta**. Se as verificações estatísticas forem aprovadas, a avaliação será concluída antes que o período de tempo especificado em **-maxT** tenha decorrido. Se a avaliação for executada pelo período de tempo especificado em **-maxT** sem satisfazer as verificações estatísticas, a avaliação será concluída naquele momento e retornará os resultados coletados.|
|-BufferSize \<n>|Especifique o tamanho do buffer que o teste de cópia de memória deve usar. Duas vezes esse valor será alocado por CPU, o que determina a quantidade de dados copiados de um buffer para outro. O padrão é 16MB. Esse valor é arredondado para o limite mais próximo de 4 KB. O valor máximo é 32 MB. O valor mínimo é 4 KB. A especificação de um tamanho de buffer inválido resultará em um erro.|
|-v|Enviar saída detalhada para STDOUT, incluindo informações de status e progresso. Todos os erros também serão gravados na janela de comando.|
|-nome \<do arquivo XML>|Salve a saída da avaliação como o arquivo XML especificado. Se o arquivo especificado existir, ele será substituído.|
|-idiskinfo|Salve informações sobre volumes físicos e discos lógicos como parte da seção de ** \<>de SystemConfig** na saída XML.|
|-iguid|Crie um GUID (identificador global exclusivo) no arquivo de saída XML.|
|-texto da nota de nota|Adicione o texto da nota à seção ** \<>de observação** no arquivo de saída XML.|
|-icn|Inclua o nome do computador local no arquivo de saída XML.|
|-EEF|Enumere informações adicionais do sistema no arquivo de saída XML.|

## <a name="examples"></a>Exemplos

- Para executar a avaliação por um mínimo de 4 segundos e não mais de 12 segundos, usando um tamanho de buffer de 32MB e salvando os resultados em formato XML no arquivo **memtest. xml**.  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>Comentários

-   A associação no grupo local de administradores, ou equivalente, é o requisito mínimo necessário para usar o **WinSAT**. O comando deve ser executado em uma janela de prompt de comandos com privilégios elevados.
-   Para abrir uma janela de prompt de comando com privilégios elevados, clique em **Iniciar**, **acessórios**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.

## <a name="additional-references"></a>Referências adicionais

