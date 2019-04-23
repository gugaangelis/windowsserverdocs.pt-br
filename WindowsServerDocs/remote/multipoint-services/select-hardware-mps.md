---
title: Selecionar o hardware para o sistema MultiPoint Services
description: Considerações de hardware para o MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e74961a2-bd38-48ae-b1c0-4b3eff761b4a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 969ab0e97b5456c71a43cc14bd82204481bdcc42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835217"
---
# <a name="selecting-hardware-for-your-multipoint-services-system"></a>Selecionar o hardware para o sistema MultiPoint Services
Quando você cria um sistema MultiPoint Services, você deve selecionar um computador que atenda aos requisitos de sistema do Windows Server 2016. Se você estiver decidindo quais componentes para selecionar, considere o seguinte:  
  
-   O intervalo de preço de destino de sua solução completa.  
  
-   Os tipos de cenários de uso que você pode esperar para o sistema MultiPoint Services, como se os usuários estão executando programas multimídia, usando o processamento de texto ou programas de produtividade ou navegação na Internet.  
  
-   Se seu cenário tem demandas de processamento ou de memória grandes.  
  
-   O número de usuários que pode estar usando o sistema ao mesmo tempo. Se você planeja ter vários usuários em seu sistema ao mesmo tempo, ou usuários que usam o sistema intensivo de programas, você deve planejar mais potência de computação para seu sistema.  
  
-   O tipo de estações. Quantas portas USB ou portas de vídeo que você precisa?  
  
-   Planos de expansão futura. Você planeja adicionar estações no sistema MultiPoint Services em uma data posterior? Você terá suficiente slots de placa de vídeo, portas USB ou toca em rede? Quantos usuários adicionais o hardware necessário dar suporte?  
  
-   Layout físico. Para obter mais informações, consulte [planejamento do Site do MultiPoint Services](MultiPoint-services-Site-Planning.md).  
  
Normalmente, um sistema MultiPoint Services inclui os seguintes componentes:  
  
-   Um computador que está executando o MultiPoint Services, que inclui uma CPU, RAM, discos rígidos e placas de vídeo.  
  
-   Um monitor, hub de estação, teclado e mouse para cada estação.  
  
-   Dispositivos periféricos opcionais para as estações do MultiPoint Services, incluindo alto-falantes, fones de ouvido, microfones ou dispositivos de armazenamento que estão disponíveis somente para o usuário da estação.  
  
-   Opcionais dispositivos periféricos que estão disponíveis para todos os usuários do sistema MultiPoint Services, conectado diretamente ao computador host, como impressoras, discos rígidos externos e dispositivos de armazenamento USB.  
  
Use as seguintes informações para tomar decisões de hardware:  
  
