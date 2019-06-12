---
title: Habilitando a faixa de descoberta da extensão
description: Habilitando a faixa de descoberta da extensão
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fafc16d6acae3c5a7a58a379d2f88998b8e98c3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811874"
---
# <a name="enabling-the-extension-discovery-banner"></a>Habilitando a faixa de descoberta da extensão

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Um novo recurso disponível no Windows Admin Center visualização 1903 é a faixa de descoberta da extensão. Esse recurso permite que uma extensão declarar os modelos que oferece suporte a ele e o fabricante de hardware de servidor e quando um usuário se conecta a um servidor ou cluster para o qual uma extensão está disponível, será exibida uma faixa de notificação para instalar a extensão com facilidade. Os desenvolvedores de extensão poderão obter visibilidade mais para suas extensões e os usuários serão capazes de descobrir com facilidade mais recursos de gerenciamento para seus servidores.

![Faixa de descoberta de extensão](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Como funciona a faixa de descoberta da extensão

Quando Windows Admin Center é iniciado, ele se conectar os feeds de extensão registradas e buscar os metadados para os pacotes de extensão disponível. Em seguida, quando um usuário se conecta a um servidor ou cluster no Windows Admin Center, lemos a fabricante de hardware de servidor e o modelo para exibir na ferramenta de visão geral. Se encontrarmos uma extensão que declara que dá suporte ao fabricante do servidor e/ou modelo, vamos exibir uma faixa para informar o usuário. Clicar no link "Configurar agora" levará o usuário para o Extension Manager, onde eles poderão instalar a extensão.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Como implementar a faixa de descoberta da extensão

Os metadados de "tags" no arquivo. NuSpec é usado para declarar quais fabricante de hardware e/ou modelos suportadas pela extensão. As marcas são delimitadas por espaços, e você pode adicionar um fabricante ou marca de modelo ou ambos para declarar os modelos de e/ou com suporte do fabricante. É o formato de marca ``"[value type]_[value condition]"`` onde [tipo de valor] é "Fabricante" ou "Modelo" (diferencia maiusculas de minúsculas) e [valor condição] é um [expressão regular Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) definindo o fabricante ou a cadeia de caracteres de modelo e a [tipo de valor] e [ condição de valor] separados por um caractere de sublinhado. Essa cadeia de caracteres, em seguida, é codificada usando o URI de codificação e adicionado à cadeia de caracteres de metadados. NuSpec "tags".

### <a name="example"></a>Exemplo

Digamos que eu desenvolvi uma extensão que oferece suporte a servidores de uma empresa chamada Contoso Inc., com o modelo de nome R3xx e R4xx.

1. A marca para o fabricante seria ``"Manufacturer_/Contoso Inc./"``. A marca para os modelos pode ser ``"Model_/^R[34][0-9]{2}$/"``. Dependendo estritamente como você deseja definir a condição de correspondência, haverá diferentes maneiras de definir sua expressão regular. Você também pode separar as marcas do fabricante ou modelo em várias marcas, por exemplo, a marca de modelo também poderia ser ``"Model_/R3../ Model_/R4../"``.
2. Você pode testar a expressão regular com o Console de DevTools do navegador da web. No Edge ou Chrome, pressionar F12 para abrir a janela DevTools e, na guia do Console, digite o seguinte e clique em Enter:

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Se você digitar e execute o seguinte, ele retornará 'true'.

   ```javascript
   regex.test('R300')
   ```

   E se você executar o seguinte, ele retornará 'false'.

   ```javascript
   regex.test('R500')
   ```

3. Depois de verificar se a expressão regular, você pode codificá-lo no Console do DevTools também, usando o método de Javascript a seguir:

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   O formato final da cadeia de caracteres de marcação para adicionar ao seu arquivo. NuSpec seria:

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Entendemos que um fabricante de hardware pode ter uma enorme variedade de nomes de modelo que alguns podem ter suporte enquanto alguns não são. Tenha em mente que esse recurso é destinado a ajudar com o **descoberta** de sua extensão, mas ele não precisa ser um inventário perfeitamente atualizado de todos os seus modelos. Você pode definir a expressão regular para ser uma expressão mais simples que corresponda a um subconjunto de seus modelos. Um usuário não pode ver a faixa de descoberta se conectarem pela primeira vez a um modelo de servidor que não corresponde à condição, mas o mais cedo ou tarde eles se conectarão para outro servidor que e de descobrir e instalar a extensão. Você também pode definir uma expressão regular simple que corresponde apenas o nome do fabricante. Em alguns casos, sua extensão pode não suportar um modelo específico, mas você pode usar o [recurso de exibição de ferramenta dinâmica](./dynamic-tool-display.md) para definir um script do PowerShell personalizado para verificar o suporte ao modelo e mostrar apenas sua extensão quando aplicável, ou fornece funcionalidade limitada em sua extensão para modelos que não dão suporte a todos os recursos.