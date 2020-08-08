---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: Criar uma regra para passar ou filtrar uma declaração de entrada
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a72be27746e64fb38c04d30a4dcef9b009257e0a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967297"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Criar uma regra para passar ou filtrar uma declaração de entrada

Usando a passagem ou filtrar um modelo de regra de declaração de entrada no Serviços de Federação do Active Directory (AD FS) \( AD FS \) , você pode passar todas as declarações de entrada com um tipo de declaração selecionado. Você também pode filtrar os valores de declarações de entrada com um tipo de declaração selecionada. Por exemplo, você pode usar este modelo de regra para criar uma regra que envie todas as declarações de grupo de entrada. Você também pode usar essa regra para enviar somente declarações UPN de nome UPN \( \) que terminem com o @fabrikam .

Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap-in de gerenciamento de AD FS \- .

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada em uma relação de confiança de terceira parte confiável no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, em **tipo de declaração de entrada** , selecione um tipo de declaração na lista e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Passar apenas um valor de declaração específica**

    -   **Passar apenas os valores de declaração que correspondem a um valor de sufixo de email específico**

    -   **Passar apenas os valores de declaração que começam com um valor específico** 
 ![ Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)

7.  Clique no botão **Concluir**.

8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada em uma confiança do provedor de declarações no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, em **tipo de declaração de entrada** , selecione um tipo de declaração na lista e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Passar apenas um valor de declaração específica**

    -   **Passar apenas os valores de declaração que correspondem a um valor de sufixo de email específico**

    -   **Passar apenas os valores de declaração que começam com um valor específico** 
 ![ Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)

7.  Clique no botão **Concluir**.

8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada no Windows Server 2012 R2

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em ** \\ relacionamentos de confiança do AD FSAD FS**, clique em relações de confiança do **provedor de declarações** ou em relações de confiança de terceira **parte confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  Na caixa de diálogo **Editar regras de declaração** , selecione uma das seguintes guias, dependendo da relação de confiança que você está editando e de qual conjunto de regras você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar o assistente de regra que está associado a esse conjunto de regras:

    -   **Regras de transformação de aceitação**

    -   **Regras de transformação de emissão**

    -   **Regras de Autorização de Emissão**

    -   Regras de autorização de **delegação** 
 ![ Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)

6.  Na página **Configurar regra** , em **nome da regra de declaração** , digite o nome para exibição desta regra, em **tipo de declaração de entrada** , selecione um tipo de declaração na lista e, em seguida, selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Passar apenas um valor de declaração específica**

    -   **Passar apenas os valores de declaração que correspondem a um valor de sufixo de email específico**

    -   **Passar apenas os valores de declaração que começam com um valor específico** 
 ![ Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)

7.  Clique no botão **Concluir**.

8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.




## <a name="additional-references"></a>Referências adicionais
[Configurar regras de declaração](Configure-Claim-Rules.md)

[Quando usar uma regra de declaração de passagem ou filtro](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)

