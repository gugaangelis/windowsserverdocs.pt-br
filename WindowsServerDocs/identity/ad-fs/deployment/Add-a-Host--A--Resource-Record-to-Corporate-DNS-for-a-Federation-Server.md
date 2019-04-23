---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Adicionar um registro de recurso de host (A) ao DNS corporativo para um servidor de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856177"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Adicionar um registro de recurso de host (A) ao DNS corporativo para um servidor de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Os clientes na empresa de rede para acesso com êxito de um servidor de Federação usando a autenticação integrada do Windows, um host \(um\) registro de recurso deve ser criado primeiramente no sistema de nomes de domínio corporativo \(DNS\) que resolve o nome do host do servidor de federação de conta \(por exemplo, fs.fabrikam.com\) para o endereço IP do servidor de Federação ou o cluster de servidor de Federação. Você pode usar o procedimento a seguir para adicionar um host \(um\) registro de recurso ao DNS corporativo para um servidor de Federação.  
  
Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para adicionar um host \(um\) registro de recurso ao DNS corporativo para um servidor de Federação  
  
1.  Em um servidor DNS para a rede corporativa, abra o snap DNS\-no.  
  
2.  Na árvore de console, com o botão direito\-clique na zona de pesquisa direta aplicável e, em seguida, clique em **novo Host \(A ou AAAA\)**.  
  
3.  Na **nome**, digite o nome de computador do servidor de Federação ou o cluster de servidor de federação; por exemplo, para o nome de domínio totalmente qualificado \(FQDN\) fs.fabrikam.com, digite **fs**.  
  
4.  Na **endereço IP**, digite o endereço IP para o servidor de Federação ou um cluster de servidor de federação, por exemplo, 192.168.1.4.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de resolução de nome para servidores de Federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

