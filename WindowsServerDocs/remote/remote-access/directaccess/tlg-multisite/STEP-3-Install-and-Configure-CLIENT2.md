---
title: Etapa 3 Instale e Configure CLIENT2
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
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d19f204e139433d11cf674c4ec39a134cde7eefa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813167"
---
# <a name="step-3-install-and-configure-client2"></a>Etapa 3 Instale e Configure CLIENT2

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

CLIENT2 é um Windows 7&reg; computador que é usado para demonstrar a versões anteriores a compatibilidade do acesso remoto em execução nos servidores do Windows Server 2016.  
  
1. Para instalar o sistema operacional em CLIENT2. Instalar Windows&reg; 7 Enterprise ou Windows&reg; 7 Ultimate em CLIENT2.  
  
2. Para ingressar CLIENT2 ao domínio CORP. Junte-se CLIENT2 ao domínio corp.contoso.com.  
  
## <a name="to-install-the-operating-system-on-client2"></a>Para instalar o sistema operacional em CLIENT2  
  
1.  Inicie a instalação do Windows 7.  
  
2.  Quando você for solicitado para um nome de usuário, digite **User1**. Quando você for solicitado para um nome de computador, digite **CLIENT2**.  
  
3.  Quando você for solicitado para uma senha, digite uma senha forte duas vezes.  
  
4.  Quando você for solicitado para as configurações de proteção, clique em **usar as configurações recomendadas**.  
  
5.  Quando você for solicitado para o local do computador atual, clique em **rede Work**.  
  
6.  Conecte o CLIENT2 a uma rede que tenha acesso à Internet e execute o Windows Update para instalar as atualizações mais recentes para o Windows 7 e, em seguida, desconecte da Internet.  
  
7.  Conecte-se CLIENT2 à sub-rede Corpnet.  
  
## <a name="user-account-control"></a>Controle de conta de usuário  
Quando você configurar o sistema operacional Windows 7, é necessário clicar **continuar** sobre o **User Account Control** caixa de diálogo (UAC) para algumas tarefas. Várias tarefas de configuração exigem a aprovação do UAC. Quando você for solicitado, sempre clicar **continuar** para autorizar a essas alterações.  
  
## <a name="to-join-client2-to-the-corp-domain"></a>Para unir CLIENT2 ao domínio CORP  
  
1.  Clique em **Iniciar**, clique com botão direito do **computador**e clique em **Propriedades**.  
  
2.  No **System** página, o **configurações de nome, domínio e grupo de trabalho do computador** área, clique em **alterar as configurações de**.  
  
3.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
4.  Sobre o **alterações de nome/domínio do computador** caixa de diálogo, clique em **domínio**, tipo **corp.contoso.com**e, em seguida, clique em **Okey**.  
  
5.  Quando você for solicitado um nome de usuário e senha, digite o nome de usuário e a senha da conta de domínio User1 e, em seguida, clique em **Okey**.  
  
6.  Quando você visualizar uma caixa de diálogo que lhe dê as boas vindas ao domínio corp.contoso.com, clique em **OK**.  
  
7.  Quando você vir uma caixa de diálogo caixa, que solicita que você reinicie o computador, clique em **Okey**.  
  
8.  Sobre o **propriedades do sistema** caixa de diálogo, clique em **fechar**, e quando você vir uma caixa de diálogo que solicita que você reinicie o computador, clique em **reiniciar agora**.  
  
9. Depois que o computador for reiniciado, faça logon como corp/User1.
