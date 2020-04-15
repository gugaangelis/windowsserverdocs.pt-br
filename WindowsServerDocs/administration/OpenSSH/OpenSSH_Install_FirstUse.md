---
ms.date: 09/27/2019
ms.topic: conceptual
contributor: maertendMSFT
author: maertendmsft
title: Instalação do OpenSSH para Windows
ms.openlocfilehash: b9889a9057a1ddd5181f4ea4aab35680d524eabf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852049"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Instalação do OpenSSH para Windows Server 2019 e Windows 10 #

O Cliente OpenSSH e o Servidor OpenSSH são componentes instaláveis separadamente no Windows Server 2019 e no Windows 10 1809.
Os usuários com essas versões do Windows devem usar as instruções a seguir para instalar e configurar o OpenSSH. 

> [!NOTE] 
> Os usuários que adquiriram o OpenSSH no repositório GitHub do PowerShell (https://github.com/PowerShell/OpenSSH-Portable) devem usar as instruções contidas nele e __não devem__ usar estas instruções). 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Instalar o OpenSSH da interface do usuário de Configurações no Windows Server 2019 ou Windows 10 1809

O cliente e o servidor OpenSSH são recursos instaláveis do Windows 10 1809. 

Para instalar o OpenSSH, inicie as Configurações e vá para Aplicativos > Aplicativos e Recursos > Gerenciar Recursos Opcionais. 

Examine esta lista para ver se o cliente OpenSSH já está instalado. Caso contrário, na parte superior da página, selecione "Adicionar um recurso" e, em seguida: 

* Para instalar o cliente OpenSSH, localize "Cliente OpenSSH" e clique em "Instalar". 
* Para instalar o servidor OpenSSH, localize "Servidor OpenSSH" e clique em "Instalar". 

Quando a instalação for concluída, volte para Aplicativos > Aplicativos e Recursos > Gerenciar Recursos Opcionais e você deverá ver os componentes do OpenSSH listados.

> [!NOTE]
> A instalação do Servidor OpenSSH criará e habilitará uma regra de firewall denominada "OpenSSH-Server-In-TCP". Isso permite o tráfego SSH de entrada na porta 22. 

## <a name="installing-openssh-with-powershell"></a>Instalar o OpenSSH com o PowerShell 

Para instalar o OpenSSH usando o PowerShell, primeiro inicie o PowerShell como Administrador.
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

## <a name="uninstalling-openssh"></a>Desinstalar o OpenSSH

Para desinstalar o OpenSSH usando as Configurações do Windows, inicie as Configurações e vá para Aplicativos > Aplicativos e Recursos > Gerenciar Recursos Opcionais. Na lista de recursos instalados, selecione o componente do Cliente OpenSSH ou Servidor OpenSSH e, em seguida, selecione Desinstalar.

Para desinstalar o OpenSSH usando o PowerShell, use um dos seguintes comandos:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Uma reinicialização do Windows poderá ser necessária após a remoção do OpenSSH se o serviço estiver em uso no momento em que ele foi desinstalado.


## <a name="initial-configuration-of-ssh-server"></a>Configuração inicial do Servidor SSH

Para configurar o servidor OpenSSH para uso inicial no Windows, inicie o PowerShell como administrador e execute os seguintes comandos para iniciar o serviço SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled
# If the firewall does not exist, create one
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

## <a name="initial-use-of-ssh"></a>Uso inicial do SSH

Depois de instalar o Servidor OpenSSH no Windows, você poderá testá-lo rapidamente usando o PowerShell de qualquer dispositivo Windows com o Cliente SSH instalado. No PowerShell, digite o seguinte comando: 

```powershell
Ssh username@servername
```

A primeira conexão com qualquer servidor resultará em uma mensagem semelhante à seguinte:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

A resposta deve ser "sim" ou "não". Responder Sim adicionará esse servidor à lista de hosts SSH conhecidos do sistema local.

Será solicitado que você forneça a senha neste momento. Como precaução de segurança, sua senha não será exibida conforme você digitar. 

Quando se conectar, você verá um prompt do shell de comando semelhante ao seguinte:

```
domain\username@SERVERNAME C:\Users\username>
```

O shell padrão usado pelo servidor Windows OpenSSH é o shell de comando do Windows. 

