---
title: ETAPA 9 configurar EDGE1
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0a47a436bbd11c795caa8b402054ae0d2c3282f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281371"
---
# <a name="step-9-configure-edge1"></a>ETAPA 9 configurar EDGE1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Os procedimentos a seguir são executados no servidor EDGE1:  
  
1. Configure os servidores DNS em EDGE1. É necessário configurar o servidor DNS do domínio corp2.corp.contoso.com em EDGE1.  
  
2. Configure o roteamento entre sub-redes. Configure o roteamento em EDGE1 para habilitar a comunicação entre as sub-redes da rede corporativa e da rede corporativa de 2.  
  
## <a name="IPv6"></a>Configurar os servidores DNS em EDGE1  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Corpnet**, clique no link.  
  
2.  Na janela Conexões de rede, clique com botão direito **Corpnet**e, em seguida, clique em **propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Na **servidor DNS alternativo**, digite **10.2.0.1**. e clique em **OK**.  
  
5.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
6.  Na **servidor DNS alternativo**, digite **2001:db8:2::1** e, em seguida, clique em **Okey**.  
  
7.  Sobre o **propriedades da rede corporativa** caixa de diálogo, clique em **fechar**.  
  
8.  Feche a janela **Conexões de Rede** .  
  
## <a name="ConfigRouting"></a>Configurar o roteamento entre sub-redes  
  
1.  Sobre o **iniciar** tela, digite**cmd.exe**, clique com botão direito **cmd**, clique em **avançado**e, em seguida, clique em **executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Na janela do Prompt de comando, digite os comandos a seguir. Depois de inserir cada comando, pressione ENTER.  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  Feche a janela do Prompt de Comando.  
  


