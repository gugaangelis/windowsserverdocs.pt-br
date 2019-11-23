---
title: Selecionar o hardware para o sistema MultiPoint Services
description: Considerações de hardware para serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e74961a2-bd38-48ae-b1c0-4b3eff761b4a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9cfd6572c82bf5c3754165420e61054ec12b9617
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388997"
---
# <a name="selecting-hardware-for-your-multipoint-services-system"></a>Selecionar o hardware para o sistema MultiPoint Services
Ao criar um sistema de serviços do MultiPoint, você deve selecionar um computador que atenda aos requisitos de sistema do Windows Server 2016. Se você estiver decidindo quais componentes selecionar, considere o seguinte:  
  
-   A faixa de preço de destino de sua solução completa.  
  
-   Os tipos de cenários de uso que você pode esperar para o sistema de serviços do MultiPoint, por exemplo, se os usuários estão executando programas multimídia, usando programas de processamento de texto ou produtividade, ou navegando na Internet.  
  
-   Se seu cenário tem grandes demandas de processamento ou memória.  
  
-   O número de usuários que podem estar usando o sistema ao mesmo tempo. Se você planeja ter muitos usuários em seu sistema ao mesmo tempo ou usuários que usam programas com uso intensivo de sistema, você deve planejar mais poder de computação para seu sistema.  
  
-   O tipo de estação. Quantas portas USB ou portas de vídeo você precisa?  
  
-   Planos de expansão futuros. Você planeja adicionar estações ao sistema de serviços do MultiPoint em uma data posterior? Você terá slots de placa de vídeo suficientes, portas USB ou toques de rede? A quantos usuários adicionais seu hardware precisará dar suporte?  
  
-   Layout físico. Para obter mais informações, consulte [planejamento de site dos serviços do MultiPoint](MultiPoint-services-Site-Planning.md).  
  
Um sistema de serviços do MultiPoint normalmente inclui os seguintes componentes:  
  
-   Um computador que esteja executando os serviços do MultiPoint, que inclui CPU, RAM, unidades de disco rígido e placas de vídeo.  
  
-   Um monitor, Hub de estação, teclado e mouse para cada estação.  
  
-   Dispositivos periféricos opcionais para estações de serviços do MultiPoint, incluindo alto-falantes, fones de ouvido, microfones ou dispositivos de armazenamento que estão disponíveis somente para o usuário da estação.  
  
-   Dispositivos periféricos opcionais que estão disponíveis para todos os usuários do sistema MultiPoint Services, conectados diretamente ao computador host, como impressoras, unidades de disco rígido externas e dispositivos de armazenamento USB.  
  
Use as informações a seguir para tomar decisões de hardware:  
  
