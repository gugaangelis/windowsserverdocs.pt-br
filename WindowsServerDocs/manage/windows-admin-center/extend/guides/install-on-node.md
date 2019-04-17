---
title: Desenvolver uma extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 2269a2ac2cabda6f8fdd829994f36e89d581bd23
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296849"
---
# Instale a carga de extensão em um nó gerenciado

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

## Configuração
> [!NOTE]
> Para seguir este guia, você precisa criará 1.2.1904.02001 ou superior. Para verificar seu build número abra o Windows Admin Center e clique no ponto de interrogação no canto superior direito.

Se você ainda não fez isto, crie uma [extensão de ferramenta](../develop-tool.md) para o Windows Admin Center. Depois de concluir essa torna anote os valores usados ao criar uma extensão:
| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da empresa (com espaços) | ```Contoso``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```InstallOnNode``` |

Dentro de sua pasta de extensão de ferramenta, crie um ```Node``` pasta (```{!Tool Name}\Node```). Qualquer coisa colocada nessa pasta será copiada para o nó gerenciado ao usar essa API. Adicione os arquivos necessários para seu caso de uso. 

Também criar um ```{!Tool Name}\Node\installNode.ps1``` script. Esse script será executado no nó gerenciado depois que todos os arquivos serão copiados do ```{!Tool Name}\Node``` pasta ao nó gerenciado. Adicione qualquer lógica adicional para seu caso de uso. Um exemplo ```{!Tool Name}\Node\installNode.ps1``` arquivo:

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` tem um nome específico que a API procurará. Alterar o nome desse arquivo causará um erro.


## Integração com a interface do usuário

Atualização ```\src\app\default.component.ts``` para o seguinte:

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
Atualize espaços reservados para os valores que foram usados ao criar a extensão:
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

Também atualize ```\src\app\default.component.html``` para:
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
E finalmente ```\src\app\default.module.ts```:
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## Criação e instalação de um pacote NuGet

A última etapa é criar um pacote NuGet com os arquivos que adicionamos e, em seguida, instalar esse pacote no Centro de administração do Windows.

Se você não tiver criado um pacote de extensão antes, siga o guia [Extensões de publicação](../publish-extensions.md) . 
> [!IMPORTANT]
> No seu arquivo .nuspec para essa extensão, é importante que o ```<id>``` valor corresponde ao nome no seu projeto ```manifest.json``` e o ```<version>``` corresponda ao que foi adicionado à ```\src\app\default.component.ts```. Além disso, adicione uma entrada em ```<files>```: 

> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

Depois que esse pacote é criado, adicione um caminho para o feed. No Windows Admin Center, vá para Configurações gt _ extensões gt _ Feeds e adicionar o caminho para onde esse pacote existe. Quando terminar a sua extensão que está sendo instalado, você poderá clicar o ```install``` botão e a API serão chamado.  