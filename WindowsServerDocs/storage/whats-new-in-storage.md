---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Novidades no armazenamento no Windows Server
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: kumudd
ms.date: 09/15/2016
ms.openlocfilehash: 9aab6246f7ddc86629834bf20a7d21cc4ce2ec8f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-storage-in-windows-server-2016"></a>Novidades no armazenamento no Windows Server 2016

>Aplica-se a: Windows Server 2016

Este tópico explica as funcionalidades novas e alteradas no Armazenamento no Windows Server 2016.

## <a name="s2d"></a>Espaços de Armazenamento Diretos  
Os Espaços de Armazenamento Direto habilitam a criação de armazenamento altamente disponível e escalonável usando servidores com armazenamento local. Ele simplifica a implantação e o gerenciamento de sistemas de armazenamento definidos por software e desbloqueia o uso de novas classes de dispositivos de disco, como dispositivos de disco SATA SSD e NVMe, que anteriormente não eram possíveis com Espaços de Armazenamento clusterizados com discos compartilhados.  

**Qual é o valor agregado desta alteração?**  
Os Espaços de Armazenamento Direto habilitam provedores de serviços e empresas a usar servidores padrão do setor com armazenamento local para criar armazenamento altamente disponível e escalonável definido por software. O uso de servidores com armazenamento local diminui a complexidade, aumenta a escalabilidade e habilita o uso de dispositivos de armazenamento que não eram possíveis antes, como discos de estado sólido SATA para reduzir os custos de armazenamento flash ou discos de estado sólido NVMe para melhorar o desempenho.  

Os Espaços de Armazenamento Direto eliminam a necessidade de malha SAS compartilhada, simplificando a implantação e a configuração. Em vez disso, eles usam a rede como uma malha de armazenamento, aproveitando SMB3 e SMB Direct (RDMA) para o armazenamento eficiente de alta velocidade e baixa latência da CPU. Para escalar horizontalmente, basta adicionar servidores para aumentar a capacidade de armazenamento e o desempenho de E/S  
Para saber mais, consulte [Novidades nos Espaços de Armazenamento Direto no Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md).  

**O que passou a funcionar de maneira diferente?**  
Esse recurso é novo no Windows Server 2016.  

## <a name="storage-replica"></a>Réplica de Armazenamento  
A Réplica de Armazenamento habilita a replicação síncrona em nível de bloco independente de armazenamento entre servidores ou clusters para recuperação de desastre, bem como a expansão de um cluster de failover entre sites. A replicação síncrona habilita o espelhamento de dados em locais físicos com volumes consistentes com falha para garantir perda zero de dados no nível do sistema de arquivos. A replicação assíncrona permite a extensão de site além das dimensões metropolitanas com a possibilidade de perda de dados.  

**Qual é o valor agregado desta alteração?**  
A Replicação de Armazenamento o habilita a fazer o seguinte:  

* Fornecer uma solução de recuperação de desastre de um único fornecedor para interrupções planejadas e não planejadas de cargas de trabalho críticas.
* Usar o transporte SMB3 com desempenho, escalabilidade e confiabilidade comprovados.
* Alongar clusters de failover do Windows para distâncias metropolitanas.
* Usar o software da Microsoft de ponta a ponta para armazenamento e clustering, como Hyper-V, Réplica de Armazenamento, Espaços de Armazenamento, Cluster, Servidor de Arquivos de Escalabilidade Horizontal, SMB3, Eliminação de Duplicação e ReFS/NTFS.
* Ajudar a reduzir o custo e a complexidade da seguinte maneira: 
    * É independente de hardware, sem a necessidade de uma configuração de armazenamento específica, como DAS ou SAN.
    * Permite armazenamento de mercadorias e tecnologias de rede.
    * Conta com facilidade de gerenciamento gráfico para nós individuais e clusters por meio do Gerenciador de Cluster de Failover.
    * Inclui opções de script abrangentes em grande escala por meio do Windows PowerShell. 
* Ajudar a reduzir o tempo de inatividade e aumentar a confiabilidade e a produtividade intrínsecas ao Windows.  
* Fornecer capacidade de suporte, métricas de desempenho e recursos de diagnóstico.  

