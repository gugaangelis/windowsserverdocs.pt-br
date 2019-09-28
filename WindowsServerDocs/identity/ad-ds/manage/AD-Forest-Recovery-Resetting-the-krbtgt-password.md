---
title: Recuperação de floresta do AD-redefinindo a senha krbtgt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 14dd09c6177d473547a67e1d79e9714f0a7a29b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390296"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Recuperação de floresta do AD-redefinindo a senha krbtgt

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para redefinir a senha krbtgt para o domínio. O procedimento a seguir aplica os DCs graváveis, mas não os RODCs (controladores de domínio somente leitura).
  
> [!IMPORTANT]
> Se você planeja recuperar os RODCs online durante a recuperação da floresta, não exclua as contas krbtgt para os RODCs. A conta krbtgt para um RODC é listada no formato krbtgt_*número*.
>
> Se você usar um filtro de senha personalizado (como passfilt. dll) em um controlador de domínio, poderá receber um erro quando tentar redefinir a senha krbtgt. Para obter mais informações, incluindo uma solução alternativa, consulte o [artigo 2549833](https://support.microsoft.com/kb/2549833) da base de dados de conhecimento Microsoft (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Para redefinir a senha krbtgt  
  
1. Clique em **Iniciar**, aponte para **painel de controle**, aponte para **ferramentas administrativas**e clique em **Active Directory usuários e computadores**.
2. Clique em **Exibir** e em **Recursos Avançados**.
3. Na árvore de console, clique duas vezes no contêiner de domínio e, em seguida, clique em **usuários**.
4. No painel de detalhes, clique com o botão direito do mouse na conta de usuário **krbtgt** e clique em **Redefinir senha**.
   Senha de @no__t 0Reset @ no__t-1
5. Em **nova senha**, digite uma nova senha, digite a senha novamente em **Confirmar senha**e clique em **OK**. A senha que você especificar não é significativa porque o sistema irá gerar uma senha forte automaticamente independente da senha que você especificar.
  
> [!NOTE]
> Você deve executar essa operação duas vezes. O histórico de senha da conta krbtgt é dois, o que significa que ele inclui as duas senhas mais recentes. Ao redefinir a senha duas vezes, você limpa efetivamente todas as senhas antigas do histórico, portanto, não há como o outro controlador de domínio será replicado com esse controlador de domínio usando uma senha antiga.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md) 
