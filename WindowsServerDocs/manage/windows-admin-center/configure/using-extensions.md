---
title: Instalar e gerenciar extensões
description: Instalar e gerenciar extensões no centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d49e25591c705afa217b2332ee48eb42c5c2f7ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357244"
---
# <a name="install-and-manage-extensions"></a>Instalar e gerenciar extensões

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

O centro de administração do Windows é criado como uma plataforma extensível onde cada tipo de conexão e ferramenta é uma extensão que você pode instalar, desinstalar e atualizar individualmente. Você pode pesquisar novas extensões publicadas pela Microsoft e outros desenvolvedores, e instalá-las e atualizá-las individualmente sem precisar atualizar toda a instalação do centro de administração do Windows. Você também pode configurar um feed do NuGet separado ou compartilhamento de arquivos e distribuir extensões para usar internamente em sua organização.

## <a name="installing-an-extension"></a>Instalando uma extensão

O centro de administração do Windows mostrará as extensões disponíveis no feed do NuGet especificado. Por padrão, o centro de administração do Windows aponta para o feed do NuGet oficial da Microsoft que hospeda as extensões publicadas pela Microsoft e por outros desenvolvedores.

1. Clique no botão **configurações** na parte superior direita > no painel esquerdo, clique em **extensões**. 
2. A guia **extensões disponíveis** listará as extensões no feed que estão disponíveis para instalação.
3. Clique em uma extensão para exibir a descrição da extensão, a versão, o Publicador e outras informações no painel de **detalhes** .
4. Clique em **instalar** para instalar uma extensão. Se o gateway precisar ser executado no modo elevado para fazer essa alteração, você receberá uma solicitação de elevação do UAC. Após a conclusão da instalação, seu navegador será atualizado automaticamente e o centro de administração do Windows será recarregado com a nova extensão instalada. Se a extensão que você está tentando instalar for uma atualização para uma extensão instalada anteriormente, você poderá clicar no botão **atualizar para a versão mais recente** para instalar a atualização. Você também pode ir para a guia **extensões instaladas** para exibir as extensões instaladas e ver se uma atualização está disponível na coluna **status** .

## <a name="installing-extensions-from-a-different-feed"></a>Instalando extensões de um feed diferente

O centro de administração do Windows dá suporte a vários feeds e você pode exibir e gerenciar pacotes de mais de um feed de cada vez. Qualquer feed do NuGet que dê suporte às APIs do NuGet v2 ou um compartilhamento de arquivos pode ser adicionado ao centro de administração do Windows para instalar extensões do.

1. Clique no botão **configurações** na parte superior direita > no painel esquerdo, clique em **extensões**.
2. No painel direito, clique na guia **feeds** .
3. Clique no botão **Adicionar** para adicionar outro feed. Para um feed do NuGet, insira a URL do feed v2 do NuGet. O provedor ou o administrador do feed do NuGet deve ser capaz de fornecer as informações de URL. Para um compartilhamento de arquivos, insira o caminho completo do compartilhamento de arquivos no qual os arquivos de pacote de extensão (. nupkg) são armazenados.
4. Clique em **Adicionar** . Se o gateway precisar ser executado no modo elevado para fazer essa alteração, você receberá uma solicitação de elevação do UAC.

A lista de **extensões disponíveis** mostrará extensões de todos os feeds registrados. Você pode verificar qual feed cada extensão é usando a coluna **feed de pacote** .

## <a name="uninstalling-an-extension"></a>Desinstalando uma extensão

Você pode desinstalar todas as extensões instaladas anteriormente ou até mesmo desinstalar todas as ferramentas que foram pré-instaladas como parte da instalação do centro de administração do Windows.

1. Clique no botão **configurações** na parte superior direita > no painel esquerdo, clique em **extensões**. 
2. Clique na guia **extensões instaladas** para exibir todas as extensões instaladas.
3. Escolha uma extensão para desinstalar e clique em **desinstalar**.

Após a conclusão da desinstalação, seu navegador será atualizado automaticamente e o centro de administração do Windows será recarregado com a extensão removida. Se você desinstalou uma ferramenta que foi pré-instalada como parte do centro de administração do Windows, a ferramenta estará disponível para reinstalação na guia **extensões disponíveis** .

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Instalando extensões em um computador sem conectividade com a Internet

Se o centro de administração do Windows estiver instalado em um computador que não está conectado à Internet ou estiver protegido por um proxy, talvez ele não consiga acessar e instalar as extensões do feed do centro de administração do Windows. Você pode baixar pacotes de extensão manualmente ou com um script do PowerShell e configurar o centro de administração do Windows para recuperar pacotes de um compartilhamento de arquivos ou unidade local.

### <a name="manually-downloading-extension-packages"></a>Baixando manualmente pacotes de extensão

1. Em outro computador que tenha conectividade com a Internet, abra um navegador da Web e navegue até a seguinte URL: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * Talvez seja necessário criar uma conta no msft-sme.myget.org e fazer logon para exibir os pacotes de extensão.

2. Clique no nome do pacote que você deseja instalar para exibir a página de detalhes do pacote.
3. Clique no link **baixar** no painel direito da página de detalhes do pacote e baixe o arquivo. nupkg para a extensão.
4. Repita as etapas 2 e 3 para todos os pacotes que você deseja baixar.
5. Copie os arquivos de pacote para um compartilhamento de arquivos que pode ser acessado no computador do centro de administração do Windows instalado em ou no disco local do computador.
6. [Siga as instruções para instalar extensões de um feed diferente](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Baixando pacotes com um script do PowerShell

Há muitos scripts disponíveis na Internet para baixar pacotes NuGet de um feed do NuGet. Usaremos o [script fornecido por Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), gerente de programas sênior da Microsoft.

1. Conforme descrito na [postagem do blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), instale o script como um pacote NuGet, ou copie e cole o script no ISE do PowerShell.
2. Edite a primeira linha do script para a URL v2 do feed do NuGet. Se você estiver baixando pacotes do feed oficial do centro de administração do Windows, use a URL abaixo.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Execute o script e ele baixará todos os pacotes NuGet do feed para a seguinte pasta local:%USERPROFILE%\Documents\NuGetLocal
4. [Siga as instruções para instalar extensões de um feed diferente](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gerenciar extensões com o PowerShell

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

A versão prévia do centro de administração do Windows inclui um módulo do PowerShell para gerenciar suas extensões de gateway.

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[Saiba mais sobre como criar uma extensão com o SDK do centro de administração do Windows](../extend/extensibility-overview.md).