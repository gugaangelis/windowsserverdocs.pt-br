---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Criar uma regra para enviar associação a um grupo como uma declaração
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5777a3310776115f02395df365352be94a89928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816659"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Criar uma regra para enviar associação a um grupo como uma declaração

Usando o modelo enviar Associação de grupo como uma regra de declaração no Serviços de Federação do Active Directory (AD FS) \(AD FS\), você pode criar uma regra que possibilitará que você selecione um grupo de segurança Active Directory para enviar como uma declaração. Apenas uma única declaração será emitida a partir dessa regra, com base no grupo que você selecionar. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará uma declaração de grupo com um valor de administrador se o usuário for membro do grupo de segurança admins. do domínio. Essa regra deve ser usada somente para usuários no domínio Active Directory local.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap\-de gerenciamento de AD FS no.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação de grupo como uma declaração em uma relação de confiança de terceira parte confiável no Windows Server 2016 

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável. 
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG) de regra  
  
3.  À direita\-clique na relação de confiança selecionada e, em seguida, clique em **Editar política de emissão de declaração**.
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG) de regra   
  
4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra. 
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG) de regra    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como declaração** na lista e clique em **Avançar**.  
![criar](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG) de regra      

6.   Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, no **grupo do usuário** , clique em **procurar** e selecione um grupo, em tipo de **declaração de saída** , selecione o tipo de declaração desejado e, em seguida, em tipo de **declaração de saída** digite um valor.
![criar](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG) de regra   

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação de grupo como uma declaração em uma confiança de provedor de declarações no Windows Server 2016 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**. 
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG) de regra  
  
3.  À direita\-clique na relação de confiança selecionada e, em seguida, clique em **Editar regras de declaração**.
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG) de regra   
  
4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG) de regra    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como declaração** na lista e clique em **Avançar**.  
![criar](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG) de regra     

6.   Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, no **grupo do usuário** , clique em **procurar** e selecione um grupo, em tipo de **declaração de saída** , selecione o tipo de declaração desejado e, em seguida, em tipo de **declaração de saída** digite um valor. 
![criar](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG) de regra      

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para criar uma regra para enviar a associação de grupo como uma declaração no Windows Server 2012 R2 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS\\relações de confiança**, clique em relações de confiança do **provedor de declarações** ou em relações de confiança de terceira parte **confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.  
  
3.  À direita\-clique na relação de confiança selecionada e, em seguida, clique em **Editar regras de declaração**.
![criar](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) de regra  
  
4.  Na caixa de diálogo **Editar regras de declaração** , selecione uma das seguintes guias, dependendo da relação de confiança que você está editando e de qual conjunto de regras você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar o assistente de regra que está associado a esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![criar](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) de regra
    
5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como uma declaração** na lista e clique em **Avançar**.  
![criar](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG) de regra

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, no **grupo do usuário** , clique em **procurar** e selecione um grupo, em tipo de **declaração de saída** , selecione o tipo de declaração desejado e, em seguida, em tipo de **declaração de saída** digite um valor.  
![criar](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG) de regra  

7.  Clique em **Concluir**.  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  



## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criando regras de declaração para uma terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criando regras de declaração para uma confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
