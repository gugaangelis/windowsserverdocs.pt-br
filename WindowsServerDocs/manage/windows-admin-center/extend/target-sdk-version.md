---
title: Direcionar uma versão diferente do SDK do Windows Admin Center
description: Direcionar uma versão diferente do SDK do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081033"
---
# Direcionar uma versão diferente do SDK do Windows Admin Center

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

É fácil manter sua extensão atualizados com alterações SDK e plataforma.  Usamos [NPM marcas](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) para organizar o lançamento dos novos recursos em versões do SDK.

Há três versões do SDK, entre que você pode escolher:

* ```latest``` – Esse pacote SDK se alinha com a versão GA atual do Windows Admin Center
* ```insider``` – Esse pacote SDK se alinha com a versão atual da visualização do Windows Admin Center (disponível no Windows Server Insider Preview)
* ```next``` – Esse pacote SDK contém a funcionalidade mais recente

> [!NOTE]
> Saiba mais sobre as diferentes [versões](https://aka.ms/WACDownloadPage) do Windows Admin Center que estão disponíveis para download.

## Versão do SDK em um novo projeto de direcionamento

Ao criar uma nova extensão, você pode incluir o ```--version``` parâmetro para direcionar uma versão diferente do SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |
| ```{!version}``` | Versão do SDK | ```latest``` |

Aqui está um exemplo de criação de uma nova extensão direcionamento ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## Direcionando a versão do SDK em um projeto existente

Para modificar um projeto existente para direcionar uma versão diferente do SDK, modifique a seguinte linha na ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
Neste exemplo, substitua ```latest``` com a versão do SDK desejado, ou seja, ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Em seguida, execute ```npm install``` atualizar referências em todo o projeto.