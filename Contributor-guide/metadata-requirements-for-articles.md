---
title: Adicione as marcas de metadados necessários para seu artigo relacionado ao Windows Server
description: Uma lista das informações você deve adicionar como marcas de metadados na parte superior do seus artigos relacionados ao Windows Server. As marcas necessárias estão sujeitos a alterações, com base nos requisitos de seu relatório e equipe.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: f7c514def1353d44386b1bc53c8cabffe1e31fda
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461642"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>Adicione as marcas de metadados necessários para seu artigo relacionado ao Windows Server

Na parte superior de cada artigo, não há metadados específicos do que devem ser incluído para fins SEO e acompanhamento. As marcas necessárias estão sujeitos a alterações, com base em requisitos de relatórios. No entanto, você deve ser notificado se você precisar adicionar ou remover todos os campos.

Ele deve ser semelhante isso, incluindo os três hifens (-) na parte superior e inferior:

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they’re looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager’s Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>Exemplo

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server-threshold
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```