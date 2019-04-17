---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: "Criar uma regra para enviar atributos LDAP como declarações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Criar uma regra para enviar atributos LDAP como declarações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Usando os atributos de LDAP enviar como modelo de regra de declarações no \(AD FS\) serviços de Federação do Active Directory, você pode criar uma regra que selecionará os atributos de um repositório de atributo \(LDAP\) Lightweight Directory Access Protocol, como o Active Directory, para enviar como declarações para a parte confiante. Por exemplo, você pode usar esse modelo de regra para criar um atributos de LDAP enviar conforme requerimentos judiciais ou Extrajudiciais de regras que extrairá valores de atributo para usuários autenticados do **displayName** e **telephoneNumber** Active Directory atributos e, em seguida, enviar esses valores como duas declarações de saída diferentes.  
  
Você também pode usar essa regra para enviar os membros do grupo Todos do usuário. Se você quiser enviar somente membros do grupo individuais, use a associação do grupo Enviar como um modelo de regra de declaração. Você pode usar o procedimento a seguir para criar uma regra de declaração com o gerenciamento do AD FS snap\-in.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para enviar atributos LDAP como declarações para uma dependência terceiros confiar no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **Editar política de emissão de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo, em **regras de transformação de emissão** clique **Adicionar regra** para iniciar o Assistente de regra. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar LDAP atributos como declarações** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  No **configurar regra** página em **nome da regra reivindicação** digite o nome de exibição para essa regra, selecione o **atributo Store**e, em seguida, selecione o atributo LDAP e mapeá-lo para o tipo de declaração de saída. 
![Criar a regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para enviar atributos LDAP como reivindicações de um provedor de declarações confiar no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, em **regras de transformação de aceitação** clique **Adicionar regra** para iniciar o Assistente de regra.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar LDAP atributos como declarações** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  No **configurar regra** página em **nome da regra reivindicação** digite o nome de exibição para essa regra, selecione o **atributo Store**e, em seguida, selecione o atributo LDAP e mapeá-lo para o tipo de declaração de saída. 
![Criar a regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Para criar uma regra para enviar atributos LDAP como declarações para o Windows Server 2012 R2  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FSAD FS\\Trust relações**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** ou **confie dependência de terceiros**e, em seguida, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  No **editar regras de declaração** caixa de diálogo, selecione uma guias a seguir, dependendo da relação de confiança que você está editando e defina qual regra você deseja criar essa regra em e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar LDAP atributos como declarações** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  No **configurar regra** página em **nome da regra reivindicação** digite o nome de exibição para essa regra, em **store atributo** selecione **do Active Directory**e, em **tipos de declaração de atributos de mapeamento de LDAP para saída** selecionar o desejado **atributo LDAP** e correspondente **saída tipo reivindicar** tipos na lista suspensa drop\.  
  
    Você precisa selecionar um novo atributo LDAP e saída par de tipo de declaração em uma linha diferente para cada atributo do Active Directory que você deseja emitir uma declaração para como parte dessa regra.  
![Criar a regra](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
