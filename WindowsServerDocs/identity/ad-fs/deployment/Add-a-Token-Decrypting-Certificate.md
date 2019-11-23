---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Adicionar um certificado de descriptografia de tokens
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 388414fff97705901bf52ee844b90508d62f8c83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408460"
---
# <a name="add-a-token-decrypting-certificate"></a>Adicionar um certificado de descriptografia de tokens

Os servidores de Federação usam um token\-certificado de descriptografia quando um servidor de Federação de terceira parte confiável deve descriptografar tokens emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia primário. Serviços de Federação do Active Directory (AD FS) \(AD FS\) usa o certificado protocolo SSL \(SSL\) para Serviços de Informações da Internet \(IIS\) como o certificado de descriptografia padrão.  
  
> [!CAUTION]  
> Os certificados usados para a descriptografia de\-de token são críticos para a estabilidade do Serviço de Federação. Como a perda ou remoção não planejada de quaisquer certificados configurados para essa finalidade pode interromper o serviço, você deve fazer backup de todos os certificados configurados para essa finalidade.  
  
Você pode usar o procedimento a seguir para adicionar o token\-descriptografar o certificado para o\-snap de gerenciamento de AD FS no de um arquivo que você exportou.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Para adicionar um token\-descriptografar o certificado  
  
1.  Na tela **Iniciar** , digite**Gerenciamento de AD FS**e pressione Enter.  
  
2.  Na árvore de console, clique duas\-vezes em **serviço**e em **certificados**.  
  
3.  No painel **ações** , clique no link **Adicionar token\-descriptografar certificado** .  
  
4.  Na caixa de diálogo **Procurar arquivo de certificado** , navegue até o arquivo de certificado que você deseja adicionar, selecione o arquivo de certificado e clique em **abrir**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
  

