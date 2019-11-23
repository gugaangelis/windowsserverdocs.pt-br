---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Adicionar um certificado de autenticação de tokens
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8b2246842dd70c06442faed995f6b883dbaf70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360092"
---
# <a name="add-a-token-signing-certificate"></a>Adicionar um certificado de autenticação de tokens


Os servidores de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS\) exigem um token\-certificados de assinatura para impedir que os invasores alterem ou falsificam tokens de segurança em uma tentativa de obter acesso não autorizado a recursos federados. Cada token\-certificado de autenticação contém chaves privadas criptográficas e chaves públicas que são usadas para assinar digitalmente \(por meio da chave privada\) um token de segurança. Posteriormente, depois que essas chaves forem recebidas por um servidor de Federação de parceiro, elas validarão a autenticidade \(por meio da\) de chave pública do token de segurança criptografado.  
  
> [!CAUTION]  
> Os certificados usados para assinatura de\-de token são críticos para a estabilidade do Serviço de Federação. Como a perda ou remoção não planejada de quaisquer certificados configurados para essa finalidade pode interromper o serviço, você deve fazer backup de todos os certificados configurados para essa finalidade.  
  
O token\-certificado de autenticação deve encadear uma raiz confiável no Serviço de Federação. Você pode usar o procedimento a seguir para adicionar o token\-certificado de assinatura ao\-snap de gerenciamento de AD FS no de um arquivo que você exportou.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   
  
### <a name="to-add-a-token-signing-certificate"></a>Para adicionar um token\-certificado de assinatura  
  
1.  Na tela **Iniciar** , digite**Gerenciamento de AD FS**e pressione Enter.  
  
2.  Na árvore de console, clique duas\-vezes em **serviço**e em **certificados**.  
  
3.  No painel **ações** , clique no link **Adicionar certificado\-assinatura de token** .  
  
4.  Na caixa de diálogo **Procurar arquivo de certificado** , navegue até o arquivo de certificado que você deseja adicionar, selecione o arquivo de certificado e clique em **abrir**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
  

