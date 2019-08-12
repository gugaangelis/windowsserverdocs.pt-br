---
title: Ajuste de desempenho de contêineres do Windows Server
description: Recomendações de ajuste de desempenho para contêineres no Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: akino
ms.date: 10/16/2017
ms.openlocfilehash: 072d71a7825907ada7d4bc02eb5390722692e81d
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863417"
---
# <a name="performance-tuning-windows-server-containers"></a>Ajuste de desempenho de contêineres do Windows Server

## <a name="introduction"></a>Introdução
O Windows Server 2016 é a primeira versão do Windows que dá suporte à tecnologia de contêiner interna do sistema operacional. No Server 2016, há dois tipos de contêineres disponíveis: Contêineres do Windows Server e contêineres do Hyper-V. Cada tipo de contêiner dá suporte ao SKU do Server Core ou do Nano Server do Windows Server 2016. 

Essas configurações têm implicações de desempenho diferentes que estão detalhadas abaixo para ajudá-lo a entender qual é a ideal para seus cenários. Além disso, detalhamos as configurações que afetam o desempenho e descrevemos as vantagens e desvantagens de cada uma dessas opções.

### <a name="windows-server-container-and-hyper-v-containers"></a>Contêiner do Windows Server e contêineres do Hyper-V

O contêiner do Windows Server e os contêineres do Hyper-V oferecem muitos dos mesmos benefícios de portabilidade e consistência, mas diferem em termos de garantias de isolamento e de características de desempenho.

Os **contêineres do Windows Server** fornecem isolamento de aplicativos por meio da tecnologia de isolamento de processo e de namespace. Um Contêiner do Windows Server compartilha um kernel com o host do contêiner e com todos os contêineres em execução no host.

Os **contêineres do Hyper-V** expandem o isolamento fornecido pelos contêineres do Windows Server executando cada contêiner em uma máquina virtual altamente otimizada. Nesta configuração, o kernel do host do contêiner não é compartilhado com os Contêineres de Hyper-V.

O isolamento adicional fornecido pelos contêineres do Hyper-V é obtido em grande parte com uma camada de isolamento do hipervisor entre o contêiner e o host do contêiner. Isso afeta a densidade do contêiner pois, ao contrário dos contêineres do Windows Server, pode ocorrer a redução do compartilhamento do sistema de arquivos e de binários, resultando no aumento geral do armazenamento e do volume de memória. Além disso há a sobrecarga adicional esperada em alguns caminhos de rede, de E/S de armazenamento e de CPU.

### <a name="nano-server-and-server-core"></a>Nano Server e Server Core

