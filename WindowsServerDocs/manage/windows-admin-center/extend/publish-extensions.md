---
title: Publicando extensões para o centro de administração do Windows
description: Publicando extensões para o centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d2bb97fb65e3fbf5c7809317a8565ff7051d0447
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869702"
---
# <a name="publishing-extensions"></a>Publicando extensões

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Depois de desenvolver sua extensão, você desejará publicá-la e disponibilizá-la para outras pessoas para teste ou uso. Dependendo do público e da finalidade da publicação, há algumas opções que apresentaremos abaixo, juntamente com as etapas e os requisitos de publicação.

## <a name="publishing-options"></a>Opções de publicação

Há três opções principais para as fontes de pacote configuráveis com suporte do centro de administração do Windows:
* Feed do NuGet do centro de administração do Windows público da Microsoft
* Seu próprio feed do NuGet privado
* Compartilhamento de arquivos local ou de rede

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Publicando no feed de extensão do centro de administração do Windows

Por padrão, o centro de administração do Windows está conectado a um feed do NuGet mantido pela equipe de produto do centro de administração do Windows na Microsoft. As versões prévias das novas extensões desenvolvidas pela Microsoft podem ser publicadas neste feed e disponíveis para os usuários do centro de administração do Windows. Os desenvolvedores externos que planejam criar e liberar extensões publicamente também podem [Enviar uma solicitação](#publishing-your-extension-to-the-windows-admin-center-feed) para publicar nesse feed.

### <a name="publishing-to-a-different-nuget-feed"></a>Publicando em um feed do NuGet diferente

Você também pode criar seu próprio feed do NuGet para publicar suas extensões usando uma das várias [opções diferentes para configurar uma fonte privada ou usar um serviço de hospedagem do NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). O feed do NuGet deve dar suporte à API NuGet v2. Como o centro de administração do Windows atualmente não dá suporte à autenticação de feeds, o feed precisa ser configurado para permitir o acesso de leitura a qualquer pessoa.

### <a name="publishing-to-a-file-share"></a>Publicando em um compartilhamento de arquivos

Para restringir o acesso de sua extensão à sua organização ou a um grupo limitado de pessoas, você pode usar um compartilhamento de arquivos SMB como um feed de extensão. Nesse caso, as permissões de pasta e compartilhamento de arquivos serão aplicadas para permitir o acesso ao feed.

## <a name="preparing-your-extension-for-release"></a>Preparando sua extensão para a versão

Certifique-se de ler e considerar os seguintes tópicos de desenvolvimento:

- [Controlar a visibilidade da ferramenta](guides/dynamic-tool-display.md)
- [Cadeias de caracteres e localização](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Considere lançar como uma versão de visualização

Se você estiver lançando uma versão prévia da sua extensão para fins de avaliação, recomendamos que você:

- Acrescente "(Preview)" ao final do título da extensão no arquivo. nuspec
- Explique as limitações na descrição da extensão no arquivo. nuspec

## <a name="creating-an-extension-package"></a>Criando um pacote de extensão

O centro de administração do Windows utiliza pacotes e feeds do NuGet para distribuir e baixar extensões.  Para que o pacote seja enviado, você precisará gerar um pacote NuGet contendo seus plug-ins e extensões.  Um único pacote pode conter uma extensão de interface do usuário, bem como um plug-in de gateway, e a seção a seguir explicará o processo.

### <a name="1-build-your-extension"></a>1. Compilar sua extensão

Assim que estiver pronto para começar a empacotar sua extensão, crie um novo diretório no sistema de arquivos, abra um console do e o CD nele.  Esse será o diretório raiz que usaremos para conter todos os diretórios nuspec e Content que criarão o nosso pacote.  Iremos fazer referência a essa pasta como "pacote NuGet" durante este documento.

#### <a name="ui-extensions"></a>Extensões de interface do usuário

Para iniciar o processo de coleta de todo o conteúdo necessário para uma extensão de interface do usuário, execute "Build Gulp" em sua ferramenta e verifique se a compilação foi bem-sucedida.  Esse processo empacota todos os componentes juntos em uma pasta chamada "Bundle" localizada no diretório raiz da extensão (no mesmo nível do diretório src).  Copie esse diretório e todo o conteúdo para a pasta "pacote NuGet".

#### <a name="gateway-plugins"></a>Plugins de gateway

Usando sua infraestrutura de compilação (isso pode ser tão simples quanto abrir o Visual Studio e clicar no botão Compilar), compilar e compilar seu plug-in.  Abra o diretório de saída da compilação e copie as DLLs que representam o plug-in e coloque-as em uma nova pasta dentro do diretório "pacote NuGet" chamado "pacote".  Você não precisa copiar a dll FeatureInterface, apenas as DLLs que representam seu código.

### <a name="2-create-the-nuspec-file"></a>2. Criar o arquivo. nuspec

Para criar o pacote NuGet, você precisa primeiro criar um arquivo. nuspec. Um arquivo. nuspec é um manifesto XML que contém metadados do pacote NuGet. Esse manifesto é usado para compilar o pacote e fornecer informações aos consumidores.  Coloque esse arquivo na raiz da pasta "pacote NuGet".

Aqui está um exemplo de arquivo. nuspec e a lista de propriedades obrigatórias ou recomendadas. Para o esquema completo, consulte a [referência. nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Salve o arquivo. nuspec na pasta raiz do projeto com um nome de arquivo de sua escolha.

> [!IMPORTANT]
> O ```<id>``` valor no arquivo. nuspec precisa ```"name"``` corresponder ao valor no arquivo do ```manifest.json``` projeto ou, caso contrário, a extensão publicada não será carregada com êxito no centro de administração do Windows.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <packageTypes>
      <packageType name="WindowsAdminCenterExtension" />
    </packageTypes>  
    <id>contoso.project.extension</id>
    <version>1.0.0</version>
    <title>Contoso Hello Extension</title>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.hello-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Hello World extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
    <tags></tags>
  </metadata>
  <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
  </files>
</package>
```

#### <a name="required-or-recommended-properties"></a>Propriedades obrigatórias ou recomendadas

| Nome da Propriedade | Obrigatório/recomendado | Descrição |
| ---- | ---- | ---- |
| PackageType | Obrigatório | Use "WindowsAdminCenterExtension", que é o tipo de pacote NuGet definido para extensões do centro de administração do Windows. |
| id | Obrigatório | Identificador de pacote exclusivo no feed. Esse valor precisa corresponder ao valor de "nome" no arquivo manifest. JSON do seu projeto.  Consulte [escolhendo um identificador de pacote exclusivo](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) para obter diretrizes. |
| title | Necessário para publicar no feed do centro de administração do Windows | Nome amigável para o pacote que é exibido no Gerenciador de extensões do centro de administração do Windows. |
| version | Obrigatório | Versão da extensão. O uso [de controle de versão semântico (Convenção de SemVer)](http://semver.org/spec/v1.0.0.html) é recomendado, mas não obrigatório. |
| Author | Obrigatório | Se estiver publicando em nome da sua empresa, use o nome da empresa. |
| description | Obrigatório | Forneça uma descrição da funcionalidade da extensão. |
| IconUrl | Recomendado ao publicar no feed do centro de administração do Windows | URL para o ícone a ser exibido no Gerenciador de extensões. |
| ProjectUrl | Necessário para publicar no feed do centro de administração do Windows | URL para o site da sua extensão. Se você não tiver um site separado, use a URL para a página da Web do pacote no feed do NuGet. |
| LicenseUrl | Necessário para publicar no feed do centro de administração do Windows | URL para o contrato de licença de usuário final da extensão. |
| files | Obrigatório | Essas duas configurações configuram a estrutura de pastas esperada pelo centro de administração do Windows para extensões de interface do usuário e plug-ins de gateway. |

### <a name="3-build-the-extension-nuget-package"></a>3. Compilar o pacote NuGet de extensão

Usando o arquivo. nuspec criado acima, você criará agora o arquivo. nupkg do pacote NuGet que você pode carregar e publicar no feed do NuGet.

1. Baixe a ferramenta da CLI do NuGet. exe no [site de ferramentas de cliente do NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Execute "NuGet. exe Pack [. nuspec nome do arquivo]" para criar o arquivo. nupkg.

### <a name="4-signing-your-extension-nuget-package"></a>4. Assinando seu pacote NuGet de extensão

Qualquer arquivo. dll incluído em sua extensão deve ser assinado com um certificado de uma autoridade de certificação (CA) confiável. Por padrão, os arquivos. dll não assinados serão impedidos de serem executados quando o centro de administração do Windows estiver sendo executado no modo de produção.

Também é altamente recomendável que você assine o pacote NuGet da extensão para garantir a integridade do pacote, mas essa não é uma etapa necessária.

### <a name="5-test-your-extension-nuget-package"></a>5. Testar seu pacote NuGet de extensão

Seu pacote de extensão agora está pronto para teste! Carregue o arquivo. nupkg em um feed do NuGet ou copie-o para um compartilhamento de arquivos. Para exibir e baixar pacotes de um compartilhamento de arquivos ou feed diferente, você precisará [alterar a configuração do feed](../configure/using-extensions.md#installing-extensions-from-a-different-feed) para apontar para o compartilhamento de arquivos ou feed do NuGet. Ao testar, verifique se as propriedades são exibidas corretamente no Gerenciador de extensões e se você pode instalar e desinstalar com êxito sua extensão.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Publicando sua extensão no feed do centro de administração do Windows

Ao publicar no feed do centro de administração do Windows, você pode disponibilizar sua extensão para qualquer usuário do centro de administração do Windows. Como o SDK do centro de administração do Windows ainda está em versão prévia, gostaríamos de trabalhar em conjunto com você para ajudar a resolver problemas de desenvolvimento e garantir que você possa fornecer um produto de qualidade e uma experiência aos seus usuários.

Antes de liberar a versão inicial de sua extensão, recomendamos que você envie uma solicitação de revisão de extensão para a Microsoft pelo menos 2-3 semanas antes do lançamento para garantir que tenhamos tempo suficiente para revisar e fazer qualquer alteração em sua extensão, se necessário. Depois que sua extensão estiver pronta para ser publicada, você precisará enviá-la a nós para revisão e, se aprovada, publicaremos no feed para você.

Posteriormente, se você quiser liberar uma atualização para sua extensão, será necessário enviar outra solicitação de revisão. Embora dependendo do escopo da alteração, o tempo de retorno das revisões de atualização deve ser geralmente mais curto.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Enviar uma solicitação de revisão de extensão para a Microsoft

Para enviar uma solicitação de revisão de extensão, insira as informações a seguir e envie como [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)um email para. Responderemos ao seu email dentro de uma semana.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>Enviar seu pacote de extensão para revisão e publicação

Certifique-se de seguir as instruções acima para [criar um pacote de extensão](#creating-an-extension-package) e o arquivo. nuspec está definido corretamente e os arquivos são assinados. Também recomendamos que você tenha um site do projeto, incluindo o seguinte:

- Descrição detalhada de sua extensão, incluindo capturas de tela ou vídeo
- Endereço de email ou recurso de site para receber comentários ou perguntas

Quando você estiver pronto para publicar sua extensão, envie um email para [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) e forneceremos instruções sobre como nos enviar seu pacote de extensão. Depois de recebermos seu pacote, vamos revisar e, se aprovado, publicar no feed do centro de administração do Windows.