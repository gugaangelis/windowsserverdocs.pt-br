---
title: Recuperação de floresta do AD - limpeza
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fa7193cc800eac0fee6425a66bd5cd82d8c822c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868957"
---
# <a name="ad-forest-recovery---cleanup"></a>Recuperação de floresta do AD - limpeza

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Execute as seguintes etapas de recuperação, conforme necessário:  
  
- Depois de toda a floresta é recuperada, você poderá reverter para a configuração original do DNS, incluindo a configuração dos servidores DNS preferenciais e alternativos em cada um dos controladores de domínio. Depois que os servidores DNS são configurados como eram antes do mau funcionamento, seus recursos de resolução de nome anterior serão restaurados. Exclua qualquer registro DNS para controladores de domínio que ainda não foram recuperados.  
- Exclua registros de serviço de nome na Internet do Windows (WINS) para todos os controladores de domínio que ainda não foram recuperados.  
- Você pode transferir as funções de mestre de operações para outros controladores de domínio no domínio ou floresta e adicionar servidores de catálogo globais com base na configuração antes da falha.  
- Porque toda a floresta é restaurada para um estado anterior, todos os objetos (como usuários e computadores) que foram adicionados e todas as atualizações feitas em objetos existentes após esse ponto (por exemplo, alterações de senha) são perdidas. Portanto, você deve recriar esses objetos ausentes e reaplique as atualizações ausentes, conforme apropriado.  
- Também convém restaurar relações de confiança de saída com domínios externos e florestas, porque essas relações de confiança externas não são restauradas automaticamente a partir de backups.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
