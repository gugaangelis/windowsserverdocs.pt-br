---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Atribuindo nomes de domínio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ec66353802b31432d182c1538c0d2f5322f457af
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624394"
---
# <a name="assigning-domain-names"></a>Atribuindo nomes de domínio

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você deve atribuir um nome a cada domínio em seu plano. Os domínios Active Directory Domain Services (AD DS) têm dois tipos de nomes: nomes DNS (sistema de nomes de domínio) e nomes NetBIOS. Em geral, os dois nomes são visíveis para os usuários finais. Os nomes DNS de domínios de Active Directory incluem duas partes, um prefixo e um sufixo. Ao criar nomes de domínio, primeiro determine o prefixo DNS. Esse é o primeiro rótulo no nome DNS do domínio. O sufixo é determinado quando você seleciona o nome do domínio raiz da floresta. A tabela a seguir lista as regras de nomenclatura de prefixo para nomes DNS.

|Regra|Explicação|
|--------|---------------|
|Selecione um prefixo que provavelmente não se tornará desatualizado.|Evite nomes como uma linha de produto ou sistema operacional que possam ser alterados no futuro. É recomendável usar nomes geográficos.|
|Selecione um prefixo que inclua somente caracteres padrão da Internet.|A-Z, a-z, 0-9 e (-), mas não totalmente numérica.|
|Inclua 15 caracteres ou menos no prefixo.|Se você escolher um comprimento de prefixo de 15 caracteres ou menos, o nome NetBIOS será o mesmo que o prefixo.|

Para obter mais informações, consulte [convenções de nomenclatura no Active Directory para computadores, domínios, sites e UOs](https://support.microsoft.com/help/909264/).

> [!NOTE]
> Embora o Dcpromo.exe no Windows Server 2008 e no Windows Server 2003 permita que você crie um nome de domínio do DNS de rótulo único, você não deve usar um nome DNS de rótulo único para um domínio por vários motivos. No Windows Server 2008 R2, o Dcpromo.exe não permite que você crie um nome DNS de rótulo único para um domínio. Para obter mais informações, consulte [implantação e operação de Active Directory domínios configurados usando nomes DNS de rótulo único](https://support.microsoft.com/help/300684/).

Se o nome NetBIOS atual do domínio for inadequado para representar a região ou não atender às regras de nomenclatura de prefixo, selecione um novo prefixo. Nesse caso, o nome NetBIOS do domínio é diferente do prefixo DNS do domínio.

Para cada novo domínio que você implantar, selecione um prefixo apropriado para a região e que satisfaça as regras de nomenclatura de prefixo. Recomendamos que o nome NetBIOS do domínio seja o mesmo que o prefixo DNS.

Documente o prefixo DNS e os nomes NetBIOS que você selecionar para cada domínio em sua floresta. Você pode adicionar as informações de nome DNS e NetBIOS à planilha "planejamento de domínio" que você criou para documentar seu plano para domínios novos e atualizados. Para abri-lo, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip de [auxílios de trabalho para o kit de implantação do Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) e abra "planejamento de domínio" (DSSLOGI_5. doc).
