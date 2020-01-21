---
title: Habilitando a faixa de descoberta de extensão
description: Habilitando a faixa de descoberta de extensão
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d761ba61ae5680373c334889799e82e5d092a0d4
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950104"
---
# <a name="enabling-the-extension-discovery-banner"></a>Habilitando a faixa de descoberta de extensão

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Um novo recurso disponível na visualização do centro de administração do Windows 1903 é a faixa de descoberta de extensão. Esse recurso permite que uma extensão declare o fabricante de hardware do servidor e os modelos aos quais ele dá suporte e quando um usuário se conecta a um servidor ou cluster para o qual uma extensão está disponível, uma faixa de notificação será exibida para instalar facilmente a extensão. Os desenvolvedores de extensão poderão obter mais visibilidade para suas extensões e os usuários poderão descobrir facilmente mais recursos de gerenciamento para seus servidores.

![Faixa de descoberta de extensão](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Como funciona a faixa de descoberta de extensão

Quando o centro de administração do Windows for iniciado, ele se conectará aos feeds de extensão registrados e buscará os metadados para os pacotes de extensão disponíveis. Em seguida, quando um usuário se conecta a um servidor ou cluster no centro de administração do Windows, podemos ler o fabricante e o modelo de hardware de servidor para serem exibidos na ferramenta de visão geral. Se encontrarmos uma extensão que declara que oferece suporte ao fabricante e/ou modelo do servidor atual, vamos exibir uma faixa para permitir que o usuário saiba. Clicar no link "configurar agora" levará o usuário para o Gerenciador de extensões, onde poderá instalar a extensão.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Como implementar a faixa de descoberta de extensão

Os metadados de "marcas" no arquivo. nuspec são usados para declarar a qual fabricante de hardware e/ou modelos sua extensão dá suporte. As marcas são delimitadas por espaços e você pode adicionar um fabricante ou uma marca de modelo, ou ambos, para declarar o fabricante e/ou modelos com suporte. O formato de marca é ``"[value type]_[value condition]"`` em que [tipo de valor] é "fabricante" ou "modelo" (diferencia maiúsculas de minúsculas) e [valor condição] é uma [expressão regular JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Regular_Expressions) que define o fabricante ou a cadeia de caracteres de modelo e [tipo de valor] e [condição de valor] são separados por um sublinhado. Em seguida, essa cadeia de caracteres é codificada usando a codificação de URI e adicionada à cadeia de caracteres de metadados. nuspec "tags".

### <a name="example"></a>Exemplo

Digamos que desenvolvi uma extensão que dá suporte a servidores de uma empresa chamada Contoso Inc., com o nome do modelo R3xx e R4xx.

1. A marca para o fabricante seria ``"Manufacturer_/Contoso Inc./"``. A marca para os modelos pode ser ``"Model_/^R[34][0-9]{2}$/"``. Dependendo de quão estritamente você deseja definir a condição de correspondência, haverá maneiras diferentes de definir sua expressão regular. Você também pode separar as marcas de fabricante ou modelo em várias marcas, por exemplo, a marca de modelo também pode ser ``"Model_/R3../ Model_/R4../"``.
2. Você pode testar a expressão regular com o console do DevTools do seu navegador da Web. No Microsoft Edge ou no Chrome, pressione F12 para abrir a janela DevTools e, na guia Console, digite o seguinte e pressione ENTER:

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Em seguida, se você digitar e executar o seguinte, ele retornará ' true '.

   ```javascript
   regex.test('R300')
   ```

   E se você executar o seguinte, ele retornará ' false '.

   ```javascript
   regex.test('R500')
   ```

3. Depois de verificar a expressão regular, você pode codificá-la no console do DevTools também, usando o seguinte método JavaScript:

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   O formato final da cadeia de caracteres de marca a ser adicionada ao seu arquivo. nuspec seria:

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Entendemos que um fabricante de hardware pode ter uma grande variedade de nomes de modelo dos quais alguns deles podem ter suporte, enquanto outros não. Tenha em mente que esse recurso destina-se a ajudar na **descoberta** de sua extensão, mas não precisa ser um inventário perfeitamente atualizado de todos os seus modelos. Você pode definir sua expressão regular para ser uma expressão mais simples que corresponda a um subconjunto de seus modelos. Um usuário poderá não ver a faixa de descoberta se se conectar pela primeira vez a um modelo de servidor que não corresponda à condição, mas, mais cedo ou tarde, eles se conectarão a outro servidor que faz e irão descobrir e instalar a extensão. Você também pode considerar a definição de uma expressão regular simples que corresponda apenas ao nome do fabricante. Em alguns casos, sua extensão pode, na verdade, não dar suporte a um modelo específico, mas você pode usar o [recurso de exibição de ferramenta dinâmica](./dynamic-tool-display.md) para definir um script do PowerShell personalizado para verificar o suporte de modelo e mostrar apenas sua extensão quando aplicável, ou fornecer funcionalidade limitada em sua extensão para modelos que não dão suporte a todos os recursos.