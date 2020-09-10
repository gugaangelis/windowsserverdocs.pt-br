---
title: Configurar o redirecionamento de pastas no servidor de destino do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: bc67b6bae8492d7e176ec5e46d268d171ad7c94a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622929"
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>Configurar o redirecionamento de pastas no servidor de destino do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Realize essa tarefa se o redirecionamento de pasta estiver habilitado no servidor de origem.

 Primeiro, exclua a configuração da política de grupo de redirecionamento de pasta antiga. Em seguida, use o painel do Windows Server Essentials para habilitar o redirecionamento de pasta no servidor de destino.

### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para excluir a antiga configuração da Política de Grupo de Redirecionamento de Pasta

1. No servidor de destino, abra a ferramenta administrativa **Gerenciamento de Política de Grupo**.

2. Em **Gerenciamento de Política de Grupo**, expandaaa **Floresta:**<em>YourNetworkDomainName</em>, expandaaa **Domínios**, expandaaa *YourNetworkDomainName*, e expanda **Objetos de Política de Grupo**.

3. Clique com botão direito em **Redirecionamento de Pasta de Política de Grupo do SBS** e clique em **Excluir**.

4. Clique com botão direito em **Modelos de Segurança da Política de Grupo do SBS** e clique em **Excluir**.

5. Leia o aviso e depois clique em **Sim**.

6. Feche o **Gerenciamento de Política de Grupo**.

### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar o redirecionamento de pasta no servidor de destino

1. No servidor de destino, abra o painel do Windows Server Essentials.

2. Na barra de navegação, clique em **Dispositivos**.

3. No painel **Tarefas de Dispositivos**, clique em **Implementar Política de Grupo**.

4. Na página **Habilitar Política de Grupo de Redirecionamento de Pasta**, selecione as pastas a serem redirecionadas e, em seguida, clique em **Avançar**.

5. Na página **Habilitar Configurações de Política de Segurança**, clique em **Concluir**.

   Para aplicar a alteração no redirecionamento de pasta, os usuários da rede devem fazer logoff do computador e fazer logon novamente. Isso garante a transferência de todas as pastas redirecionadas para o servidor de destino.
