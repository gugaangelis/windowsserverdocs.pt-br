---
title: MultiPoint Stations
description: Saiba mais sobre as estações nos serviços do MultiPoint, incluindo as diferentes opções para os usuários
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6167919b31621aef51707bc5b7acf9c07ba795e3
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517671"
---
# <a name="multipoint--stations"></a>Estações multiponto
Em um ambiente de sistema de serviços do MultiPoint, as *estações* são os pontos de extremidade do usuário para se conectarem ao computador que executa os serviços do MultiPoint. Cada estação fornece ao usuário uma experiência independente do Windows 10. Há suporte para os seguintes tipos de estação:

-   Estações conectadas diretamente a vídeo

-   USB-nenhuma estação conectada a cliente (incluindo clientes USB over-Ethernet)

-   Estações conectadas por RDP via LAN (para computadores cliente avançados ou cliente fino)

Computadores completos que têm o conector do MultiPoint instalado também podem ser monitorados e controlados usando o painel do MultiPoint. No Windows 10, o conector do MultiPoint pode ser habilitado por meio do painel de controle para recursos do Windows.

Os serviços do MultiPoint oferecem suporte a qualquer combinação desses tipos de estação, mas é recomendável que uma estação seja uma estação conectada por vídeo direto, que possa servir como a estação principal. A razão para essa recomendação é poder prever os cenários de suporte. Por exemplo, para interagir com o BIOS do sistema antes de os serviços do MultiPoint serem executados.

## <a name="primary-stations-and-standard-stations"></a>Estações primárias e estações padrão
Uma estação conectada diretamente ao vídeo é definida como a *estação principal*. As estações restantes são chamadas de *estações padrão*.

A estação principal exibe as telas de inicialização quando o computador está ligado. Ele fornece acesso à configuração do sistema e às informações de solução de problemas disponíveis somente durante a inicialização. A estação principal deve ser uma estação conectada diretamente ao vídeo. Após a inicialização, você pode usar a estação primária como qualquer outra estação do MultiPoint.

## <a name="direct-video-connected-stations"></a>Estações conectadas diretamente a vídeo
O computador que executa os serviços do MultiPoint pode conter várias placas de vídeo, cada uma delas pode ter uma ou mais portas de vídeo. Isso permite que você conecte monitores para várias estações diretamente no computador. Teclados e mouses são conectados por meio de hubs USB associados a cada monitor. Esses hubs são chamados de *hubs de estação*. Outros dispositivos periféricos, como alto-falantes, fones de ouvido ou dispositivos de armazenamento USB também podem ser conectados a um hub de estação e estão disponíveis somente para o usuário dessa estação.

> [!IMPORTANT]
> Deve haver pelo menos uma *estação conectada por vídeo direto* por servidor para atuar como a estação principal para exibir o processo de inicialização quando o computador é ligado.

![Imagem do layout do sistema baseado em USB dos serviços do MultiPoint](./media/WMSMultiPointServerUSBSystemLayout.gif)

**Figura 1** Sistema de serviços do MultiPoint com quatro estações conectadas diretamente ao vídeo

### <a name="ps2-stations"></a><a name="BKMK_PS2stations"></a>Estações PS/2
Com os serviços do MultiPoint, você pode mapear o teclado e o mouse do PS/2 na motherboard para um monitor de vídeo conectado diretamente para criar uma estação PS/2. Áudio analógico de alta definição na placa-mãe é o áudio associado a esse tipo de estação. Isso não se aplica a computadores nos quais não há conectores PS/2 na placa-mãe.

## <a name="usb-zero-client-connected-stations"></a>USB-nenhuma estação conectada ao cliente
USB-as estações conectadas a cliente com zero utilizam um *cliente USB zero* como um hub de estação. Os clientes USB com zero, às vezes, são chamados de um hub multifuncional com vídeo. Eles são um Hub que se conecta ao computador por meio de um cabo USB, e geralmente esses hubs dão suporte a um monitor de vídeo, um mouse e um teclado (PS/2 ou USB), áudio e dispositivos USB adicionais. Este guia refere-se a esses hubs especializados como clientes USB zero.

O diagrama a seguir mostra um sistema do MultiPoint Server com uma estação primária (estação conectada ao vídeo direto) e duas estações conectadas de cliente USB zero adicionais.

![USB zero-estações conectadas ao cliente](./media/WMS11_diagram7.gif)

**Figura 2** Sistema de serviços do MultiPoint com uma estação primária e duas estações conectadas a cliente com USB zero

### <a name="usb-over-ethernet-zero-clients"></a>Clientes USB over-Ethernet nulos
Clientes USB over-Ethernet são uma variação de clientes USB de zero que enviam USB por LAN para o sistema de serviços do MultiPoint. Esses tipos de clientes USB zero funcionam de forma semelhante a outros clientes USB zero, mas não são limitados por máximos de comprimento de cabo USB. Clientes USB over-Ethernet não são clientes finos tradicionais e aparecem como dispositivos USB virtuais no sistema MultiPoint Services. Ao usar esses dispositivos, consulte o fabricante do dispositivo para obter recomendações específicas de planejamento de desempenho e site. A maioria dos dispositivos tem um plug-in de terceiros para o MultiPoint Manager que permite associar e conectar dispositivos ao sistema MultiPoint Services.

