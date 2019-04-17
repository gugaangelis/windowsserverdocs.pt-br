---
title: Adicionar um módulo a uma extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do Windows Admin Center (Project Honolulu) - adicionar um módulo a uma extensão de ferramenta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e6978ce20a7c6da8addb217de8d30f733b40d261
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081203"
---
# Adicionar um módulo a uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Neste artigo, vamos adicionar um módulo vazio com uma extensão de ferramenta que criamos com a CLI do Windows Admin Center.

## Preparar o ambiente

Se você ainda não fez, siga as instruções em desenvolvem uma extensão de [ferramenta](..\develop-tool.md) (ou [solução](..\develop-solution.md)) para preparar seu ambiente e criar uma extensão de ferramenta nova e vazia.

## Usar a CLI Angular para criar um módulo (e o componente)

Se você ainda não conhece a angulares, é altamente recomendável que você leia a documentação sobre o site Angular.Io para conhecer angulares e NgModule. Para obter mais informações sobre NgModule, acesse:https://angular.io/guide/ngmodule

* Mais informações sobre como gerar um novo módulo no CLI angular: https://github.com/angular/angular-cli/wiki/generate-module
* Mais informações sobre como gerar um novo componente no CLI angular: https://github.com/angular/angular-cli/wiki/generate-component


Abra um prompt de comando, altere o diretório para \src\app em seu projeto e execute os seguintes comandos, substituindo ```{!ModuleName}``` com o nome do módulo (espaços removidos):

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


## Adicionar informações de roteamento

Se você ainda não conhece o Angular, é altamente recomendável que aprender sobre o Roteamento e a navegação angular. As seções a seguir definem os elementos de roteamento necessários que permitem o Windows Admin Center navegar até sua extensão e entre os modos de exibição dela em resposta a atividade do usuário. Para saber mais, acesse:https://angular.io/guide/router

Use o mesmo nome do módulo que você usou na etapa acima.

### Adicionar conteúdo ao novo arquivo de roteamento

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

### Adicionar conteúdo ao novo arquivo de módulo

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

### Adicionar conteúdo ao novo arquivo de typescript do componente

Abra o arquivo ```{!module-name}.component.ts```, encontrado com a seguinte convenção de nomenclatura:

| Valor | Explicação | Exemplo de nome de arquivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | O nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal.component.ts``` |
    
Modifique o conteúdo do arquivo para o seguinte:

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### Atualizar o aplicativo routing.module.ts

Abra o arquivo ```app-routing.module.ts```e modificar o caminho padrão para que ele carregará o novo módulo que você acabou de criar.  Encontre a entrada para ```path: ''```e atualizar ```loadChildren``` para carregar o módulo em vez do módulo padrão:

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


## Compilação e lado carregam sua extensão

Agora você adicionou um módulo até sua extensão.  Em seguida, você pode [carga de compilação e lado](..\develop-tool.md#build-and-side-load-your-extension) sua extensão no Centro de administração do Windows para ver os resultados.
