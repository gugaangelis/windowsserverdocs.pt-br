---
title: "Recuperação de floresta do AD - remover o catálogo global"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adfs
ms.openlocfilehash: b7da7c03cbae4691e8f902f1be0cb33c0c912248
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Recuperação de floresta do AD - removendo o catálogo global  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para remover o catálogo global de um controlador de domínio.  
  
 Restauração de um servidor de catálogo global de backup pode resultar no catálogo global segurando dados mais recentes para uma das suas réplicas parciais do domínio correspondente autoritativo daquela réplica parcial. Nesse caso, os dados mais recentes não serão removidos do catálogo global e talvez até mesmo duplicar para outros servidores de catálogo global. Como resultado, mesmo se você restaurar um controlador de domínio que foi um servidor de catálogo global, ou inadvertidamente ou porque foi o backup solitária confiáveis, você deve remover o catálogo global logo após a operação de restauração for concluída. Quando o catálogo global é removido, o computador remove todas as suas réplicas parciais.  
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Para remover o catálogo global usando serviços e Sites do Active Directory  
 
1.  Abra o Gerenciador do servidor, clique em **ferramentas** e clique em **serviços e Sites do Active Directory**.  
2.  Na árvore de console, expanda o **Sites** contêiner e selecione o site apropriado que contém o servidor de destino.  
3.  Expanda o **servidores** contêiner e, em seguida, expanda o *servidor* objeto para o controlador de domínio do qual você deseja remover o catálogo global.  
4.  Clique com botão direito **configurações NTDS**e clique em **propriedades**.  
5.  Limpar o **Catálogo Global** caixa de seleção.  
![Remover GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6.  Clique em **aplicar**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Para remover o catálogo global usando Repadmin  
  
1.  Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  
  
    ```  
    repadmin.exe /options DC_NAME –IS_GC  
    ```  
  
 ## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
