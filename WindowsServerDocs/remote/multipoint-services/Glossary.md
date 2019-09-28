---
title: Glossário
description: Define palavras, termos e conceitos nos serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 807bce1d-b993-49c6-9783-b01a3c55846c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 0c966f0c8e1ad239769c58e4648832ae5020d0dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389653"
---
# <a name="glossary"></a>Glossário
**associar uma estação**  
Para especificar qual monitor é usado com a estação e os dispositivos periféricos, como um teclado e um mouse. Para estações conectadas diretamente ao vídeo, isso é feito pressionando-se uma chave especificada no teclado da estação quando solicitado a fazê-lo. Para estações conectadas a clientes USB com zero, isso normalmente ocorre automaticamente.  
  
**Hub alimentado por barramento**  
Um Hub que desenha toda a sua potência da interface USB do computador. Os hubs com alimentação de barramento não precisam de conexões de energia separadas. No entanto, muitos dispositivos não funcionam com esse tipo de Hub porque exigem mais energia do que esse tipo de Hub fornece.  
  
**modo de console**  
Um dos dois modos dos serviços do MultiPoint pode iniciar. Quando o sistema está no modo de console, nenhuma estação está disponível para uso. Em vez disso, todos os monitores são tratados como uma única área de trabalho estendida para a sessão de console do sistema de computador. O modo de console é normalmente usado para instalar, atualizar ou configurar o software, o que não pode ser feito quando o computador está no modo de estação. Consulte também: *modo de estação*.  
  
**Estação conectada diretamente ao vídeo**  
Uma estação do MultiPoint que consiste em um monitor que está conectado diretamente a uma saída de vídeo no servidor e, no mínimo, inclui um teclado e um mouse que estão conectados ao servidor por meio de um hub USB.  
  
**conta de usuário do domínio**  
Uma conta de usuário hospedada em um computador de domínio. As contas de usuário de domínio podem ser acessadas de qualquer computador conectado ao domínio e não estão ligadas a nenhum computador específico.  
  
**Hub downstream**  
Um Hub que está conectado a um hub de estação para adicionar mais portas disponíveis para dispositivos de estação. Um hub downstream não deve ter um teclado anexado a ele.  
  
**Hub com alimentação externa**  
Também conhecido como um hub autoalimentado, esse Hub leva seu poder de uma unidade de fonte de alimentação externa; Portanto, ele pode fornecer energia completa (até 500 mA) para cada porta. Muitos hubs podem operar como hubs com alimentação de barramento ou externamente.  
  
**Dispositivo de controle de consumidor HID**  
Um dispositivos de interface humana (HID) é um dispositivo de computador que interage diretamente com os humanos. Ele pode receber entrada ou entregar a saída para os seres humanos. Os exemplos são teclado, mouse, trackball, Touchpad, ponto apontando, tabela de gráficos, joystick, scanner de impressão digital, gamepad, webcam, headset e dispositivos de simulador de condução. Um dispositivo de controle de consumidor HID é uma classe específica de dispositivos HID que inclui controles de volume de áudio e chaves de controle de multimídia e de navegador.  
  
**hub intermediário**  
Um Hub que está entre um *hub raiz* no servidor e um hub de estação. Os hubs intermediários normalmente são usados para aumentar o número de portas disponíveis para hubs de estações ou para estender a distância das estações do computador.  
  
**conta de usuário local**  
Uma conta de usuário em um computador específico. Uma conta de usuário local está disponível somente no computador em que a conta está definida.  
  
**Hub multifuncional**  
Consulte *cliente USB zero*.  
  
**Sistema de serviços do MultiPoint**  
Uma coleção de hardware e software que consiste em um computador que tem o Windows Server 2016 instalado com a função de serviços do MultiPoint habilitada e pelo menos uma estação do MultiPoint. Para obter mais informações sobre opções de layout do sistema, consulte [planejamento de site dos serviços do MultiPoint](MultiPoint-services-Site-Planning.md)  
  
**partition**  
Uma seção de espaço em um disco físico que funciona como se fosse um disco separado.  
  
