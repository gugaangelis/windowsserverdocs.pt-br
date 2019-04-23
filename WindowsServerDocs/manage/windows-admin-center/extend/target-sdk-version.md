---
title: Uma versão diferente do SDK do Windows Admin Center de destino
description: Uma versão diferente do SDK do Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833617"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Uma versão diferente do SDK do Windows Admin Center de destino

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

É fácil manter sua extensão atualizada com as alterações do SDK e plataforma.  Usamos [NPM marcas](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) para organizar o lançamento de novos recursos em versões do SDK.

Há três versões do SDK, entre que você pode escolher:

* ```latest``` – Este pacote SDK se alinha com a atual versão de GA do Windows Admin Center
* ```insider``` – Este pacote SDK se alinha com a versão de visualização atual do Windows Admin Center (disponível no Windows Server Insider Preview)
* ```next``` – Este pacote do SDK contém a funcionalidade mais recente

> [!NOTE]
> Saiba mais sobre os diferentes [versões](https://aka.ms/WACDownloadPage) do Windows Admin Center que estão disponíveis para download.

## <a name="targeting-sdk-version-on-a-new-project"></a>Direcionar a versão do SDK em um novo projeto

Ao criar uma nova extensão, você pode incluir o ```--version``` parâmetro para uma versão diferente do SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |
| ```{!version}``` | Versão do SDK | ```latest``` |

Aqui está um exemplo de criação de uma nova extensão direcionamento ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Direcionar a versão do SDK em um projeto existente

Para modificar um projeto existente para uma versão diferente do SDK de destino, modifique a seguinte linha no ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
Neste exemplo, substitua ```latest``` com a versão desejada do SDK, ou seja, ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Em seguida, execute ```npm install``` para atualizar referências em todo o projeto.