---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Políticas de controle de acesso no AD FS no Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.openlocfilehash: b09c9da43d25d7921f7687475c154540ec9d2907
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947315"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Políticas de controle de acesso no Windows Server 2012 R2 e no Windows Server 2012 AD FS


As políticas descritas neste artigo fazem uso de dois tipos de declarações

1.  As declarações AD FS cria com base nas informações que o AD FS e o proxy de aplicativo Web podem inspecionar e verificar, como o endereço IP do cliente que se conecta diretamente ao AD FS ou ao WAP.

2.  Declarações AD FS cria com base nas informações encaminhadas para AD FS pelo cliente como cabeçalhos HTTP

>**Importante**: as políticas, conforme documentado abaixo, bloquearão o ingresso no domínio do Windows 10 e os cenários de logon que exigem acesso aos seguintes pontos de extremidade adicionais

AD FS pontos de extremidade necessários para o ingresso no domínio do Windows 10 e logon
- [nome do serviço de Federação]/ADFS/Services/Trust/2005/windowstransport
- [nome do serviço de Federação]/ADFS/Services/Trust/13/windowstransport
- [nome do serviço de Federação]/ADFS/Services/Trust/2005/usernamemixed
- [nome do serviço de Federação]/ADFS/Services/Trust/13/usernamemixed
- [nome do serviço de Federação]/ADFS/Services/Trust/2005/certificatemixed
- [nome do serviço de Federação]/ADFS/Services/Trust/13/certificatemixed

>**Importante**: os pontos de extremidade/ADFS/Services/Trust/2005/windowstransport e/ADFS/Services/Trust/13/windowstransport só devem ser habilitados para acesso à intranet, pois eles devem ser pontos de extremidade voltados para a intranet que usam a associação WIA em https. Expô-los à extranet podem permitir solicitações nesses pontos de extremidade para ignorar as proteções de bloqueio. Esses pontos de extremidade devem ser desabilitados no proxy (ou seja, desabilitados da extranet) para proteger o bloqueio de conta do AD.

Para resolver, atualize todas as políticas que negam com base na declaração do ponto de extremidade para permitir a exceção para os pontos de extremidade acima.

