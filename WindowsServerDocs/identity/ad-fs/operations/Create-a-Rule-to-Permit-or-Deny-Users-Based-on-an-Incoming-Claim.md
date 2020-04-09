---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7d6be5b9194060bb16673e01e0fee8b36b081769
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816809"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada 


No Windows Server 2016, você pode usar uma **política de controle de acesso** para criar uma regra que permitirá ou negará usuários com base em uma declaração de entrada.  No Windows Server 2012 R2, usando os **usuários permitir ou negar com base em um** modelo de regra de declaração de entrada no Serviços de Federação do Active Directory (AD FS) \(AD FS\), você pode criar uma regra de autorização que concederá ou negará o acesso do usuário à terceira parte confiável com base no tipo e valor de uma declaração de entrada. 

Por exemplo, você pode usar isso para criar uma regra que permitirá que somente os usuários que têm uma declaração de grupo com um valor de administradores de domínio acessem a terceira parte confiável. Se você quiser permitir que todos os usuários acessem a terceira parte confiável, use a política de controle de acesso **permitir todos** ou o modelo de regra **permitir todos os usuários** , dependendo da sua versão do Windows Server. Usuários com permissão de acesso à terceira parte confiável do Serviço de Federação ainda podem ter o serviço negado pela terceira parte confiável.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap\-de gerenciamento de AD FS no.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para criar uma regra para permitir que os usuários com base em uma declaração de entrada no Windows Server 2016
 
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **políticas de controle de acesso**. 
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG) de regra

3. Clique com o botão direito do mouse e selecione **Adicionar política de controle de acesso**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG) de regra

4. Na caixa nome, insira um nome para a política, uma descrição e clique em **Adicionar**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG) de regra

5. No **Editor de regras**, em usuários, coloque um check-in **com declarações específicas na solicitação** e clique no sublinhado **específico** na parte inferior.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG) de regra

6. Na tela **selecionar declarações** , clique no botão de opção **declarações** , selecione o **tipo de declaração**, o **operador**e o **valor da declaração** e clique em **OK**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG) de regra

7.  No **Editor de regras** , clique em **OK**.  Na tela **Adicionar política de controle de acesso** , clique em **OK**.

8. Na árvore de console de **Gerenciamento do AD FS** , em **AD FS**, clique em relações de confiança de terceira parte **confiável**. 
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG) de regra

9.  Clique com o botão direito do mouse na **relação de confiança** de terceira parte confiável à qual você deseja permitir acesso e selecione **Editar política de controle de acesso**.  
![criar](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG) de regra

10. Na política de controle de acesso, selecione sua política e clique em **aplicar** e em **OK**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG) de regra

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>Para criar uma regra para negar usuários com base em uma declaração de entrada no Windows Server 2016
 
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **políticas de controle de acesso**. 
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG) de regra

3. Clique com o botão direito do mouse e selecione **Adicionar política de controle de acesso**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG) de regra

4. Na caixa nome, insira um nome para a política, uma descrição e clique em **Adicionar**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG) de regra

5. No **Editor de regras**, verifique se todos estão selecionados e **, em exceção** , coloque um check-in **com declarações específicas na solicitação** e clique no sublinhado **específico** na parte inferior.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG) de regra

6. Na tela **selecionar declarações** , clique no botão de opção **declarações** , selecione o **tipo de declaração**, o **operador**e o **valor da declaração** e clique em **OK**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG) de regra

7.  No **Editor de regras** , clique em **OK**.  Na tela **Adicionar política de controle de acesso** , clique em **OK**.

8. Na árvore de console de **Gerenciamento do AD FS** , em **AD FS**, clique em relações de confiança de terceira parte **confiável**. 
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG) de regra

9.  Clique com o botão direito do mouse na **relação de confiança** de terceira parte confiável à qual você deseja permitir acesso e selecione **Editar política de controle de acesso**.  
![criar](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG) de regra

10. Na política de controle de acesso, selecione sua política e clique em **aplicar** e em **OK**.
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG) de regra

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>Para criar uma regra para permitir ou negar usuários com base em uma declaração de entrada no Windows Server 2012 R2
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.    
  
2.  Na árvore de console, em **AD FS\\relações de confiança\\confianças**de terceira parte confiável, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.  
  
3.  À direita\-clique na relação de confiança selecionada e, em seguida, clique em **Editar regras de declaração**.  
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) de regra   

4.  Na caixa de diálogo **Editar regras de declaração** , clique na guia **regras de autorização de emissão** ou na guia regras de autorização de **delegação** \(com base no tipo de regra de autorização que você precisa\)e, em seguida, clique em **Adicionar regra** para iniciar o **Assistente para Adicionar regra de declaração de autorização**.  
![criar](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) de regra

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **permitir ou negar usuários com base em uma declaração de entrada** na lista e clique em **Avançar**.  
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG) de regra

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, em **tipo de declaração de entrada** , selecione um tipo de declaração na lista, em valor de declaração de **entrada** , digite um valor ou clique em procurar \(se ele estiver disponível\) e selecione um valor e, em seguida, selecione uma das opções a seguir, dependendo das necessidades da sua organização:  
  
    -   **Permitir acesso a usuários com esta declaração de entrada**  
  
    -   **Negar acesso a usuários com esta declaração de entrada**  
![criar](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG) de regra  
7.  Clique em **Concluir**.  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criando regras de declaração para uma terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
