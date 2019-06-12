---
title: Usar um plug-in de gateway personalizado em sua extensão de ferramenta
description: Desenvolver uma extensão da ferramenta (projeto Paulo) do SDK do Windows Admin Center – usar um plug-in de gateway personalizado em sua extensão da ferramenta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 348ebf5b99de7f582a3edf57b0a190f87f1c4a5b
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452604"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Usar um plug-in de gateway personalizado em sua extensão de ferramenta

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Neste artigo, usaremos um plug-in de gateway personalizado em uma extensão de ferramenta nova e vazia que criamos com a CLI do Windows Admin Center.

## <a name="prepare-your-environment"></a>Prepare o ambiente ##

Se você ainda não fez isso, siga as instruções em [desenvolver uma extensão da ferramenta](../develop-tool.md) para preparar seu ambiente e criar um novo, vazio extensão da ferramenta.

## <a name="add-a-module-to-your-project"></a>Adicionar um módulo ao seu projeto ##

Se você ainda não fez isso, adicione um novo [módulo vazio](add-module.md) ao seu projeto, que usaremos na próxima etapa.  

## <a name="add-integration-to-custom-gateway-plugin"></a>Adicionar integração ao plug-in do gateway personalizado ##

Agora vamos usar um plug-in de gateway personalizado no módulo novo e vazio que acabamos de criar.

### <a name="create-pluginservicets"></a>Criar plugin.service.ts

Altere o diretório do novo módulo de ferramenta criado acima (```\src\app\{!Module-Name}```) e crie um novo arquivo ```plugin.service.ts```.

Adicione o seguinte código para o arquivo que acabou de criar:
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

Alterar referências aos ```Sample Uno``` e ```Sample%20Uno``` para o seu nome de recurso conforme apropriado.

[!WARNING]
> É recomendável que internos no ```this.appContextService.node``` é usada para chamar qualquer API que é definido no seu plug-in de gateway personalizado. Isso garantirá que se as credenciais são necessárias dentro de seu plug-in de gateway que eles serão manipulados corretamente.

### <a name="modify-modulets"></a>Modificar Module

Abra o ```module.ts``` arquivo do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.module.ts```):

Adicione as seguintes instruções de importação:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Adicione os seguintes provedores (após as declarações):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>Modificar component.ts

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

### <a name="modify-componenthtml"></a>Modificar component.html ###

Abra o ```component.html``` arquivo do novo módulo criado anteriormente (ou seja, ```{!Module-Name}.component.html```):

Adicione o seguinte conteúdo para o arquivo html:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Compilação e o lado carregam sua extensão

Agora você está pronto para [compilação e do lado do carregamento](../develop-tool.md#build-and-side-load-your-extension) sua extensão no Windows Admin Center.
