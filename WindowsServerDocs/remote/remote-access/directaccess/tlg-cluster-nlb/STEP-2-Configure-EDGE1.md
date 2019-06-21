---
title: Etapa 2 Configurar EDGE1
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 03d7d85f730cf792238aef372337030861cd6a9d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281674"
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
