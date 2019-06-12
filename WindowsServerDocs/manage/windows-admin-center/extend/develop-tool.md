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
ms.openlocfilehash: 31d8dbd3df4c44b6e0a3780b022dfbd9fffdffec
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452585"
---
# <a name="develop-a-tool-extension"></a>Desenvolver uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Uma extensão de ferramenta é a principal maneira dos usuários interagem com o Windows Admin Center para gerenciar uma conexão, como um servidor ou cluster. Quando você clica em uma conexão na tela inicial do Windows Admin Center e conectar-se, você, em seguida, verá uma lista de ferramentas no painel de navegação à esquerda. Quando você clica em uma ferramenta, a extensão de ferramenta é carregada e exibida no painel direito.

Quando uma extensão de ferramenta é carregada, ele pode execute chamadas WMI ou scripts do PowerShell em um servidor de destino ou cluster e exibir informações na interface do usuário ou comandos com base na entrada do usuário. As extensões de ferramentas definem quais soluções ele deve ser exibido, resultando em um conjunto diferente de ferramentas para cada solução.

> [!NOTE]
> Não estiver familiarizado com os tipos de extensão diferentes? Saiba mais sobre o [tipos de arquitetura e a extensão de extensibilidade](understand-extensions.md).

## <a name="prepare-your-environment"></a>Prepare o ambiente

Se você ainda não o fez [preparar o ambiente](prepare-development-environment.md) instalando dependências e globais pré-requisitos necessários para todos os projetos.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Criar uma nova extensão de ferramenta com a CLI do Windows Admin Center ##

Depois que todas as dependências instaladas, você está pronto para criar sua nova extensão de ferramenta.  Criar ou procurar uma pasta que contém os arquivos de projeto, abra um prompt de comando e definir essa pasta como o diretório de trabalho.  Usando a CLI do Windows Admin Center que foi instalado anteriormente, crie uma nova extensão com a seguinte sintaxe:

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |

Aqui está um uso de exemplo:

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Isso cria uma nova pasta dentro do diretório de trabalho atual usando o nome especificado para sua ferramenta, copia todos os arquivos de modelo necessário para seu projeto e configura os arquivos com o nome da empresa e a ferramenta.  

Em seguida, altere o diretório para a pasta que acabou de criar e, em seguida, instalar as dependências necessárias locais executando o seguinte comando:

``` cmd
npm install
```

Quando isso for concluído, você configurou tudo o que você precisa carregar sua extensão de novo no Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Adicionar conteúdo a sua extensão

Agora que você criou uma extensão com a CLI do Windows Admin Center, você está pronto para personalizar o conteúdo.  Confira estes guias para obter exemplos de como você pode fazer:

- Adicionar um [módulo vazio](guides/add-module.md)
- Adicionar um [iFrame](guides/add-iframe.md)
 
Ainda mais exemplos podem ser encontrados nossos [site do GitHub SDK](https://aka.ms/wacsdk):
-  [Ferramentas de desenvolvedor](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) é uma extensão totalmente funcional que pode ser carregado por sideload no Windows Admin Center e contém um rico conjunto de exemplos de funcionalidade e a ferramenta de exemplo que você pode procurar e usar em sua própria extensão.

## <a name="customize-your-extensions-icon"></a>Personalizar o ícone da sua extensão

Você pode personalizar o ícone que mostra para a sua extensão na lista de ferramenta.  Para fazer isso, modifique todas ```icon``` entradas no ```manifest.json``` para a sua extensão:

``` json
"icon": "{!icon-uri}",
```

| Valor | Explicação | Uri de exemplo |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | O local do seu recurso de ícone | ```assets/foo-icon.svg``` |

OBSERVAÇÃO: Atualmente, os ícones personalizados não são visíveis ao lado de carregar sua extensão no modo de desenvolvimento.  Como alternativa, remova o conteúdo de ```target``` da seguinte maneira:

``` json
"target": "",
```

Essa configuração só é válida para sideload no modo de desenvolvimento, portanto, é importante preservar o valor contido no ```target``` e, em seguida, restaurá-lo antes de publicar sua extensão.

## <a name="build-and-side-load-your-extension"></a>Compilação e o lado carregam sua extensão

Em seguida, compilação e o lado carregam sua extensão Windows Admin Center.  Abra uma janela de comando, altere o diretório para seu diretório de origem, em seguida, você estará pronto para compilar.

* Compilar e servir com gulp:

    ``` cmd
    gulp build
    gulp serve -p 4201
    ```

Observe que você precisa escolher uma porta que está atualmente gratuita. Certifique-se de não tentar usar a porta que está em execução no Windows Admin Center.

Seu projeto pode ser transferido por sideload em uma instância local do Windows Admin Center para teste ao anexar o projeto servido localmente no Windows Admin Center.

* Iniciar o Windows Admin Center em um navegador da Web
* Abrir o depurador (F12)
* Abra o Console e digite o seguinte comando:

    ``` cmd
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Atualizar o navegador da Web

Seu projeto agora estará visível na lista Ferramentas com (sideloaded) ao lado do nome.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Uma versão diferente do SDK do Windows Admin Center de destino

É fácil manter sua extensão atualizada com as alterações do SDK e plataforma.  Leia sobre como [direcionar uma versão diferente](target-sdk-version.md) do SDK do Windows Admin Center.