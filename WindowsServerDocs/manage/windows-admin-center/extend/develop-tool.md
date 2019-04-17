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
ms.openlocfilehash: 7dd213f7032ab77021bbfcbdc966c9c2307b86eb
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081053"
---
# Desenvolver uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Uma extensão de ferramenta é a principal maneira que os usuários interagem com o Windows Admin Center para gerenciar uma conexão, como um servidor ou cluster. Quando você clicar em uma conexão na tela inicial do Windows Admin Center e conectar-se, em seguida, ser apresentado com uma lista de ferramentas no painel de navegação esquerdo. Quando você clica em uma ferramenta, a extensão de ferramenta é carregada e exibida no painel à direita.

Quando uma extensão de ferramenta é carregada, ele pode executar chamadas WMI ou scripts do PowerShell em um servidor de destino ou cluster e exibir informações na interface do usuário ou executar comandos com base na entrada do usuário. Extensões de ferramenta definem quais soluções que devem ser exibidas, resultando em um conjunto diferente de ferramentas para cada solução.

> [!NOTE]
> Não estiver familiarizado com os tipos de extensão diferente? Saiba mais sobre os [tipos de arquitetura e a extensão de extensibilidade](understand-extensions.md).

## Preparar o ambiente

Se você ainda não fez isto, [prepare seu ambiente](prepare-development-environment.md) instalando as dependências e globais pré-requisitos necessários para todos os projetos.

## Criar uma nova extensão de ferramenta com a CLI do Windows Admin Center ##

Quando você tiver todas as dependências instaladas, você estará pronto para criar a nova extensão de ferramenta.  Criar ou navegue até uma pasta que contém os arquivos de projeto, abra um prompt de comando e definido nessa pasta como o diretório de trabalho.  Usando a CLI do Windows Admin Center que foi instalado anteriormente, crie uma nova extensão com a seguinte sintaxe:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |

Aqui está um uso de exemplo:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Isso cria uma nova pasta dentro do diretório de trabalho atual usando o nome especificado para a ferramenta, copia todos os arquivos de modelo necessários para seu projeto e configura os arquivos com o nome da empresa e de ferramenta.  

Em seguida, mude o diretório para a pasta que acabou de criar e instalar as dependências de local necessárias executando o seguinte comando:

```
npm install
```

Após a conclusão, você configurou tudo o que você precisa para carregar a nova extensão no Windows Admin Center. 

## Adicionar conteúdo à sua extensão

Agora que você já criou uma extensão com a CLI do Windows Admin Center, você estará pronto para personalizar o conteúdo.  Consulte nestes guias para obter exemplos de que você pode fazer:

- Adicionar um [módulo vazio](guides\add-module.md)
- Adicionar um [iFrame](guides\add-iframe.md)
 
Ainda mais exemplos podem ser encontrados nosso [site GitHub SDK](https://aka.ms/wacsdk):
-  [Ferramentas de desenvolvedor](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) é uma extensão totalmente funcional que pode ser carregada no Windows Admin Center e contém um conjunto rico de funcionalidades de exemplo e exemplos de ferramenta que você pode procurar e usar em sua própria extensão.

## Compilação e lado carregam sua extensão

Em seguida, compilação e lado carregam sua extensão no Windows Admin Center.  Abra uma janela de comando, altere o diretório para seu diretório de origem, em seguida, você estará pronto para compilar.

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

## Direcionar uma versão diferente do SDK do Windows Admin Center

É fácil manter sua extensão atualizados com alterações SDK e plataforma.  Leia sobre como [alvo uma versão diferente](target-sdk-version.md) do SDK do Windows Admin Center.