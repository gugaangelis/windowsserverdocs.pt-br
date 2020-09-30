---
title: Desenvolver uma extensão de solução
description: Desenvolver uma extensão de solução SDK do Windows Admin Center (projeto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 293fa8a617d7ceb1628ec72df2f015b6b2547f16
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517582"
---
# <a name="develop-a-solution-extension"></a>Desenvolver uma extensão de solução

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

As soluções definem principalmente um tipo exclusivo de objeto que você deseja gerenciar por meio do centro de administração do Windows.  Esses tipos de soluções/conexões estão incluídos no centro de administração do Windows por padrão:

* Conexões do Windows Server
* Conexões de computador Windows
* Conexões de cluster de failover
* Conexões de cluster hiperconvergentes

Quando você seleciona uma conexão na página de conexão do centro de administração do Windows, a extensão da solução para o tipo dessa conexão é carregada e o centro de administração do Windows tentará se conectar ao nó de destino. Se a conexão for bem-sucedida, a interface do usuário da extensão da solução será carregada e o centro de administração do Windows exibirá as ferramentas para essa solução no painel de navegação à esquerda.

Se você quiser criar uma GUI de gerenciamento para serviços não definidos pelos tipos de conexão padrão acima, como um comutador de rede ou outro hardware não detectável pelo nome do computador, talvez você queira criar sua própria extensão de solução.

> [!NOTE]
> Não está familiarizado com os tipos de extensão diferentes? Saiba mais sobre a [arquitetura de extensibilidade e os tipos de extensão](understand-extensions.md).

## <a name="prepare-your-environment"></a>Prepare o seu ambiente

Se você ainda não fez isso, [Prepare seu ambiente](prepare-development-environment.md) instalando dependências e pré-requisitos globais necessários para todos os projetos.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Criar uma nova extensão de solução com a CLI do centro de administração do Windows ##

Depois de ter todas as dependências instaladas, você estará pronto para criar sua nova extensão de solução.  Crie ou navegue até uma pasta que contém os arquivos de projeto, abra um prompt de comando e defina essa pasta como o diretório de trabalho.  Usando a CLI do centro de administração do Windows que foi instalada anteriormente, crie uma nova extensão com a seguinte sintaxe:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Solution Name}``` | O nome da solução (com espaços) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |

Aqui está um uso de exemplo:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Isso cria uma nova pasta dentro do diretório de trabalho atual usando o nome especificado para sua solução, copia todos os arquivos de modelo necessários em seu projeto e configura os arquivos com sua empresa, solução e nome de ferramenta.

Em seguida, altere o diretório para a pasta recém-criada e instale as dependências locais necessárias executando o seguinte comando:

```
npm install
```

Quando isso for concluído, você configurou tudo o que precisa para carregar sua nova extensão no centro de administração do Windows.

## <a name="add-content-to-your-extension"></a>Adicionar conteúdo à sua extensão

Agora que você criou uma extensão com a CLI do centro de administração do Windows, você está pronto para personalizar o conteúdo.  Consulte estes guias para obter exemplos do que você pode fazer:

- Adicionar um [módulo vazio](guides/add-module.md)
- Adicionar um [iframe](guides/add-iframe.md)
- Criar um [provedor de conexão personalizado](guides/create-connection-provider.md)
- Modificar o [comportamento de navegação raiz](guides/modify-root-navigation.md)

Ainda mais exemplos podem ser encontrados em nosso guia do desenvolvedor. O guia do desenvolvedor é uma extensão de solução totalmente funcional que pode ser carregada no centro de administração do Windows e contém uma rica coleção de exemplos de funcionalidade e de ferramentas que você pode procurar e usar em sua própria extensão. 

Habilite a extensão do guia do desenvolvedor na página **avançado** de suas configurações do centro de administração do Windows. 

## <a name="build-and-side-load-your-extension"></a>Compilar e carregar lado sua extensão

Em seguida, compile e carregue sua extensão no centro de administração do Windows.  Abra uma janela de comando, altere o diretório para o diretório de origem e, em seguida, você estará pronto para compilar.

* Compilar e servir com gulp:

    ```
    gulp build
    gulp serve -p 4201
    ```

Observe que você precisa escolher uma porta que está atualmente gratuita. Certifique-se de não tentar usar a porta que está em execução no Windows Admin Center.

Seu projeto pode ser transferido por sideload em uma instância local do Windows Admin Center para teste ao anexar o projeto servido localmente no Windows Admin Center.

* Iniciar o Windows Admin Center em um navegador da Web
* Abrir o depurador (F12)
* Abra o Console e digite o seguinte comando:

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Atualizar o navegador da Web

Seu projeto agora estará visível na lista Ferramentas com (sideloaded) ao lado do nome.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Direcionar uma versão diferente do SDK do centro de administração do Windows

Manter sua extensão atualizada com alterações do SDK e alterações na plataforma é fácil.  Leia sobre como [direcionar uma versão diferente](target-sdk-version.md) do SDK do centro de administração do Windows.