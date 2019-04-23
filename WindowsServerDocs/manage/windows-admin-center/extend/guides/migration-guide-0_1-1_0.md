---
title: Migrar do SDK do Windows Admin Center 0,1 a 1,0
description: Este guia ajudará você a migrar do Windows Admin Center SDK versão 0.1 para 1.0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886467"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Migrar do SDK do Windows Admin Center 0,1 a 1,0

>Aplica-se a: Windows Admin Center Preview

Este guia ajudará você a migrar do Windows Admin Center SDK versão 0.1 a 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. Saiba mais sobre os novos controles com a extensão do guia de desenvolvimento

Windows Admin Center versão 1902 e posterior inclui o **guia de desenvolvimento** extensão, que você pode usar para localizar exemplos de cenários para ajudá-lo a criar sua própria extensão e controles (inclusive controles recentemente disponíveis).  Guia de desenvolvimento substitui o **ferramentas de desenvolvedor** extensão de versões anteriores do SDK.

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Usar o guia de desenvolvimento no Windows Admin Center

Guia de desenvolvimento está disponível como uma solução na versão Windows Admin Center 1902 e posterior.  Guia de desenvolvimento é pré-instalado, mas ele precisa ser habilitado por meio das configurações.

**Habilite o guia de desenvolvimento em Windows Admin Center:**

* Abrir Windows Admin Center (versão 1902 e posterior)
* Clique no **configurações** ícone no canto superior direito da janela
* Selecione o **avançado** guia
* Sob *chaves de experimento*, clique em **adicionar**
* Insira um novo valor ```msft.sme.shell.devguide``` no campo vazio que foi criado pela etapa anterior
* Clique em **salvar e recarregar**

**Abra a guia de desenvolvimento no Windows Admin Center:**

* Abrir Windows Admin Center (versão 1902 e posterior)
* Clique na lista suspensa na parte superior esquerda para mostrar todos os tipos de solução
* Selecione o **guia de desenvolvimento** solução 
    * Se você não vir a solução listada, verifique se você tiver habilitado o guia de desenvolvimento (consulte a seção acima) e ter recarregado Windows Admin Center.
* Procurar o conteúdo do guia de desenvolvimento, selecionando uma das guias
    * **Inicial:** Contém exemplos de código para *gerenciar como* e *notificação* cenários
    * **Controles:** Contém exemplos de cada controle disponível no SDK
    * **Pipes:** Contém exemplos de funções de conversor e formatador disponíveis
    * **Estilos:** Contém exemplos de estilos CSS disponíveis no SDK
    * **MsftSme:** Contém exemplos e diretrizes para cenários avançados 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Procurar o código-fonte do guia de desenvolvimento no GitHub

