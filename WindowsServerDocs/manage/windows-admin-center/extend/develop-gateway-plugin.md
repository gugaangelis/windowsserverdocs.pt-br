---
title: Desenvolver um plug-in de gateway
description: Desenvolver um plug-in de gateway SDK do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081153"
---
# Desenvolver um plug-in de gateway

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Um plug-in de gateway do Windows Admin Center permite a comunicação de API da interface do usuário da sua ferramenta ou a solução a um nó de destino.  Windows Admin Center hospeda um serviço de gateway que retransmite comandos e scripts do gateway plug-ins a ser executado em nós de destino. O serviço de gateway pode ser estendido para incluir personalizado gateway plug-ins que oferecem suporte a protocolos que não sejam as definições padrão.

Esses plug-ins do gateway estão incluídos por padrão no Windows Admin Center:

* Plug-in de gateway do PowerShell
* Plug-in de gateway WMI

Se você quiser se comunicar com um protocolo diferente do PowerShell ou WMI, como com o REST, você pode criar seu próprio plug-in do gateway.  Plug-ins do gateway são carregados em um AppDomain separado do processo de gateway existente, mas usam o mesmo nível de elevação de direitos.

> [!NOTE]
> Não estiver familiarizado com os tipos de extensão diferente? Saiba mais sobre os [tipos de arquitetura e a extensão de extensibilidade](understand-extensions.md).

## Preparar o ambiente

Se você ainda não fez isto, [prepare seu ambiente](prepare-development-environment.md) instalando as dependências e globais pré-requisitos necessários para todos os projetos.

## Criar um plug-in de gateway (biblioteca de c#)

Para criar um plug-in de gateway personalizado, crie uma nova classe c# que implementa o ```IPlugIn``` interface do ```Microsoft.ManagementExperience.FeatureInterfaces``` namespace.  

> [!NOTE]
> O ```IFeature``` interface, disponível em versões anteriores do SDK, agora está marcada como obsoleta.  Todos os desenvolvimentos de plug-in de gateway devem usar IPlugIn (ou, opcionalmente, a classe abstrata HttpPlugIn).

### Baixe a amostra do GitHub

Para começar rapidamente com um plug-in de gateway personalizado, você pode clonar ou baixar uma cópia do nosso [projeto plug-in de exemplo em c#](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) do SDK do Windows Admin Center [GitHub site](https://aka.ms/wacsdk).

### Adicionar conteúdo

Adicionar novo conteúdo à sua cópia clonada do projeto a [projeto plug-in de exemplo em c#](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) (ou seu próprio projeto) para conter as APIs personalizadas e criar seu arquivo DLL do plug-in de gateway personalizado a ser usado nas próximas etapas.

### Implantar o plug-in para testes

Teste o plug-in de gateway personalizado DLL carregá-lo no processo de gateway do Windows Admin Center.

Windows Admin Center procura por todos os plug-ins em um ```plugins``` na pasta dados de aplicativo do computador atual (usando o valor CommonApplicationData da enumeração SpecialFolder). No Windows 10 esse local é ```C:\ProgramData\Server Management Experience```.  Se o ```plugins``` pasta ainda não existe, você pode criar a pasta.

> [!NOTE]
> Você pode substituir o local de plug-in em uma compilação de depuração, atualizando o valor de configuração "StaticsFolder". Se você estiver depurando localmente, essa configuração está no App da solução da área de trabalho. 

Dentro da pasta de plug-ins (neste exemplo, ```C:\ProgramData\Server Management Experience\plugins```)

* Crie uma nova pasta com o mesmo nome que o ```Name``` valor da propriedade do ```Feature``` em seu plug-in de gateway personalizado DLL (em nosso projeto de exemplo, o ```Name``` é "Exemplo Uno")
* Copie o arquivo DLL do plug-in de gateway personalizados para essa nova pasta
* Reiniciar o processo de Windows Admin Center

Depois que o processo de administração do Windows for reiniciado, você será capaz de exercer as APIs em seu plug-in de gateway personalizado DLL emitindo um GET, coloque, PATCH, excluir ou postar ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### Opcional: Anexar ao plug-in para depuração

No Visual Studio 2017, no menu Depurar, selecione "Anexar ao processo". Na próxima janela, percorra a lista de processos disponíveis, selecione SMEDesktop.exe e clique em "Conectar". Uma vez o depurador é iniciado, você pode colocar um ponto de interrupção no seu código de recurso e, em seguida, exercício por meio de formato da URL acima. Para nosso projeto de exemplo (nome de recurso: "Exemplo Uno") a URL é: "http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno"

## Criar uma extensão de ferramenta com a CLI do Windows Admin Center ##

Agora, precisamos criar uma extensão de ferramenta do qual você pode chamar o plug-in de gateway personalizado.  Criar ou navegue até uma pasta onde você deseja armazenar seus arquivos de projeto, abra um prompt de comando e definido nessa pasta como o diretório de trabalho.  Usando a CLI do Windows Admin Center que foi instalada anteriormente, crie uma nova extensão com a seguinte sintaxe:

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

## Conectar sua extensão de ferramenta para o plug-in de gateway personalizado

Agora que você já criou uma extensão com a CLI do Windows Admin Center, você está pronto para se conectar sua extensão de ferramenta para o plug-in de gateway personalizado, seguindo estas etapas:

- Adicionar um [módulo vazio](guides\add-module.md)
- Use o [plug-in de gateway personalizado](guides\use-custom-gateway-plugin.md) na sua extensão de ferramenta
 
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