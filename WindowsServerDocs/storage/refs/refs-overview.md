---
title: Visão geral do ReFS (Sistema de Arquivos Resiliente)
ms.prod: windows-server
ms.author: gawatu
manager: mchad
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 06/17/2019
ms.openlocfilehash: daa766b63cd99b86abb5a9fad791061c21aba766
ms.sourcegitcommit: be4f67ae8e40a0bf1086881ba8963c69d7ea889f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82986022"
---
# <a name="resilient-file-system-refs-overview"></a>Visão geral do ReFS (Sistema de Arquivos Resiliente)

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semestral)

O ReFS (Sistema de Arquivos Resiliente) é o mais novo sistema de arquivos da Microsoft, desenvolvido para maximizar a disponibilidade de dados, dimensionar de forma eficiente conjuntos de dados muito grandes em diversas cargas de trabalho e fornecer integridade de dados por meio de resiliência a danos. Ele procura lidar com um conjunto crescente de cenários de armazenamento e estabelecer uma base para inovações futuras.

## <a name="key-benefits"></a>Principais benefícios

### <a name="resiliency"></a>Resiliência

O ReFS introduz novos recursos que podem detectar precisamente danos e também corrigir esses danos enquanto permanece online, proporcionando maior integridade e a disponibilidade para seus dados: 

- **Fluxos de integridade** - o ReFS usa somas de verificação para metadados e, opcionalmente, para dados de arquivo, o que dá ao ReFS a capacidade de detectar danos com confiança. 
- **Integração com Espaços de Armazenamento** - quando usado em conjunto com um espaço de espelhamento ou paridade, o ReFS pode reparar automaticamente os danos detectados usando a cópia alternativa dos dados fornecidos pelos Espaços de Armazenamento. Os processos de reparo são localizados na área de danos e realizados online, não exigindo tempo de inatividade do volume.
- **Recuperando dados** - se um volume for corrompido e uma cópia alterativa dos dados corrompidos não existir, o ReFS removerá os dados corrompidos do namespace. O ReFS mantém o volume online enquanto resolve a maioria dos danos não corrigíveis, mas há casos raros que exigem que o ReFS deixe o volume offline.
- **Correção de erros proativa** - além de validar os dados antes de leituras e gravações, o ReFS apresenta um analisador de integridade de dados, conhecido como <i>programa de limpeza</i>. Esse programa de limpeza verifica periodicamente o volume, identificando danos latentes e acionando o reparo dos dados corrompidos de forma proativa. 

### <a name="performance"></a>Desempenho

Além de fornecer melhorias em resiliência, o ReFS apresenta novos recursos para cargas de trabalho sensíveis ao desempenho e virtualizadas. Otimização de camada em tempo real, clonagem de blocos e VDL esparso são bons exemplos dos recursos em evolução do ReFS, que foram desenvolvidos para dar suporte a cargas de trabalho dinâmicas e diversificadas:

- **[Paridade com aceleração de espelho](./mirror-accelerated-parity.md)**: a paridade com aceleração de espelho oferece alto desempenho e também armazenamento eficiente de capacidade para seus dados. 

    - Para oferecer alto desempenho e capacidade de armazenamento eficiente, o ReFS divide um volume em dois grupos lógicos de armazenamento, conhecidos como faixas. Essas camadas podem ter seus próprios tipos de unidade e resiliência, permitindo que cada camada se otimize para desempenho ou capacidade. Alguns exemplos de configurações incluem: 
    
      | Nível de desempenho | Camada de capacidade |
      | ---------------- | ----------------- |
      | SSD espelhado | HDD espelhado |
      | SSD espelhado | SSD de paridade |
      | SSD espelhado | HDD de paridade |
            
    - Quando essas camadas estão configuradas, o ReFS as usa para fornecer armazenamento rápido para dados mais acessados e armazenamento de capacidade eficiente para dados menos acessados:
        - Todas as gravações ocorrerão na camada de desempenho, e as grandes quantidades de dados que permanecerão na camada de desempenho serão movidas com eficiência para a camada de capacidade em tempo real.
        - Se você estiver usando uma implantação híbrida (misturar unidades flash e HDD), [o cache no espaços de armazenamento diretos](../storage-spaces/understand-the-cache.md) ajudará a acelerar as leituras, reduzindo o efeito da característica de fragmentação de dados de cargas de trabalho virtualizadas. Caso contrário, se estiver usando uma implantação de todos os flash, as leituras também ocorrerão no nível de desempenho.

