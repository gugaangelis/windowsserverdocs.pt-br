---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: "Criar uma regra para membros do grupo de envio como uma declaração"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d4fb03de9de2dcca36b18ce089db11ed599de820
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>Criar uma regra para membros do grupo de envio como uma declaração

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Usando a associação do grupo Enviar como um modelo de regra de declaração no \(AD FS\) serviços de Federação do Active Directory, você pode criar uma regra que tornará possível para selecionar um grupo de segurança do Active Directory para enviar como uma reivindicação. Somente uma única declaração será emitida dessa regra, com base no grupo que você selecionar. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará uma declaração de grupo com um valor de administrador, se o usuário é um membro do grupo de segurança Administradores de domínio. Essa regra deve ser usada apenas para usuários no domínio do Active Directory local.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o gerenciamento do AD FS snap\-in.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação ao grupo como uma declaração em uma dependência terceiros confiar no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **Editar política de emissão de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo, em **regras de transformação de emissão** clique **Adicionar regra** para iniciar o Assistente de regra. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar a associação de grupo como declaração** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   No **configurar regra** página em **nome da regra de declaração** digitar o nome de exibição para essa regra, **grupo do usuário** clique **navegar** e selecionar um grupo, em **tipo de declaração de saída** selecionar o tipo de declaração desejado e, em seguida, em **saída tipo reivindicar** digitar um valor.
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
## <a name="to-create-a-rule-to-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para enviar a associação ao grupo como uma declaração em um provedor de declarações confiar no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, em **regras de transformação de aceitação** clique **Adicionar regra** para iniciar o Assistente de regra.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar a associação de grupo como declaração** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   No **configurar regra** página em **nome da regra de declaração** digitar o nome de exibição para essa regra, **grupo do usuário** clique **navegar** e selecionar um grupo, em **tipo de declaração de saída** selecionar o tipo de declaração desejado e, em seguida, em **saída tipo reivindicar** digitar um valor. 
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>Para criar uma regra para enviar a associação ao grupo como uma declaração no Windows Server 2012 R2 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS\\Trust relações**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** ou **confie dependência de terceiros**e, em seguida, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  No **editar regras de declaração** caixa de diálogo, selecione uma guias a seguir, dependendo da relação de confiança que você está editando e defina qual regra você deseja criar essa regra em e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar a associação de grupo como uma declaração** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  No **configurar regra** página em **nome da regra de declaração** digitar o nome de exibição para essa regra, **grupo do usuário** clique **navegar** e selecionar um grupo, em **tipo de declaração de saída** selecionar o tipo de declaração desejado e, em seguida, em **saída tipo reivindicar** digitar um valor.  
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  Clique em **concluir **.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  



## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
