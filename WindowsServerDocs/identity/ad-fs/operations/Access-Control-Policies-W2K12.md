---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: "Políticas de controle de acesso no AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0b4549192bc4dab9edf3b2a10421325144ed0cd2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Políticas de controle de acesso no Windows Server 2012 R2 e Windows Server FS 2012 AD

>Aplica-se a: Windows Server 2012 R2 e Windows Server 2012 

Verifique as políticas descritas neste artigo usar dois tipos de declarações  
  
1.  Declarações do que AD FS cria com base nas informações de proxy do AD FS e aplicativo Web pode inspecionar e verifique se, por exemplo, o endereço IP do cliente se conectar diretamente a AD FS ou o WAP.  
  
2.  Declarações do AD FS cria com base nas informações encaminhadas ao AD FS pelo cliente, como cabeçalhos HTTP  
  
>**Importante**: as políticas conforme documentado abaixo bloqueará o ingresso em domínio Windows 10 e entrar em cenários que exigem acesso aos seguintes pontos de extremidade adicionais

Pontos de extremidade do FS anúncios necessários para o ingresso no domínio do Windows 10 e entre em
- [nome do serviço de Federação] / adfs/serviços/confiança/2005/windowstransport
- [nome do serviço de Federação] / adfs/serviços/confiança/13/windowstransport
- [nome do serviço de Federação] / adfs/serviços/confiança/2005/usernamemixed
- [nome do serviço de Federação] / adfs/serviços/confiança/13/usernamemixed 
- [nome do serviço de Federação] / adfs/serviços/confiança/2005/certificatemixed
- [nome do serviço de Federação] / adfs/serviços/confiança/13/certificatemixed

Para resolver, atualize quaisquer políticas que negam com base na declaração do ponto de extremidade para permitir que a exceção para os pontos de extremidade acima.

Por exemplo, a regra abaixo:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

Será atualizado para:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Requerimentos judiciais ou Extrajudiciais dessa categoria só devem ser usados para implementar políticas de negócios e não como políticas de segurança para proteger o acesso à rede.  É possível que os clientes não autorizados enviar cabeçalhos com informações falsas como uma maneira de obter acesso.  
  
As políticas descritas neste artigo sempre devem ser usadas com outro método de autenticação, como autenticação de fator de nome de usuário e senha ou multi.  
  
## <a name="client-access-policies-scenarios"></a>Cenários de políticas de acesso do cliente  
  
|**Cenário**|**Descrição**| 
| --- | --- | 
|Cenário 1: Bloquear todos os acessos externo para o Office 365|Office 365 acesso seja permitido de todos os clientes na rede corporativa interna, mas solicitações de clientes externos forem negadas com base no endereço IP do cliente externo.|  
|Cenário 2: Bloquear todos os acessos externo para o Office 365, exceto do Exchange ActiveSync|Office 365 acesso seja permitido de todos os clientes na rede corporativa interna, bem como de quaisquer dispositivos cliente externo, como Smartphones, o que fazer uso do Exchange ActiveSync. Todos os outros clientes externos, como aqueles usando o Outlook, são bloqueados.|  
|Cenário 3: Bloquear todos os acessos externo para o Office 365 exceto aplicativos baseados em navegador|Blocos acesso externo para o Office 365, exceto para aplicativos passivos (baseado em navegador), como o Outlook Web Access ou SharePoint Online.|  
|Cenário 4: Bloquear todos os acessos externo para o Office 365, exceto designados grupos do Active Directory|Esse cenário é usado para testar e validar a implantação de política de acesso do cliente. Ele bloqueia o acesso externo para o Office 365 apenas para membros do grupo no Active Directory uma ou mais. Ele também pode ser usado para oferecer acesso externo apenas a membros de um grupo.|  
  
## <a name="enabling-client-access-policy"></a>Habilitar a política de acesso do cliente  
 Para habilitar a política de acesso do cliente no AD FS no Windows Server 2012 R2, você deve atualizar a plataforma de identidade do Microsoft Office 365 dependência confiança de terceiros. Escolha um dos cenários de exemplo a seguir para configurar as regras de declaração no **plataforma de identidade do Microsoft Office 365** terceira confiança de terceiros que melhor atenda às necessidades da sua organização.  
  
