---
title: MultiPoint Stations
description: Saiba mais sobre estações no MultiPoint Services, incluindo as diferentes opções para os usuários
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e747826a7cd84521bc62e48abedf3092bf6d844c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855647"
---
# <a name="multipoint--stations"></a>Estações do multiPoint
Em um ambiente de sistema MultiPoint Services, *estações* são os pontos de extremidade do usuário para se conectar ao computador executando o MultiPoint Services. Cada estação fornece ao usuário uma experiência independente do Windows 10. Há suporte para os seguintes tipos de estação:  
  
-   Estações de vídeo diretamente conectados  
  
-   Estações zero-client-conexão USB (incluindo clientes de USB over Ethernet zero)  
  
-   Estações RDP-over-conectados à rede local (para o cliente avançado ou computadores cliente fino)  
  
PCs completos com o conector do MultiPoint instalado também podem ser monitorados e controlado usando o MultiPoint Dashboard. No Windows 10, o conector do MultiPoint pode ser habilitado por meio do painel de controle para recursos do Windows. 

MultiPoint Services dá suporte a qualquer combinação desses tipos de estação, mas é recomendável que uma estação seja uma estação de vídeo diretamente conectados, que pode servir como a estação principal. O motivo para essa recomendação é ser capaz de antecipar os cenários de suporte. Por exemplo interagir com o sistema 's BIOS antes do MultiPoint Services está em execução.  
  
## <a name="primary-stations-and-standard-stations"></a>Estações primárias e estações padrão  
Uma estação de vídeo diretamente conectados é definida como o *estação principal*. As estações restantes são denominadas *estações padrão*.  
  
A estação principal exibe as telas de inicialização quando o computador é ligado. Ele fornece acesso à configuração do sistema e informações que está disponíveis apenas durante a inicialização de solução de problemas. A estação principal deve ser uma estação de vídeo diretamente conectados. Após a inicialização, você pode usar a estação principal, como qualquer outra estação do MultiPoint.  
  
## <a name="direct-video-connected-stations"></a>Estações de vídeo diretamente conectados  
O computador executando o MultiPoint Services pode conter várias placas de vídeo, cada um deles pode ter uma ou mais portas de vídeos. Isso permite que você conecte monitora várias estações diretamente no computador. Teclados e mouses são conectados por meio de hubs USB que estão associados com cada monitor. Esses hubs são denominados *hubs de estação*. Outros dispositivos periféricos, como dispositivos de armazenamento USB, fones de ouvido ou alto-falantes também podem ser conectados a um hub de estação, e eles estão disponíveis somente para o usuário dessa estação.  
  
> [!IMPORTANT]  
> Deve haver pelo menos um *estação vídeo diretamente conectados* por servidor para atuar como a estação principal para exibir o processo de inicialização, quando o computador é ligado.  
  
