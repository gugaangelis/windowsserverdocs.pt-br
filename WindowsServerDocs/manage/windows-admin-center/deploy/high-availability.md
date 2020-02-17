---
title: Implantar o Windows Admin Center com Alta Disponibilidade
description: Implantar o Windows Admin Center com Alta Disponibilidade (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406940"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Implantar o Windows Admin Center com alta disponibilidade

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Você pode implantar o Windows Admin Center em um cluster de failover para fornecer alta disponibilidade para o serviço de gateway do Windows Admin Center. A solução fornecida é uma solução ativa-passiva, em que apenas uma instância do Windows Admin Center está ativa. Se um dos nós no cluster falhar, o Windows Admin Center fará failover de maneira suave para outro nó, permitindo que você continue gerenciando os servidores em seu ambiente de maneira contínua. 

[Saiba mais sobre outras opções de implantação do Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Pré-requisitos

- Um cluster de failover de dois ou mais nós no Windows Server 2016 ou 2019. [Saiba mais sobre como implantar um Cluster de Failover](../../../failover-clustering/failover-clustering-overview.md).
- Um CSV (Volume Compartilhado Clusterizado) para o Windows Admin Center armazenar dados persistentes que podem ser acessados por todos os nós no cluster. 10 GB serão suficientes para seu CSV.
- O script de implantação de alta disponibilidade do [arquivo zip do Script de HA do Windows Admin Center](https://aka.ms/WACHAScript). Baixe o arquivo .zip que contém o script para seu computador local e copie-o conforme necessário com base nas diretrizes abaixo.
- Recomendado, mas opcional: um certificado assinado. pfx e a senha. Você não precisa já ter instalado o certificado nos nós de cluster – o script fará isso para você. Se você não fornecer um, o script de instalação gerará um certificado autoassinado, que expirará após 60 dias.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Instalar o Windows Admin Center em um cluster de failover

1. Copie o script ```Install-WindowsAdminCenterHA.ps1``` para um nó no cluster. Baixe ou copie o .msi do Windows Admin Center para o mesmo nó.
2. Conecte-se ao nó por meio do protocolo RDP e execute o script ```Install-WindowsAdminCenterHA.ps1``` com base nesse nó com os seguintes parâmetros:
    - `-clusterStorage`: o caminho local do Volume Compartilhado Clusterizado para armazenar dados do Windows Admin Center.
    - `-clientAccessPoint`: escolha um nome que será usado para acessar o Windows Admin Center. Por exemplo, se executar o script com o parâmetro `-clientAccessPoint contosoWindowsAdminCenter`, você acessará o serviço do Windows Admin Center acessando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Opcional. Um ou mais endereços estáticos para o serviço genérico do cluster. 
    - `-msiPath`: o caminho do arquivo .msi do Windows Admin Center.
    - `-certPath`: Opcional. O caminho para um arquivo .pfx de certificado.
    - `-certPassword`: Opcional. Uma senha SecureString do .pfx do certificado fornecida em `-certPath`
    - `-generateSslCert`: Opcional. Se você não quiser fornecer um certificado assinado, inclua esse sinalizador de parâmetro para gerar um certificado autoassinado. Observe que o certificado autoassinado expirará em 60 dias.
    - `-portNumber`: Opcional. Se você não especificar uma porta, o serviço de gateway será implantado na porta 443 (HTTPS). Para usar uma porta diferente, especifique nesse parâmetro. Observe que, se usar uma porta personalizada (qualquer uma além da 443), você acessará o Windows Admin Center acessando https://\<clientAccessPoint\>:\<port\>.

> [!NOTE]
> O script ```Install-WindowsAdminCenterHA.ps1``` dá suporte a parâmetros ```-WhatIf ``` e ```-Verbose```

### <a name="examples"></a>Exemplos

#### <a name="install-with-a-signed-certificate"></a>Instalar com um certificado assinado:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Instalar com um certificado autoassinado:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Atualizar uma instalação de alta disponibilidade existente

Use o mesmo script ```Install-WindowsAdminCenterHA.ps1``` para atualizar sua implantação de HA, sem perder os dados de conexão.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Atualizar para uma nova versão do Windows Admin Center

Quando uma nova versão do Windows Admin Center for lançada, execute o script ```Install-WindowsAdminCenterHA.ps1``` novamente apenas com o parâmetro ```msiPath```:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Atualizar o certificado usado pelo Windows Admin Center

Você pode atualizar o certificado usado por uma implantação de HA do Windows Admin Center a qualquer momento fornecendo o arquivo .pfx e a senha do novo certificado.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Você também pode atualizar o certificado enquanto atualiza a plataforma do Windows Admin Center com um novo arquivo .msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Desinstalar

Para desinstalar a implantação de HA do Windows Admin Center do seu cluster de failover, passe o parâmetro ```-Uninstall``` para o script ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Solução de problemas

Os logs são salvos na pasta temp do CSV (por exemplo, C:\ClusterStorage\Volume1\temp).