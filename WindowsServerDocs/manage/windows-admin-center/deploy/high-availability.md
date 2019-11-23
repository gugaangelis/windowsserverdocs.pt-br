---
title: Implantar o centro de administração do Windows com alta disponibilidade
description: Implantar o centro de administração do Windows com alta disponibilidade (projeto Honolulu)
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
# <a name="deploy-windows-admin-center-with-high-availability"></a>Implantar o centro de administração do Windows com alta disponibilidade

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Você pode implantar o centro de administração do Windows em um cluster de failover para fornecer alta disponibilidade para o serviço de gateway do centro de administração do Windows. A solução fornecida é uma solução ativa-passiva, em que apenas uma instância do centro de administração do Windows está ativa. Se um dos nós no cluster falhar, o centro de administração do Windows executará um failover normal para outro nó, permitindo que você continue gerenciando os servidores em seu ambiente sem problemas. 

[Saiba mais sobre outras opções de implantação do centro de administração do Windows.](../plan/installation-options.md)

## <a name="prerequisites"></a>Pré-requisitos

- Um cluster de failover de dois ou mais nós no Windows Server 2016 ou 2019. [Saiba mais sobre como implantar um cluster de failover](../../../failover-clustering/failover-clustering-overview.md).
- Um CSV (volume compartilhado clusterizado) do centro de administração do Windows para armazenar dados persistentes que podem ser acessados por todos os nós no cluster. 10 GB serão suficientes para seu CSV.
- Script de implantação de alta disponibilidade do [arquivo zip de script do Windows Admin Center ha](https://aka.ms/WACHAScript). Baixe o arquivo. zip que contém o script em seu computador local e, em seguida, copie o script conforme necessário com base nas diretrizes abaixo.
- Recomendado, mas opcional: um certificado assinado. pfx & senha. Você não precisa já ter instalado o certificado nos nós do cluster-o script fará isso para você. Se você não fornecer um, o script de instalação gerará um certificado autoassinado, que expira após 60 dias.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Instalar o centro de administração do Windows em um cluster de failover

1. Copie o script ```Install-WindowsAdminCenterHA.ps1``` para um nó no cluster. Baixe ou copie o centro de administração do Windows. msi para o mesmo nó.
2. Conecte-se ao nó via RDP e execute o script ```Install-WindowsAdminCenterHA.ps1``` a partir desse nó com os seguintes parâmetros:
    - `-clusterStorage`: o caminho local do Volume Compartilhado Clusterizado para armazenar dados do centro de administração do Windows.
    - `-clientAccessPoint`: escolha um nome que será usado para acessar o centro de administração do Windows. Por exemplo, se você executar o script com o parâmetro `-clientAccessPoint contosoWindowsAdminCenter`, acessará o serviço centro de administração do Windows visitando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: opcional. Um ou mais endereços estáticos para o serviço genérico do cluster. 
    - `-msiPath`: o caminho para o arquivo. msi do centro de administração do Windows.
    - `-certPath`: opcional. O caminho para um arquivo. pfx de certificado.
    - `-certPassword`: opcional. Uma senha de SecureString para o Certificate. pfx fornecido no `-certPath`
    - `-generateSslCert`: opcional. Se você não quiser fornecer um certificado assinado, inclua esse sinalizador de parâmetro para gerar um certificado autoassinado. Observe que o certificado autoassinado expirará em 60 dias.
    - `-portNumber`: opcional. Se você não especificar uma porta, o serviço de gateway será implantado na porta 443 (HTTPS). Para usar uma porta diferente, especifique nesse parâmetro. Observe que, se você usar uma porta personalizada (algo além de 443), acessará o centro de administração do Windows acessando https://\<clientAccessPoint\>:\<Port\>.

> [!NOTE]
> O script de ```Install-WindowsAdminCenterHA.ps1``` dá suporte a parâmetros de ```-WhatIf ``` e ```-Verbose```

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

Use o mesmo ```Install-WindowsAdminCenterHA.ps1``` script para atualizar sua implantação de alta disponibilidade, sem perder os dados de conexão.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Atualizar para uma nova versão do centro de administração do Windows

Quando uma nova versão do centro de administração do Windows for lançada, basta executar o script de ```Install-WindowsAdminCenterHA.ps1``` novamente com apenas o parâmetro ```msiPath```:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Atualizar o certificado usado pelo centro de administração do Windows

Você pode atualizar o certificado usado por uma implantação de alta disponibilidade do centro de administração do Windows a qualquer momento, fornecendo o arquivo. pfx do novo certificado e a senha.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Você também pode atualizar o certificado ao mesmo tempo em que atualiza a plataforma do centro de administração do Windows com um novo arquivo. msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Desinstalar

Para desinstalar a implantação de alta disponibilidade do centro de administração do Windows do seu cluster de failover, passe o parâmetro ```-Uninstall``` para o script ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Solução de problemas

Os logs são salvos na pasta Temp do CSV (por exemplo, C:\ClusterStorage\Volume1\temp).