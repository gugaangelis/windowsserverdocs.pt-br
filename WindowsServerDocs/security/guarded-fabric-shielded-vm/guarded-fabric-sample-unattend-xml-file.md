---
title: Criar arquivo de resposta do sistema operacional especialização
ms.prod: windows-server
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 526ded03c877613766b8a0b762f1db1a693d2019
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474993"
---
# <a name="create-os-specialization-answer-file"></a>Criar arquivo de resposta do sistema operacional especialização

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Na preparação para implantar VMs blindadas, talvez seja necessário criar um arquivo de resposta de especialização do sistema operacional. No Windows, isso é normalmente conhecido como arquivo "unattend.xml". A função **New-ShieldingDataAnswerFile** do Windows PowerShell ajuda você a fazer isso. Você pode usar o arquivo de resposta quando estiver criando VMs blindadas de um modelo usando System Center Virtual Machine Manager (ou qualquer outro controlador de malha).

Para obter diretrizes gerais para arquivos autônomos para VMs blindadas, consulte [criar um arquivo de resposta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).

## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Baixando a função New-ShieldingDataAnswerFile

Você pode obter a função **New-ShieldingDataAnswerFile** da [Galeria do PowerShell](https://aka.ms/gftools). Se o computador tiver conectividade com a Internet, você poderá instalá-lo do PowerShell com o seguinte comando:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

A `unattend.xml` saída pode ser empacotada nos dados de blindagem, juntamente com artefatos adicionais, para que possa ser usada para criar VMs blindadas a partir de modelos.

As seções a seguir mostram como você pode usar os parâmetros de função para um `unattend.xml` arquivo que contém várias opções:

- [Arquivo de resposta básico do Windows](#basic-windows-answer-file)
- [Arquivo de resposta do Windows com ingresso no domínio](#windows-answer-file-with-domain-join)
- [Arquivo de resposta do Windows com endereços IPv4 estáticos](#windows-answer-file-with-static-ipv4-addresses)
- [Arquivo de resposta do Windows com uma localidade personalizada](#windows-answer-file-with-a-custom-locale)
- [Arquivo de resposta básico do Linux](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>Arquivo de resposta básico do Windows

Os comandos a seguir criam um arquivo de resposta do Windows que simplesmente define a senha e o nome do host da conta de administrador.
Os adaptadores de rede VM usarão o DHCP para obter endereços IP e a VM não será unida a um domínio Active Directory.
Quando solicitado a inserir uma credencial de administrador, especifique o nome de usuário e a senha desejados.
Use "administrador" para o nome de usuário se desejar configurar a conta de administrador interno.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Arquivo de resposta do Windows com ingresso no domínio

Os comandos a seguir criam um arquivo de resposta do Windows que une a VM blindada a um domínio Active Directory.
Os adaptadores de rede VM usarão o DHCP para obter endereços IP.

A primeira solicitação de credencial solicitará as informações da conta de administrador local.
Use "administrador" para o nome de usuário se desejar configurar a conta de administrador interno.

O segundo prompt de credencial solicitará credenciais que tenham o direito de ingressar o computador no domínio de Active Directory.

Certifique-se de alterar o valor do parâmetro "-DomainName" para o FQDN do seu domínio de Active Directory.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Arquivo de resposta do Windows com endereços IPv4 estáticos

Os comandos a seguir criam um arquivo de resposta do Windows que usa endereços IP estáticos fornecidos no momento da implantação pelo Gerenciador de malha, como System Center Virtual Machine Manager.

O Virtual Machine Manager fornece três componentes para o endereço IP estático usando um pool de IPS: endereço IPv4, endereço IPv6, endereço de gateway e endereço DNS. Se você quiser que quaisquer campos adicionais sejam incluídos ou exijam uma configuração de rede personalizada, será necessário editar manualmente o arquivo de resposta produzido pelo script.

As capturas de tela a seguir mostram os pools de IPS que podem ser configurados no Virtual Machine Manager. Esses pools serão necessários se você quiser usar o IP estático.

Atualmente, a função dá suporte a apenas um servidor DNS. Veja a seguir como as configurações de DNS seriam:

![Configurando o servidor DNS com o pool de IPs estáticos](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Veja como seria o seu resumo para criar o pool de endereços IP estáticos. Resumindo, você deve ter apenas uma rota de rede, um gateway e um servidor DNS-e deve especificar seu endereço IP.

![Resumo da criação do pool de IPs estáticos](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

Você precisa configurar o adaptador de rede para sua máquina virtual. A captura de tela a seguir mostra onde definir essa configuração e como alterná-la para IP estático.

![Configurar o hardware para usar o IP estático](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

Em seguida, você pode usar o `-StaticIPPool` parâmetro para incluir os elementos IP estáticos no arquivo de resposta. Os parâmetros `@IPAddr-1@` , `@NextHop-1-1@` e `@DNSAddr-1-1@` no arquivo de resposta serão substituídos pelos valores reais que você especificou em Virtual Machine Manager no momento da implantação.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>Arquivo de resposta do Windows com uma localidade personalizada

Os comandos a seguir criam um arquivo de resposta do Windows com uma localidade personalizada.

Quando solicitado a inserir uma credencial de administrador, especifique o nome de usuário e a senha desejados.
Use "administrador" para o nome de usuário se desejar configurar a conta de administrador interno.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>Arquivo de resposta básico do Linux

A partir do Windows Server versão 1709, você pode executar determinados SOS convidados do Linux em VMs blindadas.
Se você estiver usando o agente do System Center Virtual Machine Manager Linux para especializar essas VMs, o cmdlet New-ShieldingDataAnswerFile poderá criar arquivos de resposta compatíveis para ele.

Em um arquivo de resposta do Linux, normalmente você incluirá a senha raiz, a chave SSH raiz e, opcionalmente, as informações do pool IP estático.
Substitua o caminho para a metade pública da chave SSH antes de executar o script a seguir.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="additional-references"></a>Referências adicionais

- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
