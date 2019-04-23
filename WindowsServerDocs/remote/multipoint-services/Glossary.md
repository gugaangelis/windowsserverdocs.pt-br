---
title: Glossário
description: Define as palavras, termos e conceitos no MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 807bce1d-b993-49c6-9783-b01a3c55846c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 4449d2d6fb87f74496b7d482a7a7263f703c7822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831107"
---
# <a name="glossary"></a>Glossário
**associar uma estação**  
Para especificar qual monitor é usado com quais estação e dispositivos periféricos, como um teclado e mouse. Para estações conectadas vídeos diretas, isso é feito pressionando uma chave especificada no teclado da estação quando for solicitado a fazê-lo. USB zero cliente conectado estações, isso geralmente acontece automaticamente.  
  
**hub de barramento**  
Um hub que desenha toda sua potência da interface USB do computador. Os hubs de barramento não é necessário para conexões de alimentação distintas. No entanto, muitos dispositivos não funcionam com esse tipo de hub porque eles requerem mais energia do que fornece esse tipo de hub.  
  
**modo de console**  
Um dos dois modos de MultiPoint services pode iniciar. Quando o sistema está no modo de console, não há estações estão disponíveis para uso. Em vez disso, todos os monitores são tratados como uma única área de trabalho estendida para a sessão de console do sistema do computador. Modo de console normalmente é usado para instalar, atualizar ou configurar software, que não pode ser feito quando o computador estiver no modo de estação. Consulte também: *modo de estação*.  
  
**estação de vídeo diretamente conectados**  
Uma estação do MultiPoint que consiste em um monitor que está conectado diretamente a uma saída de vídeo no servidor e, no mínimo, ele inclui um teclado e mouse que estão conectados ao servidor por meio de um hub USB.  
  
**Conta de usuário de domínio**  
Uma conta de usuário que está hospedada em um computador de domínio. Contas de usuário de domínio podem ser acessadas de qualquer computador que está conectado ao domínio, e eles não estão vinculados a qualquer computador em particular.  
  
**hub de downstream**  
Um hub que está conectado a um hub de estação para adicionar portas mais disponíveis para dispositivos de estação. Um hub de downstream não deve ter um teclado conectado a ele.  
  
**hub alimentado externamente**  
Também conhecido como um hub alimentado automaticamente, esse hub usa sua capacidade de uma unidade de fonte de alimentação externo; Portanto, ele pode fornecer energia plena (até 500 mA) para cada porta. Muitos centros podem operar como hubs de barramento ou alimentados externamente.  
  
**Dispositivo de controle do consumidor HID**  
Um dispositivo de Interface Humana (HID) é um dispositivo de computador que interage diretamente com os seres humanos. Ele pode recebem entrado de ou entregam a saída para os seres humanos. Exemplos são o teclado, mouse, trackball, touchpad, ponteiro, tabela de gráficos, joystick, scanner de impressão digital, gamepad, webcam, fone de ouvido e dispositivos de simulador de condução. Um dispositivo de controle do consumidor HID é uma classe específica de HID dispositivos que inclui controles de volume de áudio e as chaves de controle do navegador e de multimídia.  
  
**hub intermediário**  
Um hub entre um *hub raiz* no servidor e um hub de estação. Hubs intermediários geralmente são usados para aumentar o número de portas disponíveis para os hubs de estações ou estender a distância das estações do computador.  
  
**conta de usuário local**  
Uma conta de usuário em um computador específico. Uma conta de usuário local está disponível somente no computador em que a conta é definida.  
  
**hub multifuncional**  
Ver *zero cliente USB*.  
  
**Sistema multiPoint Services**  
Uma coleção de hardware e software que consiste em um computador com Windows Server 2016 instalado com a função do MultiPoint Services habilitada e pelo menos uma estação do MultiPoint. Para obter mais informações sobre opções de layout do sistema, consulte [planejamento do Site do MultiPoint Services](MultiPoint-services-Site-Planning.md)  
  
**partition**  
Uma seção de espaço em um disco físico que funciona como se fosse um disco separado.  
  
