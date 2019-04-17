---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: "Criar uma regra para permitir ou negar a usuários com base em uma declaração de entrada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: afcbb8c7a08a84eda2a794c9565061d7d61d470b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Criar uma regra para permitir ou negar a usuários com base em uma declaração de entrada 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

No Windows Server 2016, você pode usar um **política de controle de acesso** para criar uma regra que permitirá de negar aos usuários com base em uma declaração de entrada.  No Windows Server 2012 R2, usando o **permitir ou negar os usuários com base em uma entrada reivindicar** modelo de regra em \(AD FS\) serviços de Federação do Active Directory, você pode criar uma regra de autorização que será conceder ou negar o acesso do usuário para a parte confiante com base no tipo de valor de uma declaração de entrada. 

Por exemplo, você pode usar isso para criar uma regra que permitirá que apenas usuários que têm um grupo reivindicar com um valor de administradores de domínio para acessar o terceiro. Se você quiser permitir que todos os usuários para acessar o terceiro, use o **permitir todos** política de controle de acesso ou o **permitir que todos os usuários** modelo de regra dependendo da versão do Windows Server. Os usuários que têm permissão para acessar a terceira parte do serviço de federação podem ainda ser negados serviço pela terceira parte.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o gerenciamento do AD FS snap\-in.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para criar uma regra para permitir que os usuários com base em uma declaração de entrada no Windows Server 2016
 
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **políticas de controle de acesso **. 
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Clique com botão direito e selecione **adicionar política de controle de acesso **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Na caixa Nome, insira um nome para sua política, uma descrição e clique em **adicionar **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. No **regra Editor**, em usuários, coloque um check-in **com declarações específicas na solicitação** e clique no sublinhado **específico** na parte inferior.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. No **selecione requerimentos judiciais ou Extrajudiciais** de tela, clique no **requerimentos judiciais ou Extrajudiciais** botão de opção, selecione o **tipo de declaração**, o **operador**e o **reivindicação valor**, em seguida, clique em **Okey **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  Sobre o **regra Editor** clique **Okey **.  Sobre o **adicionar política de controle de acesso** de tela, clique em **Okey **.

8. No **AD FS gerenciamento** do console em árvore, **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Clique com botão direito do **confiar dependência de terceiros** que você deseja permitir o acesso a e selecione **Editar política de controle de acesso **.  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. A política de controle de acesso selecione sua política e, em seguida, clique em **aplicar** e **Okey **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para criar uma regra para negar aos usuários com base em uma declaração de entrada no Windows Server 2016
 
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **políticas de controle de acesso **. 
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Clique com botão direito e selecione **adicionar política de controle de acesso **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Na caixa Nome, insira um nome para sua política, uma descrição e clique em **adicionar **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. No **regra Editor**, verifique se todos está selecionado e, em **exceto** colocar um check-in **com declarações específicas na solicitação** e clique no sublinhado **específico** na parte inferior.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. No **selecione requerimentos judiciais ou Extrajudiciais** de tela, clique no **requerimentos judiciais ou Extrajudiciais** botão de opção, selecione o **tipo de declaração**, o **operador**e o **reivindicação valor**, em seguida, clique em **Okey **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  Sobre o **regra Editor** clique **Okey **.  Sobre o **adicionar política de controle de acesso** de tela, clique em **Okey **.

8. No **AD FS gerenciamento** do console em árvore, **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Clique com botão direito do **confiar dependência de terceiros** que você deseja permitir o acesso a e selecione **Editar política de controle de acesso **.  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. A política de controle de acesso selecione sua política e, em seguida, clique em **aplicar** e **Okey **.
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Para criar uma regra para permitir ou negar aos usuários com base em uma declaração de entrada no Windows Server 2012 R2
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.    
  
2.  Na árvore de console, em **AD FS\\Trust Relationships\\Relying terceiros confie**, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  No **editar regras de declaração** caixa de diálogo, clique no **regras de autorização de emissão** guia ou o **regras de autorização de delegação** guia \ (com base no tipo de regra de autorização você require\) e, em seguida, clique em **Adicionar regra** para iniciar o **autorização reivindicação regra Assistente para adicionar **.  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **permitir ou negar os usuários com base em uma declaração de entrada** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  No **configurar regra** página em **nome da regra reivindicação** digitar o nome de exibição para essa regra, **tipo de declaração de entrada** selecionar um tipo de declaração a lista, em **valor de solicitação de entrada** digitar um valor ou clique em Procurar \ (se for available\) e selecione um valor e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Permitir o acesso aos usuários com essa declaração de entrada**  
  
    -   **Negar acesso aos usuários com essa declaração de entrada**  
![Criar a regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Clique em **concluir **.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
