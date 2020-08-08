---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: Criar uma regra para enviar uma declaração do método de autenticação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1ea0c352036a3e7a8070fa9f1f1ddca2a7dff3dd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956653"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>Criar uma regra para enviar uma declaração do método de autenticação


Você pode usar o modelo de regra **Enviar Associação de grupo como declarações** ou o modelo **transformar um** regra de declaração de entrada para enviar uma declaração de método de autenticação. A terceira parte confiável pode usar uma declaração de método de autenticação para determinar o mecanismo de logon que o usuário usa para autenticar e obter declarações de Serviços de Federação do Active Directory (AD FS) \( AD FS \) . Você também pode usar o recurso de garantia de mecanismo de autenticação do Serviços de Federação do Active Directory (AD FS) \( AD FS \) no Windows Server 2012 R2 como entrada para gerar declarações de método de autenticação para situações em que a terceira parte confiável deseja determinar o nível de acesso baseado em logons de cartão inteligente. Por exemplo, um desenvolvedor pode atribuir diferentes níveis de acesso a usuários federados do aplicativo de terceira parte confiável. Os níveis de acesso se baseiam em se os usuários fazem logon com suas credenciais de nome de usuário e senha, em vez de seus cartões inteligentes.

Dependendo dos requisitos da sua organização, use um dos seguintes procedimentos:

-   Crie essa regra usando o modelo **Enviar Associação de grupo como** regra de declarações. \- você pode usar esse modelo de regra quando desejar que o grupo especificado nesse modelo determine, por fim, qual declaração de método de autenticação deve ser emitida.

-   Crie essa regra usando o modelo **transformar uma regra de declaração de entrada** \- você pode usar esse modelo de regra quando desejar alterar o método de autenticação existente para um novo método de autenticação que funcione com um produto que não reconheça as declarações de método de autenticação padrão AD FS.



## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar usando o modelo enviar Associação de grupo como regra de declarações em uma terceira parte confiável no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança**de terceira parte confiável.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar política de emissão de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  Na caixa de diálogo **Editar política de emissão de declaração** , em **regras de transformação de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como declaração** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Clique em **procurar**, selecione o grupo cujos membros devem receber essa declaração de método de autenticação e clique em **OK**.

8.  Em **tipo de declaração de saída**, selecione o **método de autenticação** na lista.

9. Em **valor de declaração de saída**, digite um dos valores padrão de URI do identificador de recurso uniforme \( \) na tabela a seguir, dependendo do seu método de autenticação preferencial, clique em **concluir**e, em seguida, clique em **OK** para salvar a regra.

|                            Método de autenticação real                             |                                URI correspondente                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Autenticação por nome de usuário e senha                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Autenticação do Windows                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| \(Autenticação mútua do TLS de segurança da camada \) de transporte que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  \-Autenticação baseada em X. 509 que não usa TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)

## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar usando a associação de grupo Enviar como modelo de regra de declarações em uma confiança do provedor de declarações no Windows Server 2016

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS**, clique em **relações de confiança do provedor de declarações**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  Na caixa de diálogo **Editar regras de declaração** , em **regras de transformação de aceitação** , clique em **Adicionar regra** para iniciar o assistente de regra.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como declaração** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Clique em **procurar**, selecione o grupo cujos membros devem receber essa declaração de método de autenticação e clique em **OK**.

8.  Em **tipo de declaração de saída**, selecione o **método de autenticação** na lista.

9. Em **valor de declaração de saída**, digite um dos valores padrão de URI do identificador de recurso uniforme \( \) na tabela a seguir, dependendo do seu método de autenticação preferencial, clique em **concluir**e, em seguida, clique em **OK** para salvar a regra.

|                            Método de autenticação real                             |                                URI correspondente                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Autenticação por nome de usuário e senha                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Autenticação do Windows                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| \(Autenticação mútua do TLS de segurança da camada \) de transporte que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  \-Autenticação baseada em X. 509 que não usa TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para criar essa regra usando o modelo transformar uma regra de declaração de entrada em uma relação de confiança de terceira parte confiável no Windows Server 2016

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

7.  Em **tipo de declaração de entrada**, selecione o **método de autenticação** na lista.

8.  Em **tipo de declaração de saída**, selecione o **método de autenticação** na lista.

9. Selecione **substituir um valor de declaração de entrada por um valor de declaração de saída diferente**e, em seguida, faça o seguinte:

    1.  Em **valor de declaração de entrada**, digite um dos seguintes valores de URI que se baseiam no URI do método de autenticação real que foi usado originalmente, clique em **concluir**e em **OK** para salvar a regra.

    2.  Em **valor de declaração de saída**, digite um dos valores de URI padrão na tabela a seguir, que depende da sua nova opção de método de autenticação preferencial, clique em **concluir**e em **OK** para salvar a regra.

|              Método de autenticação real              |                                URI correspondente                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Autenticação por nome de usuário e senha          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Autenticação do Windows                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Autenticação mútua de TLS que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   \-Autenticação baseada em X. 509 que não usa TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)

