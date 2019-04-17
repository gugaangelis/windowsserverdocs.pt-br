---
title: "Recuperação de floresta do AD - limpeza metadados de controladores de domínio removido"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adfs
ms.openlocfilehash: 3027c59b58801b44d20127e6bcf62dd7319708bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Recuperação de floresta do AD - limpeza metadados de controladores de domínio gravável removido 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Limpeza de metadados remove dados do Active Directory que identifica um controlador de domínio para o sistema de replicação.  
  
 Use o procedimento a seguir para excluir os objetos de DC para controladores de domínio que você pretende adicionar novamente à rede por meio da reinstalação AD DS.  
  
 Se você estiver usando a versão do Active Directory usuários e computadores ou Sites do Active Directory e serviços que é incluído ferramentas de administração de servidor remoto (RSAT), a limpeza de metadados é executada automaticamente quando você exclui um objeto DC.  
  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Excluindo um controlador de domínio usando o Active Directory usuários e computadores  
 Quando você usa a versão do Active Directory usuários e computadores ou centro administrativo do Active Directory em ferramentas de administração de servidor remoto (RSAT), a limpeza de metadados é executada automaticamente quando você exclui o objeto DC. O objeto de servidor e o objeto de computador também serão excluídos automaticamente.  
  
 Como alternativa, você também pode usar serviços e Sites do Active Directory no RSAT para excluir um objeto de DC. Se você usar os serviços e Sites do Active Directory, você deve excluir o objeto de servidor associado e o objeto de configurações NTDS antes que você pode excluí-lo DC.  
  
 Para baixar as RSAT:  

-   [Ferramentas de administração de servidor remoto para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)
  
-   [Ferramentas de administração de servidor remoto para Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)  

-   [Ferramentas de administração de servidor remoto para Windows 7 com Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=7887)  
  
-   [Ferramentas de administração de servidor remoto da Microsoft para o Windows Vista](https://www.microsoft.com/download/details.aspx?id=21090)  
  
 O procedimento a seguir é o mesmo para controladores de domínio que executam o Windows Server 2016, 2008, 2008 R2 ou 2012. O controlador de domínio de destino da operação de limpeza de metadados pode executar qualquer versão do Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Para excluir um objeto de controlador de domínio usando o Active Directory usuários e computadores em RSAT  
  
1.  Clique em **iniciar**, clique em **ferramentas administrativas**e clique em **usuários e Active Directory computadores**.  
2.  Na árvore de console, clique duas vezes o contêiner de domínio e o **controladores de domínio** unidade organizacional (UO).  
3.  No painel de detalhes, clique com botão direito do controlador de domínio que você deseja excluir e clique em **excluir**. 
![Excluir](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4.  Clique em **Sim** para confirmar a exclusão. Selecione o **neste controlador de domínio está permanentemente offline e não pode ser rebaixado usando o Active Directory domínio serviços Assistente para instalação (DCPROMO)** caixa de seleção e clique em **excluir**.  
5.  Se o controlador era um servidor de catálogo global, clique em **Sim** confirme que a exclusão.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
  
