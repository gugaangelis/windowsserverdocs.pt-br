---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Criar uma regra para enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f0eef89e651585d48ba87d14bc782efa49087669
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Criar uma regra para enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Usando o **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada** modelo nos serviços de Federação do Active Directory (AD FS), você pode criar regras de declaração personalizada para a situação em que um modelo de regra padrão não atende aos requisitos da sua organização. Regras de declaração personalizada são escritas na linguagem de regra de declaração e, em seguida, devem ser copiadas para o **regra personalizada** caixa de texto antes que eles podem ser usados em um conjunto de regras. Para obter informações sobre como construir a sintaxe para uma regra avançada, consulte [a função da linguagem de regra de declaração](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração usando o AD FS snap\-in Gerenciamento.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para passar pela ou filtrar uma declaração de entrada em uma dependência terceiros confiar no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **Editar política de emissão de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo, em **regras de transformação de emissão** clique **Adicionar regra** para iniciar o Assistente de regra. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra. Em **regra personalizada**, digite ou cole a sintaxe da linguagem reivindicação regra que você deseja para esta regra.  
![Criar a regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Clique em **concluir **.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para passar pela ou filtrar uma declaração de entrada em um provedor de declarações confiar no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, em **regras de transformação de aceitação** clique **Adicionar regra** para iniciar o Assistente de regra.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra. Em **regra personalizada**, digite ou cole a sintaxe da linguagem reivindicação regra que você deseja para esta regra.  
![Criar a regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Clique em **concluir **.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Para criar uma regra para enviar requerimentos judiciais ou Extrajudiciais usando uma declaração personalizada no Windows Server 2012 R2 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e clique em **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS\\Trust relações**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** ou **confie dependência de terceiros**e, em seguida, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  No **editar regras de declaração** caixa de diálogo, selecione uma guias a seguir, que depende de confiança que você está editando e na qual regra defina você desejam criar essa regra e clique em **Adicionar regra** para iniciar o Assistente de regra que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra. Em **regra personalizada**, digite ou cole a sintaxe da linguagem reivindicação regra que você deseja para esta regra.  
![Criar a regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  Clique em **concluir **.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
