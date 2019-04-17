---
title: "Recuperação de floresta do AD - limpeza"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 2f652d08304a17ecbfde51bbb6f35e4666cd9eca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---cleanup"></a>Recuperação de floresta do AD - limpeza 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Execute as seguintes etapas de recuperação de post conforme necessário:  
  
-   Depois que toda a floresta é recuperada, você pode reverter para a configuração original do DNS, incluindo configuração dos servidores DNS preferenciais e alternativos em cada um dos controladores de domínio. Depois que os servidores DNS são configurados como eles estavam antes do mau funcionamento, seus recursos de resolução de nome anterior serão restaurados. Exclua todos os registros DNS para controladores de domínio que não foi recuperados.  
  
-   Exclua registros de serviço de cadastramento na Internet do Windows (WINS) para todos os controladores de domínio que não foi recuperados.  
  
-   Você pode transferir as funções mestre de operações para outros controladores de domínio no domínio ou floresta e adicionar mais globais servidores de catálogo com base na configuração antes da falha.  
  
-   Porque toda a floresta é restaurada para um estado anterior, quaisquer objetos (como os usuários e computadores) que foram adicionados e todas as atualizações feitas em objetos existentes após esse ponto (por exemplo, alterações de senha) são perdidas. Portanto, você deve recriar esses objetos ausentes e reaplicar as atualizações ausentes conforme apropriado.  
  
-   Também convém restaurar saídas relações de confiança com domínios externos e florestas, porque essas relações de confiança externos não são restauradas automaticamente a partir de backups.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  



