---
title: Recuperação de floresta do AD-sincronização autoritativa do SYSVOL
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 051e3fdb1c801ab6f19b276b66599ea555026845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369376"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Recuperação de floresta do AD-executando uma sincronização autoritativa de SYSVOL replicado pelo DFSR  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Há diferentes maneiras de executar uma restauração autoritativa do SYSVOL. Você pode editar o atributo **msDFSR-Options** ou executar uma restauração de estado do sistema usando Wbadmin – authsysvol. Se você tiver a opção de restaurar um backup de estado do sistema (ou seja, estiver restaurando AD DS para a mesma instância de hardware e sistema operacional), usar Wbadmin – authsysvol é mais simples. Mas se você precisar executar uma restauração bare-metal, precisará editar o atributo **msDFSR-Options** .  

Use as etapas a seguir para executar uma sincronização autoritativa do SYSVOL (se ele for replicado usando o DFSR) editando o atributo **msDFSR-Options** . Se o SYSVOL for replicado usando o FRS, consulte o [artigo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Para executar uma sincronização autoritativa de SYSVOL replicado pelo DFSR  

1. Abra Usuários e Computadores do Active Directory.  
2. Clique em **Exibir**e selecione **usuários, contatos, grupos e computadores como contêineres** e **recursos avançados**. 

   ![REPLICA](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. No modo de exibição de árvore, clique em **controladores de domínio**, o nome do DC que você restaurou, **DFSR-LocalSettings**e, em seguida, **volume do sistema de domínio**. 

   ![REPLICA](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. No painel de detalhes, clique com o botão direito do mouse em **assinatura SYSVOL**, clique em **Propriedades**e clique em **Editor de atributos**.  

   ![REPLICA](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. Clique em **msDFSR-opções**, clique em **Editar**, digite **1**e clique em **OK**  

   ![REPLICA](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. Clique em **OK** para fechar o editor de atributos.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