Para saber mais, consulte [Novidades na Réplica de Armazenamento no Windows Server 2016](storage-replica/storage-replica-overview.md).  

**O que passou a funcionar de maneira diferente?**  
Esse recurso é novo no Windows Server 2016.  

## <a name="storage-qos"></a>Qualidade de serviço do armazenamento  
Agora você pode usar QoS (qualidade de serviço) de armazenamento para monitorar centralmente e de ponta a ponta o desempenho do armazenamento e criar políticas de gerenciamento usando clusters Hyper-V e CSV no Windows Server 2016.  

**Qual é o valor agregado desta alteração?**  
Agora você pode criar políticas de QoS de armazenamento em um cluster CSV e atribuí-las a um ou mais discos virtuais em máquinas virtuais Hyper-V. O desempenho de armazenamento é reajustado automaticamente para atender às políticas à medida que as cargas de trabalho e de armazenamento flutuam.  

* Cada política pode especificar uma reserva (mínima) e um limite (máximo) a serem aplicados a uma coleção de fluxos de dados, como um disco rígido virtual, uma única máquina virtual ou um grupo de máquinas virtuais, um serviço ou um locatário.  
* Usando o Windows PowerShell ou o WMI, você pode executar as seguintes tarefas:  
    * Crie políticas em um cluster CSV.
    * Enumere as políticas disponíveis em um cluster CSV.
    * Atribua uma política a um disco rígido virtual de uma máquina virtual Hyper-V. 
    * Monitorar o desempenho de cada fluxo e o status na política.  
* Se vários discos rígidos virtuais compartilham a mesma política, o desempenho é distribuído uniformemente para atender à demanda de acordo com as configurações mínimas e máximas da política. Portanto, uma política pode ser usada para gerenciar um disco rígido virtual, uma máquina virtual, várias máquinas virtuais que compõem um serviço ou todas máquinas virtuais pertencentes a um locatário.  

**O que passou a funcionar de maneira diferente?**  
Esse recurso é novo no Windows Server 2016. Não era possível realizar o gerenciamento de reservas mínimas, o monitoramento de fluxos de todos os discos virtuais no cluster por meio de um único comando e o gerenciamento centralizado baseado em política nas versões anteriores do Windows Server.  

Para saber mais, consulte [Qualidade de serviço do armazenamento](storage-qos/storage-qos-overview.md)

