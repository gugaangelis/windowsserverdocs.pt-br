---
title: Modificar o comportamento de navegação de raiz
description: Desenvolver uma extensão de solução SDK do centro de administração do Windows (projeto Honolulu) – modificar o comportamento de navegação raiz
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc7ef3c25c41abd6c7b91e37cecca017664ee223
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944937"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modificar o comportamento de navegação raiz para uma extensão de solução

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Neste guia, aprenderemos como modificar o comportamento de navegação raiz de sua solução para ter um comportamento de lista de conexão diferente, bem como ocultar ou mostrar a lista de ferramentas.

## <a name="modifying-root-navigation-behavior"></a>Modificando o comportamento de navegação raiz

Abra manifest.jsno arquivo em {Extension root} \src e localize a propriedade "rootNavigationBehavior". Essa propriedade tem dois valores válidos: "Connections" ou "Path". O comportamento de "conexões" é detalhado posteriormente na documentação.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Definindo o caminho como um rootNavigationBehavior

Defina o valor de ```rootNavigationBehavior``` para ```path``` e, em seguida, exclua a ```requirements``` propriedade e deixe a ```path``` propriedade como uma cadeia de caracteres vazia. Você concluiu a configuração mínima necessária para criar uma extensão de solução. Salve o arquivo e Gulp Build-> Gulp sirva como você faria com uma ferramenta e, em seguida, carregue a extensão em sua extensão do centro de administração do Windows local.

Uma matriz de ponto de entrada de manifesto válida tem esta aparência:
```
    "entryPoints": [
        {
          "entryPointType": "solution",
          "name": "main",
          "urlName": "testsln",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "path": "",
          "rootNavigationBehavior": "path"
        }
    ],
```

As ferramentas criadas com esse tipo de estrutura não exigirão que as conexões sejam carregadas, mas também não terão a funcionalidade de conectividade de nó.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Definindo conexões como um rootNavigationBehavior

Quando você define a ```rootNavigationBehavior``` propriedade como ```connections``` , está informando ao shell do centro de administração do Windows que haverá um nó conectado (sempre um servidor de algum tipo) ao qual ele deve se conectar e verificar o status da conexão. Com isso, há duas etapas para verificar a conexão. 1) o centro de administração do Windows tentará fazer logon no nó com suas credenciais (para estabelecer a sessão remota do PowerShell) e 2), ele executará o script do PowerShell que você fornecerá para identificar se o nó está em um estado de conexão.

Uma definição de solução válida com conexões terá a seguinte aparência:

``` json
        {
          "entryPointType": "solution",
          "name": "example",
          "urlName": "solutionexample",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "rootNavigationBehavior": "connections",
          "connections": {
            "header": "resources:strings:connectionsListHeader",
            "connectionTypes": [
                "msft.sme.connection-type.example"
                ]
            },
            "tools": {
                "enabled": false,
                "defaultTool": "solution"
            }
        },
```

Quando rootNavigationBehavior é definido como "Connections", você precisa criar a definição de conexões no manifesto. Isso inclui a propriedade "header" (será usada para exibir no cabeçalho da solução quando um usuário selecioná-la no menu), uma matriz ConnectionType (isso especificará quais ConnectionType serão usados na solução. Mais sobre isso na documentação do ConnectionProvider.).

## <a name="enabling-and-disabling-the-tools-menu"></a>Habilitando e desabilitando o menu ferramentas ##

Outra propriedade disponível na definição da solução é a propriedade "Tools". Isso determinará se o menu ferramentas é exibido, bem como a ferramenta que será carregada. Quando habilitado, o centro de administração do Windows processará o menu de ferramentas à esquerda. Com o defaulttool, é necessário adicionar um ponto de entrada de ferramenta ao manifesto para carregar os recursos apropriados. O valor de "defaulttool" precisa ser a propriedade "Name" da ferramenta, conforme definido no manifesto.