> [!NOTE]
> Outros valores de URI podem ser usados além dos valores na tabela. Os valores de URI que são mostrados em íon a tabela anterior refletem os URIs que a terceira parte confiável aceita por padrão.

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para criar essa regra usando o modelo transformar uma regra de declaração de entrada em uma confiança do provedor de declarações no Windows Server 2016

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

7.  Em **tipo de declaração de entrada**, selecione o **método de autenticação** na lista.

8.  Em **tipo de declaração de saída**, selecione o **método de autenticação** na lista.

9. Selecione **substituir um valor de declaração de entrada por um valor de declaração de saída diferente**e, em seguida, faça o seguinte:

    1.  Em **valor de declaração de entrada**, digite um dos seguintes valores de URI que se baseiam no URI do método de autenticação real que foi usado originalmente, clique em **concluir**e em **OK** para salvar a regra.

    2.  Em **valor de declaração de saída**, digite um dos valores de URI padrão na tabela a seguir, que depende da sua nova opção de método de autenticação preferencial, clique em **concluir**e em **OK** para salvar a regra.

|              Método de autenticação real              |                                URI correspondente                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Autenticação por nome de usuário e senha          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Autenticação do Windows                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Autenticação mútua de TLS que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   \-Autenticação baseada em X. 509 que não usa TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>Para criar essa regra usando o modelo enviar Associação de grupo como regra de declarações no Windows Server 2012 R2

1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.

2.  Na árvore de console, em **AD FS \\ relações de confiança**, clique em **confiança do provedor de declarações** ou em relações de confiança de terceira parte **confiável**e, em seguida, clique em uma relação de confiança específica na lista em que você deseja criar essa regra.

3.  Clique com o botão direito \- do mouse na relação de confiança selecionada e clique em **Editar regras de declaração**.
![Criar regra](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  Na caixa de diálogo **Editar regras de declaração** , selecione uma das seguintes guias, dependendo da relação de confiança que você está editando e de qual conjunto de regras você deseja criar essa regra e, em seguida, clique em **Adicionar regra** para iniciar o assistente de regra que está associado a esse conjunto de regras:

    -   **Regras de transformação de aceitação**

    -   **Regras de transformação de emissão**

    -   **Regras de Autorização de Emissão**

    -   Regras de autorização de **delegação** 
 ![ Criar regra](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **Enviar Associação de grupo como uma declaração** na lista e clique em **Avançar**.
![Criar regra](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  Na página **Configurar regra** , digite um nome de regra de declaração.

7.  Clique em **procurar**, selecione o grupo cujos membros devem receber essa declaração de método de autenticação e clique em **OK**.

8.  Em **tipo de declaração de saída**, selecione o **método de autenticação** na lista.

9. Em **valor de declaração de saída**, digite um dos valores padrão de URI do identificador de recurso uniforme \( \) na tabela a seguir, dependendo do seu método de autenticação preferencial, clique em **concluir**e, em seguida, clique em **OK** para salvar a regra.

|                            Método de autenticação real                             |                                URI correspondente                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        Autenticação por nome de usuário e senha                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Autenticação do Windows                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| \(Autenticação mútua do TLS de segurança da camada \) de transporte que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  \-Autenticação baseada em X. 509 que não usa TLS                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)

> [!NOTE]
> Outros valores de URI podem ser usados além dos valores na tabela. Os valores de URI que são mostrados na tabela anterior refletem os URIs que a terceira parte confiável aceita por padrão.

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para criar essa regra usando o modelo transformar uma regra de declaração de entrada no Windows Server 2012 R2



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

7.  Em **tipo de declaração de entrada**, selecione o **método de autenticação** na lista.

8.  Em **tipo de declaração de saída**, selecione o **método de autenticação** na lista.

9. Selecione **substituir um valor de declaração de entrada por um valor de declaração de saída diferente**e, em seguida, faça o seguinte:

    1.  Em **valor de declaração de entrada**, digite um dos seguintes valores de URI que se baseiam no URI do método de autenticação real que foi usado originalmente, clique em **concluir**e em **OK** para salvar a regra.

    2.  Em **valor de declaração de saída**, digite um dos valores de URI padrão na tabela a seguir, que depende da sua nova opção de método de autenticação preferencial, clique em **concluir**e em **OK** para salvar a regra.

|              Método de autenticação real              |                                URI correspondente                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         Autenticação por nome de usuário e senha          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Autenticação do Windows                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| Autenticação mútua de TLS que usa certificados X. 509 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   \-Autenticação baseada em X. 509 que não usa TLS    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![Criar regra](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)

> [!NOTE]
> Outros valores de URI podem ser usados além dos valores na tabela. Os valores de URI que são mostrados em íon a tabela anterior refletem os URIs que a terceira parte confiável aceita por padrão.

## <a name="additional-references"></a>Referências adicionais
[Configurar regras de declaração](Configure-Claim-Rules.md)

[Lista de verificação: Como criar regras de declaração para um objeto de confiança de terceira parte confiável](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[Quando usar uma regra de declaração de autorização](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[A função das declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[A função das regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
