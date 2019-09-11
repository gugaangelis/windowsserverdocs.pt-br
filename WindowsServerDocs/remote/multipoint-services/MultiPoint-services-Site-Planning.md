---
title: Planejamento do site do MultiPoint Services
description: Informações de planejamento para implantações de serviços do MultiPoint no Windows Server 2016
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
ms.openlocfilehash: 3d49b2861d81a938fb20544c3edeb0976ac6d327
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871636"
---
# <a name="multipoint-services-site-planning"></a>Planejamento do site do MultiPoint Services
Você deve considerar o local em que um ou mais computadores que executam os serviços do MultiPoint e suas estações associadas serão implantados.  
  
O computador que está executando a função de serviços do MultiPoint deve ter acesso conveniente a uma fonte de alimentação e aos dispositivos periféricos conectados diretamente a ele, como uma impressora. Além disso, o computador que executa os serviços do MultiPoint deve ter acesso conveniente a uma conexão de rede. Uma conexão de rede é necessária para acessar a Internet e, quando disponível, uma LAN.  
  
Os fatores adicionais a serem considerados incluem o seguinte:  
  
-   O sistema de serviços do MultiPoint será configurado em um espaço específico ou será configurado em um carrinho ou tabela sem interrupção, para que ele possa ser movido do local para o local?  
  
    > [!NOTE]  
    > Se você planeja usar uma configuração móvel, você pode *associar* as estações com os serviços do MultiPoint sempre que reconectá-los para garantir que cada teclado e mouse estejam associados ao monitor apropriado.  
  
-   A estação principal estará localizada ao lado das outras estações, ou ela será separada? Por exemplo, se o sistema de serviços do MultiPoint estiver configurado em uma sala de aula, a estação principal estará na mesa do professor e as estações padrão posicionadas em outro lugar na sala? Quando o computador que executa os serviços do MultiPoint for reiniciado, a estação principal terá acesso às telas de inicialização. Se você estiver preocupado com esse nível de acesso em uma configuração de sala de aula, talvez prefira colocar a estação principal na mesa do professor.  
  
-   Quantas estações serão ajustadas na sala?  
  
-   Você precisa de uma rede? Uma única solução de servidor que usa estações conectadas a um cliente via vídeo ou sem USB não precisa de uma rede.  
  
-   Há conexões de rede suficientes na sala para dar suporte ao número necessário de computadores que executam os serviços do MultiPoint  
  
-   Onde as tomadas de energia estão localizadas?  
  
-   Será necessário um dispositivo de vídeo adicional, como um projetor? Se você planeja usar um projetor, ele será interrompido do teto ou será posicionado em uma tabela?  
  
-   Que tipo de cabos será necessário e quantos serão necessários?  
  
-   Considere como você pode desejar expandir no futuro. Você estará adicionando mais estações?  
  
## <a name="station-layout-and-configuration"></a>Configuração e layout de estação  
O layout físico do seu site pode afetar sua escolha de tipo de estação. Para obter mais detalhes sobre os diferentes tipos de estação, consulte [estações multiponto](MultiPoint-services-Stations.md) neste guia. Vários tipos de estação são permitidos em um único serviço do MultiPoint. Isso proporciona flexibilidade extra para atender às suas necessidades de instalação.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Layout para estações conectadas diretamente a vídeo  
  
-   Para uma estação conectada diretamente ao vídeo, a distância entre os monitores e o computador é limitada pelo comprimento do cabo de vídeo.  
  
-   O uso de hubs intermediários ou hubs de estação conectados é suportado para facilitar a implantação, mas o número máximo recomendado de hubs consecutivos é três. Isso significa que a distância máxima do computador para o Hub de estação é 15 metros, pois cada cabo USB 2,0 tem o comprimento máximo de cinco metros.  
  
> [!IMPORTANT]  
> Sempre deve haver pelo menos uma estação conectada de vídeo direta por computador para atuar como a estação principal.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Layout para estações conectadas a clientes USB com zero  
  
