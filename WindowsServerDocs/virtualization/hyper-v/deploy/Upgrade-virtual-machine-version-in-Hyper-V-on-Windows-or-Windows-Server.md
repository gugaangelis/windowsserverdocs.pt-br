---
title: Atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou Windows Server
description: Fornece instruções e considerações para atualizar a versão de uma máquina virtual
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: KBDAzure
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 8b277796492ca7d72b1a8713484c691cb9a8dca3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819437"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou Windows Server

>Aplica-se a: Windows 10, Windows Server 2016, Windows Server 2019

Disponibiliza os recursos mais recentes do Hyper-V em suas máquinas virtuais por meio da atualização da versão de configuração. Não faça isso até que:

- Atualizar seus hosts do Hyper-V para a versão mais recente do Windows ou Windows Server.
- Atualizar o nível funcional do cluster.
- Você tiver certeza de que você não precisa mover a máquina virtual de volta para um host do Hyper-V que executa uma versão anterior do Windows ou Windows Server.

Para obter mais informações, consulte [atualização sem interrupção do Cluster de sistema operacional](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) e [executar uma atualização sem interrupção de um cluster de host do Hyper-V no VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Etapa 1: Verificar as versões de configuração de máquina virtual

1. Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.
2. Windows PowerShell com o botão direito e selecione **executar como administrador**.
3. Use o [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)cmdlet. Execute o seguinte comando para obter as versões das suas máquinas virtuais.

```PowerShell
Get-VM * | Format-Table Name, Version
```

Você também pode ver a versão de configuração no Gerenciador do Hyper-V, selecionando a máquina virtual e observando os **resumo** guia.

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>Etapa 2: Atualizar a versão de configuração de máquina virtual

1. Desligue a máquina virtual no Gerenciador do Hyper-V.
2. Selecionar ação > Atualizar versão de configuração. Se essa opção não está disponível para a máquina virtual, em seguida, ele já é a versão mais alto de configuração compatível com o host Hyper-V.

Para atualizar a versão de configuração de máquina virtual usando o Windows PowerShell, use o [VMVersion atualização](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet. Execute o seguinte comando no qual vmname é o nome da máquina virtual.

```PowerShell
Update-VMVersion <vmname>
```

## <a name="BKMK_SupportedConfigVersions"></a>Versões de configuração de máquina virtual com suporte

Execute o cmdlet do PowerShell [Get-VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) para ver quais versões de configuração de máquina virtual dá suporte ao seu Host do Hyper-V. Quando você cria uma máquina virtual, ele é criado com a versão da configuração padrão. Para ver o que é o padrão, execute o comando a seguir.

```PowerShell
Get-VMHostSupportedVersion -Default
```

Se você precisar criar uma máquina virtual que você pode mover para um Host Hyper-V que executa uma versão anterior do Windows, use o [New-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) cmdlet com o parâmetro - version. Por exemplo, para criar uma máquina virtual que você pode mover para um host Hyper-V que executa o Windows Server 2012 R2, execute o comando a seguir. Esse comando criará uma máquina virtual chamada "WindowsCV5" com uma versão de configuração 5.0.

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>Você pode importar as máquinas virtuais que foram criadas para um host Hyper-V executando uma versão mais antiga do Windows ou restaurá-los a partir do backup. Se a versão de configuração da VM não está listado como suportado para seu sistema operacional do host do Hyper-V na tabela a seguir, você precisa atualizar a versão de configuração da VM antes de iniciar a VM.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versões com suporte de configuração de VM para hosts de manutenção de longo prazo

A tabela a seguir lista as versões de configuração de VM que são compatíveis com hosts executando uma versão de manutenção de longo prazo do Windows.

|Versão do host do Hyper-V do Windows| 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
|---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise LTSC 2019|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versões com suporte de configuração de VM para hosts de canal semestral

A tabela a seguir lista as versões de configuração de VM para hosts que executam uma versão de canal semestral atualmente com suporte do Windows. Para obter mais informações sobre versões de canal semestral do Windows, visite as páginas a seguir para [Windows Server](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview) e [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

|Versão do host do Hyper-V do Windows| 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
|---|---|---|---|---|---|---|---|---|---|
|Windows Server, versão 1809|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|10 de outubro de 2018 do Windows Update (versão 1809)|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, versão 1803|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|10 de abril de 2018 do Windows Update (versão 1803)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update (versão 1709)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, versão 1709|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Atualização do Windows 10 para criadores (versão 1703)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Atualização de aniversário do Windows 10 (versão 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>Por que devo atualizar a versão de configuração de máquina virtual?

Quando você move ou importar uma máquina virtual para um computador que executa o Hyper-V no Windows Server 2019, Windows Server 2016 ou Windows 10, a máquina virtual "configuração s não será atualizada automaticamente. Isso significa que você pode mover a máquina virtual volta para um host do Hyper-V que executa uma versão anterior do Windows ou Windows Server. Mas, isso também significa que você não pode usar alguns dos novos recursos de máquina virtual até atualizar manualmente a versão de configuração. Não é possível fazer o downgrade da versão de configuração de máquina virtual após ela ter sido atualizado.

A versão de configuração de máquina virtual representa a compatibilidade da configuração da máquina virtual, salva o estado e os arquivos de instantâneo com a versão do Hyper-V. Quando você atualiza a versão de configuração, você pode alterar a estrutura de arquivo que é usada para armazenar a configuração de máquinas virtuais e os arquivos de ponto de verificação. Você também pode atualizar a versão de configuração para a versão mais recente com suporte pelo host do Hyper-V. Máquinas virtuais atualizadas agora usam um novo formato de arquivo de configuração, que foi projetado para aumentar a eficiência de leitura e gravação de dados de configuração de máquina virtual. A atualização também reduz a possibilidade de obter dados corrompidos em caso de falha de armazenamento.

A tabela a seguir lista as descrições, extensões de nome de arquivo e locais de padrão para cada tipo de arquivo que é usado para máquinas de virtuais novas ou atualizadas.

 |Tipos de arquivos de máquina virtual | Descrição|
 |---|---|
|Configuração |Informações de configuração de máquina virtual são armazenadas no formato de arquivo binário. <br /> Extensão de nome de arquivo:. vmcx <br /> Local padrão: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
 |Estado de tempo de execução|Máquina virtual informações de estado de tempo de execução que são armazenadas no formato de arquivo binário. <br />Extensão de nome de arquivo:. vmrs e .vmgs <br />Local padrão: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
|Disco rígido virtual|Armazena os discos rígidos virtuais para a máquina virtual. <br /> Extensão de nome de arquivo:. vhd ou. vhdx <br />Local padrão: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Disco rígido virtual automática |Arquivos de disco diferencial usados para pontos de verificação de máquina virtual. <br /> Extensão de nome de arquivo:. avhdx <br /> Local padrão: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Checkpoint|Pontos de verificação são armazenados em vários arquivos de ponto de verificação. Cada ponto de verificação cria um arquivo de configuração e o arquivo de estado de tempo de execução. <br /> Extensões de nome de arquivo:. vmrs e. vmcx <br />Local padrão: C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>O que acontece se eu não atualizar a versão de configuração de máquina virtual?

Se você tiver máquinas virtuais que você criou com uma versão anterior do Hyper-V, o alguns recursos que estão disponíveis no host mais recente que sistema operacional pode não funcionar com essas máquinas virtuais até você atualiza a versão de configuração.

Como uma diretriz geral, recomendamos atualizar a versão de configuração depois que os hosts de virtualização para uma versão mais recente do Windows foi atualizado com êxito e se sentir confiantes de que você não precise reverter. Quando você estiver usando o [atualização sem interrupção do SO do cluster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) recurso, isso normalmente seria depois de atualizar o nível funcional do cluster. Dessa forma, você se beneficiará de novos recursos e alterações internas e também otimizações.

>[!NOTE]
>Depois que a versão de configuração da VM for atualizada, a VM não será iniciada em hosts que não dão suporte a versão de configuração atualizada.

A tabela a seguir mostra a versão de configuração de mínimo de máquina virtual necessária para usar alguns recursos do Hyper-V.

|Recurso|Versão mínima de configuração de VM|
|---|---|
|Adição/remoção ativa de memória|6.2|
|Inicialização segura para VMs do Linux|6.2|
|Pontos de Verificação de Produção|6.2|
|PowerShell Direct |6.2|
|Agrupamento de máquina virtual|6.2|
|vTPM (Virtual Trusted Platform Module)|7.0|
|Máquina virtual com várias filas (VMMQ)|7.1|
|Suporte a XSAVE|8.0|
|Unidade de armazenamento de chaves|8.0|
|Suporte de segurança baseada em virtualização de convidado (VBS)|8.0|
|Virtualização aninhada|8.0|
|Número de processadores virtuais|8.0|
|Grande quantidade de memória de VMs|8.0|
|Aumentar o número de máximo padrão para os dispositivos virtuais 64 por dispositivo (por exemplo, rede e atribuídos dispositivos)|8.3|
|Permitir que os recursos adicionais do processador para o Perfmon|9.0|
|Expor automaticamente [simultâneas multithreading](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) configuração para VMs em execução em hosts usando o [Core Agendador](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9.0|
|Suporte à hibernação|9.0|

Para obter mais informações sobre esses recursos, consulte [o que há de novo no Hyper-V no Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).

