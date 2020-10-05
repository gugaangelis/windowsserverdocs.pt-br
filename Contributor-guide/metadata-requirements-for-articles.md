---
title: Adicionar as marcas de metadados necessárias ao seu artigo relacionado ao Windows Server
description: Uma lista das informações que você deve adicionar como marcas de metadados à parte superior dos artigos relacionados ao Windows Server. As marcas necessárias estão sujeitas a alterações, com base nos requisitos de relatório e de equipe.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 7728d6e9bb2c1e5a6ad53f638f253c53a0720059
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717743"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>Adicionar as marcas de metadados necessárias ao seu artigo relacionado ao Windows Server

Na parte superior de cada artigo, há metadados específicos que devem ser incluídos para fins de rastreamento e SEO. As marcas necessárias estão sujeitas a alterações, com base nos requisitos de relatório. No entanto, você deve ser notificado se precisar adicionar ou remover qualquer campo.

Ele deve ter esta aparência, incluindo os três hifens (---) na parte superior e inferior:

```markdown
---
title: Required. The title of the article should go here. This is used in SEO and search results.
description: Required. A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they're looking for.
author: Required. Your GitHub alias
ms.author: Required. Your Microsoft alias
manager: Optional. Your manager's Microsoft alias
ms.reviewer: Optional. The Microsoft alias for the primary PM for the feature/functionality
ms.topic: Type of article, including conceptual, how-to, hub-page, overview, quickstart, reference, sample, troubleshooting, or tutorial
ms.date: Date of change (MM/DD/YYYY)

---
```

## <a name="example"></a>Exemplo

```markdown
---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
author: danielle-github
ms.author: danielle
manager: alainch
ms.reviewer: alainch
ms.topic: overview
ms.date: 07/06/2019

---
```