Os contêineres do Windows Server e os contêineres do Hyper-V dão suporte ao Server Core e a uma nova opção de instalação disponível no Windows Server 2016: o [Nano Server](https://technet.microsoft.com/windows-server-docs/compute/nano-server/getting-started-with-nano-server). 

O Nano Server é um sistema operacional de servidor administrado remotamente e otimizado para data centers e nuvens privadas. É semelhante ao Windows Server no modo Server Core, mas significativamente menor, não tem nenhum recurso de logon local e só oferece suporte a agentes, ferramentas e aplicativos de 64 bits. Ele ocupa bem menos espaço em disco e é iniciado mais rapidamente.

## <a name="container-start-up-time"></a>Tempo de inicialização do contêiner
O tempo de inicialização do contêiner é uma métrica fundamental em muitos dos cenários nos quais os contêineres oferecem o melhor benefício. Portanto, é fundamental entender como otimizar o tempo de inicialização do contêiner da melhor maneira possível. Abaixo estão algumas vantagens e desvantagens dos ajustes para entender como melhorar o tempo de inicialização.

### <a name="first-logon"></a>Primeiro logon

A Microsoft fornece uma imagem base para o Nano Server e o Server Core. A imagem base que é fornecida para o Server Core foi otimizada, removendo a sobrecarga de tempo de inicialização associada ao primeiro logon (OOBE). Isso não ocorre com a imagem base do Nano Server. No entanto, esse custo pode ser removido das imagens baseadas no Nano Server ao confirmar pelo menos uma camada para a imagem de contêiner. Os próximos inícios do contêiner usando a imagem não gerarão o custo do primeiro logon.
### <a name="scratch-space-location"></a>Local do espaço temporário

Os contêineres, por padrão, usam um espaço temporário na mídia da unidade do sistema do host do contêiner para o armazenamento durante o tempo de vida do contêiner em execução. Isso funciona como a unidade do sistema do contêiner e, portanto, muitas das gravações e leituras feitas na operação de contêiner seguem esse caminho. Em sistemas host nos quais a unidade do sistema está na mídia magnética do disco de rotação (HDDs), mas há uma mídia de armazenamento mais rápida disponível (HDDs ou SSDs mais rápidos), é possível mover o espaço temporário do contêiner para uma unidade diferente. Isso é feito usando o comando dockerd –g. Esse comando é global e afetará todos os contêineres em execução no sistema.

### <a name="nested-hyper-v-containers"></a>Contêineres aninhados do Hyper-V
O Hyper-V para Windows Server 2016 introduz o suporte para hipervisor aninhado. Ou seja, agora é possível executar uma máquina virtual de dentro de uma máquina virtual. Isso abre muitos cenários úteis, mas também exagera o impacto de desempenho que o hipervisor gera, pois há dois níveis de hipervisores executando acima do host físico.

Para contêineres, isso tem um impacto ao executar um contêiner do Hyper-V em uma máquina virtual. Como um contêiner do Hyper-V oferece isolamento por meio de uma camada do hipervisor entre si e o host do contêiner, quando o host do contêiner é uma máquina virtual baseada no Hyper-V, há uma sobrecarga de desempenho associada em termos de tempo de inicialização do contêiner, de E/S de armazenamento, de E/S e taxa de transferência de rede e de CPU.

## <a name="storage"></a>Armazenamento
### <a name="mounted-data-volumes"></a>Volumes de dados montados

Os contêineres oferecem a capacidade de usar a unidade de sistema host do contêiner para o espaço temporário do contêiner. No entanto, o espaço temporário do contêiner tem um período de vida igual do contêiner. Ou seja, quando o contêiner for interrompido, o espaço temporário e todos os dados associados desaparecerão.

No entanto, há muitos cenários em que se deseja que os dados persistam independentemente do tempo de vida do contêiner. Nesses casos, damos suporte à montagem de volumes de dados do host do contêiner no contêiner. Para os contêineres do Windows Server, há uma sobrecarga de caminho de E/S insignificante associada aos volumes de dados montados (desempenho quase nativo). No entanto, ao montar volumes de dados em contêineres do Hyper-V, há uma certa degradação de desempenho de E/S nesse caminho. Além disso, esse impacto é exagerado quando os contêineres do Hyper-V são executados em máquinas virtuais.

### <a name="scratch-space"></a>Espaço temporário

Por padrão, os contêineres do Hyper-V e os contêineres do Windows Server fornecem um VHD dinâmico de 20 GB para o espaço temporário do contêiner. Para ambos os tipos de contêiner, o sistema operacional do contêiner ocupa uma parte desse espaço e isso ocorre para cada contêiner iniciado. Assim, é importante lembrar que cada contêiner iniciado tem algum impacto de armazenamento e, dependendo da carga de trabalho, pode gravar até 20 GB da mídia de armazenamento de backup. As configurações de armazenamento do servidor devem ser projetadas com isso em mente.
(é possível configurar o tamanho temporário)

## <a name="networking"></a>Rede
Os contêineres do Windows Server e os contêineres do Hyper-V oferecem uma variedade de modos de rede para atender melhor às necessidades de diferentes configurações de rede. Cada uma dessas opções apresenta suas próprias características de desempenho.

### <a name="windows-network-address-translation-winnat"></a>WinNAT (conversão de endereços de rede do Windows)

Cada contêiner receberá um endereço IP de um prefixo IP privado e interno (por exemplo, 172.16.0.0/12). Há suporte para o mapeamento/encaminhamento de porta do host de contêiner para os pontos de extremidade do contêiner. O Docker cria uma rede NAT por padrão, quando o dockerd é executado pela primeira vez.

Desses três modos, a configuração de NAT é o caminho de E/S de rede mais dispendioso, mas tem a menor quantidade de configuração necessária. 

Contêineres do Windows Server usam uma vNIC do host para se conectar ao comutador virtual. Os contêineres do Hyper-V usam uma NIC de VM sintética (não exposta à VM do utilitário) para se conectar ao comutador virtual. Quando os contêineres estão se comunicando com a rede externa, os pacotes são roteados pela WinNAT com conversões de endereço aplicadas, o que gera uma sobrecarga.

### <a name="transparent"></a>Transparente

Cada ponto de extremidade do contêiner é conectado diretamente à rede física. Os endereços IP da rede física podem ser atribuídos de forma estática ou dinâmica usando um servidor DHCP externo.

O modo transparente é o menos dispendioso em termos de caminho de E/S de rede e os pacotes externos são passados diretamente para o adaptador de rede virtual do contêiner fornecendo acesso direto à rede externa.

### <a name="l2-bridge"></a>Ponte L2
Cada ponto de extremidade do contêiner ficará na mesma sub-rede IP que o host do contêiner. Os endereços IP devem ser atribuídos estaticamente diretamente do mesmo prefixo que o host do contêiner. Todos os pontos de extremidade de contêiner no host terão o mesmo endereço MAC devido à conversão de endereços de camada 2.

O modo Ponte L2 tem um desempenho melhor que o modo WinNAT, pois fornece acesso direto à rede externa, mas tem um desempenho menor do que o modo Transparente, que também apresenta a conversão de endereços MAC.




