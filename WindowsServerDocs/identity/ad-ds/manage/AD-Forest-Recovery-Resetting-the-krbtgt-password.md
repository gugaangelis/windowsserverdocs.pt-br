---
title: "Recuperação de floresta do AD - redefinir a senha krbtgt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adfs
ms.openlocfilehash: 445fa18503e26d04e20a61cfe652424f78631abe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Recuperação de floresta do AD - redefinir a senha krbtgt 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para redefinir a senha krbtgt para o domínio. O procedimento a seguir se aplica a controladores de domínio graváveis, mas controladores de domínio não somente leitura (RODCs).  
  
> [!IMPORTANT]
>  Se você pretende recuperar RODCs online durante a recuperação da floresta, não exclua as contas krbtgt para os RODCs. A conta krbtgt para um RODC estiver listada na krbtgt_ formato*número*.  
>   
>  Se você usar um filtro de senha personalizados (como passfilt.dll) em um controlador de domínio, você pode receber um erro ao tentar redefinir a senha krbtgt. Para obter mais informações, incluindo uma solução alternativa, consulte Microsoft Knowledge Base [2549833 do artigo](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).  
  
## <a name="to-reset-the-krbtgt-password"></a>Para redefinir a senha krbtgt  
  
1.  Clique em **iniciar**, aponte para **painel de controle**, aponte para **ferramentas administrativas**e clique em **usuários e Active Directory computadores**.  
2.  2.  Clique em **exibição**e clique em **recursos avançados**.  
3.  Na árvore de console, clique duas vezes o contêiner de domínio e, em seguida, clique em **usuários**.  
4.  No painel de detalhes, clique com botão direito do **krbtgt** conta de usuário e clique em **Redefinir senha**.  
![Redefinir senha](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5.  Em **nova senha**, digite uma nova senha, digite novamente a senha no **Confirmar senha**e clique em **Okey**. A senha que você especificar não é significativa, porque o sistema irá gerar uma senha forte automaticamente independente da senha que você especificar.  
  
    > [!NOTE]
    >  Você deve executar essa operação duas vezes. O histórico de senhas de conta krbtgt é dois, ou seja, que ele inclui as duas senhas mais recentes. Redefinindo a senha duas vezes você limpar qualquer senhas antigas do histórico de efetivamente, portanto, não há nenhuma maneira DC outro replicará com esse controlador de domínio usando uma senha antiga.  
 
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md) 
  
