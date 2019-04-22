---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: Criar uma regra para enviar uma declaração do método de autenticação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819087"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Criar uma regra para enviar uma declaração do método de autenticação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Você pode usar o **enviar associação de grupo como declarações** modelo de regra ou o **transformar uma declaração de entrada** modelo de regra para enviar uma declaração de método de autenticação. A terceira parte confiável pode usar uma declaração de método de autenticação para determinar o mecanismo de logon que o usuário usa para autenticar e obter as declarações do Active Directory Federation Services \(do AD FS\). Você também pode usar o recurso de garantia do mecanismo de autenticação dos serviços de Federação do Active Directory \(do AD FS\) no Windows Server 2012 R2 como entrada para gerar declarações de método de autenticação para situações em que a terceira parte confiável quer determinar o nível de acesso com base em logons de cartão inteligente. Por exemplo, um desenvolvedor pode atribuir diferentes níveis de acesso para usuários federados do aplicativo de terceira parte confiável. Os níveis de acesso baseiam-se em se os usuários fizerem logon com seus nome e senha de credenciais de usuário, em vez de seus cartões inteligentes.  
  
Dependendo dos requisitos da sua organização, use um dos procedimentos a seguir:  
  
-   Crie essa regra usando o **enviar associação de grupo como declarações** modelo de regra \- você pode usar esse modelo de regra quando desejar que o grupo que você especifica, por fim, nesse modelo para determinar qual método de autenticação declaração para emitir.  
  
-   Crie essa regra usando o **transformar uma declaração de entrada** modelo de regra \- você pode usar esse modelo de regra quando você deseja alterar o método de autenticação existente para um novo método de autenticação que funciona com um produto que não reconhece declarações de método de autenticação do AD FS padrão.  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar, usando a associação de grupo Enviar como modelo de regra de declarações em uma terceira parte confiável no Windows Server 2016 

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **terceira**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  No **Editar política de emissão de declaração** caixa de diálogo **regras de transformação de emissão** clique em **Adicionar regra** para iniciar o Assistente de regra. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **Send Group Membership como declaração** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Clique em **navegue**, selecione o grupo cujos membros devem receber essa declaração de método de autenticação e, em seguida, clique em **Okey**.  
  
8.  Na **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Na **valor de declaração de saída**, digite um do padrão uniform resource identifier \(URI\) valores na tabela a seguir, dependendo de seu método de autenticação preferido, clique em **concluir**e, em seguida, clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação por nome de usuário e senha|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) autenticação mútua que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-com base em autenticação que não usa o TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar, usando a associação de grupo Enviar como modelo de regra de declarações em um provedor de declarações confiável no Windows Server 2016 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS**, clique em **confianças do provedor de declarações**. 
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  No **editar regras de declaração** caixa de diálogo **regras de transformação de aceitação** clique em **Adicionar regra** para iniciar o Assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **Send Group Membership como declaração** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Clique em **navegue**, selecione o grupo cujos membros devem receber essa declaração de método de autenticação e, em seguida, clique em **Okey**.  
  
8.  Na **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Na **valor de declaração de saída**, digite um do padrão uniform resource identifier \(URI\) valores na tabela a seguir, dependendo de seu método de autenticação preferido, clique em **concluir**e, em seguida, clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação por nome de usuário e senha|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) autenticação mútua que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-com base em autenticação que não usa o TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar essa regra usando a transformação de uma declaração de entrada modelo de regra em uma terceira parte confiável no Windows Server 2016 

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
  
7.  Na **tipo de declaração de entrada**, selecione **método de autenticação** na lista.  
  
8.  Na **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Selecione **substituir um valor de declaração de entrada com um valor diferente de declaração de saída**, e, em seguida, faça o seguinte:  
  
    1.  Na **valor de declaração de entrada**, digite um dos seguintes valores URI com base no método de autenticação real URI que foi usado originalmente, clique em **concluir**e, em seguida, clique em **Okey** para salvar a regra.  
  
    2.  Na **valor de declaração de saída**, digite um dos valores de URI padrão na tabela a seguir, que depende de sua escolha de método de autenticação preferido novo, clique em **concluir**e, em seguida, clique em **Okey**  para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação por nome de usuário e senha|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticação mútua TLS que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-com base em autenticação que não usa o TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> Outros valores URI podem ser usados além dos valores na tabela. Os valores URI que são mostrados ion tabela anterior refletem os URIs que a terceira aceita por padrão.  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar essa regra usando a transformação de uma declaração de entrada modelo de regra em um provedor de declarações confiável no Windows Server 2016 
  
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
  