Você pode procurar o [código-fonte](https://github.com/Microsoft/windows-admin-center-sdk/) do guia de desenvolvimento no GitHub para encontrar exemplos de código do TypeScript, CSS e HTML de exemplo.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. Preparar o ambiente de desenvolvimento para o SDK mais recente

Instalar ou atualizar a versão do Node. js [10.15.1 LTS ou posterior](https://nodejs.org/en/).

Atualize a CLI do Windows Admin Center para a versão mais recente:

[//]: # "npm uninstall -g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Atualize as suas dependências globais para essas versões:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. Criar um novo projeto com o SDK mais recente

Usar a CLI do Windows Admin Center para criar um novo projeto como destino o ```next``` versão (1.0 SDK):

[//]: # "wac create--'Contoso Inc –' ferramenta 'Gerenciar Foo Works' – versão experimental da empresa"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Em seguida, altere o diretório para a pasta que acabou de criar e, em seguida, instale as dependências necessárias locais executando ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. Modificar um projeto existente para usar o SDK mais recente

IMPORTANTE: Faça um backup do seu projeto antes de continuar.

Modifique a seguinte linha na ```package.json``` para o destino de ```next``` versão (1.0 SDK):

[//]: # "'@microsoft/windows-admin-center-sdk': 'experimental'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Em seguida, execute ```npm install``` para atualizar referências em todo o projeto.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Usar a CLI do SDK para corrigir problemas comuns de migração

IMPORTANTE: Faça um backup do seu projeto antes de continuar.

Na pasta raiz do seu projeto, execute o seguinte comando CLI no seu projeto para corrigir problemas comuns na migração automaticamente:

``` cmd
wac updateSeven --update
```

Este comando da CLI aborda os seguintes problemas automaticamente:

* Regenerar ```package-lock.json```
* Arquivos de atualização no ambiente de compilação do angular:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Usar a CLI do SDK para compreender os problemas comuns na migração

Na pasta raiz do seu projeto, execute o seguinte comando CLI para seu projeto de auditoria e encontrar problemas de migração comuns que precisam ser corrigidos manualmente:

``` cmd
wac updateSeven --audit
```

Isso localizará instâncias dos problemas a seguir em seu projeto:

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Substitua o uso das seguintes classes CSS com essas classes de sme:

| Classe antiga do CSS | Nova classe CSS |
| -- | -- |
| .auto-flex-size |  .sme-position-flex-auto |
| .border-all |  .SME-borda baixo-relevo sm e .sme-borda-cor-base-90 |
| .border-bottom |  .SME-borda inferior do sm e .sme-border-bottom-color-base-90 |
| .border-horizontal |  .SME-borda horizontal sm e .sme-border-horizontal-color-base-90 |
| .border-left |  .SME-border-left-sm e .sme-border-left-color-base-90 |
| .border-right |  .SME-borda direita do sm e .sme-border-right-color-base-90 |
| .border-top |  .SME-border-top-sm e .sme-border-top-color-base-90 |
| .border-vertical |  .SME-borda vertical sm e .sme-border-vertical-color-base-90 |
| .break-word |  .sme-arrange-ws-wrap |
| .btn |  botão .sme botão OR |
| .btn primário |  .SME button.sme-botão primário OR.button.sme-botão-primário |
| .color-dark |  .sme-color-alt |
| .color-light |  .sme-color-base |
| .color-light-gray |  .sme-color-base-90 |
| .fixed-flex-size |  .sme-position-flex-none |
| .flex-layout |  .sme-arrange-stack-h OR .sme-arrange-stack-v |
| .font-bold |  .sme-font-emphasis1 |
| .Highlight |  .sme-background-color-yellow |
| .horizontal |  .sme-arrange-stack-h |
| Nenhuma rolagem |  .sme-position-flex-auto |
| .nowrap |  .sme-arrange-stack-h OR .sme-arrange-stack-v |
| .Relative |  .sme-layout-relative |
| .relative-center |  .sme-layout-absolute .sme-position-center |
| .reverse |  .sme-arrange-stack-reversed |
| .stretch-absolute |  .sme-layout-absolute .sme-position-inset-none |
| .stretch-fixed |  .SME-layout fixo .sme-posição-baixo-relevo-none |
| .stretch-vertical |  .sme-position-stretch-v |
| .stretch-width |  .sme-position-stretch-h |
| .vertical |  .sme-arrange-stack-v |
| .vertical-scroll-only |  .SME-organizar-estouro-ocultar-x sme-organizar-overflow-auto-y |
| .wrap |  .sme-arrange-wrapstack-h OR .sme-arrange-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Substitua o uso dos seguintes componentes com esses componentes de sme:

| Componente antigo | Novo componente |
| -- | -- |
| .alert |  sme-alert |
| perigo .alert |  sme-alert |
| .breadCrumb |  sme-alert |
| .CheckBox |  sme-form-field[type="checkbox"] |
| .combobox |  sme-form-field[type="select"] |
| .dashboard |  sme-layout-content-zone-padded sme-arrange-stack-h |
| painel .details |  sme-property-grid |
| .details-panel-container |  sme-property-grid |
| .details-tab |  grade de propriedade de SME OR sme-pivot |
| .details-wrapper |  sme-property-grid |
| .disabled |  o SME desabilitado |
| botões. Form | campo de formulário de SME |
| .form-control | campo de formulário de SME |
| controles de. Form | campo de formulário de SME |
| .form-group | campo de formulário de SME |
| .form-group-label | campo de formulário de SME |
| .form-input | campo de formulário de SME |
| .form-stretch | campo de formulário de SME |
| .input-file | campo de formulário de SME |
| .nav-tabs |  pivot SME |
| .Radio |  sme-form-field[type="radio"] |
| .required-clue | campo de formulário de SME |
| .searchbox |  sme-form-field[type="search"] |
| comutador .toggle |  sme-form-field[type="toggle-switch"] |
| .tool-container |  SME-layout-zona de conteúdo ou o sme-layout-conteúdo-zona-preenchida |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Essas classes CSS são preteridos e não têm mais suporte:

| Classe antiga | Preterido |
| -- | -- |
| .Acceptable | (preterido) |
| .color-error | (preterido) |
| .color-info | (preterido) |
| .color-success | (preterido) |
| .color-warning | (preterido) |
| .delete-button | (preterido) |
| .details-content | (preterido) |
| .Error rosto | (preterido) |
| .error-message | (preterido) |
| botão do painel de .guided | (preterido) |
| .header-container | (preterido) |
| .icon-win | (preterido) |
| .indent | (preterido) |
| .invalid | (preterido) |
| .item-list | (preterido) |
| .modal-scrollable | (preterido) |
| . a seção | (preterido) |
| nenhuma barra de ação | (preterido) |
| .overflow-margins | (preterido) |
| .overflow-tool | (preterido) |
| .progress-cover | (preterido) |
| painel .right | (preterido) |
| .Rollup | (preterido) |
| .rollup-status | (preterido) |
| título .rollup | (preterido) |
| valor .rollup | (preterido) |
| .searchbox-action-bar | (preterido) |
| .size-h-1 | (preterido) |
| .size-h-2 | (preterido) |
| .size-h-3 | (preterido) |
| .size-h-4 | (preterido) |
| .size-h-full | (preterido) |
| .size-h-half | (preterido) |
| .size-v-1 | (preterido) |
| .size-v-2 | (preterido) |
| .size-v-3 | (preterido) |
| .size-v-4 | (preterido) |
| .status-icon | (preterido) |
| .svg-16px | (preterido) |
| .table-indent | (preterido) |
| .table-sm | (preterido) |
| .thin | (preterido) |
| .Tile | (preterido) |
| .Tile-corpo | (preterido) |
| .tile-content | (preterido) |
| rodapé .tile | (preterido) |
| .tile-header | (preterido) |
| .tile-layout | (preterido) |
| .tile-table | (preterido) |
| .ToolBar | (preterido) |
| .tool-bar | (preterido) |
| .tool-header | (preterido) |
| .tool-header-box | (preterido) |
| painel .tool | (preterido) |
| barra de. Usage | (preterido) |
| .usage-bar-area | (preterido) |
| .usage-bar-background | (preterido) |
| .usage-bar-title | (preterido) |
| .usage-bar-value | (preterido) |
| .usage-chart | (preterido) |
| .usage-message | (preterido) |
| .usage-message-area | (preterido) |
| .usage-message-title | (preterido) |
| .warning | (preterido) |
| .white-space | (preterido) |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. Entender e resolver problemas com objetos observáveis

### <a name="update--rxjs-function-use-for-observable-objects"></a>Atualização ```rxjs``` uso para objetos observáveis de função

Esses são alguns nomes de função comuns que foram alterados, pode haver outros usuários em seu projeto.

* Atualização ```Observable.empty()``` para ```empty()```
* Atualização ```Observable.of()``` para ```of()```
* Atualização ```.switchMap()``` para ```.pipe(switchMap())```
* Atualização ```.map()``` para ```.pipe(map())```
* Atualização ```flatMap()``` para ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Resolver problemas de tempo de execução com ```.map()``` e ```.filter()``` funções em objetos observáveis

Se o compilador não pode identificar corretamente uma ```observable``` tipo do objeto, ```.map()``` e ```.filter()``` funções do ```array``` objeto pode ser mapeado em vez disso, para seu objeto, causando erros em tempo de execução.  Certifique-se de que suas funções retornam um ```observable``` objeto especificando um tipo de dados explícito para evitar esse problema.

```any``` e nenhum tipo de retorno pode causar esse problema, procure o código com esses padrões:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. Resolver outros problemas comuns

Essas técnicas o ajudarão a resolver outros problemas comuns:

* Execute ```ng lint --fix``` para corrigir problemas comuns de lint
* Execute ```gulp build``` repetidamente para incrementalmente corrigir problemas que ```gulp build``` pode resolver automaticamente

## <a name="9-build-and-serve-your-project"></a>9. Criar e fornecer o seu projeto

Execute os seguintes comandos para criar e fornecer o seu projeto com a versão mais recente (SDK 1.0):

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. Ativar o tema no escuro no Windows Admin Center

Para ativar o tema no escuro no Windows Admin Center versão 1902 e posterior, siga estas etapas:

* Abrir Windows Admin Center (versão 1902 e posterior)
* Clique no **configurações** ícone no canto superior direito da janela
* Selecione o **avançado** guia
* Sob *chaves de experimento*, clique em **adicionar**
* Insira um novo valor ```msft.sme.shell.personalization``` no campo vazio que foi criado pela etapa anterior
* Clique em **salvar e recarregar**
* Configurações agora terá uma nova guia, **personalização**.  Selecione este guia
* Alteração **cores** para **modo escuro (visualização)**
