---
title: Atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou no Windows Server
description: Fornece instruções e considerações para atualizar a versão de uma máquina virtual
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 96678dfab2a3d5b6f503d8ce9d00850a3c437b35
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370600"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou no Windows Server

>Aplica-se a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Disponibilize os recursos mais recentes do Hyper-V em suas máquinas virtuais atualizando a versão de configuração. Não faça isso até:

- Você atualiza seus hosts Hyper-V para a versão mais recente do Windows ou do Windows Server.
- Você atualiza o nível funcional do cluster.
- Você tem certeza de que não precisará mover a máquina virtual de volta para um host Hyper-V que executa uma versão anterior do Windows ou Windows Server.

Para obter mais informações, consulte [atualização sem interrupção do sistema operacional do cluster](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) e [executar uma atualização sem interrupção de um cluster de host do Hyper-V no VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Etapa 1: verificar as versões de configuração da máquina virtual

1. Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.
2. Clique com o botão direito do mouse em Windows PowerShell e selecione **Executar como administrador**.
3. Use o cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm). Execute o comando a seguir para obter as versões de suas máquinas virtuais.

```PowerShell
Get-VM * | Format-Table Name, Version
```

Você também pode ver a versão de configuração no Gerenciador do Hyper-V selecionando a máquina virtual e examinando a guia **Resumo** .

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>Etapa 2: atualizar a versão de configuração da máquina virtual

1. Desligue a máquina virtual no Gerenciador do Hyper-V.
2. Selecione a ação > atualizar a versão de configuração. Se essa opção não estiver disponível para a máquina virtual, ela já estará na versão de configuração mais alta com suporte do host do Hyper-V.

Para atualizar a versão de configuração da máquina virtual usando o Windows PowerShell, use o cmdlet [Update-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) . Execute o comando a seguir, em que vmname é o nome da máquina virtual.

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>Versões de configuração de máquina virtual com suporte

Execute o cmdlet do PowerShell [Get-VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) para ver quais versões de configuração de máquina virtual seu host Hyper-V dá suporte. Quando você cria uma máquina virtual, ela é criada com a versão de configuração padrão. Para ver qual é o padrão, execute o comando a seguir.

```PowerShell
Get-VMHostSupportedVersion -Default
```

Se você precisar criar uma máquina virtual que pode ser movida para um host Hyper-V que executa uma versão mais antiga do Windows, use o cmdlet [New-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) com o parâmetro-version. Por exemplo, para criar uma máquina virtual que você pode mover para um host Hyper-V que executa o Windows Server 2012 R2, execute o comando a seguir. Este comando criará uma máquina virtual chamada "WindowsCV5" com uma versão de configuração 5,0.

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>Você pode importar máquinas virtuais que foram criadas para um host Hyper-V executando uma versão mais antiga do Windows ou restaurá-las do backup. Se a versão de configuração da VM não estiver listada como com suporte para o sistema operacional do host Hyper-V na tabela abaixo, você precisará atualizar a versão de configuração da VM antes de poder iniciar a VM.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versões de configuração de VM com suporte para hosts de manutenção de longo prazo

A tabela a seguir lista as versões de configuração da VM com suporte em hosts que executam uma versão de manutenção em longo prazo do Windows.

| Versão do Windows do host do Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versões de configuração de VM com suporte para hosts de canal semestral