7.  Na **tipo de declaração de entrada**, selecione **método de autenticação** na lista.  
  
8.  Na **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Selecione **substituir um valor de declaração de entrada com um valor diferente de declaração de saída**, e, em seguida, faça o seguinte:  
  
    1.  Na **valor de declaração de entrada**, digite um dos seguintes valores URI com base no método de autenticação real URI que foi usado originalmente, clique em **concluir**e, em seguida, clique em **Okey** para salvar a regra.  
  
    2.  Na **valor de declaração de saída**, digite um dos valores de URI padrão na tabela a seguir, que depende de sua escolha de método de autenticação preferido novo, clique em **concluir**e, em seguida, clique em **Okey**  para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação por nome de usuário e senha|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticação mútua TLS que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-com base em autenticação que não usa o TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Para criar essa regra usando a associação de grupo Enviar como modelo de regra de declarações no Windows Server 2012 R2  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **do AD FS\\relações de confiança**, clique em **confianças do provedor de declarações** ou **terceira**e, em seguida, clique em um determinado confiar na lista de onde você deseja criar essa regra.  
  
3.  À direita\-clique a relação de confiança selecionada e, em seguida, clique em **editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  No **editar regras de declaração** caixa de diálogo, selecione um guias a seguir, dependendo da relação de confiança que você está editando e qual regra definida que você deseja criar essa regra no e, em seguida, clique em **Adicionar regra** para iniciar a regra Assistente que está associado esse conjunto de regras:  
  
    -   **Regras de transformação de aceitação**  
  
    -   **Regras de transformação de emissão**  
  
    -   **Regras de autorização de emissão**  
  
    -   **Regras de autorização de delegação**  
![Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar associação de grupo como uma declaração** na lista e, em seguida, clique **Avançar**.  
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  Sobre o **configurar regra** página, digite um nome de regra de declaração.  
  
7.  Clique em **navegue**, selecione o grupo cujos membros devem receber essa declaração de método de autenticação e, em seguida, clique em **Okey**.  
  
8.  Na **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Na **valor de declaração de saída**, digite um do padrão uniform resource identifier \(URI\) valores na tabela a seguir, dependendo de seu método de autenticação preferido, clique em **concluir**e, em seguida, clique em **Okey** para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação por nome de usuário e senha|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Transport Layer Security \(TLS\) autenticação mútua que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-com base em autenticação que não usa o TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> Outros valores URI podem ser usados além dos valores na tabela. Os valores URI que são mostrados na tabela anterior refletem os URIs que a terceira aceita por padrão.  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para criar essa regra usando a transformação de um modelo de regra de declaração de entrada no Windows Server 2012 R2  
   
  
  
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
  
7.  Na **tipo de declaração de entrada**, selecione **método de autenticação** na lista.  
  
8.  Na **tipo de declaração de saída**, selecione **método de autenticação** na lista.  
  
9. Selecione **substituir um valor de declaração de entrada com um valor diferente de declaração de saída**, e, em seguida, faça o seguinte:  
  
    1.  Na **valor de declaração de entrada**, digite um dos seguintes valores URI com base no método de autenticação real URI que foi usado originalmente, clique em **concluir**e, em seguida, clique em **Okey** para salvar a regra.  
  
    2.  Na **valor de declaração de saída**, digite um dos valores de URI padrão na tabela a seguir, que depende de sua escolha de método de autenticação preferido novo, clique em **concluir**e, em seguida, clique em **Okey**  para salvar a regra.  
  
|Método de autenticação real|URI correspondente|  
|--------------------------------|---------------------|  
|Autenticação por nome de usuário e senha|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Autenticação do Windows|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|Autenticação mútua TLS que usa certificados x. 509|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X. 509\-com base em autenticação que não usa o TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> Outros valores URI podem ser usados além dos valores na tabela. Os valores URI que são mostrados ion tabela anterior refletem os URIs que a terceira aceita por padrão.  

## <a name="additional-references"></a>Referências adicionais 
[Configurar regras de declaração](Configure-Claim-Rules.md)  
 
[Lista de verificação: Criando regras de declaração para a Relying Party Trust](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de verificação: Criando regras de declaração para um provedor de declarações de confiança](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[A função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
