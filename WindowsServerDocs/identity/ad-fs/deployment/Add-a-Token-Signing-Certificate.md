---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Adicionar um certificado de autenticação de tokens
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 31a624b85d68611be0661d6efcc8f33d24efdc54
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947617"
---
# <a name="add-a-token-signing-certificate"></a>Adicionar um certificado de autenticação de tokens


Os servidores de Federação no Serviços de Federação do Active Directory (AD FS) \( AD FS \) exigem certificados de autenticação de token \- para impedir que invasores alterem ou falsificam tokens de segurança em uma tentativa de obter acesso não autorizado a recursos federados. Cada \- certificado de autenticação de token contém chaves privadas criptográficas e chaves públicas que são usadas para assinar digitalmente \( por meio da chave privada \) um token de segurança. Posteriormente, depois que essas chaves forem recebidas por um servidor de Federação de parceiro, elas validarão a autenticidade \( por meio da chave pública \) do token de segurança criptografado.

> [!CAUTION]
> Os certificados usados para \- autenticação de token são críticos para a estabilidade do serviço de Federação. Como a perda ou remoção não planejada de quaisquer certificados configurados para essa finalidade pode interromper o serviço, você deve fazer backup de todos os certificados configurados para essa finalidade.

O certificado de autenticação de token \- deve encadear uma raiz confiável no serviço de Federação. Você pode usar o procedimento a seguir para adicionar o \- certificado de autenticação de token ao snap-in de gerenciamento de AD FS \- de um arquivo que você exportou.

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .

### <a name="to-add-a-token-signing-certificate"></a>Para adicionar um certificado de autenticação de token \-

1.  Na tela **Iniciar** , digite**Gerenciamento de AD FS**e pressione Enter.

2.  Na árvore de console, clique duas vezes \- em **serviço**e, em seguida, clique em **certificados**.

3.  No painel **ações** , clique no link **Adicionar \- certificado de autenticação de token** .

4.  Na caixa de diálogo **Procurar arquivo de certificado** , navegue até o arquivo de certificado que você deseja adicionar, selecione o arquivo de certificado e clique em **abrir**.

## <a name="additional-references"></a>Referências adicionais
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)

[Requisitos de certificado para servidores de federação](../design/certificate-requirements-for-federation-servers.md)

