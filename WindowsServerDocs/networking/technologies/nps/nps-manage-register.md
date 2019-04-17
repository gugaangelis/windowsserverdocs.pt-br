---
title: Registrar um servidor NPS em um domínio do Active Directory
description: Você pode usar este tópico para registrar um servidor que executa o servidor de políticas de rede no Windows Server 2016 no domínio do padrão de servidor NPS ou em outro domínio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ba18f6c994e8b15da3a07a3e37550d5dbff24af
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="register-an-nps-server-in-an-active-directory-domain"></a>Registrar um servidor NPS em um domínio do Active Directory

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para registrar um servidor que executa o servidor de políticas de rede no Windows Server 2016 no domínio do padrão de servidor NPS ou em outro domínio.

## <a name="register-an-nps-server-in-its-default-domain"></a>Registrar um servidor NPS no domínio padrão

Você pode usar este procedimento para registrar um servidor NPS no domínio em que o servidor é um membro do domínio. 

Os servidores NPS devem ser registrados no Active Directory para que ele tenha permissão para ler as propriedades de discagem rápida de contas de usuário durante o processo de autorização. Registrar um servidor NPS adiciona o servidor para o **servidores RAS e IAS** grupo no Active Directory.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar esses procedimentos.

### <a name="to-register-an-nps-server-in-its-default-domain"></a>Para registrar um servidor NPS no domínio padrão


1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. O console do servidor de política de rede é aberta.

2. Clique com botão direito **NPS (Local)**e clique em **registrar servidor no Active Directory**. O **NPS** caixa de diálogo é aberta.

3. Em **NPS**, clique em **Okey**e clique em **Okey** novamente.

## <a name="register-an-nps-server-in-another-domain"></a>Registrar um servidor NPS em outro domínio

Para fornecer um servidor NPS com permissão para ler as propriedades de discagem rápida de contas de usuário no Active Directory, o servidor NPS deve ser registrado no domínio onde residem as contas.

Você pode usar este procedimento para registrar um servidor NPS em um domínio em que o servidor NPS não é um membro do domínio.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar esses procedimentos.

### <a name="to-register-an-nps-server-in-another-domain"></a>Para registrar um servidor NPS em outro domínio

1. No controlador de domínio, no Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**. O console de usuários e Active Directory computadores é aberto.

2. Na árvore de console, navegue até o domínio onde deseja que o servidor NPS para ler as informações de conta de usuário e, em seguida, clique no **usuários** pasta. 

3. No painel de detalhes, clique com botão direito **servidores RAS e IAS**e clique em **propriedades**. O **RAS e propriedades dos servidores IAS** caixa de diálogo é aberta.

4. No **RAS e propriedades dos servidores IAS** caixa de diálogo, clique no **membros** guia, adicione cada um dos servidores NPS que você deseja registrar no domínio e, em seguida, clique em **Okey**.


### <a name="to-register-an-nps-server-in-another-domain-by-using-netsh-commands-for-nps"></a>Para registrar um servidor NPS em outro domínio usando comandos Netsh para NPS

1. Abra o Prompt de comando ou o windows PowerShell. 

2. Digite o seguinte no prompt de comando: **netsh nps adicionar registeredserver**&nbsp;*domínio*&nbsp;*servidor*, e pressione ENTER.

>[!NOTE]
>No comando acima, *domínio* é o nome de domínio DNS do domínio em que você deseja registrar o servidor NPS, e *servidor* é o nome do computador do servidor NPS.