> [!NOTE]
> Para implantações de servidor, a paridade com aceleração de espelho é compatível somente com [Espaços de armazenamento diretos](../storage-spaces/storage-spaces-direct-overview.md). É recomendável usar a paridade acelerada por espelhamento somente com cargas de trabalho de arquivamento e backup. Para cargas de trabalho aleatórias de alto desempenho e virtualizadas, recomendamos o uso de espelhos de três vias para melhorar o desempenho.

- **Operações de VM aceleradas** - o ReFS introduz novas funcionalidades criadas especificamente para melhorar o desempenho de cargas de trabalho virtualizadas:
    - [Clonagem de blocos](./block-cloning.md) - a clonagem de blocos acelera as operações de cópia, permitindo operações de mesclagem de ponto de verificação de VM rápidas e de baixo impacto.
    - VDL esparso - o VDL esparso permite que o ReFS zere arquivos rapidamente, reduzindo o tempo necessário para criar VHDs fixos de dezenas de minutos para meros segundos.

- **Tamanhos de cluster variáveis** - o ReFS dá suporte para tamanhos de cluster de 4 K e 64 K. 4 K é o tamanho de cluster recomendado para a maioria das implantações, mas os clusters de 64 K são apropriados para grandes cargas de trabalho de E/S sequenciais.

### <a name="scalability"></a>Escalabilidade

O ReFS foi projetado para dar suporte a conjuntos de dados extremamente grandes, milhões de terabytes, sem afetar negativamente o desempenho, alcançando uma escala maior do que os sistemas de arquivos anteriores.

## <a name="supported-deployments"></a>Implantações com suporte

A Microsoft desenvolveu o NTFS especificamente para uso geral com uma ampla gama de configurações e cargas de trabalho, no entanto, para os clientes que exigem especialmente a disponibilidade, a resiliência e/ou a escala que o ReFS fornece, a Microsoft dá suporte a ReFS para uso nas seguintes configurações e cenários. 

