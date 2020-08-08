---
title: Direcionar uma versão diferente do SDK do centro de administração do Windows
description: Destino de uma versão diferente do SDK do centro de administração do Windows (projeto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 96e17326bc289b4ad018da59b01344956586a198
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964552"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Direcionar uma versão diferente do SDK do centro de administração do Windows

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Manter sua extensão atualizada com alterações do SDK e alterações na plataforma é fácil.  Usamos [marcas NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) para organizar o lançamento de novos recursos em versões do SDK.

Há três versões do SDK das quais você pode escolher:

* ```latest```– Este pacote do SDK se alinha com a versão atual do centro de administração do Windows no GA
* ```insider```– Este pacote SDK se alinha com a versão prévia atual do centro de administração do Windows (disponível no Windows Server Insider Preview)
* ```next```– Este pacote SDK contém a funcionalidade mais recente

> [!NOTE]
> Saiba mais sobre as diferentes [versões](https://aka.ms/WACDownloadPage) do centro de administração do Windows que estão disponíveis para download.

## <a name="targeting-sdk-version-on-a-new-project"></a>Direcionando a versão do SDK em um novo projeto

Ao criar uma nova extensão, você pode incluir o ```--version``` parâmetro para direcionar uma versão diferente do SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |
| ```{!version}``` | Versão do SDK | ```latest``` |

Veja um exemplo de como criar um novo direcionamento de extensão ```insider``` :

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Direcionando a versão do SDK em um projeto existente

Para modificar um projeto existente para ter como destino uma versão diferente do SDK, modifique a seguinte linha em ```package.json``` :

```
"@microsoft/windows-admin-center-sdk": "latest",
```
Neste exemplo, substitua ```latest``` pela versão do SDK desejada, ou seja ```insider``` :

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Em seguida, execute ```npm install``` para atualizar as referências em todo o seu projeto.