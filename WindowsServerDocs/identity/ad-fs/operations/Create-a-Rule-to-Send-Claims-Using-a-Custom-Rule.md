---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Criar uma regra para enviar declarações usando uma regra personalizada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8cd3e6d0073061710bd9ee76958891e036688472
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407602"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Criar uma regra para enviar declarações usando uma regra personalizada


Usando a ação **enviar declarações usando um modelo de regra personalizada** no Serviços de Federação do Active Directory (AD FS) (AD FS), você pode criar regras de declaração personalizadas para a situação em que um modelo de regra padrão não atende aos requisitos da sua organização. As regras de declaração personalizada são gravadas no idioma da regra de declaração e, em seguida, devem ser copiadas na caixa de texto **regra personalizada** antes que possam ser usadas em um conjunto de regras. Para obter informações sobre como construir a sintaxe para uma regra avançada, consulte [a função do idioma da regra de declaração](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).  
  
Você pode usar o procedimento a seguir para criar uma regra de declaração usando o snap-in de gerenciamento de AD FS @ no__t-0in.  
  
A associação em **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada em uma relação de confiança de terceira parte confiável no Windows Server 2016 

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra. Em **regra personalizada**, digite ou cole a sintaxe de linguagem da regra de declaração que você deseja para essa regra.  
![Criar regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Clique em **Finalizar**.  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar uma regra para passar ou filtrar uma declaração de entrada em uma confiança do provedor de declarações no Windows Server 2016 
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique\-com o botão direito do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra. Em **regra personalizada**, digite ou cole a sintaxe de linguagem da regra de declaração que você deseja para essa regra.  
![Criar regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Clique em **Finalizar**.  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Para criar uma regra para enviar declarações usando uma declaração personalizada no Windows Server 2012 R2 
  
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
  
5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada** na lista e clique em **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra. Em **regra personalizada**, digite ou cole a sintaxe de linguagem da regra de declaração que você deseja para essa regra.  
![Criar regra](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  Clique em **Finalizar**.  
  
8.  Na caixa de diálogo **Editar regras de declaração** , clique em **OK** para salvar a regra.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
