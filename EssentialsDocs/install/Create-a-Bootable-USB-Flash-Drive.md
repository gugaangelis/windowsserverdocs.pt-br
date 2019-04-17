---
title: "Criar uma unidade Flash USB inicializável"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9d587329e1141040b2511e1574649f1844dcec90
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-bootable-usb-flash-drive"></a>Criar uma unidade Flash USB inicializável

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode criar uma unidade flash USB inicializável usar para implantar o Windows Server Essentials. A primeira etapa é preparar a unidade flash USB usando o DiskPart, que é um utilitário de linha de comando. Para obter informações sobre o DiskPart, consulte [opções de linha de comando de DiskPart ](https://go.microsoft.com/fwlink/?LinkId=207073).  
  
 Para cenários adicionais em que você pode querer criar ou usar um USB inicializável unidade flash, consulte os seguintes tópicos:  
  
-   [Restaurar um sistema completo de um backup do computador cliente existente](https://technet.microsoft.com/library/jj713539.aspx#BKMK_CreateBootable)  
  
-   [Restaurar ou reparar o servidor que executa o Windows Server Essentials](https://technet.microsoft.com/library/jj593197.aspx#BKMK_Restore_2)  
  
### <a name="to-create-a-bootable-usb-flash-drive"></a>Para criar uma unidade flash USB inicializável  
  
1.  Insira uma unidade flash USB em um computador em execução.  
  
2.  Abra uma janela de Prompt de comando como administrador.  
  
3.  Tipo `diskpart`.  
  
4.  Na nova janela de linha de comando que aparece, para determinar o pen drive número ou uma letra de unidade, no prompt de comando, digite `list disk`e clique em ENTER. O `list disk`comando exibe todos os discos no computador. Observe o número da unidade ou uma letra de unidade da unidade flash USB.  
  
5.  No prompt de comando, digite `select disk <X>`, onde X é o número de unidade ou letra de unidade de USB unidade flash e, em seguida, clique em ENTER.  
  
6.  Tipo `clean`e clique em ENTER. Este comando exclui todos os dados da unidade flash USB.  
  
7.  Para criar uma nova partição primária na unidade flash USB, digite `create part pri`e clique em ENTER.  
  
8.  Para selecionar a partição que você acabou de criar, digite `select part 1`e clique em ENTER.  
  
9. Para formatar a partição, digite `format fs=ntfs quick`e clique em ENTER.  
  
    > [!IMPORTANT]
    >  Se sua plataforma de servidor dá suporte a Unified Extensible Firmware Interface (UEFI), você deve formatar a unidade flash USB como FAT32 em vez de NTFS. Para formatar a partição como FAT32, digite `format fs=fat32 quick`e clique em ENTER.  
  
10. Tipo `active`e clique em ENTER.  
  
11. Tipo `exit`e clique em ENTER.  
  
12. Quando você terminar de preparação de sua imagem personalizada, salvá-lo para a raiz da unidade flash USB.  
  
## <a name="see-also"></a>Consulte também  

 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)   

 [Introdução ao Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)   

 [Como podemos ajudar você?](https://windows.microsoft.com/windows/support)
