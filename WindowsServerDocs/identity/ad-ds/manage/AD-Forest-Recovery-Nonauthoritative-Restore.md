---
title: "Recuperação de floresta do AD - restauração sem autorização"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adfs
ms.openlocfilehash: 684672491a0574b12e9b117a8e3c9c1f0936f357
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Executar uma restauração sem autorização dos serviços de domínio do Active Directory 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Para executar uma restauração sem autorização, conclua o procedimento a seguir.  
  
 Os procedimentos a seguir usam o Wbadmin.exe para executar uma restauração não autorizada do Active Directory ou serviços de domínio do Active Directory (AD DS). Se você estiver usando uma solução de backup diferente ou se você pretende completar a restauração autoritativa SYSVOL mais tarde no processo de recuperação de floresta, você pode executar uma restauração autorizada do SYSVOL usando esses métodos alternativos:  
  
-   Se você estiver usando o serviço de replicação de arquivos (FRS) para replicar SYSVOL, siga as etapas em [290762 do artigo](https://go.microsoft.com/fwlink/?LinkId=148443) na Base de dados de Conhecimento da Microsoft, usando o **BurFlags** chave do registro para reinicializar os conjuntos de réplica FRS ou, se necessário, do artigo 315457 [315457](https://support.microsoft.com/kb/315457)para recriar a árvore SYSVOL. Para determinar se SYSVOL é replicado por FRS, consulte [pasta SYSVOL do determinar se um controlador de domínio é replicada por DFSR ou FRS](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
  
-   Se você estiver usando replicação do sistema de arquivos distribuído (DFS) para replicar SYSVOL, consulte [execute uma sincronização autoritativa das SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
 
## <a name="performing-a-nonauthoritative-restore"></a>Executar uma restauração não autorizada  
 Use o procedimento a seguir para executar uma restauração não autorizada do AD DS e uma restauração autorizada do SYSVOL ao mesmo tempo usando wbadmin.exe em um controlador de domínio que executa o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. O backup explicitamente deve incluir dados de estado do sistema; um backup do servidor completo que é usado para recuperação completa do servidor não funcionará. Para obter mais informações sobre como criar um backup do estado do sistema, consulte [backup dos dados de estado do sistema](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Para executar uma restauração não autorizada do AD DS e a restauração do SYSVOL usando wbadmin.exe  
  
-   Incluir a **- authsysvol** alternar o comando de recuperação, conforme mostrado no exemplo a seguir:  
  
    ```  
    wbadmin start systemstaterecovery <otheroptions> -authsysvol  
    ```  
  
     Por exemplo:  
  
    ```  
    wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
    ```  
  
 ![Restaurar](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
