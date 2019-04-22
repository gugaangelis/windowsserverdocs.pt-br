---
title: Inicializar HGS usar atestado de Admin confiável
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 664404cb72981e162bca016df14847e684d987c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821827"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inicializar HGS usar atestado de Admin confiável

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

>[!IMPORTANT]
>Atestado de Admin confiável (modo AD) é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](guarded-fabric-initialize-hgs-key-mode.md). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 


Essas etapas variam dependendo se você estiver inicializando HGS em uma nova floresta ou em uma floresta de bastiões existente:

1. [Inicializar o cluster HGS em uma nova floresta (padrão)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -Or-

   [Inicializar o cluster HGS em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurar o encaminhamento de DNS no domínio do fabric](guarded-fabric-configuring-fabric-dns.md)

3. [Configurar o encaminhamento de DNS e uma relação de confiança unidirecional no domínio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



