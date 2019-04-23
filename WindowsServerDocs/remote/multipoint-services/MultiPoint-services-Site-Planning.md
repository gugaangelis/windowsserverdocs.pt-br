---
title: Planejamento do site do MultiPoint Services
description: Informações de planejamento para implantações do MultiPoint Services no Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 60d2e1fedc018136f5668d55a8afb2e0574d94de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845067"
---
# <a name="multipoint-services-site-planning"></a>Planejamento do site do MultiPoint Services
Você deve considerar o local em que um ou mais computadores que executam o MultiPoint Services e suas estações associadas serão implantados.  
  
O computador que está executando a função do MultiPoint Services deve ter acesso conveniente para uma fonte de energia e os dispositivos periféricos conectados diretamente a ele, como uma impressora. Além disso, o computador executando o MultiPoint Services deve ter acesso conveniente a uma conexão de rede. Uma conexão de rede é necessária para acessar a Internet e onde estiver disponível, uma rede local.  
  
Fatores adicionais a serem considerados incluem o seguinte:  
  
-   Será o sistema MultiPoint Services configurado em uma sala específica ou será ele ser configurado em um carrinho sem interrupção ou a tabela, para que ele pode ser movido de um local para outro?  
  
    > [!NOTE]  
    > Se você planeja usar uma configuração móvel, você poderá *associar* as estações com o MultiPoint Services toda vez que você Reconecte-os para certificar-se de que cada teclado e mouse está associada com o monitoramento apropriado.  
  
-   A estação principal estará localizada ao lado de outras estações, ou será separado? Por exemplo, se o sistema MultiPoint Services é configurado em uma sala de aula, será a estação principal na mesa do professor e as estações padrão posicionada em outro lugar na sala? Quando o computador executando o MultiPoint Services for reiniciado, a estação principal terá acesso às telas de inicialização. Se você estiver preocupado com esse nível de acesso em uma sala de aula, talvez você prefira colocar a estação principal na mesa do professor.  
  
-   Quantas estações caiba no espaço?  
  
-   Você precisa de uma rede? Uma solução de servidor único que usa vídeo direto conectado ou zero cliente USB estações conectadas não precisa de uma rede.  
  
-   Existem conexões de rede suficientes na sala para suportar o número necessário de computadores executando o MultiPoint Services  
  
-   Onde estão localizadas as saídas de alimentação?  
  
-   Você precisará que um dispositivo de vídeo adicional, como um projetor? Se você planeja usar um projetor, ele travará do que o limite máximo ou será posicionado em uma tabela?  
  
-   Que tipo de cabos, será necessário e quantos será necessária?  
  
-   Considere como você deseja expandir no futuro. Você adicionará mais estações?  
  
## <a name="station-layout-and-configuration"></a>Configuração e o layout de estação  
O layout físico do seu site pode afetar sua escolha de tipo de estação. Para obter mais detalhes sobre os tipos diferentes de estação, consulte [estações do MultiPoint](MultiPoint-services-Stations.md) neste guia. Vários tipos de estação são permitidos em um único MultiPoint Services. Isso proporciona mais flexibilidade para atender às suas necessidades de instalação.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Layout para as estações de vídeo diretamente conectados  
  
-   Para uma estação de vídeo diretamente conectados, a distância entre os monitores e o computador é limitada pelo comprimento de cabo de vídeo.  
  
-   Usar hubs intermediários ou hubs de estação em corrente margarida tem suporte para facilidade de implantação, mas o número máximo recomendado de hubs consecutivos é três. Isso significa que a distância máxima do computador para o hub de estação é 15 metros, porque cada cabo USB 2.0 tem o comprimento máximo de cinco medidores.  
  
> [!IMPORTANT]  
> Sempre deve haver estação de conectada vídeo direta pelo menos uma por computador para atuar como a estação principal.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Estações conectadas de layout para zero cliente USB  
  
