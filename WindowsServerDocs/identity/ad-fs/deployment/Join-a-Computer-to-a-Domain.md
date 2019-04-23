---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Ingressar um computador em um domínio
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 811f5296143637974cf82e59d57665f8a96f1c8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884107"
---
# <a name="join-a-computer-to-a-domain"></a>Ingressar um computador em um domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para serviços de Federação do Active Directory \(do AD FS\) funcionar, cada computador que funciona como um servidor de federação deve ser associado a um domínio. proxies de servidor de federação podem ser unidos a um domínio, mas isso não é um requisito.  
  
Você não precisa adicionar um servidor Web a um domínio, se o servidor Web estiver hospedando declarações\-somente aplicativos com reconhecimento.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para ingressar um computador a um domínio  
  
1.  Sobre o **inicie** tela, digite **painel de controle**, e, em seguida, pressione ENTER.  
  
2.  Navegue até **sistema e segurança**e, em seguida, clique em **sistema**.  
  
3.  Em **configurações de grupo de trabalho, o domínio e o nome do computador**, clique em **alterar configurações**.  
  
4.  Na aba **Nome do Computador** , clique em **Alterar**.  
  
5.  Sob **membro**, clique em **domínio**, digite o nome do domínio que você deseja que este computador ingressar e, em seguida, clique em **Okey**.  
  
6.  Clique em **OK** e reinicie o computador.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Como configurar um Proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

