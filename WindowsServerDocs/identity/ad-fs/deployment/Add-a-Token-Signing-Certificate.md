---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Adicionar um certificado de autenticação de tokens
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826147"
---
# <a name="add-a-token-signing-certificate"></a>Adicionar um certificado de autenticação de tokens

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de federação nos serviços de Federação do Active Directory \(do AD FS\) exigem token\-certificados de assinatura para impedir que invasores alterem ou falsifiquem tokens de segurança em uma tentativa de obter acesso não autorizado recursos como federados. Todos os tokens\-certificado de assinatura contém chaves privadas de criptografia e chaves públicas que são usadas para assinar digitalmente \(por meio da chave privada\) um token de segurança. Posteriormente, depois que essas chaves são recebidas por um servidor de Federação do parceiro, elas validam a autenticidade \(por meio da chave pública\) do token de segurança criptografado.  
  
> [!CAUTION]  
> Certificados usados para o token\-assinatura são essenciais para a estabilidade do serviço de Federação. Como a perda ou remoção não planejada de todos os certificados configurados para esse fim pode interromper o serviço, você deve fazer backup de todos os certificados configurados para essa finalidade.  
  
O token\-certificado de assinatura deve estar encadeado a uma raiz confiável no serviço de Federação. Você pode usar o procedimento a seguir para adicionar o token\-certificado de autenticação para o instantâneo de gerenciamento do AD FS\-em de um arquivo que você exportou.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Para adicionar um token\-certificado de assinatura  
  
1.  Sobre o **inicie** tela, digite**gerenciamento do AD FS**, e pressione ENTER.  
  
2.  Na árvore de console, clique duas vezes\-clique em **Service**e, em seguida, clique em **certificados**.  
  
3.  No **ações** painel, clique no **adicionar Token\-certificado de assinatura** link.  
  
4.  No **procure o arquivo de certificado** diálogo caixa, navegue até o arquivo de certificado que você deseja adicionar, selecione o arquivo de certificado e, em seguida, clique em **aberto**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx)  
  

