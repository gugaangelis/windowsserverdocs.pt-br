---
title: Desenvolver uma extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do Windows Admin Center (projeto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9cf369558977c50ee0913b65e62de448959aac32
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517572"
---
# <a name="develop-a-tool-extension"></a>Desenvolver uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Uma extensão de ferramenta é a principal maneira que os usuários interagem com o centro de administração do Windows para gerenciar uma conexão, como um servidor ou cluster. Ao clicar em uma conexão na tela inicial do centro de administração do Windows e conectar-se, você verá uma lista de ferramentas no painel de navegação à esquerda. Quando você clica em uma ferramenta, a extensão de ferramenta é carregada e exibida no painel direito.

Quando uma extensão de ferramenta é carregada, ela pode executar chamadas WMI ou scripts do PowerShell em um servidor de destino ou cluster e exibir informações na interface do usuário ou executar comandos com base na entrada do usuário. As extensões de ferramenta definem quais soluções devem ser exibidas, resultando em um conjunto diferente de ferramentas para cada solução.

> [!NOTE]
> Não está familiarizado com os tipos de extensão diferentes? Saiba mais sobre a [arquitetura de extensibilidade e os tipos de extensão](understand-extensions.md).

## <a name="prepare-your-environment"></a>Prepare o seu ambiente

Se você ainda não fez isso, [Prepare seu ambiente](prepare-development-environment.md) instalando dependências e pré-requisitos globais necessários para todos os projetos.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Criar uma nova extensão de ferramenta com a CLI do centro de administração do Windows ##

Depois de ter todas as dependências instaladas, você estará pronto para criar sua nova extensão de ferramenta.  Crie ou navegue até uma pasta que contém os arquivos de projeto, abra um prompt de comando e defina essa pasta como o diretório de trabalho.  Usando a CLI do centro de administração do Windows que foi instalada anteriormente, crie uma nova extensão com a seguinte sintaxe:

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |

Aqui está um uso de exemplo:

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Isso cria uma nova pasta dentro do diretório de trabalho atual usando o nome especificado para sua ferramenta, copia todos os arquivos de modelo necessários em seu projeto e configura os arquivos com o nome da sua empresa e da ferramenta.

Em seguida, altere o diretório para a pasta recém-criada e instale as dependências locais necessárias executando o seguinte comando:

``` cmd
npm install
```

Quando isso for concluído, você configurou tudo o que precisa para carregar sua nova extensão no centro de administração do Windows.

## <a name="add-content-to-your-extension"></a>Adicionar conteúdo à sua extensão

Agora que você criou uma extensão com a CLI do centro de administração do Windows, você está pronto para personalizar o conteúdo.  Consulte estes guias para obter exemplos do que você pode fazer:

- Adicionar um [módulo vazio](guides/add-module.md)
- Adicionar um [iframe](guides/add-iframe.md)

Ainda mais exemplos podem ser encontrados em nosso guia do desenvolvedor. O guia do desenvolvedor é uma extensão de solução totalmente funcional que pode ser carregada no centro de administração do Windows e contém uma rica coleção de exemplos de funcionalidade e de ferramentas que você pode procurar e usar em sua própria extensão. 

Habilite a extensão do guia do desenvolvedor na página **avançado** de suas configurações do centro de administração do Windows. 

## <a name="customize-your-extensions-icon"></a>Personalizar o ícone da extensão

Você pode personalizar o ícone que aparece para sua extensão na lista de ferramentas.  Para fazer isso, modifique todas as ```icon``` entradas em ```manifest.json``` para sua extensão:

``` json
"icon": "{!icon-uri}",
```

| Valor | Explicação | URI de exemplo |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | O local do recurso de ícone | ```assets/foo-icon.svg``` |

Observação: atualmente, os ícones personalizados não são visíveis ao carregar o lado de sua extensão no modo de desenvolvimento.  Como alternativa, remova o conteúdo da seguinte ```target``` maneira:

``` json
"target": "",
```

Essa configuração só é válida para o carregamento lateral no modo dev, portanto, é importante preservar o valor contido em ```target``` e, em seguida, restaurá-lo antes de publicar sua extensão.

## <a name="build-and-side-load-your-extension"></a>Compilar e carregar lado sua extensão

Em seguida, compile e carregue sua extensão no centro de administração do Windows.  Abra uma janela de comando, altere o diretório para o diretório de origem e, em seguida, você estará pronto para compilar.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Direcionar uma versão diferente do SDK do centro de administração do Windows

Manter sua extensão atualizada com alterações do SDK e alterações na plataforma é fácil.  Leia sobre como [direcionar uma versão diferente](target-sdk-version.md) do SDK do centro de administração do Windows.