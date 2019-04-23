---
title: Inicializar HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c689a1d5f69731db0cb85a884f5af2ee0230e7be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865447"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar o serviço guardião de Host (HGS)

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Quando você inicializa o HGS, você pode especificar o modo de HGS será usado para medir a integridade de hosts protegidos. Há duas opções mutuamente exclusivas. Para obter informações sobre qual modo deve escolher, consulte [Blindadas guia de planejamento de VM para Hosters e malha protegida](guarded-fabric-planning-for-hosters.md).

Os tópicos a seguir abrangem as etapas de implantação para cada modo:

- [Atestado de TPM confiável (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestado de chaves do host (modo de chave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Atestado de Admin confiável (modo AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Você deve executar essas etapas em um servidor físico.