## <a name="rdp-over-lan-connected-stations"></a>Estações conectadas RDP via LAN
Clientes finos e computadores desktop, laptop ou Tablet tradicionais podem se conectar ao computador que executa os serviços do MultiPoint por meio da LAN (rede local) usando protocolo RDP (RDP) ou um protocolo proprietário e o provedor de protocolo RDP. As conexões RDP fornecem uma experiência do usuário final muito semelhante a qualquer outra estação do MultiPoint, mas utiliza o hardware do computador cliente local. Saiba mais sobre nossos aplicativos de área de trabalho remota disponíveis para Android, iOS, Mac e Windows em [clientes área de trabalho remota](../remote-desktop-services/clients/remote-desktop-clients.md).

Os clientes e dispositivos que estão executando o Microsoft RemoteFX podem fornecer uma experiência de multimídia avançada aproveitando os recursos de hardware do processador e do vídeo do cliente fino local ou do computador para fornecer vídeo de alta definição na rede.

Se você tiver clientes de LAN existentes, os serviços do MultiPoint poderão fornecer uma maneira rápida e econômica de atualizar simultaneamente todos os seus usuários para uma experiência do Windows 10.

Do ponto de vista da implantação e da administração, existem as seguintes diferenças quando você usa estações conectadas por RDP em LAN:

-   Não limitado a distâncias de conexão USB física

-   Potencial para reutilizar o hardware de computador mais antigo como estações

-   Mais fácil de dimensionar para um número maior de estações. Qualquer cliente em sua rede pode potencialmente ser usado como uma estação remota

-   Sem solução de problemas de hardware por meio do console do MultiPoint Manager

-   Nenhuma funcionalidade de tela de divisão.

    Para obter mais informações, consulte [estações de tela de divisão](#split-screen-stations) mais adiante neste tópico

-   Nenhuma estação renomeie ou configure o logon automático por meio do console do MultiPoint Manager

![Estação conectada com zero cliente USB](./media/Diagram1.gif)

**Figura 3** Sistema de serviços do MultiPoint com estações conectadas por RDP via LAN

## <a name="additional-configuration-options"></a>Opções de configuração adicionais

### <a name="split-screen-stations"></a>Estações com tela dividida
Os serviços do MultiPoint oferecem uma opção de tela dividida em computadores com estações conectadas diretamente a vídeo ou estações USB-zero-conexão com o cliente. Uma tela dividida fornece a capacidade de criar uma estação adicional por monitor. Em vez de exigir dois monitores, você pode usar um monitor com duas configurações de Hub de estação para criar duas estações com um monitor. Você pode aumentar rapidamente o número de estações disponíveis sem comprar monitores adicionais, clientes USB-zero ou placas de vídeo.

Os benefícios de usar uma estação de tela de divisão podem incluir:

-   Reduzir o custo e o espaço acomodando mais usuários em um sistema de serviços do MultiPoint.

-   Permitir que dois usuários colaborem lado a lado em um projeto.

-   Permitir que um professor demonstre um procedimento em uma estação enquanto o aluno segue na outra estação.

Qualquer monitor de estação de serviços do MultiPoint que tenha uma resolução de 1024x768 ou superior pode ser dividido em duas telas de estação. Para obter a melhor experiência de usuário de tela dividida, é recomendável uma tela larga com uma resolução mínima de 1600. Também é recomendável um mini Keyboard sem um teclado numérico para permitir que os dois teclados caibam na frente do monitor.

Para criar estações de tela de divisão, você configura uma estação conectada por um vídeo direto ou USB-zero-conexão com o cliente. Em seguida, você adiciona um hub de estação adicional conectando um teclado e um mouse a um hub USB que está conectado ao servidor. Em seguida, você pode converter a estação em duas estações usando o MultiPoint Manager para dividir a tela e mapear o novo hub para metade do monitor. A metade esquerda da tela se torna uma estação e a metade direita se torna uma segunda.

Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação à direita.

![Estações com tela dividida](./media/WMS_diagram3.gif)

**Figura 4** Sistema de serviços do MultiPoint com estações de tela dividida

## <a name="station-type-comparison"></a><a name="BKMK_StationTypeComparison"></a>Comparação de tipo de estação

| Descrição | Vídeo direto conectado | Cliente USB zero conectado | RDP via LAN conectada |
|--|--|--|--|
| Desempenho de vídeo | Recomendado para melhor desempenho de vídeo |  | Usar clientes finos que dão suporte ao RemoteFX para melhor qualidade de vídeo em largura de banda de rede menor |
| Limitações físicas | Limitado pelo comprimento do cabo de vídeo e pelo hub USB e comprimento do cabo (recomendado, 15 metros de comprimento máximo) | Limitado pelo hub USB e comprimento do cabo (recomendado, 15 metros de comprimento máximo) | Limitado pela distribuição de LAN |
| Número de estações permitidas | Limitado pelo número de slots PCIe disponíveis na motherboard vezes as portas de vídeo por placa de vídeo | O número total pode ser limitado pelo fabricante do cliente USB zero (para obter mais informações, consulte a observação que segue esta tabela.) | Limitado por portas disponíveis no comutador de rede |
| Tela de divisão | Sim | Sim | Não |
| Status de periférico de estação do MultiPoint Manager, configuração de logon automático, renomeação de estação | Sim | Sim | Não |
| Acesso aos menus de inicialização do servidor | Sim | Não | Não |

> [!NOTE]
> O número total de clientes USB zero que estão conectados ao servidor pode ser limitado pelo fabricante ou pelo recurso de hardware do computador que executa os serviços do MultiPoint.
