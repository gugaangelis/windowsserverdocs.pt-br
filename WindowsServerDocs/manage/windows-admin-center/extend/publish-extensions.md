---
title: Extensões de publicação para o Windows Admin Center
description: Extensões de publicação para o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783748"
---
# Extensões de publicação

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Depois que você já desenvolveu sua extensão, você desejará publicá-lo e disponibilizá-lo a outras pessoas para testar ou usar. Dependendo do seu público-alvo e a finalidade de publicação, há algumas opções que vai Apresentamos abaixo juntamente com as etapas e os requisitos para publicação.

## Opções de publicação

Há três opções principais para fontes de pacote configurável que dá suporte ao Windows Admin Center:
* Feed pública Windows Admin Center NuGet da Microsoft
* Seu próprio feed do NuGet particular
* Local ou compartilhamento de arquivos de rede

### Publicação para o feed de extensão do Windows Admin Center

Por padrão, o Windows Admin Center é conectado a um NuGet feed mantido pela equipe de produto do Windows Admin Center na Microsoft. Versões de visualização antecipada de novas extensões desenvolvidas pela Microsoft podem ser publicados neste feed e disponíveis para os usuários do Windows Admin Center. Desenvolvedores externos planejamento para compilar e liberar extensões publicamente também podem [Enviar uma solicitação](#publishing-your-extension-to-the-windows-admin-center-feed) para publicar o feed.

### Feed de publicação para um NuGet diferente

Você também pode criar seu próprios NuGet feed para publicar suas extensões usando uma das muitas [Opções diferentes para configurar uma fonte particular ou usar um NuGet que hospeda o serviço](https://docs.microsoft.com/nuget/hosting-packages/overview). O feed do NuGet deve dar suporte a API de v2 NuGet. Como o Windows Admin Center atualmente não oferece suporte para autenticação de feed, o feed deve ser configurado para permitir o acesso de leitura para qualquer pessoa.

### Publicar em um compartilhamento de arquivos

Para restringir o acesso de sua extensão para sua organização ou para um grupo de pessoas limitado, você pode usar um compartilhamento de arquivos SMB como um feed de extensão. Nesse caso, as permissões de compartilhamento e pasta de arquivo serão aplicadas para permitir o acesso ao feed.

## Preparando sua extensão para lançamento

Certifique-se de ler e considerar os seguintes tópicos de desenvolvimento:

- [Controlar a visibilidade da ferramenta](guides/dynamic-tool-display.md)
- [Cadeias de caracteres e localização](guides/strings-localization.md)

### Pense em Lançar como uma versão de visualização

Se você estiver lançando uma versão prévia do sua extensão para fins de avaliação, recomendamos que você:

- Acrescente "(visualização)" ao final do título da extensão no arquivo .nuspec
- Explique as limitações na descrição da extensão no arquivo .nuspec

## Criar um pacote de extensão

Windows Admin Center utiliza pacotes NuGet e feeds para distribuir e baixar extensões.  Na ordem para o pacote a ser enviados, você precisará gerar um pacote NuGet que contém seu plug-ins e extensões.  Um único pacote pode conter uma extensão de interface do usuário, bem como um plug-in de Gateway e a seção a seguir orientará você durante o processo.

### 1. criar sua extensão

Assim que estiver pronto para iniciar a sua extensão de empacotamento, crie um novo diretório no sistema de arquivos, abra um console e CD nela.  Esse será o diretório raiz que usaremos para conter todas as pastas nuspec e conteúdo que irão compor o nosso pacote.  Podemos fará referência a essa pasta como "Pacote NuGet" para a duração deste documento.

#### Extensões de interface do usuário

Para iniciar o processo em todo o conteúdo necessário para uma extensão de interface do usuário de coleta, execute "gulp compilação" em sua ferramenta e verifique se que a compilação for bem-sucedida.  Esse pacotes do processo todos os componentes juntos em uma pasta chamada "bundle" localizados no diretório raiz da sua extensão (no mesmo nível da pasta src).  Copie esse diretório e todos os-lo do conteúdo para a pasta "Pacote NuGet".

#### Gateway plug-ins

Usando sua infraestrutura de compilação (Isso pode ser tão simple quanto abrir o Visual Studio e clicando no botão de compilação), compilar e compile o plug-in.  Abra o seu diretório de saída de compilação e copiar o Dll(s) que representam o plug-in e colocá-los em uma nova pasta dentro do diretório "Pacote NuGet" chamado de "pacote".  Você não precisa copiar a dll FeatureInterface, apenas o Dll(s) que representam o seu código.

### 2. criar o arquivo .nuspec

Para criar o pacote NuGet, você precisa primeiro criar um arquivo de .nuspec. Um arquivo .nuspec é um manifesto XML que contém metadados do pacote NuGet. Esse manifesto é usado para compilar o pacote e fornecer informações para os consumidores.  Coloque este arquivo na raiz da pasta "Pacote NuGet".

Aqui está um exemplo de arquivo de .nuspec e a lista de propriedades necessárias ou recomendadas. Para o esquema completo, consulte a [referência de .nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Salve o arquivo .nuspec para a pasta de raiz do seu projeto com um nome de arquivo de sua preferência.

> [!IMPORTANT]
> O ```<id>``` valor no arquivo .nuspec precisa corresponder a ```"name"``` valor em seu projeto ```manifest.json``` arquivo, senão sua extensão publicado não serão carregados com êxito no Windows Admin Center.

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

#### Necessários ou recomendados propriedades

| Nome da propriedade | Necessária / recomendado | Descrição |
| ---- | ---- | ---- |
| tipo de pacote | Obrigatório | Use "WindowsAdminCenterExtension", que é o tipo de pacote NuGet definido para extensões do Windows Admin Center. |
| id | Obrigatório | Identificador exclusivo do pacote dentro do feed. Esse valor deve corresponder ao valor de "name" no arquivo de manifest.json do seu projeto.  Consulte a [Escolher um identificador exclusivo de pacote](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) para obter orientação. |
| title | Necessário para o Windows Admin Center feed publicação | Nome amigável para o pacote que é exibido no Windows Admin Center extensão Manager. |
| version | Obrigatório | Versão de extensão. Usando a [Semântica Versionamento (SemVer convenção)](http://semver.org/spec/v1.0.0.html) é recomendável, mas não é necessário. |
| autores | Obrigatório | Se a publicação em nome de sua empresa, use o nome da empresa. |
| description | Obrigatório | Forneça uma descrição da funcionalidade da extensão. |
| iconUrl | Recomendado na publicação para o Windows Admin Center feed | URL de ícone para exibir o Gerenciador de extensão. |
| projectUrl | Necessário para o Windows Admin Center feed publicação | URL para o site da extensão. Se você não tiver um site separado, use a URL da página da Web do pacote sobre o NuGet feed. |
| licenseUrl | Necessário para o Windows Admin Center feed publicação | URL para o contrato de licença de usuário final da extensão. |
| arquivos | Obrigatório | Essas duas configurações configurar a estrutura de pastas que o Windows Admin Center espera para extensões de interface do usuário e Gateway plug-ins. |

### 3. criar o pacote NuGet de extensão

Usando o arquivo .nuspec criado anteriormente, agora você criará o arquivo do nupkg pacote NuGet que você pode carregar e publicar o NuGet feed.

1. Baixe a ferramenta CLI nuget.exe do [site de ferramentas de cliente do NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Execute "nuget.exe pack [nome do arquivo .nuspec]" para criar o arquivo. nupkg.

### 4. assinatura do pacote do NuGet extensão

Todos os arquivos. dll incluídos na sua extensão são necessários para ser assinado com um certificado de uma autoridade de certificação confiável (AC). Por padrão, arquivos. dll não assinados serão impedidos de serem executados quando o Windows Admin Center é executado no modo de produção.

Também recomendamos que você assinar o pacote NuGet de extensão para garantir a integridade do pacote, mas isso não é uma etapa necessária.

### 5. testar seu pacote NuGet de extensão

O pacote de extensão agora está pronto para teste! Carregar o arquivo. nupkg um feed do NuGet ou copiá-lo para um compartilhamento de arquivos. Para exibir e baixar pacotes de um feed diferente ou compartilhamento de arquivos, você vai precisar [alterar a configuração de feed](../configure/using-extensions.md#installing-extensions-from-a-different-feed) para apontar para o NuGet feed ou o arquivo compartilhar. Quando o teste, verifique se as propriedades são exibidas corretamente no Gerenciador de extensão e com êxito, você pode instalar e desinstalar sua extensão.

## Publicar sua extensão para o Windows Admin Center feed

Publicando para o Windows Admin Center feed, você pode tornar sua extensão disponíveis para qualquer usuário do Windows Admin Center. Desde que o SDK do Windows Admin Center ainda está em visualização, gostaríamos trabalham em conjunto com você para ajudar a resolver problemas de desenvolvimento e, verifique se que você é capaz de fornecer um produto de qualidade e experiência para seus usuários.

Antes de liberar a versão inicial da sua extensão, recomendamos que você envia uma solicitação de revisão da extensão à Microsoft pelo menos 2 a 3 semanas antes do lançamento para garantir que temos tempo suficiente para revisar e para fazer alterações em sua extensão, se necessário. Depois que sua extensão está pronto para ser publicado, você precisará enviá-lo para nós para revisão e se aprovada, nós vai publicá-lo o feed para você.

Posteriormente, se você deseja liberar uma atualização para sua extensão, você precisará enviar outra solicitação de revisão. Enquanto dependendo do escopo de alteração, o tempo de retorno para avaliações de atualização de geralmente deve ser menor.

### Enviar uma solicitação de revisão de extensão para a Microsoft

Para enviar uma solicitação de revisão de extensão, insira as seguintes informações e envie como um email para [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Podemos responderá ao seu email dentro de uma semana.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### Envie seu pacote de extensão para revisão e publicação

Verifique se você seguir as instruções acima para a [criação de um pacote de extensão](#creating-an-extension-package) e o arquivo .nuspec é definido corretamente e arquivos são assinados. Também recomendamos que você tenha um site de projeto, incluindo o seguinte:

- Descrição detalhada da sua extensão incluindo capturas de tela ou vídeos
- Recurso de site ou endereço de email para receber comentários ou perguntas

Quando você estiver pronto para publicar sua extensão, enviar um email para [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) e forneceremos instruções sobre como para nos enviar seu pacote de extensão. Depois que recebemos seu pacote, vamos analisará e se aprovada, publicar o feed do Windows Admin Center.