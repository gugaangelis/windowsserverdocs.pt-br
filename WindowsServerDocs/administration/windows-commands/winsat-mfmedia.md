---
title: winsat mfmedia
description: Referência para WinSAT mfmedia, que mede o desempenho da decodificação de vídeo (reprodução) usando a estrutura de Media Foundation.
ms.topic: reference
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 889ef018e5803f9905100b5ae0b65f1bc0c4093e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038094"
---
# <a name="winsat-mfmedia"></a>winsat mfmedia



Mede o desempenho da decodificação de vídeo (reprodução) usando o Media Foundation Framework.



## <a name="syntax"></a>Sintaxe

```
winsat mfmedia <parameters>
```

### <a name="parameters"></a>Parâmetros

|Parâmetros|Descrição|
|----------|-----------|
|-entrada \<file name>|Obrigatório: especifique o arquivo que contém o clipe de vídeo a ser reproduzido ou codificado. O arquivo pode estar em qualquer formato que possa ser renderizado pelo Media Foundation.|
|-dumpgraph|Especifique que o grafo de filtro deve ser salvo em um arquivo compatível com GraphEdit antes do início da avaliação.|
|-NS|Especifique que o grafo de filtro deve ser executado na velocidade de reprodução normal do arquivo de entrada. Por padrão, o grafo de filtro é executado o mais rápido possível, ignorando os tempos de apresentação.|
|-reproduzir|Execute a avaliação no modo de decodificação e reproduza qualquer conteúdo de áudio fornecido no arquivo especificado na **entrada** usando o dispositivo DirectSound padrão. Por padrão, a reprodução de áudio é desabilitada.|
|-nopmp|Não use o processo de Media Foundation MFPMP (pipeline de mídia protegido) durante a avaliação.|
|-PMP|Sempre faça uso do processo MFPMP durante a avaliação.</br>Observação: se **-PMP** ou **-nopmp** não for especificado, MFPMP será usado somente quando necessário.|
|-v|Enviar saída detalhada para STDOUT, incluindo informações de status e progresso. Todos os erros também serão gravados na janela de comando.|
|-XML \<file name>|Salve a saída da avaliação como o arquivo XML especificado. Se o arquivo especificado existir, ele será substituído.|
|-idiskinfo|Salve informações sobre volumes físicos e discos lógicos como parte da **\<SystemConfig>** seção na saída XML.|
|-iguid|Crie um GUID (identificador global exclusivo) no arquivo de saída XML.|
|-texto da nota de nota|Adicione o texto da nota à **\<note>** seção no arquivo de saída XML.|
|-icn|Inclua o nome do computador local no arquivo de saída XML.|
|-EEF|Enumere informações adicionais do sistema no arquivo de saída XML.|

## <a name="examples"></a>Exemplos

- Para executar a avaliação com o arquivo de entrada usado durante uma avaliação **formal do WinSAT** , sem empregar o Media Foundation MFPMP (pipeline de mídia protegida), em um computador em que c:\Windows é o local da pasta do Windows.
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>Comentários

-   A associação no grupo local de administradores, ou equivalente, é o requisito mínimo necessário para usar o **WinSAT**. O comando deve ser executado em uma janela de prompt de comandos com privilégios elevados.
-   Para abrir uma janela de prompt de comando com privilégios elevados, clique em **Iniciar**, **acessórios**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.

## <a name="additional-references"></a>Referências adicionais

