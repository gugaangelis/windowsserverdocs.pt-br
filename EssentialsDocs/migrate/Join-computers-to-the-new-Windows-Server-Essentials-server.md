---
title: Ingressar computadores para a nova servidor1 Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdfa9504-9881-4265-b308-c7ee8721bfaa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1a67cda9e4b04e8d861232b48f45915fb2b460d1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="join-computers-to-the-new-windows-server-essentials-server1"></a>Ingressar computadores para a nova servidor1 Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 A próxima etapa no processo de migração é ingressar computadores cliente para a nova rede do Windows Server Essentials e atualizar as configurações de política de grupo.  
  
> [!NOTE]
>  Se um computador cliente já está associado ao servidor de origem, você deve primeiro desinstalar o software de conector no computador cliente antes que o computador pode se conectar ao servidor de destino.  
  
 O processo para se conectar a um computador cliente para o servidor é o mesmo para computadores de domínio ou ingressado no domínio.  
  
-   Navegue até **http://***destino nomedoservidor***/ connect** e instalar o software do Windows Server Connector conforme se tiver havido um novo computador.  
  
> [!NOTE]
>  O software do Windows Server Connector não oferece suporte a computadores que executam o Windows XP ou Windows Vista. Se você tiver computadores com Windows XP ou Windows Vista que já ingressou no domínio, você pode pular esta etapa.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Certifique-se de que atualizou a política de grupo  
  
> [!NOTE]
>  Essa é uma etapa opcional e só é necessária se o servidor de origem foi configurado com configurações de política de grupo personalizadas como redirecionamento de pasta.  
  
 Enquanto o servidor de origem e o servidor de destino estão ainda online, você deve garantir que a política de grupo configurações foram replicados do servidor de destino para os computadores cliente. Execute as seguintes etapas em cada computador cliente:  
  
1.  Abra uma janela de prompt de comando.  
  
2.  No prompt de comando, digite **GPRESULT /R**, e pressione ENTER.  
  
3.  Examine a saída resultante para a seção política de grupo foi aplicada de: e certifique-se de que o servidor de destino, ela lista como **DestinationSrv.Domain.local**. Por exemplo:  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2011  
  
    ```  
  
4.  Se o servidor de destino não estiver listado, em um prompt de comando, digite **gpupdate /force**, e pressione ENTER para atualizar as configurações de política de grupo. Em seguida, execute novamente o procedimento anterior.  
  
5.  Se o servidor de destino não for exibido, pode haver um erro nas configurações da política de grupo ou um erro no aplicá-las neste computador cliente específica. Se o servidor de destino não aparecer, execute as seguintes etapas:  
  
    1.  Clique em **iniciar**, clique em **executar**, tipo **rsop.msc** (conjunto de diretivas resultante), e pressione ENTER.  
  
    2.  Expanda a árvore com o X nela até chegar a um nó.  
  
    3.  Clique com botão direito no nó e clique em **erro de modo de exibição** para obter informações sobre por que as configurações de política de grupo estão falhando no computador listado.