-   [Selecionando uma CPU](#selecting-a-cpu)  
-   [Selecionando componentes de hardware](#selecting-hardware-components)  
  
## <a name="selecting-a-cpu"></a>Selecionando uma CPU  
Um sistema de serviços do MultiPoint é um ambiente de usuário de vários\-, com todos os usuários conectados a um único computador host. Isso aumenta o uso da CPU porque todos os usuários compartilham o mesmo computador. Algumas tarefas, como programas multimídia \(por exemplo, media players ou\-de software de edição de vídeo\), têm demandas de processamento maiores. Portanto, certifique-se de selecionar uma CPU que possa lidar com os requisitos de processamento para o número de usuários e tipos de cenários de usuário aos quais ele precisará dar suporte.  
  
Os serviços do MultiPoint requerem uma CPU baseada em x64\-e devem atender aos requisitos do sistema para o computador, conforme descrito em [requisitos de hardware e recomendações de desempenho](Hardware-Requirements-and-Performance-Recommendations.md).  
  
Os seguintes tipos de processadores foram testados para serem usados em um sistema de serviços do MultiPoint com programas de processamento de alta demanda\-, como programas multimídia:  
  
-   **Processador Dual\-Core:** O pode oferecer suporte a até 8 estações.  
-   **Processador Quad\-Core:** O pode oferecer suporte a até 16 estações.
-   **Processador Quad\-core com multithreading:** O pode dar suporte a até 20 estações.      
-   **Seis processadores de núcleo\-com multithreading:** O pode oferecer suporte a até 24 estações.  
  
Com essas informações, selecione uma CPU que atenda aos requisitos de processamento do seu sistema de serviços do MultiPoint. 
> [!NOTE] 
> Se você estiver executando aplicativos com uso intensivo de vídeo, a recomendação será pelo menos um núcleo por estação. 
  
## <a name="selecting-hardware-components"></a>Selecionando componentes de hardware  
Ao criar um sistema de serviços do MultiPoint, considere os seguintes componentes de hardware que você pode precisar:  
  
-   Hardware de vídeo  
  
-   Hardware de estação dos serviços do MultiPoint  
  
    -   *Hubs USB*  
  
    -   Clientes USB zero  
  
    -   Teclados e dispositivos de mouse  
  
    -   Monitores  
  
-   Dispositivos periféricos  
  
    -   Dispositivos de áudio, como alto-falantes e fones de ouvido  
  
    -   Microfones  
  
    -   Dispositivos de armazenamento em massa USB  
  
Quando você tiver selecionado os componentes de hardware para seu sistema de serviços do MultiPoint, certifique-se de obter drivers atuais de 64\-bits para os componentes.  
  
Os tópicos a seguir fornecem informações detalhadas para ajudá-lo a selecionar componentes para seu sistema de serviços do MultiPoint:  
  
[Selecionando hardware de vídeo](#selecting-video-hardware)  
[Selecionando vídeo de\-direta\-dispositivos de estação de cliente USB ou conectados](#BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices)   
[Selecionando outros dispositivos periféricos de estação](#selecting-other-station-peripheral-devices)  
[Selecionando\-RDP em\-LAN\-hardware de estação conectada](#BKMK_SelectingRDP-over-LAN-connectedstationhardware)  
[Selecionando dispositivos de áudio](#selecting-audio-devices)  
  
## <a name="selecting-video-hardware"></a>Selecionando hardware de vídeo
O hardware de vídeo que você selecionar deve dar suporte ao número de monitores que será necessário para o número de usuários que você pretende ter trabalhando em estações de serviços do MultiPoint. Além disso, tipos diferentes de hardware de vídeo podem fornecer uma solução de desempenho mais alta\-para gráficos\-programas intensivos, como conteúdo multimídia.  
  
Selecione o hardware de vídeo que pode dar suporte ao número máximo de monitores para o tipo de desempenho exigido pelo sistema de serviços do MultiPoint. Certifique-se de validar o desempenho do hardware de vídeo que você escolher para garantir que ele atenda aos seus requisitos de desempenho.  
  
> [!NOTE]  
> Você deve instalar um driver de vídeo que ofereça suporte à extensão da área de trabalho em vários monitores.  
  
As opções de hardware de vídeo incluem:  
  
-   Placas de vídeo internas que usam uma interface de barramento PCI ou PCIe  
  
-   Controladores de vídeo externos conectados por USB  
  
As seções a seguir descrevem os recursos de cada um desses tipos de hardware de vídeo. Você pode combinar placas de vídeo internas e controladores de vídeo externos para criar o sistema desejado.  
  
### <a name="internal-video-cards"></a>Placas de vídeo internas  
Uma placa de vídeo interna é conectada\-à placa-mãe no computador. A placa de vídeo interna é uma solução que pode ajudar o desempenho de gráficos\-programas multimídia intensivos. No entanto, uma placa de vídeo interna requer um slot PCI ou PCIe disponível para conectar\-na placa-mãe. Muitas placas de vídeo de alto\-desempenho exigem um slot PCIe, mas há um número limitado de slots PCIe em uma placa-mãe. Você deve saber que tipos de Slots de placa de vídeo estão disponíveis no seu computador para que você possa comprar o tipo correto de placas de vídeo.  
  
O número de monitores que podem se conectar a cada placa de vídeo depende da GPU usada no cartão e do número de portas às quais ele dá suporte, que normalmente varia de 2 a 6.  
  
Ao selecionar placas de vídeo internas, selecione placas de vídeo que dão suporte ao número de monitores necessários para criar o número desejado de estações conectadas diretas ao vídeo. O número máximo de monitores que podem ter suporte é igual ao número de placas de vídeo internas que são conectadas\-à placa-mãe multiplicada pelo número de portas de monitor em cada uma dessas placas de vídeo. Por exemplo, se você tivesse duas placas de vídeo internas e cada placa tivesse duas portas de monitor, você poderia dar suporte a até quatro monitores.    
  
### <a name="external-video-controllers"></a>Controladores de vídeo externos  
Clientes USB zero contêm um controlador de vídeo externo para conectar um monitor ao cliente. O cliente USB zero também pode incluir conexões para fones de ouvido, alto-falantes, um microfone ou outros dispositivos periféricos.  
  
Selecione um cliente USB zero se você quiser habilitar o suporte para monitores adicionais sem abrir o computador ou se quiser dar suporte a mais estações do que as saídas de vídeo disponíveis. Por exemplo, se você tiver quatro monitores conectados\-em placas de vídeo internas e quiser adicionar mais dois monitores, poderá conectar\-em dois controladores de vídeo externos ao computador e ter espaço para mais dois monitores. Dessa maneira, você pode combinar um cliente USB zero com o controlador de vídeo e não usar slots PCI ou PCIe adicionais na placa-mãe.  
  
## <a name="BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices"></a>Selecionando vídeo de\-direta\-dispositivos de estação de cliente USB ou conectados  
Uma estação de serviços do MultiPoint consiste em um hub de estação ou cliente USB com um teclado e mouse conectados\-e um monitor que é conectado\-no computador host ou em um cliente USB zero. Outros dispositivos periféricos podem ser conectados\-no Hub de estação ou cliente USB zero, mas não são necessários para criar a estação do MultiPoint. Esses outros dispositivos periféricos são descritos em [selecionando outros dispositivos periféricos de estação](#selecting-other-station-peripheral-devices).  
  
Os dispositivos que você selecionar para criar uma estação de serviços do MultiPoint devem atender aos requisitos mínimos para trabalhar com os serviços do MultiPoint. Os detalhes sobre os requisitos para os seguintes dispositivos de estação de serviços do MultiPoint são fornecidos neste tópico:  
  
-   [Selecionando hubs USB](#selecting-usb-hubs)  
-   [Selecionando clientes USB zero](#selecting-usb-zero-clients)  
-   [Selecionando teclados e dispositivos de mouse](#selecting-keyboards-and-mouse-devices)  
-   [Selecionando monitores](#selecting-monitors)  
  
### <a name="selecting-usb-hubs"></a>Selecionando hubs USB  
Os hubs USB usados em um sistema MultiPoint Services podem ser um hub USB genérico. Esses hubs normalmente têm quatro ou mais portas USB e permitem que vários dispositivos USB sejam conectados a uma única porta USB no computador. Alguns outros dispositivos, como teclados e monitores de vídeo, também podem incorporar um hub USB ao seu design.  
  
Uma consideração adicional é o uso de um hub com *energia externa* , em vez de um *barramento com\-Hub ligado* . Com um barramento\-Hub, a quantidade de corrente fornecida pelo computador host deve ser suficiente para fornecer energia a todos os dispositivos periféricos que são conectados\-no Hub, sem degradar o desempenho do sistema. Um hub com alimentação externa permite que você conecte mais dispositivos periféricos e forneça energia suficiente a todos eles. O uso de hubs com energia externa pode ajudar a evitar problemas de desempenho, falhas de porta e outros problemas intermitentes.  
  
Ao selecionar um hub USB para seu sistema de serviços do MultiPoint, considere seu uso. O Hub pode ser usado como um *Hub de estação*, um *hub intermediário*ou um *Hub downstream*. Consulte a tabela a seguir para obter descrições sobre cada tipo de Hub. Recomendamos que todos os dispositivos USB sejam USB 2,0 ou posterior.
  
||Smartphone|  
|-|-----------|  
|Hub de estação|Pode ser de barramento\-powered, a menos que dispositivos com alta\-sejam conectados\-a ele ou um hub downstream esteja conectado a ele|  
|Hub intermediário |Deve ser conectado externamente|  
|Hub downstream|Pode ser de energia externa ou de barramento, dependendo dos dispositivos que são conectados\-no Hub|  
|Cabo do extensor USB ativo|Os cabos USB ativos que incluem um hub USB normalmente são alimentados por barramento; Portanto, eles não são recomendados para conectar os hubs de estação ao computador.|  
  
### <a name="selecting-usb-zero-clients"></a>Selecionando clientes USB zero  
Um cliente USB com zero é um hub USB que contém uma saída de vídeo. Portanto, ele permite que um monitor seja conectado ao computador por meio de uma conexão USB. Para obter mais informações sobre como usar clientes USB com zero para vídeo, consulte [selecionando hardware de vídeo](#selecting-video-hardware) neste documento. Um cliente USB zero também pode habilitar a conexão de uma variedade de dispositivos USB e não\-USB para o Hub. Clientes USB zero são produzidos por fabricantes de hardware específicos e exigem a instalação de um dispositivo\-driver específico.  
  
### <a name="selecting-keyboards-and-mouse-devices"></a>Selecionando teclados e dispositivos de mouse  
Os dispositivos de teclado e mouse que você conecta\-na estação normalmente serão dispositivos USB. Alguns clientes USB zero fornecem portas PS\/2. nesse caso, o teclado e o mouse devem usar o PS\/2 para se conectar ao Hub de estação. Você também pode usar o teclado e o mouse do PS\/2 se estiver configurando uma estação de vídeo do PS\/2 Direct\-\-conexão conectada.  
  
Um teclado com um Hub interno pode ser usado como um hub de estação. No entanto, todos os outros dispositivos de estação devem se conectar ao Hub interno usando portas no teclado. Se esse teclado estiver conectado ao computador por meio de outro hub, esse Hub será tratado como um hub intermediário.  
  
Se você estiver usando as estações de tela de divisão\-, convém considerar o uso de um mini teclado que não tenha um teclado numérico, de modo que as duas teclas possam ser ajustadas na frente do monitor.  
  
### <a name="selecting-monitors"></a>Selecionando monitores  
Deve haver um monitor fornecido para cada estação de serviços do MultiPoint, a menos que uma tela de\-de divisão esteja planejada. Os monitores são conectados à placa de vídeo no computador, o cliente USB zero ou o cliente baseado em LAN\-. Qualquer tipo de monitor com suporte na placa de vídeo, cliente USB zero ou cliente baseado em LAN\-pode ser usado, incluindo monitores CRT.  
  
Alguns monitores especiais incluem um cliente baseado em LAN\-interno ou cliente USB zero. Esses monitores normalmente incluirão entrada de áudio\/tomadas de saída e hubs USB internos para conectar teclados e mouses. Eles se conectam ao servidor por meio de uma conexão de LAN ou USB.  
  
#### <a name="display-resolution"></a>Resolução da tela  
A resolução mínima com suporte para a área de exibição de uma estação é 512 x 768 pixels. Se o sistema de serviços do MultiPoint for iniciado e descobrir que a área de exibição de uma estação é menor que a resolução mínima, uma tela em branco será exibida nessa estação e a estação não poderá ser usada.  
  
Se um monitor de exibição for compartilhado por duas estações como estações de tela de divisão\-, o requisito mínimo para a exibição será 1024 x 768, para que as áreas de tela de estação individuais resultantes sejam pelo menos 512 x 768. Para obter a melhor divisão\-experiência do usuário, uma tela larga com um mínimo de resolução de 1600 x 900 é recomendada.  
  
## <a name="selecting-other-station-peripheral-devices"></a>Selecionando outros dispositivos periféricos de estação  
Os serviços do MultiPoint dão suporte a dispositivos periféricos conectados a um hub de estação, um cliente USB zero ou diretamente ao computador. Os dispositivos conectados a um hub de estação serão associados a essa estação específica. Outros dispositivos estão disponíveis para todas as estações quando conectados diretamente ao computador. Os clientes de LAN também podem oferecer suporte a dispositivos periféricos.  
  
> [!IMPORTANT]  
> Um teclado não pode ser conectado a um hub downstream \(por exemplo, um Hub que está conectado a um hub de estação\). Se você conectar um teclado a um hub downstream, todos os periféricos conectados\-ao Hub downstream não estarão mais disponíveis para essa estação. Esse comportamento permite o suporte a correntes\-hubs de estação encadeada.  
  
**Disponível para todas as estações** Um dispositivo USB que é conectado ao computador \(por exemplo, não por meio de um hub de estação\) está disponível para todas as estações. Dependendo do dispositivo, ele pode ser usado por vários usuários ao mesmo tempo ou apenas um usuário pode acessá-lo por vez. A tabela a seguir explica como os dispositivos USB podem ser acessados.  
  
> [!NOTE]  
> A coluna "conectado ao computador host" na tabela refere-se ao comportamento quando o computador executando os serviços do MultiPoint está sendo executado no modo de estação com estações. Se você estiver executando no modo de console, os periféricos conectados em qualquer lugar se comportarão da mesma maneira que um servidor padrão em uma sessão de console.  
  
||Conectado ao computador host|Conectado ao Hub de estação ou ao Hub downstream|  
|-|------------------------------|----------------------------------------------|  
|Teclado|Não funcional, a menos que faça parte de uma estação PS/2. |Disponível para estação individual<br /><br />Não pode ser conectado a um hub downstream|  
|Mouse|Não funcional, a menos que faça parte de uma estação PS/2. |Disponível para estação individual|  
|Palestrante/fone de ouvido|Não funcional, a menos que faça parte de uma estação PS/2.|Disponível para estação individual|  
|Dispositivo de armazenamento USB|Disponível para todas as estações|Disponível para estação individual|  
|Controle de consumidor HID|Não funcional|Disponível para estação individual|  
|Outros dispositivos USB, como câmeras, leitores de documentos e unidades de DVD|Disponível para todas as estações, se houver suporte no Windows Server 2012|Disponível para todas as estações, se houver suporte do Windows Server 2008 R2 Serviços de Área de Trabalho Remota|  
  
## <a name="BKMK_SelectingRDP-over-LAN-connectedstationhardware"></a>Selecionando\-RDP em\-LAN\-hardware de estação conectada  
Qualquer cliente de LAN que possa se conectar a Serviços de Área de Trabalho Remota, usando protocolo RDP, pode se tornar uma estação de serviços de MultiPoint.  
  
Se você quiser que o cliente de LAN seja usado apenas como uma estação de MultiPoint, convém "bloquear" o cliente de LAN. Por exemplo, configure seu cliente fino para que ele possa se conectar somente a uma sessão de serviços do MultiPoint ou Configure seus computadores desktop para que o acesso a ícones da área de trabalho e a itens do menu Iniciar, como um navegador da Web, seja removido para evitar acesso direto à Internet. Você pode fazer essas configurações usando suas ferramentas de configuração de cliente de LAN ou políticas locais ou de grupo.  
  
## <a name="selecting-audio-devices"></a>Selecionando dispositivos de áudio  
É importante certificar-se de que, quando você selecionar dispositivos de áudio, eles possam ser conectados ao Hub de estação, cliente USB zero ou cliente de LAN. Alguns hubs USB, clientes USB zero e clientes de LAN têm uma tomada de áudio analógica que pode ser usada com dispositivos de áudio analógicos tradicionais \(como fones de ouvido ou earbuds\). Os hubs de estação que não têm conectores analógicos podem usar dispositivos de áudio USB.  
  
Se você tiver configurado uma estação de\-de vídeo do\/do PS 2 Direct\-usando portas do PS\/2 na placa-mãe do computador para o teclado e o mouse, deverá usar o áudio analógico na placa-mãe do computador para que o dispositivo de áudio fique disponível para essa estação quando o sistema MultiPoint Services estiver em execução no modo de estação.  
  
Se você não tiver uma estação de vídeo do\/do PS 2 Direct\-\-conectada, o dispositivo de áudio do host na placa-mãe do sistema estará disponível somente quando o sistema de serviços do MultiPoint estiver em execução no modo de console.  