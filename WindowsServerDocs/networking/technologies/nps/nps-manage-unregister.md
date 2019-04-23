---
title: Cancelar o registro de um NPS de um domínio do Active Directory
description: Você pode usar este tópico para registrar um servidor que executa o servidor de políticas de rede no Windows Server 2016 no NPS padrão domínio ou em outro domínio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe4773efd89aeb413b3793f874ad6a1b030294a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864347"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Cancelar o registro de um NPS de um domínio do Active Directory

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

No processo de gerenciar sua implantação do NPS, você talvez ache útil para mover um NPS para outro domínio, para substituir um NPS ou desativar um NPS. 

Quando você move ou encerrar um NPS, você pode cancelar o registro o NPS em domínios do Active Directory em que o NPS tem permissão para ler as propriedades de contas de usuário no Active Directory.

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução destes procedimentos.

## <a name="to-unregister-an-nps"></a>Para cancelar o registro de um NPS

1. No controlador de domínio, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Active Directory Users and Computers**. O console de Usuários e Computadores do Active Directory é aberto.

2. Clique em **os usuários**e, em seguida, clique duas vezes em **servidores RAS e IAS**.

3. Clique o **membros** guia e, em seguida, selecione o NPS que você deseja cancelar o registro.

4. Clique em **remova**, clique em **Yes**e, em seguida, clique em **Okey**.

