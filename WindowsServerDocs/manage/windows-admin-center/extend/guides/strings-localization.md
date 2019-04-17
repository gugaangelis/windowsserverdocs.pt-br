---
title: Cadeias de caracteres e localização no Windows Admin Center
description: Saiba mais sobre Preparando as cadeias de caracteres para a localização no SDK do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f0671160bd790d80e35f6d6c1586a07969939070
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296728"
---
# Cadeias de caracteres e localização no Windows Admin Center #

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Vamos mais detalhada para o SDK do Windows Admin Center extensões e falar sobre cadeias de caracteres e localização.

Para permitir a localização de todas as cadeias de caracteres que são renderizados na camada de apresentação, tirar proveito do arquivo strings.resjson sob /src/resources/strings - ele já está configurado. Quando você precisa adicionar uma nova cadeia de caracteres para sua extensão, adicione-a esse arquivo resjson como uma nova entrada. A estrutura existente segue este formato:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Você pode usar qualquer formato que desejar para as cadeias de caracteres, mas lembre-se de que o processo de geração (o processo que utiliza a resjson e gera a classe TypeScript utilizável) converte sublinhado (_) em pontos (.).

Por exemplo, esta entrada:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Gera a seguinte estrutura de acessador:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## Adicionar outros idiomas para a localização ## 

Para a localização para outros idiomas, um arquivo strings.resjson precisa ser criado para cada idioma. Esses arquivos precisam ser lugares no ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```. Os idiomas disponíveis com pastas correspondentes são:

| Idioma      | Pasta      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Deutsch | de-DE |
| Português | en-US |
| Espanhol | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| Italiano | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| Países baixos | nl-NL |
| Polski | pl-PL |
| Português (Brasil) | pt-BR |
| Português (Portugal) | pt-PT |
| РУССКИЙ | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> Se suas necessidades de estrutura de arquivo são diferente dentro da saída/loc, você precisará ajustar o localeOffset para a tarefa de gulp 'Gerar resjson-json-localizada' na gulpfile.js. Esse deslocamento é como aprofundar na pasta loc deve começar a procurar strings.resjson arquivos.

Cada arquivo strings.resjson será formatado da mesma maneira, conforme mencionado anteriormente na parte superior deste guia. 

Por exemplo, para incluir uma localização para espanhol incluir essa entrada no ```\loc\output\HelloWorld\es-ES\strings.resjson```: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
A qualquer momento que você adicionou cadeias de caracteres localizadas, os gulp gerar deve ser executado novamente para que eles aparecem. Executar:
``` cmd
gulp generate 
```

Para confirmar que as cadeias de caracteres foram geradas navegue até ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```. Sua entrada recém-adicionado será exibido nesse arquivo.
Agora se você alternar a opção de idioma no Windows Admin Center, você poderá ver as cadeias de caracteres localizadas em sua extensão. 
