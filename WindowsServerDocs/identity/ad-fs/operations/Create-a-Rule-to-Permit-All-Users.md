---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: "Criar uma regra para permitir que todos os usuários"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-all-users"></a>Criar uma regra para permitir que todos os usuários

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

No Windows Server 2016, você pode usar um **política de controle de acesso** para criar uma regra que vai dar todos os usuários acesso a um terceiro.  No Windows Server 2012 R2, usando o **permitir que todos os usuários** modelo de regra em \(AD FS\) serviços de Federação do Active Directory, você pode criar uma regra de autorização que vai dar todos os usuários acesso à parte confiável. 

Você pode usar regras de autorização adicional para restringir o acesso. Os usuários que têm permissão para acessar a terceira parte do serviço de federação podem ainda ser negados serviço pela terceira parte.  
  
Você pode usar os procedimentos a seguir para criar uma regra de declaração com o gerenciamento do AD FS snap\-in.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>Para criar uma regra para permitir que todos os usuários no Windows Server 2016

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  Clique com botão direito do **confiar dependência de terceiros** que você deseja permitir o acesso a e selecione **Editar política de controle de acesso **.  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. Selecione política de controle no acesso **permitir todos** e, em seguida, clique em **aplicar** e **Okey **.
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>Para criar uma regra para permitir que todos os usuários no Windows Server 2012 R2 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS\\Trust Relationships\\Relying terceiros confie**, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  

3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  No **editar regras de declaração** caixa de diálogo, clique no **regras de autorização de emissão** guia ou o **regras de autorização de delegação** guia \ (com base no tipo de regra de autorização você require\) e, em seguida, clique em **Adicionar regra** para iniciar o **autorização reivindicação regra Assistente para adicionar **.  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **permitir que todos os usuários** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  No **configurar regra** página, clique em **concluir **.  
  
7.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
