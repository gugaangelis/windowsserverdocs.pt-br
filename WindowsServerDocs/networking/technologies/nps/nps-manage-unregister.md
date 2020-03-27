---
title: Cancelar o registro de um NPS de um domínio do Active Directory
description: Você pode usar este tópico para registrar um servidor que executa o servidor de diretivas de rede no Windows Server 2016 no domínio padrão do NPS ou em outro domínio.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 366e3e7eef6ac1e8682dd3064e0d133f21d1a8da
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315892"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Cancelar o registro de um NPS de um domínio do Active Directory

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

No processo de gerenciamento da implantação do NPS, talvez seja útil mover um NPS para outro domínio, substituir um NPS ou desativar um NPS. 

Ao mover ou desativar um NPS, você pode cancelar o registro do NPS nos domínios de Active Directory em que o NPS tem permissão para ler as propriedades de contas de usuário em Active Directory.

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução destes procedimentos.

## <a name="to-unregister-an-nps"></a>Para cancelar o registro de um NPS

1. No controlador de domínio, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **Active Directory usuários e computadores**. O console de Usuários e Computadores do Active Directory é aberto.

2. Clique em **usuários**e, em seguida, clique duas vezes em **Servidores RAS e ias**.

3. Clique na guia **Membros** e selecione o NPS que você deseja cancelar o registro.

4. Clique em **remover**, em **Sim**e em **OK**.

