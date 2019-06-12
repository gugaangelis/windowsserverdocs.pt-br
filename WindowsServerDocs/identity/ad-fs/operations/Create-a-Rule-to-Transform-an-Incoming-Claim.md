---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Criar uma regra para transformar uma declaração de entrada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a29406880481f0e4e257105e94bc1a33ee661164
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444447"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Criar uma regra para transformar uma declaração de entrada


Usando o **transformar uma declaração de entrada** modelo de regra nos serviços de Federação do Active Directory \(AD FS\), selecione uma declaração de entrada, alterar seu tipo de declaração e altere seu valor de declaração. Por exemplo, você pode usar esse modelo de regra para criar uma regra que envia uma declaração de função com o mesmo valor de declaração de uma declaração de grupo de entrada. Você também pode usar essa regra para enviar a declaração de um grupo com um valor de declaração de compradores quando há uma declaração de grupo de entrada com um valor de administradores, ou você pode enviar somente nome UPN \(UPN\) declarações que terminam com @fabrikam.  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração com o snap de gerenciamento do AD FS\-no.  
  
Associação na **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477). 

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

6.  Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra. Na **tipo de declaração de entrada**, selecione um tipo de declaração na lista. Na **tipo de declaração de saída**, selecione um tipo de declaração na lista e, em seguida, selecione uma das opções a seguir, que depende dos requisitos da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substituir e entrada\-declarações de sufixo com um novo e de email\-sufixo de email**  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.
  
> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa o AD FS\-emissões de declarações, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e na **tipo de declaração de entrada**, digite o nome para a declaração de entrada, ou, se um Descrição de declaração foi criada anteriormente, selecione-o na lista. Segundo, na **tipo de declaração de saída**, selecione a URL de declaração que você deseja e, em seguida, crie uma regra de transformação na terceira parte confiável emita a declaração do dispositivo.  
>   
> Para obter mais informações sobre cenários de controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando declarações de AD DS com o AD FS](https://technet.microsoft.com/library/hh831504.aspx). 

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

6.  Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra. Na **tipo de declaração de entrada**, selecione um tipo de declaração na lista. Na **tipo de declaração de saída**, selecione um tipo de declaração na lista e, em seguida, selecione uma das opções a seguir, que depende dos requisitos da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substituir e entrada\-declarações de sufixo com um novo e de email\-sufixo de email**  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Clique o **concluir** botão.  
  
8.  No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa o AD FS\-emissões de declarações, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e na **tipo de declaração de entrada**, digite o nome para a declaração de entrada, ou, se um Descrição de declaração foi criada anteriormente, selecione-o na lista. Segundo, na **tipo de declaração de saída**, selecione a URL de declaração que você deseja e, em seguida, crie uma regra de transformação na terceira parte confiável emita a declaração do dispositivo.  
>   
> Para obter mais informações sobre cenários de controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando declarações de AD DS com o AD FS](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Para criar uma regra para transformar uma declaração de entrada no Windows Server 2012 R2 
  
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

6.  Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra. Na **tipo de declaração de entrada**, selecione um tipo de declaração na lista. Na **tipo de declaração de saída**, selecione um tipo de declaração na lista e, em seguida, selecione uma das opções a seguir, que depende dos requisitos da sua organização:  
  
    -   **Passar todos os valores de declaração**  
  
    -   **Substituir um valor de declaração de entrada com um valor diferente de declaração de saída**  
  
    -   **Substituir e entrada\-declarações de sufixo com um novo e de email\-sufixo de email**  
![Criar regra](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Se você estiver configurando o cenário de controle de acesso dinâmico que usa o AD FS\-emissões de declarações, primeiro crie uma regra de transformação na relação de confiança do provedor de declarações e na **tipo de declaração de entrada**, digite o nome para a declaração de entrada, ou, se um Descrição de declaração foi criada anteriormente, selecione-o na lista. Segundo, na **tipo de declaração de saída**, selecione a URL de declaração que você deseja e, em seguida, crie uma regra de transformação na terceira parte confiável emita a declaração do dispositivo.  
>   
> Para obter mais informações sobre cenários de controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/dynamic-access-control--scenario-overview.md) ou [usando declarações de AD DS com o AD FS](https://technet.microsoft.com/library/hh831504.aspx).  
  
7. Clique em **concluir**.  
  
8. No **editar regras de declaração** caixa de diálogo, clique em **Okey** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
