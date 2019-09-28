---
title: Migrar do SDK do centro de administração do Windows 0,1 para 1,0
description: Este guia irá ajudá-lo a migrar do SDK do Windows Admin Center versão 0,1 para 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c52870178e7caff0abc8ddcccc62966d637dd3c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357063"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Migrar do SDK do centro de administração do Windows 0,1 para 1,0

>Aplica-se a: Windows Admin Center Preview

Este guia ajudará você a migrar do SDK do centro de administração do Windows versão 0,1 para 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. Saiba mais sobre os novos controles com a extensão do guia de desenvolvimento

O centro de administração do Windows versão 1902 e posterior inclui a extensão do **Guia de desenvolvimento** , que pode ser usada para encontrar exemplos de controles (incluindo controles recentemente disponíveis) e cenários para ajudá-lo a criar sua própria extensão.  O guia do desenvolvedor substitui a extensão de **ferramentas para desenvolvedores** de versões anteriores do SDK.

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Usar o guia de desenvolvimento no centro de administração do Windows

O guia do desenvolvedor está disponível como uma solução no centro de administração do Windows versão 1902 e posterior.  O guia do desenvolvedor é pré-instalado, mas precisa ser habilitado por meio de configurações.

**Habilitar guia de desenvolvimento no centro de administração do Windows:**

* Abrir o centro de administração do Windows (versão 1902 e posterior)
* Clique no ícone de **configurações** no canto superior direito da janela
* Selecione a guia **avançado**
* Em *chaves de experimento*, clique em **Adicionar**
* Insira um novo valor ```msft.sme.shell.devguide``` no campo vazio que foi criado pela etapa anterior
* Clique em **salvar e recarregar**

**Abra o guia de desenvolvimento no centro de administração do Windows:**

* Abrir o centro de administração do Windows (versão 1902 e posterior)
* Clique na lista suspensa na parte superior esquerda para mostrar todos os tipos de solução
* Selecione a solução de **Guia de desenvolvimento** 
    * Se você não vir a solução listada, certifique-se de ter habilitado o guia de desenvolvimento (consulte a seção acima) e recarregou o centro de administração do Windows.
* Procure o conteúdo do guia do desenvolvedor selecionando uma das guias
    * **Da** Contém exemplos de código para cenários de *Gerenciamento* e de *notificação*
    * **Controles** Contém exemplos de cada controle disponível no SDK
    * **Especificadas** Contém exemplos de funções de conversor e formatador disponíveis
    * **Estilos** Contém exemplos de estilos CSS disponíveis no SDK
    * **MsftSme:** Contém exemplos e diretrizes para cenários avançados 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Procurar o código-fonte do guia do desenvolvedor no GitHub

