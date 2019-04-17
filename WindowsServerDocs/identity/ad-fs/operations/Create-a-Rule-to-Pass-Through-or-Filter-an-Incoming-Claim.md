---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: "Criar uma regra para passar pela ou filtrar uma declaração de entrada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Criar uma regra para passar pela ou filtrar uma declaração de entrada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Usando a passagem ou filtro um modelo de regra reivindicar entrada em serviços de Federação do Active Directory \(AD FS\), você pode passar por meio de todas as reclamações de entrada com um tipo de declaração selecionado. Você também pode filtrar os valores das declarações de entrada com um tipo de declaração selecionado. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará todas as declarações de grupo recebidas. Você também pode usar essa regra para enviar somente usuário UPN \(UPN\) requerimentos judiciais ou Extrajudiciais que terminam com @fabrikam.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o gerenciamento do AD FS snap\-in.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para passar pela ou filtrar uma declaração de entrada em uma dependência terceiros confiar no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **Editar política de emissão de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo, em **regras de transformação de emissão** clique **Adicionar regra** para iniciar o Assistente de regra. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passagem ou filtro uma declaração de entrada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  No **configurar regra** página em **nome da regra reivindicação** digitar o nome de exibição para essa regra, **tipo de declaração de entrada** selecione um tipo de declaração a lista e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Passagem somente um determinado valor de solicitação**  
  
    -   **Passar por meio de apenas os valores de declaração que correspondem a um valor de sufixo email específico**  
  
    -   **Passar por meio de apenas os valores de declaração que começam com um valor específico**  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para passar pela ou filtrar uma declaração de entrada em um provedor de declarações confiar no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, em **regras de transformação de aceitação** clique **Adicionar regra** para iniciar o Assistente de regra.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passagem ou filtro uma declaração de entrada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  No **configurar regra** página em **nome da regra reivindicação** digitar o nome de exibição para essa regra, **tipo de declaração de entrada** selecione um tipo de declaração a lista e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Passagem somente um determinado valor de solicitação**  
  
    -   **Passar por meio de apenas os valores de declaração que correspondem a um valor de sufixo email específico**  
  
    -   **Passar por meio de apenas os valores de declaração que começam com um valor específico**  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Para criar uma regra para passar pela ou filtrar uma declaração de entrada no Windows Server 2012 R2

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FSAD FS\\Trust relações**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** ou **confie dependência de terceiros**e, em seguida, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, selecione uma guias a seguir, dependendo da relação de confiança que você está editando e qual regra configurá-lo deseja criar essa regra em e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passagem ou filtro uma declaração de entrada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  No **configurar regra** página em **nome da regra reivindicação** digitar o nome de exibição para essa regra, **tipo de declaração de entrada** selecione um tipo de declaração a lista e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Passagem somente um determinado valor de solicitação**  
  
    -   **Passar por meio de apenas os valores de declaração que correspondem a um valor de sufixo email específico**  
  
    -   **Passar por meio de apenas os valores de declaração que começam com um valor específico**  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  



  
## <a name="additional-references"></a>Referências adicionais  
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
  
[Quando usar uma passagem ou uma regra de declaração de filtro](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
