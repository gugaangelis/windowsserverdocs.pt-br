---
title: Preparar seu ambiente para o Windows Admin Center
description: Preparar seu ambiente para o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 598eeae64925d24ec6d97b59da9cae1e2d10585d
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339584"
---
# Preparar seu ambiente para o Windows Admin Center

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Há algumas versões de servidor que precisam de preparação adicional antes de serem prontos para gerenciar com o Windows Admin Center:

- [Windows Server 2012 e 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server 2008 R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

## Preparar o Windows Server 2012 e 2012 R2

### Instalar o WMF versão 5.1 ou posterior

O Windows Admin Center exige recursos do PowerShell que não estão incluídos por padrão no Windows Server 2012 e 2012 R2. Para gerenciar o Windows Server 2012 ou 2012 R2 com o Windows Admin Center, você precisará instalar o WMF versão 5.1 ou posterior nesses servidores.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você pode [baixar e instalar 5.1 WMF](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

## Preparar o Windows Server 2008 R2

### Instalar WMF versão 5.1 ou posterior

O Windows Admin Center exige recursos do PowerShell que não estão incluídos por padrão no Windows Server 2008 R2. Para gerenciar o Windows Server 2008 R2 com o Windows Admin Center, você precisará instalar o WMF versão 5.1 ou posterior nesses servidores. 

Certifique-se de que [do .NET Framework 4.5.2 ou posterior](https://docs.microsoft.com/dotnet/framework/install/on-windows-7) já está instalado no computador.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você pode [baixar e instalar o WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

Execute `Enable-PSRemoting –force` em um console do PowerShell para habilitar a conexão remota do Powershell. 

### Habilitar a área de trabalho remota

Para usar a área de trabalho remota no Windows Admin Center, você deverá habilitar a Área de trabalho remota no seu servidor do Windows Server 2008 R2.

No **Gerenciador do Servidor**, vá para **Configurar área de trabalho remota**. Habilite a área de trabalho remota para "Permitir conexões de computadores executando qualquer versão da área de trabalho remota".

## Preparar o Microsoft Hyper-V Server 2016

Para gerenciar o Microsoft Hyper-V Server 2016 com o Windows Admin Center, há algumas funções de servidor, que você precisará habilitar antes que você pode fazê-lo.

### Para gerenciar o Microsoft Hyper-V Server 2016 com o Windows Admin Center:

1. Habilite o gerenciamento remoto.
2. Habilite a função de servidor de arquivo.
3. Habilite o módulo do Hyper-V para o PowerShell.

### **Etapa 1:** habilitar o gerenciamento remoto

Para habilitar o gerenciamento remoto no Hyper-V Server.

1. Faça logon no servidor Hyper-V.
2. Na ferramenta de **Configuração do Servidor** (SCONFIG), digite **4** para configurar o gerenciamento remoto.
3. Digite **1** para habilitar o gerenciamento remoto.
4. Digite **4** para retornar ao menu principal.

### **Etapa 2:** habilitar a função de servidor de arquivos

Para habilitar a função de servidor de arquivos para o gerenciamento remoto e compartilhamento de arquivos básico:

1. Clique em **Funções e recursos** no menu **Ferramentas** .
2. Em **Funções e recursos**, encontre **Arquivo e Serviços de armazenamento** e marque **Arquivo e serviços de iSCSI** e **Servidor de arquivos**:

![](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### **Etapa 3:** habilitar o módulo do Hyper-V para o PowerShell

Para habilitar o módulo do Hyper-V para recursos do PowerShell:

1. Clique em **Funções e recursos** no menu **Ferramentas** .
2. Em **Funções e recursos**, encontre as **Ferramentas de administração de servidor remoto** e marque **Ferramentas de administração de função** e **Módulo do Hyper-V para PowerShell**:

![](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

O Microsoft Hyper-V Server 2016 agora está pronto para o gerenciamento com o Windows Admin Center.

## Preparar o Microsoft Hyper-V Server 2012 R2

Para gerenciar o Microsoft Hyper-V Server 2012 R2 com o Windows Admin Center, há algumas funções de servidor que você deverá habilitar antes.  Além disso, você deverá instalar o WMF versão 5.1 ou posterior.

### Para gerenciar o Microsoft Hyper-V Server 2012 R2 com o Windows Admin Center:

1. Instalar o Windows Management Framework (WMF) versão 5.1 ou posterior
2. Habilitar o gerenciamento remoto
3. Habilitar a função de servidor de arquivo
4. Habilitar o módulo do Hyper-V para o PowerShell

### **Etapa 1:** instalar o Windows Management Framework 5.1

Requer o Windows Admin Center PowerShell recursos que não estão incluídos por padrão no Microsoft Hyper-V Server 2012 R2. Para gerenciar o Microsoft Hyper-V Server 2012 R2 com o Windows Admin Center, você precisará instalar WMF versão 5.1 ou posterior.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior. 

Se não estiver instalado, você pode [baixar o WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### **Etapa 2:** habilitar o gerenciamento remoto 

Para habilitar o gerenciamento remoto do Hyper-V Server.

1. Entre no Hyper-V Server.
2. Em que a ferramenta de **Configuração do servidor** (SCONFIG), digite **4** para configurar o gerenciamento remoto.
3. Digite **1** para habilitar o gerenciamento remoto.
4. Digite **4** para retornar ao menu principal.

### Etapa 3: Habilitar a função de servidor de arquivo

Para habilitar a função de servidor de arquivo para o gerenciamento de compartilhamento e remoto arquivo básicas:

1. Clique em **Funções e recursos** no menu **Ferramentas** .
2. Em **Funções e recursos**, encontre **Arquivo e serviços de armazenamento** e marque **Arquivo e serviços de iSCSI** e **Servidor de arquivos**:

![](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### Etapa 4: habilitar o módulo do Hyper-V do PowerShell ##

Para habilitar o módulo do Hyper-V para recursos do PowerShell:

1. Clique em **Funções e recursos** no menu **Ferramentas** .
2. Em **Funções e recursos**, encontre as **Ferramentas de administração de servidor remoto** e marque **Ferramentas de administração de função** e **Módulo do Hyper-V para PowerShell**:

![](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

O Microsoft Hyper-V Server 2012 R2 agora está pronto para o gerenciamento com o Windows Admin Center.

> [!Tip]
> Pronto para instalar o Windows Admin Center? [Baixe agora](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)