---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Criar uma regra para enviar associação a um grupo como uma declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b8217302cc0ec5bc6972004cb2f26ffae1371614
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407585"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Criar uma regra para enviar associação a um grupo como uma declaração

Usando o modelo enviar Associação de grupo como uma regra de Declaração \(no\)serviços de Federação do Active Directory (AD FS) AD FS, você pode criar uma regra que possibilitará selecionar um grupo de segurança de Active Directory para enviar como uma declaração. Apenas uma única declaração será emitida a partir dessa regra, com base no grupo que você selecionar. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará uma declaração de grupo com um valor de administrador se o usuário for membro do grupo de segurança admins. do domínio. Essa regra deve ser usada somente para usuários no domínio Active Directory local.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap\--in de gerenciamento de AD FS.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação de grupo como uma declaração em uma relação de confiança de terceira parte confiável no Windows Server 2016 

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como declaração** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, no **grupo do usuário** , clique em **procurar** e selecione um grupo, em tipo de **declaração de saída** , selecione o tipo de declaração desejado e, em seguida, em  **Tipo de declaração de saída** digite um valor.
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação de grupo como uma declaração em uma confiança de provedor de declarações no Windows Server 2016 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como declaração** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, no **grupo do usuário** , clique em **procurar** e selecione um grupo, em tipo de **declaração de saída** , selecione o tipo de declaração desejado e, em seguida, em  **Tipo de declaração de saída** digite um valor. 
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para criar uma regra para enviar a associação de grupo como uma declaração no Windows Server 2012 R2 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS\\relações de confiança**, clique em **confiança do provedor de declarações** ou em relações de confiança de terceira parte **confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Na caixa de diálogo **Editar regras de declaração** , selecione uma das seguintes guias, dependendo da relação de confiança que você está editando e de qual conjunto de regras você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar o assistente de regra que está associado a esse conjunto de regras :  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como uma declaração** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, no **grupo do usuário** , clique em **procurar** e selecione um grupo, em tipo de **declaração de saída** , selecione o tipo de declaração desejado e, em seguida, em  **Tipo de declaração de saída** digite um valor.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Clique em **Finalizar**.  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  



## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