###  <a name="scenario1"></a>Cenário 1: Bloquear todos os acessos externo para o Office 365  
 Esse cenário de política de acesso do cliente permite o acesso de todos os clientes internos e blocos de todos os clientes externos com base no endereço IP do cliente externo. Você pode usar os procedimentos a seguir para adicionar as regras de autorização de emissão corretas para o Office 365 dependência confiança de terceiros para o seu cenário escolhida.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Para criar regras para bloquear todos os acessos externo para o Office 365  
  
1.  De **Gerenciador do servidor**, clique em **ferramentas**, clique em **AD FS gerenciamento**.  
  
2.  Na árvore de console, em **AD FS\Trust relações**, clique em **confie dependência de terceiros**, clique com botão direito do **plataforma de identidade do Microsoft Office 365** confiar e, em seguida, clique em **editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e, em seguida, clique em **próxima**.  
  
5.  No **configurar regra** página, em **nome da regra reivindicação**, tipo, o nome de exibição para essa regra, por exemplo "se houver qualquer reivindicação IP fora do intervalo desejado, negar". Em **regra personalizada**, digite ou cole a seguinte sintaxe reivindicação regra idioma (substitua o valor acima para "x-ms-encaminhado-client-ip" com uma expressão de IP válida):  
`c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Clique em **concluir **. Verifique se a nova regra aparece na lista de regras de autorização de emissão antes para o padrão **permitir acesso a todos os usuários** regra (a regra de negação terá precedência mesmo que ela apareça anterior na lista de).  Se você não tiver o padrão permitir regras de acesso, você pode adicionar um no final da sua lista usando a linguagem de regra de declaração da seguinte maneira:  </br>
    
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Para salvar as novas regras, no **editar regras de declaração** caixa de diálogo, clique em **Okey**. A lista resultante deve se parecer com o seguinte.  
  
     ![Regras de autorização de emissão](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a>Cenário 2: Bloquear todos os acessos externo para o Office 365, exceto do Exchange ActiveSync  
 O exemplo a seguir permite o acesso a todos os aplicativos do Office 365, inclusive Exchange Online, de clientes internos, incluindo Outlook. Ele bloqueia o acesso de clientes localizados fora da rede corporativa, conforme indicado pelo endereço IP do cliente, exceto para os clientes do Exchange ActiveSync, como Smartphones.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Para criar regras para bloquear todos os acessos externo para o Office 365, exceto do Exchange ActiveSync  
  
1.  De **Gerenciador do servidor**, clique em **ferramentas**, clique em **AD FS gerenciamento**.  
  
2.  Na árvore de console, em **AD FS\Trust relações**, clique em **confie dependência de terceiros**, clique com botão direito do **plataforma de identidade do Microsoft Office 365** confiar e, em seguida, clique em **editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e, em seguida, clique em **próxima**.  
  
5.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra, por exemplo "se houver qualquer reivindicação IP fora do intervalo desejado, execute ipoutsiderange declaração". Em **regra personalizada**, digite ou cole a seguinte sintaxe reivindicação regra idioma (substitua o valor acima para "x-ms-encaminhado-client-ip" com uma expressão de IP válida):  
    
    `c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Clique em **concluir **. Verifique se a nova regra aparece no **regras de autorização de emissão** lista.  
  
7.  Em seguida, no **editar regras de declaração** caixa de diálogo, pela **regras de autorização de emissão**, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração novamente.  
  
8.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e, em seguida, clique em **próxima**.  
  
9. No **configurar regra** página, em **nome da regra reivindicação**, tipo, o nome de exibição para essa regra, por exemplo "se houver um IP fora do intervalo desejado e há uma reivindicação de x-ms-aplicativo cliente não EAS, negar". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  

    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. Clique em **concluir **. Verifique se a nova regra aparece no **regras de autorização de emissão** lista.  
  
11. Em seguida, no **editar regras de declaração** caixa de diálogo, pela **regras de autorização de emissão**, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração novamente.  
  
