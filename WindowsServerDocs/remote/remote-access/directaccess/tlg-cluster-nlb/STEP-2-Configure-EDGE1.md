---
title: Etapa 2 Configurar EDGE1
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c31e65fc7210849564bd0541085322a7a6c284e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838577"
---
# <a name="step-2-configure-edge1"></a>Etapa 2 Configurar EDGE1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

O procedimento a seguir é executado no servidor do DirectAccess:

## <a name="to-configure-directaccess-on-edge1"></a>Para configurar o DirectAccess em EDGE1
  
1.  Sobre o **inicie** tela, digite**RAMgmtUI.exe**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, no painel esquerdo, clique em **configuração**.  
  
3.  No painel central do console, nos **etapa 2 servidor de acesso remoto** área, clique em **editar**.  
  
4.  No **configuração do servidor de acesso remoto** assistente, clique em **configuração de prefixo**. No **configuração de prefixo** página, na **prefixo IPv6 atribuído a computadores cliente DirectAccess**, insira **2001:db8:1:1000:: / 59**e, em seguida, clique em **Avançar** .  
  
5.  Clique em **concluir**.  
  
6.  No painel central do console, clique em **concluir**.  
  
7.  Sobre o **análise de acesso remoto** caixa de diálogo, examine as configurações e, em seguida, clique em **aplicar**. Na caixa de diálogo **Aplicando Configurações do Assistente de Configuração de Acesso Remoto** , clique em **Fechar**.
