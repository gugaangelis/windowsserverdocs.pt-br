---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, instalação
contributor: maertendMSFT
author: maertendMSFT
title: Instalação do OpenSSH para Windows
ms.openlocfilehash: 6a5d4d47fbb3f962c2a19582eb0a72810145a28c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866879"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Instalação do OpenSSH para Windows Server 2019 e Windows 10 #

O cliente OpenSSH e o servidor OpenSSH são componentes instaláveis separadamente no Windows Server 2019 e no Windows 10 1809.
Os usuários com essas versões do Windows devem usar as instruções a seguir para instalar e configurar o OpenSSH. 

> [!NOTE] 
> Os usuários que adquiriram o OpenSSH do repositório GitHub https://github.com/PowerShell/OpenSSH-Portable) do PowerShell (devem usar as instruções de lá e __não devem__ usar essas instruções. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Instalando o OpenSSH na interface do usuário de configurações no Windows Server 2019 ou no Windows 10 1809

O cliente e o servidor OpenSSH são recursos instaláveis do Windows 10 1809. 

Para instalar o OpenSSH, inicie as configurações e vá para aplicativos > aplicativos e recursos > gerenciar recursos opcionais. 

Examine esta lista para ver se o cliente OpenSSH já está instalado. Caso contrário, na parte superior da página, selecione "adicionar um recurso" e: 

* Para instalar o cliente OpenSSH, localize "cliente do OpenSSH" e clique em "instalar". 
* Para instalar o servidor OpenSSH, localize "servidor OpenSSH" e clique em "instalar". 

Quando a instalação for concluída, retorne a aplicativos > aplicativos e recursos > gerenciar recursos opcionais e você deverá ver os componentes do OpenSSH listados.

> [!NOTE]
> A instalação do servidor OpenSSH criará e habilitará uma regra de firewall denominada "OpenSSH-Server-in-TCP". Isso permite o tráfego de entrada SSH na porta 22. 

## <a name="installing-openssh-with-powershell"></a>Instalando o OpenSSH com o PowerShell 

Para instalar o OpenSSH usando o PowerShell, primeiro inicie o PowerShell como administrador.
Para certificar-se de que os recursos do OpenSSH estejam disponíveis para instalação:

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Em seguida, instale os recursos de servidor e/ou cliente:

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Both of these should return the following output:

Path          :
Online        : True
RestartNeeded : False
```

## <a name="uninstalling-openssh"></a>Desinstalando o OpenSSH

Para desinstalar o OpenSSH usando as configurações do Windows, inicie as configurações e vá para aplicativos > aplicativos e recursos > gerenciar recursos opcionais. Na lista de recursos instalados, selecione o cliente OpenSSH ou o componente de servidor OpenSSH e, em seguida, selecione Desinstalar.

Para desinstalar o OpenSSH usando o PowerShell, use um dos seguintes comandos:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Uma reinicialização do Windows pode ser necessária após a remoção do OpenSSH, se o serviço estiver em uso no momento em que ele foi desinstalado.


## <a name="initial-configuration-of-ssh-server"></a>Configuração inicial do servidor SSH

Para configurar o servidor OpenSSH para uso inicial no Windows, inicie o PowerShell como administrador e execute os seguintes comandos para iniciar o serviço SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Uso inicial do SSH

Depois de instalar o servidor OpenSSH no Windows, você poderá testá-lo rapidamente usando o PowerShell de qualquer dispositivo Windows com o cliente SSH instalado. No PowerShell, digite o seguinte comando: 

```powershell
Ssh username@servername
```

A primeira conexão com qualquer servidor resultará em uma mensagem semelhante à seguinte:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

A resposta deve ser "Sim" ou "não". Responder sim adicionará esse servidor à lista de hosts SSH conhecidos do sistema local.

Você será solicitado a fornecer a senha neste momento. Como uma precaução de segurança, sua senha não será exibida à medida que você digitar. 

Quando você se conectar, verá um prompt de Shell de comando semelhante ao seguinte:

```
domain\username@SERVERNAME C:\Users\username>
```

O shell padrão usado pelo servidor Windows OpenSSH é o Shell de comando do Windows. 

