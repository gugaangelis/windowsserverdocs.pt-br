---
title: Visão geral do Azure Stack HCI
description: O Azure Stack HCI é um cluster hiperconvergente do Windows Server 2019 que usa hardware validado para executar cargas de trabalho virtualizadas locais, conectando-se, opcionalmente, aos serviços do Azure para backup baseado em nuvem, recuperação de site e muito mais. As soluções do Azure Stack HCI usam hardware validado pela Microsoft para garantir desempenho e confiabilidade ideais e incluem suporte para tecnologias como unidades NVMe, memória persistente e rede RDMA (acesso remoto direto à memória).
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/22/2019
ms.openlocfilehash: 92d600eeb833cd70bd714702b1fd950c4fe2cd87
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008975"
---
# <a name="azure-stack-hci-overview"></a>Visão geral do Azure Stack HCI

>Aplica-se a: Windows Server 2019

O Azure Stack HCI é um cluster hiperconvergente do Windows Server 2019 que usa hardware validado para executar cargas de trabalho virtualizadas locais, conectando-se, opcionalmente, aos serviços do Azure para backup baseado em nuvem, recuperação de site e muito mais. As soluções do Azure Stack HCI usam hardware validado pela Microsoft para garantir desempenho e confiabilidade ideais e incluem suporte para tecnologias como unidades NVMe, memória persistente e rede RDMA (acesso remoto direto à memória).

O Azure Stack HCI é uma solução que combina vários produtos:

- Hardware de um parceiro OEM

- Windows Server 2019 Datacenter Edition

- Windows Admin Center

- Serviços do Azure (opcional)

![O Azure Stack HCI é uma solução hiperconvergente da Microsoft disponível em uma ampla variedade de parceiros de hardware.](media/AS_HCI_solution.png)

O Azure Stack HCI é uma solução hiperconvergente da Microsoft disponível em uma ampla variedade de parceiros de hardware. Considere os seguintes cenários para que uma solução hiperconvergente ajude-o a determinar se o Azure Stack HCI é a solução que melhor atende às suas necessidades:

- **Atualizar hardware com vencimento.** Substitua a infraestrutura de armazenamento e servidores mais antigos e execute máquinas virtuais do Windows e do Linux locais e na borda com habilidades e ferramentas de TI existentes.

- **Consolidar cargas de trabalho virtualizadas.** Consolide aplicativos herdados em uma infraestrutura eficiente e hiperconvergente. Explore os mesmos tipos de eficiências de nuvem usadas para executar datacenters de hiperescala, como o Microsoft Azure.

- **Conectar-se ao Azure para serviços de nuvem híbrida.** Simplifique o acesso aos serviços de gerenciamento e de segurança de nuvem no Azure, incluindo backup externo e recuperação de site, monitoramento baseado em nuvem e muito mais.

## <a name="the-azure-stack-family"></a>A família Azure Stack

O Azure Stack HCI faz parte do Azure e da família Azure Stack, que usa a mesma computação definida pelo software e o mesmo armazenamento e software de rede que o Azure Stack. Veja um resumo das diferentes soluções:

- [Azure](https://azure.microsoft.com) – usar serviços de nuvem pública
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) – operar serviços de nuvem localmente
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) – executar aplicativos virtualizados localmente, com conexões opcionais com o Azure

![O Azure e o Azure Stack executam serviços de nuvem, enquanto o Azure Stack HCI executa aplicativos virtualizados localmente](media/azure_family.png)

|Azure: usar serviços de nuvem pública|Azure Stack: operar serviços de nuvem localmente|Azure Stack HCI: executar aplicativos virtualizados localmente|
|-----------------|-----------------|-----------------|
|Para que os recursos de computação de autoatendimento sob demanda migrem e modernizem aplicativos existentes e criem aplicativos nativos da nuvem.|Crie e execute aplicativos de nuvem na borda, quando desconectado ou para atender requisitos regulatórios, usando serviços do Azure locais consistentes.| Execute aplicativos virtualizados locais, substitua e consolide infraestrutura de servidor de vencimento e conecte-se ao Azure para serviços de nuvem.|
|Mais de 100 serviços disponíveis em 54 regiões em todo o mundo|VMs do Azure para Windows e Linux, Aplicativos Web do Azure e Azure Functions, Azure Key Vault, Azure Resource Manager, Azure Marketplace, Contêineres, Azure IoT e Hubs de Eventos, Ferramentas de administrador (planos, ofertas, RBAC)|Soluções HCI validadas da plataforma Hyper V e Espaços de Armazenamento Diretos com o Windows Server 2019 e Windows Admin Center para gerenciamento e acesso integrado aos serviços do Azure.|

