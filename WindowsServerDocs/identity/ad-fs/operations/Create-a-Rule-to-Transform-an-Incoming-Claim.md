---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: "Criar uma regra para transformar uma declaração de entrada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e982b7608f7602268657ceae74f641bbaaaec939
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Criar uma regra para transformar uma declaração de entrada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Usando o **transformar uma entrada reivindicar** modelo de regra em \(AD FS\) serviços de Federação do Active Directory, você pode selecionar uma declaração de entrada, alterar seu tipo de declaração e alterar seu valor de declaração. Por exemplo, você pode usar esse modelo de regra para criar uma regra que envia uma declaração de função com o mesmo valor de declaração de uma declaração de grupo de entrada. Você também pode usar essa regra para enviar a declaração de um grupo com um valor de declaração de compradores quando há uma declaração de grupo de entrada com um valor de administradores, ou você pode enviar somente UPN \(UPN\) diz que terminam com @fabrikam.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o gerenciamento do AD FS snap\-in.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477). 

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

6.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra. Em **tipo de declaração de entrada**, selecione uma reivindicação digitar a lista. Em **tipo de declaração de saída**, selecione um tipo de declaração a lista e, em seguida, selecione uma das opções a seguir, que depende dos requisitos da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substitua declarações de sufixo e\ email de entrada um novo sufixo e\ mail**  
![Criar a regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa AD FS\ emitidas requerimentos judiciais ou Extrajudiciais, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e, em **tipo de declaração de entrada**, digite o nome para a declaração de entrada ou, se uma descrição de declaração criada anteriormente, selecione-o na lista. Segundo, em **tipo de declaração de saída**, selecione o URL reivindicação que você deseja e, em seguida, criar uma regra de transformação na terceira confiança de terceiros para emitir a declaração de dispositivo.  
>   
> Para saber mais sobre cenários de controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando o AD DS declarações com o AD FS ](https://technet.microsoft.com/library/hh831504.aspx). 

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

6.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra. Em **tipo de declaração de entrada**, selecione uma reivindicação digitar a lista. Em **tipo de declaração de saída**, selecione um tipo de declaração a lista e, em seguida, selecione uma das opções a seguir, que depende dos requisitos da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substitua declarações de sufixo e\ email de entrada um novo sufixo e\ mail**  
![Criar a regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Clique no **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa AD FS\ emitidas requerimentos judiciais ou Extrajudiciais, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e, em **tipo de declaração de entrada**, digite o nome para a declaração de entrada ou, se uma descrição de declaração criada anteriormente, selecione-o na lista. Segundo, em **tipo de declaração de saída**, selecione o URL reivindicação que você deseja e, em seguida, criar uma regra de transformação na terceira confiança de terceiros para emitir a declaração de dispositivo.  
>   
> Para saber mais sobre cenários de controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando o AD DS declarações com o AD FS ](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Para criar uma regra para transformar uma declaração de entrada no Windows Server 2012 R2 
  
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

6.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra. Em **tipo de declaração de entrada**, selecione uma reivindicação digitar a lista. Em **tipo de declaração de saída**, selecione um tipo de declaração a lista e, em seguida, selecione uma das opções a seguir, que depende dos requisitos da sua organização:  
  
    -   **Passar por todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substitua declarações de sufixo e\ email de entrada um novo sufixo e\ mail**  
![Criar a regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa AD FS\ emitidas requerimentos judiciais ou Extrajudiciais, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e, em **tipo de declaração de entrada**, digite o nome para a declaração de entrada ou, se uma descrição de declaração criada anteriormente, selecione-o na lista. Segundo, em **tipo de declaração de saída**, selecione o URL reivindicação que você deseja e, em seguida, criar uma regra de transformação na terceira confiança de terceiros para emitir a declaração de dispositivo.  
>   
> Para saber mais sobre cenários de controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando o AD DS declarações com o AD FS ](https://technet.microsoft.com/library/hh831504.aspx).  
  
7.  Clique em **concluir **.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
