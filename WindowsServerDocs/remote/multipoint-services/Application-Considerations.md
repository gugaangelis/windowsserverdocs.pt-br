---
title: Considerações do aplicativo
description: Informações de compatibilidade de aplicativos no MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 400f87c09f1b2e897d67f94e9b7ac12ae0a1e799
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839827"
---
# <a name="application-considerations"></a>Considerações do aplicativo
  
## <a name="application-compatibility"></a>Compatibilidade com aplicativos

Qualquer aplicativo que você deseja executar em um sistema MultiPoint Services deve atender aos seguintes requisitos:
  
- Ele deve instalar e executar no Windows Server 2016 
- Ele precisa estar ciente da sessão para que cada usuário possa executar uma instância do aplicativo em um sistema MultiPoint.
  
Se o aplicativo especificar esse requisito, é recomendável tentar instalar o aplicativo e usá-lo em uma sessão de área de trabalho remota. 

## <a name="addressing-application-compatibility-problems"></a>Resolver problemas de compatibilidade de aplicativo  
MultiPoint Services oferece a opção para associar estações instâncias completas das edições do Windows 10 Enterprise sendo executado virtualmente no mesmo computador host. Para aplicativos críticos que não serão executados de várias instâncias para vários usuários ou não serão instalado em um sistema operacional de 64 bits, isso pode ser uma solução. A implantação de áreas de trabalho dessa maneira requer usando a guia de áreas de trabalho virtuais no Gerenciador do MultiPoint para:  
  
-   Habilitar áreas de trabalho virtuais  
-   Criar um modelo de área de trabalho  
-   Personalizar o modelo com o aplicativo com problema  
-   Associar estações do modelo personalizado  

Cada estação inicia do mesmo modelo, portanto, todas as alterações são apagadas a cada vez que o computador é iniciado.  
  
>[!NOTE] 
>É importante verificar os requisitos de licenciamento para os aplicativos que deseja executar em um MultiPoint. Embora você está instalando uma cópia aplicativos podem exigir que o licenciamento por usuário.  
  
