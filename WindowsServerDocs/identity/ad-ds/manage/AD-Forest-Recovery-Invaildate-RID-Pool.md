---
title: "Recuperação de floresta do AD - invalidar o Pool RID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adfs
ms.openlocfilehash: cb024356ae5f872e93448d73ea54b271fe3fae4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Recuperação de floresta do AD - invalidar o pool RID atual  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para nós do Windows PowerShell para invalidar o pool RID atual em um controlador de domínio. Windows PowerShell é habilitado por padrão no Windows Server 2012 e Windows Server 2008 R2, mas não o Windows Server 2008 onde ele deve ser instalado usando **adicionar recursos**. Ele pode ser [baixado](https://www.microsoft.com/download/details.aspx?id=20020) para ser executado no Windows Server 2003.  
  
 Para verificar se o comando foi concluído com êxito, verifique o ID de evento 16654 (origem é o diretório de serviços de SAM) no log do sistema no Visualizador de eventos no Windows Server 2012. Versões anteriores do Windows não registra esse evento.  
  
> [!NOTE]
>  Depois que você invalida o pool RID, você receberá um erro quando você tenta primeiro criar entidade de segurança (usuário, computador ou grupo). A tentativa de criar um objeto dispara uma solicitação para um novo pool RID. Repetição da operação de êxito porque o novo pool RID será alocado.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Para invalidar o pool RID atual  
  
1.  Abra uma sessão do Windows PowerShell com privilégios elevados, execute o seguinte comando e pressione ENTER:  
  
    ```  
    $Domain = New-Object System.DirectoryServices.DirectoryEntry  
    $DomainSid = $Domain.objectSid  
    $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
    $RootDSE.UsePropertyCache = $false  
    $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
    $RootDSE.SetInfo()  
    ```  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
