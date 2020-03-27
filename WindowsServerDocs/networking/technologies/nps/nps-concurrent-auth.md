---
title: Aumentar autenticações simultâneas processadas pelo NPS
description: Este tópico fornece instruções sobre como configurar as autenticações simultâneas do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5397a1de41717dfc65ee0b0582c1289c6a53b33a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316291"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentar autenticações simultâneas processadas pelo NPS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter instruções sobre como configurar as autenticações simultâneas do servidor de políticas de rede.

Se você instalou o servidor de políticas de rede \(\) NPS em um computador que não seja um controlador de domínio e o NPS estiver recebendo um grande número de solicitações de autenticação por segundo, você poderá melhorar o desempenho do NPS aumentando o número de autenticações simultâneas permitidas entre o NPS e o controlador de domínio.

Para fazer isso, você deve editar a seguinte chave do registro: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Adicione um novo valor chamado **MaxConcurrentApi** e atribua a ele um valor de 2 a 5. 

>[!CAUTION]
>Se você atribuir um valor a **MaxConcurrentApi** que seja muito alto, o NPS poderá fazer uma carga excessiva em seu controlador de domínio.

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).