---
title: o WinSAT mfmedia
description: Comandos do Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c63682e474311a49b01dc8078b023547e1fb170
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440026"
---
# <a name="winsat-mfmedia"></a>o WinSAT mfmedia



Mede o desempenho de decodificação de vídeo (reprodução) usando a estrutura do Media Foundation.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>Parâmetros

|Parâmetros|Descrição|
|----------|-----------|
|-entrada \<nome do arquivo >|Obrigatório: Especifique o arquivo que contém o clipe de vídeo a ser reproduzida ou codificado. O arquivo pode estar em qualquer formato que pode ser processado pelo Media Foundation.|
|-dumpgraph|Especifique que o gráfico de filtro deve ser salvo em um arquivo compatível com o GraphEdit antes de inicia a avaliação.|
|-ns|Especifique que o gráfico de filtro deve ser executado na velocidade de reprodução normal do arquivo de entrada. Por padrão, o gráfico de filtro é executado mais rápido possível, ignorando os tempos de apresentação.|
|-play|Executar a avaliação na decodificação modo e reproduzir qualquer fornecido no arquivo especificado no conteúdo de áudio **-entrada** usando o dispositivo de DirectSound padrão. Por padrão, a reprodução de áudio está desabilitada.|
|-nopmp|Faz uso do Media Foundation protegido mídia Pipeline (MFPMP) processo durante a avaliação.|
|-pmp|Sempre utilizam o MFPMP processo durante a avaliação.</br>Observação: Se **- pmp** ou **- nopmp** não for especificado, MFPMP será usado somente quando necessário.|
|-v|Envie a saída detalhada para STDOUT, incluindo informações de status e o progresso. Os erros também serão gravados na janela de comando.|
|-xml \<file name>|Salve a saída da avaliação como o arquivo XML especificado. Se o arquivo especificado existir, ele será substituído.|
|-idiskinfo|Salvar informações sobre volumes físicos e os discos lógicos como parte do  **\<caixa >** seção na saída XML.|
|-iguid|Crie um identificador global exclusivo (GUID) no arquivo de saída XML.|
|-Observe "texto de Observação"|Adicione o texto de anotação para o  **\<Observação >** seção no arquivo de saída XML.|
|-icn|Inclua o nome do computador local no arquivo de saída XML.|
|-eef|Enumere as informações de sistema adicionais no arquivo de saída XML.|

## <a name="BKMK_examples"></a>Exemplos

- O exemplo a seguir executa a avaliação com o arquivo de entrada que é usado durante um **winsat formal** avaliação, sem empregar o Media Foundation protegido mídia Pipeline (MFPMP), em um computador onde o c:\windows é o local do a pasta Windows.  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>Comentários

-   A associação no grupo Administradores local, ou equivalente, é o mínimo necessário para usar **winsat**. O comando deve ser executado em uma janela de prompt de comando com privilégios elevados.
-   Para abrir uma janela de prompt de comando com privilégios elevados, clique em **inicie**, clique em **Acessórios**, clique com botão direito **Prompt de comando**e clique em **executar como administrador**.

#### <a name="additional-references"></a>Referências adicionais