Por exemplo, a regra abaixo:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`

seria atualizado para:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  As declarações dessa categoria devem ser usadas apenas para implementar políticas de negócios e não como políticas de segurança para proteger o acesso à sua rede.  É possível que clientes não autorizados enviem cabeçalhos com informações falsas como uma maneira de obter acesso.

As políticas descritas neste artigo sempre devem ser usadas com outro método de autenticação, como nome de usuário e senha ou autenticação multifator.

## <a name="client-access-policies-scenarios"></a>Cenários de políticas de acesso de cliente

|**Cenário**|**Descrição**|
| --- | --- |
|Cenário 1: bloquear todo o acesso externo ao Office 365|O acesso ao Office 365 é permitido de todos os clientes na rede corporativa interna, mas as solicitações de clientes externos são negadas com base no endereço IP do cliente externo.|
|Cenário 2: bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync|O acesso ao Office 365 é permitido de todos os clientes na rede corporativa interna, bem como de quaisquer dispositivos cliente externos, como smartphones, que usam o Exchange ActiveSync. Todos os outros clientes externos, como aqueles que usam o Outlook, são bloqueados.|
|Cenário 3: bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador|Bloqueia o acesso externo ao Office 365, exceto para aplicativos passivos (baseados em navegador), como o Outlook Acesso via Web ou o SharePoint Online.|
|Cenário 4: bloquear todo o acesso externo ao Office 365, exceto para grupos de Active Directory designados|Esse cenário é usado para testar e validar a implantação da política de acesso do cliente. Ele bloqueia o acesso externo ao Office 365 somente para membros de um ou mais grupos de Active Directory. Ele também pode ser usado para fornecer acesso externo somente aos membros de um grupo.|

## <a name="enabling-client-access-policy"></a>Habilitando a política de acesso do cliente
 Para habilitar a política de acesso do cliente no AD FS no Windows Server 2012 R2, você deve atualizar a terceira parte confiável da plataforma de identidade Microsoft Office 365. Escolha um dos cenários de exemplo abaixo para configurar as regras de declaração na relação de confiança de terceira parte confiável da **plataforma de identidade Microsoft Office 365** que melhor atende às necessidades da sua organização.

###  <a name="scenario-1-block-all-external-access-to-office-365"></a><a name="scenario1"></a>Cenário 1: bloquear todo o acesso externo ao Office 365
 Esse cenário de política de acesso de cliente permite o acesso de todos os clientes internos e bloqueia todos os clientes externos com base no endereço IP do cliente externo. Você pode usar os procedimentos a seguir para adicionar as regras de autorização de emissão corretas à relação de confiança de terceira parte confiável do Office 365 para o cenário escolhido.

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Para criar regras para bloquear todo o acesso externo ao Office 365

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de AD FS**.

2.  Na árvore de console, em **relações do AD FS\Trust**, clique em relações de confiança de terceira parte **confiável**, clique com o botão direito do mouse na relação de confiança da **plataforma de identidade Microsoft Office 365** e clique em **Editar regras de declaração**.

3.  Na caixa de diálogo **Editar regras de declaração** , selecione a guia **regras de autorização de emissão** e clique em **Adicionar regra** para iniciar o assistente de regra de declaração.

4.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

5.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo, "se houver alguma declaração de IP fora do intervalo desejado, negar". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração (substitua o valor acima por "x-MS-Forwarded-Client-IP" por uma expressão de IP válida):`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Clique em **Concluir**. Verifique se a nova regra aparece na lista regras de autorização de emissão antes da regra padrão **permitir acesso a todos os usuários** (a regra de negação terá precedência, mesmo que apareça anteriormente na lista).  Se você não tiver a regra de acesso de permissão padrão, poderá adicionar uma no final da lista usando o idioma da regra de declaração da seguinte maneira:  </br>

    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); `

7.  Para salvar as novas regras, na caixa de diálogo **Editar regras de declaração** , clique em **OK**. A lista resultante deve ser parecida com a seguinte.

     ![Regras de Autorização de Emissão](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")

###  <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a><a name="scenario2"></a>Cenário 2: bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync
 O exemplo a seguir permite o acesso a todos os aplicativos do Office 365, incluindo o Exchange Online, de clientes internos, incluindo o Outlook. Ele bloqueia o acesso de clientes que residem fora da rede corporativa, conforme indicado pelo endereço IP do cliente, exceto para clientes do Exchange ActiveSync, como Smart Phones.

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Para criar regras para bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de AD FS**.

2.  Na árvore de console, em **relações do AD FS\Trust**, clique em relações de confiança de terceira parte **confiável**, clique com o botão direito do mouse na relação de confiança da **plataforma de identidade Microsoft Office 365** e clique em **Editar regras de declaração**.

3.  Na caixa de diálogo **Editar regras de declaração** , selecione a guia **regras de autorização de emissão** e clique em **Adicionar regra** para iniciar o assistente de regra de declaração.

4.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

5.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo, "se houver alguma declaração de IP fora do intervalo desejado, emita a declaração ipoutsiderange". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração (substitua o valor acima por "x-MS-Forwarded-Client-IP" por uma expressão de IP válida):

    `c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`

6.  Clique em **Concluir**. Verifique se a nova regra aparece na lista **regras de autorização de emissão** .

7.  Em seguida, na caixa de diálogo **Editar regras de declaração** , na guia **regras de autorização de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra de declaração novamente.

8.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

9. Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra, por exemplo, "se houver um IP fora do intervalo desejado e houver uma declaração não EAS x-MS-Client-Application, negar". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:

    ```
    c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");
    ```

10. Clique em **Concluir**. Verifique se a nova regra aparece na lista **regras de autorização de emissão** .

11. Em seguida, na caixa de diálogo **Editar regras de declaração** , na guia **regras de autorização de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra de declaração novamente.

12. Na página **selecionar modelo de regra** , em **modelo de regra de declaração,** selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

13. Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo, "verificar se a declaração do aplicativo existe". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:

    ```
    NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");
    ```

14. Clique em **Concluir**. Verifique se a nova regra aparece na lista **regras de autorização de emissão** .

15. Em seguida, na caixa de diálogo **Editar regras de declaração** , na guia **regras de autorização de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra de declaração novamente.

16. Na página **selecionar modelo de regra** , em **modelo de regra de declaração,** selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

17. Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "negar usuários com ipoutsiderange verdadeiro e falha do aplicativo". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:

    ```
    c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");
    ```

18.  Clique em **Concluir**. Verifique se a nova regra aparece imediatamente abaixo da regra anterior e antes da regra padrão permitir acesso a todos os usuários na lista regras de autorização de emissão (a regra de negação terá precedência, embora apareça anteriormente na lista).<p>Se você não tiver a regra de acesso de permissão padrão, poderá adicionar uma no final da lista usando o idioma da regra de declaração da seguinte maneira:

        ```
        c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");
        ```

1.  Para salvar as novas regras, na caixa de diálogo **Editar regras de declaração** , clique em OK. A lista resultante deve ser parecida com a seguinte.

    ![Regras de Autorização de Emissão](media/Access-Control-Policies-W2K12/clientaccess2.png )

###  <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a><a name="scenario3"></a>Cenário 3: bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para criar regras para bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de AD FS**.

2.  Na árvore de console, em **relações do AD FS\Trust**, clique em relações de confiança de terceira parte **confiável**, clique com o botão direito do mouse na relação de confiança da **plataforma de identidade Microsoft Office 365** e clique em **Editar regras de declaração**.

3.  Na caixa de diálogo **Editar regras de declaração** , selecione a guia **regras de autorização de emissão** e clique em **Adicionar regra** para iniciar o assistente de regra de declaração.

4.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

5.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo, "se houver alguma declaração de IP fora do intervalo desejado, emita a declaração ipoutsiderange". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração (substitua o valor acima por "x-MS-Forwarded-Client-IP" por uma expressão de IP válida):

   ```
   c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");
   ```

1.  Clique em **Concluir**. Verifique se a nova regra aparece na lista **regras de autorização de emissão** .

2.  Em seguida, na caixa de diálogo **Editar regras de declaração** , na guia **regras de autorização de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra de declaração novamente.

3.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração,** selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

4. Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo, "se houver um IP fora do intervalo desejado e o ponto de extremidade não for/adfs/ls, negar". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:

    ```
    c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`
    ```

