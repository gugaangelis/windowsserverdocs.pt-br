---
title: Publicando extensões para o Windows Admin Center
description: Publicando extensões para o Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820417"
---
# <a name="publishing-extensions"></a>Extensões de publicação

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Depois que você desenvolveu sua extensão, você desejará publicá-lo e disponibilizá-lo a outras pessoas para testar ou usar. Dependendo do público-alvo e a finalidade da publicação, há algumas opções que apresentaremos abaixo junto com as etapas e os requisitos para publicação.

## <a name="publishing-options"></a>Opções de publicação

Há três opções principais para origens de pacote configurável que dá suporte ao Windows Admin Center:
* Público Windows Admin Center feed do NuGet da Microsoft
* Seu próprio feed do NuGet privado
* Local ou compartilhamento de arquivos de rede

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Publicar a extensão Windows Admin Center do feed

Por padrão, o Windows Admin Center é conectado a um NuGet feed mantido pela equipe de produto do Windows Admin Center da Microsoft. Versões anteriores de visualização de novas extensões desenvolvidas pela Microsoft podem ser publicados neste feed e disponíveis para os usuários do Windows Admin Center. Desenvolvedores externos, planejamento para compilar e liberar extensões publicamente também poderão [enviar uma solicitação](#publishing-your-extension-to-the-windows-admin-center-feed) para publicar neste feed.

### <a name="publishing-to-a-different-nuget-feed"></a>Feed de publicação em um diferente do NuGet

Você também pode criar seu próprio feed para publicar suas extensões ao uso de um dos muitos do NuGet [diferentes opções para configurar uma origem privada ou usar um serviço de hospedagem de NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). O feed do NuGet deve dar suporte a API do NuGet v2. Como o Windows Admin Center atualmente não oferece suporte para autenticação de alimentação, o feed deve ser configurado para permitir o acesso de leitura para qualquer pessoa.

### <a name="publishing-to-a-file-share"></a>Publicando em um compartilhamento de arquivos

Para restringir o acesso de sua extensão para sua organização ou para um grupo limitado de pessoas, você pode usar um compartilhamento de arquivos SMB como uma extensão do feed. Nesse caso, as permissões de compartilhamento e a pasta do arquivo serão aplicadas para permitir o acesso ao feed.

## <a name="preparing-your-extension-for-release"></a>Preparar sua extensão para lançamento

Certifique-se de ler e considere os seguintes tópicos de desenvolvimento:

- [Controlar a visibilidade da sua ferramenta](guides/dynamic-tool-display.md)
- [Cadeias de caracteres e localização](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Considerariam o lançamento de uma versão de visualização

Se você estiver liberando uma versão de visualização de sua extensão para fins de avaliação, é recomendável que você:

- Acrescentar "(visualização)" ao final do título da sua extensão no arquivo. NuSpec
- Explique as limitações na descrição da sua extensão no arquivo. NuSpec

## <a name="creating-an-extension-package"></a>Criando um pacote de extensão

Windows Admin Center utiliza os pacotes do NuGet e feeds para distribuir e baixar extensões.  Em ordem para o pacote a ser enviado, você precisará gerar um pacote do NuGet contendo seus plug-ins e extensões.  Um único pacote pode conter uma extensão de interface do usuário, bem como um plug-in de Gateway e a seção a seguir orientará você pelo processo.

### <a name="1-build-your-extension"></a>1. Compile sua extensão

Assim que você está pronto para iniciar a empacotar sua extensão, crie um novo diretório no sistema de arquivos, abra um console e CD para ele.  Esse será o diretório raiz que usaremos para conter todos os diretórios de nuspec e o conteúdo que irão compor o nosso pacote.  Faremos referência a essa pasta como "Pacote do NuGet" para a duração deste documento.

#### <a name="ui-extensions"></a>Extensões de interface do usuário

Para iniciar o processo em todo o conteúdo necessário para uma extensão de interface do usuário de coleta, execute "gulp compilação" em sua ferramenta e certifique-se que a compilação for bem-sucedida.  Esse pacotes processo todos os componentes juntos em uma pasta chamada "pacote" localizados no diretório raiz da sua extensão (no mesmo nível do diretório src).  Copie esse diretório e todos os é um conteúdo para a pasta "Pacote do NuGet".

#### <a name="gateway-plugins"></a>Plug-ins de gateway

Usando sua infraestrutura de compilação (Isso pode ser tão simple quanto abrir o Visual Studio e clicando no botão de compilação), compilar e criar seu plug-in.  Abra seu diretório de saída de compilação e copiar os dll que representam seu plug-in e colocá-los em uma nova pasta dentro do diretório de "Pacote do NuGet" chamado "package".  Você não precisará copiar a dll FeatureInterface, apenas os dll que representam seu código.

### <a name="2-create-the-nuspec-file"></a>2. Criar o arquivo. NuSpec

Para criar o pacote do NuGet, você precisa primeiro criar um arquivo. NuSpec. Um arquivo. NuSpec é um manifesto XML que contém metadados de pacote do NuGet. Esse manifesto é usado para compilar o pacote e fornecer informações aos consumidores.  Coloque esse arquivo na raiz da pasta "Pacote do NuGet".

Aqui está um exemplo de arquivo. NuSpec e a lista de propriedades obrigatórias ou recomendadas. Para o esquema completo, consulte o [referência do. NuSpec](https://docs.microsoft.com/nuget/reference/nuspec). Salve o arquivo. NuSpec para sua pasta do projeto raiz com um nome de arquivo de sua escolha.

> [!IMPORTANT]
> O ```<id>``` precisa corresponder ao valor no arquivo. NuSpec a ```"name"``` valor em seu projeto ```manifest.json``` arquivo; caso contrário, sua extensão publicado não é possível carregar com êxito no Windows Admin Center.

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

#### <a name="required-or-recommended-properties"></a>Necessários ou recomendados propriedades

| Nome da propriedade | Obrigatório / recomendado | Descrição |
| ---- | ---- | ---- |
| packageType | Obrigatório | Use "WindowsAdminCenterExtension", que é o tipo de pacote do NuGet definido para extensões do Windows Admin Center. |
| id | Obrigatório | Identificador exclusivo do pacote dentro do feed. Esse valor precisa corresponder ao valor de "name" no arquivo manifest. JSON de seu projeto.  Ver [escolhendo um identificador de pacote exclusivo](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) para obter orientação. |
| title | Necessário para a publicação para o do Windows Admin Center do feed | Nome amigável para o pacote que é exibido no Windows Admin Center Extension Manager. |
| version | Obrigatório | Versão da extensão. Usando o [controle de versão semântico (convenção de SemVer)](http://semver.org/spec/v1.0.0.html) é recomendado, mas não obrigatório. |
| Autores | Obrigatório | Se publicar em nome de sua empresa, use o nome da sua empresa. |
| descrição | Obrigatório | Forneça uma descrição da funcionalidade da extensão. |
| iconUrl | Recomendado ao publicar o do Windows Admin Center do feed | URL de ícone a ser exibida no Gerenciador de extensões. |
| projectUrl | Necessário para a publicação para o do Windows Admin Center do feed | URL para o site da sua extensão. Se você não tiver um site separado, use a URL da página da Web do pacote no feed do NuGet. |
| licenseUrl | Necessário para a publicação para o do Windows Admin Center do feed | URL para o contrato de licença de usuário final da sua extensão. |
| arquivos | Obrigatório | Essas duas configurações a estrutura de pasta Windows Admin Center espera para extensões de interface do usuário e plug-ins de Gateway. |

### <a name="3-build-the-extension-nuget-package"></a>3. Crie o pacote do NuGet de extensão

Usando o arquivo. NuSpec criada acima, agora você criará o arquivo. nupkg de pacote do NuGet que você pode carregar e publicar no feed do NuGet.

1. Baixar a ferramenta CLI nuget.exe a [site de ferramentas de cliente do NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Execute o "pacote de nuget.exe [nome do arquivo. NuSpec]" para criar o arquivo. nupkg.

### <a name="4-signing-your-extension-nuget-package"></a>4. Assinar seu pacote do NuGet de extensão

Todos os arquivos. dll incluídos em sua extensão são necessários para ser assinado com um certificado de uma autoridade de certificação confiável (CA). Por padrão, arquivos. dll não assinados serão impedidos de que está sendo executado quando o Windows Admin Center está em execução no modo de produção.

Também recomendamos que você assinar o pacote de NuGet de extensão para garantir a integridade do pacote, mas isso não é uma etapa necessária.

### <a name="5-test-your-extension-nuget-package"></a>5. Testar seu pacote do NuGet de extensão

Seu pacote de extensão agora está pronto para teste! Carregue o arquivo. nupkg para um feed do NuGet ou copiá-lo para um compartilhamento de arquivos. Para exibir e baixar pacotes de um feed diferente ou o compartilhamento de arquivos, você precisará [alterar a configuração do feed](../configure/using-extensions.md#installing-extensions-from-a-different-feed) para apontar para seu feed do NuGet ou compartilhamento de arquivos. Durante o teste, verifique se as propriedades são exibidas corretamente no Gerenciador de extensões e você pode instalar e desinstalar a extensão com êxito.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Publicar sua extensão para o Windows Admin Center do feed

Publicando o do Windows Admin Center feed, você pode disponibilizar sua extensão para qualquer usuário do Windows Admin Center. Uma vez que o SDK do Windows Admin Center ainda está em visualização, gostaríamos de trabalham em conjunto com você para ajudar a resolver problemas de desenvolvimento e, verifique se que você é capaz de entregar um produto de qualidade e experiência para seus usuários.

Antes de lançar a versão inicial da sua extensão, é recomendável que você envie uma solicitação de revisão de extensão para a Microsoft pelo menos 2 a 3 semanas antes do lançamento para garantir que temos tempo suficiente para revisar e para que você possa fazer alterações à sua extensão, se necessário. Depois que sua extensão está pronta para ser publicado, você precisará enviá-lo em contato conosco para revisão e se aprovado, publicaremos-lo para o feed para você.

Depois disso, se desejar liberar uma atualização para sua extensão, você precisará enviar outra solicitação de revisão. Embora dependendo do escopo da alteração, o tempo de resposta para análises de atualização geralmente deve ser menor.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Enviar uma solicitação de revisão de extensão para a Microsoft

Para enviar uma solicitação de revisão de extensão, insira as seguintes informações e enviar como um email para [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Responderemos ao seu email dentro de uma semana.

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

Certifique-se de seguir as instruções acima para [criando um pacote de extensão](#creating-an-extension-package) e o arquivo. NuSpec é definido corretamente e arquivos são assinados. Também recomendamos que você tenha um site de projeto, incluindo o seguinte:

- Descrição detalhada da sua extensão, incluindo capturas de tela ou vídeo
- Recurso de site ou o endereço de email para receber comentários ou perguntas

Quando você estiver pronto para publicar sua extensão, envie um email para [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) e forneceremos instruções sobre como enviar seu pacote de extensão. Depois que recebemos seu pacote, examinaremos e se aprovado, publicar o do Windows Admin Center feed.