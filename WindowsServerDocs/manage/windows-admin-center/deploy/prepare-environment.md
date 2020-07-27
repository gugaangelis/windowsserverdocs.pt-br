---
title: Preparar seu ambiente para o Windows Admin Center
description: Preparar seu ambiente para o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: a37c7e8765ba6f83fc1ebe20aaba3dfb8bc29a3d
ms.sourcegitcommit: b35fbd2a67d7a3395b50b2a3acd0817ba4e36b26
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86891341"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>Preparar seu ambiente para o Windows Admin Center

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Há algumas versões de servidor que precisam de preparação adicional antes de estarem prontos para serem gerenciados com o Windows Admin Center:

- [Windows Server 2012 e 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

Também há alguns cenários em que a [configuração de porta no servidor de destino](#port-configuration-on-the-target-server) pode precisar ser modificada antes de ser gerenciada com o Windows Admin Center.

## <a name="prepare-windows-server-2012-and-2012-r2"></a>Preparar o Windows Server 2012 e 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>Instalar o WMF versão 5.1 ou posterior

O Windows Admin Center exige recursos do PowerShell que não estão incluídos por padrão no Windows Server 2012 e 2012 R2. Para gerenciar o Windows Server 2012 ou 2012 R2 com o Windows Admin Center, você deverá instalar o WMF versão 5.1 ou posterior nesses servidores.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você poderá [baixar e instalar o WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure).

## <a name="prepare-microsoft-hyper-v-server-2016"></a>Preparar o Microsoft Hyper-V Server 2016

Para gerenciar o Microsoft Hyper-V Server 2016 com o Windows Admin Center, há algumas funções de servidor que você deverá habilitar antes.

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>Para gerenciar o Microsoft Hyper-V Server 2016 com o Windows Admin Center:

1. Habilite o Gerenciamento Remoto.
2. Habilite a função de servidor de arquivos.
3. Habilite o módulo Hyper-V para PowerShell.

### <a name="step-1-enable-remote-management"></a>**Etapa 1:** Habilitar o Gerenciamento Remoto

Para habilitar o gerenciamento remoto no Hyper-V Server:

1. Faça logon no Hyper-V Server.
2. Na ferramenta **SCONFIG** (Configuração do Servidor), digite **4** para configurar o gerenciamento remoto.
3. Digite **1** para habilitar o Gerenciamento Remoto.
4. Digite **4** para retornar ao menu principal.

### <a name="step-2-enable-file-server-role"></a>**Etapa 2:** Habilitar a função de servidor de arquivos

Para habilitar a função de servidor de arquivos para o gerenciamento remoto e compartilhamento de arquivos básico:

1. Clique em **Funções e Recursos** no menu **Ferramentas**.
2. Em **Funções e Recursos**, encontre **Arquivo e Serviços de Armazenamento** e marque **Arquivo e Serviços iSCSI** e **Servidor de Arquivos**:

![Captura de tela de Funções e Recursos mostrando a função Arquivo e Serviços iSCSI selecionada](../media/prepare-environment/prepare-your-environment-image-1.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**Etapa 3:** Habilitar o módulo Hyper-V para PowerShell

Para habilitar os recursos do módulo Hyper-V para PowerShell:

1. Clique em **Funções e Recursos** no menu **Ferramentas**.
2. Em **Funções e Recursos**, encontre as **Ferramentas de Administração de Servidor Remoto** e marque **Ferramentas de Administração de Função** e **Módulo Hyper-V para PowerShell**:

![Captura de tela de Funções e Recursos mostrando as funções do Hyper-V selecionadas](../media/prepare-environment/prepare-your-environment-image-2.png)

O Microsoft Hyper-V Server 2016 agora está pronto para o gerenciamento com o Windows Admin Center.

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>Preparar o Microsoft Hyper-V Server 2012 R2

Para gerenciar o Microsoft Hyper-V Server 2012 R2 com o Windows Admin Center, há algumas funções de servidor que você deverá habilitar antes.  Além disso, você deverá instalar o WMF versão 5.1 ou posterior.

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>Para gerenciar o Microsoft Hyper-V Server 2012 R2 com o Windows Admin Center:

1. Instalar o WMF (Windows Management Framework) versão 5.1 ou posterior
2. Habilitar o Gerenciamento Remoto
3. Habilitar a função de servidor de arquivos
4. Habilitar o módulo Hyper-V para PowerShell

### <a name="step-1-install-windows-management-framework-51"></a>Etapa 1: Instalar o Windows Management Framework 5.1

O Windows Admin Center PowerShell requer recursos que não estão incluídos por padrão no Microsoft Hyper-V Server 2012 R2. Para gerenciar o Microsoft Hyper-V Server 2012 R2 com o Windows Admin Center, você deverá instalar o WMF versão 5.1 ou posterior.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você poderá [baixar o WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure).

### <a name="step-2-enable-remote-management"></a>Etapa 2: Habilitar o Gerenciamento Remoto

Para habilitar o gerenciamento remoto do Hyper-V Server:

1. Faça logon no Hyper-V Server.
2. Na ferramenta **SCONFIG** (Configuração do Servidor), digite **4** para configurar o gerenciamento remoto.
3. Digite **1** para habilitar o gerenciamento remoto.
4. Digite **4** para retornar ao menu principal.

### <a name="step-3-enable-file-server-role"></a>Etapa 3: Habilitar a função de servidor de arquivos

Para habilitar a função de servidor de arquivos para o gerenciamento remoto e compartilhamento de arquivos básico:

1. Clique em **Funções e Recursos** no menu **Ferramentas**.
2. Em **Funções e Recursos**, encontre **Arquivo e Serviços de Armazenamento** e marque **Arquivo e Serviços iSCSI** e **Servidor de Arquivos**:

![Captura de tela de Funções e Recursos mostrando a função Arquivo e Serviços iSCSI selecionada](../media/prepare-environment/prepare-your-environment-image-1.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>Etapa 4: Habilitar o módulo Hyper-V para PowerShell

Para habilitar os recursos do módulo Hyper-V para PowerShell:

1. Clique em **Funções e Recursos** no menu **Ferramentas**.
2. Em **Funções e Recursos**, encontre as **Ferramentas de Administração de Servidor Remoto** e marque **Ferramentas de Administração de Função** e **Módulo Hyper-V para PowerShell**:

![Captura de tela de Funções e Recursos mostrando as Ferramentas de Administração de Servidor Remoto do Hyper-V selecionadas](../media/prepare-environment/prepare-your-environment-image-2.png)

O Microsoft Hyper-V Server 2012 R2 agora está pronto para o gerenciamento com o Windows Admin Center.

## <a name="port-configuration-on-the-target-server"></a>Configuração de porta no servidor de destino

O Windows Admin Center usa o protocolo de compartilhamento de arquivos SMB para algumas tarefas de cópia de arquivo, como ao importar um certificado em um servidor remoto. Para que essas operações de cópia de arquivo tenham êxito, o firewall no servidor remoto deverá permitir conexões de entrada na porta 445.  Você pode usar a ferramenta Firewall no Windows Admin Center para verificar se a regra de entrada para “Gerenciamento Remoto do Servidor de Arquivos (SMB-In)” está definida para permitir o acesso nessa porta.

> [!Tip]
> Pronto para instalar o Windows Admin Center? [Baixar agora](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)
