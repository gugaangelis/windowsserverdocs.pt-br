---
title: Migrar do SDK do Windows Admin Center 0,1 a 1.0
description: Este guia ajudará você a migrar do Windows Admin Center SDK versão 0,1 a 1.0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 10d7606a5c2a1ddb234af597f199b6779b0058d4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116579"
---
# Migrar do SDK do Windows Admin Center 0,1 a 1.0

>Aplicável a: Windows Admin Center Preview

Este guia ajudará você a migrar do Windows Admin Center SDK versão 0,1 a 1.0.  

## 1. Saiba mais sobre novos controles com a extensão do guia de desenvolvimento

Windows Admin Center versão 1902 e posterior inclui a extensão do **Guia de desenvolvimento** , que você pode usar para encontrar exemplos de controles (incluindo controles disponíveis recentemente) e cenários para ajudar você a criar sua própria extensão.  Guia de desenvolvimento substitui a extensão de **Ferramentas de desenvolvedor** de versões anteriores do SDK.

### Use o guia de desenvolvimento no Centro de administração do Windows

Guia de desenvolvimento está disponível como uma solução no Windows Admin Center versão 1902 e posterior.  Guia de desenvolvimento é pré-instalado, mas ele precisa ser ativado por meio das configurações.

**Habilite o guia de desenvolvimento no Centro de administração do Windows:**

* Abrir o Windows Admin Center (versão 1902 e posterior)
* Clique no ícone de **configurações** no canto superior direito da janela
* Selecione a guia **Avançado**
* Em um *Experimento chaves*, clique em **Adicionar**
* Insira um novo valor ```msft.sme.shell.devguide``` no campo vazio que foi criado pelo etapa anterior
* Clique em **Salvar e recarregar**

**Abra o guia de desenvolvimento no Centro de administração do Windows:**

* Abrir o Windows Admin Center (versão 1902 e posterior)
* Clique na lista suspensa na parte superior esquerda para mostrar todos os tipos de solução
* Selecione a solução do **Guia de desenvolvimento** 
    * Se você não vir a solução listada, verifique se você habilitou o guia de desenvolvimento (consulte a seção acima) e têm recarregado Windows Admin Center.
* Navegar no conteúdo do guia de desenvolvimento, selecionando uma das guias
    * **Inicial:** Contém exemplos de código para cenários de *Gerenciar como* e *notificação*
    * **Controles:** Contém exemplos de cada controle disponível no SDK
    * **Pipes:** Contém exemplos de funções de conversor e formatador disponíveis
    * **Estilos:** Contém exemplos de estilos CSS disponíveis no SDK
    * **MsftSme:** Contém exemplos e diretrizes para cenários avançados 

### Procure o código-fonte do guia de desenvolvimento no GitHub

