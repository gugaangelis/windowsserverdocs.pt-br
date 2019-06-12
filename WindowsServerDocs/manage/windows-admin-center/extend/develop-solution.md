---
title: Desenvolver uma extensão de solução
description: Desenvolver uma solução de extensão Windows Admin Center SDK (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 268a7d2833f73e9fab006501e9b3dc261d1b1d9e
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452567"
---
# <a name="develop-a-solution-extension"></a>Desenvolver uma extensão de solução

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Soluções basicamente definem um tipo exclusivo do objeto que você deseja gerenciar por meio do Windows Admin Center.  Esses tipos de soluções/conexão estão incluídos no Windows Admin Center por padrão:

* Conexões de servidor do Windows
* Conexões de PC do Windows
* Conexões de cluster de failover
* Conexões de cluster hiperconvergente

Quando você seleciona uma conexão da página de conexão do Windows Admin Center, a extensão da solução para o tipo de conexão que é carregada e Windows Admin Center tentarão se conectar ao nó de destino. Se a conexão for bem-sucedida, a solução de interface de usuário da extensão será carregado e Windows Admin Center exibirá as ferramentas para a solução no painel de navegação esquerdo.

Se você quiser criar uma GUI de gerenciamento para serviços não são definidos os tipos de conexão padrão acima, tal um comutador de rede ou outros dispositivos de hardware não podem ser descobertos pelo nome do computador, você talvez queira criar sua própria extensão da solução.

> [!NOTE]
> Não estiver familiarizado com os tipos de extensão diferentes? Saiba mais sobre o [tipos de arquitetura e a extensão de extensibilidade](understand-extensions.md).

## <a name="prepare-your-environment"></a>Prepare o ambiente

Se você ainda não o fez [preparar o ambiente](prepare-development-environment.md) instalando dependências e globais pré-requisitos necessários para todos os projetos.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Criar uma nova extensão de solução com a CLI do Windows Admin Center ##

Depois que todas as dependências instaladas, você está pronto para criar sua nova extensão de solução.  Criar ou procurar uma pasta que contém os arquivos de projeto, abra um prompt de comando e definir essa pasta como o diretório de trabalho.  Usando a CLI do Windows Admin Center que foi instalado anteriormente, crie uma nova extensão com a seguinte sintaxe:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Solution Name}``` | Nome da solução (com espaços) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |

Aqui está um uso de exemplo:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Isso cria uma nova pasta dentro do diretório de trabalho atual usando o nome especificado para sua solução, copia todos os arquivos de modelo necessário para seu projeto e configura os arquivos com a sua empresa, a solução e o nome da ferramenta.  

Em seguida, altere o diretório para a pasta que acabou de criar e, em seguida, instalar as dependências necessárias locais executando o seguinte comando:

```
npm install
```

Quando isso for concluído, você configurou tudo o que você precisa carregar sua extensão de novo no Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Adicionar conteúdo a sua extensão

Agora que você criou uma extensão com a CLI do Windows Admin Center, você está pronto para personalizar o conteúdo.  Confira estes guias para obter exemplos de como você pode fazer:

- Adicionar um [módulo vazio](guides/add-module.md)
- Adicionar um [iFrame](guides/add-iframe.md)
- Criar um [provedor de conexão personalizada](guides/create-connection-provider.md)
- Modificar [raiz o comportamento de navegação](guides/modify-root-navigation.md)
 
Ainda mais exemplos podem ser encontrados nossos [site do GitHub SDK](https://aka.ms/wacsdk):
-  [Ferramentas de desenvolvedor](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) é uma extensão totalmente funcional que pode ser carregado por sideload no Windows Admin Center e contém um rico conjunto de exemplos de funcionalidade e a ferramenta de exemplo que você pode procurar e usar em sua própria extensão.

## <a name="build-and-side-load-your-extension"></a>Compilação e o lado carregam sua extensão

Em seguida, compilação e o lado carregam sua extensão Windows Admin Center.  Abra uma janela de comando, altere o diretório para seu diretório de origem, em seguida, você estará pronto para compilar.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Uma versão diferente do SDK do Windows Admin Center de destino

É fácil manter sua extensão atualizada com as alterações do SDK e plataforma.  Leia sobre como [direcionar uma versão diferente](target-sdk-version.md) do SDK do Windows Admin Center.