---
title: Habilitar o redirecionamento de pastas no servidor de destino 1 do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: Habilitar o redirecionamento de pasta no servidor de destino do Windows Server Essentials
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 50995f0d03b400d6e44d16389afc69e5f25465ac
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432975"
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>Habilitar o redirecionamento de pastas no servidor de destino 1 do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Se o redirecionamento de pasta estiver habilitado no servidor de origem, você poderá executar essa tarefa.  
  
 Primeiro, use o painel do Windows Server Essentials para habilitar o redirecionamento de pasta no servidor de destino. Então, exclua a antiga configuração da Política de Grupo de Redirecionamento de Pasta.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar o redirecionamento de pasta no servidor de destino  
  
1.  No servidor de destino, abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **DISPOSITIVOS**.  
  
3.  No painel **Tarefas de Dispositivos**, clique em **Implementar Política de Grupo**.  
  
4.  Na página **Habilitar Política de Grupo de Redirecionamento de Pasta**, selecione as pastas a serem redirecionadas e, em seguida, clique em **Avançar**.  
  
5.  Na página **Habilitar Configurações de Política de Segurança** , clique em **Concluir**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para excluir a antiga configuração da Política de Grupo de Redirecionamento de Pasta  
  
1. No servidor de destino, abra a ferramenta administrativa **Gerenciamento de Política de Grupo**.  
  
2. Em **Gerenciamento de Política de Grupo**, expandaaa **Floresta:** <em>YourNetworkDomainName</em>, expandaaa **Domínios**, expandaaa *YourNetworkDomainName*, e expanda **Objetos de Política de Grupo**.  
  
3. Clique com botão direito em **Redirecionamento de Pasta W7PVP**e clique em **Excluir**.  
  
4. Leia o aviso e depois clique em **Sim**.  
  
5. Feche o **Gerenciamento de Política de Grupo**.  
  
   Para aplicar a alteração no redirecionamento de pasta, os usuários da rede devem fazer logoff do computador e fazer logon novamente. Isso garante a transferência de todas as pastas redirecionadas para o servidor de destino.
