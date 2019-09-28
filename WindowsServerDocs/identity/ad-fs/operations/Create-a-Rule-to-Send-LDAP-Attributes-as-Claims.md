---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Criar uma regra para enviar atributos LDAP como declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4fd5a8683c9bb7bc096298caa1597a760d33edc4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358130"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Criar uma regra para enviar atributos LDAP como declarações


Usando o modelo de regra enviar atributos LDAP como declarações no \(serviços de Federação do Active Directory (AD FS)\)AD FS, você pode criar uma regra que selecionará atributos de um LDAP \(\)deprotocolodeacessodediretórioLightweightrepositório de atributos, como Active Directory, para enviar como declarações para a terceira parte confiável. Por exemplo, você pode usar esse modelo de regra para criar um enviar atributos LDAP como regra de declarações que extrairá valores de atributo para usuários autenticados dos atributos **DisplayName** e **telephoneNumber** Active Directory e, em seguida, enviará esses valores como duas declarações de saída diferentes.  
  
Você também pode usar essa regra para enviar todas as associações de grupo do usuário. Se você desejar enviar apenas as associações de grupo individuais, use o modelo de regra Enviar Associação de Grupo como uma Declaração. Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap\--in de gerenciamento de AD FS.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para enviar atributos LDAP como declarações para uma relação de confiança de terceira parte confiável no Windows Server 2016 

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar atributos LDAP como declarações** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, selecione o **repositório de atributos**e, em seguida, selecione o atributo LDAP e mapeie-o para o tipo de declaração de saída. 
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para enviar atributos LDAP como declarações para uma confiança do provedor de declarações no Windows Server 2016 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar atributos LDAP como declarações** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, selecione o **repositório de atributos**e, em seguida, selecione o atributo LDAP e mapeie-o para o tipo de declaração de saída. 
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Para criar uma regra para enviar atributos LDAP como declarações para o Windows Server 2012 R2  
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **relacionamentos de confiança\\do AD FSAD FS**, clique em relações de confiança do **provedor de declarações** ou em relações de confiança de terceira **parte confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  Na caixa de diálogo **Editar regras de declaração** , selecione uma das seguintes guias, dependendo da relação de confiança que você está editando e de qual conjunto de regras você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar o assistente de regra que está associado a esse conjunto de regras :  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar atributos LDAP como declarações** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, em **repositório de atributos** , selecione **Active Directory**e, em **mapeamento de atributos LDAP para tipos de declaração de saída** , selecione o desejado **Atributo LDAP** e tipos de **tipo de declaração de saída** correspondentes nas\-listas suspensas.  
  
    Você precisa selecionar um novo atributo LDAP e um par de tipo de declaração de saída em uma linha diferente para cada atributo de Active Directory para o qual você deseja emitir uma declaração como parte dessa regra.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
