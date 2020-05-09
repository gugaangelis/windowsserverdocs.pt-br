---
title: Espaços de Armazenamento Diretos-perguntas frequentes
description: Saiba mais sobre Espaços de Armazenamento Diretos
ms.prod: windows-server
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5c033a5a810d1cdedeb4c733ba4bf0ac99e669f0
ms.sourcegitcommit: 3483f886f331b9d954a0e5dba8e910dbe5ee5765
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82977244"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Espaços de Armazenamento Diretos-perguntas frequentes (FAQ)

Este artigo lista algumas perguntas comuns e frequentes relacionadas a [espaços de armazenamento diretos](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>Quando você usa Espaços de Armazenamento Diretos com 3 nós, pode obter as camadas de desempenho e capacidade?

Sim, você pode obter uma camada de desempenho e capacidade em uma configuração de Espaços de Armazenamento Diretos de dois nós ou de três nós. No entanto, você deve verificar se tem 2 dispositivos de capacidade. Isso significa que você deve usar todos os três tipos de dispositivos: NVME, SSD e HDD.

## <a name="refs-file-system-provides-real-time-tiering-with-storage-spaces-direct-does-refs-provide-the-same-functionality-with-shared-storage-spaces-in-2016"></a>O sistema de arquivos refs fornece camadas em tempo real com Espaços de Armazenamento Diretos. O REFS fornece a mesma funcionalidade com espaços de armazenamento compartilhados em 2016?

Não, você não obterá camadas em tempo real com espaços de armazenamento compartilhados com 2016. Isso é apenas para Espaços de Armazenamento Diretos.

## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>Posso usar um sistema de arquivos NTFS com Espaços de Armazenamento Diretos?

Sim, você pode usar o sistema de arquivos NTFS com Espaços de Armazenamento Diretos. No entanto, o REFS é recomendado. O NTFS não fornece camadas em tempo real.

## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>Configurei dois clusters de Espaços de Armazenamento Diretos de nó, em que o disco virtual está configurado como resiliência de espelho de duas vias. Se eu adicionar um novo domínio de falha, a resiliência do disco virtual existente será alterada?

Depois de adicionar o novo domínio de falha, os novos discos virtuais que você criar vão para um espelho de 3 vias. No entanto, o disco virtual existente permanecerá em um disco espelhado de 2 vias. Você pode copiar os dados para os novos discos virtuais dos volumes existentes para obter os benefícios da nova resiliência.

## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-was-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-saying-enable-clusters2d-again-what-should-i-do"></a>O Espaços de Armazenamento Diretos foi criado usando a configuração automática: 0 a opção e o pool foram criados manualmente. Quando tento consultar o pool de Espaços de Armazenamento Diretos para criar um novo volume, recebo uma mensagem dizendo: "Enable-ClusterS2D novamente." O que devo fazer?

Por padrão, quando você configura Espaços de Armazenamento Diretos usando o cmdlet Enable-S2D, o cmdlet faz tudo para você. Ele cria o pool e as camadas. Ao usar a configuração automática: 0, tudo deve ser feito manualmente. Se você criou apenas o pool, a camada não será necessariamente criada. Você receberá uma mensagem de erro "Enable-ClusterS2D novamente" se não tiver criado camadas nem camadas criadas de forma correspondente aos dispositivos anexados. Recomendamos que você não use a opção AutoConfig em um ambiente de produção.

## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>É possível adicionar um disco de rotação (HDD) ao pool de Espaços de Armazenamento Diretos depois de você ter criado Espaços de Armazenamento Diretos com dispositivos SSD?

Não. Por padrão, se você usar o tipo de dispositivo único para criar o pool, ele não configurará discos de cache e todos os discos seriam usados para a capacidade. Você pode adicionar discos de NVME à configuração, e discos de NVME seriam configurados para cache.

## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>Configurei um domínio de falha de 2 rack: o RACK 1 tem 2 domínios de falha, o RACK 2 tem um domínio de falha. Cada servidor tem 4 dispositivos de 100 GB de capacidade. Posso usar todos os 1.200 GB de espaço do pool?

Não, você pode usar apenas 800 GB. Em um domínio de falha de rack, você deve verificar se tem uma configuração de espelho de duas vias para que cada Chuck e seu terreno duplicado em um rack diferente.

## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>O que deve ser o tamanho do cache quando estou Configurando Espaços de Armazenamento Diretos?

O cache deve ser dimensionado para acomodar o conjunto de trabalho (os dados que estão sendo lidos ativamente ou gravados em um determinado momento) de seus aplicativos e cargas de trabalhos.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>Como posso determinar o tamanho do cache que está sendo usado pelo Espaços de Armazenamento Diretos?

Use o utilitário interno PerfMon para inspecionar os erros de cache. Examine as leituras de erros de cache/s do contador de disco híbrido de armazenamento de cluster. Lembre-se de que, se houver muitas leituras ausentes no cache, o cache será subdimensionado e você poderá desejar expandi-lo.

## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>Há uma calculadora que mostra o tamanho exato dos discos que estão sendo separados para o cache, a capacidade e a resiliência que me permitirão planejar melhor?

Você pode usar a calculadora de espaços de armazenamento para ajudar com seu planejamento. Ele está disponível em https://aka.ms/s2dcalc.

## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>Qual é a melhor configuração que você recomendaria ao configurar 6 servidores e três racks?

Use dois servidores em cada um dos racks para obter a resiliência de disco virtual de um espelho de 3 vias. Lembre-se de que a configuração de rack funcionará corretamente apenas se você estiver fornecendo a configuração para o sistema operacional da maneira como ele é colocado no rack.

## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>Posso habilitar o modo de manutenção para um disco específico em um servidor específico em Espaços de Armazenamento Diretos cluster?

Sim, você pode habilitar o modo de manutenção de armazenamento em um disco específico e em um domínio de falha específico. O comando Enable-StorageMaintenanceMode é invocado automaticamente quando você pausa um nó. Você pode habilitá-lo para um disco específico executando o seguinte comando:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>Há suporte para Espaços de Armazenamento Diretos no meu hardware?

Recomendamos que você entre em contato com seu fornecedor de hardware para verificar o suporte. Os fornecedores de hardware testam a solução em seu hardware e comentam se há suporte ou não. Por exemplo, no momento da redação deste artigo, servidores como R730/R730xd/R630 com mais de 8 slots de unidade podem dar suporte a SES e são compatíveis com Espaços de Armazenamento Diretos. A Dell dá suporte apenas ao HBA330 com Espaços de Armazenamento Diretos. R620 não dá suporte a SES e não é compatível com Espaços de Armazenamento Diretos.

Para obter mais informações de suporte de hardware, acesse o seguinte site: catálogo do Windows Server

## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>Como Espaços de Armazenamento Diretos fazer uso do SES?

O Espaços de Armazenamento Diretos usa o mapeamento de SES (serviços de compartimento SCSI) para garantir que Slabs de dados e os metadados sejam distribuídos entre os domínios de falha de maneira resiliente. Se o hardware não oferecer suporte a SES, não haverá mapeamento dos compartimentos e o posicionamento dos dados não será resiliente.

## <a name="which-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>Qual comando você pode usar para verificar a extensão física de um disco virtual?

Este:

```powershell
get-virtualdisk -friendlyname "xyz" | get-physicalextent
```
