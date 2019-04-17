---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: "Criar uma regra para transformar uma declaração de entrada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3745a0ab9d313223c611e58864dd6b4d747f0624
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Criar uma regra para enviar uma reivindicação compatível do AD FS 1. x

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2


Em situações em que você estiver usando os serviços de Federação do Active Directory \(AD FS\) para emitir diz que será recebida por servidores de Federação executando o AD FS 1.0 \ (Windows Server 2003 R2\) ou AD FS 1.1 \ (Windows Server 2008 ou Windows Server 2008 R2\), você deve fazer o seguinte:  
  
-   Crie uma regra que enviará um tipo de declaração de nome ID com um formato de UPN, Email ou nome comum.  
  
-   Todos os outros requerimentos são enviados devem ter um dos seguintes tipos de declaração:  
  
    -   AD FS 1. *x* endereço de Email  
  
    -   AD FS 1. *x* UPN  
  
    -   Nome comum  
  
    -   Grupo  
  
    -   Qualquer outro tipo de declaração que começa com https://schemas.xmlsoap.org/claims/, como https://schemas.xmlsoap.org/claims/EmployeeID  
  
Dependendo das necessidades da sua organização, use um dos seguintes procedimentos para criar um AD FS 1. *x* compatível reivindicação NameID:  
  
-   Criar essa regra problema um AD FS 1. x nome ID reivindicação usando o **passagem ou um modelo de regra reivindica a entrada de filtro**  
  
-   Criar essa regra problema um AD FS 1. x nome ID reivindicação usando o **transformar um modelo de regra reivindicar entrada**. Você pode usar esse modelo de regra em situações em que você deseja alterar o tipo de declaração existente para um novo tipo de declaração que funcione com o AD FS 1. *x* requerimentos judiciais ou Extrajudiciais.  
  
> [!NOTE]  
> Para essa regra funcione como esperado, certifique-se de que a terceira confiança de terceiros ou relação de confiança do provedor requerimentos judiciais ou Extrajudiciais onde você estiver criando essa regra tiver sido configurada para usar o **perfil do AD FS 1.0 e 1.1**. 




## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para emitir um AD FS 1. *x* nome ID reivindicar usando a passagem ou filtrar um modelo de regra reivindicar entrada em uma dependência terceiros confiar no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **Editar política de emissão de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo, em **regras de transformação de emissão** clique **Adicionar regra** para iniciar o Assistente de regra. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passagem ou filtro uma declaração de entrada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Em **tipo de declaração de entrada**, selecione **nome ID** na lista.  
  
8.  Em **formato de ID de nome de entrada**, selecione uma das seguinte AD FS 1. *x*\-compatible reivindicar formatos da lista:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comum**  
  
9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Passagem somente um determinado valor de solicitação**  
  
    -   **Passar por meio de apenas os valores de declaração que correspondem a um valor de sufixo email específico**  
  
    -   **Passar por meio de apenas os valores de declaração que começam com um valor específico**  
![Criar a regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Clique em **concluir**e clique em **Okey** para salvar a regra.  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para emitir um AD FS 1. *x* nome ID reivindicar usando a passagem ou filtrar um modelo de regra reivindicar entrada em um provedor de declarações confiar no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, em **regras de transformação de aceitação** clique **Adicionar regra** para iniciar o Assistente de regra.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passagem ou filtro uma declaração de entrada** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Em **tipo de declaração de entrada**, selecione **nome ID** na lista.  
  
8.  Em **formato de ID de nome de entrada**, selecione uma das seguinte AD FS 1. *x*\-compatible reivindicar formatos da lista:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comum**  
  
9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Passagem somente um determinado valor de solicitação**  
  
    -   **Passar por meio de apenas os valores de declaração que correspondem a um valor de sufixo email específico**  
  
    -   **Passar por meio de apenas os valores de declaração que começam com um valor específico**  
![Criar a regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Clique em **concluir**e clique em **Okey** para salvar a regra.  

  

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em uma dependência terceiros confiar no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **Editar política de emissão de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo, em **regras de transformação de emissão** clique **Adicionar regra** para iniciar o Assistente de regra. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Em **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.  
  
8.  Em **tipo de declaração de saída**, selecione **nome ID** na lista.  
  
9. Em **formato de ID de nome de saída**, selecione uma das seguinte AD FS 1. *x*\-compatible reivindicar formatos da lista:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comum**  
  
10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substitua declarações de sufixo e\ email de entrada um novo sufixo e\ mail**  
![Criar a regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Clique em **concluir**e clique em **Okey** para salvar a regra.  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em um provedor de declarações confiar no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, em **regras de transformação de aceitação** clique **Adicionar regra** para iniciar o Assistente de regra.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Em **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.  
  
8.  Em **tipo de declaração de saída**, selecione **nome ID** na lista.  
  
9. Em **formato de ID de nome de saída**, selecione uma das seguinte AD FS 1. *x*\-compatible reivindicar formatos da lista:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comum**  
  
10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substitua declarações de sufixo e\ email de entrada um novo sufixo e\ mail**  
![Criar a regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Clique em **concluir**e clique em **Okey** para salvar a regra.  













  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Para criar uma regra para emitir um AD FS 1. *x* nome ID reivindicar usando a passagem ou filtrar um modelo de regra reivindicar entrada no Windows Server 2012 R2
  
1.  No Gerenciador do servidor, clique em **ferramentas**e clique em **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS\\Trust relações**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** ou **confie dependência de terceiros**e, em seguida, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
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
  
6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Em **tipo de declaração de entrada**, selecione **nome ID** na lista.  
  
8.  Em **formato de ID de nome de entrada**, selecione uma das seguinte AD FS 1. *x*\-compatible reivindicar formatos da lista:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comum**  
  
9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Passagem somente um determinado valor de solicitação**  
  
    -   **Passar por meio de apenas os valores de declaração que correspondem a um valor de sufixo email específico**  
  
    -   **Passar por meio de apenas os valores de declaração que começam com um valor específico**  
![Criar a regra](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. Clique em **concluir**e clique em **Okey** para salvar a regra.  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para criar uma regra para emitir um AD FS 1. *x* declaração de nome ID usando a transformação de um modelo de regra reivindicar entrada no Windows Server 2012 R2  
  
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
  
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Em **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.  
  
8.  Em **tipo de declaração de saída**, selecione **nome ID** na lista.  
  
9. Em **formato de ID de nome de saída**, selecione uma das seguinte AD FS 1. *x*\-compatible reivindicar formatos da lista:  
  
    -   **UPN**  
  
    -   **E\ Mail**  
  
    -   **Nome comum**  
  
10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substitua declarações de sufixo e\ email de entrada um novo sufixo e\ mail**  
![Criar a regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. Clique em **concluir**e clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
