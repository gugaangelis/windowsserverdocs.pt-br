---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: "Adicionar um registro de recurso de Host (A) ao DNS corporativa para um servidor de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Adicionar um registro de recurso de Host (A) ao DNS corporativa para um servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Para os clientes sobre o corporativa de rede para acesso com êxito um servidor de Federação usando autenticação integrada do Windows, um registro de recurso do host \(A\) deve ser primeiramente criado no \(DNS\) Domain Name System corporativos que resolve o nome do host do servidor de federação de conta \ (por exemplo, fs.fabrikam.com\) para o endereço IP do servidor de Federação ou cluster de servidor de Federação. Você pode usar o procedimento a seguir para adicionar um registro de recurso do host \(A\) a corporativo DNS para um servidor de Federação.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para adicionar um registro de recurso do host \(A\) a corporativo DNS para um servidor de Federação  
  
1.  Em um servidor DNS para a rede corporativa, abra o DNS snap\-in.  
  
2.  No console de árvore, clique right\ a zona de pesquisa direta aplicável e clique em **novo Host \(A or AAAA\)**.  
  
3.  Em **nome**, digite apenas o nome do computador do servidor de Federação ou cluster de servidor de Federação; Por exemplo, para o domínio totalmente qualificado, nome \(FQDN\) fs.fabrikam.com, tipo **fs**.  
  
4.  Em **endereço IP**, digite o endereço IP do servidor de Federação ou cluster de servidor de federação, por exemplo, 192.168.1.4.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de resolução de nome para servidores de Federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

