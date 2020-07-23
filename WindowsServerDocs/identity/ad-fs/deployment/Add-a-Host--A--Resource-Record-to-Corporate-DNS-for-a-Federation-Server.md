---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Adicionar um registro de recurso de host (A) ao DNS corporativo para um servidor de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fdf75408e4627fb587f95e9b1c8d2fb93c8e9410
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960038"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Adicionar um registro de recurso de host (A) ao DNS corporativo para um servidor de federação



Para clientes na rede corporativa para acessar com êxito um servidor de Federação usando a autenticação integrada do Windows, um host \( um \) registro de recurso deve ser criado primeiro no DNS do sistema de nome de domínio corporativo \( \) que resolve o nome do host do servidor de Federação de conta \( , por exemplo, FS.fabrikam.com \) para o endereço IP do servidor de Federação ou cluster de servidor de Federação. Você pode usar o procedimento a seguir para adicionar um host \( um \) registro de recurso ao DNS corporativo para um servidor de Federação.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para adicionar um host \( um \) registro de recurso ao DNS corporativo para um servidor de Federação  
  
1.  Em um servidor DNS para a rede corporativa, abra o snap- \- in DNS.  
  
2.  Na árvore de console, clique com o botão direito \- do mouse na zona de pesquisa direta aplicável e clique em **novo host \( A ou aaaa \) **.  
  
3.  Em **nome**, digite somente o nome do computador do servidor de Federação ou do cluster do servidor de Federação; por exemplo, para o nome de domínio totalmente qualificado \( FQDN \) FS.fabrikam.com, digite **FS**.  
  
4.  Em **endereço IP**, digite o endereço IP para o servidor de Federação ou o cluster de servidor de Federação, por exemplo, 192.168.1.4.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de resolução de nomes para servidores de federação](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))  
  
