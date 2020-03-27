---
title: Registrar um NPS em um domínio do Active Directory
description: Você pode usar este tópico para registrar um servidor que executa o servidor de diretivas de rede no Windows Server 2016 no domínio padrão do NPS ou em outro domínio.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 63d630250b0b24937a3dfc01bcba7ec63faa3c3e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315961"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Registrar um NPS em um domínio do Active Directory

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para registrar um servidor que executa o servidor de diretivas de rede no Windows Server 2016 no domínio padrão do NPS ou em outro domínio.

## <a name="register-an-nps-in-its-default-domain"></a>Registrar um NPS em seu domínio padrão

Você pode usar este procedimento para registrar um NPS no domínio em que o servidor é membro do domínio. 

NPSs deve ser registrado em Active Directory para que eles tenham permissão para ler as propriedades de discagem de contas de usuário durante o processo de autorização. O registro de um NPS adiciona o servidor ao grupo de **Servidores RAS e ias** em Active Directory.

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução destes procedimentos.

### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar um NPS em seu domínio padrão


1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do servidor de diretivas de rede é aberto.

2. Clique com o botão direito do mouse em **NPS (local)** e, em seguida, clique em **registrar servidor em Active Directory**. A caixa de diálogo **Servidor de Políticas de Rede** é aberta.

3. Em **Servidor de Políticas de Rede**, clique em **OK** e em **OK** novamente.

## <a name="register-an-nps-in-another-domain"></a>Registrar um NPS em outro domínio

Para fornecer um NPS com permissão para ler as propriedades de discagem de contas de usuário no Active Directory, o NPS deve ser registrado no domínio em que as contas residem.

Você pode usar este procedimento para registrar um NPS em um domínio em que o NPS não é membro do domínio.

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução destes procedimentos.

### <a name="to-register-an-nps-in-another-domain"></a>Para registrar um NPS em outro domínio

1. No controlador de domínio, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **Active Directory usuários e computadores**. O console de Usuários e Computadores do Active Directory é aberto.

2. Na árvore de console, navegue até o domínio em que você deseja que o NPS Leia as informações da conta de usuário e clique na pasta **usuários** . 

3. No painel de detalhes, clique com o botão direito do mouse em **Servidores RAS e ias**e clique em **Propriedades**. A caixa de diálogo **Propriedades de servidores RAS e ias** é aberta.

4. Na caixa de diálogo **Propriedades de servidores RAS e ias** , clique na guia **Membros** , adicione cada NPSs que você deseja registrar no domínio e clique em **OK**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Para registrar um NPS em outro domínio usando comandos netsh para NPS

1. Abra o prompt de comando ou o Windows PowerShell. 

2. Digite o seguinte no prompt de comando: **netsh nps add registeredserver** &nbsp;*domínio* &nbsp;*Server*e pressione Enter.

>[!NOTE]
>No comando anterior, *Domain* é o nome de domínio DNS do domínio em que você deseja registrar o NPS e *Server* é o nome do computador NPS.

