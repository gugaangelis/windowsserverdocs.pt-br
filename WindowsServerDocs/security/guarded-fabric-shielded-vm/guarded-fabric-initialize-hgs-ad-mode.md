---
title: Inicializar o HGS usando o atestado de administrador confiável
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 491754e8bcaad4524084604b78c7c6ed0fdee295
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402353"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inicializar o HGS usando o atestado de administrador confiável

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

>[!IMPORTANT]
>O atestado confiável de administrador (modo de anúncio) é preterido a partir do Windows Server 2019. Para ambientes em que o atestado do TPM não é possível, configure o [atestado de chave do host](guarded-fabric-initialize-hgs-key-mode.md). O atestado de chave de host fornece garantia semelhante ao modo AD e é mais simples de configurar. 


Essas etapas variam dependendo se você está inicializando o HGS em uma nova floresta ou em uma floresta de bastiões existente:

1. [Inicializar o cluster HGS em uma nova floresta (padrão)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -Or-

   [Inicializar o cluster HGS em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurar o encaminhamento de DNS no domínio de malha](guarded-fabric-configuring-fabric-dns.md)

3. [Configurar o encaminhamento de DNS e uma relação de confiança unidirecional no domínio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



