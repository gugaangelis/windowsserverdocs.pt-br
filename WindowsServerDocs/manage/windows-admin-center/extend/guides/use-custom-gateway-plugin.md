---
title: Usar um plug-in de gateway personalizado em sua extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do centro de administração do Windows (projeto Honolulu) – usar um plug-in de gateway personalizado em sua extensão de ferramenta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 5bcaaa452a2b42a54cbc3b1d8f9a296504054e34
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269223"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Usar um plug-in de gateway personalizado em sua extensão de ferramenta

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Neste artigo, usaremos um plug-in de gateway personalizado em uma nova extensão de ferramenta vazia que criamos com a CLI do centro de administração do Windows.

## <a name="prepare-your-environment"></a>Prepare o ambiente ##

Se você ainda não fez isso, siga as instruções em [desenvolver uma extensão de ferramenta](../develop-tool.md) para preparar seu ambiente e criar uma nova extensão de ferramenta vazia.

## <a name="add-a-module-to-your-project"></a>Adicionar um módulo ao seu projeto ##

Se você ainda não fez isso, adicione um novo [módulo vazio](add-module.md) ao projeto, que usaremos na próxima etapa.  

## <a name="add-integration-to-custom-gateway-plugin"></a>Adicionar integração ao plug-in de gateway personalizado ##

Agora, usaremos um plug-in de gateway personalizado no novo módulo vazio que acabamos de criar.

### <a name="create-pluginservicets"></a>Criar plugin. Service. TS

Altere para o diretório do novo módulo de ferramenta criado acima (```\src\app\{!Module-Name}```) e crie um novo arquivo ```plugin.service.ts```.

Adicione o seguinte código ao arquivo recém-criado:
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

> [!WARNING]
> É recomendável que o ```this.appContextService.node``` interno seja usado para chamar qualquer API que esteja definida em seu plug-in de gateway personalizado. Isso garantirá que, se as credenciais forem necessárias dentro do seu plug-in de gateway, elas serão tratadas corretamente.

### <a name="modify-modulets"></a>Modificar módulo. TS

Abra o arquivo ```module.ts``` do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.module.ts```):

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

### <a name="modify-componentts"></a>Modificar componente. TS

Abra o arquivo ```component.ts``` do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.component.ts```):

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

Modifique o construtor e modifique/adicione as seguintes funções:

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

### <a name="modify-componenthtml"></a>Modificar Component. html ###

Abra o arquivo ```component.html``` do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.component.html```):

Adicione o seguinte conteúdo ao arquivo HTML:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Compilar e carregar lado sua extensão

Agora você está pronto para [Compilar e carregar lado](../develop-tool.md#build-and-side-load-your-extension) sua extensão no centro de administração do Windows.
