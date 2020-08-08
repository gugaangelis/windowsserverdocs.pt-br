---
title: Adicionar um módulo a uma extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do centro de administração do Windows (projeto Honolulu) – adicionar um módulo a uma extensão de ferramenta
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: e7875f8aa2320d7292b314cb18f3e17894e76fa0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945039"
---
# <a name="add-a-module-to-a-tool-extension"></a>Adicionar um módulo a uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Neste artigo, adicionaremos um módulo vazio a uma extensão de ferramenta que criamos com a CLI do centro de administração do Windows.

## <a name="prepare-your-environment"></a>Prepare o seu ambiente

Se você ainda não fez isso, siga as instruções em desenvolver uma extensão de [ferramenta](../develop-tool.md) (ou [solução](../develop-solution.md)) para preparar seu ambiente e criar uma nova extensão de ferramenta vazia.

## <a name="use-the-angular-cli-to-create-a-module-and-component"></a>Usar a CLI do angular para criar um módulo (e um componente)

Se você ainda não conhece a angulares, é altamente recomendável que você leia a documentação sobre o site Angular.Io para conhecer angulares e NgModule. Para obter mais informações sobre NgModule, acesse:https://angular.io/guide/ngmodule

* Mais informações sobre como gerar um novo módulo no CLI angular: https://github.com/angular/angular-cli/wiki/generate-module
* Mais informações sobre como gerar um novo componente no CLI angular: https://github.com/angular/angular-cli/wiki/generate-component


Abra um prompt de comando, altere o diretório para \src\app em seu projeto e, em seguida, execute os seguintes comandos, substituindo ```{!ModuleName}``` pelo nome do módulo (espaços removidos):

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | O nome do módulo (espaços removidos) | ```ManageFooWorksPortal``` |

Exemplo de uso:
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## <a name="add-routing-information"></a>Adicionar informações de roteamento

Se você ainda não conhece o Angular, é altamente recomendável que aprender sobre o Roteamento e a navegação angular. As seções a seguir definem os elementos de roteamento necessários que permitem o Windows Admin Center navegar até sua extensão e entre os modos de exibição dela em resposta a atividade do usuário. Para saber mais, acesse:https://angular.io/guide/router

Use o mesmo nome de módulo que você usou na etapa anterior.

### <a name="add-content-to-new-routing-file"></a>Adicionar conteúdo ao novo arquivo de roteamento

* Navegue até a pasta do módulo que foi criada por ``` ng generate ``` na etapa anterior.

* Criar um novo arquivo ```{!module-name}.routing.ts```, seguindo este convenção de nomenclatura:

    | Valor | Explicação | Exemplo de nome de arquivo |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal.routing.ts``` |

* Adicione este conteúdo ao arquivo recém criado:

    ``` ts
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { {!ModuleName}Component } from './{!module-name}.component';

    const routes: Routes = [
        {
            path: '',
            component: {!ModuleName}Component,
            // if the component has child components that need to be routed to, include them in the children array.
            children: [
                {
                    path: '',
                    redirectTo: 'base',
                    pathMatch: 'full'
                }
            ]
    }];

    @NgModule({
        imports: [
            RouterModule.forChild(routes)
        ],
        exports: [
            RouterModule
        ]
    })
    export class Routing { }
    ```

* Substitua os valores no arquivo recém criado pelos seus valores desejados:

    | Valor | Explicação | Exemplo |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | O nome do módulo (espaços removidos) | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal``` |

### <a name="add-content-to-new-module-file"></a>Adicionar conteúdo ao novo arquivo de módulo

Abra o arquivo ```{!module-name}.module.ts```, encontrado com a seguinte convenção de nomenclatura:

| Valor | Explicação | Exemplo de nome de arquivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal.module.ts``` |

* Adicione conteúdo para o arquivo:

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* Substitua os valores no conteúdo adicionada pelos seus valores desejados:

    | Valor | Explicação | Exemplo |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal``` |

* Modifique a instrução importações para importar roteamento:

    | Valor original | Novo valor |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* Verifique se as instruções ```import``` estão em ordem alfabética por fonte.

### <a name="add-content-to-new-component-typescript-file"></a>Adicionar conteúdo ao novo arquivo typescript de componente

Abra o arquivo ```{!module-name}.component.ts```, encontrado com a seguinte convenção de nomenclatura:

| Valor | Explicação | Exemplo de nome de arquivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal.component.ts``` |

Modifique o conteúdo do arquivo para o seguinte:

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### <a name="update-app-routingmodulets"></a>Atualizar app-Routing. Module. TS

Abra ```app-routing.module.ts``` o arquivo e modifique o caminho padrão para que ele carregue o novo módulo que você acabou de criar.  Localize a entrada para ```path: ''``` e atualize ```loadChildren``` para carregar seu módulo em vez do módulo padrão:

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | O nome do módulo (espaços removidos) | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal``` |

``` ts
    {
        path: '',
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
Aqui está um exemplo de um caminho padrão atualizado:
``` ts
    {
        path: '',
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## <a name="build-and-side-load-your-extension"></a>Compilar e carregar lado sua extensão

Agora você adicionou um módulo à sua extensão.  Em seguida, você pode [criar e carregar o lado](../develop-tool.md#build-and-side-load-your-extension) de sua extensão no centro de administração do Windows para ver os resultados.
