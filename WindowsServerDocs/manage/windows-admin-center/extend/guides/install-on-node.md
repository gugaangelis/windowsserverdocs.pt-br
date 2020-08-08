---
title: Instalar a carga de extensão em um nó gerenciado
description: Instruções sobre como instalar a carga de extensão em um nó gerenciado
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6a8675ff481f908feeeef4f97be0f1889b117291
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944968"
---
# <a name="install-extension-payload-on-a-managed-node"></a>Instalar a carga de extensão em um nó gerenciado

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

## <a name="setup"></a>Instalação

> [!NOTE]
> Para seguir este guia, você precisará de Build 1.2.1904.02001 ou superior. Para verificar seu número de Build, abra o centro de administração do Windows e clique no ponto de interrogação no canto superior direito.

Se você ainda não fez isso, crie uma [extensão de ferramenta](../develop-tool.md) para o centro de administração do Windows. Depois de concluir isso, anote os valores usados ao criar uma extensão:

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da sua empresa (com espaços) | ```Contoso``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```InstallOnNode``` |

Dentro de sua pasta de extensão de ferramenta, crie uma ```Node``` pasta ( ```{!Tool Name}\Node``` ). Qualquer coisa colocada nesta pasta será copiada para o nó gerenciado ao usar essa API. Adicione todos os arquivos necessários para seu caso de uso.

Crie também um ```{!Tool Name}\Node\installNode.ps1``` script. Esse script será executado no nó gerenciado depois que todos os arquivos forem copiados da ```{!Tool Name}\Node``` pasta para o nó gerenciado. Adicione qualquer lógica adicional para seu caso de uso. Um arquivo de exemplo ```{!Tool Name}\Node\installNode.ps1``` :

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1```tem um nome específico que a API procurará. A alteração do nome desse arquivo causará um erro.


## <a name="integration-with-ui"></a>Integração com a interface do usuário

Atualize ```\src\app\default.component.ts``` para o seguinte:

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
Atualizar espaços reservados para valores que foram usados ao criar a extensão:
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
E, por fim, ```\src\app\default.module.ts``` :
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

## <a name="creating-and-installing-a-nuget-package"></a>Criando e instalando um pacote NuGet

A última etapa é criar um pacote NuGet com os arquivos que adicionamos e, em seguida, instalando esse pacote no centro de administração do Windows.

Siga o guia de [extensões de publicação](../publish-extensions.md) se você não tiver criado um pacote de extensão antes.
> [!IMPORTANT]
> No seu arquivo. nuspec para essa extensão, é importante que o ```<id>``` valor corresponda ao nome no seu projeto ```manifest.json``` e ```<version>``` que corresponda ao que foi adicionado ```\src\app\default.component.ts``` . Adicione também uma entrada em ```<files>``` :
>
> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="https://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
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

Depois que esse pacote for criado, adicione um caminho para esse feed. No centro de administração do Windows, acesse configurações > extensões > feeds e adicione o caminho para onde o pacote existe. Quando a extensão for concluída, você poderá clicar no ```install``` botão e a API será chamada.