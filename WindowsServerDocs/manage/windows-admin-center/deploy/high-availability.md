---
title: Implantar o Windows Admin Center com alta disponibilidade
description: Implantar o Windows Admin Center com alta disponibilidade (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 802a7bd537cab22893abb7e6657c4be90346ef88
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2019
ms.locfileid: "9025028"
---
# Implantar o Windows Admin Center com alta disponibilidade

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Você pode implantar o Windows Admin Center em um cluster de failover para fornecer alta disponibilidade para seu serviço de gateway do Windows Admin Center. A solução fornecida é uma solução de ativo-passivo, onde apenas uma instância do Windows Admin Center está ativa. Se um de nós do cluster falhar, o Windows Admin Center normalmente failover para outro nó, permitindo que você continue a gerenciar os servidores no seu ambiente perfeitamente. 

[Saiba mais sobre outras opções de implantação do Windows Admin Center.](../plan/installation-options.md)

## Pré-requisitos

- Um cluster de failover de 2 ou mais nós no Windows Server 2016 ou 2019. [Saiba mais sobre como implantar um Cluster de Failover](../../../failover-clustering/failover-clustering-overview.md).
- Um volume compartilhado clusterizado (CSV para o Windows Admin Center armazenar dados persistentes que podem ser acessados por todos os nós no cluster). 10 GB será suficiente para seu CSV.
- Script de implantação de alta disponibilidade Windows Admin Center alta disponibilidade zip do [arquivo](https://aka.ms/WACHAScript)de Script. Baixar o arquivo. zip que contém o script no computador local e, em seguida, copie o script conforme necessário com base nas diretrizes abaixo.
- Recomendado, mas opcional: uma senha do certificado. pfx &. Você não precisa já ter instalado o certificado em nós de cluster - o script fará isso para você. Se você não fornecer um, o script de instalação gera um certificado autoassinado, expira após 60 dias.

## Instalar o Windows Admin Center em um cluster de failover

1. Copie o ```Install-WindowsAdminCenterHA.ps1``` script para um nó no cluster. Baixar ou copiar o arquivo. msi do Windows Admin Center para o mesmo nó.
2. Conectar-se ao nó via RDP e execute o ```Install-WindowsAdminCenterHA.ps1``` script de nó com os seguintes parâmetros:
    - `-clusterStorage`: o caminho local do Volume compartilhado do Cluster para armazenar dados do Windows Admin Center.
    - `-clientAccessPoint`: escolha um nome que você usará para acessar o Windows Admin Center. Por exemplo, se você executar o script com o parâmetro `-clientAccessPoint contosoWindowsAdminCenter`, você irá acessar o serviço do Windows Admin Center visitando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Opcional. Um ou mais endereços estáticos para o serviço de cluster genérico. 
    - `-msiPath`: O caminho para o arquivo. msi do Windows Admin Center.
    - `-certPath`: Opcional. O caminho para um arquivo. pfx de certificado.
    - `-certPassword`: Opcional. Uma senha SecureString para pfx o certificado fornecido no `-certPath`
    - `-generateSslCert`: Opcional. Se você não quiser fornecer um certificado, inclua esse sinalizador de parâmetro para gerar um certificado autoassinado. Observe que o certificado autoassinado expirará em 60 dias.
    - `-portNumber`: Opcional. Se você não especificar uma porta, o serviço de gateway é implantado na porta 443 (HTTPS). Para usar uma porta diferente especificar neste parâmetro. Observe que, se você usar uma porta personalizada (algo que não seja 443), você vai acessar o Windows Admin Center acessando https://\<clientAccessPoint\>:\<port\>.

> [!NOTE]
> O ```Install-WindowsAdminCenterHA.ps1``` script suporta ```-WhatIf ``` e ```-Verbose``` parâmetros

### Exemplos

#### Instale com um certificado:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### Instale com um certificado autoassinado:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## Atualizar uma instalação existente de alta disponibilidade

Usar a mesma ```Install-WindowsAdminCenterHA.ps1``` script para atualizar sua implantação de alta disponibilidade, sem perder seus dados de conexão.

### Atualizar para uma nova versão do Windows Admin Center

Quando uma nova versão do Windows Admin Center é liberada, basta executar o ```Install-WindowsAdminCenterHA.ps1``` script novamente com apenas o ```msiPath``` parâmetro:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### Atualizar o certificado usado pelo Windows Admin Center

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

## Uninstall

Para desinstalar a implantação de alta disponibilidade do Windows Admin Center de seu cluster de failover, passe o ```-Uninstall``` parâmetro para o ```Install-WindowsAdminCenterHA.ps1``` script.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## Solução de problemas

Logs são salvos na pasta temporária do CSV (por exemplo, C:\ClusterStorage\Volume1\temp).