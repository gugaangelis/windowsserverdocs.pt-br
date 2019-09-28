---
title: ETAPA 3 instalar e configurar o CLIENT2
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b2d8cb1538de35a97ae83888d26abd7ad7bc8b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388323"
---
# <a name="step-3-install-and-configure-client2"></a>ETAPA 3 instalar e configurar o CLIENT2

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

CLIENT2 é um computador Windows 7 @ no__t-0 que é usado para demonstrar a compatibilidade com versões anteriores do acesso remoto em execução em servidores Windows Server 2016.  
  
1. Para instalar o sistema operacional em CLIENT2. Instale o Windows @ no__t-0 7 Enterprise ou o Windows @ no__t-1 7 Ultimate em CLIENT2.  
  
2. Para unir o CLIENT2 ao domínio CORP. Junte-se a CLIENT2 ao domínio corp.contoso.com.  
  
## <a name="to-install-the-operating-system-on-client2"></a>Para instalar o sistema operacional em CLIENT2  
  
1.  Inicie a instalação do Windows 7.  
  
2.  Quando for solicitado um nome de usuário, digite **Usuário1**. Quando for solicitado um nome de computador, digite **CLIENT2**.  
  
3.  Quando for solicitada uma senha, digite uma senha forte duas vezes.  
  
4.  Quando as configurações de proteção forem solicitadas, clique em **usar configurações recomendadas**.  
  
5.  Quando for solicitado o local atual do computador, clique em rede de **trabalho**.  
  
6.  Conecte o CLIENT2 a uma rede que tenha acesso à Internet e execute Windows Update para instalar as atualizações mais recentes para o Windows 7 e desconecte-se da Internet.  
  
7.  Conecte o CLIENT2 à sub-rede corpnet.  
  
## <a name="user-account-control"></a>Controle de conta de usuário  
Ao configurar o sistema operacional Windows 7, é necessário clicar em **continuar** na caixa de diálogo **controle de conta de usuário** (UAC) para algumas tarefas. Várias das tarefas de configuração exigem aprovação do UAC. Quando solicitado, sempre clique em **continuar** para autorizar essas alterações.  
  
## <a name="to-join-client2-to-the-corp-domain"></a>Para unir o CLIENT2 ao domínio CORP  
  
1.  Clique em **Iniciar**, clique com botão direito do **computador**e clique em **Propriedades**.  
  
2.  Na página **sistema** , na área **nome do computador, domínio e configurações do grupo de trabalho** , clique em **alterar configurações**.  
  
3.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
4.  Na caixa de diálogo **alterações no nome do computador/domínio** , clique em **domínio**, digite **Corp.contoso.com**e clique em **OK**.  
  
5.  Quando for solicitado um nome de usuário e uma senha, digite o nome de usuário e a senha da conta de domínio Usuário1 e clique em **OK**.  
  
6.  Quando você visualizar uma caixa de diálogo que lhe dê as boas vindas ao domínio corp.contoso.com, clique em **OK**.  
  
7.  Quando você vir uma caixa de diálogo solicitando a reinicialização do computador, clique em **OK**.  
  
8.  Na caixa de diálogo **Propriedades do sistema** , clique em **fechar**e, quando você vir uma caixa de diálogo solicitando a reinicialização do computador, clique em **reiniciar agora**.  
  
9. Depois que o computador for reiniciado, faça logon como CORP\User1.