## <a name="dedup"></a>Eliminação de duplicação de dados  
| Funcionalidade | Novo ou atualizado | Descrição |
|---------------|----------------|-------------|
| [Suporte a volumes grandes](data-deduplication/whats-new.md#large-volume-support) | Atualizado | Antes do Windows Server 2016, os volumes tinham que ser dimensionados especificamente para a variação esperada e tamanhos de volume acima de 10 TB não eram bons candidatos para eliminação de duplicação. No Windows Server 2016, a Eliminação de Duplicação de Dados dá suporte a tamanhos de volume de **até 64 TB**. |
| [Suporte a arquivos grandes](data-deduplication/whats-new.md#large-file-support) | Atualizado | Antes do Windows Server 2016, arquivos com tamanho próximo a 1 TB não eram bons candidatos para eliminação de duplicação. No Windows Server 2016, arquivos com **até 1 TB** têm suporte completo. |
| [Suporte ao Nano Server](data-deduplication/whats-new.md#nano-server-support) | Novo | A Eliminação de Duplicação de Dados está disponível e tem suporte completo na nova opção de implantação do Nano Server para Windows Server 2016. |
| [Suporte de backup simplificado](data-deduplication/whats-new.md#simple-backup-support) | Novo | No Windows Server 2012 R2, Aplicativos de Backup Virtualizado, como o [Data Protection Manager](https://technet.microsoft.com/en-us/library/hh758173.aspx), tinham suporte por meio de uma série de etapas de configuração manual. No Windows Server 2016, um novo "Backup" de tipo de uso padrão foi adicionado para implantação perfeita da Eliminação de Duplicação de Dados para aplicativos de backup virtualizado. |
| [Suporte a atualizações do sistema operacional de cluster sem interrupção](data-deduplication/whats-new.md#cluster-upgrade-support) | Novo | A Eliminação de Duplicação de Dados dá suporte completo ao novo recurso [Atualização sem Interrupção do SO de Cluster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) do Windows Server 2016. |

## <a name="smb-hardening-improvements"></a>Melhorias em proteção de SMB para conexões SYSVOL e NETLOGON  
No Windows 10 e no Windows Server 2016, conexões de cliente para os compartilhamentos padrão SYSVOL e NETLOGON do Active Directory Domain Services em controladores de domínio agora exigem assinatura SMB e autenticação mútua (como Kerberos).   

**Qual é o valor agregado desta alteração?**  
Essa alteração reduz a probabilidade de ataques intermediários.   

**O que passou a funcionar de maneira diferente?**  
Se a assinatura SMB e a autenticação mútua não estiverem disponíveis, um computador com Windows 10 ou Windows Server 2016 não processará scripts e a Política de Grupo baseada em domínio.  

> [!NOTE]  
> Os valores do Registro para essas configurações não estão presentes por padrão, mas as regras de proteção ainda se aplicam até serem substituídas pela Política de Grupo ou outros valores do Registro.  

Para saber mais sobre esses aprimoramentos de segurança (também conhecidos como Proteção UNC), consulte o artigo da Base de Dados de Conhecimento [3000483](http://support.microsoft.com/kb/3000483) e [MS15-011 & MS15-014: Política de Grupo de Proteção](http://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy).  

## <a name="work-folders"></a>Pastas de trabalho
Melhor notificação de alteração quando o servidor de pastas de trabalho está executando o Windows Server 2016, e o cliente de pastas de trabalho é o Windows 10.

**Qual é o valor agregado desta alteração?**<br>
Para o Windows Server 2012 R2, quando as alterações do arquivo são sincronizadas com o servidor de pastas de trabalho, os clientes não são notificados sobre a alteração e aguardam até 10 minutos para receber a atualização.  Ao usar o Windows Server 2016, o servidor de pastas de trabalho notifica imediatamente os clientes do Windows 10, e as alterações do arquivo são sincronizadas imediatamente.

**O que passou a funcionar de maneira diferente?**<br>
Esse recurso é novo no Windows Server 2016. Isso requer um servidor de pastas de trabalho do Windows Server 2016, e o cliente deve ser o Windows 10.

Se você estiver usando um cliente mais antigo ou o servidor de Pastas de Trabalho for o Windows Server 2012 R2, o cliente continuará sondando alterações a cada 10 minutos.

## <a name="refs"></a>ReFS 
A próxima iteração do ReFS dá suporte a implantações de armazenamento em grande escala com cargas de trabalho diversificadas, o que proporciona confiabilidade, resiliência e escalabilidade para os dados.     

**Qual é o valor agregado desta alteração?**<br>
O ReFS apresenta as seguintes melhorias:

* O ReFS implementa a nova funcionalidade de camadas de armazenamento, ajudando a oferecer um desempenho mais rápido e mais capacidade de armazenamento. Essa nova funcionalidade permite:
    * Vários tipos de resiliência no mesmo disco virtual (usando espelhamento na camada desempenho e paridade na camada de capacidade, por exemplo).
    * Define a maior capacidade de resposta para conjuntos de trabalho erráticos. 
    * Suporte para mídia SMR (Shingled Magnetic Recording). 
* A introdução da clonagem do bloco melhora muito o desempenho de operações de VM, como operações de mesclagem do ponto de verificação .vhdx.
* A nova ferramenta de verificação ReFS permite a recuperação de armazenamento vazado e ajuda na recuperação de dados de danos críticos. 

**O que passou a funcionar de maneira diferente?**<br>
Esses recursos são novos no Windows Server 2016. 

## <a name="see-also"></a>Consulte também  
* [Novidades no Windows Server 2016](../get-started/what-s-new-in-windows-server-2016.md)  
