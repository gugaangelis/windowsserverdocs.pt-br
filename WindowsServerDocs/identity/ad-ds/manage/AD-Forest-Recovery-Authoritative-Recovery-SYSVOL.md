---
title: Recuperação de floresta do AD – sincronização autoritativa de SYSVOL
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 246a2ea589ee05110362cff99d50e93a0a18c2b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826997"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Recuperação de floresta do AD - executar uma sincronização autoritativa de DFSR-replicated SYSVOL  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Há diferentes maneiras de executar uma restauração autoritativa do SYSVOL. Você pode editar a **msDFSR-Options** de atributo ou executar uma restauração de estado do sistema usando o wbadmin – authsysvol. Se você tem a opção de restaurar um backup de estado do sistema (ou seja, você está restaurando AD DS para a mesma instância de hardware e sistema operacional) e em seguida, usar o wbadmin – authsysvol é mais simples. Mas se você precisar executar uma restauração bare metal, você precisa editar o **msDFSR-Options** atributo.  

Use as seguintes etapas para executar uma sincronização autoritativa de SYSVOL (se ele é replicado usando DFSR) por meio da edição do **msDFSR-Options** atributo. Se o SYSVOL é replicado usando FRS, consulte [290762 do artigo](https://go.microsoft.com/fwlink/?LinkId=148443).  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Para executar uma sincronização autoritativa de DFSR-replicated SYSVOL  

1. Abra Usuários e Computadores do Active Directory.  
2. Clique em **modo de exibição**e, em seguida, selecione **usuários, contatos, grupos e computadores como contêineres** e **recursos avançados**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. Na exibição de árvore, clique em **controladores de domínio**, o nome do controlador de domínio é restaurado, **DFSR-LocalSettings**e então **Domain System Volume**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. No painel de detalhes, clique com botão direito **assinatura SYSVOL**, clique em **Properties**e clique em **Editor de atributos**.  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. Clique em **msDFSR-Options**, clique em **editar**, digite **1**e clique em **Okey**  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. Clique em **Okey** para fechar o Editor de atributo.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
