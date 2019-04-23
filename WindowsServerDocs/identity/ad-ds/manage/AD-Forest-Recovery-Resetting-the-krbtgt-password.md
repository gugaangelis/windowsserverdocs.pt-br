---
title: Recuperação de floresta do AD - redefinição de senha de krbtgt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 1ac0dcb9da1d10a417c128cb8498a5d8362d9a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883737"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Recuperação de floresta do AD - redefinição de senha de krbtgt

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para redefinir a senha de krbtgt de domínio. O procedimento a seguir se aplica a controladores de domínio graváveis, mas os controladores de domínio não somente leitura (RODCs).
  
> [!IMPORTANT]
> Se você planeja recuperar os RODCs online durante a recuperação de floresta, não exclua as contas de krbtgt para os RODCs. A conta de krbtgt para um RODC é listada no formato krbtgt_*número*.
>
> Se você usar um filtro de senha personalizada (como passfilt) em um controlador de domínio, você poderá receber um erro ao tentar redefinir a senha de krbtgt. Para obter mais informações, incluindo uma solução alternativa, consulte Microsoft Knowledge Base [2549833 do artigo](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Para redefinir a senha de krbtgt  
  
1. Clique em **inicie**, aponte para **painel de controle**, aponte para **ferramentas administrativas**e, em seguida, clique em **Active Directory Users and Computers**.
2. Clique em **Exibir** e em **Recursos Avançados**.
3. Na árvore de console, clique duas vezes no contêiner de domínio e, em seguida, clique em **usuários**.
4. No painel de detalhes, clique com botão direito do **krbtgt** conta de usuário e clique **Redefinir senha**.
   ![Redefinição de senha](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. Na **nova senha**, digite uma nova senha, redigite a senha na **Confirmar senha**e, em seguida, clique em **Okey**. A senha que você especifica não é significativa porque o sistema irá gerar uma senha forte automaticamente independente da senha que você especifica.
  
> [!NOTE]
> Você deve executar essa operação duas vezes. O histórico de senha da conta krbtgt é que dois, que significa que ele inclui as duas senhas mais recentes. Redefinindo a senha duas vezes você efetivamente limpar todas as senhas antigas do histórico, portanto, não há nenhuma maneira de outro controlador de domínio serão replicadas com esse controlador de domínio usando uma senha antiga.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md) 
