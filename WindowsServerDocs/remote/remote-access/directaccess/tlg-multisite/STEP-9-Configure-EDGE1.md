---
title: ETAPA 9 configurar o EDGE1
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ce86a75ac5b8d53874d2fc5c6743979506591680
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388229"
---
# <a name="step-9-configure-edge1"></a>ETAPA 9 configurar o EDGE1

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Os procedimentos a seguir são executados no servidor EDGE1:  
  
1. Configure os servidores DNS em EDGE1. É necessário configurar o servidor DNS do domínio corp2.corp.contoso.com em EDGE1.  
  
2. Configure o roteamento entre sub-redes. Configure o roteamento no EDGE1 para habilitar a comunicação entre as sub-redes corpnet e 2-corpnet.  
  
## <a name="IPv6"></a>Configurar os servidores DNS em EDGE1  
  
1.  No console do Gerenciador do Servidor, clique em **servidor local**e, na área **Propriedades** , ao lado de **corpnet**, clique no link.  
  
2.  Na janela conexões de rede, clique com o botão direito do mouse em **corpnet**e clique em **Propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Em **servidor DNS alternativo**, digite **10.2.0.1**. e clique em **OK**.  
  
5.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
6.  Em **servidor DNS alternativo**, digite **2001: DB8:2:: 1** e clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do corpnet** , clique em **fechar**.  
  
8.  Feche a janela **Conexões de Rede** .  
  
## <a name="ConfigRouting"></a>Configurar o roteamento entre sub-redes  
  
1.  Na tela **Iniciar** , digite**cmd. exe**, clique com o botão direito do mouse em **cmd**, clique em **avançado**e, em seguida, clique em **Executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Na janela do prompt de comando, digite os comandos a seguir. Depois de inserir cada comando, pressione ENTER.  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  Feche a janela do Prompt de Comando.  
  


