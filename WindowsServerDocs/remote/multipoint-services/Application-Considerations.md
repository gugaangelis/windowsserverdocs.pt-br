---
title: Considerações do aplicativo
description: Informações de compatibilidade para aplicativos nos serviços do MultiPoint
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 21531273b1dd6d643df3f816a880a0efb3117c70
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405122"
---
# <a name="application-considerations"></a>Considerações do aplicativo
  
## <a name="application-compatibility"></a>Compatibilidade com aplicativos

Qualquer aplicativo que você deseja executar em um sistema de serviços do MultiPoint deve atender aos seguintes requisitos:
  
- Ele deve ser instalado e executado no Windows Server 2016 
- Ele precisa estar ciente da sessão para que cada usuário possa executar uma instância do aplicativo em um sistema do MultiPoint.
  
Se o aplicativo especificar esse requisito, é recomendável tentar instalar o aplicativo e usá-lo em uma sessão de área de trabalho remota. 

## <a name="addressing-application-compatibility-problems"></a>Solucionando problemas de compatibilidade de aplicativos  
Os serviços do MultiPoint oferecem a opção de associar estações com instâncias completas do Windows 10 Enterprise Edition em execução virtualmente no mesmo computador host. Para aplicativos críticos que não executarão várias instâncias para vários usuários ou que não serão instalados em um sistema operacional de 64 bits, isso pode ser uma solução. A implantação de áreas de trabalho dessa maneira requer o uso da guia áreas de trabalho virtuais no Gerenciador do MultiPoint para:  
  
-   Habilitar áreas de trabalho virtuais  
-   Criar um modelo de área de trabalho  
-   Personalizar o modelo com o aplicativo problemático  
-   Associar estações ao modelo personalizado  

Cada estação começa do mesmo modelo, portanto, todas as alterações são apagadas sempre que o computador é iniciado.  
  
>[!NOTE] 
>É importante verificar os requisitos de licenciamento para os aplicativos que você deseja executar em um MultiPoint. Embora você esteja instalando uma cópia, os aplicativos podem exigir o licenciamento por usuário.  
  
