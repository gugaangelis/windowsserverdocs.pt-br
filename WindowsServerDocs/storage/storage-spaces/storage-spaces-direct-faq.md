---
title: Espaços de armazenamento diretos - perguntas frequentes
description: Saiba mais sobre espaços de armazenamento diretos
keywords: Espaços de Armazenamento
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818907"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Espaços de armazenamento diretos - perguntas frequentes (FAQ)

Este artigo lista algumas comuns e perguntas frequentes relacionadas ao [espaços de armazenamento diretos](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>Quando você usar espaços de armazenamento diretos com 3 nós, você pode iniciar desempenho e níveis de capacidade?

Sim, você pode obter um desempenho e a camada de capacidade em uma configuração de espaços de armazenamento diretos 2 ou 3 nós. No entanto, certifique-se de que você tenha 2 dispositivos de capacidade. Isso significa que você deve usar todos os três tipos de dispositivos: NVME, SSD e HDD.
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Sistema de arquivos refs fornece tiaring em tempo real com espaços de armazenamento diretos. O REFS oferece a mesma funcionalidade com espaços de armazenamento compartilhado em 2016?

Não, você não obterá em tempo real para as camadas com espaços de armazenamento compartilhado com o 2016. Isso é somente para espaços de armazenamento diretos. 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>Pode usar um sistema de arquivos NTFS com espaços de armazenamento diretos?
  
Sim, você pode usar o sistema de arquivos NTFS com espaços de armazenamento diretos. No entanto, é recomendável REFS. NTFS não fornecem camadas em tempo real. 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>Configurei o espaços de armazenamento diretos clusters de 2 nós, na qual o disco virtual é configurado como resiliência de espelho de 2 vias. Se eu adicionar um novo domínio de falha, a resiliência do disco virtual existente será alterado?

Depois de adicionar o novo domínio de falha, os novos discos virtuais que você criar saltará para 3 vias. No entanto, o disco virtual existente permanecerá um disco de 2 vias espelhado. Você pode copiar os dados para os novos discos virtuais dos volumes existentes para aproveitar os benefícios da resiliência de novo.
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>Os espaços de armazenamento diretos foi criado usando o autoconfig:0 comutador e o pool criado manualmente. Ao tentar consultar o pool de espaços de armazenamento diretos para criar um novo volume, recebo uma mensagem que diz "Novamente Enable-ClusterS2D." O que devo fazer?

Por padrão, quando você configurar espaços de armazenamento diretos usando o cmdlet enable-S2D, o cmdlet faz a tudo para você. Ele cria o pool e as camadas. Ao usar autoconfig:0, tudo o que deve ser feito manualmente. Se você criou apenas o pool, a camada não é necessariamente criada. Se você tiver não criar camadas em todos os ou não criar camadas de maneira correspondente para os dispositivos conectados, você receberá uma mensagem de erro "Enable-ClusterS2D novamente". É recomendável que você não use a opção de configuração automática em um ambiente de produção. 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>É possível adicionar um disco de rotação (HDD) ao pool de espaços de armazenamento diretos, depois de criar espaços de armazenamento diretos com dispositivos SSD?

Nenhum. Por padrão, se você usar o tipo de dispositivo único para criar o pool, ele não seria configurar discos de cache e todos os discos seriam usados para a capacidade. Você pode adicionar discos NVME à configuração e discos NVME seriam configurados para o cache.
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>Eu configurei um domínio de falha 2 rack: RACK 1 tem 2 domínios de falha, RACK 2 tem o domínio de falha 1. Cada servidor tem 4 dispositivos de 100 GB de capacidade. Pode usar todos os 1.200 GB de espaço do pool?

Não, você pode usar apenas de 800 GB. Em um domínio de falha do rack, certifique-se de que você tenha uma configuração de 2 vias, portanto, o que cada chuck e seu land duplicado em um rack diferente.
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>O que o tamanho do cache deve ser quando estou configurando espaços de armazenamento diretos?

O cache deve ser dimensionado para acomodar o conjunto de trabalho (os dados que está sendo ativamente lida ou gravada em um determinado momento) de seus aplicativos e cargas de trabalho.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>Como determinar o tamanho do cache que está sendo usado por espaços de armazenamento diretos?

Use o utilitário interno PerfMon para inspecionar os erros de cache. Examine o leituras/s de perda de cache do contador de disco de híbrido do armazenamento de Cluster. Lembre-se de que se o cache não tiver muitas leituras, o cache é subdimensionado e talvez você queira expandi-lo. 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>Há uma calculadora que mostra o tamanho exato dos discos que estão sendo definidas separadamente para cache, a capacidade e a resiliência que permite que me planejar melhor?

Você pode usar a Calculadora de espaços de armazenamento para ajudá-lo com seus planos. Ele está disponível em http://aka.ms/s2dcalc.
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>O que é a melhor configuração que você recomendaria ao configurar e racks de 3 a 6 servidores?

Use 2 servidores em cada um dos racks para obter a resiliência do disco virtual de um espelho de 3 vias. Lembre-se de que a configuração de rack funcionaria corretamente somente se você estiver fornecendo a configuração para o sistema operacional da maneira que ele é colocado no rack. 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>Pode habilitar o modo de manutenção de um disco específico em um servidor específico no cluster espaços de armazenamento diretos?

Sim, você pode habilitar o modo de manutenção do armazenamento em um disco específico e um domínio de falha específico. O comando Enable-StorageMaintenanceMode é invocado automaticamente quando você pausa um nó. Você pode habilitá-lo para um disco específico executando o seguinte comando:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>É espaços de armazenamento diretos com suporte no meu hardware?

É recomendável que você entre em contato com o fornecedor do hardware para verificar o suporte. Fornecedores de hardware testar a solução em seu hardware e um comentário sobre se ele tem suporte ou não. Por exemplo, no momento deste texto, servidores, como R730 / R730xd / R630 que têm mais de 8 slots de unidade pode dar suporte a SES e são compatíveis com espaços de armazenamento diretos. Dell oferece suporte apenas a HBA330 com espaços de armazenamento diretos. R620 não oferece suporte a SES e não é compatível com espaços de armazenamento diretos.

Informações de suporte para mais hardware, vá para o seguinte site: Catálogo do Windows Server
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>Como espaços de armazenamento diretos faz uso de SES?

Espaços de armazenamento diretos usa o mapeamento de serviços de compartimento SCSI (SES) para certificar-se de que discos de dados e os metadados são distribuídos entre os domínios de falha de forma resiliente. Se o hardware não oferece suporte a SES, não há nenhum mapeamento dos compartimentos e o posicionamento de dados não é resiliente.
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>O comando que você pode usar para verificar a extensão física de um disco virtual?
  
Este aqui:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
