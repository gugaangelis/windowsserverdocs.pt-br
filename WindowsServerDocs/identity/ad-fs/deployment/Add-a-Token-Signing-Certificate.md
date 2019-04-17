---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Adicionar um certificado de assinatura de Token
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-signing-certificate"></a>Adicionar um certificado de assinatura de Token

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de Federação em serviços de Federação do Active Directory \(AD FS\) exigem certificados de assinatura de token\ para impedir que invasores alterem ou falsifiquem tokens de segurança em uma tentativa de obter acesso não autorizado a recursos federados. Cada certificado de assinatura de token\ contém chaves privadas de criptografia e chaves públicas são usadas para assinar digitalmente \ (por meio do key\ privada) um token de segurança. Posteriormente, depois que essas chaves são recebidos por um servidor de federação de parceiro, eles validam a autenticidade \ (por meio do key\ pública) do token de segurança criptografado.  
  
> [!CAUTION]  
> Certificados usados para autenticação de token\ são fundamentais para a estabilidade do serviço de Federação. Porque perda ou não planejada remoção de todos os seus certificados configuradas para essa finalidade pode interromper o serviço, você deve fazer backup de todos os seus certificados configurados para essa finalidade.  
  
O certificado de assinatura de token\ deve encadear em uma raiz confiável no serviço de Federação. Você pode usar o procedimento a seguir para adicionar o certificado de assinatura de token\ para o AD FS snap\-in Gerenciamento de um arquivo que você tiver exportado.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Para adicionar um certificado de assinatura de token\  
  
1.  Sobre o **iniciar** de tela, digite**AD FS gerenciamento**, e pressione ENTER.  
  
2.  Na árvore de console, clique em double\ **Service**e clique em **certificados**.  
  
3.  No **ações** painel, clique no **certificado de assinatura de adicionar Token\** link.  
  
4.  No **navegar para o arquivo de certificado** caixa de diálogo caixa, navegue até o arquivo do certificado que você deseja adicionar, selecione o arquivo de certificado e clique em **abrir**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx)  
  

