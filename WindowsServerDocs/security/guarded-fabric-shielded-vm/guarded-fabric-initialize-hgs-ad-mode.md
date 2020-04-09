---
title: Inicializar o HGS usando o atestado de administrador confiável
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b7c0b88071a28953ddda8abb57a805ef119511e0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856669"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inicializar o HGS usando o atestado de administrador confiável

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

>[!IMPORTANT]
>O atestado confiável de administrador (modo de anúncio) é preterido a partir do Windows Server 2019. Para ambientes em que o atestado do TPM não é possível, configure o [atestado de chave do host](guarded-fabric-initialize-hgs-key-mode.md). O atestado de chave de host fornece garantia semelhante ao modo AD e é mais simples de configurar. 


Essas etapas variam dependendo se você está inicializando o HGS em uma nova floresta ou em uma floresta de bastiões existente:

1. [Inicializar o cluster HGS em uma nova floresta (padrão)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -Or-

   [Inicializar o cluster HGS em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurar o encaminhamento de DNS no domínio de malha](guarded-fabric-configuring-fabric-dns.md)

3. [Configurar o encaminhamento de DNS e uma relação de confiança unidirecional no domínio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



