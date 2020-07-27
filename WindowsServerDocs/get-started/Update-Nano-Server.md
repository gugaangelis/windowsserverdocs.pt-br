---
title: Como atualizar o Nano Server
description: ''
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 4b8a674d61379c0a645cc379ab9f9eafa3cc19b1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960688"
---
# <a name="updating-nano-server"></a>Como atualizar o Nano Server

> [!IMPORTANT]
> Começando com o Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem de sistema operacional base do contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa. 

O Nano Server oferece uma variedade de métodos para se manter atualizado. Em comparação com outras opções de instalação do Windows Server, o Nano Server segue um modelo de manutenção mais ativo semelhante ao do Windows 10. Essas versões periódicas são conhecidas como **CBB (Branch Atual para Negócios)** . Essa abordagem dá suporte aos clientes que desejam fazer inovações mais rapidamente e evoluir a uma cadência de nuvem de ciclos de vida de desenvolvimento rápido. Mais informações sobre o CBB estão disponíveis no [Blog do Windows Server](https://cloudblogs.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/).

**Entre essas versões do CBB**, o Nano Server permanece atualizado com uma série de *atualizações cumulativas*. Por exemplo, a primeira atualização cumulativa para o Nano Server foi lançada em 26 de setembro de 2016 com [KB4093120](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120). Com essa e as atualizações cumulativas seguintes, fornecemos várias opções para instalar essas atualizações no Nano Server. Neste artigo, usaremos a atualização KB3192366 como exemplo para ilustrar como obter e aplicar atualizações cumulativas ao Nano Server. Para obter mais informações sobre o modelo de atualização cumulativa, confira o [blog do Microsoft Update](/archive/blogs/mu/patching-with-windows-server-2016).

> [!NOTE]
> Se você instalar um pacote opcional do Nano Server por meio de uma mídia ou um repositório online, ele não terá as correções de segurança recentes incluídas. Para evitar uma incompatibilidade de versão entre os pacotes opcionais e o sistema operacional básico, você deve instalar a atualização cumulativa mais recente imediatamente após a instalação de todos os pacotes opcionais e **antes** de reiniciar o servidor.

No caso de uma Atualização Cumulativa para Windows Server 2016: 26 de setembro de 2016 ([KB3192366](https://support.microsoft.com/kb/3192366)), você deve instalar primeiro a atualização de pilha de manutenção mais recente para Windows 10, versão 1607: 23 de agosto de 2016 como um pré-requisito ([KB3176936](https://support.microsoft.com/kb/3176936)). Para a maioria das opções abaixo, você precisará dos arquivos .msu que contêm os pacotes de atualização .cab. Visite o Catálogo do Microsoft Update para baixar cada um destes pacotes de atualização:
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366)
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936)

Depois de baixar os arquivos .msu do Catálogo do Microsoft Update, salve-os em um compartilhamento de rede ou em um diretório local, como C:\ServicingPackages. Você pode renomear os arquivos .msu com base no número da KB como fizemos abaixo para facilitar a identificação. Em seguida, use o utilitário EXPAND para extrair os arquivos .cab dos arquivos .msu em diretórios separados e copie os .cabs em uma só pasta.

```code
    mkdir C:\ServicingPackages_expanded
    mkdir C:\ServicingPackages_expanded\KB3176936
    mkdir C:\ServicingPackages_expanded\KB3192366
    Expand C:\ServicingPackages\KB3176936.msu -F:* C:\ServicingPackages_expanded\KB3176936
    Expand C:\ServicingPackages\KB3192366.msu -F:* C:\ServicingPackages_expanded\KB3192366
    mkdir C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3176936\Windows10.0-KB3176936-x64.cab C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3192366\Windows10.0-KB3192366-x64.cab C:\ServicingPackages_cabs
```

Agora você pode usar os arquivos extraídos .cab para aplicar as atualizações a uma imagem do Nano Server de diversas maneiras, de acordo com as suas necessidades. As opções a seguir são apresentadas sem uma ordem de preferência específica: use a opção que faz mais sentido para o seu ambiente.

> [!NOTE]
> Ao usar as ferramentas DISM para fazer a manutenção do Nano Server, use uma versão do DISM que seja igual à versão do Nano Server que está passando pela manutenção ou mais recente. Faça isso executando o DISM em uma versão correspondente do Windows, instalando uma versão correspondente do [Windows ADK (Kit de Avaliação e Implantação)](https://developer.microsoft.comwindows/hardware/windows-assessment-deployment-kit) ou executando o DISM no próprio Nano Server.

## <a name="option-1-integrate-a-cumulative-update-into-a-new-image"></a>Opção 1: Integrar uma atualização cumulativa a uma nova imagem
Se você estiver criando uma imagem do Nano Server, integre a última atualização cumulativa diretamente à imagem, de modo que ela seja totalmente corrigida na primeira inicialização.

```powershell
New-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -<other parameters>
```

## <a name="option-2-integrate-a-cumulative-update-into-an-existing-image"></a>Opção 2: Integrar uma atualização cumulativa a uma imagem existente
Caso tenha uma imagem existente do Nano Server usada como uma linha de base para criar instâncias específicas do Nano Server, integre a última atualização cumulativa diretamente à imagem de linha de base existente, de modo que as máquinas criadas com a imagem sejam totalmente corrigidas na primeira inicialização.

```powershell
Edit-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -TargetPath .\NanoServer.wim
```

## <a name="option-3-apply-the-cumulative-update-to-an-existing-offline-vhd-or-vhdx"></a>Opção 3: Aplicar a atualização cumulativa a um VHD ou VHDX offline existente
Caso tenha um disco rígido virtual (VHD ou VHDX) existente, use as ferramentas DISM para aplicar a atualização ao disco rígido virtual. Você precisa garantir que o disco não esteja em uso desligando as VMs que estejam usando o disco ou desmontando o arquivo do disco rígido virtual.

- Usando o PowerShell

   ```powershell
   Mount-WindowsImage -ImagePath .\NanoServer.vhdx -Path .\MountDir -Index 1
   Add-WindowsPackage -Path .\MountDir -PackagePath  C:\ServicingPackages_cabs
   Dismount-WindowsImage -Path .\MountDir -Save
   ```

- Como usar o dism.exe

   ```code
   dism.exe /Mount-Image /ImageFile:C:\NanoServer.vhdx /Index:1 /MountDir:C:\MountDir
   dism.exe /Image:C:\MountDir /Add-Package /PackagePath:C:\ServicingPackages_cabs
   dism.exe /Unmount-Image /MountDir:C:\MountDir /Commit
   ```

## <a name="option-4-apply-the-cumulative-update-to-a-running-nano-server"></a>Opção 4: Aplicar a atualização cumulativa a um Nano Server em execução
Caso tenha uma VM do Nano Server em execução ou host físico e baixou o arquivo .cab para a atualização, use as ferramentas DISM para aplicar a atualização enquanto o sistema operacional estiver online. Você precisará copiar o arquivo .cab localmente no Nano Server ou em um local acessível na rede. Se você estiver aplicando uma atualização da pilha de manutenção, reinicie o servidor depois de aplicar a atualização da pilha de manutenção antes de aplicar atualizações adicionais.

> [!NOTE]
> Se você criar a imagem do VHD ou do VHDX do Nano Server usando o cmdlet New-NanoServerImage e não especificar um MaxSize para o arquivo do disco rígido virtual, o tamanho padrão de 4 GB será muito pequeno para aplicar a atualização cumulativa. Antes de instalar a atualização, use o Gerenciador do Hyper-V, o Gerenciamento de Disco, o PowerShell ou outra ferramenta para expandir o tamanho do disco rígido virtual e o volume do sistema para, pelo menos, 10 GB ou use o parâmetro ScratchDir nas ferramentas DISM para definir o diretório temporário com um volume que tenha, pelo menos, 10 GB de espaço livre.

```powershell
$s = New-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
Copy-Item -ToSession $s -Path C:\ServicingPackages_cabs -Destination C:\ServicingPackages_cabs -Recurse
Enter-PSSession $s
```

- Usando o PowerShell

   ```powershell
   # Apply the servicing stack update first and then restart
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

- Como usar o dism.exe
   ```powershell
   # Apply the servicing stack update first and then restart
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   
   # After the operation completes successfully and you are prompted to restart, it's safe to
   # press Ctrl+C to cancel the pipeline and return to the prompt
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

## <a name="option-5-download-and-install-the-cumulative-update-to-a-running-nano-server"></a>Opção 5: Baixar e instalar a atualização cumulativa para um Nano Server em execução

Caso tenha um host físico ou uma VM do Nano Server em execução, use o provedor WMI do Windows Update para baixar e instalar a atualização enquanto o sistema operacional estiver online. Com esse método, você não precisará baixar o arquivo .msu separadamente do Catálogo do Microsoft Update. O provedor WMI detectará, baixará e instalará todas as atualizações disponíveis de uma vez.

```powershell
Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
```

- Verificar se há atualizações disponíveis
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}
   $result.Updates
   ```

- Instalar todas as atualizações disponíveis
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   Invoke-CimMethod -InputObject $ci -MethodName ApplyApplicableUpdates
   Restart-Computer; exit
   ```

- Obter uma lista das atualizações instaladas
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}
   $result.Updates
   ```
   
## <a name="additional-options"></a>Opções adicionais
Outros métodos para atualizar o Nano Server podem sobrepor ou complementar as opções acima. Essas opções incluem o uso do WSUS (Windows Server Update Services), do System Center VMM (Virtual Machine Manager), do Agendador de Tarefas ou de uma solução não Microsoft.
- [Configurar o Windows Update para o WSUS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd939844(v=ws.10)) definindo as chaves do Registro a seguir:
  - WUServer
  - WUStatusServer (geralmente usa o mesmo valor de WUServer)
  - UseWUServer
  - AUOptions
- [Gerenciando atualizações de malha no VMM](/previous-versions/system-center/system-center-2012-R2/gg675084(v=sc.12))
- [Registrando uma tarefa agendada](/previous-versions/system-center/system-center-2012-R2/gg675084(v=sc.12))
