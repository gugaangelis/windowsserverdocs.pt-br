---
title: Instalar e gerenciar extensões
description: Instalar e gerenciar extensões no Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c775dd5a3011115bbb031c0b9e4e24a8911d378e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296698"
---
# Instalar e gerenciar extensões

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Windows Admin Center é criado como uma plataforma extensível onde cada tipo de conexão e a ferramenta é uma extensão que você pode instalar, desinstalar e atualizar individualmente. Você pode procurar novas extensões publicadas pela Microsoft e por outros desenvolvedores e instalar e atualizá-las individualmente, sem precisar atualizar a instalação do Windows Admin Center inteira. Você também pode configurar um feed do NuGet separado ou compartilhamento de arquivo e distribuir extensões usar internamente em sua organização.

## Instalar uma extensão

Windows Admin Center mostrará extensões disponíveis do NuGet especificado feed. Por padrão, o Windows Admin Center aponta para o NuGet oficial da Microsoft feed que hospeda extensões publicadas pela Microsoft e por outros desenvolvedores.

1. Clique no botão **configurações** no gt _ superior direito no painel esquerdo, clique em **extensões**. 
2. Na guia **Extensões disponíveis** listará as extensões no feed que estão disponíveis para instalação.
3. Clique em uma extensão para exibir a descrição de extensão, versão, fornecedor e outras informações no painel de **detalhes** .
4. Clique em **instalar** para instalar uma extensão. Se o gateway deve ser executadas em modo elevado para fazer essa alteração, você receberá uma solicitação de elevação do UAC. Depois que a instalação for concluída, seu navegador será atualizado automaticamente e o Windows Admin Center será recarregado com a nova extensão instalada. Se a extensão que você está tentando instalar é uma atualização para uma extensão instalada anteriormente, você pode clicar no botão **atualizar para informações mais recentes** para instalar a atualização. Você também pode ir para a guia **Extensões instaladas** para exibir extensões instaladas e verificar se uma atualização está disponível na coluna **Status** .

## Instalando extensões de um feed diferente

Windows Admin Center oferece suporte a vários feeds e você pode exibir e gerenciar pacotes de mais de um feed por vez. Qualquer NuGet feed que ofereça suporte às APIs do NuGet V2 ou um compartilhamento de arquivos pode ser adicionado ao Windows Admin Center para a instalação de extensões do.

1. Clique no botão **configurações** no gt _ superior direito no painel esquerdo, clique em **extensões**.
2. No painel direito, clique na guia **Feeds** .
3. Clique no botão **Adicionar** para adicionar outro feed. Para um feed do NuGet, insira o V2 NuGet URL de feed. O NuGet feed provedor ou administrador deve ser capaz de fornecer as informações de URL. Para um compartilhamento de arquivo, insira o caminho completo do arquivo de compartilhamento no qual os arquivos do pacote de extensão (nupkg) são armazenados.
4. Clique em **Adicionar**. Se o gateway deve ser executadas em modo elevado para fazer essa alteração, você receberá uma solicitação de elevação do UAC.

A lista de **Extensões disponíveis** mostrará as extensões de todos os feeds registrados. Você pode verificar quais feed cada extensão é de usando a coluna de **Feed de pacote** .

## Desinstalando uma extensão

Você pode desinstalar qualquer extensão que você tenha instalado anteriormente, ou até mesmo desinstalar quaisquer ferramentas que foram pré-instalados como parte da instalação do Windows Admin Center.

1. Clique no botão **configurações** no gt _ superior direito no painel esquerdo, clique em **extensões**. 
2. Clique na guia **Extensões instaladas** para exibir as extensões todos instaladas.
3. Escolha uma extensão para desinstalar e clique em **desinstalar**.

Após a desinstalação é concluída, seu navegador será atualizado automaticamente e o Windows Admin Center será recarregado com a extensão removida. Se você desinstalou uma ferramenta que foi pré-instalado como parte do Windows Admin Center, a ferramenta estará disponível para reinstalação na guia **Extensões disponíveis** .

## Instalar extensões em um computador sem conectividade com a internet

Se o Windows Admin Center é instalado em um computador que não está conectado à internet ou por trás de um proxy, ele não poderá acessar e instalar as extensões de feed do Centro de administração do Windows. Você pode baixar pacotes de extensão manualmente ou com um script do PowerShell e configurar o Windows Admin Center para recuperar pacotes de um compartilhamento de arquivos ou unidade local.

### Baixar manualmente os pacotes de extensão

1. Em outro computador que tenha conectividade com a internet, abra um navegador da web e navegue até a seguinte URL:[https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * Talvez seja necessário criar uma conta no msft-sme.myget.org e logon para exibir os pacotes de extensão.

2. Clique no nome do pacote que você deseja instalar para exibir a página de detalhes do pacote.
3. Clique no link **Baixar** no painel do lado direito da página de detalhes do pacote e baixar o arquivo. nupkg para a extensão.
4. Repita as etapas 2 e 3 para todos os pacotes que você deseja baixar.
5. Copie os arquivos de pacote para um compartilhamento de arquivos que pode ser acessado do Windows Admin Center está instalado no computador, ou para o disco local do computador.
6. [Siga as instruções para instalar as extensões de um feed diferente](#installing-extensions-from-a-different-feed).

### Baixar pacotes com um script do PowerShell

Há muitos scripts disponíveis na Internet para baixar pacotes NuGet de um feed de NuGet. Vamos usar o [script fornecido pelo servidor Jon Gilberto](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), gerente de programa sênior da Microsoft.

1. Conforme descrito a [postagem de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), instalar o script como um pacote NuGet, ou copiar e colar o script em ISE do PowerShell.
2. Editar que a primeira linha do script para o NuGet feed's v2 URL. Se você for baixar pacotes do Windows Admin Center oficial feed, use a URL abaixo.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Execute o script e ele baixará todos os pacotes NuGet do feed para a pasta local a seguir: %USERPROFILE%\Documents\NuGetLocal
4. [Siga as instruções para instalar as extensões de um feed diferente](#installing-extensions-from-a-different-feed).

## Gerenciar extensões com o PowerShell

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Windows Admin Center Preview inclui um módulo do PowerShell para gerenciar suas extensões de gateway.

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

### [Saiba mais sobre como criar uma extensão com o SDK do Windows Admin Center](../extend/extensibility-overview.md).