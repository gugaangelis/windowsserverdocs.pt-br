---
title: Introdução ao Software de log de inventário
description: Descreve como instalar e começar a usar o log de inventário de Software
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6944eac179c605db6c7b6f3e08f87c2329fb777f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435368"
---
# <a name="get-started-with-software-inventory-logging"></a>Introdução ao Software de log de inventário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Log de inventário de software coleta dados de inventário de software Microsoft em uma base por servidor. Antes de usar o log de inventário de Software com o Windows Server 2012 R2, certifique-se de que o Windows Update [KB 3000850](https://support.microsoft.com/kb/3000850) e [KB 3060681](https://support.microsoft.com/kb/3060681) são instalados em cada sistema que será inventariado. Nenhuma atualização do Windows é necessária para o Windows Server 2016. Além disso, se você quiser usar a funcionalidade do SIL para encaminhar dados para um servidor de agregação, certifique-se de que ter certificados SSL válidos para sua rede.

## <a name="BKMK_OVER"></a>Descrição do recurso
O Log de Inventário de Software no Windows Server é um recurso com um conjunto simples de cmdlets do PowerShell que ajudam os administradores de servidor a recuperar uma lista dos softwares Microsoft instalados em seus servidores. Ele também fornece a capacidade de coletar e encaminhar esses dados periodicamente pela rede para um servidor Web de destino, usando o protocolo HTTPS, para agregação. O gerenciamento do recurso, principalmente para coleta e encaminhamento por hora, é igualmente feito com comandos do PowerShell.

> [!NOTE]
> Um servidor de agregação que executa um serviço Web pode ser configurado separadamente. Saiba mais sobre [Agregador de Log de Inventário de Software](software-inventory-logging-aggregator.md).

> [!IMPORTANT]
> Nenhum dos dados coletados pelo Log de Inventário de Software será enviado à Microsoft como parte da funcionalidade do recurso.

## <a name="BKMK_APP"></a>Aplicativos práticos
O Log de Inventário de Software destina-se à redução dos custos operacionais relativos à recuperação de informações precisas sobre o software da Microsoft implantado localmente em um servidor, mas especialmente em vários servidores em um ambiente de TI (presumindo-se que ele foi implantado e habilitado no ambiente de TI). A capacidade de encaminhar esses dados para um servidor de agregação (se instalado em separado por um administrador de TI), permite a coleta dos dados em um local por um processo uniforme e automático. Embora isso possa ser feito consultando as interfaces diretamente, o Log de Inventário de Software, ao utilizar uma arquitetura de encaminhamento (pela rede) iniciada por cada servidor, pode superar os desafios de descoberta de computador comuns para muitos cenários de gerenciamento de ativos e inventário de software. O SSL é usado para proteger os dados encaminhados por HTTPS para um servidor de agregação do administrador. Ter os dados em um só local (um único servidor) facilita a análise, manipulação e criação de relatórios de dados. Vale observar que nenhum desses dados são enviados à Microsoft como parte da funcionalidade do recurso. Os dados e a funcionalidade do Log de Inventário de Software destinam-se para utilização exclusiva do proprietário e administradores autorizados do software do servidor.

O Log de Inventário de Software pode ajudar os administradores de servidor a executar as seguintes tarefas:

-   Recuperar informações de inventário do servidor e de software dos servidores Windows, remotamente e sob demanda.

-   Agregar informações de inventário de software e servidor para uma ampla variedade de cenários de gerenciamento de ativos de software habilitando o recurso de Log de Inventário de Software de cada servidor e selecionando um URI de destino de servidor Web e a impressão digital do certificado para autenticação.

## <a name="see-also"></a>Consulte também
[Agregador de registro em log de inventário de software](https://technet.microsoft.com/library/mt572043.aspx)<br>
[Gerenciar o Registro em log de inventário de software](manage-software-inventory-logging.md)<br>
[Cmdlets de registro em log de inventário de software no Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Microsoft Assessment and Planning Toolkit](https://www.microsoft.com/download/en/details.aspx?id=7826)
[Volume Activation Management Tool](http://blogs.technet.com/b/volume-licensing/)

