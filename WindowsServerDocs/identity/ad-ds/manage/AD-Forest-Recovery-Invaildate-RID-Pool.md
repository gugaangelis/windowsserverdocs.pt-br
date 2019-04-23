---
title: Recuperação de floresta do AD - invalidar o Pool RID
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 46115991e48da301a8a739009bac27415ebe73df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842507"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Recuperação de floresta do AD - invalidar o pool RID atual  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para nós do Windows PowerShell para invalidar o pool RID atual em um controlador de domínio. Windows PowerShell é habilitada por padrão no Windows Server 2012 e Windows Server 2008 R2, mas não o Windows Server 2008 onde ele deve ser instalado por meio **adicionar recursos**. Ele pode ser [baixados](https://www.microsoft.com/download/details.aspx?id=20020) para ser executado no Windows Server 2003.  

Para verificar se o comando foi concluído com êxito, verifique a ID de evento 16654 (origem é Directory-Services-SAM) no log do sistema no Visualizador de eventos do Windows Server 2012. Versões anteriores do Windows não registra esse evento.  
  
> [!NOTE]
> Depois que você invalidar o pool RID, você receberá um erro quando você tenta primeiro criar a entidade de segurança (usuário, computador ou grupo). A tentativa de criar um objeto dispara uma solicitação para um novo pool RID. Repetição da operação é bem-sucedida pois o novo pool RID será alocado.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Para invalidar o pool RID atual  
  
- Abra uma sessão do Windows PowerShell com privilégios elevados, execute o seguinte comando e pressione ENTER:  

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry  
   $DomainSid = $Domain.objectSid  
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
   $RootDSE.UsePropertyCache = $false  
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
   $RootDSE.SetInfo()  
   ```  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