**Estação primária**  
A estação que é a primeira a ser iniciada quando os serviços do MultiPoint são iniciados. A estação principal pode ser usada por um administrador para acessar os menus e as configurações de inicialização. Quando não está sendo usado pelo administrador, ele pode ser usado como uma estação normal (não precisa ser reservado exclusivamente para administração). O monitor da estação principal sempre deve estar conectado diretamente a uma saída de vídeo no computador que está executando os serviços do MultiPoint. Consulte também: estação.  
  
**Estação conectada RDP via LAN**  
Uma estação que é um cliente fino, uma área de trabalho tradicional ou um computador laptop que se conecta aos serviços do MultiPoint usando protocolo RDP (RDP) por meio da LAN (rede local).  
  
**hub raiz**  
Um hub USB que é integrado ao controlador de host na placa-mãe de um computador.  
  
**dividir tela**  
Uma estação em que um único monitor pode ser usado para exibir dois desktops de usuário independentes. Dois conjuntos de hubs, teclados e mouses estão associados a um único monitor. Um conjunto é associado ao lado esquerdo do monitor e o outro conjunto é associado ao lado direito do monitor.  
  
**Estação padrão**  
Ao contrário da *estação primária*, que pode ser usada por um administrador para acessar os menus de inicialização, as estações padrão não exibirão os menus de inicialização e só poderão ser usadas depois que os serviços do MultiPoint tiverem concluído o processo de inicialização. Consulte também: estação.  
  
*Estação*  
Ponto de extremidade do usuário para se conectar ao computador que executa os serviços do MultiPoint. Há suporte para três tipos de estação: estações conectadas por vídeo direto, USB-zero-cliente e RDP através da LAN. Para obter mais informações sobre estações, consulte [MultiPoint stations](MultiPoint-services-Stations.md).  
  
**Hub de estação**  
Um hub USB que foi associado a um monitor para criar uma estação do MultiPoint. Ele conecta os dispositivos USB periféricos aos serviços do MultiPoint. Confira também:   *Zero cliente USB* e *hub USB*.  
  
**modo de estação**  
Um dos dois modos dos serviços do MultiPoint pode iniciar. Normalmente, o sistema de serviços do MultiPoint está no modo de estação. Quando no modo de estação, as estações de serviços do MultiPoint se comportam como se cada estação fosse um computador separado que esteja executando o sistema operacional Windows, e vários usuários podem usar o sistema ao mesmo tempo. Consulte também: *modo de console*.  
  
**Hub USB**  
Um hub de expansão USB multiporta genérico que está em conformidade com as especificações USB (barramento serial universal) 2,0 ou posteriores. Esses hubs normalmente têm várias portas USB, o que permite que vários dispositivos USB sejam conectados a uma única porta USB no computador. Os hubs USB normalmente são dispositivos separados que podem ser *conectados externamente* ou *com barramento*. Alguns outros dispositivos, como alguns teclados e monitores de vídeo, podem incorporar um hub USB ao seu design. Confira também:   *Zero cliente USB*.  
  
**USB sobre cliente Ethernet zero**  
Um cliente USB zero que se conecta ao computador por meio de uma conexão de LAN em vez de uma porta USB. Esse cliente aparece para o servidor como um dispositivo USB mesmo através dos dados enviados por meio da conexão Ethernet.  
  
**Cliente USB zero**  
Um hub de expansão que se conecta ao computador por meio de uma porta USB e permite a conexão de uma variedade de dispositivos não USB ao Hub. Clientes USB zero são produzidos por fabricantes de hardware específicos e exigem a instalação de um driver específico de dispositivo. Os clientes USB zero dão suporte à conexão de um monitor de vídeo (por meio de VGA, DVI e assim por diante) e periféricos (por meio de USB, às vezes PS/2, e áudio analógico). O cliente USB com zero pode ser *externo* ou *equipado com barramento*. Consulte também *hubs USB*.  
  
**Estação conectada a cliente USB zero**  
Uma estação de serviços do MultiPoint que consiste em (no mínimo) um monitor, teclado e mouse, que são conectados ao servidor por meio de um cliente USB zero.  
  
