---
title: "Recuperação de floresta do AD - autoritativa sincronização das SYSVOL"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adfs
ms.openlocfilehash: 5bdc619ddf9f28fb074e90dfbf22a7be076a05d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Recuperação de floresta do AD - realizar uma sincronização autoritativa da replicados DFSR SYSVOL  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Há diferentes maneiras de executar uma restauração autorizada SYSVOL. Você pode editar o **msDFSR-Options** de atributo ou executar uma restauração de estado do sistema usando wbadmin – authsysvol. Se você tem a opção para restaurar um backup do estado do sistema (ou seja, você está restaurando AD DS para a mesma instância de hardware e sistema operacional) e depois usar wbadmin – authsysvol é mais simples. Mas se você precisar executar uma restauração bare de metal, então você precisa editar o **msDFSR-Options** atributo.  
  
 Use as seguintes etapas para realizar uma sincronização autoritativa SYSVOL (se ele é replicado usando DFSR) editando o **msDFSR-Options** atributo. Se SYSVOL é replicado usando FRS, consulte [290762 do artigo](https://go.microsoft.com/fwlink/?LinkId=148443).  
  
## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Para executar uma sincronização autoritativa replicados DFSR SYSVOL  
  
1.  Abra Usuários e computadores Active Directory.  
2.  Clique em **exibição**e, em seguida, selecione **usuários, contatos, grupos e computadores como contêineres** e **recursos avançados**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 
3.  Na exibição de árvore, clique em **controladores de domínio**, o nome do controlador de domínio que você restaurou, **DFSR LocalSettings**e depois **Volume de sistema do domínio**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  
4.  No painel de detalhes, clique com botão direito **SYSVOL assinatura**, clique em **propriedades**e clique em **atributo Editor**.  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 
5.  Clique em **msDFSR-Options**, clique em **editar**, tipo **1**e clique em **Okey**  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 
6.  Clique em **Okey** para fechar o Editor de atributo.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
