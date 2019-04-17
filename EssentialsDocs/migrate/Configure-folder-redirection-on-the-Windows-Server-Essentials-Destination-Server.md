---
title: Configurar o redirecionamento de pasta no servidor de destino do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7cc1207f36d3a921b49cc3ecd02acf3fe4fa243c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>Configurar o redirecionamento de pasta no servidor de destino do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Execute essa tarefa se redirecionamento de pasta está habilitado no servidor de origem.  
  
 Primeiro, exclua a configuração de política de grupo de redirecionamento de pasta antigos. Em seguida, use o painel do Windows Server Essentials para habilitar o redirecionamento de pasta no servidor de destino.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para excluir a configuração de política de grupo de redirecionamento de pasta antiga  
  
1.  No servidor de destino, abra o **Group Policy Management** ferramenta administrativa.  
  
2.  Em **Group Policy Management**, expanda **floresta:***YourNetworkDomainName*, expanda **domínios**, expanda *YourNetworkDomainName*e, em seguida, expanda **objetos de política de grupo**.  
  
3.  Clique com botão direito **redirecionamento de pasta de política de grupo SBS**e clique em **excluir**.  
  
4.  Clique com botão direito **modelos de segurança de diretiva de grupo SBS**e clique em **excluir**.  
  
5.  Leia o aviso e clique em **Sim**.  
  
6.  Fechar **gerenciamento de política de grupo**.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar o redirecionamento de pasta no servidor de destino  
  
1.  No servidor de destino, abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **dispositivos**.  
  
3.  No **dispositivos tarefas** painel, clique em **política de grupo de implementar**.  
  
4.  Sobre o **permitir política de grupo de redirecionamento de pasta** página, selecione as pastas para ser redirecionado e, em seguida, clique em **próxima**.  
  
5.  Sobre o **ativar configurações de política de segurança** página, clique em **concluir**.  
  
 Para aplicar a mudança para redirecionamento de pasta, os usuários da rede devem fazer logoff do computador e faça logon novamente. Isso garante a transferência de todas as pastas redirecionadas para o servidor de destino.
