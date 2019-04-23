---
title: Modificar o comportamento de navegação de raiz
description: Desenvolver uma solução de extensão (projeto Paulo) do SDK do Windows Admin Center – modificar o comportamento de navegação de raiz
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861727"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modificar o comportamento de navegação de raiz de uma extensão da solução

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Neste guia, aprenderá como modificar o comportamento de navegação de raiz para sua solução para ter o comportamento da lista de conexão diferentes, bem como ocultar ou mostrar a lista de ferramentas.

## <a name="modifying-root-navigation-behavior"></a>Modificando o comportamento de navegação de raiz

Abra o arquivo manifest. JSON no {raiz da extensão} \src. e encontrar a propriedade "rootNavigationBehavior". Essa propriedade tem dois valores válidos: "conexões" ou "path". O comportamento de "conexões" é detalhado posteriormente na documentação.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Caminho de configuração como um rootNavigationBehavior

Defina o valor da ```rootNavigationBehavior``` à ```path```e, em seguida, exclua o ```requirements``` propriedade e deixe o ```path``` a propriedade como uma cadeia de caracteres vazia. Você concluiu a configuração mínima necessária para criar uma extensão da solução. Salve o arquivo e compilação do gulp -> gulp sirvam como você fariam uma ferramenta e o lado, em seguida, carregar a extensão em sua extensão Windows Admin Center local.

Uma matriz de pontos de entrada de manifesto válido tem esta aparência:
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

Ferramentas incorporadas com esse tipo de estrutura serão conexões não é necessárias para carregar, mas não terá tanto a funcionalidade de conectividade de nó.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Configuração de conexões como um rootNavigationBehavior

Quando você define o ```rootNavigationBehavior``` propriedade para ```connections```, você está informando o Shell do Windows Admin Center que haverá um nó conectado (sempre um servidor de algum tipo) que ele deve se conectar e verificar o status de conexão. Com isso, há etapas de 2 verificar a conexão. 1) Windows Admin Center tentará fazer uma tentativa de logon no nó com suas credenciais (para estabelecer a sessão remota do PowerShell) e 2) executará o script do PowerShell que você fornece para identificar se o nó está em um estado conectável.

Uma definição de solução válido com conexões terá esta aparência:

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

Quando o rootNavigationBehavior é definido como "conexões" são necessárias para criar a definição de conexões no manifesto. Isso inclui a propriedade "cabeçalho" (será usado para exibir no cabeçalho de sua solução quando um usuário seleciona-lo a partir do menu), uma matriz de connectionTypes (Isso especificará quais connectionTypes são usados na solução. Mais informações sobre isso na documentação do connectionProvider.).

## <a name="enabling-and-disabling-the-tools-menu"></a>Habilitando e desabilitando o menu Ferramentas ##

Outra propriedade disponível na definição de solução é a propriedade "ferramentas". Isso ditará se é exibido no menu Ferramentas, bem como a ferramenta que será carregada. Quando habilitado, o Windows Admin Center será renderizado no menu de ferramentas à esquerda. Com defaultTool, é necessário que você adicione um ponto de entrada de ferramenta para o manifesto para carregar os recursos apropriados. O valor de "defaultTool" precisa ser a propriedade "name" da ferramenta, conforme definido no manifesto.
