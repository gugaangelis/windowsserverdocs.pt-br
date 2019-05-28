---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: Criar uma regra para permitir todos os usuários
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: abb00e14dd0b3ce7b06efba816fbd7452e7bf0f1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189408"
---
# <a name="create-a-rule-to-permit-all-users"></a>Criar uma regra para permitir todos os usuários

No Windows Server 2016, você pode usar um **política de controle de acesso** para criar uma regra que dará todos os usuários acesso a uma terceira parte confiável.  No Windows Server 2012 R2, usando o **permitir todos os usuários** modelo de regra nos serviços de Federação do Active Directory \(AD FS\), você pode criar uma regra de autorização que fornecerá todos os usuários acesso para a terceira parte confiável de terceiros. 

Você pode usar regras de autorização adicionais para restringir o acesso. Usuários com permissão de acesso à terceira parte confiável do Serviço de Federação ainda podem ter o serviço negado pela terceira parte confiável.  
  
Você pode usar os procedimentos a seguir para criar uma regra de declaração com o snap de gerenciamento do AD FS\-no.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Para criar uma regra para permitir que todos os usuários no Windows Server 2016

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Clique com botão direito do **terceira parte confiável** que você deseja permitir o acesso a e selecione **Editar política de controle de acesso**.  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. No acesso select de política de controle **permitir todos** e, em seguida, clique em **Apply** e **Okey**.
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Para criar uma regra para permitir que todos os usuários no Windows Server 2012 R2 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS\\relações de confiança\\terceira**, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  

3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  No **editar regras de declaração** caixa de diálogo, clique o **regras de autorização de emissão** guia ou o **regras de autorização de delegação** guia \(com base no tipo de regra de autorização precisar\)e, em seguida, clique em **Adicionar regra** para iniciar o **Adicionar Assistente de regra de declaração de autorização**.  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **permitir todos os usuários** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  Sobre o **configurar regra** , clique em **concluir**.  
  
7.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
