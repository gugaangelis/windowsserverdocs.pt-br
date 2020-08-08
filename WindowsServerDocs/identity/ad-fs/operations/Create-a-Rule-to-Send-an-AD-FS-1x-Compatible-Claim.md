---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: Criar uma regra para enviar uma declaração compatível com o AD FS 1. x
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4f46130129617113b8877b764bf081d0fb585e4a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967223"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Criar uma regra para enviar uma declaração compatível com o AD FS 1. x

Em situações em que você está usando Serviços de Federação do Active Directory (AD FS) \( AD FS \) para emitir declarações que serão recebidas por servidores de Federação que executam AD FS 1,0 \( Windows Server 2003 R2 \) ou AD FS 1,1 \( Windows server 2008 ou Windows Server 2008 R2 \) , você deve fazer o seguinte:

-   Crie uma regra que enviará um tipo de declaração de ID de nome com um formato de UPN, email ou nome comum.

-   Todas as outras declarações enviadas devem ter um dos seguintes tipos de declaração:

    -   AD FS 1. Endereço de email *x*

    -   AD FS 1. UPN de *x*

    -   Nome comum

    -   Agrupar

    -   Qualquer outro tipo de declaração que comece com https://schemas.xmlsoap.org/claims/ , comohttps://schemas.xmlsoap.org/claims/EmployeeID

Dependendo das necessidades da sua organização, use um dos procedimentos a seguir para criar um AD FS 1. declaração de NameID compatível com *x* :

-   Criar esta regra para emitir uma declaração de ID de nome AD FS 1. x usando o **modelo passagem ou filtrar um regra de declaração de entrada**

-   Crie essa regra para emitir uma declaração de ID de nome AD FS 1. x usando o **modelo transformar uma regra de declaração de entrada**. Você pode usar esse modelo de regra em situações em que deseja alterar o tipo de declaração existente para um novo tipo de declaração que funcionará com AD FS 1. declarações  *x* .

> [!NOTE]
> Para que essa regra funcione conforme o esperado, verifique se a relação de confiança de terceira parte confiável ou provedor de declarações em que você está criando essa regra foi configurada para usar o **perfil AD FS 1,0 e 1,1**.

## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para emitir um AD FS 1. identificação de ID *x* Name usando a passagem ou filtrar um modelo de regra de declaração de entrada em uma terceira parte confiável no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Em **tipo de declaração de entrada**, selecione **ID de nome** na lista.

8.  Em **formato de ID de nome de entrada**, selecione uma das seguintes AD FS 1. *x* \- formatos de declaração compatíveis da lista:

    -   **UPN**

    -   **\-Email**

    -   **Nome comum**

9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Passar apenas um valor de declaração específica**

    -   **Passar apenas os valores de declaração que correspondem a um valor de sufixo de email específico**

    -   **Passar apenas os valores de declaração que começam com um valor específico** 
 ![ Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. Clique em **concluir**e em **OK** para salvar a regra.


## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para emitir um AD FS 1. identificação de ID *x* Name usando a passagem ou filtrar um modelo de regra de declaração de entrada em uma confiança do provedor de declarações no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Em **tipo de declaração de entrada**, selecione **ID de nome** na lista.

8.  Em **formato de ID de nome de entrada**, selecione uma das seguintes AD FS 1. *x* \- formatos de declaração compatíveis da lista:

    -   **UPN**

    -   **\-Email**

    -   **Nome comum**

9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Passar apenas um valor de declaração específica**

    -   **Passar apenas os valores de declaração que correspondem a um valor de sufixo de email específico**

    -   **Passar apenas os valores de declaração que começam com um valor específico** 
 ![ Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. Clique em **concluir**e em **OK** para salvar a regra.


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em uma relação de confiança de terceira parte confiável no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Em **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.

8.  Em **tipo de declaração de saída**, selecione **ID de nome** na lista.

9. Em **formato de ID de nome de saída**, selecione uma das seguintes AD FS 1. *x* \- formatos de declaração compatíveis da lista:

    -   **UPN**

    -   **\-Email**

    -   **Nome comum**

10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Substituir um valor de declaração de entrada por um valor diferente de declaração de saída**

    -   **Substituir declarações de \- sufixo de email de entrada por uma nova regra de criação de \- sufixo de email** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. Clique em **concluir**e em **OK** para salvar a regra.




## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em uma confiança do provedor de declarações no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Em **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.

8.  Em **tipo de declaração de saída**, selecione **ID de nome** na lista.

9. Em **formato de ID de nome de saída**, selecione uma das seguintes AD FS 1. *x* \- formatos de declaração compatíveis da lista:

    -   **UPN**

    -   **\-Email**

    -   **Nome comum**

10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Substituir um valor de declaração de entrada por um valor diferente de declaração de saída**

    -   **Substituir declarações de \- sufixo de email de entrada por uma nova regra de criação de \- sufixo de email** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. Clique em **concluir**e em **OK** para salvar a regra.














## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Para criar uma regra para emitir um AD FS 1. declaração de ID de nome *x* usando a passagem ou filtrar um modelo de regra de declaração de entrada no Windows Server 2012 R2

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de AD FS**.

2.  Na árvore de console, em **AD FS \\ relações de confiança**, clique em **confiança do provedor de declarações** ou em relações de confiança de terceira parte **confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.

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

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Em **tipo de declaração de entrada**, selecione **ID de nome** na lista.

8.  Em **formato de ID de nome de entrada**, selecione uma das seguintes AD FS 1. *x* \- formatos de declaração compatíveis da lista:

    -   **UPN**

    -   **\-Email**

    -   **Nome comum**

9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Passar apenas um valor de declaração específica**

    -   **Passar apenas os valores de declaração que correspondem a um valor de sufixo de email específico**

    -   **Passar apenas os valores de declaração que começam com um valor específico** 
 ![ Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)

10. Clique em **concluir**e em **OK** para salvar a regra.


## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para criar uma regra para emitir um AD FS 1. declaração de ID de nome *x* usando o modelo transformar uma regra de declaração de entrada no Windows Server 2012 R2

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de AD FS**.

2.  Na árvore de console, em **AD FS \\ relações de confiança**, clique em **confiança do provedor de declarações** ou em relações de confiança de terceira parte **confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  Na caixa de diálogo **Editar regras de declaração** , selecione uma das seguintes guias, que depende da relação de confiança que você está editando e de qual conjunto de regras você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar o assistente de regra que está associado a esse conjunto de regras:

    -   **Regras de transformação de aceitação**

    -   **Regras de transformação de emissão**

    -   **Regras de Autorização de Emissão**

    -   Regras de autorização de **delegação** 
 ![ Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Em **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.

8.  Em **tipo de declaração de saída**, selecione **ID de nome** na lista.

9. Em **formato de ID de nome de saída**, selecione uma das seguintes AD FS 1. *x* \- formatos de declaração compatíveis da lista:

    -   **UPN**

    -   **\-Email**

    -   **Nome comum**

10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:

    -   **Passar todos os valores da declaração**

    -   **Substituir um valor de declaração de entrada por um valor diferente de declaração de saída**

    -   **Substituir declarações de \- sufixo de email de entrada por uma nova regra de criação de \- sufixo de email** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)

11. Clique em **concluir**e em **OK** para salvar a regra.

## <a name="additional-references"></a>Referências adicionais
[Configurar regras de declaração](Configure-Claim-Rules.md)

[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
