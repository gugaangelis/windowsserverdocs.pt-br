---
title: Cancelar o registro de um servidor NPS de um domínio do Active Directory
description: Você pode usar este tópico para registrar um servidor que executa o servidor de políticas de rede no Windows Server 2016 no domínio do padrão de servidor NPS ou em outro domínio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55c3b00146706831351ce63d1e5b74f45d7b9be1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="unregister-an-nps-server-from-an-active-directory-domain"></a>Cancelar o registro de um servidor NPS de um domínio do Active Directory

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

No processo de gerenciar a implantação do servidor NPS, talvez seja útil para mover um servidor NPS para outro domínio, para substituir um servidor NPS ou para desativar um servidor NPS. 

Quando você move ou encerrar um servidor NPS, você pode cancelar o registro do servidor NPS nos domínios do Active Directory em que o servidor NPS tem permissão para ler as propriedades de contas de usuário no Active Directory.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar esses procedimentos.

## <a name="to-unregister-an-nps-server"></a>Para cancelar o registro de um servidor NPS

1. No controlador de domínio, no Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**. O console de usuários e Active Directory computadores é aberto.

2. Clique em **usuários**e clique duas vezes em **servidores RAS e IAS**.

3. Clique no **membros** guia e selecione o servidor NPS que você deseja cancelar.

4. Clique em **remover**, clique em **Sim**e clique em **Okey**.

