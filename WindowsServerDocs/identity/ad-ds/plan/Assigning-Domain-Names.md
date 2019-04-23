---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Atribuindo nomes de domínio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba5a7ee8b7d9728c48e2798853aab8d55047e86f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866557"
---
# <a name="assigning-domain-names"></a>Atribuindo nomes de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você deve atribuir um nome para cada domínio em seu plano. Domínios do Active Directory Domain Services (AD DS) tem dois tipos de nomes: Nomes de nome DNS (sistema) de domínio e nomes NetBIOS. Em geral, os dois nomes são visíveis aos usuários finais. Os nomes DNS dos domínios do Active Directory incluem duas partes, um prefixo e um sufixo. Ao criar nomes de domínio, primeiro determine o prefixo DNS. Isso é o primeiro rótulo no nome DNS do domínio. O sufixo é determinado quando você seleciona o nome do domínio raiz da floresta. A tabela a seguir lista o prefixo de regras de nomenclatura para nomes DNS.  
  
|Regra|Explicação|  
|--------|---------------|  
|Selecione um prefixo que não provavelmente fiquem desatualizados.|Evite nomes como uma linha de produto ou sistema operacional que pode ser alterado no futuro. É recomendável usar nomes geográficos.|  
|Selecione um prefixo que inclui apenas a caracteres de padrão de Internet.|A-Z, a-z, 0 a 9 e (-), mas não totalmente numérica.|  
|Incluir 15 caracteres ou menos no prefixo.|Se você escolher um tamanho de prefixo de 15 caracteres ou menos, o nome NetBIOS é o mesmo que o prefixo.|  
  
Para obter mais informações, consulte convenções de nomenclatura no Active Directory para computadores, domínios, sites e unidades organizacionais ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Embora o Dcpromo.exe no Windows Server 2008 e no Windows Server 2003 permita que você crie um nome de domínio do DNS de rótulo único, você não deve usar um nome DNS de rótulo único para um domínio por vários motivos. No Windows Server 2008 R2, o Dcpromo.exe não permite que você crie um nome DNS de rótulo único para um domínio. Para obter mais informações, consulte [ https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Se o nome NetBIOS atual do domínio é inadequado representar a região ou falha satisfazer o prefixo de regras de nomeação, selecione um novo prefixo. Nesse caso, o nome NetBIOS do domínio é diferente do prefixo DNS do domínio.  
  
Para cada novo domínio em que você implantar, selecione um prefixo que é apropriado para a região e que satisfaz as regras de nomenclatura de prefixo. É recomendável que o nome NetBIOS do domínio seja o mesmo que o prefixo DNS.  
  
Documente o prefixo DNS e nomes de NetBIOS que você selecionar para cada domínio na floresta. Você pode adicionar as informações de nome NetBIOS e DNS para a planilha "Planejando a domínio" que você criou para documentar seu plano para domínios novos e atualizados. Para abri-lo, baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip do trabalho auxílios para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra "Planejamento de domínio" (DSSLOGI_5.doc).  
  


