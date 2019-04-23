---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, configurar
contributor: maertendMSFT
author: maertendMSFT
title: Instalação do OpenSSH para Windows
ms.openlocfilehash: f617b01ee7dabd4897f99e374420f673e209e145
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859557"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Instalação do OpenSSH para Windows Server 2019 e Windows 10 #

O cliente OpenSSH e OpenSSH Server são componentes podem ser instaladas separadamente 2019 do Windows Server e Windows 10 1809.
Os usuários com essas versões do Windows devem usar as instruções a seguir para instalar e configurar o OpenSSH. 

> [!NOTE] 
> Os usuários que adquiriu OpenSSH do repositório Github do PowerShell (https://github.com/PowerShell/OpenSSH-Portable) deve usar as instruções a partir daí, e __não deve__ use estas instruções. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Instalando o OpenSSH das configurações de interface do usuário no Windows Server 2019 ou Windows 10 1809

Servidor e cliente OpenSSH são instaláveis recursos do Windows 10 1809. 

Para instalar o OpenSSH, iniciar as configurações e em seguida, vá para aplicativos > aplicativos e recursos > Gerenciar recursos opcionais. 

Examine esta lista para ver se o cliente OpenSSH já está instalado. Caso contrário, em seguida, na parte superior da página Selecione "Adicionar um recurso", em seguida: 

* Para instalar o cliente OpenSSH, localize o "Cliente OpenSSH" e clique em "Instalar". 
* Para instalar o servidor do OpenSSH, localize "OpenSSH Server", e em seguida, clique em "Instalar". 

Depois de concluir a instalação, retorne ao aplicativos > aplicativos e recursos > Gerenciar recursos opcionais e você deverá ver os componentes do OpenSSH listados.

> [!NOTE]
> Instalando o OpenSSH Server criar e habilitar uma regra de firewall denominada "OpenSSH-Server-em-TCP". Isso permite que o tráfego SSH de entrada na porta 22. 

## <a name="installing-openssh-with-powershell"></a>Instalando o OpenSSH com o PowerShell 

Para instalar o OpenSSH usando o PowerShell, primeiro inicie o PowerShell como administrador.
Para certificar-se de que os recursos do OpenSSH estão disponíveis para instalação:

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

Para desinstalar o OpenSSH usando as configurações do Windows, inicie as configurações e vá para aplicativos > aplicativos e recursos > Gerenciar recursos opcionais. Na lista de recursos instalados, selecione o componente cliente OpenSSH ou OpenSSH Server e, em seguida, selecione Desinstalar.

Para desinstalar o OpenSSH usando o PowerShell, use um dos seguintes comandos:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Uma reinicialização do Windows pode ser necessária depois de remover o OpenSSH, se o serviço está em uso no momento ele foi desinstalado.


## <a name="initial-configuration-of-ssh-server"></a>Configuração inicial do servidor SSH

Para configurar o servidor do OpenSSH para uso inicial no Windows, inicie o PowerShell como administrador e, em seguida, execute os seguintes comandos para iniciar o serviço SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Uso inicial do SSH

Depois que você instalou OpenSSH Server no Windows, você pode rapidamente testar usando o PowerShell de qualquer dispositivo Windows com o cliente SSH instalado. No PowerShell, digite o seguinte comando: 

```powershell
Ssh username@servername
```

A primeira conexão a qualquer servidor resultará em uma mensagem semelhante à seguinte:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

A resposta deve ser "Sim" ou "não". Respondendo Sim irá adicionar esse servidor para o sistema local da lista de conhecida ssh hosts.

Você será solicitado a senha neste momento. Como uma precaução de segurança, sua senha não será exibida conforme você digita. 

Depois que você se conectar, você verá um prompt de shell de comando semelhante ao seguinte:

```
domain\username@SERVERNAME C:\Users\username>
```

O shell padrão usado pelo Windows OpenSSH server é o shell de comando do Windows. 

