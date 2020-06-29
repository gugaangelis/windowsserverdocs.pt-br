---
title: Requisitos do Sistema para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/31/2013
ms.prod: windows-server
ms.topic: article
ms.assetid: 0951a67d-492f-41ad-9ae5-8e4cd25e3041
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ef68f4f34e7a543510dbde5dd0b3edbdf221aef9
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471071"
---
# <a name="system-requirements-for-windows-server-essentials"></a>Requisitos do Sistema para o Windows Server Essentials

>Aplica-se a: Windows Server 2019 Essentials, Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

  O software Windows Server Essentials Server é um sistema operacional somente de 64 bits. A tabela 1 define os requisitos mínimos de hardware recomendados para o Windows Server Essentials. A Tabela 2 define os requisitos adicionais de hardware e software para o servidor.


## <a name="table-1-system-requirements-for-windows-server-essentials"></a>Tabela 1. Requisitos do sistema para o Windows Server Essentials

|Componente|Mínimo|Recomendado*|Máximo|
|---------------|-------------|-------------------|-------------|
|Soquete da CPU|1,4 GHz (processador de 64 bits) ou mais rápido para núcleo único<br /><br /> 1,3 GHz (processador de 64 bits) ou mais rápido para núcleo múltiplo|3,1 GHz (processador de 64 bits) ou mais rápido para núcleo múltiplo|2 soquetes|
|Memória (RAM)|2 GB<br /><br /> 4 GB se você implantar o Windows Server Essentials como uma máquina virtual|16 GB|64 GB|
|Discos rígidos e espaço de armazenamento disponível|Disco rígido de 160 GB com uma partição de sistema de 60 GB||Sem limite|

 *Os requisitos de hardware recomendados têm suporte a limites máximos de usuários e dispositivos.

## <a name="table-2-additional-hardware-and-software-requirements-for-windows-server-essentials"></a>Tabela 2. Requisitos adicionais de hardware e software para o Windows Server Essentials

|Componente|Descrição|
|---------------|-----------------|
|Adaptador de rede|Adaptador Gigabit Ethernet (10/100/1000baseT PHY/MAC)|
|Internet|Alguma funcionalidade pode requerer acesso à Internet (tarifas podem ser aplicadas) ou uma conta da Microsoft|
|Sistemas operacionais cliente suportados|Windows 8.1, Windows 8, Windows 7, versões Macintosh OS X 10.5 a 10.8.<br /><br /> **Observação:** Alguns recursos exigem edições Professional ou superior.<br /><br /> 1 GB de espaço disponível no disco rígido (uma parte desse disco será liberada depois da instalação)|
|Router|Um roteador ou firewall com suporte para IPv4 NAT ou Ipv6|
|Requisitos adicionais|Leitor de DVD-ROM|

 Os requisitos reais irão variar com base na configuração do seu sistema, nos aplicativos e nos recursos que você optou por instalar. O desempenho do processador depende não apenas da frequência do relógio, mas também do número de núcleos e do tamanho do cache do processador. Os requisitos de espaço de armazenamento para a partição do sistema são aproximados. Talvez seja exigido espaço de armazenamento disponível adicional se você estiver instalando em uma rede.

 Para obter mais informações sobre os requisitos de hardware, consulte o [Windows Server Catalog](https://www.windowsservercatalog.com/).

 Todo o hardware do servidor deve atender aos requisitos estabelecidos para o programa de logotipo do Windows Server 2012 R2 para sistemas. Para obter mais informações, consulte o [Programa de Logotipo do Windows](https://msdn.microsoft.com/windows/hardware/gg487403.aspx).

> [!IMPORTANT]
> Não há suporte para discos dinâmicos no Windows Server Essentials.

## <a name="additional-references"></a>Referências adicionais

-   [Instalar o Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

-   [Requisitos do sistema para o Windows Server Essentials](system-requirements.md)