A tabela a seguir lista as versões de configuração de VM para hosts que executam uma versão de canal semianual com suporte no momento do Windows. Para obter mais informações sobre versões de canal semianuais do Windows, visite as páginas a seguir para [Windows Server](../../../get-started-19/servicing-channels-19.md) e [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

| Versão do Windows do host do Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Atualização de 2019 de maio do Windows 10 (versão 1903) |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server, versão 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, versão 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Atualização de 10 de outubro de 2018 do Windows (versão 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, versão 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Atualização do Windows 10 de abril de 2018 (versão 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Atualização dos criadores de outono do Windows 10 (versão 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Atualização do Windows 10 para criadores (versão 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Atualização de aniversário do Windows 10 (versão 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>Por que devo atualizar a versão de configuração da máquina virtual?

Quando você move ou importa uma máquina virtual para um computador que executa o Hyper-V no Windows Server 2019, no Windows Server 2016 ou no Windows 10, a configuração da máquina virtual não é atualizada automaticamente. Isso significa que você pode mover a máquina virtual de volta para um host Hyper-V que executa uma versão anterior do Windows ou Windows Server. Mas isso também significa que você não pode usar alguns dos novos recursos de máquina virtual até atualizar manualmente a versão de configuração. Você não pode fazer downgrade da versão de configuração da máquina virtual depois de atualizá-la.

A versão de configuração da máquina virtual representa a compatibilidade da configuração da máquina virtual, do estado salvo e dos arquivos de instantâneo com a versão do Hyper-V. Ao atualizar a versão de configuração, você altera a estrutura de arquivos usada para armazenar a configuração de máquinas virtuais e os arquivos de ponto de verificação. Você também atualiza a versão de configuração para a versão mais recente com suporte pelo host do Hyper-V. Máquinas virtuais atualizadas agora usam um novo formato de arquivo de configuração, que foi projetado para aumentar a eficiência de leitura e gravação de dados de configuração de máquina virtual. A atualização também reduz a possibilidade de obter dados corrompidos em caso de falha de armazenamento.

A tabela a seguir lista as descrições, extensões de nome de arquivo e locais padrão para cada tipo de arquivo usado para máquinas virtuais novas ou atualizadas.

 |Tipos de arquivo de máquina virtual | Descrição|
 |---|---|
|Configuração |Informações de configuração de máquina virtual que são armazenadas em formato de arquivo binário. <br /> Extensão de nome de arquivo:. vmcx <br /> Local padrão: computadores C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |Estado do tempo de execução|Informações de estado do tempo de execução da máquina virtual armazenadas em formato de arquivo binário. <br />Extensão de nome de arquivo:. VMRS e. vmgs <br />Local padrão: computadores C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
|Disco rígido virtual|Armazena discos rígidos virtuais para a máquina virtual. <br /> Extensão de nome de arquivo:. VHD ou. vhdx <br />Local padrão: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual discos rígidos|
 |Disco rígido virtual automático |Arquivos de disco diferencial usados para pontos de verificação de máquina virtual. <br /> Extensão de nome de arquivo:. avhdx <br /> Local padrão: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual discos rígidos|
 |Ponto de verificação|Pontos de verificação são armazenados em vários arquivos de ponto de verificação. Cada ponto de verificação cria um arquivo de configuração e o arquivo de estado de runtime. <br /> Extensões de nome de arquivo:. VMRS e. vmcx <br />Local padrão: C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>O que acontecerá se eu não atualizar a versão de configuração da máquina virtual?

Se você tiver máquinas virtuais que criou com uma versão anterior do Hyper-V, alguns recursos que estão disponíveis no sistema operacional host mais recente podem não funcionar com essas máquinas virtuais até que você atualize a versão de configuração.

Como uma orientação geral, é recomendável atualizar a versão de configuração depois de atualizar com êxito os hosts de virtualização para uma versão mais recente do Windows e sentir a certeza de que você não precisa reverter. Quando você estiver usando o recurso de [atualização sem interrupção do sistema operacional do cluster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) , isso normalmente seria depois de atualizar o nível funcional do cluster. Dessa forma, você também se beneficiará dos novos recursos e das otimizações e alterações internas.

>[!NOTE]
>Depois que a versão da configuração da VM for atualizada, a VM não poderá ser iniciada em hosts que não dão suporte à versão de configuração atualizada.

A tabela a seguir mostra a versão mínima de configuração de máquina virtual necessária para usar alguns recursos do Hyper-V.

|Recurso|Versão mínima da configuração da VM|
|---|---|
|Adição/remoção ativa de memória|6.2|
|Inicialização segura para VMs do Linux|6.2|
|Pontos de Verificação de Produção|6.2|
|PowerShell Direct |6.2|
|Agrupamento de máquina virtual|6.2|
|vTPM (Virtual Trusted Platform Module)|7.0|
|VMMQ (várias filas de máquina virtual)|7.1|
|Suporte do XSAVE|8.0|
|Unidade de armazenamento de chaves|8.0|
|VBS (suporte de segurança baseado em virtualização) do convidado|8.0|
|Virtualização aninhada|8.0|
|Contagem de processadores virtuais|8.0|
|VMs de memória grande|8.0|
|Aumentar o número máximo padrão de dispositivos virtuais para 64 por dispositivo (por exemplo, rede e dispositivos atribuídos)|8.3|
|Permitir recursos de processador adicionais para Perfmon|9,0|
|Expor automaticamente a configuração de [multithreading simultânea](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) para VMs em execução em hosts usando o [Agendador principal](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9,0|
|Suporte à hibernação|9,0|

Para obter mais informações sobre esses recursos, consulte [o que há de novo no Hyper-V no Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).