-   O uso de hubs intermediários ou hubs de estação conectados é suportado para facilitar a implantação, mas o número máximo recomendado de hubs consecutivos é três. Isso significa que a distância máxima do computador para o Hub de estação é 15 metros, pois cada cabo USB 2,0 tem o comprimento máximo de cinco metros.  
  
-   O número máximo recomendado de clientes USB zero conectados a um único hub intermediário é três.  
  
    > [!NOTE]  
    > Alguns computadores vêm com um hub genérico na placa-mãe, que tem o efeito de adicionar um hub adicional entre o *hub raiz* do computador e os hubs de estação.  
  
-   Se o vídeo for muito usado, é recomendável que você conecte não mais do que dois clientes USB com uma porta USB no servidor. Por exemplo, se um hub intermediário for usado, somente dois clientes USB zero deverão estar conectados a ele. Ou, se você estiver interligando clientes USB zero, somente dois clientes USB serão encadeados. A adição de cada cliente USB zero à porta USB no servidor diminui a largura de banda do vídeo disponível.  
  
-   Se você planeja conectar mais de três clientes USB de zero a uma única porta USB no servidor, é recomendável usar USB 3,0 entre o servidor e o hub intermediário.  
  
> [!NOTE]  
> É recomendável que você verifique o desempenho usando seus aplicativos e hardware para decidir quantos clientes USB zero você pode conectar a uma porta USB no servidor.  
  
![Hubs downstream](./media/WMS_diagram6.gif)  
  
**Figura 5** Sistema de serviços do MultiPoint com três clientes USB sem conexão com um único hub intermediário  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Layout para estações conectadas RDP em LAN  
Não há nenhuma limitação de distância física para clientes de LAN. Desde que estejam na LAN, eles podem se conectar ao sistema MultiPoint Services.  
  
## <a name="using-additional-hubs"></a>Usando hubs adicionais  
Hubs adicionais podem ser usados para facilitar a instalação. Há três tipos de hubs que são usados em um sistema de serviços do MultiPoint:  
  