![Imagem do layout do sistema com base em USB do MultiPoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**Figura 1** MultiPoint services sistema com quatro estações vídeo diretamente conectados  
  
### <a name="BKMK_PS2stations"></a>Estações de PS/2  
Com o MultiPoint Services, você pode mapear o PS/teclado e mouse 2 na placa-mãe para um monitor conectado de vídeo direto para criar uma estação de PS/2. Áudio analógico na placa-mãe de alta definição é o áudio associado com esse tipo de estação. Isso não se aplica a computadores em que não há nenhum pinos PS/2 na placa-mãe.  
  
## <a name="usb-zero-client-connected-stations"></a>Estações com zero-client-conexão USB  
Estações conectados por USB zero-client utilizam uma *zero cliente USB* como um hub de estação. Os clientes USB zero, às vezes, são chamados de um hub multifuncional com vídeo. Eles são um hub que se conecta ao computador por meio de um cabo USB, e esses hubs normalmente dão suporte a um vídeo monitor, um mouse e teclado (PS/2 ou USB), áudio e mais dispositivos USB. Este guia se refere a esses hubs especializadas como USB zero clientes.  
  
O diagrama a seguir mostra um sistema MultiPoint server com uma estação principal (vídeo direto conectado estação) e dois adicionais zero cliente USB conectado estações.  
  
![Estações conectadas do zero cliente USB](./media/WMS11_diagram7.gif)  
  
**Figura 2** sistema MultiPoint Services com uma estação principal e duas estações conectada com zero cliente USB  
  
### <a name="usb-over-ethernet-zero-clients"></a>Clientes de USB over Ethernet zero  
Clientes de USB over Ethernet zero são uma variação de clientes USB zero que enviam USB através de LAN para o sistema MultiPoint Services. Esses tipos de clientes USB zero funcionam da mesma forma outro USB zero clientes, mas não são limitados por limites máximos de comprimento de cabo USB. Clientes de USB over Ethernet zero não são clientes finos tradicionais, e eles são exibidos como dispositivos virtuais de USB no sistema MultiPoint Services. Ao usar esses dispositivos, consulte o fabricante do dispositivo para específicos de desempenho e recomendações de planejamento do site. A maioria dos dispositivos têm um plug-in de terceiros para o MultiPoint Manager que permite que você associar e conectar dispositivos ao sistema MultiPoint Services.  
  
## <a name="rdp-over-lan-connected-stations"></a>Estações conectadas à RDP através de LAN  
Clientes finos e tradicionais para desktop, laptop ou tablet PCs, podem se conectar ao computador executando o MultiPoint Services por meio da rede de área local (LAN) usando o protocolo de área de trabalho remota (RDP) ou um protocolo proprietário e o protocolo de área de trabalho remota Provedor. Conexões RDP fornecem uma experiência de usuário final que é muito semelhante a qualquer outra estação do MultiPoint, mas faz uso de hardware do computador cliente local. Saiba mais sobre nossa área de trabalho aplicativos remotos disponíveis para o Android, iOS, Mac e Windows no [clientes de área de trabalho remota](../remote-desktop-services/clients/remote-desktop-clients.md). 
  
Os clientes e dispositivos que estão executando o Microsoft RemoteFX podem fornecer uma experiência multimídia avançada ao aproveitar os processador e o vídeo de recursos de hardware do cliente fino local ou do computador para fornecer o vídeo de alta definição através da rede.  
  
Se você tiver clientes de rede local existentes do MultiPoint Services pode fornecer uma maneira rápida e econômica para atualizar simultaneamente todos os seus usuários uma experiência do Windows 10.  
  
De uma perspectiva de implantação e administração, as seguintes diferenças existem ao usar estações RDP-over-conectados à rede local:  
  
-   Não limitado a físicas distâncias de conexão USB  
  
-   Potencial para reutilizar o hardware mais antigos como estações  
  
-   Mais fácil de ser dimensionado para um número maior de estações. Qualquer cliente na sua rede potencialmente pode ser usado como uma estação remota  
  
-   Nenhum hardware de solução de problemas por meio do console do Gerenciador do MultiPoint  
  
-   Nenhuma funcionalidade de tela dividida.  
  
    Para obter mais informações, consulte [estações com tela dividida](#a-namebkmksplitscreenstationsasplit-screen-stations) mais adiante neste tópico  
  
-   Nenhuma estação renomeação ou configurar o logon automático por meio do console do Gerenciador do MultiPoint  
  
![Estação conectada com zero cliente USB](./media/Diagram1.gif)  
  
**Figura 3** sistema MultiPoint Services com estações RDP-over-conectados à rede local  
  
## <a name="additional-configuration-options"></a>Opções de configuração adicionais  
  
### <a name="BKMK_SplitscreenStations"></a>Estações com tela dividida  
MultiPoint Services oferece uma opção de tela de divisão em computadores com estações de vídeo-conectados diretamente ou conectados por USB zero-client estações. Uma tela dividida fornece a capacidade de criar uma estação adicional por monitor. Em vez de exigir dois monitores, você pode usar um monitor com duas configurações de hub de estação para criar duas estações com um monitor. Você pode aumentar o número de estações disponíveis rapidamente sem precisar adquirir monitores adicionais, os clientes de USB de zero ou placas de vídeo.  
  
Os benefícios do uso de uma estação de tela dividida podem incluir:  
  
-   Reduzindo o custo e espaço ao acomodar mais usuários em um sistema MultiPoint Services.  
  
-   Permitindo que dois usuários colaborem lado a lado em um projeto.  
  
-   Permitindo que um professor demonstrar um procedimento em uma estação enquanto um aluno acompanha na outra estação.  
  
Monitor que tem uma resolução 1024 x 768 ou posteriores podem ser divididos em duas telas de estação de estação de quaisquer serviços do MultiPoint. Para obter a melhor experiência de usuário de tela divisão, recomenda-se uma tela ampla com uma resolução mínima de 1600 x 900. Um teclado mini sem um teclado numérico também é recomendável para permitir que os dois teclados caber na frente do monitor.  
  
Para criar estações com tela dividida, configurar uma estação de vídeo-conectados diretamente ou conectados por USB zero-client. Em seguida, adiciona um hub de estação adicionais conectando um teclado e mouse para um hub USB que está conectado ao servidor. Em seguida, você pode converter a estação de em duas estações usando o Gerenciador do MultiPoint para dividir a tela e mapear o novo hub a metade do monitor. A metade esquerda da tela torna-se em uma estação e metade direita se torna uma segunda estação.  
  
Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação direita.  
  
![Estações de tela dividida](./media/WMS_diagram3.gif)  
  
**Figura 4** sistema MultiPoint Services com estações com tela dividida  
  
## <a name="BKMK_StationTypeComparison"></a>Comparação de tipo de estação  
  
||Vídeo direto conectado|Zero cliente USB conectado|RDP através de LAN conectada|  
|-|--------------------------|-----------------------------|----------------------------|  
|Desempenho do vídeo|Recomendado para melhor desempenho de vídeo||Usar clientes finos compatíveis com RemoteFX para melhor qualidade de vídeo em menos largura de banda de rede|  
|Limitações físicas|Limitado pelo tamanho do vídeo cabo e hub USB e comprimento de cabo (tamanho máximo recomendado 15 medidor)|Limitado pelo hub USB e o comprimento de cabo (tamanho máximo recomendado 15 medidor)|Limitado pela distribuição de LAN|  
|Número de estações permitidas |Limitado pelo número de slots de PCIe disponíveis na placa-mãe vezes as portas de vídeo por placa de vídeo|Número total pode ser limitado pelo fabricante do cliente USB zero (para obter mais informações, consulte a observação após esta tabela).|Limitado por portas disponíveis no comutador de rede|  
|Tela dividida|Sim|Sim|Não|  
|Estação de status de periféricos de estação do multiPoint Manager, configuração de logon automático, renomeando|Sim|Sim|Não|  
|Acesso aos menus de inicialização do servidor|Sim|Não|Não|  
  
> [!NOTE]  
> O número total de clientes USB zero que estão conectados ao servidor pode ser limitado pelo fabricante ou a capacidade de hardware do computador executando o MultiPoint Services.