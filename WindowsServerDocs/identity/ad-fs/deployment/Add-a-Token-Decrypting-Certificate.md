---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Adicionar um certificado de descriptografia de tokens
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cf89972120f3f0effa3eb1cf0fee6d29dbc8ed4e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192476"
---
# <a name="add-a-token-decrypting-certificate"></a>Adicionar um certificado de descriptografia de tokens

Servidores de federação usam um token\-certificado de descriptografia quando um servidor de federação de terceiros terceira parte confiável deve descriptografar tokens emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia primário. Serviços de Federação do Active Directory \(do AD FS\) usa o Secure Sockets Layer \(SSL\) certificados para serviços de informações da Internet \(IIS\) como a descriptografia padrão certificado.  
  
> [!CAUTION]  
> Certificados usados para o token\-descriptografia são essenciais para a estabilidade do serviço de Federação. Como a perda ou remoção não planejada de todos os certificados configurados para esse fim pode interromper o serviço, você deve fazer backup de todos os certificados configurados para essa finalidade.  
  
Você pode usar o procedimento a seguir para adicionar o token\-descriptografar o certificado para o instantâneo de gerenciamento do AD FS\-em de um arquivo que você exportou.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Para adicionar um token\-descriptografar certificado  
  
1.  Sobre o **inicie** tela, digite**gerenciamento do AD FS**, e pressione ENTER.  
  
2.  Na árvore de console, clique duas vezes\-clique em **Service**e, em seguida, clique em **certificados**.  
  
3.  No **ações** painel, clique no **adicionar Token\-certificado de descriptografia** link.  
  
4.  No **procure o arquivo de certificado** diálogo caixa, navegue até o arquivo de certificado que você deseja adicionar, selecione o arquivo de certificado e, em seguida, clique em **aberto**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
  

