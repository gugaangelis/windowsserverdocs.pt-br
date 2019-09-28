---
title: Desenvolver um plug-in de gateway
description: Desenvolver um plug-in de gateway SDK do centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2b096b226190ad1ca3fd07c38b7b939d019ee30f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406934"
---
# <a name="develop-a-gateway-plugin"></a>Desenvolver um plug-in de gateway

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Um plug-in de gateway do centro de administração do Windows habilita a comunicação de API da interface do usuário da sua ferramenta ou solução para um nó de destino.  O centro de administração do Windows hospeda um serviço de gateway que retransmite comandos e scripts de plugins de gateway a serem executados em nós de destino. O serviço de gateway pode ser estendido para incluir plug-ins de gateway personalizados que dão suporte a protocolos diferentes daqueles padrão.

Esses plug-ins de gateway são incluídos por padrão no centro de administração do Windows:

* Plug-in do gateway do PowerShell
* Plug-in de gateway WMI

Se você quiser se comunicar com um protocolo diferente do PowerShell ou WMI, como com REST, você pode criar seu próprio plug-in de gateway.  Os plug-ins de gateway são carregados em um AppDomain separado do processo de gateway existente, mas usam o mesmo nível de elevação para direitos.

> [!NOTE]
> Não está familiarizado com os tipos de extensão diferentes? Saiba mais sobre a [arquitetura de extensibilidade e os tipos de extensão](understand-extensions.md).

## <a name="prepare-your-environment"></a>Prepare o ambiente

Se você ainda não fez isso, [Prepare seu ambiente](prepare-development-environment.md) instalando dependências e pré-requisitos globais necessários para todos os projetos.

## <a name="create-a-gateway-plugin-c-library"></a>Criar um plug-inC# de gateway (biblioteca)

Para criar um plug-in de gateway personalizado, C# crie uma nova classe que implemente a interface ```IPlugIn``` do namespace ```Microsoft.ManagementExperience.FeatureInterfaces```.  

> [!NOTE]
> A interface ```IFeature```, disponível em versões anteriores do SDK, agora está sinalizada como obsoleta.  Todo o desenvolvimento de plugin de gateway deve usar IPlugIn (ou, opcionalmente, a classe abstrata HttpPlugIn).

### <a name="download-sample-from-github"></a>Baixar exemplo do GitHub

Para começar rapidamente com um plug-in de gateway personalizado, você pode clonar ou baixar uma cópia do nosso [projeto de plug-in de C# exemplo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) de nosso [site GitHub](https://aka.ms/wacsdk)do SDK do centro de administração do Windows.

### <a name="add-content"></a>Adicionar conteúdo

Adicione novo conteúdo à cópia clonada do projeto de [projeto C# de plug-in de exemplo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) (ou seu próprio projeto) para conter suas APIs personalizadas e, em seguida, crie seu arquivo DLL de plug-in de gateway personalizado para ser usado nas próximas etapas.

### <a name="deploy-plugin-for-testing"></a>Implantar plug-in para teste

Teste sua DLL de plug-in de gateway personalizado carregando-a no processo do gateway do centro de administração do Windows.

O centro de administração do Windows procura todos os plug-ins em uma pasta ```plugins``` na pasta dados do aplicativo do computador atual (usando o valor CommonApplicationData da enumeração Environment. SpecialFolder). No Windows 10, esse local é ```C:\ProgramData\Server Management Experience```.  Se a pasta ```plugins``` ainda não existir, você poderá criar a pasta por conta própria.

> [!NOTE]
> Você pode substituir o local do plug-in em uma compilação de depuração atualizando o valor de configuração "StaticsFolder". Se você estiver Depurando localmente, essa configuração estará no app. config da solução de desktop. 

Dentro da pasta plugins (neste exemplo, ```C:\ProgramData\Server Management Experience\plugins```)

* Crie uma nova pasta com o mesmo nome que o valor da propriedade ```Name``` do ```Feature``` em sua DLL de plug-in de gateway personalizado (em nosso projeto de exemplo, o ```Name``` é "Uno de exemplo")
* Copie seu arquivo DLL de plug-in de gateway personalizado para esta nova pasta
* Reiniciar o processo do centro de administração do Windows

Após a reinicialização do processo de administração do Windows, você poderá exercitar as APIs em sua DLL de plug-in de gateway personalizado emitindo GET, PUT, PATCH, DELETE ou POST para ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Opcional: Anexar ao plug-in para depuração

No Visual Studio 2017, no menu Depurar, selecione "anexar ao processo". Na próxima janela, percorra a lista processos disponíveis e selecione SMEDesktop. exe e, em seguida, clique em "anexar". Depois que o depurador for iniciado, você poderá inserir um ponto de interrupção em seu código de recurso e, em seguida, exercitar por meio do formato de URL acima. Para nosso projeto de exemplo (nome do recurso: "Uno de exemplo") a URL é: "<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Criar uma extensão de ferramenta com a CLI do centro de administração do Windows ##

Agora, precisamos criar uma extensão de ferramenta da qual você possa chamar seu plug-in de gateway personalizado.  Crie ou navegue até uma pasta na qual você deseja armazenar os arquivos de projeto, abra um prompt de comando e defina essa pasta como o diretório de trabalho.  Usando a CLI do centro de administração do Windows instalada anteriormente, crie uma nova extensão com a seguinte sintaxe:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | O nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |

Aqui está um uso de exemplo:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Isso cria uma nova pasta dentro do diretório de trabalho atual usando o nome especificado para sua ferramenta, copia todos os arquivos de modelo necessários em seu projeto e configura os arquivos com o nome da sua empresa e da ferramenta.  

Em seguida, altere o diretório para a pasta recém-criada e instale as dependências locais necessárias executando o seguinte comando:

```
npm install
```

Quando isso for concluído, você configurou tudo o que precisa para carregar sua nova extensão no centro de administração do Windows. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Conecte sua extensão de ferramenta ao seu plug-in de gateway personalizado

Agora que você criou uma extensão com a CLI do centro de administração do Windows, está pronto para conectar sua extensão de ferramenta ao plug-in de gateway personalizado, seguindo estas etapas:

- Adicionar um [módulo vazio](guides/add-module.md)
- Usar seu [plug-in de gateway personalizado](guides/use-custom-gateway-plugin.md) em sua extensão de ferramenta
 
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