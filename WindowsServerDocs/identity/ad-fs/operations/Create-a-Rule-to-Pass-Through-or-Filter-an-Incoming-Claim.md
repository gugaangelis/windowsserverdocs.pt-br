---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: Criar uma regra para passar ou filtrar uma declaração de entrada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860267"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Criar uma regra para passar ou filtrar uma declaração de entrada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Usando a passagem ou filtrar um modelo de regra de declaração de entrada nos serviços de Federação do Active Directory \(do AD FS\), você pode passar por todas as declarações de entrada com um tipo de declaração selecionado. Você também pode filtrar os valores das declarações de entrada com um tipo de declaração selecionado. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará todas as declarações de grupo de entrada. Você também pode usar essa regra para enviar apenas nome UPN \(UPN\) declarações que terminam com @fabrikam.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap de gerenciamento do AD FS\-no.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada em uma terceira parte confiável no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo **regras de transformação de emissão** clique em **Adicionar regra** para iniciar o Assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e, em seguida, clique **Avançar** .  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, na **tipo de declaração de entrada** selecione um tipo de declaração na lista e, em seguida, selecione uma da opções a seguir, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Passar apenas um determinado valor de declaração**  
  
    -   **Passar apenas valores de declaração que correspondem a um valor de sufixo de email específico**  
  
    -   **Passar apenas valores de declaração que começam com um valor específico**  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada em um provedor de declarações confiável no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **confianças do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo **regras de transformação de aceitação** clique em **Adicionar regra** para iniciar o Assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e, em seguida, clique **Avançar** .  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, na **tipo de declaração de entrada** selecione um tipo de declaração na lista e, em seguida, selecione uma da opções a seguir, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Passar apenas um determinado valor de declaração**  
  
    -   **Passar apenas valores de declaração que correspondem a um valor de sufixo de email específico**  
  
    -   **Passar apenas valores de declaração que começam com um valor específico**  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada no Windows Server 2012 R2

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **FSAD do AD FS\\relações de confiança**, clique em **confianças do provedor de declarações** ou **terceira**e, em seguida, clique em um relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, selecione um guias a seguir, dependendo da relação de confiança que você está editando e qual regra definida que você deseja criar essa regra no e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e, em seguida, clique **Avançar** .  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  No **configurar regra** página sob **nome da regra de declaração** digite o nome de exibição para essa regra, na **tipo de declaração de entrada** selecione um tipo de declaração na lista e, em seguida, selecione uma da opções a seguir, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Passar apenas um determinado valor de declaração**  
  
    -   **Passar apenas valores de declaração que correspondem a um valor de sufixo de email específico**  
  
    -   **Passar apenas valores de declaração que começam com um valor específico**  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  



  
## <a name="additional-references"></a>Referências adicionais  
[Configurar regras de declaração](Configure-Claim-Rules.md)  
  
[Quando usar uma passagem pelo or Filter Claim Rule](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
