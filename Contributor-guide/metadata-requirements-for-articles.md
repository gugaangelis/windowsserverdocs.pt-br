---
title: Adicionar as marcas de metadados necessárias ao seu artigo relacionado ao Windows Server
description: Uma lista das informações que você deve adicionar como marcas de metadados à parte superior dos artigos relacionados ao Windows Server. As marcas necessárias estão sujeitas a alterações, com base nos requisitos de relatório e de equipe.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 9bedc7ec13aedab9cfaa655f537d5e431d20cccb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363962"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>Adicionar as marcas de metadados necessárias ao seu artigo relacionado ao Windows Server

Na parte superior de cada artigo, há metadados específicos que devem ser incluídos para fins de rastreamento e SEO. As marcas necessárias estão sujeitas a alterações, com base nos requisitos de relatório. No entanto, você deve ser notificado se precisar adicionar ou remover qualquer campo.

Ele deve ter esta aparência, incluindo os três hifens (---) na parte superior e inferior:

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they're looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager's Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>Exemplo

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```