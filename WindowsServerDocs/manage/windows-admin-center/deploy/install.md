---
title: Instalar o Windows Admin Center
description: Como instalar o Windows Admin Center em um computador Windows ou em um servidor, de modo que vários usuários possam acessar o Windows Admin Center usando um navegador da Web.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: cab128a3da9fa58c598cebcdf188058631c33977
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950005"
---
# <a name="install-windows-admin-center"></a>Instalar o Windows Admin Center

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Este tópico descreve como instalar o Windows Admin Center em um computador Windows ou em um servidor, de modo que vários usuários possam acessar o Windows Admin Center usando um navegador da Web.

> [!Tip]
> Conhecendo o Windows Admin Center agora?
> [Saiba mais sobre o Windows Admin Center](../overview.md) ou [baixe-o agora](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Determinar o tipo de instalação

Examine as [opções de instalação](../plan/installation-options.md) que incluem os [sistemas operacionais compatíveis](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems). Para instalar o Windows Admin Center em uma VM no Azure, confira [Implantar o Windows Admin Center no Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Instalar no Windows 10

Quando você instala o Windows Admin Center no Windows 10, por padrão, ele usa a porta 6516, mas você tem a opção de especificar outra porta. Você também pode criar um atalho da área de trabalho e deixar o Windows Admin Center gerenciar os TrustedHosts.

> [!NOTE]
> A modificação de TrustedHosts é necessária em um ambiente de grupo de trabalho ou quando são usadas credenciais de administrador local em um domínio. Caso você opte por ignorar essa configuração, [configure o TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Quando você inicia o Windows Admin Center por meio do menu **Iniciar**, ele é aberto no navegador padrão.

Ao iniciar o Windows Admin Center pela primeira vez, você verá um ícone na área de notificação da área de trabalho. Clique com o botão direito do mouse nesse ícone e escolha **Abrir** para abrir a ferramenta no navegador padrão ou **Sair** para encerrar o processo em segundo plano.

## <a name="install-on-windows-server-with-desktop-experience"></a>Instalação no Windows Server com experiência desktop

No Windows Server, o Windows Admin Center é instalado como um serviço de rede. É necessário especificar a porta em que o serviço faz a escuta, e isso exige um certificado para HTTPS. O instalador pode criar um certificado autoassinado para teste ou você pode fornecer a impressão digital de um certificado já instalado no computador. Se você usar o certificado gerado, ele fará a correspondência com o nome DNS do servidor. Se você usar o próprio certificado, verifique se o nome fornecido no certificado corresponde ao nome do computador (não há suporte para certificados curinga). Você também tem a opção de permitir que o Windows Admin Center gerencie o TrustedHosts.

> [!NOTE]
> A modificação de TrustedHosts é necessária em um ambiente de grupo de trabalho ou quando são usadas credenciais de administrador local em um domínio. Caso você opte por ignorar essa configuração, [configure o TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts)

Quando a instalação for concluída, abra um navegador em um computador remoto e navegue até a URL apresentada na última etapa do instalador.

> [!WARNING]
> Os certificados gerados automaticamente expiram 60 dias após a instalação.

## <a name="install-on-server-core"></a>Instalação no Server Core

Caso tenha uma instalação Server Core do Windows Server, instale o Windows Admin Center por meio do prompt de comando (em execução como Administrador). Especifique uma porta e um certificado SSL usando os argumentos `SME_PORT` e `SSL_CERTIFICATE_OPTION`, respectivamente. Caso pretenda usar um certificado existente, use o `SME_THUMBPRINT` para especificar a impressão digital.

> [!WARNING]
> A instalação do Windows Admin Center reiniciará o serviço WinRM, que desconectará todas as sessões remotas do PowerShell. É recomendável que você faça a instalação por meio de um Cmd ou um PowerShell local. Se estiver fazendo a instalação com uma solução de automação que será interrompida pela reinicialização do serviço WinRM, adicione o parâmetro ```RESTART_WINRM=0``` aos argumentos de instalação, mas o WinRM precisará ser reiniciado para que o Windows Admin Center funcione.

Execute o seguinte comando para instalar o Windows Admin Center e gerar automaticamente um certificado autoassinado:

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Execute o seguinte comando para instalar o Windows Admin Center com um certificado existente:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> Não invoque `msiexec` no PowerShell usando a notação de caminho relativo ponto-barra "/" (como, `.\<WindowsAdminCenterInstallerName>.msi`). A instalação falhará, pois não há suporte para essa notação. Remova o prefixo `.\` ou especifique o caminho completo para o MSI.

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>Atualização para uma nova versão do Windows Admin Center

É possível atualizar as versões não prévias do Windows Admin Center por meio do Microsoft Update ou pela instalação manual.

Suas configurações são preservadas ao fazer a atualização para uma nova versão do Windows Admin Center. Não damos suporte oficial à atualização das versões do Insider Preview do Windows Admin Center: acreditamos que é melhor fazer uma instalação limpa, mas não a bloqueamos.

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Atualização do certificado usado pelo Windows Admin Center

Quando o Windows Admin Center é implantado como um serviço, é necessário fornecer um certificado para HTTPS. Para atualizar esse certificado posteriormente, execute novamente o instalador e escolha ```change```.
