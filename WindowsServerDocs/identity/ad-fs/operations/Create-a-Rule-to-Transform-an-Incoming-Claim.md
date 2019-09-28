---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Criar uma regra para transformar uma declaração de entrada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 15a4583d429de9383e9405cfcd444777aa55c921
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407578"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Criar uma regra para transformar uma declaração de entrada


Usando o modelo **transformar uma** regra de declaração de entrada em serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-2, você pode selecionar uma declaração de entrada, alterar seu tipo de declaração e alterar seu valor de declaração. Por exemplo, você pode usar esse modelo de regra para criar uma regra que envia uma declaração de função com o mesmo valor de declaração de uma declaração de grupo de entrada. Você também pode usar essa regra para enviar uma declaração de grupo com um valor de declaração de compradores quando houver uma declaração de grupo de entrada com um valor de administradores, ou você pode enviar somente declarações de nome UPN \(UPN @ no__t-1 que terminem com @fabrikam.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap\--in de gerenciamento de AD FS.  
  
A associação em **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em uma relação de confiança de terceira parte confiável no Windows Server 2016 

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra. Em **tipo de declaração de entrada**, selecione um tipo de declaração na lista. Em **tipo de declaração de saída**, selecione um tipo de declaração na lista e, em seguida, selecione uma das seguintes opções, que depende dos requisitos da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada por um valor de declaração de saída diferente**  
  
    -   **Substituir declarações de sufixo de entrada e @ no__t-1mail por um novo sufixo e @ no__t-2mail**  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.
  
> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa declarações AD FS @ no__t-0issued, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e, em **tipo de declaração de entrada**, digite o nome da declaração de entrada ou, se uma descrição de declaração tiver sido criado anteriormente, selecione-o na lista. Em segundo lugar, em **tipo de declaração de saída**, selecione a URL de declaração que você deseja e, em seguida, crie uma regra de transformação na relação de confiança da terceira parte confiável para emitir a declaração do dispositivo.  
>   
> Para obter mais informações sobre cenários de controle de acesso dinâmico, consulte [roteiro de conteúdo de controle de acesso dinâmico](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando declarações de AD DS com AD FS](https://technet.microsoft.com/library/hh831504.aspx). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para transformar uma declaração de entrada em uma confiança do provedor de declarações no Windows Server 2016 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra. Em **tipo de declaração de entrada**, selecione um tipo de declaração na lista. Em **tipo de declaração de saída**, selecione um tipo de declaração na lista e, em seguida, selecione uma das seguintes opções, que depende dos requisitos da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada por um valor de declaração de saída diferente**  
  
    -   **Substituir declarações de sufixo de entrada e @ no__t-1mail por um novo sufixo e @ no__t-2mail**  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Clique no botão **concluir** .  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  

> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa declarações AD FS @ no__t-0issued, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e, em **tipo de declaração de entrada**, digite o nome da declaração de entrada ou, se uma descrição de declaração tiver sido criado anteriormente, selecione-o na lista. Em segundo lugar, em **tipo de declaração de saída**, selecione a URL de declaração que você deseja e, em seguida, crie uma regra de transformação na relação de confiança da terceira parte confiável para emitir a declaração do dispositivo.  
>   
> Para obter mais informações sobre cenários de controle de acesso dinâmico, consulte [roteiro de conteúdo de controle de acesso dinâmico](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando declarações de AD DS com AD FS](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Para criar uma regra para transformar uma declaração de entrada no Windows Server 2012 R2 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS\\relações de confiança**, clique em **confiança do provedor de declarações** ou em relações de confiança de terceira parte **confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.  
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  Na caixa de diálogo **Editar regras de declaração** , selecione uma das seguintes guias, que depende da relação de confiança que você está editando e de qual conjunto de regras você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar o assistente de regra que está associado a esse conjunto de regras :  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **transformar uma declaração de entrada** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra. Em **tipo de declaração de entrada**, selecione um tipo de declaração na lista. Em **tipo de declaração de saída**, selecione um tipo de declaração na lista e, em seguida, selecione uma das seguintes opções, que depende dos requisitos da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada por um valor de declaração de saída diferente**  
  
    -   **Substituir declarações de sufixo de entrada e @ no__t-1mail por um novo sufixo e @ no__t-2mail**  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa declarações AD FS @ no__t-0issued, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e, em **tipo de declaração de entrada**, digite o nome da declaração de entrada ou, se uma descrição de declaração tiver sido criado anteriormente, selecione-o na lista. Em segundo lugar, em **tipo de declaração de saída**, selecione a URL de declaração que você deseja e, em seguida, crie uma regra de transformação na relação de confiança da terceira parte confiável para emitir a declaração do dispositivo.  
>   
> Para obter mais informações sobre cenários de controle de acesso dinâmico, consulte [roteiro de conteúdo de controle de acesso dinâmico](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando declarações de AD DS com AD FS](https://technet.microsoft.com/library/hh831504.aspx).  
  
7. Clique em **Finalizar**.  
  
8. Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
