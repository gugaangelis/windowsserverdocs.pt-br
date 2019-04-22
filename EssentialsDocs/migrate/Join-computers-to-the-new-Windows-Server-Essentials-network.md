---
title: Fazer computadores ingressarem o network1 novos do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d94de050-3300-4323-a5ea-c824cb9cecc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c6abc11ba2ce8a9f1d32c6a884db6332586de78b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822637"
---
# <a name="join-computers-to-the-new-windows-server-essentials-network1"></a>Fazer computadores ingressarem o network1 novos do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 A próxima etapa no processo de migração é ingressar os computadores cliente para a nova rede do Windows Server Essentials e atualizar as configurações de diretiva de grupo.  
  
### <a name="domain-joined-client-computers"></a>Computadores cliente associados ao domínio  
 Navegue até **http://***destino servername***/ connect** e instalar o software Windows Server Connector como se fosse um novo computador. O processo de instalação é o mesmo para computadores cliente ingressados no domínio ou não ingressados no domínio.  
  
> [!NOTE]
>  O software Connector do Windows Server não dá suporte a computadores que executam o Windows XP ou Windows Vista. Se você tiver computadores que executam o Windows XP ou Windows Vista já associados ao domínio, ignore esta etapa.  
  
### <a name="non-domain-joined-client-computers"></a>Computadores cliente não associados ao domínio  
 Navegue até **http://***destino servername***/ connect** e instalar o software Windows Server Connector como se fosse um novo computador. O processo de instalação é o mesmo para associado a um domínio ou computadores cliente associados fora do domínio.  
  
> [!NOTE]
>  O software Connector do Windows Server não dá suporte a computadores que executam o Windows XP ou Windows Vista. Se você tiver computadores que executam o Windows XP ou Windows Vista já associados ao domínio, ignore esta etapa.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Certifique-se de que atualizou a política de grupo  
  
> [!NOTE]
>  Essa é uma etapa opcional e só é necessário se o servidor de origem foi configurado com configurações de política de grupo personalizadas, como redirecionamento de pasta.  
  
 Com o servidor de origem e o servidor de destino ainda online, você deve garantir que a política de grupo, as configurações foram replicadas do servidor de destino para os computadores cliente. Execute as seguintes etapas em cada computador cliente:  
  
1.  Abra uma janela de Prompt de Comando.  
  
2.  No prompt de comando, digite **GPRESULT /R** e pressione Enter.  
  
3.  Examine a saída resultante para a seção de política de grupo foi aplicada de: e certifique-se de que ela lista o servidor de destino, como **Destinationsrv**. Por exemplo:  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2008  
  
    ```  
  
4.  Se o servidor de destino não estiver listado, em um prompt de comando, digite **gpupdate /force**e pressione ENTER para atualizar as configurações de política de grupo. Em seguida, execute o procedimento anterior novamente.  
  
5.  Se o servidor de destino não for exibido, pode haver um erro nas configurações da política de grupo ou um erro no aplicá-las para esse computador cliente específico. Se o servidor de destino não for exibido, execute as seguintes etapas:  
  
    1.  Clique em **Iniciar**, clique em **Executar**, digite **rsop.msc** (Conjunto de Políticas Resultante) e pressione ENTER.  
  
    2.  Expanda a árvore com o X nele até chegar a um nó.  
  
    3.  Com o botão direito no nó e clique em **Erro de modo de exibição** para obter informações sobre por que as configurações de política de grupo não estão carregando no computador listado.
