---
title: Habilitar a faixa de descoberta de extensão
description: Habilitar a faixa de descoberta de extensão
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/08/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 389acba6d1fe6f65320bd780c9fde6469b387e0b
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262884"
---
# Habilitar a faixa de descoberta de extensão #

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Um novo recurso disponível no Windows Admin Center Preview 1903 é a faixa de descoberta de extensão. Esse recurso permite que uma extensão declarar o fabricante do hardware de servidor e modelos que oferece suporte a ele e, quando um usuário se conecta a um servidor ou cluster para o qual uma extensão está disponível, uma faixa de notificação será exibida para instalar facilmente a extensão. Os desenvolvedores de extensão será capazes de obter visibilidade mais de suas extensões e os usuários poderão facilmente descobrir mais recursos de gerenciamento para seus servidores.

![Faixa de descoberta de extensão](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## Como funciona a faixa de descoberta de extensão ##

Quando o Windows Admin Center é iniciado, ele se conectará a extensão registrada feeds e buscar os metadados para os pacotes de extensão disponíveis. Em seguida, quando um usuário se conecta a um servidor ou cluster no Windows Admin Center, podemos ler o fabricante do hardware de servidor e o modelo para exibir na ferramenta de visão geral. Se encontrarmos uma extensão que declara que dá suporte ao fabricante do servidor atual e/ou modelo, exibiremos uma faixa para informar o usuário. Clicar no link "Configurar agora" levará o usuário ao Gerenciador de extensão onde eles podem instalar a extensão.

## Como implementar a faixa de descoberta de extensão ##

Os metadados de "marcas" no arquivo .nuspec é usado para declarar que o fabricante do hardware e/ou modelos seu dá suporte à extensão. Marcas são delimitadas por espaços e você pode adicionar um fabricante ou marca de modelo ou ambos para declarar o fabricante com suporte e/ou modelos. O formato de marca é ``"[value type]_[value condition]"`` onde [tipo de valor] é "Fabricante" ou "Modelo" (diferencia maiusculas de minúsculas) e [condição valor] é uma [expressão regular Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) definindo o fabricante ou cadeia de caracteres de modelo e [tipo de valor] e [valor condição] são separados por um sublinhado. Essa cadeia de caracteres é codificada, em seguida, usando o URI de codificação e adicionados à cadeia de caracteres de metadados .nuspec "marcas".

### Exemplo ###

Digamos que eu já desenvolveu uma extensão que oferece suporte a servidores de uma empresa chamada Contoso Inc., com o modelo de nomes R3xx e R4xx.

1. A marca do fabricante do seria ``"Manufacturer_/Contoso Inc./"``. A marca para os modelos poderia ser ``"Model_/^R[34][0-9]{2}$/"``. Dependendo de como estritamente você deseja definir a condição de correspondência, haverá diferentes maneiras de definir sua expressão regular. Você também pode separar as marcas fabricante ou modelo em várias marcas, por exemplo, a marca de modelo também pode ser ``"Model_/R3../ Model_/R4../"``.
2. Você pode testar a expressão regular com o Console de ferramentas do navegador da web. No Edge ou cromo, pressione F12 para abrir a janela de ferramentas e na guia do Console, digite Enter a seguir e clique:

```javascript
var regex = /^R[34][0-9]{2}$/
```

Em seguida, se você digitar e execute o seguinte, ele retornará 'true'.

```javascript
regex.test('R300')
```

E se você executar o seguinte, ele retornará 'false'.

```javascript
regex.test('R500')
```

3. Depois que você verificou a expressão regular, você pode codificá-lo no Console de ferramentas, usando o seguinte método Javascript:

```javascript
encodeURI(/^R[34][0-9]{2}$/)
```

O formato final da cadeia de caracteres marca para adicionar ao seu arquivo .nuspec seria:

```
<tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
```

> [!Tip]
> Entendemos que um fabricante de hardware pode ter uma ampla variedade de nomes de modelos de que alguns podem ter suporte enquanto alguns não são. Tenha em mente que esse recurso é destinado a ajudar com a **Descoberta** de sua extensão, mas ele não precisa ser um inventário perfeitamente atualizado de todos os seus modelos. Você pode definir sua expressão regular para ser uma expressão mais simples que corresponde a um subconjunto de seus modelos. Um usuário não pode ver a faixa de descoberta se conectarem inicialmente a um modelo de servidor que não corresponda a condição, mas o mais cedo ou mais tarde, eles se conectam ao outro servidor que e de descobrir e instale a extensão. Você também pode considerar definir uma expressão regular simple que corresponde somente o nome do fabricante. Em alguns casos, sua extensão pode não suportar um modelo específico, mas você pode usar a [ferramenta dinâmica exibir recurso](./dynamic-tool-display.md) para definir um script PowerShell personalizado para verificar o suporte ao modelo e mostrar somente sua extensão quando aplicável, ou fornecer limitado funcionalidade em sua extensão modelos que não dão suporte a todos os recursos.