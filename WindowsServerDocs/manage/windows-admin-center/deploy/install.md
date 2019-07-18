---
title: Instalar o Windows Admin Center
description: Como instalar o centro de administração do Windows em um computador Windows ou em um servidor para que vários usuários possam acessar o centro de administração do Windows usando um navegador da Web.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 994e2324042dd441abbb114da2b8806574ce0352
ms.sourcegitcommit: e5553285d509f15c20ba98ad9e8bf69b09531560
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68307484"
---
# <a name="install-windows-admin-center"></a>Instalar o Windows Admin Center

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Este tópico descreve como instalar o centro de administração do Windows em um computador Windows ou em um servidor para que vários usuários possam acessar o centro de administração do Windows usando um navegador da Web.

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Determinar o tipo de instalação

Examine as [Opções de instalação](../plan/installation-options.md) que incluem os [sistemas operacionais com suporte](../plan/installation-options.md#supported-operating-systems-installation). Para instalar o centro de administração do Windows em uma VM no Azure, consulte [implantar o centro de administração do Windows no Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Instalar no Windows 10

Quando você instala o Windows Admin Center no Windows 10, ele usa a porta 6516 por padrão, mas você tem a opção de especificar uma porta diferente. Você também pode criar um atalho da área de trabalho e deixar o Windows Admin Center gerenciar os TrustedHosts.

> [!NOTE]
> A modificação de TrustedHosts é necessária em um ambiente de grupo de trabalho ou quando são usadas credenciais de administrador local em um domínio. Caso opte por ignorar essa configuração, você deve [configurar o TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Quando você inicia o Windows Admin Center no menu **Iniciar** , ele é aberto no navegador padrão.

Ao iniciar o Windows Admin Center pela primeira vez, você verá um ícone na área de notificação da sua área de trabalho. Clique com o botão direito do mouse nesse ícone e selecione **Abrir** para abrir a ferramenta no navegador padrão ou escolha **Sair** para encerrar o processo em segundo plano.

## <a name="install-on-windows-server-with-desktop-experience"></a>Instalação no Windows Server com experiência desktop

No Windows Server, o Windows Admin Center é instalado como um serviço de rede. Você deve especificar a porta em que o serviço faz a escuta e requer um certificado para HTTPS. O instalador pode criar um certificado autoassinado para teste ou você pode fornecer a impressão digital de um certificado já instalado no computador. Se você usar o certificado gerado, ele coincidirá com o nome DNS do servidor. Se você usar seu próprio certificado, verifique se o nome fornecido no certificado corresponde ao nome do computador (não há suporte para certificados curinga). Você também terá a opção de permitir que o centro de administração do Windows gerencie seu TrustedHosts.

> [!NOTE]
> A modificação de TrustedHosts é necessária em um ambiente de grupo de trabalho ou quando são usadas credenciais de administrador local em um domínio. Caso opte por ignorar essa configuração, você deve [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Quando a instalação for concluída, abra um navegador de um computador remoto e navegue até a URL apresentada na última etapa do instalador.

> [!WARNING]
> Os certificados gerados automaticamente expiram 60 dias após a instalação.

## <a name="install-on-server-core"></a>Instalar no Server Core

Se você tiver uma instalação Server Core do Windows Server, você pode instalar o Windows Admin Center no prompt de comando (em execução como Administrador). Especifique uma porta e um certificado SSL usando os argumentos `SME_PORT` e `SSL_CERTIFICATE_OPTION` respectivamente. Se for usar um certificado existente, use o `SME_THUMBPRINT` para especificar sua impressão digital.

> [!WARNING]
> A instalação do centro de administração do Windows reiniciará o serviço WinRM, que desconectará todas as sessões remotas do PowerShell. É recomendável que você instale a partir de um cmd ou PowerShell local. Se você estiver instalando o com uma solução de automação que seria interrompida pela reinicialização do serviço WinRM, poderá adicionar ```RESTART_WINRM=0``` o parâmetro aos argumentos de instalação, mas o WinRM deverá ser reiniciado para que o centro de administração do Windows funcione.

Execute o seguinte comando para instalar o Windows Admin Center e gerar automaticamente um certificado autoassinado:

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Execute o seguinte comando para instalar o Windows Admin Center com um certificado existente:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> Não invoque `msiexec` do PowerShell usando a notação de caminho relativo ponto-barra (como, `.\<WindowsAdminCenterInstallerName>.msi`). A instalação falhará, pois não há suporte para essa notação. Remova o prefixo `.\` ou especifique o caminho completo para o MSI.

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>Atualizando para uma nova versão do centro de administração do Windows

É possível atualizar as versões não prévias do Windows Admin Center por meio do Microsoft Update ou pela instalação manual.

Suas configurações são preservadas ao atualizar para uma nova versão do centro de administração do Windows. Não há suporte oficialmente para atualizar as versões do insider preview do centro de administração do Windows – acreditamos que é melhor fazer uma instalação limpa, mas não a bloqueamos.

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Atualizando o certificado usado pelo centro de administração do Windows

Quando o centro de administração do Windows for implantado como um serviço, você deverá fornecer um certificado para HTTPS. Para atualizar esse certificado posteriormente, execute novamente o instalador e escolha ```change```.
