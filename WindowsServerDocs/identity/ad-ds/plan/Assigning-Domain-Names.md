---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: "Atribuir nomes de domínio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0d5ec9b76798f21503f650527a7961cff0b592b4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="assigning-domain-names"></a>Atribuir nomes de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você deve atribuir um nome a cada domínio no seu plano. Os domínios do Active Directory serviços de domínio (AD DS) tem dois tipos de nomes: nomes de NetBIOS e sistema de nomes de domínio (DNS). Em geral, os dois nomes ficam visíveis para os usuários finais. Os nomes DNS dos domínios do Active Directory incluem duas partes, um prefixo e um sufixo. Durante a criação de nomes de domínio, primeiro determine o prefixo DNS. Esse é o primeiro rótulo no nome DNS do domínio. O sufixo é determinado quando você selecionar o nome do domínio raiz da floresta. A tabela a seguir lista o prefixo regras de nomenclatura para nomes DNS.  
  
|Regra|Explicação|  
|--------|---------------|  
|Selecione um prefixo que não é provável que se tornar desatualizados.|Evite nomes como uma linha de produto ou um sistema operacional que pode ser alterada no futuro. Recomendamos o uso de nomes geográficos.|  
|Selecione um prefixo que inclua apenas a caracteres de padrão de Internet.|À Z, à z, 0-9 e (-), mas não inteiramente numérica.|  
|Incluir 15 caracteres ou menos no prefixo.|Se você escolher um comprimento de prefixo de 15 caracteres ou menos, o nome NetBIOS é o mesmo que o prefixo.|  
  
Para obter mais informações, consulte convenções de nomenclatura no Active Directory para computadores, domínios, sites e unidades organizacionais ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Embora o Dcpromo.exe no Windows Server 2008 e Windows Server 2003 permite que você crie um nome de domínio do DNS de rótulo único, você não deve usar um nome DNS de rótulo único para um domínio por vários motivos. No Windows Server 2008 R2, Dcpromo.exe não permite que você crie um nome DNS de rótulo único para um domínio. Para obter mais informações, consulte [https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Se o nome NetBIOS atual do domínio é inadequado representar a região ou não cumprir o regras de nomenclatura de prefixo, selecione um novo prefixo. Nesse caso, o nome NetBIOS do domínio é diferente do prefixo do DNS do domínio.  
  
Para cada novo domínio em que você implantar, selecione um prefixo que é adequado para a região e que atenda as regras de nomenclatura de prefixo. Recomendamos que o nome NetBIOS do domínio seja o mesmo que o prefixo DNS.  
  
Documente o prefixo DNS e nomes de NetBIOS que você selecionar para cada domínio na floresta. Você pode adicionar as informações de nome DNS e NetBIOS à planilha "Domínio planejamento" que você criou para documentar o plano para domínios novos e atualizados. Para abri-lo, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip do trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra "Domínio planejamento" (DSSLOGI_5.doc).  
  


