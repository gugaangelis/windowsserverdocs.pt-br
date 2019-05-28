---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 72fe425b040f83a217a144976265c7754830c91b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189496"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada 


No Windows Server 2016, você pode usar um **política de controle de acesso** para criar uma regra que vai permitir ou negar usuários com base em uma declaração de entrada.  No Windows Server 2012 R2, usando o **permitir ou negar usuários com base em uma declaração de entrada** modelo de regra nos serviços de Federação do Active Directory \(AD FS\), você pode criar uma regra de autorização que concederão ou nega o acesso do usuário para a terceira, com base no tipo e valor de uma declaração de entrada. 

Por exemplo, você pode usar isso para criar uma regra que permitirá que apenas usuários que têm um grupo de declaração com um valor de Admins. do domínio para acessar a terceira parte confiável. Se você quiser que todos os usuários acessem a terceira, use o **permitir todos** política de controle de acesso ou o **permitir todos os usuários** modelo de regra dependendo de sua versão do Windows Server. Usuários com permissão de acesso à terceira parte confiável do Serviço de Federação ainda podem ter o serviço negado pela terceira parte confiável.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap de gerenciamento do AD FS\-no.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para criar uma regra para permitir que os usuários com base em uma declaração de entrada no Windows Server 2016
 
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **políticas de controle de acesso**. 
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Clique com botão direito e selecione **adicionar política de controle de acesso**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Na caixa Nome, insira um nome para sua política, uma descrição e clique em **adicionar**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. No **Editor de regras**, em usuários, coloque uma marca de seleção **com declarações específicas na solicitação** e clique no sublinhado **específico** na parte inferior.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. No **selecionar declarações** tela, clique no **declarações** botão de opção, selecione o **tipo de declaração**, o **operador**e o  **Valor de declaração** , em seguida, clique em **Okey**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  Sobre o **Editor de regras** clique em **Okey**.  Sobre o **adicionar política de controle de acesso** tela, clique em **Okey**.

8. No **gerenciamento do AD FS** árvore de console, em **AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Clique com botão direito do **terceira parte confiável** que você deseja permitir o acesso a e selecione **Editar política de controle de acesso**.  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Na política de controle de acesso, selecione sua política e, em seguida, clique em **Apply** e **Okey**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para criar uma regra para negar usuários com base em uma declaração de entrada no Windows Server 2016
 
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **políticas de controle de acesso**. 
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. Clique com botão direito e selecione **adicionar política de controle de acesso**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. Na caixa Nome, insira um nome para sua política, uma descrição e clique em **adicionar**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. No **Editor de regras**, verifique se todos está selecionado e, em **exceto** colocar uma marca de seleção **com declarações específicas na solicitação** e clique no sublinhado  **específico** na parte inferior.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. No **selecionar declarações** tela, clique no **declarações** botão de opção, selecione o **tipo de declaração**, o **operador**e o  **Valor de declaração** , em seguida, clique em **Okey**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  Sobre o **Editor de regras** clique em **Okey**.  Sobre o **adicionar política de controle de acesso** tela, clique em **Okey**.

8. No **gerenciamento do AD FS** árvore de console, em **AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  Clique com botão direito do **terceira parte confiável** que você deseja permitir o acesso a e selecione **Editar política de controle de acesso**.  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. Na política de controle de acesso, selecione sua política e, em seguida, clique em **Apply** e **Okey**.
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Para criar uma regra para permitir ou negar usuários com base em uma declaração de entrada no Windows Server 2012 R2
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.    
  
2.  Na árvore de console, sob **do AD FS\\relações de confiança\\terceira**, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  No **editar regras de declaração** caixa de diálogo, clique o **regras de autorização de emissão** guia ou o **regras de autorização de delegação** guia \(com base no tipo de regra de autorização precisar\)e, em seguida, clique em **Adicionar regra** para iniciar o **Adicionar Assistente de regra de declaração de autorização**.  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **permitir ou negar usuários com base em uma declaração de entrada** na lista e, em seguida, clique  **Próxima**.  
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  Sobre o **configurar regra de** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, na **tipo de declaração de entrada** selecionar um tipo de declaração na lista, em  **Valor de declaração de entrada** digite um valor ou clique em Browse \(se ele estiver disponível\) e selecione um valor e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Permitir o acesso a usuários com esta declaração de entrada**  
  
    -   **Negar acesso a usuários com esta declaração de entrada**  
![Criar regra](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  Clique em **concluir**.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
