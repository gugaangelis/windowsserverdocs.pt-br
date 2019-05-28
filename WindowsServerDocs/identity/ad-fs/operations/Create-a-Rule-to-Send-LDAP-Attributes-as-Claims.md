---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Criar uma regra para enviar atributos LDAP como declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00ea4f9f868b9c82c2a0859be971db26394251a3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189354"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Criar uma regra para enviar atributos LDAP como declarações


Usando enviar atributos LDAP como modelo de regra de declarações nos serviços de Federação do Active Directory \(do AD FS\), você pode criar uma regra que selecionará os atributos de um Lightweight Directory Access Protocol \(LDAP\)repositório de atributos, como o Active Directory, para enviar como declarações à terceira. Por exemplo, você pode usar esse modelo de regra para criar um enviar atributos LDAP como declarações de regra que extrairá os valores de atributo para usuários autenticados do **displayName** e **telephoneNumber** Active Directory Diretório de atributos e, em seguida, enviar esses valores como duas declarações de saída diferentes.  
  
Você também pode usar essa regra para enviar todas as associações de grupo do usuário. Se você desejar enviar apenas as associações de grupo individuais, use o modelo de regra Enviar Associação de Grupo como uma Declaração. Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap de gerenciamento do AD FS\-no.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para enviar atributos LDAP como declarações para uma terceira parte confiável no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo **regras de transformação de emissão** clique em **Adicionar regra** para iniciar o Assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar atributos LDAP como declarações** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para esta regra, selecione o **atributo Store**e, em seguida, selecione o atributo LDAP e mapeá-lo para o tipo de declaração de saída. 
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para enviar atributos LDAP como declarações para um provedor confiável de declarações no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **confianças do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo **regras de transformação de aceitação** clique em **Adicionar regra** para iniciar o Assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar atributos LDAP como declarações** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para esta regra, selecione o **atributo Store**e, em seguida, selecione o atributo LDAP e mapeá-lo para o tipo de declaração de saída. 
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Para criar uma regra para enviar atributos LDAP como declarações para o Windows Server 2012 R2  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **FSAD do AD FS\\relações de confiança**, clique em **confianças do provedor de declarações** ou **terceira**e, em seguida, clique em um relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  No **editar regras de declaração** caixa de diálogo, selecione um guias a seguir, dependendo da relação de confiança que você está editando e qual regra definida que você deseja criar essa regra no e, em seguida, clique em **Adicionar regra** para iniciar a regra Assistente que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar atributos LDAP como declarações** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, em **repositório de atributos** selecione **do Active Directory**e, em **tipos de declaração de mapeamento de atributos LDAP para saída** selecione o desejado **atributo LDAP** e o correspondente **tipo de declaração de saída** tipos na lista\-listas suspensas.  
  
    Você precisa selecionar um novo atributo LDAP e o par de tipo de declaração de saída em uma linha diferente para cada atributo do Active Directory que você deseja emitir uma declaração para como parte dessa regra.  
![Criar regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