-   Usar hubs intermediários ou hubs de estação em corrente margarida tem suporte para facilidade de implantação, mas o número máximo recomendado de hubs consecutivos é três. Isso significa que a distância máxima do computador para o hub de estação é 15 metros, porque cada cabo USB 2.0 tem o comprimento máximo de cinco medidores.  
  
-   O número máximo recomendado de USB zero clientes conectados a um único hub intermediário é três.  
  
    > [!NOTE]  
    > Alguns computadores vêm com um hub genérico na placa-mãe, que tem o efeito da adição de um hub adicional entre o *hub raiz* do computador e os hubs de estação.  
  
-   Se o vídeo será muito usado, é recomendável que você se conectar não mais de dois clientes USB zero a uma porta USB no servidor. Por exemplo, se um hub intermediário for usado, apenas dois clientes USB zero devem conectados a ele. Ou, se você estiver com corrente margarida encadeamento clientes USB zero, apenas dois USB zero clientes deveriam ser encadeados juntos. A adição de cada zero cliente USB à porta USB no servidor reduz a largura de banda vídeo disponível.  
  
-   Se você pretende conectar mais de três clientes USB zero para uma única porta USB no servidor, é recomendável a usar o USB 3.0 entre o servidor e o hub intermediário.  
  
> [!NOTE]  
> É recomendável que você verifique se o desempenho por meio de seus aplicativos e hardware para decidir quantos USB zero clientes, você pode se conectar a uma porta USB no servidor.  
  
![Hubs downstream](./media/WMS_diagram6.gif)  
  
**Figura 5** sistema MultiPoint Services com três USB zero clientes conectados a um único hub intermediário  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Estações conectadas de layout para RDP através de LAN  
Não há nenhuma limitação de distância física para clientes de LAN. Eles estão na LAN, desde que eles possam se conectar ao sistema MultiPoint Services.  
  
## <a name="using-additional-hubs"></a>Usando os hubs adicionais  
Hubs adicionais podem ser usados para facilitar a instalação. Há três tipos de hubs que são usados em um sistema MultiPoint Services:  
  
