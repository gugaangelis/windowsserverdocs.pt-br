---
title: Modificar o comportamento de navegação de raiz
description: Desenvolver uma extensão de solução SDK do Windows Admin Center (Project Honolulu) - modificar o comportamento de navegação de raiz
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905036"
---
# Modificar o comportamento de navegação de raiz de uma extensão de solução

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Neste guia, podemos aprenderá como modificar o comportamento de navegação raiz para sua solução ter o comportamento da lista de conexão diferentes, bem como ocultar ou mostrar a lista de ferramentas.

## Modificar o comportamento de navegação de raiz

Abra o arquivo de manifest.json em {raiz da extensão} \src e localize a propriedade "rootNavigationBehavior". Essa propriedade tem dois valores válidos: "conexões" ou "path". O comportamento de "conexões" é detalhado posteriormente na documentação.

### Definindo o caminho como um rootNavigationBehavior

Defina o valor de ```rootNavigationBehavior``` para ```path```e, em seguida, excluir o ```requirements``` propriedade e deixe o ```path``` propriedade como uma cadeia de caracteres vazia. Você concluiu a configuração mínima necessária para criar uma extensão de solução. Salve o arquivo e compilação gulp -> gulp servem como você faria uma ferramenta e, em seguida, lado carregar a extensão em sua extensão de Windows Admin Center local.

Uma matriz de pontos de manifesto válido tem esta aparência:
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

Ferramentas criadas com esse tipo de estrutura serão conexões não é necessárias para carregar, mas não têm funcionalidade de conectividade do nó.

### Configurar conexões como um rootNavigationBehavior

Quando você define o ```rootNavigationBehavior``` propriedade ```connections```, você está solicitando que o Shell do Windows Admin Center que haverá um nó conectado (sempre um servidor de algum tipo) que ele deve se conectar, e verificar o status de conexão. Com isso, há 2 etapas no verificando a conexão. 1) o Windows Admin Center tentará fazer uma tentativa de efetuar login no nó com suas credenciais (para estabelecer a sessão remota do PowerShell), e 2) ele será executado o script do PowerShell que você forneceu para identificar se o nó está em um estado conectável.

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

Quando o rootNavigationBehavior estiver definido como "conexões" são necessários para criar a definição de conexões no manifesto. Isso inclui a propriedade "cabeçalho" (será usado para exibir no cabeçalho sua solução quando um usuário seleciona-lo no menu), uma matriz de connectionTypes (isso deve especificar quais connectionTypes são usadas na solução. Falaremos mais sobre isso na documentação do connectionProvider.).

## Habilitando e desabilitando no menu Ferramentas ##

Outra propriedade disponível na definição da solução é o "ferramentas". Isso será determinam se é exibido no menu Ferramentas, bem como a ferramenta que será carregada. Quando habilitado, o Windows Admin Center renderizará no menu Ferramentas de mão esquerda. Com defaultTool, é necessário que você adicionar um ponto de entrada de ferramenta o manifesto para carregar os recursos apropriados. O valor de "defaultTool" precisa ser a propriedade "name" da ferramenta conforme ele é definido no manifesto.
