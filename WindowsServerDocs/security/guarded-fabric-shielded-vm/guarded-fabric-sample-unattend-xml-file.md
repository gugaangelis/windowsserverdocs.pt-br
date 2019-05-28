---
title: Criar arquivo de resposta do sistema operacional especialização
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5717fcc9e1732b6273620e633c140c6df58ec8b7
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624649"
---
# <a name="create-os-specialization-answer-file"></a>Criar arquivo de resposta do sistema operacional especialização

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Na preparação para implantar VMs blindadas, você precisa criar um arquivo de resposta de especialização do sistema operacional. No Windows, isso é conhecido como o arquivo "Unattend. xml". O **New-ShieldingDataAnswerFile** função do Windows PowerShell ajuda você a fazer isso. Você pode usar o arquivo de resposta quando você estiver criando o VMs blindadas de um modelo usando o System Center Virtual Machine Manager (ou qualquer outro controlador de malha).

Para obter diretrizes gerais para arquivos autônomos para VMs blindadas, consulte [criar um arquivo de resposta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Baixando a função New-ShieldingDataAnswerFile

Você pode obter o **New-ShieldingDataAnswerFile** função da [Galeria do PowerShell](https://aka.ms/gftools). Se o computador tem conectividade com a Internet, você pode instalá-lo com o seguinte comando PowerShell:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

O `unattend.xml` saída pode ser empacotada nos dados de blindagem, juntamente com artefatos adicionais, para que possa ser usado para criar VMs blindadas de modelos.

As seções a seguir mostram como você pode usar os parâmetros de função para um `unattend.xml` arquivo que contém várias opções:

- [Arquivo de resposta básico do Windows](#basic-windows-answer-file)
- [Arquivo com o ingresso no domínio de resposta do Windows](#windows-answer-file-with-domain-join)
- [Arquivo de resposta do Windows com endereços IPv4 estáticos](#windows-answer-file-with-static-ipv4-addresses)
- [Arquivo de resposta do Windows com uma localidade personalizada](#windows-answer-file-with-a-custom-locale)
- [Arquivo de resposta básico do Linux](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>Arquivo de resposta básico do Windows

Os seguintes comandos criam um arquivo de resposta do Windows que simplesmente define a senha da conta de administrador e o nome do host.
Os adaptadores de rede VM usará o DHCP para obter endereços IP e a VM não será adicionada a um domínio do Active Directory.
Quando solicitado a inserir uma credencial de administrador, especifique o nome de usuário desejado e a senha.
Use "Administrador" para o nome de usuário, se você quiser configurar a conta interna administrador.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Arquivo com o ingresso no domínio de resposta do Windows

Os seguintes comandos criam um arquivo de resposta do Windows que une a VM blindada a um domínio do Active Directory.
Os adaptadores de rede VM usará o DHCP para obter endereços IP.

A primeira solicitação de credenciais solicitará as informações da conta de administrador local.
Use "Administrador" para o nome de usuário, se você quiser configurar a conta interna administrador.

A segunda solicitação de credenciais solicitará credenciais que tenham o direito de ingressar a máquina no domínio do Active Directory.

Certifique-se de alterar o valor da "-DomainName" parâmetro para o FQDN do domínio do Active Directory.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Arquivo de resposta do Windows com endereços IPv4 estáticos

Os seguintes comandos criam um arquivo de resposta do Windows que usa endereços IP estáticos fornecidos no momento da implantação pelo Gerenciador de malha, como o System Center Virtual Machine Manager.

Virtual Machine Manager oferece três componentes para o endereço IP estático por meio de um pool de IP: Endereço IPv4, IPv6 endereço, endereço do gateway e endereço DNS. Se você quiser que todos os campos adicionais a serem incluídos ou exigem uma configuração de rede personalizada, será preciso editar manualmente o arquivo de resposta gerado pelo script.

As capturas de tela a seguir mostram os pools de IP que você pode configurar no Virtual Machine Manager. Esses pools são necessários se você quiser usar IP estático.

Atualmente, a função dá suporte a apenas um servidor DNS. Aqui está o que suas configurações DNS pareceria com:

![Configurando o servidor DNS com o pool de IP estático](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Eis como seria a aparência seu resumo para a criação de pool de endereços IP estáticos. Em resumo, você deve ter apenas uma rede route, um gateway e um servidor DNS — e você deve especificar o endereço IP.

![Resumo da criação de pool IP estático](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

Você precisa configurar o adaptador de rede para sua máquina virtual. Captura de tela a seguir mostra onde definir que a configuração e como alterná-lo para o endereço IP estático.

![Configurar o hardware para usar IP estático](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

Em seguida, você pode usar o `-StaticIPPool` parâmetro para incluir os elementos IP estáticos no arquivo de resposta. Os parâmetros `@IPAddr-1@`, `@NextHop-1-1@`, e `@DNSAddr-1-1@` na resposta arquivo será substituído pelos valores reais que você especificou no Virtual Machine Manager no momento da implantação.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>Arquivo de resposta do Windows com uma localidade personalizada

Os comandos a seguir criam um arquivo de resposta do Windows com uma localidade personalizada.

Quando solicitado a inserir uma credencial de administrador, especifique o nome de usuário desejado e a senha.
Use "Administrador" para o nome de usuário, se você quiser configurar a conta interna administrador.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>Arquivo de resposta básico do Linux

Começando com o Windows Server versão 1709, você pode executar determinados sistemas operacionais de convidados do Linux em VMs blindadas.
Se você estiver usando o agente Linux do System Center Virtual Machine Manager para especializar essas VMs, o cmdlet New-ShieldingDataAnswerFile pode criar arquivos de resposta compatíveis para ele.

Em um arquivo de resposta do Linux, você normalmente incluirão a senha raiz, chave SSH raiz e informações de pool IP estático opcionalmente.
Substitua o caminho para a metade pública de sua chave SSH antes de executar o script a seguir.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>Consulte também

- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
