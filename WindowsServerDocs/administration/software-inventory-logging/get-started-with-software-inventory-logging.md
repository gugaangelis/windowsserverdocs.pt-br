---
title: Introdução ao log de inventário de software
description: Descreve como instalar e começar a usar o log de inventário de software
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8584b5e2cf1048e0bba5c217aa3e6031600839a
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993045"
---
# <a name="get-started-with-software-inventory-logging"></a>Introdução ao log de inventário de software

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 O log de inventário de software coleta dados de inventário de software da Microsoft por servidor. Antes de usar o log de inventário de software com o Windows Server 2012 R2, verifique se Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850) e [KB 3060681](https://support.microsoft.com/kb/3060681) estão instalados em cada sistema que será inventariado. Nenhum Windows Update é necessário para o Windows Server 2016. Além disso, se você quiser usar a capacidade do SIL de encaminhar dados para um servidor de agregação, certifique-se de que você tenha certificados SSL válidos para sua rede.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrição do recurso
O Log de Inventário de Software no Windows Server é um recurso com um conjunto simples de cmdlets do PowerShell que ajudam os administradores de servidor a recuperar uma lista dos softwares Microsoft instalados em seus servidores. Ele também fornece a capacidade de coletar e encaminhar esses dados periodicamente pela rede para um servidor Web de destino, usando o protocolo HTTPS, para agregação. O gerenciamento do recurso, principalmente para coleta e encaminhamento por hora, é igualmente feito com comandos do PowerShell.

> [!NOTE]
> Um servidor de agregação que executa um serviço Web pode ser configurado separadamente. Saiba mais sobre [Agregador de Log de Inventário de Software](software-inventory-logging-aggregator.md).

> [!IMPORTANT]
> Nenhum dos dados coletados pelo log de inventário de software é enviado à Microsoft como parte da funcionalidade do recurso.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
O Log de Inventário de Software destina-se à redução dos custos operacionais relativos à recuperação de informações precisas sobre o software da Microsoft implantado localmente em um servidor, mas especialmente em vários servidores em um ambiente de TI (presumindo-se que ele foi implantado e habilitado no ambiente de TI). A capacidade de encaminhar esses dados para um servidor de agregação (se instalado em separado por um administrador de TI), permite a coleta dos dados em um local por um processo uniforme e automático. Embora isso possa ser feito consultando as interfaces diretamente, o Log de Inventário de Software, ao utilizar uma arquitetura de encaminhamento (pela rede) iniciada por cada servidor, pode superar os desafios de descoberta de computador comuns para muitos cenários de gerenciamento de ativos e inventário de software. O SSL é usado para proteger os dados encaminhados por HTTPS para o servidor de agregação de um administrador. Ter os dados em um só local (um único servidor) facilita a análise, manipulação e criação de relatórios de dados. Vale observar que nenhum desses dados são enviados à Microsoft como parte da funcionalidade do recurso. Os dados e a funcionalidade de log de inventário de software são destinados ao uso exclusivo do proprietário e dos administradores licenciados do software do servidor.

O Log de Inventário de Software pode ajudar os administradores de servidor a executar as seguintes tarefas:

-   Recuperar informações de inventário do servidor e de software dos servidores Windows, remotamente e sob demanda.

-   Agregar informações de inventário de software e de servidor para uma ampla variedade de cenários de gerenciamento de ativos de software, habilitando o recurso de log de inventário de software de cada servidor e escolhendo um URI de destino do servidor Web e a impressão digital do certificado para autenticação.

## <a name="see-also"></a>Consulte Também
[Agregador de registro em log de inventário de software](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/mt572043(v=ws.11))<br>
[Gerenciar o Registro em log de inventário de software](manage-software-inventory-logging.md)<br>
[Software Inventory Logging Cmdlets in Windows PowerShell (Cmdlets de Log de Inventário de Software no Windows PowerShell)](/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)<br>
[Kit de ferramentas](https://www.microsoft.com/download/en/details.aspx?id=7826) 
 de avaliação e planejamento da Microsoft [Ferramenta de gerenciamento de ativação de volume](https://blogs.technet.com/b/volume-licensing/)