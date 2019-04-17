---
title: Instalar o Windows Admin Center
description: Como instalar o Windows Admin Center em um computador Windows ou o servidor.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 496a67cfb93bf9b42b202f6a61211a085a9253b6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296688"
---
# Instalar o Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## Determinar o tipo de instalação

Examine as [Opções de instalação](..\plan\installation-options.md) que inclui o [suporte a sistemas operacionais](..\plan\installation-options.md#supported-operating-systems-installation). Para instalar o Windows Admin Center em uma VM no Azure, consulte [Implantar o Windows Admin Center no Azure](../azure/deploy-wac-in-azure.md).

## Instalar no Windows 10

Quando você instala o Windows Admin Center no Windows 10, ele usa a porta 6516 por padrão, mas você tem a opção de especificar uma porta diferente. Você também pode criar um atalho da área de trabalho e deixar o Windows Admin Center gerenciar os TrustedHosts.

> [!NOTE]
> A modificação de TrustedHosts é necessária em um ambiente de grupo de trabalho ou quando são usadas credenciais de administrador local em um domínio. Caso opte por ignorar essa configuração, você deve [configurar o TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Quando você inicia o Windows Admin Center no menu **Iniciar** , ele é aberto no navegador padrão.

Ao iniciar o Windows Admin Center pela primeira vez, você verá um ícone na área de notificação da sua área de trabalho. Clique com o botão direito do mouse nesse ícone e selecione **Abrir** para abrir a ferramenta no navegador padrão ou escolha **Sair** para encerrar o processo em segundo plano.

## Instalação no Windows Server com experiência desktop

No Windows Server, o Windows Admin Center é instalado como um serviço de rede. Você deve especificar a porta em que o serviço faz a escuta e requer um certificado para HTTPS. O instalador pode criar um certificado autoassinado para teste ou você pode fornecer a impressão digital de um certificado já instalado no computador. Se você usar o certificado gerado, ele coincidirá com o nome DNS do servidor. Se você usar seu próprio certificado, verifique se o nome fornecido no certificado corresponde ao nome do computador (não há suporte para certificados de curinga.) Você também terá a opção de permitir que o Windows Admin Center gerencie seus TrustedHosts.

> [!NOTE]
> A modificação de TrustedHosts é necessária em um ambiente de grupo de trabalho ou quando são usadas credenciais de administrador local em um domínio. Caso opte por ignorar essa configuração, você deve [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Depois que a instalação for concluída, abra um navegador de um computador remoto e navegue até a URL apresentado na última etapa do instalador.

> [!WARNING]
> Os certificados gerados automaticamente expiram 60 dias após a instalação.

## Instalar no Server Core

Se você tiver uma instalação Server Core do Windows Server, você pode instalar o Windows Admin Center no prompt de comando (em execução como Administrador). Especifique uma porta e um certificado SSL usando os argumentos `SME_PORT` e `SSL_CERTIFICATE_OPTION` respectivamente. Se for usar um certificado existente, use o `SME_THUMBPRINT` para especificar sua impressão digital.

> [!WARNING]
> Instalar o Windows Admin Center reiniciará o serviço WinRM, que será sever todas as sessões remotas do PowerShells. É recomendável que você instale em um local Cmd ou o PowerShell. Se você estiver instalando com uma solução de automação que poderia ser quebrada reiniciando o WinRM serviço, você pode adicionar o parâmetro ```RESTART_WINRM=0``` para a instalação argumentos, mas WinRM deve ser reiniciado para o Windows Admin Center funcione.

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

## Atualizar o Windows Admin Center

Você pode atualizar versões não-preview do Windows Admin Center usando o Microsoft Update ou instalação manual. 

As configurações são preservadas ao atualizar para uma nova versão do Windows Admin Center. Nós não oficialmente dá suporte a versões de visualização de Insider de atualização do Windows Admin Center - acreditamos que é melhor fazer uma instalação limpa - mas nós não bloqueá-lo.