> [!NOTE]
> Todas as configurações com suporte do ReFS devem usar hardware certificado pelo [catálogo do Windows Server](https://www.WindowsServerCatalog.com) e atender aos requisitos do aplicativo.

### <a name="storage-spaces-direct"></a>Espaços de Armazenamento Direct

Implantar o ReFS nos Espaços de Armazenamento Diretos é recomendado para cargas de trabalho individualizadas ou armazenamento junto à rede: 
- A paridade acelerada por espelhamento e [o cache nos Espaços de armazenamento diretos](../storage-spaces/understand-the-cache.md) oferecem armazenamento de alto desempenho e de capacidade eficiente. 
- A introdução do clone de blocos e do VDL esparso acelera drasticamente as operações de arquivos .vhdx, como criação, mesclagem e expansão.
- Fluxos de integridade, reparo online e cópias de dados alternativas permitem que ReFS e Espaços de Armazenamento Diretos em conjunto para detectar e corrigir corrupções de mídia de armazenamento e controlador de armazenamento em metadados e dados. 
- O ReFS fornece a funcionalidade para dimensionar e oferecer suporte a grandes conjuntos de dados. 

### <a name="storage-spaces"></a>Espaços de Armazenamento

- Os fluxos de integridade, reparo online e cópias de dados alternativas permitem que o ReFS e os [espaços de armazenamento](../storage-spaces/overview.md) entrem em conjunto para detectar e corrigir corrupções de mídia de armazenamento e controlador de armazenamento em metadados e dados.
- As implantações dos Espaços de armazenamento também podem utilizar a clonagem de bloco e a escalabilidade oferecida na ReFS.
- Implantar ReFS em espaços de armazenamento com compartimentos SAS compartilhados é adequado para hospedar dados de arquivamento e armazenar documentos de usuário.

> [!NOTE]
> Os espaços de armazenamento dão suporte à conexão direta local não removível via BusTypes SATA, SAS, NVME ou anexada via HBA (também conhecido como controlador RAID no modo de passagem).

### <a name="basic-disks"></a>Discos básicos

A implantação de ReFS em discos básicos é mais adequada para aplicativos que implementam suas próprias soluções de resiliência e disponibilidade de software. 
- Os aplicativos que apresentam suas próprias soluções de software de resiliência e disponibilidade podem aproveitar fluxos de integridade, clonagem de bloco, além da capacidade de dimensionar e oferecer suporte a grandes conjuntos de dados. 

> [!NOTE]
> Os discos básicos incluem conexão direta local não removível via BusTypes SATA, SAS, NVME ou RAID. Os discos básicos não incluem espaços de armazenamento.

### <a name="backup-target"></a>Destino de backup

A implantação de ReFS como um destino de backup é mais adequada para aplicativos e hardware que implementam suas próprias soluções de resiliência e disponibilidade.
- Os aplicativos que apresentam suas próprias soluções de software de resiliência e disponibilidade podem aproveitar fluxos de integridade, clonagem de bloco, além da capacidade de dimensionar e oferecer suporte a grandes conjuntos de dados.

> [!NOTE]
> Os destinos de backup incluem as configurações com suporte acima. Entre em contato com os fornecedores de aplicativos e de matriz de armazenamento para obter detalhes de suporte em Fibre Channel e SANs iSCSI. Para SANs, se recursos como provisionamento dinâmico, corte/desmapeamento ou Transferência de Dados descarregado (ODX) forem necessários, o NTFS deverá ser usado.   

## <a name="feature-comparison"></a>Comparação de recursos

### <a name="limits"></a>limites

| Recurso       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Tamanho máximo do nome do arquivo | 255 caracteres Unicode  | 255 caracteres Unicode               |
| Tamanho máximo do nome do caminho |32 mil caracteres Unicode | 32 mil caracteres Unicode                |
| Tamanho máximo do arquivo | 35 PB (petabytes)  | 256 TB               |
| Tamanho máximo do volume | 35 PB                           | 256 TB                |

### <a name="functionality"></a>Funcionalidade

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>Os seguintes recursos estão disponíveis em ReFS e NTFS:

| Funcionalidade       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Criptografia de BitLocker | Sim | Sim |
| Eliminação de duplicação de dados | Sim<sup>1</sup> | Sim |
| Suporte para CSV (Volume Compartilhado Clusterizado) | Sim<sup>2</sup> | Sim |
| Links virtuais | Sim | Sim |
| Suporte para cluster de failover | Sim | Sim |
| Listas de controle de acesso | Sim | Sim |
| Diário USN | Sim | Sim |
| Notificações de alterações | Sim | Sim |
| Pontos de junção | Sim | Sim |
| Pontos de montagem | Sim | Sim |
| Pontos de nova análise | Sim | Sim |
| Instantâneos de volume | Sim | Sim |
| IDs de Arquivo | Sim | Sim |
| Oplocks | Sim | Sim |
| Arquivos esparsos | Sim | Sim |
| Fluxos nomeados | Sim | Sim |
| Provisionamento dinâmico | Sim<sup>3</sup> | Sim |
| Aparar/cancelar mapeamento | Sim<sup>3</sup> | Sim |
1. Disponível no Windows Server, versão 1709 e posterior.
2. Disponível no Windows Server 2012 R2 e posterior.
3. Somente espaços de armazenamento

#### <a name="the-following-features-are-only-available-on-refs"></a>Os seguintes recursos só estão disponíveis em ReFS:

| Funcionalidade       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clone de blocos | Sim | Não |
| VDL Esparso | Sim | Não |
| Paridade acelerada por espelho| Sim (em Espaços de Armazenamento Diretos) | Não |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>Os seguintes recursos não estão disponíveis em ReFS no momento:

| Funcionalidade       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compactação de sistema de arquivos | Não | Sim |
| Criptografia de sistema de arquivos | Não | Sim |
| Transactions | Não | Sim |
| Links físicos | Não | Sim |
| Identificadores de objeto | Não | Sim |
| Transferência de Dados descarregadas (ODX) | Não | Sim |
| Nomes curtos | Não | Sim |
| Atributos estendidos | Não | Sim |
| Cotas de disco | Não | Sim |
| Inicializável | Não | Sim |
| Suporte ao arquivo de paginação | Não | Sim |
| Com suporte em mídia removível | Não | Sim |

## <a name="see-also"></a>Confira também

- [Recomendações de tamanho de cluster para ReFS e NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Visão geral de Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md)
- [Clonagem de blocos ReFS](block-cloning.md)
- [Fluxos de integridade ReFS](integrity-streams.md)
