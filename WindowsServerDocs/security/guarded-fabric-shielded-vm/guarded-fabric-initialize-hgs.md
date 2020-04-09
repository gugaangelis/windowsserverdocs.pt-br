---
title: Inicializar HGS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 42f76dabbe150f229027909f8b58b843f31ae4e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856589"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar o serviço guardião de host (HGS)

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Ao inicializar o HGS, você especifica o modo que o HGS usará para medir a integridade dos hosts protegidos. Há duas opções mutuamente exclusivas. Para obter informações básicas sobre qual modo escolher, consulte [malha protegida e guia de planejamento de VM blindada para hosters](guarded-fabric-planning-for-hosters.md).

Os tópicos a seguir abrangem as etapas de implantação para cada modo:

- [TPM-atestado confiável (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestado de chave de host (modo de chave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Administrador-atestado confiável (modo de AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Você deve executar essas etapas em um servidor físico.