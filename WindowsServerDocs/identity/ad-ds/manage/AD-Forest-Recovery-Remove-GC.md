---
title: Recuperação de floresta do AD - remover o catálogo global
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: d730ce65fc179aee6a98f7cfc1a5b693bfcd6c93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817067"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Recuperação de floresta do AD - remover o catálogo global  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para remover o catálogo global de um controlador de domínio. 
  
 Restaurar um servidor de catálogo global de backup pode resultar no catálogo global contendo dados mais recentes para uma de suas réplicas parciais do domínio correspondente que é autoritativo para que a réplica parcial. Nesse caso, os dados mais recentes não serão removidos do catálogo global em ainda poderão ser replicados em outros servidores de catálogo global. Como resultado, mesmo que você restaurar um controlador de domínio era um servidor de catálogo global, ou inadvertidamente ou pois esse era o backup solitário confiáveis, você deve remover o catálogo global logo após a operação de restauração for concluída. Quando o catálogo global é removido, o computador remove todas as suas réplicas parciais. 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Para remover o catálogo global usando os serviços e Sites do Active Directory  
 
1. Abra o Gerenciador do servidor, clique em **ferramentas** e clique em **serviços e Sites do Active Directory**. 
2. Na árvore de console, expanda o **Sites** contêiner e, em seguida, selecione o site apropriado que contém o servidor de destino. 
3. Expanda o **servidores** contêiner e, em seguida, expanda o *server* objeto para o controlador de domínio do qual você deseja remover o catálogo global. 
4. Clique com botão direito **configurações de NTDS**e, em seguida, clique em **propriedades**. 
5. Desmarque a **Catálogo Global** caixa de seleção. 
   ![Remover o GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. Clique em **Aplicar**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Para remover o catálogo global usando Repadmin  
  
Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