-   [Hubs de estação](#station-hubs)  
  
-   [Hubs intermediários](#intermediate-hubs)  
  
-   [Hubs downstream](#downstream-hubs)  
  
### <a name="station-hubs"></a>Hubs de estação  
Um hub de estação é um hub externo que foi associado a uma estação de serviços do MultiPoint. No mínimo, o Hub de estação terá um teclado conectado a ele. Ele também pode ter periféricos adicionais anexados. Um hub de estação pode ser um hub USB genérico que esteja de acordo com a especificação USB 2,0 ou posterior. Os hubs de estação devem ser conectados externamente se dispositivos de alta potência forem plugin para eles.  
  
**Hub raiz** Um hub USB interno do controlador de host na placa-mãe de um computador é conhecido como um *hub raiz*. Os hubs de estação geralmente são conectados ao hub raiz no computador que executa os serviços do MultiPoint.  
  
> [!NOTE]  
> Os hubs raiz não devem ser usados como hubs de estação. Quando as portas USB são internas a um computador, geralmente não é possível determinar a qual hub raiz USB está conectada internamente. Dessa forma, se você conectou um teclado e um mouse de estação diretamente às portas USB do computador, você pode realmente estar conectando o teclado e o mouse a diferentes hubs raiz USB. Para garantir que o teclado e o mouse estejam no mesmo Hub, conecte um hub de estação à porta USB do computador e, em seguida, conecte o teclado e o mouse ao Hub de estação.  
  
**Estações de encadeamento de Margarida** Pode ser mais fácil conectar os hubs de estação a outro hub de estação em vez de diretamente ao computador. Isso permite que você conecte um hub USB a um hub de estação que já esteja conectado ao computador, para que você tenha um hub de estação conectado a outro hub de estação.  
  
Não deve haver mais de três clientes USB ou hubs de estação conectados consecutivamente. Deve-se ter cuidado para que a largura de banda USB não seja excedida ao encadear os hubs de estação.  
  
![Estações com corrente margarida](./media/WMS_diagram5.gif)  
  
**Figura 6** Sistema de serviços do MultiPoint com estações encadeadas  
  
### <a name="intermediate-hubs"></a>Hubs intermediários  
Um hub intermediário é um hub entre o servidor e um hub de estação. Normalmente, ele é usado para aumentar o número de portas que estão disponíveis para os hubs de estação ou para estender a distância das estações do computador. É recomendável que não mais do que dois hubs intermediários sejam usados entre um hub de estação e o servidor.  
  
Os hubs intermediários devem ser USB 2,0 ou posterior e devem ser ligados externamente. O USB 3,0 é recomendado entre o servidor e o hub intermediário se você estiver conectando mais de três clientes USB de zero a um hub intermediário.  
  
### <a name="downstream-hubs"></a>Hubs downstream  
Um hub downstream é conectado a um hub de estação para adicionar mais portas disponíveis para dispositivos de estação. Um hub downstream pode ser externo ou com capacidade de barramento, dependendo dos dispositivos que estão conectados ao Hub do.  
  
![Várias conexões de cliente USB com zero](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figura 7** Sistema MultiPoint Services com um hub intermediário, um hub de estação e um hub downstream  
  
## <a name="users-stations-and-computers"></a>Usuários, estações e computadores  
O número de estações que serão necessárias dependerá do número de pessoas que terão que acessar os computadores que executam os serviços do MultiPoint ao mesmo tempo. Da mesma forma, o número de computadores que executam os serviços do MultiPoint necessários dependerá do número total de estações necessárias. As estações conectadas a vídeo direto, estações USB-zero-conexão com o cliente e estações conectadas por RDP via LAN são todas consideradas estações. Além disso, se a funcionalidade da tela de divisão for usada, cada metade será considerada uma estação.  
  
## <a name="power-considerations"></a>Considerações de energia  
Os seguintes componentes exigem acesso a uma faixa de energia ou tomada:  
  
-   Servidor  
-   Monitores
-   Hubs \(intermediários, se usados\) 
-   Alguns clientes USB zero  
-   Dispositivos USB com alimentação, como alguns dispositivos de armazenamento externo e unidades de DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Exemplos de layouts do sistema de serviços do MultiPoint  
Dependendo dos móveis disponíveis, do tamanho da sala, do número de computadores que estão executando os serviços do MultiPoint e das estações na sala, há várias maneiras pelas quais as estações físicas podem ser organizadas. Os diagramas a seguir ilustram cinco alternativas possíveis.  
  
> [!NOTE]  
> Alguns desses diagramas mostram um projetor conectado ao sistema MultiPoint Services. Este é apenas um exemplo; a inclusão de um projetor em um sistema de serviços do MultiPoint é opcional.  
  
**Laboratório do computador** Nessa configuração, as estações são organizadas em volta das paredes da sala, com os alunos voltados para as paredes.  
  
![Configuração de laboratório de informática](./media/WMS_ComputerLabLayout.gif)  
  
**Grupos** do Nessa configuração, há três computadores que executam os serviços do MultiPoint, com estações agrupadas em todo o computador.  
  
![Sala de aula configurada com compartimentos de servidores](./media/WMS_ClassroomPods.gif)  
  
**Sala de palestras** Nessa configuração, as estações são configuradas em linhas. Uma vantagem dessa configuração é que todos os alunos enfrentam o instrutor.  
  
![Sala de aula configurada como sala de conferência](./media/WMS_LectureRoom.gif)  
  
**Centro de atividades** Essa configuração consiste em um layout de sala de palestras tradicional para as mesas e tem uma área separada com um único computador que está executando os serviços do MultiPoint com suas estações associadas.  
  
![Centro de atividades dos serviços do MultiPoint](./media/WMSActivityCenter.gif)  
  
**Escritório de pequena empresa** Nessa configuração, o computador que está executando os serviços do MultiPoint é colocado em um local central e os usuários em todo o escritório se conectam a ele usando \(uma\)LAN de rede de área local.  
  
![Estação conectada com zero cliente USB](./media/Diagram1.gif)