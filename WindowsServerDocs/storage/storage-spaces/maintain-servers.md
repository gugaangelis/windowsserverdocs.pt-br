---
title: Colocando um servidor de Espaços de Armazenamento Diretos offline para manutenção
ms.prod: windows-server
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: a317f358c37f607475890efe773b57ee8efaeb14
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473473"
---
# <a name="taking-a-storage-spaces-direct-server-offline-for-maintenance"></a>Colocando um servidor de Espaços de Armazenamento Diretos offline para manutenção

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece diretrizes sobre como reiniciar ou desligar corretamente servidores com [Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md).

Com os Espaços de Armazenamento Diretos, colocar um servidor offline (desativando-o) também significa colocar offline partes do armazenamento que é compartilhado entre todos os servidores do cluster. Isso requer a pausa (suspensão) do servidor que você quer colocar offline, movendo funções para outros servidores no cluster e verificando se todos os dados estão disponíveis nos outros servidores do cluster para que os dados permaneçam seguros e acessíveis durante toda a manutenção.

Use os procedimentos a seguir para pausar corretamente um servidor em um cluster de Espaços de Armazenamento Diretos antes de colocá-lo offline.

   > [!IMPORTANT]
   > Para instalar atualizações em um cluster de Espaços de Armazenamento Diretos, use o CAU (Atualização com Suporte a Cluster), que executa automaticamente os procedimentos neste tópico para que você não precise fazê-lo ao instalar atualizações. Para obter mais informações, consulte [CAU (Atualização com Suporte a Cluster)](https://technet.microsoft.com/library/hh831694.aspx).

## <a name="verifying-its-safe-to-take-the-server-offline"></a>Verificar se é seguro colocar o servidor offline

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

Para fazer isso no Gerenciador de cluster de failover, vá para **Storage**  >  **discos**de armazenamento.

Verifique se a coluna **Status** coluna de cada volume (disco virtual) mostra **Online**.

## <a name="pausing-and-draining-the-server"></a>Pausar e esvaziar o servidor

Antes de reiniciar ou desligar o servidor, pause e remova (retire) todas as funções, como máquinas virtuais em execução. Isso também dá aos Espaços de Armazenamento Diretos uma oportunidade de liberar e confirmar dados discretamente para garantir que o desligamento seja transparente para qualquer aplicativo em execução nesse servidor.

   > [!IMPORTANT]
   > Sempre pause e esvazie servidores clusterizados antes de reiniciá-los ou desligá-los.

No PowerShell, execute o seguinte cmdlet (como administrador) para pausar e esvaziar.

```PowerShell
Suspend-ClusterNode -Drain
```

Para fazer isso no Gerenciador de Cluster de Failover, acesse **Nós**, clique com botão direito do mouse no nó e selecione **Pausar** > **Esvaziar Funções**.

![Pausar/Esvaziar](media/maintain-servers/pause-drain.png)

Todas as máquinas virtuais começarão a realizar a migração ao vivo para outros servidores no cluster. Isso pode levar alguns minutos.

   > [!NOTE]
   > Quando você pausar e esvaziar o nó do cluster corretamente, o Windows executará uma verificação de segurança automática para garantir que é seguro continuar. Se houver volumes não íntegros, ele interromperá o processo e alertará você de que não é seguro continuar.

![Verificação de segurança](media/maintain-servers/safety-check.png)

## <a name="shutting-down-the-server"></a>Desligar o servidor

Depois que o servidor tiver concluído o esvaziamento, ele aparecerá como **Em Pausa** no Gerenciador de Cluster de Failover e no PowerShell.

![Em Pausa](media/maintain-servers/paused.png)

Agora você pode reiniciá-lo ou desligá-lo com segurança como faria normalmente (por exemplo, usando os cmdlets Restart-Computer ou Stop-Computer do PowerShell).

```PowerShell
Get-VirtualDisk

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

O status operacional incompleto ou degradado é normal quando os nós estão sendo encerrados ou começam/parando o serviço de cluster em um nó e não devem causar preocupação. Todos os volumes permanecem online e acessíveis.

## <a name="resuming-the-server"></a>Retomar o servidor

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

## <a name="waiting-for-storage-to-resync"></a>Aguardar armazenamento para ressincronizar

Quando o servidor é retomado, todas as novas gravações que ocorreram enquanto não estavam disponíveis precisam ser ressincronizadas. Isso acontece automaticamente. Usando o controle de alterações inteligente, não é necessário que *todos* os dados sejam digitalizados ou sincronizados, mas apenas as alterações. Esse processo é reprimido para atenuar o impacto das cargas de trabalho de produção. Dependendo de quanto tempo o servidor permaneceu pausado e quantos dados novos foram gravados, esse processo pode levar vários minutos para ser concluído.

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

## <a name="how-to-update-storage-spaces-direct-nodes-offline"></a>Como atualizar nós Espaços de Armazenamento Diretos offline
Use as etapas a seguir para demarcar o sistema Espaços de Armazenamento Diretos rapidamente. Ele envolve o agendamento de uma janela de manutenção e o desligamento do sistema para aplicação de patches. Se houver uma atualização de segurança crítica que você precisa aplicar rapidamente ou talvez precise garantir que a aplicação de patch seja concluída na janela de manutenção, esse método pode ser para você. Esse processo abre o Espaços de Armazenamento Diretos cluster, o corrige e o coloca tudo novamente. A compensação é o tempo de inatividade para os recursos hospedados.

1. Planeje sua janela de manutenção.
2. Coloque os discos virtuais offline.
3. Interrompa o cluster para colocar o pool de armazenamento offline. Execute o cmdlet **Stop-cluster** ou use Gerenciador de cluster de failover para interromper o cluster.
4. Defina o serviço de cluster como **desabilitado** em Services. msc em cada nó. Isso impede que o serviço de cluster seja inicializado durante o patch.
5. Aplique a atualização cumulativa do Windows Server e todas as atualizações necessárias da pilha de manutenção a todos os nós. (Você pode atualizar todos os nós ao mesmo tempo, não há necessidade de esperar, pois o cluster está inoperante).
6. Reinicie os nós e verifique se tudo está correto.
7. Defina o serviço de cluster novamente como **automático** em cada nó.
8. Inicie o cluster. Execute o cmdlet **Start-cluster** ou use Gerenciador de cluster de failover.

   Aguarde alguns minutos.  Verifique se o pool de armazenamento está íntegro.
9. Coloque os discos virtuais novamente online.
10. Monitore o status dos discos virtuais executando os cmdlets **Get-volume** e **Get-VirtualDisk** .


## <a name="additional-references"></a>Referências adicionais

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [CAU (Atualização com Suporte a Cluster)](https://technet.microsoft.com/library/hh831694.aspx)
