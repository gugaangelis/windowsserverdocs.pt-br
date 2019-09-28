---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Adicionar um registro de recurso de host (A) ao DNS corporativo para um servidor de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 132e71cec134d17dd73be998683c09f752fdc414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360335"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Adicionar um registro de recurso de host (A) ao DNS corporativo para um servidor de federação



Para clientes na rede corporativa para acessar com êxito um servidor de Federação usando a autenticação integrada do Windows, um registro de recurso do host \(A @ no__t-1 deve ser criado primeiro no sistema de nomes de domínio corporativos \(DNS @ no__t-3 que resolve o nome do host do servidor de Federação de conta \(for exemplo, FS. fabrikam. com @ no__t-5 para o endereço IP do servidor de Federação ou cluster de servidor de Federação. Você pode usar o procedimento a seguir para adicionar um registro de recurso do host \(A @ no__t-1 ao DNS corporativo para um servidor de Federação.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para adicionar um registro de recurso do host \(A @ no__t-1 ao DNS corporativo para um servidor de Federação  
  
1.  Em um servidor DNS para a rede corporativa, abra o snap do DNS @ no__t-0in.  
  
2.  Na árvore de console, clique em @ no__t-0click a zona de pesquisa direta aplicável e, em seguida, em **novo Host \(A ou aaaa @ no__t-3**.  
  
3.  Em **nome**, digite somente o nome do computador do servidor de Federação ou do cluster do servidor de Federação; por exemplo, para o nome de domínio totalmente qualificado \(FQDN @ no__t-2 fs.fabrikam.com, digite **FS**.  
  
4.  Em **endereço IP**, digite o endereço IP para o servidor de Federação ou o cluster de servidor de Federação, por exemplo, 192.168.1.4.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de resolução de nomes para servidores de federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

