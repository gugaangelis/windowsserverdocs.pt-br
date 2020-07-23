---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Adicionar um certificado de descriptografia de tokens
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e703b9966fdd443c2799120464dcb92b2458428b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966778"
---
# <a name="add-a-token-decrypting-certificate"></a>Adicionar um certificado de descriptografia de tokens

Os servidores de Federação usam um \- certificado de descriptografia de token quando um servidor de Federação de terceira parte confiável deve descriptografar tokens emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia primário. Serviços de Federação do Active Directory (AD FS) \( AD FS \) usa o \( certificado SSL protocolo SSL \) para serviços de informações da Internet \( IIS \) como o certificado de descriptografia padrão.  
  
> [!CAUTION]  
> Os certificados usados para a \- descriptografia de token são críticos para a estabilidade do serviço de Federação. Como a perda ou remoção não planejada de quaisquer certificados configurados para essa finalidade pode interromper o serviço, você deve fazer backup de todos os certificados configurados para essa finalidade.  
  
Você pode usar o procedimento a seguir para adicionar o \- certificado de descriptografia de token ao snap-in de gerenciamento de AD FS \- de um arquivo que você exportou.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Para adicionar um \- certificado de descriptografia de token  
  
1.  Na tela **Iniciar** , digite**Gerenciamento de AD FS**e pressione Enter.  
  
2.  Na árvore de console, clique duas vezes \- em **serviço**e, em seguida, clique em **certificados**.  
  
3.  No painel **ações** , clique no link **Adicionar \- certificado de descriptografia de token** .  
  
4.  Na caixa de diálogo **Procurar arquivo de certificado** , navegue até o arquivo de certificado que você deseja adicionar, selecione o arquivo de certificado e clique em **abrir**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de federação](../design/certificate-requirements-for-federation-servers.md)  
  
