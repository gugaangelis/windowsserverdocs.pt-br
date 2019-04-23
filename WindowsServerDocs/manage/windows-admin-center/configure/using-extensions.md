---
title: Instalar e gerenciar extensões
description: Instalar e gerenciar extensões no Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6091edd7aa7f790f6029ca6b6ae402bf1b7e61ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877017"
---
# <a name="install-and-manage-extensions"></a>Instalar e gerenciar extensões

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center é compilado como uma plataforma extensível onde cada tipo de conexão e a ferramenta são uma extensão que você pode instalar, desinstalar e atualizar individualmente. Você pode pesquisar novas extensões publicadas pela Microsoft e outros desenvolvedores e, em seguida, instalar e atualizá-los individualmente sem a necessidade de atualizar toda a instalação do Windows Admin Center. Você também pode configurar um feed do NuGet separado ou compartilhamento de arquivos e distribuir as extensões para uso interno em sua organização.

## <a name="installing-an-extension"></a>Instalar uma extensão

Windows Admin Center mostrará as extensões disponíveis do feed do NuGet especificado. Por padrão, Windows Admin Center aponta para o Microsoft official feed do NuGet que hospeda extensões publicadas pela Microsoft e outros desenvolvedores.

1. Clique o **as configurações** botão no canto superior direito > no painel esquerdo, clique em **extensões**. 
2. O **extensões disponíveis** guia listará as extensões que estão disponíveis para instalação no feed.
3. Clique em uma extensão para exibir a descrição de extensão, versão, publicador e outras informações na **detalhes** painel.
4. Clique em **instalar** para instalar uma extensão. Se o gateway deve ser executado no modo elevado para fazer essa alteração, você verá um prompt de elevação do UAC. Após a instalação for concluída, seu navegador será atualizado automaticamente e será recarregado Windows Admin Center com a nova extensão instalada. Se a extensão você está tentando instalar é uma atualização para uma extensão instalada anteriormente, você pode clicar o **atualizar para a versão mais recente** botão para instalar a atualização. Você também pode ir para o **extensões instaladas** guia para exibir instalado extensões e veja se uma atualização está disponível na **Status** coluna.

## <a name="installing-extensions-from-a-different-feed"></a>Instalando extensões de um feed de diferente

Windows Admin Center oferece suporte a vários feeds e você pode exibir e gerenciar pacotes de mais de um feed de cada vez. Qualquer NuGet feed que ofereça suporte a APIs do NuGet V2 ou um compartilhamento de arquivos pode ser adicionado ao Windows Admin Center para instalar as extensões da.

1. Clique o **as configurações** botão no canto superior direito > no painel esquerdo, clique em **extensões**.
2. No painel direito, clique no **Feeds** guia.
3. Clique o **adicionar** botão para adicionar outro feed. Para um feed do NuGet, insira o V2 NuGet URL do feed. Provedor de feed do NuGet ou o administrador deve ser capaz de fornecer as informações de URL. Para um compartilhamento de arquivo, insira o caminho completo do arquivo de compartilhamento no qual os arquivos de pacote de extensão (. nupkg) são armazenados.
4. Clique em **Adicionar**. Se o gateway deve ser executado no modo elevado para fazer essa alteração, você verá um prompt de elevação do UAC.

O **extensões disponíveis** lista mostrará as extensões de todos os feeds registrados. Você pode verificar quais feed cada extensão é de usando o **Feed do pacote** coluna.

## <a name="uninstalling-an-extension"></a>Desinstalando uma extensão

Você pode desinstalar qualquer extensão que você já tiver instalado, ou até mesmo desinstalar quaisquer ferramentas que foram previamente instaladas como parte da instalação do Windows Admin Center.

1. Clique o **as configurações** botão no canto superior direito > no painel esquerdo, clique em **extensões**. 
2. Clique o **extensões instaladas** guia para exibir todas as extensões.
3. Escolha uma extensão para desinstalar e, em seguida, clique em **desinstalação**.