Para saber mais:

- saiba mais em nosso site de soluções do [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci).
- Assista aos especialistas da Microsoft Jeff Woolsey e Vijay Tewari [abordarem as novas soluções do Azure Stack HCI](https://aka.ms/AzureStackOverviewVideo).

## <a name="hyperconverged-efficiencies"></a>Eficiências hiperconvergentes

As soluções do Azure Stack HCI reúnem computação altamente virtualizada, armazenamento e rede em servidores e componentes x86 padrão do setor. Combinar recursos no mesmo cluster facilita a implantação, o gerenciamento e o ajuste da escala. Gerencie com sua opção de automação de linha de comando ou com o Windows Admin Center.

Obtenha desempenho de máquina virtual líder no setor para seus aplicativos de servidor com o Hyper-V, a tecnologia básica de hipervisor da nuvem da Microsoft e a tecnologia Espaços de Armazenamento Diretos com suporte interno para NVMe, memória persistente e rede RDMA (acesso remoto direto à memória).

Ajude a manter aplicativos e dados protegidos com máquinas virtuais blindadas, microssegmentação de rede e criptografia nativa de dados em repouso e em trânsito.

## <a name="hybrid-capabilities"></a>Funcionalidades híbridas

Aproveite o funcionamento em conjunto de nuvem e local com uma plataforma de infraestrutura hiperconvergente na nuvem pública. Sua equipe pode começar criando habilidades de nuvem com a integração interna com os serviços de gerenciamento de infraestrutura do Azure:

- Azure Site Recovery para alta disponibilidade e DRaaS (recuperação de desastre como serviço).

- Azure Monitor, um hub centralizado para rastrear o que está acontecendo em seus aplicativos, em sua rede e em sua infraestrutura – com análises avançadas da plataforma de IA.

- Testemunha em Nuvem, para usar o Azure como o desempate leve do quorum de cluster.

- Backup do Azure para proteção de dados externa e para proteção contra ransomware.

- Gerenciamento de Atualizações do Azure para avaliação e implantações de atualizações para as VMs Windows em execução no Azure e localmente.

- Adaptador de Rede do Azure para conectar recursos locais às suas VMs no Azure por meio de uma VPN ponto a site.

- Sincronize seu servidor de arquivos com a nuvem usando a Sincronização de Arquivos do Azure.

Para obter detalhes, confira [Connecting Windows Server to Azure hybrid services](..\manage\windows-admin-center\azure\index.md) (Como conectar o Windows Server aos serviços híbridos do Azure).

## <a name="management-tools-and-system-center"></a>Ferramentas de gerenciamento e System Center

O Azure Stack HCI usa a mesma virtualização e o mesmo armazenamento definido pelo software e software de rede que o Azure Stack. Com o Azure Stack HCI, você tem direitos de administrador completos sobre o cluster: é possível usar o [Windows Admin Center](..\manage\windows-admin-center\overview.md), o [System Center](https://www.microsoft.com/cloud-platform/system-center) e qualquer recurso do [Hyper-V](../virtualization/hyper-v/hyper-v-on-windows-server.md), dos [Espaços de Armazenamento Diretos](..\storage\storage-spaces\storage-spaces-direct-overview.md), do PowerShell e ferramentas de terceiros, como o 5Nine Manager.

O Microsoft Azure é executado na mesma plataforma do Windows Server e do Hyper-V que está incluída no Windows Server. O Windows Server e o System Center incluem aprimoramentos e melhores práticas da experiência da Microsoft em operar redes de data centers de escala global como o Microsoft Azure para você, para que você possa implantar as mesmas tecnologias para ter flexibilidade, automação e controle ao usar as tecnologias de rede projetadas pelo software.

Implante e gerencie a infraestrutura com o VMM (Gerenciamento de Máquina Virtual) do System Center e com o System Center Operations Manager. Com o VMM, você provisiona e gerencia os recursos necessários para criar e implantar máquinas virtuais e serviços em nuvens privadas. Com o Operations Manager, você monitora serviços, dispositivos e operações em toda a empresa para identificar problemas para ação imediata.

## <a name="hardware-partners"></a>Parceiros de hardware

É possível comprar soluções do Azure Stack HCI validadas que executam o Windows Server 2019 de 15 parceiros. O parceiro da Microsoft de sua preferência coloca você em funcionamento sem um tempo de criação e de design demorado e oferece um único ponto de contato para serviços de implementação e de suporte.

Acesse o [site do Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) para ver nossas mais de 70 soluções do Azure Stack HCI disponíveis desses parceiros da Microsoft no momento: ASUS, Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, primeLine Solutions, QCT, SecureGUARD e Supermicro.

## <a name="faq"></a>Perguntas frequentes

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>O que as soluções do Azure Stack e do Azure Stack HCI têm em comum?

As soluções do Azure Stack HCI contam com a mesma computação definida pelo software baseada em Hyper-V e com as mesmas tecnologias de armazenamento e de rede que o Azure Stack. As duas ofertas atendem a rigorosos critérios de teste e de validação para garantir a confiabilidade e a compatibilidade com a plataforma de hardware subjacente.

### <a name="how-are-they-different"></a>Como elas são diferentes?

Com o Azure Stack, você executa serviços de nuvem localmente. É possível executar serviços IaaS e PaaS do Azure localmente para criar e executar de maneira consistente aplicativos de nuvem em qualquer lugar, gerenciados com o portal do Azure local.

Com o Azure Stack HCI, você executa cargas de trabalho virtualizadas localmente, gerenciadas com o Windows Admin Center e com as ferramentas conhecidas do Windows Server. Opcionalmente, é possível conectar-se ao Azure para cenários híbridos como recuperação de site baseada em nuvem, monitoramento e outros.

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>Por que a Microsoft está trazendo sua oferta de HCI para a família Azure Stack?

A tecnologia hiperconvergente da Microsoft já é a base do Azure Stack.

Muitos clientes da Microsoft têm ambientes de TI complexos e nossa meta é fornecer soluções que os atendam onde eles estiverem com a tecnologia certa para a necessidade de negócios certa. O Azure Stack HCI é uma evolução das soluções WSSD (Definidas por Software do Windows Server) baseadas no Windows Server 2016 disponíveis anteriormente dos nossos parceiros de hardware. Nós o trouxemos para a família Azure Stack, porque começamos a oferecer novas opções para se conectar perfeitamente ao Azure para se ter serviços de gerenciamento de infraestrutura.

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>O Azure Stack HCI precisa estar conectado ao Azure?

Não, isso é totalmente opcional. É possível aproveitar a integração com o Azure para cenários híbridos como recuperação de desastre e backup externo, monitoramento baseado em nuvem e gerenciamento de atualizações, mas eles são opcionais. Compreenderemos totalmente e o adaptaremos se preferir ou precisar executar desconectado.

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>Como o Azure Stack HCI se relaciona com o Windows Server?

O Windows Server 2019 é a base de quase todo produto do Azure. Todos os recursos que você valoriza continuam sendo fornecidos no Windows Server e compatíveis com ele. O Azure Stack HCI é a maneira recomendada de implantar o HCI localmente, usando hardware validado pela Microsoft dos nossos parceiros.

### <a name="will-i-be-able-to-upgrade-from-azure-stack-hci-to-azure-stack"></a>Eu conseguirei atualizar do Azure Stack HCI para o Azure Stack? 

Não, mas os clientes podem migrar suas cargas de trabalho do Azure Stack HCI para o Azure Stack ou para o Azure.

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>Quais serviços do Azure eu posso conectar ao Azure Stack HCI?

Para ver uma lista atualizada de serviços do Azure aos quais você pode conectar o Azure Stack HCI, confira [Connecting Windows Server to Azure hybrid services](../azure-hybrid-services/index.md) (Como conectar o Windows Server aos serviços híbridos do Azure).

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>Como o custo do Azure Stack HCI compara-se ao do Azure Stack? 

O Azure Stack é vendido como um sistema totalmente integrado que inclui serviços e suporte. É possível comprar o Azure Stack como um sistema que você gerencia ou como um serviço totalmente gerenciado dos nossos parceiros. Além do sistema básico, os serviços do Azure executados no Azure Stack ou no Azure são vendidos para serem pagos conforme o uso.

As soluções do Azure Stack HCI seguem o modelo de compra tradicional. É possível comprar hardware validado de parceiros e do software do Azure Stack HCI (Windows Server 2019 Datacenter Edition com funcionalidades de datacenter definidas por software e o Windows Admin Center) de vários canais existentes. Para os serviços do Azure que podem ser usados com o Windows Admin Center, pague com uma assinatura do Azure.

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>Como posso comprar soluções do Azure Stack HCI?

Siga estas etapas:

1. Compre um sistema de hardware validado pela Microsoft do parceiro de hardware de sua preferência.
1. Instale o Windows Server 2019 Datacenter Edition e o Windows Admin Center para gerenciamento e para a capacidade de se conectar ao Azure para serviços de nuvem
1. Opcionalmente, use sua conta do Azure para anexar o gerenciamento baseado em nuvem e os serviços de segurança às suas cargas de trabalho.

![Para comprar soluções do Azure Stack HCI, escolha o parceiro e a configuração de hardware que melhor atende às suas necessidades.](media/howbuy_ashci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>Comparar o Azure Stack e o Azure Stack HCI

Conforme sua organização se transforma digitalmente, você pode achar que pode se mover mais rápido usando serviços de nuvem pública para se desenvolver em arquiteturas modernas e para atualizar aplicativos herdados. Contudo, por motivos que incluem obstáculos tecnológicos e regulatórios, muitas cargas de trabalho devem continuar localmente. A tabela a seguir ajuda você a determinar qual estratégia de nuvem híbrida da Microsoft oferece o que você precisa e onde precisa, fornecendo inovação de nuvem para cargas de trabalho onde quer que elas residam.

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Novas habilidades, processos inovadores|Mesmas habilidades, processos conhecidos|
|Serviços do Azure em seu datacenter|Conecte seu datacenter aos serviços do Azure|

### <a name="when-to-use-azure-stack"></a>Quando usar o Azure Stack

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Use o Azure Stack para IaaS (infraestrutura como serviço) de autoatendimento, com isolamento forte e estorno e acompanhamento de uso precisos para vários locatários colocalizados. Ideal para provedores de serviço e nuvens privadas corporativas. Modelos do Azure Marketplace.|O Azure Stack HCI não impõe nativamente nem fornece para multilocação.|
|Use o Azure Stack para desenvolver e executar aplicativos que dependem de serviços PaaS (plataforma como serviço) como Aplicativos Web, Functions ou Hubs de Eventos locais. Esses serviços são executados no Azure Stack exatamente como acontece no Azure, fornecendo um ambiente de desenvolvimento e de tempo de execução híbrido consistente.|O Azure Stack HCI não executa serviços PaaS localmente.
|Use o Azure Stack para modernizar a implantação e a operação de aplicativos com práticas de DevOps como a infraestrutura como código, a CI/CD (integração contínua e implantação contínua) e recursos convenientes como Extensões de VM consistentes com o Azure. Ideal para equipes Dev e DevOps.|O Azure Stack HCI não inclui nativamente nenhuma ferramenta de DevOps.

### <a name="when-to-use-azure-stack-hci"></a>Quando usar o Azure Stack HCI

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|O Azure Stack requer, no mínimo, quatro nós e seus próprios comutadores de rede.|Use o Azure Stack HCI para ter volume de memória mínimo para ROBO (escritório remoto/filial). Comece com apenas dois nós de servidor e rede back-to-back sem comutador para ter simplicidade de pico e acessibilidade. O hardware oferece início em quatro unidades e 64 GB de memória, bem abaixo de US$ 10 mil/nó.
|O Azure Stack restringe o conjunto de recursos e a capacidade de configuração do Hyper V para ter consistência com o Azure.|Use o Azure Stack HCI para ter uma visualização de Hyper-V sem sofisticação para aplicativos empresariais clássicos como o Exchange, o SharePoint e o SQL Server, e para virtualizar as funções do Windows Server como o Servidor de Arquivos, a DNS, o DHCP, o IIS e o AD. Acesso irrestrito a todos os recursos do Hyper-V como VMs blindadas.|
|O Azure Stack não expõe essas tecnologias de infraestrutura.|Use o Azure Stack HCI para substituir a infraestrutura definida por software no lugar de matrizes de armazenamento ou dispositivos de rede com vencimento, sem uma nova arquitetura principal. Os Espaços de Armazenamento Diretos internos e a SDN (Rede Definida por Software) oferecem integração sem problemas com ambientes Hyper-V.|