**estação principal**  
A estação que é o primeiro a ser iniciado quando o MultiPoint Services é iniciado. A estação principal pode ser usada por um administrador para acessar as configurações e os menus de inicialização. Quando não está sendo usado pelo administrador, ele pode ser usado como uma estação normal (ele não precisa ser reservado exclusivamente para administração). Monitor da estação primária sempre deve estar conectado diretamente a uma saída de vídeo no computador que está executando o MultiPoint Services. Consulte também: estação.  
  
**Estação de RDP-over-conectados à rede local**  
Uma estação é um cliente fino, tradicionais para desktop ou laptop que se conecta ao MultiPoint services usando o protocolo de área de trabalho remota (RDP) por meio da rede local (LAN).  
  
**hub raiz**  
Um hub USB que é integrado ao controlador de host na placa-mãe de um computador.  
  
**o Split screen**  
Uma estação em que um único monitor pode ser usado para exibir duas áreas de trabalho do usuário independente. Dois conjuntos de hubs, teclados e mouses são associados um único monitor. Um conjunto é associado com o lado esquerdo do monitor e o outro conjunto é associado à direita do monitor.  
  
**estação padrão**  
Em contraste com o *estação principal*, que pode ser usado por um administrador a menus de inicialização de acesso, estações padrão não exibirá os menus de inicialização e só podem ser usados depois de concluir o processo de inicialização do MultiPoint Services . Consulte também: estação.  
  
*station*  
Ponto de extremidade de usuário para se conectar ao computador executando o MultiPoint Services. Há suporte para três tipos de estação: estações vídeo diretamente conectados, conectados por USB zero-client e RDP-over-conectados à rede local. Para obter mais informações sobre as estações, consulte [estações do MultiPoint](MultiPoint-services-Stations.md).  
  
**hub de estação**  
Um hub USB que tenha sido associado um monitor para criar uma estação do MultiPoint. Ele se conecta dispositivos periféricos de USB ao MultiPoint Services. Consulte também: *Zero cliente USB* e *hub USB*.  
  
**modo de estação**  
Um dos dois modos de MultiPoint services pode iniciar. Normalmente, o sistema MultiPoint Services está no modo de estação. Quando estiver no modo de estação, as estações do MultiPoint Services se comportam como se cada estação é um computador separado que esteja executando o sistema operacional Windows e vários usuários podem usar o sistema ao mesmo tempo. Consulte também: *modo de console*.  
  
**Hub USB**  
Um genérico multiporta expansão hub USB que está em conformidade com as especificações de barramento serial universal (USB) 2.0 ou posterior. Tais hubs normalmente têm várias portas USB, que permite que vários dispositivos USB sejam conectados a uma única porta USB no computador. Hubs USB são dispositivos normalmente separados que podem ser *alimentados externamente* ou *barramento*. Alguns outros dispositivos, como teclados e monitores de vídeo podem incorporar um hub USB em seu design. Consulte também: *Zero cliente USB*.  
  
**USB ao longo do cliente Ethernet zero**  
Um zero cliente USB que se conecta ao computador por meio de uma conexão de rede local, em vez de uma porta USB. Esse cliente é exibida para o servidor como um dispositivo USB até mesmo por meio dos dados é enviado por meio de conexão de Ethernet.  
  
**Zero cliente USB**  
Um hub de expansão que se conecta ao computador por meio de uma porta USB e permite a conexão de uma variedade de dispositivos USB não para o hub. Os clientes USB zero produzidos pelos fabricantes de hardware específico e eles requerem a instalação de um driver de dispositivo específico. Suporte a USB zero clientes se conectam a um monitor de vídeo (por meio de VGA, DVI e assim por diante) e periféricos (por meio de USB, às vezes, PS/2 e áudio analógico). O USB zero cliente pode ser *alimentados externamente* ou *barramento*. Consulte também *hubs USB*.  
  
**Estação conectada com zero cliente USB**  
Uma estação do MultiPoint Services que consiste de (, no mínimo) um monitor, teclado e mouse, o qual está conectado ao servidor por meio de um zero cliente USB.  
  