Após a desinstalação é concluída, seu navegador será atualizado automaticamente e será recarregado Windows Admin Center com a extensão removida. Se você desinstalou uma ferramenta que foi previamente instalada como parte do Windows Admin Center, a ferramenta estará disponível para instalação do **extensões disponíveis** guia.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Instalando extensões em um computador sem conectividade com a internet

Se o Windows Admin Center é instalado em um computador que não está conectado à internet ou que está atrás de um proxy, ele não poderá acessar e instalar as extensões dos do Windows Admin Center feed. Você pode baixar pacotes de extensão manualmente ou com um script do PowerShell e configurar o Windows Admin Center para recuperar pacotes de um compartilhamento de arquivos ou uma unidade local.

### <a name="manually-downloading-extension-packages"></a>Baixar manualmente os pacotes de extensão

1. Em outro computador que tenha conectividade com a internet, abra um navegador da web e navegue até a URL a seguir: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * Você talvez precise criar uma conta no msft sme.myget.org e faça logon para exibir os pacotes de extensão.

2. Clique no nome do pacote que você deseja instalar para exibir a página de detalhes do pacote.
3. Clique no **baixar** link no painel do lado direito da página de detalhes do pacote e baixe o arquivo. nupkg para a extensão.
4. Repita as etapas 2 e 3 para todos os pacotes que você deseja baixar.
5. Copie os arquivos de pacote para um compartilhamento de arquivos que pode ser acessado do Windows Admin Center está instalado no computador ou no disco local do computador.
6. [Siga as instruções para instalar as extensões de um feed de diferente](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Download de pacotes com um script do PowerShell

Muitos scripts estão disponíveis na Internet para baixar os pacotes do NuGet de um feed do NuGet. Vamos usar o [script fornecido pelo Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), gerente de programa sênior na Microsoft.

1. Conforme descrito na [postagem de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), o script de instalação como um pacote do NuGet, ou copie e cole o script no ISE do PowerShell.
2. Editar que a primeira linha do script para o NuGet feed a URL de v2 do. Se você estiver baixando pacotes a partir de oficial do Windows Admin Center feed, use a URL abaixo.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Execute o script e ele baixará todos os pacotes NuGet do feed na seguinte pasta local: %USERPROFILE%\Documents\NuGetLocal
4. [Siga as instruções para instalar as extensões de um feed de diferente](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gerenciar extensões com o PowerShell

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Visualização do Windows Admin Center inclui um módulo do PowerShell para gerenciar suas extensões de gateway.

>[!IMPORTANT]
>Gerenciamento de extensões de gateway com o módulo do PowerShell tem suporte apenas ao Windows Admin Center é implantado como um serviço de gateway no Windows Server.

```powershell
# Add the module to the current session
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ExtensionTools"
# Available cmdlets: Get-Feed, Add-Feed, Remove-Feed, Get-Extension, Install-Extension, Uninstall-Extension, Update-Extension

# List feeds
Get-Feed "https://wac.contoso.com"

# Add a new extension feed
Add-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# Remove an extension feed
Remove-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# List all extensions
Get-Extension "https://wac.contoso.com"

# Install an extension (locate the latest version from all feeds and install it)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers"

# Install an extension (latest version from a specific feed, if the feed is not present, it will be added)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers" -Feed "https://aka.ms/sme-extension-feed"

# Install an extension (install a specific version)
Install-Extension "https://wac.contoso.com" "msft.sme.certificate-manager" "0.133.0"

# Uninstall-Extension
Uninstall-Extension "https://wac.contoso.com" "msft.sme.containers"

# Update-Extension
Update-Extension "https://wac.contoso.com" "msft.sme.containers"
```

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[Saiba mais sobre a criação de uma extensão com o SDK do Windows Admin Center](../extend/extensibility-overview.md).