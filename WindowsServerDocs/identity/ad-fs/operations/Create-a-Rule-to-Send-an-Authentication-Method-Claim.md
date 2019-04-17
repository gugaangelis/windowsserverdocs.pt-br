---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: "Criar uma regra para enviar uma declaração de método de autenticação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Criar uma regra para enviar uma declaração de método de autenticação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Você pode usar o o **enviar a associação de grupo como declarações** modelo de regra ou o **transformar uma entrada reivindicar** modelo de regra para enviar uma declaração de método de autenticação. O terceiro pode usar uma declaração de método de autenticação para determinar o mecanismo de logon que o usuário usa para autenticar e obter requerimentos judiciais ou Extrajudiciais de \(AD FS\) serviços de Federação do Active Directory. Você também pode usar o recurso de garantia do mecanismo de autenticação de serviços de Federação do Active Directory \(AD FS\) no Windows Server 2012 R2 como entrada para gerar as declarações de método de autenticação para situações em que deseja que o terceiro determinar o nível de acesso com base em logons de cartão inteligente. Por exemplo, um desenvolvedor pode atribuir diferentes níveis de acesso aos usuários federados do aplicativo terceira parte. Os níveis de acesso são baseados em se os usuários fazer logon com seus nome e a senha credenciais do usuário, em vez de seus cartões inteligentes.  
  
Dependendo dos requisitos da sua organização, use um dos seguintes procedimentos:  
  
-   Criar essa regra usando o **enviar a associação de grupo como declarações** modelo de regra \-você pode usar esse modelo de regra quando quiser que o grupo que você especifica neste modelo, por fim, determinar qual método de autenticação reivindicar emitir.  
  
-   Criar essa regra usando o **transformar uma declaração de entrada** modelo de regra \-você pode usar esse modelo de regra quando você quiser alterar o método de autenticação existente para um novo método de autenticação que funciona com um produto que não reconhece declarações de método de autenticação do AD FS padrão.  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar usando a associação do grupo Enviar como modelo de regra de declarações em uma dependência terceiros confiar no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **confie dependência de terceiros **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **Editar política de emissão de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo, em **regras de transformação de emissão** clique **Adicionar regra** para iniciar o Assistente de regra. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar a associação de grupo como declaração** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Clique em **procurar**, selecione o grupo cujos membros devem receber esta declaração de método de autenticação e, em seguida, clique em **Okey**.  
  
8.  Em **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Em **valor de solicitação de saída**, digite um dos valores padrão uniform resource identifier \(URI\) na tabela a seguir, dependendo do seu método de autenticação preferencial, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação de nome e a senha do usuário|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) autenticação mútua que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticação baseada em X.509\ que não usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar a regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar usando a associação do grupo Enviar como modelo de regra de declarações em um provedor de declarações confiar no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie **. 
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo, em **regras de transformação de aceitação** clique **Adicionar regra** para iniciar o Assistente de regra.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar a associação de grupo como declaração** na lista e, em seguida, clique **próxima **.  
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Clique em **procurar**, selecione o grupo cujos membros devem receber esta declaração de método de autenticação e, em seguida, clique em **Okey**.  
  
8.  Em **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Em **valor de solicitação de saída**, digite um dos valores padrão uniform resource identifier \(URI\) na tabela a seguir, dependendo do seu método de autenticação preferencial, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação de nome e a senha do usuário|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) autenticação mútua que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticação baseada em X.509\ que não usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar a regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar essa regra usando a transformação uma entrada reivindicar o modelo de regra em uma dependência terceiros confiar no Windows Server 2016 

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
  
7.  Em **tipo de declaração de entrada**, selecione **método de autenticação** na lista.  
  
8.  Em **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Selecione **substituir um valor de declaração de entrada com um valor diferente de declaração de saída**, e depois faça o seguinte:  
  
    1.  Em **valor de solicitação de entrada**, digite um dos seguintes valores URI que se baseiam o método de autenticação real URI que foi usado originalmente, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
    2.  Em **valor de solicitação de saída**, digite um dos valores URI padrão na tabela a seguir, dependendo da sua nova opção de método de autenticação preferencial, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação de nome e a senha do usuário|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticação mútua TLS que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticação baseada em X.509\ que não usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar a regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> Outros valores URI podem ser usados com os valores na tabela. Os valores URI que são mostrados ação tabela anterior refletem os URIs que aceita o terceiro por padrão.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar essa regra usando a transformação uma entrada reivindicar o modelo de regra em um provedor de declarações confiar no Windows Server 2016 
  
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
  
7.  Em **tipo de declaração de entrada**, selecione **método de autenticação** na lista.  
  
8.  Em **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Selecione **substituir um valor de declaração de entrada com um valor diferente de declaração de saída**, e depois faça o seguinte:  
  
    1.  Em **valor de solicitação de entrada**, digite um dos seguintes valores URI que se baseiam o método de autenticação real URI que foi usado originalmente, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
    2.  Em **valor de solicitação de saída**, digite um dos valores URI padrão na tabela a seguir, dependendo da sua nova opção de método de autenticação preferencial, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação de nome e a senha do usuário|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticação mútua TLS que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticação baseada em X.509\ que não usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar a regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Para criar essa regra usando a associação do grupo Enviar como modelo de regra de declarações no Windows Server 2012 R2  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Na árvore de console, em **AD FS\\Trust relações**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** ou **confie dependência de terceiros**e, em seguida, clique em uma relação de confiança específica na lista de onde você deseja criar essa regra.  
  
3.  Clique Right\ a confiança selecionada e clique em **editar regras de declaração **.
![Criar a regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  No **editar regras de declaração** caixa de diálogo, selecione uma guias a seguir, dependendo da relação de confiança que você está editando e defina qual regra você deseja criar essa regra em e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar a regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar a associação de grupo como uma declaração** na lista e, em seguida, clique **próxima**.  
![Criar a regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Clique em **procurar**, selecione o grupo cujos membros devem receber esta declaração de método de autenticação e, em seguida, clique em **Okey**.  
  
8.  Em **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Em **valor de solicitação de saída**, digite um dos valores padrão uniform resource identifier \(URI\) na tabela a seguir, dependendo do seu método de autenticação preferencial, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação de nome e a senha do usuário|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) autenticação mútua que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticação baseada em X.509\ que não usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar a regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> Outros valores URI podem ser usados com os valores na tabela. Os valores URI que são mostrados na tabela anterior refletem os URIs que aceita o terceiro por padrão.  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para criar essa regra usando a transformação um modelo de regra de declaração de entrada no Windows Server 2012 R2  
   
  
  
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
  
7.  Em **tipo de declaração de entrada**, selecione **método de autenticação** na lista.  
  
8.  Em **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Selecione **substituir um valor de declaração de entrada com um valor diferente de declaração de saída**, e depois faça o seguinte:  
  
    1.  Em **valor de solicitação de entrada**, digite um dos seguintes valores URI que se baseiam o método de autenticação real URI que foi usado originalmente, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
    2.  Em **valor de solicitação de saída**, digite um dos valores URI padrão na tabela a seguir, dependendo da sua nova opção de método de autenticação preferencial, clique em **concluir**e clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação de nome e a senha do usuário|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticação mútua TLS que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|Autenticação baseada em X.509\ que não usa TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar a regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> Outros valores URI podem ser usados com os valores na tabela. Os valores URI que são mostrados ação tabela anterior refletem os URIs que aceita o terceiro por padrão.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar as regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criação de regras de declaração para uma terceira confiança de terceiros](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função de reivindicações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
