---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Adicionar um computador a um domínio
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 174f585f3e156fc8e068b9300fc90a20a67869cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855349"
---
# <a name="join-a-computer-to-a-domain"></a>Adicionar um computador a um domínio

Para Serviços de Federação do Active Directory (AD FS) \(AD FS\) funcionar, cada computador que funciona como um servidor de Federação deve ser Unido a um domínio. os proxies do servidor de Federação podem ser associados a um domínio, mas isso não é um requisito.  
  
Você não precisa ingressar um servidor Web em um domínio se o servidor Web estiver hospedando declarações\-apenas aplicativos cientes.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para ingressar um computador em um domínio  
  
1.  Na tela **Iniciar** , digite **painel de controle**e pressione Enter.  
  
2.  Navegue até **sistema e segurança**e clique em **sistema**.  
  
3.  Em **Nome do computador, domínio e configurações de grupo de trabalho**, clique em **Alterar configurações**.  
  
4.  Na guia **Nome do Computador**, clique em **Alterar**.  
  
5.  Em **membro de**, clique em **domínio**, digite o nome do domínio que você deseja que este computador ingresse e clique em **OK**.  
  
6.  Clique em **OK** e reinicie o computador.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

