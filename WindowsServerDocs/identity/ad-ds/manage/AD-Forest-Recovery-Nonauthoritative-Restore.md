---
title: Recuperação de floresta do AD - restauração não autoritativa
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: 65e33e6507d2affc4d07cc0780a7baf91a170a09
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280586"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Executar uma restauração não autoritativa de serviços de domínio do Active Directory 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Para executar uma restauração não autoritativa, conclua o procedimento a seguir.  
  
Os procedimentos a seguir usam o Wbadmin.exe para executar uma restauração não autoritativa do Active Directory ou os serviços de domínio do Active Directory (AD DS). Se você estiver usando uma solução de backup diferente ou se você pretende concluir a restauração autoritativa do SYSVOL posteriormente no processo de recuperação de floresta, você pode executar uma restauração autoritativa do SYSVOL, usando esses métodos alternativos:  
  
- Se você estiver usando replicação FRS (serviço) para replicar o SYSVOL, siga as etapas em [290762 do artigo](https://go.microsoft.com/fwlink/?LinkId=148443) na Base de dados de Conhecimento Microsoft, usando o **BurFlags** chave do registro para reinicializar réplicas do FRS Define ou, se necessário, artigo 315457 [315457](https://support.microsoft.com/kb/315457)para recriar a árvore SYSVOL. Para determinar se o SYSVOL é replicado pelo FRS, consulte [pasta SYSVOL de determinar se um controlador de domínio é replicada por DFSR ou FRS](https://msdn.microsoft.com/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
- Se você estiver usando replicação de sistema de arquivos distribuído (DFS) para replicar o SYSVOL, consulte [executar uma sincronização autoritativa de SYSVOL replicado por DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  

## <a name="performing-a-nonauthoritative-restore"></a>Executar uma restauração não autoritativa

Use o procedimento a seguir para executar uma restauração não autoritativa de AD DS e uma restauração autoritativa do SYSVOL, ao mesmo tempo usando wbadmin.exe em um controlador de domínio que executa o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. O backup deve incluir explicitamente os dados de estado do sistema; um backup completo do servidor que é usado para recuperação de servidor completo não funcionarão. Para obter mais informações sobre como criar um backup de estado do sistema, consulte [backup de dados de estado do sistema](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Para executar uma restauração não autoritativa de AD DS e autoritativo restaurar do SYSVOL usando wbadmin.exe  
  
- Incluir o **authsysvol -** alternar no comando de recuperação, como mostrado no exemplo a seguir:  

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
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
