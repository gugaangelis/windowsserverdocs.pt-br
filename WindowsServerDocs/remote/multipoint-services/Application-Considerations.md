---
title: Considerações do aplicativo
description: Informações de compatibilidade para aplicativos nos serviços do MultiPoint
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 6657e8d9f7c206cb5532eda3491af98f161ccb7a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944456"
---
# <a name="application-considerations"></a>Considerações do aplicativo

## <a name="application-compatibility"></a>Compatibilidade de aplicativos

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

