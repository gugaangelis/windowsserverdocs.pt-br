---
title: Usar um plug-in de gateway personalizado em sua extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do Windows Admin Center (Project Honolulu) - usar um plug-in de gateway personalizado em sua extensão de ferramenta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4652616478b7b05bde97db48bf84648984b5a325
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296758"
---
# Usar um plug-in de gateway personalizado em sua extensão de ferramenta

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Neste artigo, vamos usar um plug-in de gateway personalizado em uma extensão de ferramenta nova e vazia que criamos com a CLI do Windows Admin Center.

## Preparar o ambiente ##

Se você ainda não fez isto, siga as instruções em [desenvolver uma extensão de ferramenta](..\develop-tool.md) para preparar seu ambiente e criar uma extensão de ferramenta nova e vazia.

## Adicionar um módulo ao seu projeto ##

Se você ainda não fez isto, adicione um novo [módulo vazio](add-module.md) ao seu projeto, que usaremos na próxima etapa.  

## Adicionar integração ao plug-in de gateway personalizado ##

Agora vamos usar um plug-in de gateway personalizado no módulo novo e vazio que acabamos de criar.

### Criar plugin.service.ts

Vá para o diretório do novo módulo ferramenta criado acima (```\src\app\{!Module-Name}```) e criar um novo arquivo ```plugin.service.ts```.

Adicione o código a seguir ao arquivo recém criado:
``` ts
import { Injectable } from '@angular/core';
import { AppContextService, HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Cim, Http, PowerShell, PowerShellSession } from '@microsoft/windows-admin-center-sdk/core';
import { AjaxResponse, Observable } from 'rxjs';

@Injectable()
export class PluginService {
    constructor(private appContextService: AppContextService, private http: Http) {
    }
    
    public getGatewayRestResponse(): Observable<any> {
        let callUrl = this.appContextService.activeConnection.nodeName;

        return this.appContextService.node.get(callUrl, 'features/Sample%20Uno').map(
            (response: any) => {
                return response;
            }
        )
    }
}
```

Altere as referências a ```Sample Uno``` e ```Sample%20Uno``` ao nome do recurso conforme apropriado.

[!WARNING]
> É recomendável que interna em ```this.appContextService.node``` é usado para chamar qualquer API que é definido em seu plug-in de gateway personalizado. Isso garantirá que se as credenciais são necessárias dentro de seu gateway plug-in que eles serão manipulados corretamente.

### Modificar module.ts

Abra o ```module.ts``` arquivo do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.module.ts```):

Adicione as seguintes instruções de importação:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Adicione os seguintes provedores (após declarações):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### Modificar component.ts

Abra o ```component.ts``` arquivo do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.component.ts```):

Adicione as seguintes instruções de importação:

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

Adicione as seguintes variáveis:

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

Modifique o construtor e modificar ou adicionar as seguintes funções:

``` ts
  constructor(private appContextService: AppContextService, private plugin: PluginService) {
    //
  }

  public ngOnInit() {
    this.responseResult = 'click go to do something';
  }

  public onClick() {
    this.serviceSubscription = this.plugin.getGatewayRestResponse().subscribe(
      (response: any) => {
        this.responseResult = 'response: ' + response.message;
      },
      (error) => {
        console.log(error);
      }
    );
  }
```

### Modificar component.html ###

Abra o ```component.html``` arquivo do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.component.html```):

Adicione o seguinte conteúdo para o arquivo html:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## Compilação e lado carregam sua extensão

Agora você está pronto para a [carga de compilação e lado](..\develop-tool.md#build-and-side-load-your-extension) sua extensão no Windows Admin Center.
