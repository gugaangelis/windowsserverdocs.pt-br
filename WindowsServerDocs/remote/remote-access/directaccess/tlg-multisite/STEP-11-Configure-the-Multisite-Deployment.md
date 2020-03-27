---
title: ETAPA 11 configurar a implantação multissite
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d90b20716c49b2ea0b1cd002a1c1933fbd6e26e5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314570"
---
# <a name="step-11-configure-the-multisite-deployment"></a>ETAPA 11 configurar a implantação multissite

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Para configurar uma implantação multissite, faça alterações no assistente de configuração de acesso remoto atual no EDGE1, habilite o recurso multissite e, em seguida, adicione 2-EDGE1 como um segundo ponto de entrada.  
  
- Configurar o acesso remoto no EDGE1  
  
- Habilitar a configuração multissite no EDGE1  
  
- Adicionar 2-EDGE1 como um segundo ponto de entrada  
  
## <a name="configure-remote-access-on-edge1"></a><a name="configDA"></a>Configurar o acesso remoto no EDGE1  
  
1.  Na tela **Iniciar** , digite**RAMgmtUI. exe**e pressione Enter. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**.  
  
3.  No painel central do console do, na área **etapa 2 servidor de acesso remoto** , clique em **Editar**.  
  
4.  Clique em **configuração de prefixo**. Na página **configuração de prefixo** , em **prefixos IPv6 de rede interna**, digite **2001: DB8:1::/64; 2001: DB8:2::/64**. Em **prefixo IPv6 atribuído a computadores cliente DirectAccess**, **digite 2001: DB8:1: 1000::/64**, clique em **Avançar**e, em seguida, clique em **concluir**.  
  
5.  No painel central do console do, na área **servidores de infraestrutura da etapa 3** , clique em **Editar**.  
  
6.  Clique em **lista de pesquisa de sufixo DNS**. Na página **lista de pesquisa de sufixo DNS** , verifique se a caixa de seleção **configurar clientes DirectAccess com o sufixo de cliente DNS** está marcada e se os sufixos de domínio **Corp.contoso.com** e **corp2.Corp.contoso.com** aparecem na lista **sufixos de domínio a serem usados** , clique em **Avançar**e em concluir.  
  
7.  No painel central do console do, clique em **concluir**.  
  
8.  Na caixa de diálogo **revisão de acesso remoto** , examine as definições de configuração e clique em **aplicar**. Na caixa de diálogo **Aplicando Configurações do Assistente de Configuração de Acesso Remoto**, clique em **Fechar**.  
  
9. No painel **tarefas** , clique em **Atualizar servidores de gerenciamento**e clique em **Fechar** quando terminar.  
  
## <a name="enable-multisite-configuration-on-edge1"></a><a name="EnabledMultisite"></a>Habilitar a configuração multissite no EDGE1  
  
1.  No console de gerenciamento de acesso remoto, no painel **tarefas** , clique em **habilitar multissite**.  
  
2.  No Assistente para habilitar implantação multissite, na página **antes de começar** , clique em **Avançar**.  
  
3.  Na página **nome da implantação** , em **nome da implantação multissite**, digite **contoso**, no **primeiro nome do ponto de entrada**, digite **Edge1-site**e clique em **Avançar**.  
  
4.  Na página **seleção de ponto de entrada** , clique em **atribuir pontos de entrada automaticamente e permita que os clientes selecionem manualmente**e clique em **Avançar**.  
  
5.  Na página **balanceamento de carga global** , clique em **não, não use balanceamento de carga global**e clique em **Avançar**.  
  
6.  Na página **suporte ao cliente** , clique em **permitir que computadores cliente que executam o Windows 7 acessem esse ponto de entrada**e clique em **Adicionar**.  
  
7.  Na caixa de diálogo **Selecionar grupos** , em **digite os nomes de objeto a serem selecionados**, digite **Win7_Clients_Site1**, clique em **OK**e, em seguida, clique em **Avançar**.  
  
8.  Na página **configurações de GPO do cliente** , clique em **Avançar**.  
  
9. Na página **Resumo** , clique em **confirmar**.  
  
10. Na caixa de diálogo **habilitando a implantação** multissite, clique em **fechar** e, em seguida, no assistente habilitar implantação multissite, clique em **Fechar**.  
  
## <a name="add-2-edge1-as-a-second-entry-point"></a><a name="AddEP"></a>Adicionar 2-EDGE1 como um segundo ponto de entrada  
  
1.  No console de gerenciamento de acesso remoto, no painel **tarefas** , clique em **Adicionar um ponto de entrada**.  
  
2.  No assistente Adicionar um ponto de entrada, na página **detalhes do ponto de entrada** , em **servidor de acesso remoto**, digite **2-EDGE1.corp2.Corp.contoso.com**, em nome do **ponto de entrada**, digite **2-EDGE1-site**e clique em **Avançar**.  
  
3.  Na página **topologia de rede** , clique em **borda**e em **Avançar**.  
  
4.  Na página **nome da rede ou endereço IP** , em **digite o nome público ou o endereço IP usado pelos clientes para se conectar ao servidor de acesso remoto**, digite **2-EDGE1.contoso.com**e clique em **Avançar**.  
  
5.  Na página **adaptadores de rede** , verifique se o **adaptador externo** é **Internet**, se **o adaptador interno** é **2-corpnet**, se o certificado é **CN = 2-EDGE1.contoso.com**e clique em **Avançar**.  
  
6.  Na página **configuração do prefixo** , em **prefixo IPv6 atribuído aos computadores cliente do DirectAccess**, digite **2001: DB8:2: 2000::/64**e clique em **Avançar**.  
  
7.  Na página **suporte ao cliente** , clique em **permitir que computadores cliente que executam o Windows 7 acessem esse ponto de entrada**e clique em **Adicionar**.  
  
8.  Na caixa de diálogo **Selecionar grupos** , em **digite os nomes de objeto a serem selecionados**, digite **Win7_Clients_Site2**, clique em **OK**e, em seguida, clique em **Avançar**.  
  
9. Na página **configurações de GPO do cliente** , clique em **Avançar**.  
  
10. Na página **configurações do GPO do servidor** , clique em **Avançar**.  
  
11. Na página **Resumo** , clique em **confirmar**.  
  
12. Na caixa de diálogo **Adicionar ponto de entrada** , clique em **fechar** e, em seguida, no assistente Adicionar um ponto de entrada, clique em **Fechar**.  
  


