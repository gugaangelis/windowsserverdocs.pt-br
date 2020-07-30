---
title: Requisitos de Hardware do Computador de Destino
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c20b06b9-ce0d-4c20-bf49-257c3f5dc01b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9cb68b88bc19b8f0f62c5586e39e010d9caad151
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181232"
---
# <a name="hardware-requirements-for-the-target-computer"></a>Requisitos de Hardware do Computador de Destino

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta seção fornece os requisitos de hardware para o Windows Server Essentials.

## <a name="hardware-requirements-for-windows-server-essentials"></a>Requisitos de hardware para o Windows Server Essentials
 Você deve instalar o Windows Server Essentials no computador de destino que atende aos requisitos na tabela a seguir.

### <a name="table-1--system-requirements-for-windows-server-essentials"></a>Tabela 1: requisitos do sistema para o Windows Server Essentials

|Componente|Mínimo|Recomendado*|Máximo|
|---------------|-------------|-------------------|-------------|
|Soquete da CPU|1,4 GHz (processador de 64 bits) ou mais rápido para núcleo único<br /><br /> 1,3 GHz (processador de 64 bits) ou mais rápido para núcleo múltiplo|3,1 GHz (processador de 64 bits) ou mais rápido para núcleo múltiplo|2 soquetes|
|Memória (RAM)|2 GB|16 GB|64 GB|
|Discos rígidos e espaço de armazenamento disponível|Disco rígido de 160 GB com uma partição de sistema de 60 GB||Sem limite|

 * Requisitos de hardware recomendados para dar suporte a limites máximos de usuário e de dispositivo do Windows Server Essentials.

## <a name="additional-hardware-and-software-requirements"></a>Requisitos de hardware e software adicionais
 A tabela a seguir relaciona os requisitos de hardware e software adicionais.

|Componente|Descrição|
|---------------|-----------------|
|Adaptador de rede|Adaptador Gigabit Ethernet (10/100/1000baseT PHY/MAC)|
|Internet|Algumas funcionalidades podem exigir acesso à Internet (tarifas podem ser aplicadas) ou uma conta do Windows Live &reg; ID|
|Sistemas Operacionais Clientes com Suporte|-Windows 7<br />-Windows 8<br />-Macintosh OS X 10,5 a 10,8.<br /><br /> **Observação:** Alguns recursos exigem edições Professional ou superior.<br /><br /> 1 GB de espaço disponível no disco rígido (uma parte desse disco será liberada depois da instalação)|
|Roteador|Um roteador ou firewall com suporte a NAT IPv4|
|Requisitos adicionais|Leitor de DVD-ROM|

 Os requisitos reais irão variar com base na configuração do seu sistema, nos aplicativos e nos recursos que você optou por instalar. O desempenho do processador depende não apenas da frequência do relógio, mas também do número de núcleos e do tamanho do cache do processador. Os requisitos de espaço de armazenamento para a partição do sistema são aproximados. Talvez seja exigido espaço de armazenamento disponível adicional se você estiver instalando em uma rede.

 Para obter mais informações sobre os requisitos de hardware, consulte o [Windows Server Catalog](https://www.windowsservercatalog.com).

 Todo o hardware do servidor deve atender aos requisitos estabelecidos para o programa de logotipo do Windows Server 2012 para sistemas. Para obter mais informações, consulte o [Programa de Logotipo do Windows](https://www.microsoft.com/whdc/winlogo/hwrequirements.mspx).

## <a name="see-also"></a>Consulte Também

 [Introdução com o Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)

