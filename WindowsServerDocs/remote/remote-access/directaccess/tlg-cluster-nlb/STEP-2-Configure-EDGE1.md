---
title: ETAPA 2 configurar o EDGE1
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8705e69debec2f0a19f5cc010ed5ca254cc56741
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819309"
---
# <a name="step-2-configure-edge1"></a>ETAPA 2 configurar o EDGE1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O procedimento a seguir é executado no servidor DirectAccess:

## <a name="to-configure-directaccess-on-edge1"></a>Para configurar o DirectAccess no EDGE1
  
1.  Na tela **Iniciar** , digite**RAMgmtUI. exe**e pressione Enter. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, no painel esquerdo, clique em **configuração**.  
  
3.  No painel central do console do, na área **etapa 2 servidor de acesso remoto** , clique em **Editar**.  
  
4.  No assistente de **instalação do servidor de acesso remoto** , clique em **configuração de prefixo**. Na página **configuração do prefixo** , no **prefixo IPv6 atribuído aos computadores cliente do DirectAccess**, digite **2001: DB8:1: 1000::/59**e clique em **Avançar**.  
  
5.  Clique em **Concluir**.  
  
6.  No painel central do console do, clique em **concluir**.  
  
7.  Na caixa de diálogo **revisão de acesso remoto** , examine as definições de configuração e clique em **aplicar**. Na caixa de diálogo **Aplicando Configurações do Assistente de Configuração de Acesso Remoto**, clique em **Fechar**.
