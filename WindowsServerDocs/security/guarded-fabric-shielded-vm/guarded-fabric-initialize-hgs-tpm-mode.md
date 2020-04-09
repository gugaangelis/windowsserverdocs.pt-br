---
title: Inicializar o HGS usando o atestado confiável do TPM
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b8ceebe63a586ec95b502dfea12f99d174549448
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856609"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Inicializar o HGS usando o atestado confiável do TPM

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Essas etapas variam dependendo se você está inicializando o HGS em uma nova floresta ou em uma floresta de bastiões existente:

1. [Inicializar o cluster HGS em uma nova floresta (padrão)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   -Or-

   [Inicializar o cluster HGS em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Instalar certificados raiz de TPM confiáveis](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configurar o DNS de malha](guarded-fabric-configuring-fabric-dns.md)

