---
title: ETAPA 11 configurar uma implantação multissite
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2dc70d9c89b19a3e38d7f6cd682f047530f87cc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839517"
---
# <a name="step-11-configure-the-multisite-deployment"></a>ETAPA 11 configurar uma implantação multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Para configurar uma implantação multissite, fazer alterações para o Assistente de configuração de acesso remoto atual em EDGE1, habilitar o recurso de vários sites e, em seguida, adicione EDGE1 2 como um segundo ponto de entrada.  
  
- Configurar o acesso remoto em EDGE1  
  
- Habilitar a configuração multissite em EDGE1  
  
- Adicionar EDGE1 2 como um segundo ponto de entrada  
  
## <a name="configDA"></a>Configurar o acesso remoto em EDGE1  
  
1.  Sobre o **inicie** tela, digite**RAMgmtUI.exe**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**.  
  
3.  No painel central do console, nos **etapa 2 servidor de acesso remoto** área, clique em **editar**.  
  
4.  Clique em **configuração de prefixo**. Sobre o **configuração de prefixo** página, na **prefixos IPv6 da rede interna**, insira **2001:db8:1:: / 64; 2001:db8:2:: 64**. Na **prefixo IPv6 atribuído a computadores cliente DirectAccess**, insira **2001:db8:1:1000:: 64**, clique em **próxima**e, em seguida, clique em **concluir** .  
  
5.  No painel central do console, nos **etapa 3: servidores de infraestrutura** área, clique em **editar**.  
  
6.  Clique em **lista de pesquisa de sufixos DNS**. No **lista de pesquisa de sufixos DNS** página, certifique-se de que o **clientes de configurar o DirectAccess com a lista de pesquisa de sufixos DNS client** caixa de seleção é marcada e que o **corp.contoso.com** e **corp2.corp.contoso.com** sufixos de domínio aparecem no **sufixos de domínio para usar** , clique em **próximo**e, em seguida, clique em Concluir.  
  
7.  No painel central do console, clique em **concluir**.  
  
8.  Sobre o **análise de acesso remoto** caixa de diálogo, examine as configurações e, em seguida, clique em **aplicar**. Na caixa de diálogo **Aplicando Configurações do Assistente de Configuração de Acesso Remoto** , clique em **Fechar**.  
  
9. No **tarefas** painel, clique em **atualizar servidores de gerenciamento**e clique em **fechar** quando terminar.  
  
## <a name="EnabledMultisite"></a>Habilitar a configuração multissite em EDGE1  
  
1.  No Console de gerenciamento de acesso remoto, nos **tarefas** painel, clique em **habilitar multissite**.  
  
2.  No assistente habilitar implantação multissite, sobre o **antes de começar** , clique em **próxima**.  
  
3.  Sobre o **nome de implantação** página, na **nome da implantação multissite**, tipo **Contoso**, no **primeiro ponto de entrada nome**, tipo **Site Edge1**e, em seguida, clique em **próxima**.  
  
4.  Sobre o **seleção de ponto de entrada** , clique em **atribuir pontos de entrada automaticamente, e permitir que os clientes selecionar manualmente**e, em seguida, clique em **próxima**.  
  
5.  Sobre o **balanceamento de carga Global** , clique em **não, não use balanceamento de carga global**e, em seguida, clique em **próxima**.  
  
6.  Sobre o **suporte ao cliente** , clique em **permitem que o cliente computadores que executam o Windows 7 para acessar esse ponto de entrada**e clique em **adicionar**.  
  
7.  Sobre o **selecionar grupos** na caixa **digite os nomes de objeto para selecionar**, tipo **Win7_Clients_Site1**, clique em **Okey**e, em seguida, clique em **Próxima**.  
  
8.  Sobre o **configurações de GPO do cliente** , clique em **próxima**.  
  
9. Sobre o **resumo** , clique em **confirmar**.  
  
10. Sobre o **habilitando a implantação de multissite** caixa de diálogo, clique em **fechar** e, em seguida, no assistente habilitar implantação multissite, clique em **fechar**.  
  
## <a name="AddEP"></a>Adicionar EDGE1 2 como um segundo ponto de entrada  
  
1.  No Console de gerenciamento de acesso remoto, nos **tarefas** painel, clique em **adicionar um ponto de entrada**.  
  
2.  Em Adicionar um ponto de entrada no assistente, no **detalhes do ponto de entrada** página, na **servidor de acesso remoto**, tipo **2 edge1.corp2.corp.contoso.com**, no **entrada nome do ponto**, digite **Edge1-2-Site**e, em seguida, clique em **próximo**.  
  
3.  Sobre o **topologia de rede** , clique em **borda**e, em seguida, clique em **próxima**.  
  
4.  Sobre o **nome de rede ou o endereço IP** página, na **digite o nome público ou endereço IP usado por clientes para se conectar ao servidor de acesso remoto**, tipo **2 edge1.contoso.com**, e em seguida, clique em **próxima**.  
  
5.  Sobre o **adaptadores de rede** página, certifique-se de que o **adaptador externo** é **Internet**, o **adaptador interno** é **2 -Corpnet**, o certificado está **CN = 2 edge1.contoso.com**e, em seguida, clique em **próximo**.  
  
6.  No **configuração de prefixo** página, na **prefixo IPv6 atribuído a computadores cliente DirectAccess**, tipo **2001:db8:2:2000:: 64**e, em seguida, clique em **Avançar** .  
  
7.  Sobre o **suporte ao cliente** , clique em **permitem que o cliente computadores que executam o Windows 7 para acessar esse ponto de entrada**e clique em **adicionar**.  
  
8.  Sobre o **selecionar grupos** na caixa **digite os nomes de objeto para selecionar**, tipo **Win7_Clients_Site2**, clique em **Okey**e, em seguida, clique em **Próxima**.  
  
9. Sobre o **configurações de GPO do cliente** , clique em **próxima**.  
  
10. Sobre o **configurações de GPO do servidor** , clique em **próxima**.  
  
11. Sobre o **resumo** página, clique em **confirmar**.  
  
12. Sobre o **adicionando ponto de entrada** caixa de diálogo, clique em **fechar** e, em seguida, na caixa Adicionar um Assistente de ponto de entrada, clique em **fechar**.  
  


