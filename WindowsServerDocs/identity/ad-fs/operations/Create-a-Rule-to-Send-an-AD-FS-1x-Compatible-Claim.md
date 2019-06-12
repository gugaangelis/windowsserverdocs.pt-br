---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: Criar uma regra para transformar uma declaração de entrada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bda071be6668710361205643125fc8ad44246012
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453019"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Criar uma regra para enviar uma declaração compatível do AD FS 1.x

Em situações em que você estiver usando os serviços de Federação do Active Directory \(do AD FS\) para emitir declarações que serão recebidas pelos servidores de Federação executando o AD FS 1.0 \(Windows Server 2003 R2\) ou AD FS 1.1 \(Windows Server 2008 ou Windows Server 2008 R2\), você deve fazer o seguinte:  
  
-   Crie uma regra que enviará um tipo de declaração de ID de nome com um formato de UPN, Email ou nome comum.  
  
-   Todas as outras declarações que são enviadas devem ter um dos seguintes tipos de declaração:  
  
    -   O AD FS 1. *x* endereço de Email  
  
    -   O AD FS 1. *x* UPN  
  
    -   Nome comum  
  
    -   Grupo  
  
    -   Qualquer outro tipo de declaração que começa com https://schemas.xmlsoap.org/claims/, tais como https://schemas.xmlsoap.org/claims/EmployeeID  
  
Dependendo das necessidades da sua organização, use um dos procedimentos a seguir para criar um AD FS 1. *x* declaração de NameID compatível com:  
  
-   Crie essa regra de declaração usando o problema an AD FS 1.x ID de nome de **passar ou filtrar um modelo de regra de declaração de entrada**  
  
-   Crie essa regra de declaração usando o problema an AD FS 1.x ID de nome de **transformar um modelo de regra de declaração de entrada**. Você pode usar esse modelo de regra em situações em que você deseja alterar o tipo de declaração existente para um novo tipo de declaração que funcionarão com o AD FS 1.  *x* declarações.  
  
> [!NOTE]  
> Para esta regra funcione conforme o esperado, certifique-se de que a terceira parte confiável ou relação de confiança de provedor de declarações em que você está criando essa regra foi configurada para usar o **perfil do AD FS 1.0 e 1.1**. 

## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para emitir um AD FS 1. *x* ID de nome de declaração usando a passagem ou filtrar um modelo de regra de declaração de entrada em uma terceira parte confiável no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo **regras de transformação de emissão** clique em **Adicionar regra** para iniciar o Assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e, em seguida, clique **Avançar** .  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Na **tipo de declaração de entrada**, selecione **ID de nome** na lista.  
  
8.  Na **formato de ID de nome de entrada**, selecione uma da seguir do AD FS 1. *x*\-compatível com formatos na lista de declaração:  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nome comum**  
  
9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Passar apenas um determinado valor de declaração**  
  
    -   **Passar apenas valores de declaração que correspondem a um valor de sufixo de email específico**  
  
    -   **Passar apenas valores de declaração que começam com um valor específico**  
![Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Clique em **terminar**e, em seguida, clique em **Okey** para salvar a regra.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para emitir um AD FS 1. *x* ID de nome de declaração usando a passagem ou filtrar um modelo de regra de declaração de entrada em um provedor de declarações confiável no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **confianças do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo **regras de transformação de aceitação** clique em **Adicionar regra** para iniciar o Assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **passar ou filtrar uma declaração de entrada** na lista e, em seguida, clique **Avançar** .  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Na **tipo de declaração de entrada**, selecione **ID de nome** na lista.  
  
8.  Na **formato de ID de nome de entrada**, selecione uma da seguir do AD FS 1. *x*\-compatível com formatos na lista de declaração:  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nome comum**  
  
9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Passar apenas um determinado valor de declaração**  
  
    -   **Passar apenas valores de declaração que correspondem a um valor de sufixo de email específico**  
  
    -   **Passar apenas valores de declaração que começam com um valor específico**  
![Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. Clique em **terminar**e, em seguida, clique em **Okey** para salvar a regra.  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em uma terceira parte confiável no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)
  
4.  No **Editar política de emissão de declaração** caixa de diálogo **regras de transformação de emissão** clique em **Adicionar regra** para iniciar o Assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Na **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.  
  
8.  Na **tipo de declaração de saída**, selecione **ID de nome** na lista.  
  
9. Na **formato de ID de nome de saída**, selecione uma da seguir do AD FS 1. *x*\-compatível com formatos na lista de declaração:  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nome comum**  
  
10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substituir e entrada\-declarações de sufixo com um novo e de email\-sufixo de email**  
![Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. Clique em **terminar**e, em seguida, clique em **Okey** para salvar a regra.  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em um provedor de declarações confiável no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **confianças do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo **regras de transformação de aceitação** clique em **Adicionar regra** para iniciar o Assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Na **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.  
  
8.  Na **tipo de declaração de saída**, selecione **ID de nome** na lista.  
  
9. Na **formato de ID de nome de saída**, selecione uma da seguir do AD FS 1. *x*\-compatível com formatos na lista de declaração:  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nome comum**  
  
10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substituir e entrada\-declarações de sufixo com um novo e de email\-sufixo de email**  
![Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Clique em **terminar**e, em seguida, clique em **Okey** para salvar a regra.  













  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Para criar uma regra para emitir um AD FS 1. *x* ID de nome de declaração usando a passagem ou filtrar um modelo de regra de declaração de entrada no Windows Server 2012 R2
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS\\relações de confiança**, clique em **confianças do provedor de declarações** ou **terceira**e, em seguida, clique em um determinado confiar na lista de onde você deseja criar essa regra.  
  
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
  
6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Na **tipo de declaração de entrada**, selecione **ID de nome** na lista.  
  
8.  Na **formato de ID de nome de entrada**, selecione uma da seguir do AD FS 1. *x*\-compatível com formatos na lista de declaração:  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nome comum**  
  
9. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Passar apenas um determinado valor de declaração**  
  
    -   **Passar apenas valores de declaração que correspondem a um valor de sufixo de email específico**  
  
    -   **Passar apenas valores de declaração que começam com um valor específico**  
![Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)

10. Clique em **terminar**e, em seguida, clique em **Okey** para salvar a regra.  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para criar uma regra para emitir um AD FS 1. *x* declaração de ID de nome usando a transformação de um modelo de regra de declaração de entrada no Windows Server 2012 R2  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS\\relações de confiança**, clique em **confianças do provedor de declarações** ou **terceira**e, em seguida, clique em um determinado confiar na lista de onde você deseja criar essa regra.  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  No **editar regras de declaração** caixa de diálogo, selecione uma ou mais guias a seguir, que depende a relação de confiança que você está editando e em qual regra conjunto que você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar a regra Assistente que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Na **tipo de declaração de entrada**, selecione o tipo de declaração de entrada que você deseja transformar na lista.  
  
8.  Na **tipo de declaração de saída**, selecione **ID de nome** na lista.  
  
9. Na **formato de ID de nome de saída**, selecione uma da seguir do AD FS 1. *x*\-compatível com formatos na lista de declaração:  
  
    -   **UPN**  
  
    -   **E\-Mail**  
  
    -   **Nome comum**  
  
10. Selecione uma das seguintes opções, dependendo das necessidades da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substituir e entrada\-declarações de sufixo com um novo e de email\-sufixo de email**  
![Criar regra](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. Clique em **terminar**e, em seguida, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
