---
title: ETAPA 2 configurar o EDGE1
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cdd0d58f9f664211da44b125a466132ea0301bd2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970283"
---
# <a name="step-2-configure-edge1"></a>ETAPA 2 configurar o EDGE1

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O procedimento a seguir é executado no servidor DirectAccess:

## <a name="to-configure-directaccess-on-edge1"></a>Para configurar o DirectAccess no EDGE1

1.  Na tela **Iniciar** , digite**RAMgmtUI.exe**e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.

2.  No console de gerenciamento de acesso remoto, no painel esquerdo, clique em **configuração**.

3.  No painel central do console do, na área **etapa 2 servidor de acesso remoto** , clique em **Editar**.

4.  No assistente de **instalação do servidor de acesso remoto** , clique em **configuração de prefixo**. Na página **configuração do prefixo** , no **prefixo IPv6 atribuído aos computadores cliente do DirectAccess**, digite **2001: DB8:1: 1000::/59**e clique em **Avançar**.

5.  Clique em **Concluir**.

6.  No painel central do console do, clique em **concluir**.

7.  Na caixa de diálogo **revisão de acesso remoto** , examine as definições de configuração e clique em **aplicar**. Na caixa de diálogo **Aplicando Configurações do Assistente de Configuração de Acesso Remoto**, clique em **Fechar**.
