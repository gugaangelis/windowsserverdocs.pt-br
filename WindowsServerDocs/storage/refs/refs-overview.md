---
title: "Visão geral do ReFS (Sistema de Arquivos Resiliente)"
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: dmoss
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 1/3/2016
ms.assetid: 
ms.openlocfilehash: 4460df978f2a3d5e30e43b82cf952ed37f3d91a9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="resilient-file-system-refs-overview"></a>Visão geral do ReFS (Sistema de Arquivos Resiliente)
>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
    
    
    | Camada de desempenho | Camada de capacidade |
    |----------------|-----------------|
     SSD espelhado | HDD espelhado |
     SSD espelhado | SSD de paridade |
     SSD espelhado | HDD de paridade |   
            
    - Quando essas camadas estão configuradas, o ReFS as usa para fornecer armazenamento rápido para dados mais acessados e armazenamento de capacidade eficiente para dados menos acessados:
        - Todas as gravações ocorrerão na camada de desempenho, e as grandes quantidades de dados que permanecerão na camada de desempenho serão movidas com eficiência para a camada de capacidade em tempo real.
        - Se estiver usando uma [implantação híbrida](../storage-spaces/choosing-drives.md) (misturando unidades flash e HDD), [o cache nos Espaços de Armazenamento Diretos](../storage-spaces/understand-the-cache.md) ajudará a acelerar as leituras, reduzindo o efeito da fragmentação dados característica de cargas de trabalho virtualizadas. Caso contrário, se estiver usando uma implantação totalmente flash, as leituras também ocorrerão na camada de desempenho. 

>[!NOTE]
>Para implantações de servidor, a paridade com aceleração de espelho é compatível somente com [Espaços de armazenamento diretos](../storage-spaces/storage-spaces-direct-overview.md).

- **Operações de VM aceleradas** - o ReFS introduz novas funcionalidades criadas especificamente para melhorar o desempenho de cargas de trabalho virtualizadas:
    - [Clonagem de blocos](./block-cloning.md) - a clonagem de blocos acelera as operações de cópia, permitindo operações de mesclagem de ponto de verificação de VM rápidas e de baixo impacto. 
    - VDL esparso - o VDL esparso permite que o ReFS zere arquivos rapidamente, reduzindo o tempo necessário para criar VHDs fixos de dezenas de minutos para meros segundos.


- **Tamanhos de cluster variáveis** - o ReFS dá suporte para tamanhos de cluster de 4 K e 64 K. 4 K é o tamanho de cluster recomendado para a maioria das implantações, mas os clusters de 64 K são apropriados para grandes cargas de trabalho de E/S sequenciais.
    
    
### **<a name="scalability"></a>Escalabilidade**
O ReFS foi projetado para dar suporte a conjuntos de dados extremamente grandes, milhões de terabytes, sem afetar negativamente o desempenho, alcançando uma escala maior do que os sistemas de arquivos anteriores. 

## <a name="supported-deployments"></a>Implantações com suporte

### <a name="storage-spaces-direct"></a>Espaços de Armazenamento Diretos ###

Implantar o ReFS nos Espaços de Armazenamento Diretos é recomendado para cargas de trabalho individualizadas ou armazenamento junto à rede: 
- A paridade acelerada por espelhamento e [o cache nos Espaços de armazenamento diretos](../storage-spaces/understand-the-cache.md) oferecem armazenamento de alto desempenho e de capacidade eficiente. 
- A introdução do clone de blocos e do VDL esparso acelera drasticamente as operações de arquivos .vhdx, como criação, mesclagem e expansão.
- Fluxos de integridade, reparo online e cópias alternativas de dados permitem que o ReFS e os Espaços de Armazenamento Diretos detectem e corrijam metadados e dados corrompidos em conjunto. 
- O ReFS fornece a funcionalidade para dimensionar e oferecer suporte a grandes conjuntos de dados. 

### <a name="storage-spaces-with-sas-drive-enclosures"></a>Espaços de Armazenamento com compartimentos de unidades SAS ###
Implantar o ReFS nos Espaços de Armazenamento com compartimentos SAS compartilhados é adequado para hospedar dados de arquivamento e armazenar documentos dos usuários:
- Fluxos de integridade, reparo online e cópias alternativas de dados permitem que o ReFS e os Espaços de Armazenamento detectem e corrijam metadados e dados corrompidos em conjunto. 
- As implantações dos Espaços de armazenamento também podem utilizar a clonagem de bloco e a escalabilidade oferecida na ReFS.

### <a name="basic-disks"></a>Discos básicos ###
A implantação do ReFS em discos básicos é mais adequada para aplicativos que implementam suas próprias soluções de resiliência e disponibilidade de software. 
- Os aplicativos que apresentam suas próprias soluções de software de resiliência e disponibilidade podem aproveitar fluxos de integridade, clonagem de bloco, além da capacidade de dimensionar e oferecer suporte a grandes conjuntos de dados. 

>[!NOTE]
>O ReFS não é compatível com o armazenamento vinculado a SAN.


## <a name="feature-comparison"></a>Comparação de recursos

### <a name="limits"></a>Limites

| Recurso       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Tamanho máximo do nome do arquivo | 255 caracteres Unicode  | 255 caracteres Unicode               |
| Tamanho máximo do nome do caminho |32 mil caracteres Unicode | 32 mil caracteres Unicode                |
| Tamanho máximo do arquivo | 18 EB (exabytes)  | 18 EB (exabytes)                |
| Tamanho máximo do volume | 4,7 ZB (zettabytes)                           | 256 TB                |


### <a name="functionality"></a>Funcionalidade

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>Os seguintes recursos estão disponíveis em ReFS e NTFS:

| Funcionalidade       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Criptografia BitLocker | Sim | Sim |
| Suporte para CSV (Volume Compartilhado Clusterizado) | Sim | Sim |
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
| Eliminação de Duplicação de Dados | Não | Sim |
| Transações | Não | Sim |
| Links físicos | Não | Sim |
| Identificadores de objeto | Não | Sim |
| Nomes curtos | Não | Sim |
| Atributos estendidos | Não | Sim |
| Cotas de disco | Não | Sim |
| Inicializável | Não | Sim |
| Com suporte em mídia removível | Não | Sim |
| Camadas de armazenamento NTFS | Não | Sim |


## <a name="see-also"></a>Consulte também

-   [Clonagem de blocos ReFS](block-cloning.md)
-   [Fluxos de integridade ReFS](integrity-streams.md)
-   [Visão geral de Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md)
