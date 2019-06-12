---
title: Desenvolver uma extensão de ferramenta
description: Desenvolver uma extensão da ferramenta Windows Admin Center SDK (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1a068c0d33887e8e9287ff15c1aa14f3dc84915a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445937"
---
# <a name="install-extension-payload-on-a-managed-node"></a>Instalar a carga de extensão em um nó gerenciado

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

## <a name="setup"></a>Configuração
> [!NOTE]
> Para seguir este guia, você precisa criar 1.2.1904.02001 ou superior. Para verificar seu build número abrir Windows Admin Center e clique no ponto de interrogação no canto superior direito.

Se você ainda não fez isso, crie uma [extensão de ferramentas](../develop-tool.md) para Windows Admin Center. Depois de concluir esta marca Observação dos valores usados durante a criação de uma extensão:

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome da sua empresa (com espaços) | ```Contoso``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```InstallOnNode``` |

Dentro de sua pasta de extensão da ferramenta criar uma ```Node``` pasta (```{!Tool Name}\Node```). Qualquer coisa colocados nessa pasta será copiada para o nó gerenciado ao usar essa API. Adicione os arquivos necessários para seu caso de uso. 

Crie também um ```{!Tool Name}\Node\installNode.ps1``` script. Esse script será executado no nó gerenciado depois que todos os arquivos são copiados do ```{!Tool Name}\Node``` pasta para o nó gerenciado. Adicione nenhuma lógica adicional para seu caso de uso. Um exemplo ```{!Tool Name}\Node\installNode.ps1``` arquivo:

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` tem um nome específico que a API irá procurar. Alterar o nome deste arquivo fará com que um erro.


## <a name="integration-with-ui"></a>Integração com a interface do usuário

Atualização ```\src\app\default.component.ts``` ao seguinte:

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
Atualize os espaços reservados para valores que foram usados ao criar a extensão:
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

Também atualizar ```\src\app\default.component.html``` para:
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
Finalmente, ```\src\app\default.module.ts```:
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

## <a name="creating-and-installing-a-nuget-package"></a>Criando e instalando um pacote do NuGet

A última etapa é criar um pacote do NuGet com os arquivos que adicionamos e, em seguida, instalar esse pacote em Windows Admin Center.

Siga as [extensões publicação](../publish-extensions.md) guia se você não tiver criado um pacote de extensão antes. 
> [!IMPORTANT]
> Em seu arquivo. NuSpec para essa extensão, é importante que o ```<id>``` valor corresponde ao nome no seu projeto ```manifest.json``` e o ```<version>``` coincide com o que foi adicionado ao ```\src\app\default.component.ts```. Também adicionar uma entrada em ```<files>```: 
> 
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

Depois que esse pacote é criado, adicione um caminho para o feed. No Windows Admin Center, vá para Configurações > extensões > Feeds e adicione o caminho para onde o pacote existe. Quando terminar a sua extensão que está sendo instalado, você deve ser capaz de clicar o ```install``` botão e a API serão chamado.  