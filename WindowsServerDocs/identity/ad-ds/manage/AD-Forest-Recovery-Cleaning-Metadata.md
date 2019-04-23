---
title: Recuperação de floresta do AD - limpeza de metadados de controladores de domínio removidos
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: b71cab51a362a96ab6071e5eed3cf31c4421041c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843037"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Recuperação de floresta do AD - limpeza de metadados de controladores de domínio graváveis removidos

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Limpeza de metadados remove dados do Active Directory que identifica um sistema de replicação do controlador de domínio.  

Use o procedimento a seguir para excluir os objetos do controlador de domínio para controladores de domínio que você planeja adicionar novamente à rede, reinstalando o AD DS.  
  
Se você estiver usando a versão do Active Directory Users and Computers ou Sites do Active Directory e serviços que está incluíam ferramentas de administração de servidor remoto (RSAT), a limpeza de metadados é executada automaticamente quando você exclui um objeto de controlador de domínio.  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Excluindo um controlador de domínio usando computadores e usuários do Active Directory

Quando você usa a versão de usuários do Active Directory e computadores ou o Centro Administrativo do Active Directory em ferramentas de administração de servidor remoto (RSAT), a limpeza de metadados é executada automaticamente quando você exclui o objeto de controlador de domínio. O objeto de servidor e o objeto de computador também são excluídos automaticamente.  

Como alternativa, você também pode usar serviços e Sites do Active Directory no RSAT para excluir um objeto de controlador de domínio. Se você usar os serviços e Sites do Active Directory, você deve excluir o objeto de servidor associado e o objeto de configurações NTDS antes de excluir o objeto de controlador de domínio.  

Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
  
O procedimento a seguir é o mesmo para os controladores de domínio que executam o Windows Server 2016, 2012, 2008 R2 ou 2008. O controlador de domínio de destino da operação de limpeza de metadados pode executar qualquer versão do Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Para excluir um objeto de controlador de domínio usando computadores e usuários do Active Directory em RSAT  
  
1. Clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**.  
2. Na árvore de console, clique duas vezes no contêiner de domínio e, em seguida, clique duas vezes o **controladores de domínio** unidade organizacional (UO).  
3. No painel de detalhes, clique com botão direito o controlador de domínio que você deseja excluir e, em seguida, clique em **excluir**.
   ![Excluir](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. Clique em **Sim** para confirmar a exclusão. Selecione o **este controlador de domínio está permanentemente offline e não pode ser rebaixado usando o Active Directory domínio serviços de Assistente para instalação (DCPROMO)** caixa de seleção e clique em **excluir**.  
5. Se o controlador de domínio era um servidor de catálogo global, clique em **Sim** confirme a exclusão.  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
