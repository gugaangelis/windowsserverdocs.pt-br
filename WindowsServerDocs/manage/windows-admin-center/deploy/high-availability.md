---
title: Implantar o Windows Admin Center com alta disponibilidade
description: Implantar o Windows Admin Center com alta disponibilidade (projeto Paulo)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861057"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Implantar o Windows Admin Center com alta disponibilidade

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Você pode implantar o Windows Admin Center em um cluster de failover para fornecer alta disponibilidade para seu serviço de gateway do Windows Admin Center. A solução fornecida é uma solução de ativo-passivo, onde apenas uma instância do Windows Admin Center está ativa. Se um de nós do cluster falhar, Windows Admin Center normalmente fará failover para outro nó, permitindo que você continuar a gerenciar os servidores em seu ambiente perfeitamente. 

[Saiba mais sobre outras opções de implantação do Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Pré-requisitos

- Um cluster de failover de 2 ou mais nós no Windows Server 2016 ou de 2019. [Saiba mais sobre como implantar um Cluster de Failover](../../../failover-clustering/failover-clustering-overview.md).
- Um volume compartilhado clusterizado (CSV) para Windows Admin Center armazenar dados persistentes que podem ser acessados por todos os nós no cluster. 10 GB será suficiente para o CSV.
- Script de implantação de alta disponibilidade do [arquivo de zip do Windows Admin Center HA Script](https://aka.ms/WACHAScript). Baixe o arquivo. zip que contém o script em seu computador local e, em seguida, copie o script conforme necessário com base nas orientações abaixo.
- Opcional mas recomendado: um. pfx do certificado autoassinado e uma senha. Você não precisa já ter instalado o certificado em nós de cluster, o script fará isso para você. Se você não fornecer um, o script de instalação gera um certificado autoassinado, que expira após 60 dias.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Instalar Windows Admin Center em um cluster de failover

1. Copie o ```Install-WindowsAdminCenterHA.ps1``` script para um nó no cluster. Baixar ou copiar o arquivo. msi do Windows Admin Center ao mesmo nó.
2. Conectar-se ao nó via RDP e execute o ```Install-WindowsAdminCenterHA.ps1``` script a partir desse nó com os seguintes parâmetros:
    - `-clusterStorage`: o caminho local do Volume compartilhado do Cluster para armazenar dados de Windows Admin Center.
    - `-clientAccessPoint`: escolha um nome que você usará para acessar a Windows Admin Center. Por exemplo, se você executar o script com o parâmetro `-clientAccessPoint contosoWindowsAdminCenter`, você acessará o serviço Windows Admin Center visitando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Opcional. Um ou mais endereços estáticos para o serviço genérico de cluster. 
    - `-msiPath`: O caminho para o arquivo. msi do Windows Admin Center.
    - `-certPath`: Opcional. O caminho para um arquivo. pfx de certificado.
    - `-certPassword`: Opcional. Uma senha de SecureString para o certificado. pfx fornecida no `-certPath`
    - `-generateSslCert`: Opcional. Se você não quiser fornecer um certificado assinado, inclua esse sinalizador de parâmetro para gerar um certificado autoassinado. Observe que o certificado autoassinado expirará em 60 dias.
    - `-portNumber`: Opcional. Se você não especificar uma porta, o serviço de gateway é implantado na porta 443 (HTTPS). Para usar uma porta diferente especificar nesse parâmetro. Observe que se você usar uma porta personalizada (qualquer coisa além da 443), você acessará o Windows Admin Center, vá para https://\<clientAccessPoint\>:\<porta\>.

> [!NOTE]
> O ```Install-WindowsAdminCenterHA.ps1``` dá suporte de script ```-WhatIf ``` e ```-Verbose``` parâmetros

### <a name="examples"></a>Exemplos

#### <a name="install-with-a-signed-certificate"></a>Instale com um certificado assinado:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Instale com um certificado autoassinado:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Atualizar uma instalação existente de alta disponibilidade

Use o mesmo ```Install-WindowsAdminCenterHA.ps1``` script para atualizar sua implantação de alta disponibilidade, sem perda de dados de conexão.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Atualizar para uma nova versão do Windows Admin Center

Quando uma nova versão do Windows Admin Center é liberada, basta executar o ```Install-WindowsAdminCenterHA.ps1``` script novamente com apenas o ```msiPath``` parâmetro:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Atualizar o certificado usado pelo Windows Admin Center

Você pode atualizar o certificado usado por uma implantação de alta disponibilidade do Windows Admin Center a qualquer momento, fornecendo o arquivo. pfx do novo certificado e e a senha.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Você também pode atualizar o certificado ao mesmo tempo em que você atualizar a plataforma Windows Admin Center com um novo arquivo. msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Desinstalar

Para desinstalar a implantação de alta disponibilidade do Windows Admin Center do seu cluster de failover, passe o ```-Uninstall``` parâmetro para o ```Install-WindowsAdminCenterHA.ps1``` script.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Solução de problemas

Os logs são salvos na pasta temporária do CSV (por exemplo, C:\ClusterStorage\Volume1\temp).