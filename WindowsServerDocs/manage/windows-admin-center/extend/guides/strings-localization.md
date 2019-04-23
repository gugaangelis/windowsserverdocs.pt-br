---
title: Cadeias de caracteres e a localização no Windows Admin Center
description: Saiba mais sobre Preparando suas cadeias de caracteres para a localização no Windows Admin Center SDK (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fb328f86c98141a5a2a1c4fd05ec1d4c96a7bc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845397"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Cadeias de caracteres e a localização no Windows Admin Center #

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Vamos mais detalhada para o SDK do Windows Admin Center extensões e falar sobre cadeias de caracteres e localização.

Para habilitar a localização de todas as cadeias de caracteres que são renderizados na camada de apresentação, tirar proveito do arquivo strings.resjson sob /src/resources/strings – ele já está definido. Quando você precisa adicionar uma nova cadeia de caracteres para sua extensão, adicione-a esse arquivo resjson como uma nova entrada. A estrutura existente segue este formato:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Você pode usar qualquer formato que você deseja para as cadeias de caracteres, mas lembre-se de que o processo de geração (o processo que usa o resjson e gera a classe do TypeScript utilizável) converte o caractere de sublinhado (_) em pontos (.).

Por exemplo, esta entrada:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Gera a seguinte estrutura de acessador:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```
