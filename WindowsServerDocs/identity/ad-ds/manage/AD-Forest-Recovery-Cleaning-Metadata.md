---
title: Recuperação de floresta do AD-limpando metadados de DCS removidos
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.openlocfilehash: ae95364ffa09a385e2fa03d630536165f50697b5
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938956"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Recuperação de floresta do AD-limpeza de metadados de controladores de domínio graváveis removidos

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

A limpeza de metadados remove Active Directory dados que identificam um controlador de domínio para o sistema de replicação.

Use o procedimento a seguir para excluir os objetos de DC para controladores de domínio que você planeja adicionar de volta à rede reinstalando AD DS.

Se você estiver usando a versão do Active Directory usuários e computadores ou Active Directory sites e serviços incluídos no Ferramentas de Administração de Servidor Remoto (RSAT), a limpeza de metadados será executada automaticamente quando você excluir um objeto DC.

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Excluindo um controlador de domínio usando Active Directory usuários e computadores

Quando você usa a versão do Active Directory usuários e computadores ou Centro Administrativo do Active Directory no Ferramentas de Administração de Servidor Remoto (RSAT), a limpeza de metadados é executada automaticamente quando você exclui o objeto do controlador de domínio. O objeto de servidor e o objeto de computador também são excluídos automaticamente.

Como alternativa, você também pode usar Active Directory sites e serviços no RSAT para excluir um objeto DC. Se você usar Active Directory sites e serviços, deverá excluir o objeto de servidor associado e o objeto de configurações NTDS antes de excluir o objeto DC.

Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](../../../remote/remote-server-administration-tools.md).

O procedimento a seguir é o mesmo para DCs que executam o Windows Server 2016, 2012, 2008 R2 ou 2008. O DC de destino da operação de limpeza de metadados pode executar qualquer versão do Windows Server.

### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Para excluir um objeto de controlador de domínio usando Active Directory usuários e computadores no RSAT

1. Clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**.
2. Na árvore de console, clique duas vezes no contêiner de domínio e clique duas vezes na UO (unidade organizacional) dos **controladores de domínio** .
3. No painel de detalhes, clique com o botão direito do mouse no controlador de domínio que você deseja excluir e clique em **excluir**.
   ![Delete (excluir)](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png)
4. Clique em **Sim** para confirmar a exclusão. Selecione o **controlador de domínio está permanentemente offline e não pode mais ser rebaixado usando a caixa de seleção assistente para instalação do Active Directory Domain Services (Dcpromo)** e clique em **excluir**.
5. Se o controlador de domínio for um servidor de catálogo global, clique em **Sim** confirmar que a exclusão.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
