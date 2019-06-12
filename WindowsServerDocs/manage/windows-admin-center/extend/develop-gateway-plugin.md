---
title: Desenvolver um plug-in de gateway
description: Desenvolver um plug-in de gateway (projeto Paulo) do SDK do Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 66e36a349fc6bd38a77ccf4f00d380788ea4b422
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445949"
---
# <a name="develop-a-gateway-plugin"></a>Desenvolver um plug-in de gateway

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Um plug-in do Windows Admin Center gateway permite a comunicação de API da interface do usuário da sua ferramenta ou solução para um nó de destino.  Windows Admin Center hospeda um serviço de gateway que retransmite comandos e scripts do plug-ins de gateway a ser executado em nós de destino. O serviço de gateway pode ser estendido para incluir o plug-ins personalizados de gateway que dão suporte a protocolos diferentes do padrão.

Esses plug-ins do gateway são incluídos por padrão com o Windows Admin Center:

* Plug-in do PowerShell gateway
* Plug-in de gateway WMI

Se você quiser se comunicar com um protocolo diferente do PowerShell ou WMI, como com REST, você pode criar seu próprio plug-in do gateway.  Plug-ins de gateway são carregados em um AppDomain separado do processo de gateway existente, mas usam o mesmo nível de elevação de direitos.

> [!NOTE]
> Não estiver familiarizado com os tipos de extensão diferentes? Saiba mais sobre o [tipos de arquitetura e a extensão de extensibilidade](understand-extensions.md).

## <a name="prepare-your-environment"></a>Prepare o ambiente

Se você ainda não o fez [preparar o ambiente](prepare-development-environment.md) instalando dependências e globais pré-requisitos necessários para todos os projetos.

## <a name="create-a-gateway-plugin-c-library"></a>Criar um plug-in do gateway (C# biblioteca)

Para criar um plug-in de gateway personalizado, crie um novo C# classe que implementa a ```IPlugIn``` interface da ```Microsoft.ManagementExperience.FeatureInterfaces``` namespace.  

> [!NOTE]
> O ```IFeature``` interface, disponível em versões anteriores do SDK, agora é sinalizado como obsoleto.  Todo desenvolvimento de plug-in de gateway deve usar IPlugIn (ou, opcionalmente, a classe abstrata de HttpPlugIn).

### <a name="download-sample-from-github"></a>Baixe o exemplo do GitHub

Para se familiarizar rapidamente com um plug-in de gateway personalizado, você pode clonar ou baixar uma cópia do nosso [exemplo C# projeto de plug-in](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) de nosso SDK do Windows Admin Center [site do GitHub](https://aka.ms/wacsdk).

### <a name="add-content"></a>Adicionar conteúdo

Adicionar novo conteúdo a sua cópia clonada do [exemplo C# projeto de plug-in](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) arquivo de projeto (ou seu próprio projeto) para conter suas APIs personalizadas, em seguida, crie o seu plug-in de gateway personalizado DLL para ser usado nas próximas etapas.

### <a name="deploy-plugin-for-testing"></a>Implantar o plug-in para teste

Teste seu plug-in de gateway personalizado DLL carregá-los no processo de gateway do Windows Admin Center.

Windows Admin Center procura todos os plug-ins em um ```plugins``` na pasta dados de aplicativo do computador atual (usando o valor de CommonApplicationData da enumeração SpecialFolder). No Windows 10 esse local é ```C:\ProgramData\Server Management Experience```.  Se o ```plugins``` pasta ainda não existe, você pode criar a pasta.

> [!NOTE]
> Você pode substituir o local do plug-in em uma compilação de depuração atualizando o valor de configuração "StaticsFolder". Se você estiver depurando localmente, essa configuração está no App. config da solução da área de trabalho. 

Dentro da pasta de plug-ins (neste exemplo, ```C:\ProgramData\Server Management Experience\plugins```)

* Crie uma nova pasta com o mesmo nome que o ```Name``` valor da propriedade a ```Feature``` no seu plug-in de gateway personalizado DLL (em nosso projeto de exemplo, o ```Name``` é "Exemplo Uno")
* Copie o arquivo DLL de plug-in de gateway personalizado para essa nova pasta
* Reinicie o processo de Windows Admin Center

Depois que o processo de administração do Windows é reiniciado, você poderá utilizar as APIs no seu plug-in de gateway personalizado DLL emitindo um GET, PUT, PATCH, excluir ou postar ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Opcional: Anexar ao plug-in para depuração

No Visual Studio 2017, no menu Depurar, selecione "Anexar ao processo". Na próxima janela, percorra a lista de processos disponíveis, selecione SMEDesktop.exe e clique em "Anexar". Uma vez o depurador é iniciado, você pode colocar um ponto de interrupção no seu código de recurso e o exercício, em seguida, por meio do formato de URL acima. Para nosso projeto de exemplo (nome de recurso: "Exemplo Uno") a URL é: "<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Criar uma extensão de ferramenta com a CLI do Windows Admin Center ##

Agora, precisamos criar uma extensão de ferramenta da qual você pode chamar seu plug-in de gateway personalizado.  Criar ou procurar uma pasta onde você deseja armazenar seus arquivos de projeto, abra um prompt de comando e definir essa pasta como o diretório de trabalho.  Usando a CLI do Windows Admin Center que foi instalado anteriormente, crie uma nova extensão com a seguinte sintaxe:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicação | Exemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome da sua empresa (com espaços) | ```Contoso Inc``` |
| ```{!Tool Name}``` | O nome da ferramenta (com espaços) | ```Manage Foo Works``` |

Aqui está um uso de exemplo:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Isso cria uma nova pasta dentro do diretório de trabalho atual usando o nome especificado para sua ferramenta, copia todos os arquivos de modelo necessário para seu projeto e configura os arquivos com o nome da empresa e a ferramenta.  

Em seguida, altere o diretório para a pasta que acabou de criar e, em seguida, instalar as dependências necessárias locais executando o seguinte comando:

```
npm install
```

Quando isso for concluído, você configurou tudo o que você precisa carregar sua extensão de novo no Windows Admin Center. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Conectar-se a sua extensão de ferramenta para seu plug-in de gateway personalizado

Agora que você criou uma extensão com a CLI do Windows Admin Center, você está pronto para se conectar a sua extensão de ferramenta para seu plug-in do gateway personalizado, seguindo estas etapas:

- Adicionar um [módulo vazio](guides/add-module.md)
- Use sua [plug-in do gateway personalizado](guides/use-custom-gateway-plugin.md) em sua extensão de ferramenta
 
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