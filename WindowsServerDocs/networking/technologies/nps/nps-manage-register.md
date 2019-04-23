---
title: Registrar um NPS em um domínio do Active Directory
description: Você pode usar este tópico para registrar um servidor que executa o servidor de políticas de rede no Windows Server 2016 no NPS padrão domínio ou em outro domínio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a289ec519e5107576becf2905cd881cf9def190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877647"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Registrar um NPS em um domínio do Active Directory

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para registrar um servidor que executa o servidor de políticas de rede no Windows Server 2016 no NPS padrão domínio ou em outro domínio.

## <a name="register-an-nps-in-its-default-domain"></a>Registrar um NPS em seu domínio padrão

Você pode usar este procedimento para registrar um NPS no domínio onde o servidor é um membro do domínio. 

NPSs devem ser registrados no Active Directory para que eles tenham permissão para ler as propriedades de discagem das contas de usuário durante o processo de autorização. Registrar um NPS adiciona o servidor para o **servidores RAS e IAS** grupo no Active Directory.

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução destes procedimentos.

### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar um NPS em seu domínio padrão


1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do servidor de diretivas de rede.

2. Clique com botão direito **NPS (Local)** e, em seguida, clique em **registrar servidor no Active Directory**. A caixa de diálogo **Servidor de Políticas de Rede** é aberta.

3. Em **Servidor de Políticas de Rede**, clique em **OK** e em **OK** novamente.

## <a name="register-an-nps-in-another-domain"></a>Registrar um NPS em outro domínio

Para fornecer um NPS com permissão para ler as propriedades de discagem das contas de usuário no Active Directory, o NPS deve ser registrado no domínio onde residem as contas.

Você pode usar este procedimento para registrar um NPS em um domínio em que o NPS não é um membro do domínio.

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução destes procedimentos.

### <a name="to-register-an-nps-in-another-domain"></a>Para registrar um NPS em outro domínio

1. No controlador de domínio, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Active Directory Users and Computers**. O console de Usuários e Computadores do Active Directory é aberto.

2. Na árvore de console, navegue até o domínio onde você deseja que o NPS para ler as informações de conta de usuário e, em seguida, clique no **usuários** pasta. 

3. No painel de detalhes, clique com botão direito **servidores RAS e IAS**e, em seguida, clique em **propriedades**. O **RAS e IAS Servers Properties** caixa de diálogo é aberta.

4. No **RAS e IAS Servers Properties** caixa de diálogo, clique no **membros** guia, adicione cada um dos que você deseja registrar no domínio e, em seguida, clique em NPSs **Okey**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Para registrar um NPS em outro domínio usando comandos Netsh para NPS

1. Abra o Prompt de comando ou o windows PowerShell. 

2. Digite o seguinte no prompt de comando: **netsh nps adicionar registeredserver** &nbsp; *domínio* &nbsp; *server*, e pressione ENTER.

>[!NOTE]
>No comando anterior, *domínio* é o nome de domínio DNS do domínio no qual você deseja registrar o NPS, e *server* é o nome do computador NPS.