12. No **Selecionar modelo de regra** página, em **modelo de regra de declaração,** selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e clique em **próxima**.  
  
13. No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra, por exemplo "verificar se o aplicativo reivindicação existe". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  
    ```  
    NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. Clique em **concluir **. Verifique se a nova regra aparece no **regras de autorização de emissão** lista.  
  
15. Em seguida, no **editar regras de declaração** caixa de diálogo, pela **regras de autorização de emissão**, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração novamente.  
  
16. No **Selecionar modelo de regra** página, em **modelo de regra de declaração,** selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e clique em **próxima**.  
  
17. No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra, por exemplo "Negar usuários com ipoutsiderange true e aplicativo falharem". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  
`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Clique em **concluir **. Verifique se a nova regra aparece imediatamente abaixo a regra anterior e antes de permitir o acesso a todos os usuários padrão regra na lista de regras de autorização de emissão (a regra de negação terá precedência mesmo que ela apareça anterior na lista de).  </br>Se você não tiver o padrão permitir regras de acesso, você pode adicionar um no final da sua lista usando a linguagem de regra de declaração da seguinte maneira:</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Para salvar as novas regras, no **editar regras de declaração** caixa de diálogo, clique em Okey. A lista resultante deve se parecer com o seguinte.  
  
     ![Regras de autorização de emissão](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a>Cenário 3: Bloquear todos os acessos externo para o Office 365 exceto aplicativos baseados em navegador  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para criar regras para bloquear o acesso externo para o Office 365 exceto aplicativos baseados em navegador  
  
1.  De **Gerenciador do servidor**, clique em **ferramentas**, clique em **AD FS gerenciamento**.  
  
2.  Na árvore de console, em **AD FS\Trust relações**, clique em **confie dependência de terceiros**, clique com botão direito do **plataforma de identidade do Microsoft Office 365** confiar e, em seguida, clique em **editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e, em seguida, clique em **próxima**.  
  
5.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra, por exemplo "se houver qualquer reivindicação IP fora do intervalo desejado, execute ipoutsiderange declaração". Em **regra personalizada**, digite ou cole a seguinte sintaxe reivindicação regra idioma (substitua o valor acima para "x-ms-encaminhado-client-ip" com uma expressão de IP válida):  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Clique em **concluir **. Verifique se a nova regra aparece no **regras de autorização de emissão** lista.  
  
7.  Em seguida, no **editar regras de declaração** caixa de diálogo, pela **regras de autorização de emissão**, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração novamente.  
  
8.  No **Selecionar modelo de regra** página, em **modelo de regra de declaração,** selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e clique em **próxima**.  
  
9. No **configurar regra** página, em **nome da regra reivindicação**, tipo, o nome de exibição para essa regra, por exemplo "se houver um IP fora do intervalo desejado e o ponto de extremidade não é/adfs/ls, negar". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  
 
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. Clique em **concluir **. Verifique se a nova regra aparece na lista de regras de autorização de emissão antes para o padrão **permitir acesso a todos os usuários** regra (a regra de negação terá precedência mesmo que ela apareça anterior na lista de).  </br></br> Se você não tiver o padrão permitir regras de acesso, você pode adicionar um no final da sua lista usando a linguagem de regra de declaração da seguinte maneira:  
  
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. Para salvar as novas regras, no **editar regras de declaração** caixa de diálogo, clique em **Okey**. A lista resultante deve se parecer com o seguinte.  
  
     ![Emissão](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a>Cenário 4: Bloquear todos os acessos externo para o Office 365, exceto designados grupos do Active Directory  
 O exemplo a seguir permite acesso de clientes internos com base no endereço IP. Ele bloqueia o acesso de clientes localizados fora da rede corporativa que possuem um endereço IP do cliente externo, exceto para as pessoas em um Active Directory especificado Group.Use as seguintes etapas para adicionar as regras de autorização de emissão corretas para o **plataforma de identidade do Microsoft Office 365** terceira confiança de terceiros usando o Assistente de regra de declaração:  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Para criar regras para bloquear todos os acessos externo para o Office 365, exceto para designado grupos do Active Directory  
  
1.  De **Gerenciador do servidor**, clique em **ferramentas**, clique em **AD FS gerenciamento**.  
  
2.  Na árvore de console, em **AD FS\Trust relações**, clique em **confie dependência de terceiros**, clique com botão direito do **plataforma de identidade do Microsoft Office 365** confiar e, em seguida, clique em **editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e, em seguida, clique em **próxima**.  
  
5.  No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra, por exemplo "se houver qualquer reivindicação IP fora do intervalo desejado, execute ipoutsiderange reivindicação." Em **regra personalizada**, digite ou cole a seguinte sintaxe reivindicação regra idioma (substitua o valor acima para "x-ms-encaminhado-client-ip" com uma expressão de IP válida):  
  
      
    `c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Clique em **concluir **. Verifique se a nova regra aparece no **regras de autorização de emissão** lista.  
  
7.  Em seguida, no **editar regras de declaração** caixa de diálogo, pela **regras de autorização de emissão**, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração novamente.  
  
8.  No **Selecionar modelo de regra** página, em **modelo de regra de declaração,** selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e clique em **próxima**.  
  
9. No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra, por exemplo "Verificar grupo SID". Em **regra personalizada**, digite ou cole a seguinte sintaxe reivindicação regra idioma (substituir "Get groupsid" com o SID real do grupo de anúncios que você está usando):  
   
    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Clique em **concluir **. Verifique se a nova regra aparece no **regras de autorização de emissão** lista.  
  
11. Em seguida, no **editar regras de declaração** caixa de diálogo, pela **regras de autorização de emissão**, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração novamente.  
  
12. No **Selecionar modelo de regra** página, em **modelo de regra de declaração,** selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e clique em **próxima**.  
  
13. No **configurar regra** página, em **nome da regra reivindicação**, digite o nome de exibição para essa regra, por exemplo "Negar usuários com ipoutsiderange true e Get groupsid falharem". Em **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
   
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Clique em **concluir **. Verifique se a nova regra aparece imediatamente abaixo a regra anterior e antes de permitir o acesso a todos os usuários padrão regra na lista de regras de autorização de emissão (a regra de negação terá precedência mesmo que ela apareça anterior na lista de).  </br></br>Se você não tiver o padrão permitir regras de acesso, você pode adicionar um no final da sua lista usando a linguagem de regra de declaração da seguinte maneira:  
   
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Para salvar as novas regras, no **editar regras de declaração** caixa de diálogo, clique em Okey. A lista resultante deve se parecer com o seguinte.  
  
     ![Emissão](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a>Criando a expressão de intervalo de endereços IP  
 A declaração de x-ms-encaminhado-client-ip é preenchida com um cabeçalho HTTP que atualmente é definido pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da reivindicação pode ser um destes procedimentos:  
  
> [!NOTE]
>  Exchange Online atualmente dá suporte apenas endereços IPV4 e IPV6 não.  
  
-   Um único endereço IP: O endereço IP do cliente que está conectado diretamente ao Exchange Online  
  
> [!NOTE]
>  -   O endereço IP de um cliente na rede corporativa aparecerá como o endereço IP da interface externa do proxy de saída da organização ou gateway.  
> -   Os clientes que estão conectados à rede corporativa por uma VPN ou pela Microsoft DirectAccess (DA) podem aparecer como clientes corporativos internos ou externos clientes dependendo da configuração de VPN ou DA.  
  
-   Um ou mais endereços IP: ao Exchange Online não pode determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor do cabeçalho x-encaminhado-para, um cabeçalho não padrão que pode ser incluído em solicitações de HTTP e é compatível com muitos clientes, balanceadores de carga e proxies do mercado.  
  
> [!NOTE]
>  1.  Vários endereços IP, indicando que o endereço IP do cliente e o endereço de cada proxy que passado a solicitação serão ser separados por vírgula.  
> 2.  Endereços IP relacionada à infraestrutura Exchange Online serão não está na lista.  
  
### <a name="regular-expressions"></a>Expressões regulares  
 Quando você tem que corresponder a um intervalo de endereços IP, é necessário construir uma expressão regular para executar a comparação. Na próxima série de etapas, fornecemos exemplos de como construir uma expressão como essa para coincidir com os seguintes intervalos de endereços (Observe que você precisará alterar esses exemplos para corresponder o intervalo IP público):  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 Primeiro, o padrão básico que corresponderá a um único endereço IP é o seguinte: \b###\\.###\\.###\\.###\b  
  
 Estendendo isso, podemos combinar dois endereços IP com uma expressão ou da seguinte forma: \b###\\.###\\.###\\.###\b & #124;\b###\\.###\\.###\\.###\b  
  
 Portanto, um exemplo para corresponder a apenas dois endereços (como 192.168.1.1 ou 10.0.0.1) seria: \b192\\.168\\.1\\.1\b & #124;\b10\\.0\\.0\\.1\b  
  
 Isso lhe dá a técnica pela qual você pode inserir qualquer número de endereços. Quando um intervalo de endereços precisam ter, por exemplo 192.168.1.1 – 192.168.1.25, a correspondência deve ser feita apenas caractere em caractere: \b192\\.168\\.1\\. ([1-9] & #124; 1 [0-9] & #124; 2 [0-5]) \b  
  
 Observe o seguinte:  
  
-   O endereço IP é tratado como cadeia de caracteres e não é um número.  
  
-   A regra é dividida da seguinte forma: \b192\\.168\\.1\\.  
  
-   Isso corresponde a qualquer 192.168.1 a partir do valor.  
  
-   O seguinte corresponde os intervalos necessários para a parte do endereço após o ponto decimal final:  
  
    -   ([1-9] correspondências endereços terminados em 1-9  
  
    -   & #124; 1 [0-9] corresponde endereços terminando em 10 19  
  
    -   & endereços de correspondências #124;2[0-5]) terminados em 20-25  
  
-   Observe que os parênteses devem ser posicionados corretamente, para que você não iniciar a correspondência com outras partes do endereços IP.  
  
-   Com o bloco 192 correspondido, podemos escrever uma expressão semelhante para o bloco de 10: \b10\\.0\\.0\\. ([1-9] & #124; 1 [0-4]) \b  
  
-   E colocá-los juntos, a seguinte expressão deve corresponder a todos os endereços de "192.168.1.1~25" e "10.0.0.1~14": \b192\\.168\\.1\\. ([1-9] & #124; 1 [0-9] & #124; 2 [0-5]) \b & #124;\b10\\.0\\.0\\. ([1-9] & #124; 1 [0-4]) \b  
  
### <a name="testing-the-expression"></a>Testando a expressão  
 Regex expressões podem ficar bem complicadas, portanto, é altamente recomendável usando uma ferramenta de verificação de regex. Se você fizer uma pesquisa na internet para "construtor de expressão regex online", você encontrará vários utilitários online BOM que permitirá que você experimente suas expressões contra dados de amostra.  
  
 Ao testar a expressão, é importante entender o que esperar para deve ser correspondido. O sistema de online do Exchange pode enviar vários endereços IP, separados por vírgulas. As expressões fornecidas acima funcionará para isso. No entanto, é importante pensar ao testar suas expressões de regex. Por exemplo, um pode usar o exemplo a seguir para verificar os exemplos acima de entrada:  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>Tipos de declaração  
 AD FS no Windows Server 2012 R2 fornece informações de contexto de solicitação usando os seguintes tipos de declaração:  
  
### <a name="x-ms-forwarded-client-ip"></a>X-MS-encaminhado-Client-IP  
 Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 Essa declaração do AD FS representa uma "melhor tentativa" na verificar o endereço IP do usuário (por exemplo, o cliente Outlook) que faz a solicitação. Essa declaração pode conter vários endereços IP, incluindo o endereço de cada proxy que encaminhado a solicitação.  Essa declaração é preenchida com um HTTP. O valor da reivindicação pode ser um destes procedimentos:  
  
-   Um único endereço IP - o endereço IP do cliente que está conectado diretamente ao Exchange Online  
  
> [!NOTE]
>  O endereço IP de um cliente na rede corporativa aparecerá como o endereço IP da interface externa do proxy de saída da organização ou gateway.  
  
-   Um ou mais endereços IP  
  
    -   Se o Exchange Online não conseguir determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor de cabeçalho x-encaminhado-para, um cabeçalho não padrão que pode ser incluído em HTTP com base em solicitações e é compatível com muitos clientes, balanceadores de carga e proxies do mercado.  
  
    -   Vários endereços IP indicando o endereço IP do cliente e o endereço de cada proxy que passado a solicitação serão ser separados por vírgula.  
  
> [!NOTE]
>  Não estará presentes na lista de endereços IP relacionada à infraestrutura Exchange Online.  
  
> [!WARNING]
>  Exchange Online atualmente dá suporte apenas endereços IPV4; ele não oferece suporte a endereços IPV6.  
  
### <a name="x-ms-client-application"></a>X-MS-aplicativo cliente  
 Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 Essa declaração do AD FS representa o protocolo usado pelo cliente final, que corresponde flexível para o aplicativo está sendo usado.  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente apenas definido pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. Dependendo do aplicativo, o valor desta declaração será um destes procedimentos:  
  
-   No caso de dispositivos que usam o Exchange Active Sync, o valor é Microsoft.Exchange.ActiveSync.  
  
-   Uso do cliente Microsoft Outlook pode resultar em qualquer um dos seguintes valores:  
  
    -   Microsoft.Exchange.Autodiscover  
  
    -   Microsoft.Exchange.OfflineAddressBook  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
-   Outros valores possíveis para esse cabeçalho incluem o seguinte:  
  
    -   Microsoft.Exchange.Powershell  
  
    -   Microsoft.Exchange.SMTP  
  
    -   Microsoft.Exchange.Pop  
  
    -   Microsoft.Exchange.Imap  
  
### <a name="x-ms-client-user-agent"></a>X-MS-Client-agente do usuário  
 Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 Essa declaração do AD FS fornece uma cadeia de caracteres para representar o tipo de dispositivo que o cliente está usando para acessar o serviço. Isso pode ser usado quando os clientes gostaria de evitar o acesso para certos dispositivos (por exemplo, determinados tipos de Smartphones).  Valores de exemplo para essa declaração incluem (mas não está limitados a) os valores abaixo.  
  
 A seguir são exemplos de para um cliente cujo x-ms-aplicativo cliente é "Microsoft.Exchange.ActiveSync" pode conter o valor de x-ms-agente do usuário  
  
-   Vortex/1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android/0.3  
  
 Também é possível que esse valor está vazio.  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 Essa declaração do AD FS indica que a solicitação passou por meio do proxy de aplicativo Web.  Essa declaração é preenchida pelo proxy de aplicativo da Web, o que preenche o cabeçalho ao passar a solicitação de autenticação para o serviço de Federação back-end. AD FS converte em uma reivindicação.  
  
 O valor da reivindicação é o nome DNS de proxy o aplicativo Web que passado a solicitação.  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo de solicitação: `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 Tipo de declaração semelhante do x-ms-proxy acima, esse tipo de declaração indica se a solicitação tiver passado por meio do proxy de aplicativo web. Ao contrário de x-ms-proxy, insidecorporatenetwork é um valor booleano com True indicando que uma solicitação diretamente para o serviço de federação de dentro da rede corporativa.  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-ponto de extremidade--caminho absoluto (Active vs passivo)  
 Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 Esse tipo de declaração pode ser usado para determinar as solicitações originadas clientes (avançados) "ativos" versus "passivos" (navegador-clientes baseados na web). Isso permite que solicitações externas de aplicativos baseados em navegador como o Outlook Web Access, SharePoint Online ou o portal do Office 365 para poder enquanto as solicitações originadas clientes sofisticados como o Microsoft Outlook estiverem bloqueadas.  
  
 O valor da reivindicação é o nome do serviço do AD FS que recebeu a solicitação.  
  
## <a name="see-also"></a>Consulte também  
 [Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