Você pode procurar o [código-fonte](https://github.com/Microsoft/windows-admin-center-sdk/) do guia de desenvolvimento no GitHub para encontrar exemplos de código HTML, CSS e TypeScript.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. Preparar seu ambiente de desenvolvimento para o SDK mais recente

Instale ou atualize o Node. js versão [10.15.1 LTS ou posterior](https://nodejs.org/en/).

Atualize a CLI do centro de administração do Windows para a versão mais recente:

[//]: # "NPM desinstalar-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Atualize suas dependências globais para estas versões:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. Criar um novo projeto com o SDK mais recente

Use a CLI do centro de administração do Windows para criar um novo projeto direcionado para a versão ```next``` (SDK 1,0):

[//]: # "WAC Create--empresa ' Contoso Inc '--ferramenta ' gerenciar foo Works '--versão experimental"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Em seguida, altere o diretório para a pasta recém-criada e instale as dependências locais necessárias executando ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. Modificar um projeto existente para usar o SDK mais recente

IMPORTANTE: Faça um backup do seu projeto antes de continuar.

Modifique a linha a seguir em ```package.json``` para direcionar a versão ```next``` (SDK 1,0):

[//]: # "' @microsoft/windows-admin-center-sdk ': ' experimental '"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Em seguida, execute ```npm install``` para atualizar referências em todo o seu projeto.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Usar a CLI do SDK para corrigir problemas comuns de migração

IMPORTANTE: Faça um backup do seu projeto antes de continuar.

Na pasta raiz do seu projeto, execute o seguinte comando da CLI em seu projeto para corrigir problemas comuns de migração automaticamente:

``` cmd
wac updateSeven --update
```

Esse comando da CLI aborda os seguintes problemas automaticamente:

* Regenerar ```package-lock.json```
* Atualizar arquivos no ambiente de compilação angular:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Usar a CLI do SDK para entender problemas comuns de migração

Na pasta raiz do seu projeto, execute o seguinte comando da CLI para auditar seu projeto e encontrar problemas comuns de migração que precisam ser resolvidos manualmente:

``` cmd
wac updateSeven --audit
```

Isso encontrará instâncias dos seguintes problemas em seu projeto:

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Substitua o uso das seguintes classes CSS por estas classes de SME:

| Classe CSS antiga | Nova classe CSS |
| -- | -- |
| . Flex-tamanho automático |  . SME-posição-Flex-auto |
| . Border-todos |  . SME-Border-relevo-SM e. SME-border-color-base-90 |
| . borda-inferior |  . SME-border-bottom-SM e. SME-border-bottom-Color-base-90 |
| . Border-horizontal |  . SME-Border-horizontal-SM e. SME-Border-horizontal-Color-base-90 |
| . borda-esquerda |  . SME-border-left-SM e. SME-border-left-color-base-90 |
| . borda-direita |  . SME-border-right-SM e. SME-border-right-color-base-90 |
| . borda-superior |  . SME-border-top-SM e. SME-border-top-color-base-90 |
| . Border-vertical |  . SME-Border-vertical-SM e. SME-Border-vertical-Color-base-90 |
| . quebra de palavra |  . SME-organize-WS-Wrap |
| . btn |  . SME – botão ou botão |
| . btn-primário |  . SME-Button. SME-botão-primário ou. botão. SME-botão-primário |
| . cor-escuro |  . SME-cor-Alt |
| . cor-claro |  . SME-base de cores |
| . cor-claro-cinza |  . SME-cor-base-90 |
| . Fixed-Flex-size |  . SME-position-Flex-None |
| . Flex-layout |  . SME-organize-Stack-h ou. SME-organize-Stack-v |
| . fonte-negrito |  . SME-Font-emphasis1 |
| . realce |  . SME-background-cor-amarelo |
| . horizontal |  . SME-organize-Stack-h |
| . sem rolagem |  . SME-posição-Flex-auto |
| . NoWrap |  . SME-organize-Stack-h ou. SME-organize-Stack-v |
| . relativo |  . SME-layout-relativo |
| . relative-Center |  . SME-layout-absoluto. SME-position-Center |
| . Reverse |  . SME-organize-Stack-invertido |
| . Stretch-absoluta |  . SME-layout-absoluto. SME-position-relevo-nenhum |
| . Stretch – fixo |  . SME-layout-corrigido. SME-position--None |
| . Stretch-vertical |  . SME-posição-Stretch-v |
| . Stretch-largura |  . SME-posição-Stretch-h |
| . vertical |  . SME-organize-Stack-v |
| . vertical-somente rolagem |  . SME-organize-overflow-Hide-x SME-organize-overflow-auto-y |
| . Wrap |  . SME-Arrange-wrapstack-h ou. SME-Arrange-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Substitua o uso dos seguintes componentes por estes componentes de SME:

| Componente antigo | Novo componente |
| -- | -- |
| . alerta |  SME-alerta |
| . alerta-perigo |  SME-alerta |
| . breadCrumb |  SME-alerta |
| . CheckBox |  SME-formulário-campo [tipo = "caixa de seleção"] |
| . ComboBox |  SME-Form-Field [tipo = "selecionar"] |
| . Dashboard |  SME-layout-Content-Zone-preenchido SME-organize-Stack-h |
| . detalhes-painel |  SME-Property-Grid |
| . detalhes-painel-contêiner |  SME-Property-Grid |
| . detalhes-guia |  SME-Property-Grid ou SME-pivot |
| . detalhes-Wrapper |  SME-Property-Grid |
| . desabilitado |  SME-desabilitado |
| . formulário-botões | SME-formulário-campo |
| . formulário-controle | SME-formulário-campo |
| . formulário-controles | SME-formulário-campo |
| . formulário-grupo | SME-formulário-campo |
| . formulário-grupo-rótulo | SME-formulário-campo |
| . formulário-entrada | SME-formulário-campo |
| ampliação de formulário | SME-formulário-campo |
| . arquivo de entrada | SME-formulário-campo |
| . NAV – guias |  SME-pivô |
| . rádio |  SME-formulário-campo [tipo = "rádio"] |
| . obrigatório-pista | SME-formulário-campo |
| . SearchBox |  SME-Form-Field [tipo = "pesquisa"] |
| . toggle-switch |  SME-Form-Field [type = "alternância-switch"] |
| . ferramenta-contêiner |  SME-layout-Content-Zone ou SME-layout-Content-Zone-preenchido |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Essas classes CSS foram preteridas e não são mais suportadas:

| Classe antiga | Preterido |
| -- | -- |
| . aceitável | preterido |
| . cor-erro | preterido |
| . cor-informações | preterido |
| . cor-êxito | preterido |
| . cor-aviso | preterido |
| . Delete – botão | preterido |
| . detalhes-conteúdo | preterido |
| . erro-cobertura | preterido |
| . erro-mensagem | preterido |
| . guiado-botão | preterido |
| . Header-contêiner | preterido |
| . ícone-Win | preterido |
| . recuo | preterido |
| . inválido | preterido |
| . item-lista | preterido |
| . modal-rolável | preterido |
| . várias seções | preterido |
| . sem barra de ação | preterido |
| . margens de estouro | preterido |
| . overflow-ferramenta | preterido |
| . progresso-cobertura | preterido |
| . painel direito | preterido |
| . rollup | preterido |
| . rollup-status | preterido |
| . rollup-título | preterido |
| . rollup-valor | preterido |
| . SearchBox-barra de ação | preterido |
| . tamanho-h-1 | preterido |
| . tamanho-h-2 | preterido |
| . tamanho-h-3 | preterido |
| . tamanho-h-4 | preterido |
| . tamanho-h-completo | preterido |
| . tamanho-h-metade | preterido |
| . tamanho-v-1 | preterido |
| . tamanho-v-2 | preterido |
| . tamanho-v-3 | preterido |
| . tamanho-v-4 | preterido |
| . status-ícone | preterido |
| . svg-16px | preterido |
| . Table-recuo | preterido |
| . Table-SM | preterido |
| . fino | preterido |
| . bloco | preterido |
| . bloco-corpo | preterido |
| . bloco-conteúdo | preterido |
| . bloco-rodapé | preterido |
| . bloco-cabeçalho | preterido |
| . bloco-layout | preterido |
| . Tile-tabela | preterido |
| . barra de ferramentas | preterido |
| . barra de ferramentas | preterido |
| . ferramenta-cabeçalho | preterido |
| . ferramenta-cabeçalho-caixa | preterido |
| . ferramenta-painel | preterido |
| . Usage-bar | preterido |
| . Usage-área da barra | preterido |
| . Usage-barra-plano de fundo | preterido |
| . Usage-barra-título | preterido |
| . Usage-barra-valor | preterido |
| . Usage-gráfico | preterido |
| . Usage-mensagem | preterido |
| . Usage-área de mensagem | preterido |
| . Usage-mensagem-título | preterido |
| . aviso | preterido |
| . espaço em branco | preterido |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. Entender e resolver problemas com objetos observáveis

### <a name="update--rxjs-function-use-for-observable-objects"></a>Atualizar o uso da função ```rxjs``` para objetos observáveis

Esses são alguns nomes de funções comuns que foram alterados, podendo haver outros em seu projeto.

* Atualizar ```Observable.empty()``` para ```empty()```
* Atualizar ```Observable.of()``` para ```of()```
* Atualizar ```.switchMap()``` para ```.pipe(switchMap())```
* Atualizar ```.map()``` para ```.pipe(map())```
* Atualizar ```flatMap()``` para ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Resolver problemas de tempo de execução com funções ```.map()``` e ```.filter()``` em objetos observáveis

Se o compilador não puder identificar corretamente um tipo de objeto ```observable```, as funções ```.map()``` e ```.filter()``` do objeto ```array``` poderão ser mapeadas em vez de seu objeto, causando erros em tempo de execução.  Certifique-se de que suas funções retornem um objeto ```observable``` especificando um tipo de dados explícito para evitar esse problema.

```any``` e nenhum tipo de retorno pode causar esse problema, procure o código com estes padrões:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. Resolver outros problemas comuns

Essas técnicas ajudarão a resolver outros problemas comuns:

* Execute ```ng lint --fix``` para corrigir problemas comuns de fiapos
* Execute ```gulp build``` repetidamente para corrigir problemas incrementais que ```gulp build``` pode resolver automaticamente

## <a name="9-build-and-serve-your-project"></a>9. Crie e sirva seu projeto

Execute os seguintes comandos para compilar e fornecer seu projeto com a versão mais recente (SDK 1,0):

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. Ativar o tema escuro no centro de administração do Windows

Para ativar o tema escuro no centro de administração do Windows versão 1902 e posterior, siga estas etapas:

* Abrir o centro de administração do Windows (versão 1902 e posterior)
* Clique no ícone de **configurações** no canto superior direito da janela
* Selecione a guia **avançado**
* Em *chaves de experimento*, clique em **Adicionar**
* Insira um novo valor ```msft.sme.shell.personalization``` no campo vazio que foi criado pela etapa anterior
* Clique em **salvar e recarregar**
* Agora, as configurações terão uma nova guia, **personalização**.  Selecione esta guia
* Alterar **as cores** para o **modo escuro (visualização)**
