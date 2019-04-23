---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: Criar uma regra para enviar associação a um grupo como uma declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 96ab653393fbc5f0a4306db53f84c2d9ba6c7f5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847447"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Criar uma regra para enviar associação a um grupo como uma declaração

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Usando a associação de grupo Enviar como um modelo de regra de declaração em serviços de Federação do Active Directory \(do AD FS\), você pode criar uma regra que tornará possível para você selecionar um grupo de segurança do Active Directory para enviar como uma declaração. Somente uma única declaração será emitida dessa regra, com base no grupo que você seleciona. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará uma declaração de grupo com um valor de administrador, se o usuário for um membro do grupo de segurança Admins. do domínio. Essa regra deve ser usada apenas para usuários no domínio do Active Directory local.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap de gerenciamento do AD FS\-no.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação de grupo como uma declaração em uma terceira parte confiável no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo **regras de transformação de emissão** clique em **Adicionar regra** para iniciar o Assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **Send Group Membership como declaração** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, na **grupo do usuário** clique **procurar** e selecione um grupo, sob **tipo de declaração de saída** selecionar o tipo de declaração desejado e, em **tipo de declaração de saída** digite um valor.
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação de grupo como uma declaração em um provedor de declarações confiável no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **confianças do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo **regras de transformação de aceitação** clique em **Adicionar regra** para iniciar o Assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **Send Group Membership como declaração** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, na **grupo do usuário** clique **procurar** e selecione um grupo, sob **tipo de declaração de saída** selecionar o tipo de declaração desejado e, em **tipo de declaração de saída** digite um valor. 
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para criar uma regra para enviar a associação de grupo como uma declaração no Windows Server 2012 R2 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS\\relações de confiança**, clique em **confianças do provedor de declarações** ou **terceira**e, em seguida, clique em um determinado confiar na lista de onde você deseja criar essa regra.  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  No **editar regras de declaração** caixa de diálogo, selecione um guias a seguir, dependendo da relação de confiança que você está editando e qual regra definida que você deseja criar essa regra no e, em seguida, clique em **Adicionar regra** para iniciar a regra Assistente que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar associação de grupo como uma declaração** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, na **grupo do usuário** clique **procurar** e selecione um grupo, sob **tipo de declaração de saída** selecionar o tipo de declaração desejado e, em **tipo de declaração de saída** digite um valor.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Clique em **concluir**.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  



## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criando regras de declaração para a Relying Party Trust](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criando regras de declaração para um provedor de declarações de confiança](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