-   [Selecionando uma CPU](#selecting-a-cpu)  
-   [Selecionar os componentes de hardware](#selecting-hardware-components)  
  
## <a name="selecting-a-cpu"></a>Selecionando uma CPU  
Um sistema MultiPoint Services é um múltiplo\-ambiente do usuário, com todos os usuários conectados a um único computador host. Isso aumenta o uso da CPU, porque todos os usuários compartilham o mesmo computador. Algumas tarefas, como programas multimídia \(por exemplo, players de mídia ou vídeo\-software de edição\), têm maior demanda de processamento. Portanto, certifique-se selecionar uma CPU que pode lidar com os requisitos de processamento para o número de usuários e tipos de cenários de usuário que ele será necessário para dar suporte.  
  
Requer o multiPoint Services x64\-com base em CPU e deve atender os requisitos de sistema para o computador, conforme descrito em [requisitos de Hardware e recomendações de desempenho](Hardware-Requirements-and-Performance-Recommendations.md).  
  
Os seguintes tipos de processadores foram testados para ser usado em um sistema MultiPoint Services com alta\-exigem programas de processamento, como programas multimídia:  
  
-   **Dupla\-processador core:** Pode dar suporte a até 8 estações.  
-   **Quad\-processador core:** Pode dar suporte a até 16 estações.
-   **Quad\-núcleos de processador com multithreading:** Pode dar suporte a até 20 estações.      
-   **Seis\-núcleos de processador com multithreading:** Pode dar suporte a até 24 estações.  
  
Com essas informações, selecione uma CPU que atende aos requisitos de processamento para o seu sistema MultiPoint Services. 
> [!NOTE] 
> Se você estiver executando aplicativos com uso intensivo de vídeo a recomendação é pelo menos um núcleo por estação. 
  
## <a name="selecting-hardware-components"></a>Selecionar os componentes de hardware  
Quando você estiver criando um sistema MultiPoint Services, considere os seguintes componentes de hardware que podem ser necessários:  
  
-   Hardware de vídeo  
  
-   Hardware da estação do multiPoint Services  
  
    -   *Hubs USB*  
  
    -   Clientes USB zero  
  
    -   Teclados e mouses  
  
    -   Monitores  
  
-   Dispositivos periféricos  
  
    -   Dispositivos de áudio como fones de ouvido e alto-falantes  
  
    -   Microfones  
  
    -   Dispositivos de armazenamento em massa USB  
  
Quando você tiver selecionado os componentes de hardware para o seu sistema MultiPoint Services, certifique-se de que você obtenha atual, 64\-drivers para os componentes de bits.  
  
Os tópicos a seguir fornecem informações detalhadas para ajudá-lo a selecionar componentes para seu sistema MultiPoint Services:  
  
[Selecionando hardware de vídeo](#selecting-video-hardware)  
[Seleção direta\-vídeo\-conectado ou USB zero dispositivos de estação de cliente](#BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices)   
[Selecionando outros dispositivos periféricos de estação](#selecting-other-station-peripheral-devices)  
[Selecionando o RDP\-pela\-LAN\-conectado hardware da estação](#BKMK_SelectingRDP-over-LAN-connectedstationhardware)  
[Selecionar dispositivos de áudio](#selecting-audio-devices)  
  
## <a name="selecting-video-hardware"></a>Selecionando hardware de vídeo
O hardware de vídeo que você selecionar deve dar suporte o vários monitores que serão necessárias para o número de usuários que você pretende ter trabalho em estações do MultiPoint Services. Além disso, os diferentes tipos de hardware de vídeo podem fornecer uma maior\-solução de desempenho para gráficos\-programas intensivos, como o conteúdo de multimídia.  
  
Selecione o hardware de vídeo que dão suporte ao número máximo de monitores para o tipo de desempenho que requer que seu sistema MultiPoint Services. Certifique-se de que você valide o desempenho do hardware de vídeo que você escolher para garantir que ele atenda aos seus requisitos de desempenho.  
  
> [!NOTE]  
> Você deve instalar um driver de vídeo que oferece suporte à extensão de sua área de trabalho em vários monitores.  
  
Opções de hardware de vídeo incluem:  
  
-   Placas de vídeo internas que usam uma PCI ou uma interface de barramento PCIe  
  
-   Controladores de vídeo externo conectado por USB  
  
As seções a seguir descrevem os recursos de cada um desses tipos de hardware de vídeo. Você pode combinar as placas de vídeo internas e externo controladores de vídeo para criar o sistema que você deseja.  
  
### <a name="internal-video-cards"></a>Placas de vídeo internas  
Está conectada a uma placa de vídeo interna\-na placa-mãe no computador. A placa de vídeo interna é uma solução que pode ajudar no desempenho de gráficos\-programas multimídia com uso intensivo. No entanto, uma placa de vídeo interna requer um slot de PCI ou PCIe conectem\-na placa-mãe. Muitos alta\-placas de vídeo de desempenho exigem um slot de PCIe, mas há um número limitado de PCIe slots em uma placa-mãe. Você deve saber que tipo de slots de placa de vídeo estão disponíveis em seu computador para que você pode adquirir o tipo correto de placas de vídeo.  
  
O número de monitores que pode se conectar a cada placa de vídeo depende da GPU que é usada no cartão e o número de portas que ele suporta, que normalmente varia de 2 a 6.  
  
Quando você seleciona as placas de vídeo internas, selecione as placas de vídeo que dão suporte o vários monitores necessários para criar o número desejado de diretos estações conectadas vídeos. O número máximo de monitores que podem ter suporte é igual ao número de placas de vídeo internas que estejam conectados\-na placa-mãe multiplicado pelo número de portas de monitor em cada um desses placas de vídeo. Por exemplo, se você tivesse duas placas de vídeo internas e cada cartão tinha duas portas de monitor, você poderia dar suporte até quatro monitores.    
  
### <a name="external-video-controllers"></a>Controladores de vídeo externo  
Os clientes USB zero contêm um controlador de vídeo externo para um monitor de conexão para o cliente. O zero cliente USB também pode incluir conexões para fones de ouvido, alto-falantes, um microfone ou outros dispositivos periféricos.  
  
Selecione um USB zero cliente se você quiser habilitar o suporte para monitores adicionais sem abrir o computador, ou se você quiser dar suporte a mais estações de saídas de vídeos disponíveis. Por exemplo, se você já tinha quatro monitores conectados\-em placas de vídeo internas, e você deseja adicionar dois monitores mais, você pode conectar\-em dois controladores de vídeos externos ao computador e ter espaço para dois monitores mais. Dessa forma, você pode combinar um zero cliente USB com o controlador de vídeo e usar slots PCI ou PCIe adicionais na placa-mãe.  
  
## <a name="BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices"></a>Seleção direta\-vídeo\-conectado ou USB zero dispositivos de estação de cliente  
Uma estação do MultiPoint Services consiste em um hub de estação ou USB zero cliente com um teclado e mouse conectados\-no e um monitor conectado\-no computador host ou para um zero cliente USB. Podem ser conectados a outros dispositivos periféricos\-na estação hub ou USB zero cliente, mas eles não são necessários para criar estação do MultiPoint. Esses outros dispositivos periféricos são descritos em [selecionando outros dispositivos periféricos de estação](#selecting-other-station-peripheral-devices).  
  
Os dispositivos que você selecionar para criar uma estação do MultiPoint Services devem atender aos requisitos mínimos para trabalhar com o MultiPoint Services. Detalhes sobre os requisitos para os seguintes dispositivos de estação do MultiPoint Services são fornecidos neste tópico:  
  
-   [Selecionando hubs USB](#selecting-usb-hubs)  
-   [Selecionando USB zero clientes](#selecting-usb-zero-clients)  
-   [Selecionar dispositivos de mouse e teclados](#selecting-keyboards-and-mouse-devices)  
-   [Seleção de monitores](#selecting-monitors)  
  
### <a name="selecting-usb-hubs"></a>Selecionando hubs USB  
Os hubs USB que são usados em um sistema MultiPoint Services podem ser um hub USB genérico. Tais hubs normalmente têm quatro ou mais portas USB e permitem que vários dispositivos USB sejam conectados a uma única porta USB no computador. Alguns outros dispositivos, como teclados e monitores de vídeo, também podem incorporar um hub USB em seu design.  
  
Uma consideração adicional é o uso de um *alimentados externamente* hub, em vez de uma *barramento\-alimentado* hub. Com um barramento\-hub alimentado, a quantidade de atual que é fornecido pelo host do computador deve ser suficiente para fornecer a potência para todos os dispositivos periféricos são conectados\-para o hub, sem comprometer o desempenho do sistema. Um hub alimentado externamente permite que você se conecte dispositivos periféricos mais e fornecer energia suficiente para todos eles. O uso de hubs alimentados externamente pode ajudar a evitar problemas de desempenho, falhas de porta e outros problemas de intermitentes.  
  
Ao selecionar um hub USB para seu sistema MultiPoint Services, considere seu uso. O hub pode ser usado como um *hub de estação*, um *hub intermediário*, ou uma *hub downstream*. Consulte a tabela a seguir para obter descrições sobre cada tipo de hub. É recomendável que todos os dispositivos USB seja USB 2.0 ou posterior.
  
||Plataforma|  
|-|-----------|  
|Hub de estação|Pode ser barramento\-fornecida, a menos que alta\-dispositivos ligados serão conectados\-para ele ou um hub de downstream serão conectadas a ele|  
|Hub intermediário |Devem ser alimentados externamente|  
|Downstream Hub|Podem ser alimentados externamente ou barramento alimentado, dependendo dos dispositivos que estão conectados\-para o hub|  
|Cabo extensor Active Directory|Cabos USB Active Directory que incluem um hub USB costumam ser alimentado pelo; barramento Portanto, eles não são recomendados para se conectar a hubs de estação para o computador.|  
  
### <a name="selecting-usb-zero-clients"></a>Selecionando USB zero clientes  
Um zero cliente USB é um hub USB que contém uma saída de vídeo. Portanto, ele permite que um monitor seja conectado ao computador por meio de uma conexão USB. Para obter mais informações sobre como usar clientes USB zero para vídeo, consulte [selecionando o hardware de vídeo](#selecting-video-hardware) neste documento. Um zero cliente USB também pode habilitar a conexão de uma variedade de USB e não\-dispositivos USB ao hub. Os clientes USB zero produzidos pelos fabricantes de hardware específico e eles exigem a instalação de um dispositivo\-driver específico.  
  
### <a name="selecting-keyboards-and-mouse-devices"></a>Selecionar dispositivos de mouse e teclados  
Os dispositivos de teclado e mouse que você conecte\-em estação normalmente será dispositivos USB. Alguns USB zero clientes fornecem PS\/2 portas, caso em que o teclado e mouse deve usar PS\/2 para se conectar ao hub de estação. Você também pode usar um PS\/2 do teclado e mouse se você estiver configurando um PS\/2 direto\-vídeo\-estação conectada.  
  
Um teclado com um hub interno pode ser usado como um hub de estação. No entanto, todos os outros dispositivos de estação devem se conectar ao hub interno por meio de portas no teclado. Se tal um teclado estiver conectado ao computador por meio do hub de outro, esse hub será tratado como um hub intermediário.  
  
Se você estiver usando divisão\-estações com tela, você talvez queira considerar usar um teclado minidespejos que não tem um teclado numérico para que os dois teclados podem caber na frente do monitor.  
  
### <a name="selecting-monitors"></a>Seleção de monitores  
Deve haver um monitor fornecido para cada estação do MultiPoint Services, a menos que uma divisão\-tela está planejada. Monitores são conectados a placa de vídeo no computador, o zero cliente USB ou LAN\-com base em cliente. Qualquer tipo de monitor que é compatível com a placa de vídeo, zero cliente USB ou LAN\-cliente baseado em pode ser usado, incluindo monitores CRT.  
  
Alguns monitores especiais incluem uma LAN interna\-com base em zero cliente USB ou cliente. Esses monitores normalmente incluirão a entrada de áudio\/pinos e internos hubs USB para conectar-se de teclados e mouses de saída. Eles se conectam ao servidor por meio de um USB ou uma conexão de rede local.  
  
#### <a name="display-resolution"></a>Resolução de vídeo  
A resolução mínima com suporte para a área de exibição de uma estação é 512 x 768 pixels. Se o sistema MultiPoint Services é iniciado e descobre que a área de exibição de uma estação é menor que a resolução mínima, será exibida uma tela em branco nessa estação e estação não será utilizável.  
  
Se um monitor de vídeo vai ser compartilhado por duas estações como divisão\-tela estações, o requisito mínimo para a exibição é de 1024 x 768, para que as áreas da tela resultante estação individuais são pelo menos 512 x 768. Para obter a melhor dividir\-experiência de usuário da tela, uma tela ampla com um mínimo de resolução de 1600 x 900 é recomendada.  
  
## <a name="selecting-other-station-peripheral-devices"></a>Selecionando outros dispositivos periféricos de estação  
MultiPoint Services dá suporte a dispositivos periféricos que estão conectados a um hub de estação, um zero cliente USB, ou diretamente ao computador. Conectado a um hub de estação de dispositivos será associados essa estação específica. Outros dispositivos estão disponíveis para cada estação quando conectado diretamente ao computador. Clientes de rede local também podem dar suporte a dispositivos periféricos.  
  
> [!IMPORTANT]  
> Um teclado não pode ser conectado a um hub de downstream \(por exemplo, um hub que estiver conectado a um hub de estação\). Se você conectar um teclado para um hub de downstream, todos os periféricos são conectados\-para o hub de downstream não estarão disponíveis para essa estação. Esse comportamento permite que o suporte de margarida\-encadeadas hubs de estação.  
  
**Disponível em todas as estações** dispositivo USB de um que é conectado ao computador \(por exemplo, não por meio de um hub de estação\) está disponível em todas as estações. Dependendo do dispositivo, ele pode ser usado por vários usuários ao mesmo tempo, ou apenas um usuário pode acessá-lo por vez. A tabela a seguir explica como os dispositivos USB podem ser acessados.  
  
> [!NOTE]  
> A coluna da tabela "Conectado ao computador Host" refere-se ao comportamento quando o computador executando o MultiPoint Services está em execução no modo de estação com estações. Se você estiver executando no modo de console, os periféricos que estão conectados em qualquer lugar se comportam da mesma maneira que um servidor padrão em uma sessão de console.  
  
||Conectado ao computador Host|Conectado ao Downstream ou Hub de estação|  
|-|------------------------------|----------------------------------------------|  
|Teclado|Não funcional, a menos que ele seja parte de uma estação de PS/2. |Disponível para a estação individual<br /><br />Não pode ser conectado a um hub de downstream|  
|Mouse|Não funcional, a menos que ele seja parte de uma estação de PS/2. |Disponível para a estação individual|  
|Fones de ouvido/alto-falante|Não funcional, a menos que ele seja parte de uma estação de PS/2.|Disponível para a estação individual|  
|Dispositivo de armazenamento USB|Disponível em todas as estações|Disponível para a estação individual|  
|Controle do consumidor HID|Não é funcional|Disponível para a estação individual|  
|Outros dispositivos USB, como câmeras, os leitores de documento e unidades de DVD|Disponível em todas as estações se compatível com o Windows Server 2012|Disponível em todas as estações se compatível com o Windows Server 2008 R2 Remote Desktop Services|  
  
## <a name="BKMK_SelectingRDP-over-LAN-connectedstationhardware"></a>Selecionando o RDP\-pela\-LAN\-conectado hardware da estação  
Qualquer cliente de rede local que pode se conectar aos serviços de área de trabalho remota, usando o protocolo de área de trabalho remota, pode se tornar uma estação do MultiPoint Services.  
  
Se você quiser que o cliente de rede local para ser usado apenas como uma estação do MultiPoint, você talvez queira "bloquear" cliente de sua rede local. Por exemplo, configure seu cliente fino para que ele só pode se conectar a uma sessão do MultiPoint Services ou configurar seus computadores desktop, para que os itens que o acesso a ícones da área de trabalho e Menu Iniciar, como um navegador da web for removido para impedir o acesso direto à Internet. Você pode fazer essas configurações usando suas ferramentas de configuração de cliente de rede local ou grupo ou diretivas locais.  
  
## <a name="selecting-audio-devices"></a>Selecionar dispositivos de áudio  
É importante certificar-se de que quando você selecionar dispositivos de áudio, eles podem ser conectados à hub de estação, zero cliente USB ou cliente de rede local. Alguns hubs USB, os clientes USB zero e clientes de LAN têm uma tomada de áudio analógica que pode ser usada com dispositivos de áudio analógicos tradicionais \(como fones de ouvido ou fones de ouvido\). Hubs de estação que não têm conectores analógicas podem usar dispositivos de áudio USB.  
  
Se você tiver configurado um PS\/2 direto\-vídeo\-estação conectada por meio de PS\/2 portas na placa-mãe do computador para o teclado e mouse, você deve usar o áudio analógico na placa-mãe do computador no ordem para o dispositivo de áudio estejam disponíveis para essa estação quando o sistema MultiPoint Services está em execução no modo de estação.  
  
Se você não tiver um PS\/2 direto\-vídeo\-estação conectada, o dispositivo de áudio de host na placa-mãe do sistema estará disponível somente quando o sistema MultiPoint Services está em execução no modo de console.  