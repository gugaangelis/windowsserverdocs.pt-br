---
title: Espaços de armazenamento diretos - perguntas frequentes
description: Saiba como cerca espaços de armazenamento diretos
keywords: Espaços de Armazenamento
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066880"
---
# Espaços de armazenamento diretos - perguntas frequentes (FAQ)

Este artigo lista algumas perguntas mais comuns e perguntas frequentes relacionadas a [Espaços de armazenamento diretos](storage-spaces-direct-overview.md).

## Quando você usa espaços de armazenamento diretos com 3 nós, você pode obter o desempenho e níveis de capacidade?

Sim, você pode obter um desempenho e a camada de capacidade em uma configuração de espaços de armazenamento diretos 2 ou 3 nós. No entanto, você deve certificar-se de que você tenha 2 dispositivos de capacidade. Isso significa que você deve usar todos os três tipos de dispositivos: NVME, SSD e HDD.
 
## Sistema de arquivos do refs fornece tiaring em tempo real com espaços de armazenamento diretos. O REFS fornece a mesma funcionalidade com espaços de armazenamento compartilhado no 2016?

Não, você não terá em tempo real de armazenamento hierárquico com espaços de armazenamento compartilhado com 2016. Isso é para espaços de armazenamento diretos. 
 
## Pode usar um sistema de arquivos NTFS com espaços de armazenamento diretos?
  
Sim, você pode usar o sistema de arquivos NTFS com espaços de armazenamento diretos. No entanto, é recomendável REFS. NTFS não fornece hierarquia em tempo real. 
 
## Posso ter configurado espaços de armazenamento diretos clusters de 2 nós, onde o disco virtual está configurado como resiliência de 2 vias. Se eu adicionar um novo domínio de falha, a resiliência do disco virtual existente será alterado?

Depois de adicionar o novo domínio de falha, os novos discos virtuais que você cria pulará para 3 vias. No entanto, o disco virtual existente permanecerá um disco espelhado 2 vias. Você pode copiar os dados para os novos discos virtuais dos volumes existentes para obter os benefícios da resiliência de novo.
 
## Os espaços de armazenamento diretos foi criado usando o switch autoconfig:0 e o pool criado manualmente. Ao tentar consultar o pool de espaços de armazenamento diretos para criar um novo volume, recebo uma mensagem que diz "Novamente Enable-ClusterS2D." O que devo fazer?

Por padrão, quando você configura a espaços de armazenamento diretos usando o cmdlet enable-S2D, o cmdlet faz a tudo para você. Ele cria o pool e as camadas. Ao usar autoconfig:0, tudo o que deve ser feito manualmente. Se você criou somente o pool, a faixa não é necessariamente criada. Você receberá uma mensagem de erro "Novamente Enable-ClusterS2D" Se você tiver não criado em todos os níveis ou não criado camadas de maneira correspondente para os dispositivos conectados. É recomendável que você não use a opção de configuração automática em um ambiente de produção. 
 
## É possível adicionar um disco giratório (HDD) ao pool de espaços de armazenamento diretos depois que você criou espaços de armazenamento diretos com dispositivos SSD?

Não. Por padrão, se você usar o tipo de dispositivo único para criar o pool, ele não seria configurar discos de cache e todos os discos seriam usados para capacidade. Você pode adicionar discos NVME para a configuração e discos NVME seriam configurados para o cache.
 
## Eu tiver configurado um domínio de falha de rack de 2: 1 de RACK tem 2 domínios de falha, 2 RACK tem 1 domínio de falha. Cada servidor tem 4 dispositivos de 100 GB de capacidade. Pode usar todos os 1.200 GB de espaço do pool?

Não, você pode usar apenas 800 GB. Em um domínio de falhas de rack, você deve garantir que você tenha uma configuração de 2 vias para que cada chuck e seu land duplicado em um rack diferente.
 
## O que o tamanho do cache deve ser quando estou configurando espaços de armazenamento diretos?

O cache deve ser dimensionado para acomodar o conjunto de trabalho (os dados que está sendo ativamente lidos ou gravados a qualquer momento) dos seus aplicativos e cargas de trabalho.

## Como posso determinar o tamanho do cache que está sendo usado por espaços de armazenamento diretos?

Use o utilitário interno PerfMon inspecionar as perdas no cache. Examine o cache miss leituras/s do contador de Cluster Storage Hybrid Disk. Lembre-se de que, se muitas são perdas o cache, o cache é subdimensionado e você pode querer para expandi-la. 
 
## Há uma calculadora que mostra o tamanho exato dos discos que estão sendo reservada cache, capacidade e resiliência que permitiria que me planejar melhor?

Você pode usar a Calculadora de espaços de armazenamento para ajudar seu planejamento. Ele está disponível em http://aka.ms/s2dcalc.
 
## Qual é a melhor configuração que você faria recomendam ao configurar servidores de 6 e 3 racks?

Use 2 servidores em cada um dos racks para obter a resiliência de disco virtual de um espelho de 3 vias. Lembre-se de que a configuração de rack funcionaria corretamente somente se você estiver fornecendo a configuração para o sistema operacional da maneira que ele é colocado no rack. 
 
## Posso habilitar o modo de manutenção de um disco específico em um servidor específico no cluster de espaços de armazenamento diretos?

Sim, você pode habilitar o modo de manutenção de armazenamento em um disco específico e um domínio de falha específicos. O comando Enable-StorageMaintenanceMode é invocado automaticamente quando você pausa um nó. Você pode ativá-lo para um disco específico, executando o seguinte comando:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## É espaços de armazenamento diretos compatível com o hardware?

É recomendável que você entre em contato com seu fornecedor de hardware para verificar o suporte. Fornecedores de hardware testar a solução em seu hardware e o comentário sobre ele tem suporte ou não. Por exemplo, no momento dessa gravação, servidores, como R730 / R730xd / R630 que têm mais de 8 slots de unidade pode dar suporte a SES e são compatíveis com espaços de armazenamento diretos. Dell oferece suporte apenas a HBA330 com espaços de armazenamento diretos. R620 não dá suporte a SES e não é compatível com espaços de armazenamento diretos.

Para hardware mais informações de suporte, vá para o site a seguir: Windows Server Catalog
 
## Como espaços de armazenamento diretos faz uso de SES?

Espaços de armazenamento diretos usa o mapeamento de SCSI Enclosure Services (SES) para certificar-se de que os discos de dados e os metadados é distribuído nos domínios de falha de maneira resiliente. Se o hardware não oferece suporte a SES, não há nenhum mapeamento dos compartimentos e o posicionamento de dados não é resiliente.
 
## O comando que você pode usar para verificar a extensão física para um disco virtual?
  
Este one:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
