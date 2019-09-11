---
title: Minroot
description: Configurando controles de recurso de CPU do host
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: 92de899a39aed05e2f598fcb3aae3fbae3f1cb67
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872043"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Gerenciamento de recursos de CPU do host Hyper-V

Os controles de recurso de CPU do host Hyper-V introduzidos no Windows Server 2016 ou posterior permitem que os administradores do Hyper-V gerenciem e aloquem melhor os recursos de CPU do servidor host entre a partição de gerenciamento e a "raiz", bem como as VMs convidadas. Usando esses controles, os administradores podem dedicar um subconjunto dos processadores de um sistema host à partição raiz. Isso pode separar o trabalho feito em um host Hyper-V das cargas de trabalhos em execução em máquinas virtuais convidadas executando-as em subconjuntos separados dos processadores do sistema.

Para obter detalhes sobre o hardware para hosts Hyper-V, consulte [requisitos do sistema do Hyper-v do Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Informações preliminares

Antes de definir controles para recursos de CPU do host Hyper-V, é útil examinar os conceitos básicos da arquitetura do Hyper-V.  
Você pode encontrar um resumo geral na seção [arquitetura do Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) .
Estes são conceitos importantes para este artigo:

* O Hyper-V cria e gerencia partições de máquina virtual, entre as quais os recursos de computação são alocados e compartilhados, sob o controle do hipervisor.  As partições fornecem limites de isolamento fortes entre todas as máquinas virtuais convidadas e entre as VMs convidadas e a partição raiz.

* A partição raiz é, em si, uma partição de máquina virtual, embora tenha propriedades exclusivas e privilégios muito maiores do que as máquinas virtuais convidadas.  A partição raiz fornece os serviços de gerenciamento que controlam todas as máquinas virtuais convidadas, fornece suporte a dispositivos virtuais para convidados e gerencia todas as e/s de dispositivo para máquinas virtuais convidadas.  A Microsoft recomenda enfaticamente não executar cargas de trabalho de aplicativo em uma partição de host.

* Cada processador virtual (VP) da partição raiz é mapeado 1:1 para um processador lógico subjacente (LP).  Um VP de host sempre será executado no mesmo LP subjacente – não há nenhuma migração do VPSs da partição raiz.  

* Por padrão, o LPs em que o host VPSs executado também pode executar o convidado VPSs.

* Um vice-presidente convidado pode ser agendado pelo hipervisor para ser executado em qualquer processador lógico disponível.  Enquanto o Agendador de hipervisor se preocupa em considerar a localidade do cache temporal, a topologia NUMA e muitos outros fatores ao programar um vice-presidente convidado, por fim, o VP pode ser agendado em qualquer host LP.

## <a name="the-minimum-root-or-minroot-configuration"></a>A configuração de raiz mínima ou "Minroot"

As versões anteriores do Hyper-V tinham um limite máximo de arquitetura de 64 VPSs por partição.  Isso se aplica às partições raiz e de convidado.  Como sistemas com mais de 64 processadores lógicos apareciam em servidores high-end, o Hyper-V também evoluiu seus limites de escala de host para dar suporte a esses sistemas maiores, em um ponto dando suporte a um host com até 320 LPs.  No entanto, a interrupção do limite de 64 por partição nesse momento apresentou vários desafios e introduziu complexidades que tornaram o suporte a mais de 64 VPSs por partição, proibitiva.  Para resolver isso, o Hyper-V limitou o número de VPSs dadas à partição raiz para 64, mesmo que a máquina subjacente tivesse muito mais processadores lógicos disponíveis.  O hipervisor continuará a utilizar todos os LPs disponíveis para a execução de VPSs de convidado, mas, artificialmente, a partição raiz em 64.  Essa configuração se tornou conhecida como a configuração "raiz mínima" ou "minroot".  Os testes de desempenho confirmaram que, mesmo em sistemas de grande escala com mais de 64 LPs, a raiz não precisou de mais de 64 VPSs raiz para fornecer suporte suficiente a um grande número de VMs convidadas e VPSs de convidado – na verdade, muito menos que 64 raiz VPSs era geralmente adequado , dependendo do número e do tamanho das VMs convidadas, das cargas de trabalho específicas sendo executadas, etc.

Esse conceito de "minroot" continua sendo utilizado hoje.  Na verdade, mesmo que o Windows Server 2016 Hyper-V tenha aumentado seu limite máximo de suporte arquitetônico para o host LPs a 512 LPs, a partição raiz ainda será limitada a um máximo de 320 LPs.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>Usando o Minroot para restringir e isolar recursos de computação do host
Com o limite padrão alto de 320 LPs no Windows Server 2016 Hyper-V, a configuração do minroot só será utilizada nos maiores sistemas de servidor.  No entanto, esse recurso pode ser configurado para um limite muito menor pelo administrador de host do Hyper-V e, portanto, utilizado para restringir bastante a quantidade de recursos de CPU do host disponíveis para a partição raiz.  É claro que o número específico de LPs raiz a ser utilizado deve ser escolhido com cuidado para dar suporte às demandas máximas das VMs e cargas de trabalho alocadas para o host.  No entanto, valores razoáveis para o número de LPs do host podem ser determinados por meio da avaliação cuidadosa e do monitoramento de cargas de trabalho de produção e validados em ambientes de não produção antes da ampla implantação.

## <a name="enabling-and-configuring-minroot"></a>Habilitando e Configurando o Minroot

A configuração minroot é controlada por meio de entradas BCD do hipervisor. Para habilitar o minroot, em um prompt de comando com privilégios de administrador:

```
    bcdedit /set hypervisorrootproc n
```
Em que n é o número de VPSs raiz. 

O sistema deve ser reinicializado e o novo número de processadores raiz continuará durante o tempo de vida da inicialização do sistema operacional.  A configuração minroot não pode ser alterada dinamicamente no tempo de execução.

Se houver vários nós numa, cada nó `n/NumaNodeCount` receberá processadores.

Observe que, com vários nós NUMA, você deve garantir que a topologia da VM esteja de modo que haja LPs livres suficientes (ou seja, LPs sem o VPSs raiz) em cada nó NUMA para executar o VPSs do nó NUMA da VM correspondente.

## <a name="verifying-the-minroot-configuration"></a>Verificando a configuração do Minroot

Você pode verificar a configuração do minroot do host usando o Gerenciador de tarefas, conforme mostrado abaixo.

![](./media/minroot-taskman.png)

Quando Minroot estiver ativo, o Gerenciador de tarefas exibirá o número de processadores lógicos atualmente alocados para o host, além do número total de processadores lógicos no sistema.
 
