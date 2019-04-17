---
title: Colocando um servidor de Espaços de Armazenamento Diretos offline para manutenção
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
Keywords: Storage Spaces Direct, S2D, maintenance
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 96ae0ad0d1def12ab68466f0a9ae60d0afcc2c17
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992519"
---
# Colocando um servidor de Espaços de Armazenamento Diretos offline para manutenção

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece diretrizes sobre como reiniciar ou desligar corretamente servidores com [Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md).

Com os Espaços de Armazenamento Diretos, colocar um servidor offline (desativando-o) também significa colocar offline partes do armazenamento que é compartilhado entre todos os servidores do cluster. Isso requer a pausa (suspensão) do servidor que você quer colocar offline, movendo funções para outros servidores no cluster e verificando se todos os dados estão disponíveis nos outros servidores do cluster para que os dados permaneçam seguros e acessíveis durante toda a manutenção.

Use os procedimentos a seguir para pausar corretamente um servidor em um cluster de Espaços de Armazenamento Diretos antes de colocá-lo offline. 

   > [!IMPORTANT]
   > Para instalar atualizações em um cluster de Espaços de Armazenamento Diretos, use o CAU (Atualização com Suporte a Cluster), que executa automaticamente os procedimentos neste tópico para que você não precise fazê-lo ao instalar atualizações. Para obter mais informações, consulte [CAU (Atualização com Suporte a Cluster)](https://technet.microsoft.com/library/hh831694.aspx).

## Verificar se é seguro colocar o servidor offline

Antes de colocar um servidor offline para manutenção, verifique a integridade de todos os volumes.

Para fazer isso, abra uma sessão do PowerShell com permissões de administrador e execute o seguinte comando para exibir o status do volume:

```PowerShell
Get-VirtualDisk 
```

Este é um exemplo da saída do comando:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Verifique se a propriedade **HealthStatus** de cada volume (disco virtual) está **Íntegra**.

Para fazer isso no Gerenciador de Cluster de Failover, acesse **Armazenamento** > **Discos**.

Verifique se a coluna **Status** coluna de cada volume (disco virtual) mostra **Online**.

## Pausar e esvaziar o servidor

Antes de reiniciar ou desligar o servidor, pause e remova (retire) todas as funções, como máquinas virtuais em execução. Isso também dá aos Espaços de Armazenamento Diretos uma oportunidade de liberar e confirmar dados discretamente para garantir que o desligamento seja transparente para qualquer aplicativo em execução nesse servidor.

   > [!IMPORTANT]
   > Sempre pause e esvazie servidores clusterizados antes de reiniciá-los ou desligá-los.

No PowerShell, execute o seguinte cmdlet (como administrador) para pausar e esvaziar.

```PowerShell
Suspend-ClusterNode -Drain
```

Para fazer isso no Gerenciador de Cluster de Failover, acesse **Nós**, clique com botão direito do mouse no nó e selecione **Pausar** > **Esvaziar Funções**.

![Pausar/Esvaziar](media/maintain-servers/pause-drain.png)

Todas as máquinas virtuais começarão a realizar a migração ao vivo para outros servidores no cluster. Isso pode demorar alguns minutos.

   > [!NOTE]
   > Quando você pausar e esvaziar o nó do cluster corretamente, o Windows executará uma verificação de segurança automática para garantir que é seguro continuar. Se houver volumes não íntegros, ele interromperá o processo e alertará você de que não é seguro continuar.

![Verificação de segurança](media/maintain-servers/safety-check.png)

## Desligar o servidor

Depois que o servidor tiver concluído o esvaziamento, ele aparecerá como **Em Pausa** no Gerenciador de Cluster de Failover e no PowerShell.

![Em pausa](media/maintain-servers/paused.png)

Agora você pode reiniciá-lo ou desligá-lo com segurança como faria normalmente (por exemplo, usando os cmdlets Restart-Computer ou Stop-Computer do PowerShell).

```PowerShell
Get-VirtualDisk 

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

Incompleto ou degradação Status operacional é normal quando nós são desligado ou iniciar/parar o cluster em um nó de serviço e não deve causar preocupação. Todos os volumes permanecem online e acessíveis.

## Retomar o servidor

Quando o servidor estiver pronto para começar a hospedagem das cargas de trabalho novamente, retome-o.

No PowerShell, execute o seguinte cmdlet (como administrador) para continuar.

```PowerShell
Resume-ClusterNode
```

Para mover novamente as funções que anteriormente estavam sendo executadas nesse servidor, use o sinalizador opcional **-Failback**.

```PowerShell
Resume-ClusterNode –Failback Immediate
```

Para fazer isso no Gerenciador de Cluster de Failover, acesse **Nós**, clique com botão direito do mouse no nó e selecione **Retomar** > **Fazer Failback de Funções**.

![Retomar/Failback](media/maintain-servers/resume-failback.png)

## Aguardar armazenamento para ressincronizar

Quando o servidor for retomado, todas as gravações realizadas enquanto estava disponível precisam ser sincronizadas novamente. Isso acontece automaticamente. Usando o controle de alterações inteligente, não é necessário que *todos* os dados sejam digitalizados ou sincronizados, mas apenas as alterações. Esse processo é reprimido para atenuar o impacto das cargas de trabalho de produção. Dependendo de quanto tempo o servidor permaneceu pausado e quantos dados novos foram gravados, esse processo pode levar vários minutos para ser concluído.

Você deve esperar a ressincronização ser concluída antes de executar quaisquer outros servidores no cluster offline.

No PowerShell, execute o seguinte cmdlet (como administrador) para monitorar o progresso.

```PowerShell
Get-StorageJob
```
Este é um exemplo de saída, mostrando os trabalhos de ressincronização (reparo):
```
Name   IsBackgroundTask ElapsedTime JobState  PercentComplete BytesProcessed BytesTotal
----   ---------------- ----------- --------  --------------- -------------- ----------
Repair True             00:06:23    Running   65              11477975040    17448304640
Repair True             00:06:40    Running   66              15987900416    23890755584
Repair True             00:06:52    Running   68              20104802841    22104819713
```

O **BytesTotal** mostra a quantidade de armazenamento que precisa ser ressincronizada. O **PercentComplete** exibe o progresso.

   > [!WARNING]
   > Não é seguro colocar outro servidor offline até que esses trabalhos de reparo sejam concluídos.

Durante esse tempo, os volumes continuarão aparecendo como **Aviso**, o que é normal. 

Por exemplo, se você usar o cmdlet `Get-VirtualDisk`, poderá ver o seguinte resultado:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                InService         Warning      True           1 TB
MyVolume2    Mirror                InService         Warning      True           1 TB
MyVolume3    Mirror                InService         Warning      True           1 TB
```

Depois que os trabalhos forem concluídos, verifique se os volumes aparecem como **Íntegro** novamente usando o cmdlet `Get-VirtualDisk`. Veja a seguir um exemplo de saída:

```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Agora é seguro pausar e reiniciar outros servidores no cluster.

## Como atualizar nós espaços de armazenamento diretos offline
Use as etapas a seguir para o caminho de seu sistema de espaços de armazenamento diretos rapidamente. Ele envolve o agendamento de uma janela de manutenção e colocar o sistema para baixo para a aplicação de patch. Se houver uma atualização de segurança críticas que você precisa rapidamente aplicada ou talvez você precise garantir a aplicação de patch é concluída na sua janela de manutenção, esse método pode ser para você. Esse processo traz o cluster de espaços de armazenamento diretos, patches-lo e junta tudo isso novamente. A compensação é o tempo de inatividade os recursos hospedado.

1. Planeje sua janela de manutenção.
2. Coloque os discos virtuais off-line.
3. Pare o cluster para colocar o pool de armazenamento offline. Execute o cmdlet **Stop-Cluster** ou use o Gerenciador de Cluster de Failover para interromper o cluster.
4. Configurar o serviço de cluster para **desabilitado** no Services. msc em cada nó. Isso impede que o serviço de cluster sendo inicializado enquanto está sendo corrigido.
5. Aplicar a atualização cumulativa do Windows Server e as necessárias atualizações da pilha de manutenção para todos os nós. (Você pode atualizar todos os nós ao mesmo tempo, sem precisar aguardar como o cluster é para baixo).  
6. Reinicie os nós e garantir que tudo tenha uma boa aparência.
7. Defina o serviço cluster como **automático** em cada nó.
8. Inicie o cluster. Execute o cmdlet **Start-Cluster** ou usar o Gerenciador de Cluster de Failover. 

   Dê a ele alguns minutos.  Verifique se o pool de armazenamento está íntegro.
9. Coloque os discos virtuais on-line.
10. Monitore o status dos discos virtuais executando os cmdlets **Get-Volume** e **Get-VirtualDisk** .


## Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [CAU (Atualização com Suporte a Cluster)](https://technet.microsoft.com/library/hh831694.aspx)
