---
title: Recuperação de floresta do AD-remover o catálogo global
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: 3ba1336828ad6031ce7fb47a659d084494466e4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409095"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Recuperação de floresta do AD-removendo o catálogo global  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para remover o catálogo global de um controlador de domínio. 
  
 A restauração de um servidor de catálogo global a partir do backup pode fazer com que o catálogo global Mantenha os dados mais recentes de uma de suas réplicas parciais do que o domínio correspondente que é autoritativo para essa réplica parcial. Nesse caso, os dados mais recentes não serão removidos do catálogo global e poderão até mesmo replicar para outros servidores de catálogo global. Como resultado, mesmo que você tenha restaurado um controlador de domínio que era um servidor de catálogo global, seja inadvertidamente ou porque esse era o backup solitários confiável, você deverá remover o catálogo global logo após a conclusão da operação de restauração. Quando o catálogo global é removido, o computador remove todas as suas réplicas parciais. 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Para remover o catálogo global usando Active Directory sites e serviços  
 
1. Abra Gerenciador do Servidor, clique em **ferramentas** e em **Active Directory sites e serviços**. 
2. Na árvore de console, expanda o contêiner **sites** e selecione o site apropriado que contém o servidor de destino. 
3. Expanda o contêiner **servidores** e expanda o objeto de *servidor* para o controlador de domínio do qual você deseja remover o catálogo global. 
4. Clique com o botão direito do mouse em **Configurações NTDS**e clique em **Propriedades**. 
5. Desmarque a caixa de seleção **catálogo global** . 
   ![remover](media/AD-Forest-Recovery-Remove-GC/removegc1.png) GC
6. Clique em **Aplicar**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Para remover o catálogo global usando repadmin  
  
Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
