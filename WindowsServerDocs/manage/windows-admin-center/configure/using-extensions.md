---
title: Instalar e gerenciar extensões
description: Instalar e gerenciar extensões no Windows Admin Center (Projeto Honolulu)
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.openlocfilehash: c2feaaff614d00afeaf5d132c446eebe5fdf0989
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966773"
---
# <a name="install-and-manage-extensions"></a>Instalar e gerenciar extensões

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

O Windows Admin Center é criado como uma plataforma extensível em que cada tipo de conexão e ferramenta é uma extensão que você pode instalar, desinstalar e atualizar individualmente. Você pode pesquisar novas extensões publicadas pela Microsoft e outros desenvolvedores e instalá-las e atualizá-las individualmente sem precisar atualizar toda a instalação do Windows Admin Center. Você também pode configurar um compartilhamento de arquivo ou feed do NuGet separado e distribuir extensões para usar internamente em sua organização.

## <a name="installing-an-extension"></a>Como instalar uma extensão

O Windows Admin Center mostrará as extensões disponíveis no feed do NuGet especificado. Por padrão, o Windows Admin Center aponta para o feed do NuGet oficial da Microsoft que hospeda as extensões publicadas pela Microsoft e por outros desenvolvedores.

1. Clique no botão **Configurações** no > superior direito. No painel esquerdo, clique em **Extensões**.
2. A guia **Extensões Disponíveis** listará as extensões no feed que estão disponíveis para instalação.
3. Clique em uma extensão para exibir a descrição da extensão, a versão, o editor e outras informações no painel **Detalhes**.
4. Clique em **Instalar** para instalar uma extensão. Se o gateway precisar ser executado no modo elevado para fazer essa alteração, você receberá uma solicitação de elevação do UAC. Após a conclusão da instalação, seu navegador será atualizado automaticamente e o Windows Admin Center será recarregado com a nova extensão instalada. Se a extensão que você está tentando instalar for uma atualização para uma extensão instalada anteriormente, você poderá clicar no botão **Atualizar para a mais recente** para instalar a atualização. Você também pode ir para a guia **Extensões Instaladas** para exibir as extensões instaladas e ver se uma atualização está disponível na coluna **Status**.

## <a name="installing-extensions-from-a-different-feed"></a>Como instalar extensões de um feed diferente

O Windows Admin Center dá suporte a vários feeds e você pode exibir e gerenciar pacotes de mais de um feed de cada vez. Qualquer feed do NuGet que dê suporte às APIs do NuGet V2 ou um compartilhamento de arquivo pode ser adicionado ao Windows Admin Center para instalar extensões.

1. Clique no botão **Configurações** no > superior direito. No painel esquerdo, clique em **Extensões**.
2. No painel direito, clique na guia **Feeds**.
3. Clique no botão **Adicionar** para adicionar outro feed. Para um feed do NuGet, insira a URL do feed do NuGet V2. O provedor ou o administrador do feed do NuGet deve ser capaz de fornecer as informações de URL. Para um compartilhamento de arquivo, insira o caminho completo do compartilhamento de arquivo no qual os arquivos de pacote de extensão (.nupkg) são armazenados.
4. Clique em **Adicionar**. Se o gateway precisar ser executado no modo elevado para fazer essa alteração, você receberá uma solicitação de elevação do UAC. Esse prompt será apresentado somente se você estiver executando o Windows Admin Center no modo de área de trabalho.

A lista **Extensões Disponíveis** mostrará extensões de todos os feeds registrados. Você pode verificar de qual feed cada extensão vem usando a coluna **Feed de Pacote**.

## <a name="uninstalling-an-extension"></a>Como desinstalar uma extensão

Você pode desinstalar todas as extensões instaladas anteriormente ou até mesmo desinstalar todas as ferramentas pré-instaladas como parte da instalação do Windows Admin Center.

1. Clique no botão **Configurações** no > superior direito. No painel esquerdo, clique em **Extensões**.
2. Clique na guia **Extensões Instaladas** para exibir todas as extensões instaladas.
3. Escolha uma extensão para desinstalar e clique em **Desinstalar**.

Após a conclusão da desinstalação, seu navegador será atualizado automaticamente e o Windows Admin Center será recarregado com a extensão removida. Se você desinstalou uma ferramenta pré-instalada como parte do Windows Admin Center, a ferramenta estará disponível para reinstalação na guia **Extensões Disponíveis**.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Como instalar extensões em um computador sem conectividade com a Internet

Se o Windows Admin Center estiver instalado em um computador que não está conectado à Internet ou estiver protegido por um proxy, talvez ele não consiga acessar e instalar as extensões do feed do Windows Admin Center. Você pode baixar pacotes de extensão manualmente ou com um script do PowerShell e configurar o Windows Admin Center para recuperar pacotes de um compartilhamento de arquivo ou unidade local.

### <a name="manually-downloading-extension-packages"></a>Como baixar manualmente pacotes de extensão

1. Em outro computador que tenha conectividade com a Internet, abra um navegador da Web e vá até a seguinte URL: [https://dev.azure.com/WindowsAdminCenter/Windows%20Admin%20Center%20Feed/_packaging?_a=feed&feed=WAC](https://dev.azure.com/WindowsAdminCenter/Windows%20Admin%20Center%20Feed/_packaging?_a=feed&feed=WAC)

   * Talvez seja necessário criar uma conta Microsoft e fazer logon para exibir os pacotes de extensão.

2. Clique no nome do pacote que você deseja instalar para exibir a página de detalhes do pacote.
3. Clique no link **Baixar** na barra de navegação da página de detalhes do pacote e baixe o arquivo .nupkg para a extensão.
4. Repita as etapas 2 e 3 para todos os pacotes que você deseja baixar.
5. Copie os arquivos de pacote para um compartilhamento de arquivo que possa ser acessado no computador em que o Windows Admin Center está instalado ou no disco local do computador.
6. [Siga as instruções para instalar extensões de um feed diferente](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Como baixar pacotes com um script do PowerShell

Há muitos scripts disponíveis na Internet para baixar pacotes NuGet de um feed do NuGet. Usaremos o [script fornecido por Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), gerente de programas sênior da Microsoft.

1. Conforme descrito na [postagem do blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), instale o script como um pacote do NuGet ou copie e cole o script no ISE do PowerShell.
2. Edite a primeira linha do script para a URL v2 do feed do NuGet. Se você estiver baixando pacotes do feed oficial do Windows Admin Center, use a URL abaixo.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Execute o script e ele baixará todos os pacotes NuGet do feed para a seguinte pasta local: %USERPROFILE%\Documents\NuGetLocal
4. [Siga as instruções para instalar extensões de um feed diferente](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gerenciar extensões com o PowerShell

A versão prévia do Windows Admin Center inclui um módulo do PowerShell para gerenciar as extensões de gateway.

[!INCLUDE [ps-extensions](../includes/ps-extensions.md)]

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdk"></a>[Saiba mais sobre como criar uma extensão com o SDK do Windows Admin Center](../extend/extensibility-overview.md).