Você pode procurar o [código-fonte](https://github.com/Microsoft/windows-admin-center-sdk/) do guia de desenvolvimento no GitHub localizar HTML, CSS e TypeScript exemplos de código de exemplo.

## 2. preparar o ambiente de desenvolvimento para o SDK mais recente

Instalar ou atualizar a versão do Node. js [10.15.1 LTS ou posterior](https://nodejs.org/en/).

Atualize a CLI do Windows Admin Center para a versão mais recente:

[//]: # "NPM desinstalar windows-admin-center-cli@next -g"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Atualize suas dependências globais para essas versões:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## 3. criar um novo projeto com o SDK mais recente

Usar a CLI do Windows Admin Center para criar um novo projeto direcionando a ```next``` versão (1.0 SDK):

[//]: # "Criar wac – 'Contoso Inc' – ferramenta 'Gerenciar Foo funciona' – versão experimental da empresa"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Em seguida, mude o diretório para a pasta que acabou de criar e, em seguida, instale as dependências de local necessárias executando ```npm install ```.

## 4. modificar um projeto existente para usar o SDK mais recente

IMPORTANTE: Faça um backup do seu projeto antes de continuar.

Modifique a seguinte linha no ```package.json``` destino a ```next``` versão (1.0 SDK):

[//]: # "'@microsoft/windows-admin-center-sdk': 'experimental'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Em seguida, execute ```npm install``` atualizar referências em todo o seu projeto.

## 5. usar a CLI do SDK para corrigir problemas comuns de migração

IMPORTANTE: Faça um backup do seu projeto antes de continuar.

Na pasta raiz do seu projeto, execute o seguinte comando CLI no seu projeto para corrigir problemas comuns de migração automaticamente:

``` cmd
wac updateSeven --update
```

Esse comando CLI aborda os seguintes problemas automaticamente:

* Gerar novamente ```package-lock.json```
* Arquivos de atualização no ambiente de compilação angular:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## 6. usar a CLI do SDK para compreender os problemas comuns de migração

Na pasta raiz do seu projeto, execute o seguinte comando CLI seu projeto de auditoria e encontrar problemas comuns de migração que precisem ser abordados manualmente:

``` cmd
wac updateSeven --audit
```

Isso será possível encontrar instâncias dos seguintes problemas em seu projeto:

### Substitua o uso das seguintes classes CSS com essas classes de EA:

| Classe CSS antiga | Nova classe CSS |
| -- | -- |
| tamanho do .auto flexível |  .SME-posição flexível automática |
| .Border tudo |  borda .sme-sm embutida e .sme a borda-cor-base-90 |
| .Border inferior |  .SME-borda inferior sm e .sme-border-bottom-color-base-90 |
| .Border-horizontal |  .SME-borda horizontal sm e .sme-border-horizontal-color-base-90 |
| .Border esquerdo |  .SME-borda esquerda sm e .sme-border-left-color-base-90 |
| .Border direita |  .SME-borda direita sm e .sme-border-right-color-base-90 |
| .Border superior |  .SME-borda superior sm e .sme-border-top-color-base-90 |
| .Border vertical |  .SME-borda vertical sm e .sme-border-vertical-color-base-90 |
| .break word |  .SME-organizá-ws-wrap |
| .btn |  botão .sme botão OR |
| .btn principal |  .SME button.sme-botão primário ou.button.sme-botão-principal |
| .Color-escuro |  cor-alt-.sme |
| luz .color |  base de cor .sme |
| .Color cinza-claro |  cor .sme-90 base |
| tamanho do .fixed flexível |  .SME-posição-flexível-nenhum |
| layout de .flex |  .SME-organizá-pilha-h ou .sme-organizá-pilha-v |
| .Font negrito |  .SME-fonte-emphasis1 |
| .Highlight |  .SME em segundo plano-cor amarelo |
| .horizontal |  .SME-organizá-pilha-h |
| rolagem .no |  .SME-posição flexível automática |
| .nowrap |  .SME-organizá-pilha-h ou .sme-organizá-pilha-v |
| .Relative |  .SME layout-relativo |
| Centro de .relative |  layout-absoluto .sme .sme posição central |
| . Reverse |  .SME-organizá-pilha-revertidas |
| .Stretch absoluto |  .SME layout-absoluto .sme-posição-embutida-nenhum |
| corrigido .stretch |  .SME layout fixo .sme-posição-embutida-nenhum |
| .Stretch vertical |  .SME-posição-stretch-v |
| largura .stretch |  .SME-posição-stretch-h |
| .vertical |  .SME-organizá-pilha-v |
| somente para a rolagem do .vertical |  .SME-organizá-estouro-ocultar-x EA-organizá-estouro-auto-y |
| .Wrap |  .SME-organizá-wrapstack-h ou .sme-organizá-wrapstack-v |

### Substitua esses componentes EA uso dos seguintes componentes:

| Componente antigo | Novo componente |
| -- | -- |
| .Alert |  alerta de EA |
| .Alert perigo |  alerta de EA |
| .breadCrumb |  alerta de EA |
| .CheckBox |  campos de formulário EA [tipo = "checkbox"] |
| .ComboBox |  campos de formulário EA [tipo = "selecionar"] |
| .Dashboard |  EA-layout-conteúdo-zona-preenchidas EA-organizá-pilha-h |
| painel .details |  grade de propriedade de EA |
| contêiner de painel .details |  grade de propriedade de EA |
| guia .details |  grade de propriedade de EA ou EA pivô |
| wrapper .details |  grade de propriedade de EA |
| .Disabled |  EA desativada |
| . Form-botões | campos de formulário EA |
| . Form-controle | campos de formulário EA |
| . Form-controles | campos de formulário EA |
| . Form-grupo | campos de formulário EA |
| rótulo do grupo de. Form | campos de formulário EA |
| entrada de. Form | campos de formulário EA |
| . Form-stretch | campos de formulário EA |
| arquivo .input | campos de formulário EA |
| guias de .nav |  EA pivô |
| .Radio |  campos de formulário EA [tipo = "rádio"] |
| Dica .required | campos de formulário EA |
| .searchbox |  campos de formulário EA [tipo = "Pesquisar"] |
| switch .toggle |  campos de formulário EA [tipo = "alternância"] |
| .Tool-container |  EA layout-conteúdo zona ou EA-layout-conteúdo-zona-preenchidas |

### Essas classes CSS foram preteridos e não há suporte para:

| Classe antigo | Preterido |
| -- | -- |
| .Acceptable | (preteridos) |
| erro .color | (preteridos) |
| informações de .color | (preteridos) |
| sucesso .color | (preteridos) |
| Aviso de .color | (preteridos) |
| botão Excluir | (preteridos) |
| conteúdo .details | (preteridos) |
| capa .error | (preteridos) |
| mensagem .error | (preteridos) |
| botão do painel de .guided | (preteridos) |
| .Header-container | (preteridos) |
| win .icon | (preteridos) |
| .indent | (preteridos) |
| Falha | (preteridos) |
| lista de .item | (preteridos) |
| .modal rolável | (preteridos) |
| . seção vários | (preteridos) |
| barra de ação .no | (preteridos) |
| margens .overflow | (preteridos) |
| ferramenta de .overflow | (preteridos) |
| capa .progress | (preteridos) |
| painel .right | (preteridos) |
| .Rollup | (preteridos) |
| status .rollup | (preteridos) |
| título .rollup | (preteridos) |
| valor .rollup | (preteridos) |
| barra de ação .searchbox | (preteridos) |
| .Size-h-1 | (preteridos) |
| .Size-h-2 | (preteridos) |
| .Size-h-3 | (preteridos) |
| .Size-h-4 | (preteridos) |
| .Size-h-completo | (preteridos) |
| .Size-h-metade | (preteridos) |
| .Size-v-1 | (preteridos) |
| .Size-v-2 | (preteridos) |
| .Size-v-3 | (preteridos) |
| .Size-v-4 | (preteridos) |
| ícone de status | (preteridos) |
| .SVG-16 px | (preteridos) |
| recuo .table | (preteridos) |
| .Table-sm | (preteridos) |
| .thin | (preteridos) |
| .Tile | (preteridos) |
| corpo .tile | (preteridos) |
| conteúdo .tile | (preteridos) |
| rodapé .tile | (preteridos) |
| cabeçalho .tile | (preteridos) |
| layout de .tile | (preteridos) |
| tabela .tile | (preteridos) |
| .ToolBar | (preteridos) |
| barra de .tool | (preteridos) |
| cabeçalho .tool | (preteridos) |
| caixa de cabeçalho .tool | (preteridos) |
| painel .tool | (preteridos) |
| barra de .usage | (preteridos) |
| área da barra de .usage | (preteridos) |
| .Usage-barra-em segundo plano | (preteridos) |
| .Usage de barra de título | (preteridos) |
| valor de barra .usage | (preteridos) |
| gráfico de .usage | (preteridos) |
| mensagem .usage | (preteridos) |
| área de mensagem .usage | (preteridos) |
| título da mensagem .usage | (preteridos) |
| .Warning | (preteridos) |
| espaço .white | (preteridos) |

## 7. entender e resolver problemas com objetos observáveis

### Atualização ```rxjs``` função use para objetos observáveis

Estes são alguns nomes de função comuns que foram alteradas, pode haver outras pessoas em seu projeto.

* Atualização ```Observable.empty()``` para ```empty()```
* Atualização ```Observable.of()``` para ```of()```
* Atualização ```.switchMap()``` para ```.pipe(switchMap())```
* Atualização ```.map()``` para ```.pipe(map())```
* Atualização ```flatMap()``` para ```mergeMap()```


### Resolver problemas de tempo de execução com ```.map()``` e ```.filter()``` funções em objetos observáveis

Se o compilador não pode identificar corretamente um ```observable``` tipo de objeto, ```.map()``` e ```.filter()``` funções do ```array``` objeto pode ser mapeado em vez disso, para seu objeto, causando erros em tempo de execução.  Certifique-se de que suas funções de retornam um ```observable``` objeto especificando um tipo de dados explícito para evitar esse problema.

```any``` e nenhum tipo de retorno podem causar esse problema, procure o código com esses padrões:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## 8. resolver outros problemas comuns

Essas técnicas ajudará a resolver outros problemas comuns:

* Execute ```ng lint --fix``` corrigir problemas comuns de lint
* Execute ```gulp build``` repetidamente incrementalmente corrigir problemas que ```gulp build``` pode resolver automaticamente

## 9. compilar e servir seu projeto

Execute os seguintes comandos para compilar e servir seu projeto com a versão mais recente (1.0 SDK):

``` cmd
gulp build
gulp serve --port 4201
```

## 10. ativar o tema escuro no Windows Admin Center

Para ativar o tema escuro no Windows Admin Center versão 1902 e posterior, siga estas etapas:

* Abrir o Windows Admin Center (versão 1902 e posterior)
* Clique no ícone de **configurações** no canto superior direito da janela
* Selecione a guia **Avançado**
* Em um *Experimento chaves*, clique em **Adicionar**
* Insira um novo valor ```msft.sme.shell.personalization``` no campo vazio que foi criado pelo etapa anterior
* Clique em **Salvar e recarregar**
* Configurações agora terá uma nova guia, **personalização**.  Selecione essa guia
* Alterar **as cores** para o **modo escuro (visualização)**