-   [Hubs de estação](#Station-hubs)  
  
-   [Hubs intermediários](#Intermediate-hubs)  
  
-   [Hubs downstream](#Downstream-hubs)  
  
### <a name="station-hubs"></a>Hubs de estação  
Um hub de estação é um hub externo que tenha sido associado uma estação do MultiPoint Services. Como mínimo, o hub de estação terá um teclado conectado-na ele. Ele também pode ter adicionais periféricos conectados. Um hub de estação pode ser um hub USB genérico que esteja de acordo com o USB 2.0 ou posterior especificação. Hubs de estação devem ser alimentados externamente se os dispositivos de alta potência serão o plug-in a eles.  
  
**Hub raiz** hub USB de um que é integrado ao controlador de host na placa-mãe de um computador é conhecido como um *hub raiz*. Hubs de estação são geralmente conectado no hub raiz no computador executando o MultiPoint Services.  
  
> [!NOTE]  
> Os hubs de raiz não devem ser usados como hubs de estação. Quando as portas USB estão incorporadas em um computador, geralmente não é possível determinar qual hub USB de raiz, internamente, eles estão conectados a. Como tal, se você conectado um mouse diretamente às portas USB do computador e o teclado de estação, você pode realmente ser conectando no teclado e mouse aos hubs de raiz USB diferentes. Para garantir que o teclado e mouse estão no mesmo hub, plug-in um hub de estação para a porta USB do computador e, em seguida, plug-in de teclado e mouse ao estação hub.  
  
**Margarida encadeamento estações** pode ser mais fácil conectar hubs de estação para outro hub de estação, em vez de diretamente ao computador. Isso permite que você se conecte a um hub USB a um hub de estação que é já conectado no computador, para que você tenha um hub de estação anexado a outro hub de estação.  
  
Deve haver mais do que três clientes USB zero ou estação hubs interligados consecutivamente. É necessário ter cuidado se a largura de banda USB não foi excedida quando correntes interligadas hubs de estação.  
  
![Estações com corrente margarida](./media/WMS_diagram5.gif)  
  
**Figura 6** sistema MultiPoint Services com estações em corrente margarida  
  
### <a name="intermediate-hubs"></a>Hubs intermediários  
Um hub intermediário é um hub que está entre o servidor e um hub de estação. Ele é normalmente usado para aumentar o número de portas que estão disponíveis para hubs de estação ou estender a distância das estações do computador. É recomendável que não mais do que dois hubs intermediários são usados entre um hub de estação e o servidor.  
  
Hubs intermediários devem ser o USB 2.0 ou posterior, e devem ser alimentados externamente. USB 3.0 é recomendável entre o servidor e o hub intermediário, se você estiver se conectando a mais de três clientes USB zero para um hub intermediário.  
  
### <a name="downstream-hubs"></a>Hubs downstream  
Um hub de downstream está conectado a um hub de estação para adicionar portas mais disponíveis para dispositivos de estação. Um hub de downstream pode ser externamente alimentado ou barramento, dependendo dos dispositivos que estão conectados no hub.  
  
![Várias conexões de cliente zero USB](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figura 7** sistema MultiPoint Services com um hub intermediário, um hub de estação e um hub de downstream  
  
## <a name="users-stations-and-computers"></a>Computadores, usuários e estações  
O número de estações, que será necessário depende do número de pessoas que tenham acesso a computadores que executam o MultiPoint Services ao mesmo tempo. Da mesma forma, o número de computadores executando o MultiPoint Services, será necessário depende do número total de estações necessárias. Estações de vídeo diretamente conectados, conectados por USB zero-client estações e estações de RDP-over-conectados à rede local são todas as estações consideradas. Além disso, se for usada a funcionalidade de tela dividida, cada metade é considerado uma estação.  
  
## <a name="power-considerations"></a>Considerações sobre a alimentação  
Os seguintes componentes exigem acesso a uma extensão de energia ou tomada:  
  
-   Servidor  
-   Monitores
-   Intermediário hubs \(se usado\) 
-   Alguns clientes USB zero  
-   Dispositivos USB ligados, como alguns dispositivos de armazenamento externo e unidades de DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Layouts de sistema do MultiPoint Services de exemplo  
Dependendo os móveis disponíveis, o tamanho da sala, o número de computadores que estão executando o MultiPoint Services e as estações na sala, há uma variedade de maneiras que as estações físicas podem ser organizadas. Os diagramas a seguir ilustram cinco alternativas possíveis.  
  
> [!NOTE]  
> Alguns desses diagramas mostram um projetor conectado ao sistema MultiPoint Services. Este é apenas um exemplo; incluir um projetor em um sistema MultiPoint Services é opcional.  
  
**Laboratório de informática** nessa configuração, as estações são organizadas em torno das paredes da sala, com os alunos voltada para as paredes.  
  
![Configuração de laboratório de informática](./media/WMS_ComputerLabLayout.gif)  
  
**Grupos de** nessa configuração, há três computadores que estejam executando o MultiPoint Services, com estações agrupados em torno de cada computador.  
  
![Sala de aula configurada com compartimentos de servidores](./media/WMS_ClassroomPods.gif)  
  
**Palestra sala** nessa configuração, as estações são definidas em linhas. Uma vantagem dessa configuração é que todos os alunos enfrentam o instrutor.  
  
![Sala de aula configurada como sala de conferência](./media/WMS_LectureRoom.gif)  
  
**Centro de atividade** essa configuração consiste em um layout de sala de conferência tradicionais para a mesa do escritório e ele tem uma área separada com um único computador que esteja executando o MultiPoint Services com suas estações associadas.  
  
![Centro de atividade do multiPoint Services](./media/WMSActivityCenter.gif)  
  
**Office Small business** nessa configuração, o computador que está executando o MultiPoint Services é colocado em um local central e os usuários em todo o office se conectar a ele usando uma rede local \(LAN\).  
  
![Estação conectada com zero cliente USB](./media/Diagram1.gif)