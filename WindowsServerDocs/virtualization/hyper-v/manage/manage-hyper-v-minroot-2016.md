---
title: Minroot
description: Configurando controles de recursos de CPU do Host
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: e1269c11df32c8ce95cc7455d47d7170e9d0b1c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844327"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Gerenciamento de recursos de CPU do Host do Hyper-V

Controles de recursos de CPU do host Hyper-V introduzido no Windows Server 2016 ou posterior permitem que os administradores do Hyper-V gerenciar melhor e alocar recursos de CPU entre a "raiz", ou partição de gerenciamento e as VMs convidadas de servidor de host. Usando esses controles, os administradores podem dedicar um subconjunto dos processadores de um sistema de host para a partição raiz. Isso pode separar o trabalho realizado em um host Hyper-V das cargas de trabalho em execução em máquinas virtuais convidadas, executá-los em separado subconjuntos dos processadores do sistema.

Para obter detalhes sobre o hardware para hosts Hyper-V, consulte [requisitos de sistema do Windows 10 Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Histórico

Antes de recursos de CPU de host de controles de configuração do Hyper-V, é útil para examinar os conceitos básicos da arquitetura do Hyper-V.  
Você pode encontrar um resumo geral no console do [arquitetura do Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) seção.
Estes são os conceitos importantes para este artigo:

* Hyper-V cria e gerencia as partições de máquina virtual, em qual computação recursos são alocados e compartilhados, sob o controle do hipervisor.  Partições fornecem limites de isolamento forte entre todas as máquinas virtuais convidadas e entre VMs convidadas e na partição raiz.

* A partição de raiz em si é uma partição de máquina virtual, embora ele tenha privilégios muito maiores do que as máquinas virtuais de convidado e propriedades exclusivas.  A partição raiz fornece os serviços de gerenciamento que controlam todas as máquinas virtuais de convidado, fornece suporte a dispositivos virtuais para convidados e gerencia todos os dispositivos e/s para máquinas virtuais convidadas.  Microsoft recomenda não executando qualquer carga de trabalho do aplicativo em uma partição de host.

* Cada processador virtual (VP) da partição raiz é mapeada 1:1 para um processador lógico subjacente (LP).  Um host VP sempre será executado sobre o LP subjacente mesmo – não há nenhuma migração dos vice-presidentes da partição raiz.  

* Por padrão, os LPs no qual executar vice-presidentes host também podem executar vice-presidentes de convidado.

* Uma VICE-PRESIDENTE de convidado pode ser agendado pelo hipervisor para ser executado em qualquer processador lógico disponível.  Enquanto o Agendador de hipervisor se encarrega de considerar a localidade do cache temporal, topologia NUMA e muitos outros fatores ao agendar uma VP de convidado, por fim, o vice-Presidente pudessem ser agendada em qualquer host LP.

## <a name="the-minimum-root-or-minroot-configuration"></a>A raiz mínima ou configuração de "Minroot"

As versões anteriores do Hyper-V tinham um limite máximo da arquitetura dos 64 vice-presidentes por partição.  Isso seja aplicado às partições raiz e convidado.  Como apareceram sistemas com mais de 64 processadores lógicos em servidores high-end, o Hyper-V também evoluiu seus limites de escala do host para dar suporte a esses sistemas maiores, em um ponto que dão suporte a um host com até 320 LPs.  No entanto, quebrando a 64 VP de limite de partição no momento por apresentados vários desafios e introduziu complexidades que fez a dar suporte a mais de 64 vice-presidentes por partição proibitiva.  Para resolver isso, o Hyper-V limita o número de vice-presidentes fornecido para a partição raiz 64, mesmo se a máquina subjacente tinha o número de processadores lógico mais disponível.  O hipervisor, continuará a utilizar todos os LPs disponíveis para execução pelo VP de convidado, mas limitado artificialmente a partição raiz a 64.  Essa configuração se tornou conhecida como "mínimo" raiz"ou"minroot"configuration.  Teste de desempenho confirmado que, até mesmo em sistemas de grande escala com mais de 64 LPs, a raiz não precisam mais de 64 vice-presidentes raiz para fornecer suporte suficiente para um grande número de VMs convidadas e vice-presidentes convidado – na verdade, muito menos de 64 raiz vice-presidentes geralmente era adequado , dependendo do curso o número e tamanho das VMs convidadas, as cargas de trabalho específicas que estão sendo executadas, etc.

Esse conceito de "minroot" continua a ser utilizado hoje em dia.  Na verdade, mesmo que o Windows Server 2016 Hyper-V aumentado seu limite máximo de suporte de arquitetura para o host LPs 512 LPs, a partição raiz ainda será limitada a um máximo de 320 LPs.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>Usando Minroot para restringir e isolar recursos de computação do Host
Com o limite de alto padrão de 320 LPs no Windows Server 2016 Hyper-V, a configuração de minroot só será utilizada nos sistemas de servidor muito maiores.  No entanto, essa funcionalidade pode ser configurada com um limite muito inferior pelo administrador do host do Hyper-V e, portanto, é utilizada para restringir significativamente a quantidade de recursos de CPU do host disponíveis na partição raiz.  O número específico de raiz LPs utilizar obviamente deve ser escolhido cuidadosamente para atender às demandas máximo de VMs e as cargas de trabalho alocadas para o host.  No entanto, valores razoáveis para o número de host LPs podem ser determinado por meio da avaliação cuidadosa e monitoramento de cargas de trabalho de produção e validado em ambientes de não produção antes da implantação ampla.

## <a name="enabling-and-configuring-minroot"></a>Habilitando e configurando o Minroot

A configuração minroot é controlada por meio de entradas de BCD do hipervisor. Para habilitar minroot, em um prompt de comando com privilégios de administrador:

```
    bcdedit /set hypervisorrootproc n
```
Onde n é o número de raiz vice-presidentes. 

O sistema deve ser reinicializado, e o novo número de processadores de raiz será mantido pelo tempo de vida da inicialização do sistema operacional.  A configuração minroot não pode ser alterada dinamicamente em tempo de execução.

Se houver vários nós NUMA, cada nó receberá `n/NumaNodeCount` processadores.

Observe que com vários nós NUMA, você deve garantir que topologia da VM é o modo que haja suficiente LPs livres (ou seja, LPs sem raiz vice-presidentes) em cada nó para executar o nó da VM correspondente vice-presidentes.

## <a name="verifying-the-minroot-configuration"></a>Verificando a configuração de Minroot

Você pode verificar a configuração do host minroot usando o Gerenciador de tarefas, conforme mostrado abaixo.

![](./media/minroot-taskman.png)

Quando Minroot está ativa, o Gerenciador de tarefas exibirá o número de processadores lógicos atualmente alocado para o host, além do número total de processadores lógicos no sistema.
 