10. Clique em **Concluir**. Verifique se a nova regra aparece na lista regras de autorização de emissão antes da regra padrão **permitir acesso a todos os usuários** (a regra de negação terá precedência, mesmo que apareça anteriormente na lista).  </br></br> Se você não tiver a regra de acesso de permissão padrão, poderá adicionar uma no final da lista usando o idioma da regra de declaração da seguinte maneira:

   `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. Para salvar as novas regras, na caixa de diálogo **Editar regras de declaração** , clique em **OK**. A lista resultante deve ser parecida com a seguinte.

    ![Emissão](media/Access-Control-Policies-W2K12/clientaccess3.png)

###  <a name="scenario-4-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a><a name="scenario4"></a>Cenário 4: bloquear todo o acesso externo ao Office 365, exceto para grupos de Active Directory designados
 O exemplo a seguir habilita o acesso de clientes internos com base no endereço IP. Ele bloqueia o acesso de clientes que residem fora da rede corporativa que têm um endereço IP de cliente externo, exceto aqueles indivíduos em um grupo de Active Directory especificado. Use as etapas a seguir para adicionar as regras de autorização de emissão corretas para a terceira parte confiável da **plataforma de identidade Microsoft Office 365** usando o assistente de regra de declaração:

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Para criar regras para bloquear todo o acesso externo ao Office 365, exceto para grupos de Active Directory designados

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de AD FS**.

2.  Na árvore de console, em **relações do AD FS\Trust**, clique em relações de confiança de terceira parte **confiável**, clique com o botão direito do mouse na relação de confiança da **plataforma de identidade Microsoft Office 365** e clique em **Editar regras de declaração**.

3.  Na caixa de diálogo **Editar regras de declaração** , selecione a guia **regras de autorização de emissão** e clique em **Adicionar regra** para iniciar o assistente de regra de declaração.

4.  Na página **selecionar modelo de regra** , em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

5.  Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo, "se houver alguma declaração de IP fora do intervalo desejado, emita a declaração ipoutsiderange." Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração (substitua o valor acima por "x-MS-Forwarded-Client-IP" por uma expressão de IP válida):

    ```
    `c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`
    ```

6. Clique em **Concluir**. Verifique se a nova regra aparece na lista **regras de autorização de emissão** .

7. Em seguida, na caixa de diálogo **Editar regras de declaração** , na guia **regras de autorização de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra de declaração novamente.

8. Na página **selecionar modelo de regra** , em **modelo de regra de declaração,** selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

9. Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra, por exemplo, "verificar SID do grupo". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração (substitua "GroupId" pelo Sid real do grupo do AD que você está usando):

    ```
    NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");
    ```

10. Clique em **Concluir**. Verifique se a nova regra aparece na lista **regras de autorização de emissão** .

11. Em seguida, na caixa de diálogo **Editar regras de declaração** , na guia **regras de autorização de emissão** , clique em **Adicionar regra** para iniciar o assistente de regra de declaração novamente.

12. Na página **selecionar modelo de regra** , em **modelo de regra de declaração,** selecione **enviar declarações usando uma regra personalizada**e clique em **Avançar**.

13. Na página **Configurar regra** , em **nome da regra de declaração**, digite o nome para exibição dessa regra, por exemplo "negar usuários com ipoutsiderange true e GroupId falha". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:

   ```
   c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");
   ```

14. Clique em **Concluir**. Verifique se a nova regra aparece imediatamente abaixo da regra anterior e antes da regra padrão permitir acesso a todos os usuários na lista regras de autorização de emissão (a regra de negação terá precedência, embora apareça anteriormente na lista).  </br></br>Se você não tiver a regra de acesso de permissão padrão, poderá adicionar uma no final da lista usando o idioma da regra de declaração da seguinte maneira:

   ```
   c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");
   ```

15. Para salvar as novas regras, na caixa de diálogo **Editar regras de declaração** , clique em OK. A lista resultante deve ser parecida com a seguinte.

     ![Emissão](media/Access-Control-Policies-W2K12/clientaccess4.png)

##  <a name="building-the-ip-address-range-expression"></a><a name="buildingip"></a>Criando a expressão de intervalo de endereços IP
 A declaração x-MS-encaminhar-Client-IP é populada a partir de um cabeçalho HTTP atualmente definido somente pelo Exchange Online, que popula o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da declaração pode ser um dos seguintes:

> [!NOTE]
> Atualmente, o Exchange Online dá suporte apenas a endereços IPV4 e não IPV6.

- Um único endereço IP: o endereço IP do cliente que está conectado diretamente ao Exchange Online

> [!NOTE]
> - O endereço IP de um cliente na rede corporativa será exibido como o endereço IP da interface externa do proxy ou gateway de saída da organização.
> - Os clientes que estão conectados à rede corporativa por uma VPN ou pelo Microsoft DirectAccess (DA) podem aparecer como clientes corporativos internos ou como clientes externos, dependendo da configuração da VPN ou DA.

- Um ou mais endereços IP: quando o Exchange Online não pode determinar o endereço IP do cliente que está se conectando, ele definirá o valor com base no valor do cabeçalho x-Forwarded-for, um cabeçalho não padrão que pode ser incluído em solicitações baseadas em HTTP e tem suporte de vários clientes, balanceadores de carga e proxies no mercado.

> [!NOTE]
> 1. Vários endereços IP, indicando o endereço IP do cliente e o endereço de cada proxy que passou na solicitação, serão separados por uma vírgula.
> 2. Os endereços IP relacionados à infraestrutura do Exchange Online não serão na lista.

### <a name="regular-expressions"></a>Expressões regulares
 Quando você precisa corresponder a um intervalo de endereços IP, é necessário construir uma expressão regular para executar a comparação. Na próxima série de etapas, forneceremos exemplos de como construir essa expressão para corresponder aos intervalos de endereços a seguir (Observe que você precisará alterar esses exemplos para corresponder ao seu intervalo de IP público):

- 192.168.1.1 – 192.168.1.25

- 10.0.0.1 – 10.0.0.14

  Primeiro, o padrão básico que corresponderá a um único endereço IP é o seguinte: \b # # # \\ . # # # \\ . # # # \\ . # # # \b

  Estendendo isso, podemos corresponder dois endereços IP diferentes com uma expressão or da seguinte maneira: \b # # # \\ . # # # \\ . # # # \\ . # # # \b&#124; \b # # # \\ . # # #. # # \\ # \\ . # # #

  Portanto, um exemplo para corresponder apenas a dois endereços (como 192.168.1.1 ou 10.0.0.1) seria: \b192 \\ . 168 \\ 0,1 \\ 0,1 \ b&#124; \b10 0. \\ \\ 0 \\ . 1 \ b

  Isso oferece a técnica pela qual você pode inserir qualquer número de endereços. Onde um intervalo de endereços precisa ser permitido, por exemplo, 192.168.1.1 – 192.168.1.25, a correspondência deve ser feita caractere por caractere: \b192 \\ . 168 \\ . 1 \\ . ( [1-9] &#124;1 [0-9] &#124;2 [0-5]) \b

  Observe o seguinte:

- O endereço IP é tratado como cadeia de caracteres e não como um número.

- A regra é dividida da seguinte maneira: \b192 \\ . 168 \\ . 1 \\ .

- Isso corresponde a qualquer valor que comece com 192.168.1.

- O seguinte corresponde aos intervalos necessários para a parte do endereço após o último ponto decimal:

  - ([1-9] corresponde a endereços que terminam em 1-9

  - &#124;1 [0-9] corresponde a endereços que terminam em 10-19

  - &#124;2 [0-5]) corresponde aos endereços que terminam em 20-25

- Observe que os parênteses devem ser posicionados corretamente, para que você não comece a corresponder a outras partes de endereços IP.

- Com o bloco 192 correspondido, podemos gravar uma expressão semelhante para o 10 bloco: \b10 \\ 0 \\ \\ . ( [1-9] &#124;1 [0-4]) \b

- E colocando-os juntos, a expressão a seguir deve corresponder a todos os endereços de "192.168.1.1 ~ 25" e "10.0.0.1 ~ 14": \b192 \\ . 168 \\ . 1 \\ . ( [1-9] &#124;1 [0-9] &#124;2 [0-5]) \b&#124; \b10 \\ 0 \\ . 0 \\ . ( [1-9] &#124;1 [0-4]) \b

### <a name="testing-the-expression"></a>Testando a expressão
 As expressões Regex podem se tornar muito complicadas, portanto, é altamente recomendável usar uma ferramenta de verificação Regex. Se você fizer uma pesquisa na Internet por "Construtor de expressões de Regex online", encontrará vários bons utilitários online que permitirão experimentar suas expressões em relação aos dados de exemplo.

 Ao testar a expressão, é importante que você entenda o que esperar deve corresponder. O sistema Exchange Online pode enviar vários endereços IP, separados por vírgulas. As expressões fornecidas acima funcionarão para isso. No entanto, é importante pensar nisso ao testar suas expressões Regex. Por exemplo, uma delas pode usar a seguinte entrada de exemplo para verificar os exemplos acima:

 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20

 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1

## <a name="claim-types"></a>Tipos de declaração
 AD FS no Windows Server 2012 R2 fornece informações de contexto de solicitação usando os seguintes tipos de declaração:

### <a name="x-ms-forwarded-client-ip"></a>X-MS-encaminhar-Client-IP
 Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

 Essa declaração de AD FS representa uma "melhor tentativa" ao garantir o endereço IP do usuário (por exemplo, o cliente Outlook) fazendo a solicitação. Essa declaração pode conter vários endereços IP, incluindo o endereço de cada proxy que encaminhou a solicitação.  Essa declaração é preenchida a partir de um HTTP. O valor da declaração pode ser um dos seguintes:

- Um único endereço IP-o endereço IP do cliente que está conectado diretamente ao Exchange Online

> [!NOTE]
> O endereço IP de um cliente na rede corporativa será exibido como o endereço IP da interface externa do proxy ou gateway de saída da organização.

- Um ou mais endereços IP

  - Se o Exchange Online não puder determinar o endereço IP do cliente que está se conectando, ele definirá o valor com base no valor do cabeçalho x-Forwarded-for, um cabeçalho não padrão que pode ser incluído em solicitações baseadas em HTTP e tem suporte de vários clientes, balanceadores de carga e proxies no mercado.

  - Vários endereços IP que indicam o endereço IP do cliente e o endereço de cada proxy que passou na solicitação serão separados por uma vírgula.

> [!NOTE]
> Os endereços IP relacionados à infraestrutura do Exchange Online não estarão presentes na lista.

> [!WARNING]
> Atualmente, o Exchange Online dá suporte apenas a endereços IPV4; Ele não dá suporte a endereços IPV6.

### <a name="x-ms-client-application"></a>X-MS-cliente-aplicativo
 Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

 Essa declaração de AD FS representa o protocolo usado pelo cliente final, que corresponde livremente ao aplicativo que está sendo usado.  Essa declaração é populada a partir de um cabeçalho HTTP que atualmente só é definido pelo Exchange Online, que popula o cabeçalho ao passar a solicitação de autenticação para AD FS. Dependendo do aplicativo, o valor dessa declaração será um dos seguintes:

- No caso de dispositivos que usam Exchange Active Sync, o valor é Microsoft. Exchange. ActiveSync.

- O uso do cliente do Microsoft Outlook pode resultar em qualquer um dos seguintes valores:

    - Microsoft. Exchange. autodiscover

    - Microsoft. Exchange. OfflineAddressBook

    - Microsoft. Exchange. RPCMicrosoft. Exchange. WebServices

    - Microsoft. Exchange. RPCMicrosoft. Exchange. WebServices

- Outros valores possíveis para esse cabeçalho incluem o seguinte:

    - Microsoft. Exchange. PowerShell

    - Microsoft. Exchange. SMTP

    - Microsoft. Exchange. pop

    - Microsoft. Exchange. IMAP

### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent
 Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

 Essa declaração de AD FS fornece uma cadeia de caracteres para representar o tipo de dispositivo que o cliente está usando para acessar o serviço. Isso pode ser usado quando os clientes desejarem impedir o acesso a determinados dispositivos (como tipos específicos de Smart Phone).  Os valores de exemplo para essa declaração incluem (mas não estão limitados a) os valores abaixo.

 Veja a seguir exemplos de o que o valor x-MS-User-Agent pode conter para um cliente cujo x-MS-Client-Application seja "Microsoft. Exchange. ActiveSync"

- Vortex/1.0

- Apple-iPad1C1/812.1

- Apple-iPhone3C1/811.2

- Apple-iPhone/704.11

- Moto-DROID2/4.5.1

- SAMSUNGSPHD700/100.202

- Android/0.3

  Também é possível que esse valor esteja vazio.

### <a name="x-ms-proxy"></a>X-MS-proxy
 Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

 Essa declaração de AD FS indica que a solicitação passou pelo proxy de aplicativo Web.  Essa declaração é preenchida pelo proxy de aplicativo Web, que popula o cabeçalho ao passar a solicitação de autenticação para o back-end Serviço de Federação. AD FS, em seguida, converte-o em uma declaração.

 O valor da declaração é o nome DNS do proxy de aplicativo Web que passou na solicitação.

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork
 Tipo de declaração:`https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`

 Semelhante ao tipo de declaração x-MS-proxy acima, esse tipo de declaração indica se a solicitação passou pelo proxy de aplicativo Web. Ao contrário de x-MS-proxy, insidecorporatenetwork é um valor booliano com true indicando uma solicitação diretamente para o serviço de Federação de dentro da rede corporativa.

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (ativo vs passivo)
 Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

 Esse tipo de declaração pode ser usado para determinar solicitações originadas de clientes "ativos" (ricos) versus clientes "passivos" (baseados em navegador da Web). Isso permite que solicitações externas de aplicativos baseados em navegador, como o Outlook Acesso via Web, o SharePoint Online ou o portal do Office 365, sejam permitidas enquanto as solicitações originadas de clientes avançados, como o Microsoft Outlook, são bloqueadas.

 O valor da declaração é o nome do serviço de AD FS que recebeu a solicitação.

## <a name="see-also"></a>Consulte Também
 [Operações do AD FS](../ad-fs-operations.md)
