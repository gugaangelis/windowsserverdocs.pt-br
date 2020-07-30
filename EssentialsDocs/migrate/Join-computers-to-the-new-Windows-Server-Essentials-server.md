---
title: Unir computadores ao novo Server1 do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: cdfa9504-9881-4265-b308-c7ee8721bfaa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cbc2276eb3c627ebb94469e0acef2b57d086b5e7
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180742"
---
# <a name="join-computers-to-the-new-windows-server-essentials-server1"></a>Unir computadores ao novo Server1 do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>
 A próxima etapa do processo de migração é unir computadores cliente à nova rede do Windows Server Essentials e atualizar Política de Grupo configurações.

> [!NOTE]
>  Se um computador cliente já está associado ao servidor de origem, você deve primeiro desinstalar o software Connector no computador cliente antes que o computador pode se conectar ao servidor de destino.

 O processo para se conectar a um computador cliente para o servidor é o mesmo para computadores associados ao domínio ou não ingressou no domínio.

- Navegue até **http://**<em>destino servername</em>**/connect** e instale o software Windows Server Connector como se fosse um novo computador.

> [!NOTE]
>  O software Connector do Windows Server não dá suporte a computadores que executam o Windows XP ou Windows Vista. Se você tiver computadores que executam o Windows XP ou Windows Vista já associados ao domínio, ignore esta etapa.

### <a name="ensure-that-group-policy-has-updated"></a>Certifique-se de que atualizou a política de grupo

> [!NOTE]
>  Essa é uma etapa opcional e só é necessário se o servidor de origem foi configurado com configurações de política de grupo personalizadas, como redirecionamento de pasta.

 Com o servidor de origem e o servidor de destino ainda online, você deve garantir que a política de grupo, as configurações foram replicadas do servidor de destino para os computadores cliente. Execute as seguintes etapas em cada computador cliente:

1.  Abra uma janela de prompt de comando.

2.  No prompt de comando, digite **GPRESULT /R** e pressione Enter.

3.  Examine a saída resultante para a seção Política de Grupo foi aplicada de: e verifique se ela lista o servidor de destino, como **DestinationSrv. domain. local**. Por exemplo:

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

4.  Se o servidor de destino não estiver listado, em um prompt de comando, digite  **gpupdate /force** e pressione ENTER para atualizar as configurações de política de grupo. Em seguida, execute o procedimento anterior novamente.

5.  Se o servidor de destino não for exibido, pode haver um erro nas configurações da política de grupo ou um erro no aplicá-las para esse computador cliente específico. Se o servidor de destino não for exibido, execute as seguintes etapas:

    1.  Clique em **Iniciar**, clique em **Executar**, digite **rsop.msc** (Conjunto de Políticas Resultante) e pressione ENTER.

    2.  Expanda a árvore com o X nela até chegar a um nó.

    3.  Com o botão direito no nó e clique em **Erro de modo de exibição** para obter informações sobre por que as configurações de política de grupo não estão carregando no computador listado.
