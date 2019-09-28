---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Adicionar um computador a um domínio
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9f6d657397cb07d081a229135e3e6c97c7191164
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408348"
---
# <a name="join-a-computer-to-a-domain"></a>Adicionar um computador a um domínio

Para Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 para funcionar, cada computador que funciona como um servidor de Federação deve ser Unido a um domínio. os proxies do servidor de Federação podem ser associados a um domínio, mas isso não é um requisito.  
  
Você não precisa ingressar um servidor Web em um domínio se o servidor Web estiver hospedando declarações @ no__t-0aware somente aplicativos.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para ingressar um computador em um domínio  
  
1.  Na tela **Iniciar** , digite **painel de controle**e pressione Enter.  
  
2.  Navegue até **sistema e segurança**e clique em **sistema**.  
  
3.  Em **configurações de grupo de trabalho, o domínio e o nome do computador**, clique em **alterar configurações**.  
  
4.  Na aba **Nome do Computador** , clique em **Alterar**.  
  
5.  Em **membro de**, clique em **domínio**, digite o nome do domínio que você deseja que este computador ingresse e clique em **OK**.  
  
6.  Clique em **OK** e reinicie o computador.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

