---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Políticas de controle de acesso no AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6e56c95bb3284615d8cc9487e70ca32abbb0f22b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855557"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Políticas de controle de acesso no Windows Server 2012 R2 e Windows Server 2012 do AD FS

>Aplica-se a: Windows Server 2012 R2 e Windows Server 2012 

Verifique as políticas descritas neste artigo usam dois tipos de declarações  
  
1.  Declarações que do AD FS cria com base nas informações de proxy do AD FS e o aplicativo Web pode inspecionar e verificar como o endereço IP do cliente que se conecta diretamente ao AD FS ou o WAP.  
  
2.  Declarações do AD FS cria com base nas informações encaminhadas para o AD FS pelo cliente, como cabeçalhos HTTP  
  
>**Importante**: As políticas, como documentado abaixo bloqueará o ingresso no domínio Windows 10 e entrar em cenários que exigem acesso aos seguintes pontos de extremidade adicionais

AD FS os pontos de extremidade necessários para o ingresso no domínio do Windows 10 e entrar no
- [nome do serviço de Federação] / adfs/services/trust/2005/windowstransport
- [nome do serviço de Federação] / adfs/services/trust/13/windowstransport
- [nome do serviço de Federação] / adfs/services/trust/2005/usernamemixed
- [nome do serviço de Federação] / adfs/services/trust/13/usernamemixed 
- [nome do serviço de Federação] / adfs/services/trust/2005/certificatemixed
- [nome do serviço de Federação] / adfs/services/trust/13/certificatemixed

Para resolver, atualize todas as políticas que negam com base na declaração de ponto de extremidade para permitir que a exceção para os pontos de extremidade acima.

Por exemplo, a regra abaixo:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

seria atualizado para:

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Declarações desta categoria só devem ser usadas para implementar políticas de negócios e não como as políticas de segurança para proteger o acesso à sua rede.  É possível que os clientes não autorizados enviem cabeçalhos com informações falsas como uma maneira de obter acesso.  
  
As políticas descritas neste artigo, sempre devem ser usadas com outro método de autenticação, como autenticação de multifator de nome de usuário e senha ou multicamada.  
  
## <a name="client-access-policies-scenarios"></a>Cenários de políticas de acesso do cliente  
  
|**Cenário**|**Descrição**| 
| --- | --- | 
|Cenário 1: Bloquear todo o acesso externo ao Office 365|Acesso ao Office 365 é permitido de todos os clientes na rede corporativa interna, mas as solicitações de clientes externos são negadas com base no endereço IP do cliente externo.|  
|Cenário 2: Bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync|Acesso ao Office 365 é permitido de todos os clientes na rede corporativa interna, bem como quaisquer dispositivos de cliente externo, como Smartphones, o que fazer uso do Exchange ActiveSync. Todos os outros clientes externos, como aqueles que usam o Outlook, são bloqueados.|  
|Cenário 3: Bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador|Blocos acesso externo ao Office 365, exceto para aplicações passivas (baseado em navegador), como o Outlook Web Access ou no SharePoint Online.|  
|Cenário 4: Bloquear todo o acesso externo ao Office 365, exceto para grupos do Active Directory designados|Esse cenário é usado para testar e validar a implantação de política de acesso do cliente. Ele bloqueia o acesso externo ao Office 365 somente para membros do grupo do Active Directory de um ou mais. Ele também pode ser usado para fornecer acesso externo somente para membros de um grupo.|  
  
## <a name="enabling-client-access-policy"></a>Habilitar política de acesso de cliente  
 Para habilitar a política de acesso de cliente no AD FS no Windows Server 2012 R2, você deve atualizar a plataforma de identidade do Microsoft Office 365 de confiança de terceira parte confiável. Escolha um dos cenários de exemplo abaixo para configurar as regras de declaração sobre o **plataforma de identidade do Microsoft Office 365** terceira parte confiável que melhor atenda às necessidades da sua organização.  
  
###  <a name="scenario1"></a> Cenário 1: Bloquear todo o acesso externo ao Office 365  
 Nesse cenário de política de acesso de cliente permite o acesso de todos os clientes internos e blocos todos os clientes externos com base no endereço IP do cliente externo. Você pode usar os procedimentos a seguir para adicionar as regras de autorização de emissão corretas para o Office 365 para o cenário escolhido de confiança de terceira parte confiável.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Para criar regras para bloquear todo o acesso externo ao Office 365  
  
1.  Partir **Gerenciador do servidor**, clique em **ferramentas**, em seguida, clique em **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **relações de confiança do AD**, clique em **terceira**, clique com botão direito a **plataforma de identidade do Microsoft Office 365** confia e, em seguida, clique em **Editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
5.  Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "se não houver nenhuma declaração IP fora do intervalo desejado, negar". Sob **regra personalizada**, digite ou cole a sintaxe de linguagem da regra do declaração seguir (substitua o valor acima de "x-ms-forwarded-cliente-ip" com uma expressão válida de IP):  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Clique em **concluir**. Verifique se a nova regra aparece na lista de regras de autorização de emissão antes para o padrão **permitir o acesso a todos os usuários** regra (a regra de negação terá precedência mesmo que ela apareça na lista).  Se você não tiver o padrão permitir regra de acesso, você pode adicionar um no final da lista usando a linguagem de regra de declaração da seguinte maneira:  </br>
    
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Para salvar as novas regras, nos **editar regras de declaração** caixa de diálogo, clique em **Okey**. A lista resultante deve ter a aparência.  
  
     ![Regras de autorização de emissão](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a> Cenário 2: Bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync  
 O exemplo a seguir permite o acesso a todos os aplicativos do Office 365, incluindo o Exchange Online, de clientes internos, incluindo o Outlook. Ele bloqueia o acesso de clientes que residem fora da rede corporativa, conforme indicado pelo endereço IP do cliente, exceto para clientes do Exchange ActiveSync, como Smartphones.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Para criar regras para bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync  
  
1.  Partir **Gerenciador do servidor**, clique em **ferramentas**, em seguida, clique em **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **relações de confiança do AD**, clique em **terceira**, clique com botão direito a **plataforma de identidade do Microsoft Office 365** confia e, em seguida, clique em **Editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
5.  Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "se não houver nenhuma declaração IP fora do intervalo desejado, emitir ipoutsiderange declaração". Sob **regra personalizada**, digite ou cole a sintaxe de linguagem da regra do declaração seguir (substitua o valor acima de "x-ms-forwarded-cliente-ip" com uma expressão válida de IP):  
    
    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Clique em **concluir**. Verifique se a nova regra aparece na **regras de autorização de emissão** lista.  
  
7.  Em seguida, no **editar regras de declaração** caixa de diálogo do **regras de autorização de emissão** , clique em **Adicionar regra de** para iniciar o Assistente de regra de declaração novamente.  
  
8.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
9. Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "se há um IP fora do intervalo desejado e não há uma declaração de x-ms-client-aplicativo não EAS, negar ". Sob **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  

    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. Clique em **concluir**. Verifique se a nova regra aparece na **regras de autorização de emissão** lista.  
  
11. Em seguida, no **editar regras de declaração** caixa de diálogo do **regras de autorização de emissão** , clique em **Adicionar regra de** para iniciar o Assistente de regra de declaração novamente.  
  
12. No **Selecionar modelo de regra** página, em **modelo de regra de declaração** selecionar **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
13. Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "verificar se a declaração do aplicativo existe". Sob **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  
    ```  
    NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. Clique em **concluir**. Verifique se a nova regra aparece na **regras de autorização de emissão** lista.  
  
15. Em seguida, no **editar regras de declaração** caixa de diálogo do **regras de autorização de emissão** , clique em **Adicionar regra de** para iniciar o Assistente de regra de declaração novamente.  
  
16. No **Selecionar modelo de regra** página, em **modelo de regra de declaração** selecionar **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
17. Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "Negar usuários com ipoutsiderange true e o aplicativo não". Sob **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Clique em **concluir**. Verifique se a nova regra aparece imediatamente abaixo da regra anterior e antes de permitir o acesso a todos os usuários padrão regra na lista de regras de autorização de emissão (a regra de negação terá precedência mesmo que ela apareça na lista).  </br>Se você não tiver o padrão permitir regra de acesso, você pode adicionar um no final da lista usando a linguagem de regra de declaração da seguinte maneira:</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Para salvar as novas regras, nos **editar regras de declaração** caixa de diálogo, clique em Okey. A lista resultante deve ter a aparência.  
  
     ![Regras de Autorização de Emissão](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a> Cenário 3: Bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para criar regras para bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador  
  
1.  Partir **Gerenciador do servidor**, clique em **ferramentas**, em seguida, clique em **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **relações de confiança do AD**, clique em **terceira**, clique com botão direito a **plataforma de identidade do Microsoft Office 365** confia e, em seguida, clique em **Editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
5.  Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "se não houver nenhuma declaração IP fora do intervalo desejado, emitir ipoutsiderange declaração". Sob **regra personalizada**, digite ou cole a sintaxe de linguagem da regra do declaração seguir (substitua o valor acima de "x-ms-forwarded-cliente-ip" com uma expressão válida de IP):  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Clique em **concluir**. Verifique se a nova regra aparece na **regras de autorização de emissão** lista.  
  
7.  Em seguida, no **editar regras de declaração** caixa de diálogo do **regras de autorização de emissão** , clique em **Adicionar regra de** para iniciar o Assistente de regra de declaração novamente.  
  
8.  No **Selecionar modelo de regra** página, em **modelo de regra de declaração** selecionar **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
9. Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "se há um IP fora do intervalo desejado e o ponto de extremidade não é/adfs/ls, negar". Sob **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
  
 
    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. Clique em **concluir**. Verifique se a nova regra aparece na lista de regras de autorização de emissão antes para o padrão **permitir o acesso a todos os usuários** regra (a regra de negação terá precedência mesmo que ela apareça na lista).  </br></br> Se você não tiver o padrão permitir regra de acesso, você pode adicionar um no final da lista usando a linguagem de regra de declaração da seguinte maneira:  
  
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. Para salvar as novas regras, nos **editar regras de declaração** caixa de diálogo, clique em **Okey**. A lista resultante deve ter a aparência.  
  
     ![Emissão](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a> Cenário 4: Bloquear todo o acesso externo ao Office 365, exceto para grupos do Active Directory designados  
 O exemplo a seguir habilita o acesso de clientes internos com base no endereço IP. Ele bloqueia o acesso de clientes que residem fora da rede corporativa com um endereço IP de cliente externo, exceto para as pessoas em um grupo de diretório Active Directory especificado as etapas a seguir para adicionar a autorização de emissão correto regras para o  **Plataforma de identidade do Microsoft Office 365** terceira parte confiável usando o Assistente de regra de declaração:  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Para criar regras para bloquear todo o acesso externo ao Office 365, exceto para designado grupos do Active Directory  
  
1.  Partir **Gerenciador do servidor**, clique em **ferramentas**, em seguida, clique em **gerenciamento do AD FS**.  
  
2.  Na árvore de console, sob **relações de confiança do AD**, clique em **terceira**, clique com botão direito a **plataforma de identidade do Microsoft Office 365** confia e, em seguida, clique em **Editar regras de declaração**.  
  
3.  No **editar regras de declaração** caixa de diálogo, selecione o **regras de autorização de emissão** guia e, em seguida, clique em **Adicionar regra** para iniciar o Assistente de regra de declaração.  
  
4.  Sobre o **Selecionar modelo de regra** página, em **modelo de regra de declaração**, selecione **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
5.  Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "se não houver nenhuma declaração IP fora do intervalo desejado, emitir declaração ipoutsiderange." Sob **regra personalizada**, digite ou cole a sintaxe de linguagem da regra do declaração seguir (substitua o valor acima de "x-ms-forwarded-cliente-ip" com uma expressão válida de IP):  
  
      
    `c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Clique em **concluir**. Verifique se a nova regra aparece na **regras de autorização de emissão** lista.  
  
7.  Em seguida, no **editar regras de declaração** caixa de diálogo do **regras de autorização de emissão** , clique em **Adicionar regra de** para iniciar o Assistente de regra de declaração novamente.  
  
8.  No **Selecionar modelo de regra** página, em **modelo de regra de declaração** selecionar **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
9. Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "Verificar SID de grupo". Sob **regra personalizada**, digite ou cole a sintaxe de linguagem da regra do declaração seguir (substitua "groupsid" com o SID real do grupo do AD que você está usando):  
   
    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Clique em **concluir**. Verifique se a nova regra aparece na **regras de autorização de emissão** lista.  
  
11. Em seguida, no **editar regras de declaração** caixa de diálogo do **regras de autorização de emissão** , clique em **Adicionar regra de** para iniciar o Assistente de regra de declaração novamente.  
  
12. No **Selecionar modelo de regra** página, em **modelo de regra de declaração** selecionar **enviar declarações usando uma regra personalizada**e, em seguida, clique em **Avançar**.  
  
13. Sobre o **configurar regra** página, em **nome da regra de declaração**, digite o nome de exibição para essa regra, por exemplo "Negar usuários com ipoutsiderange true e groupsid não". Sob **regra personalizada**, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:  
   
    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Clique em **concluir**. Verifique se a nova regra aparece imediatamente abaixo da regra anterior e antes de permitir o acesso a todos os usuários padrão regra na lista de regras de autorização de emissão (a regra de negação terá precedência mesmo que ela apareça na lista).  </br></br>Se você não tiver o padrão permitir regra de acesso, você pode adicionar um no final da lista usando a linguagem de regra de declaração da seguinte maneira:  
   
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Para salvar as novas regras, nos **editar regras de declaração** caixa de diálogo, clique em Okey. A lista resultante deve ter a aparência.  
  
     ![Emissão](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a> Compilar a expressão de intervalo de endereço IP  
 A declaração de x-ms-forwarded--ip do cliente é preenchida com um cabeçalho HTTP que está definido atualmente pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da declaração pode ser um dos seguintes:  
  
> [!NOTE]
>  O Exchange Online atualmente suporta apenas endereços IPV4 e IPV6 não.  
  
-   Um único endereço IP: O endereço IP do cliente que está conectado diretamente ao Exchange Online  
  
> [!NOTE]
>  -   O endereço IP de um cliente na rede corporativa será exibido como o endereço IP de interface externa do gateway ou proxy de saída da organização.  
> -   Clientes que estão conectados à rede corporativa por uma VPN ou pela Microsoft DirectAccess (DA) podem aparecer como clientes corporativos internos ou clientes externos, dependendo da configuração de VPN ou DA.  
  
-   Um ou mais endereços IP: Quando o Exchange Online não puder determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor do cabeçalho x-forwarded-for, um cabeçalho não padrão que pode ser incluído em solicitações com base em HTTP e é compatível com muitos clientes, balanceadores de carga, e proxies no mercado.  
  
> [!NOTE]
>  1.  Vários endereços IP, que indica o endereço IP do cliente e o endereço de cada proxy que passou a solicitação serão ser separados por uma vírgula.  
> 2.  Endereços IP relacionados à infraestrutura do Exchange Online serão não está na lista.  
  
### <a name="regular-expressions"></a>Expressões regulares  
 Quando você tem que corresponder a um intervalo de endereços IP, ele se torna necessário construir uma expressão regular para executar a comparação. A próxima série de etapas, forneceremos exemplos de como construir uma expressão de acordo com os seguintes intervalos de endereços (Observe que você terá que alterar esses exemplos para coincidir com o intervalo de IP público):  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 Primeiro, o padrão básico que corresponderá a um único endereço IP é da seguinte maneira: \b###\\. # # #\\. # # #\\. # # # \b  
  
 Estendendo isso, podemos combinar dois endereços IP diferentes com uma expressão OR da seguinte maneira: \b###\\. # # #\\. # # #\\. # # # \b&#124;\b###\\. # # #\\. # # #\\. # # # \b  
  
 Portanto, seria um exemplo para corresponder a apenas dois endereços (por exemplo, 192.168.1.1 ou 10.0.0.1): \b192\\.168\\.1\\. 1\b&#124;\b10\\.0\\.0\\. 1\b  
  
 Isso lhe dá a técnica pela qual você pode inserir qualquer número de endereços. Sempre que um intervalo de endereço precisa ter permissão de, por exemplo, 192.168.1.1 – 192.168.1.25, a correspondência é necessário fazer caractere por caractere: \b192\\.168\\.1\\. ( [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b  
  
 Observe o seguinte:  
  
-   O endereço IP é tratado como cadeia de caracteres e não é um número.  
  
-   A regra é dividida da seguinte maneira: \b192\\.168\\.1\\.  
  
-   Isso corresponde a qualquer valor começando com 192.168.1.  
  
-   A seguir corresponde os intervalos necessários para a parte do endereço após o ponto decimal final:  
  
    -   ([1-9] correspondências terminados em 1 a 9  
  
    -   &#124;1 de [0-9] corresponde a endereços que terminam em 19 de 10  
  
    -   &#124;2[0-5]) correspondências terminados em 20 a 25  
  
-   Observe que os parênteses que devem ser posicionados corretamente, para que você não comece a outras partes de endereços IP correspondentes.  
  
-   Com o bloco de 192 correspondido, podemos escrever uma expressão semelhante para o bloco de 10: \b10\\.0\\.0\\. ( [1-9] &#124;1 [0-4]) \b  
  
-   E colocá-las em conjunto, a expressão a seguir deve corresponder todos os endereços para "192.168.1.1~25" e "10.0.0.1~14": \b192\\.168\\.1\\. ( [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\.0\\.0\\. ( [1-9] &#124;1 [0-4]) \b  
  
### <a name="testing-the-expression"></a>Testar a expressão  
 Expressões de regex podem se tornar bastante complicadas, portanto, é altamente recomendável usar uma ferramenta de verificação de regex. Se você fizer uma pesquisa na internet para "construtor de expressão regex online", você encontrará vários utilitários online BOM que permitirá que você experimente as expressões em relação aos dados de exemplo.  
  
 Quando a expressão de teste, é importante entender o que esperar para devem corresponder. O sistema do Exchange online pode enviar muitos endereços IP, separados por vírgulas. As expressões fornecidas acima funcionará para isso. No entanto, é importante pensar sobre isso durante o teste suas expressões de regex. Por exemplo, um pode usar o exemplo a seguir para verificar se os exemplos acima de entrada:  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>Tipos de declaração  
 O AD FS no Windows Server 2012 R2 fornece informações de contexto de solicitação usando os seguintes tipos de declaração:  
  
### <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP  
 Tipo de declaração: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 Essa declaração do AD FS representa uma "melhor tentativa" atestar o endereço IP do usuário (por exemplo, o cliente Outlook) que está fazendo a solicitação. Essa declaração pode conter vários endereços IP, incluindo o endereço de cada proxy que encaminhou a solicitação.  Essa declaração é preenchida com um HTTP. O valor da declaração pode ser um dos seguintes:  
  
-   Um único endereço IP – endereço IP do cliente que está conectado diretamente ao Exchange Online  
  
> [!NOTE]
>  O endereço IP de um cliente na rede corporativa será exibido como o endereço IP de interface externa do gateway ou proxy de saída da organização.  
  
-   Um ou mais endereços IP  
  
    -   Se o Exchange Online não puder determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor do cabeçalho x-forwarded-for, um cabeçalho não padrão que pode ser incluído em HTTP com base em solicitações e é compatível com muitos clientes, balanceadores de carga, e proxies do mercado.  
  
    -   Vários endereços IP que indica o endereço IP do cliente e o endereço de cada proxy que passou a solicitação serão ser separados por uma vírgula.  
  
> [!NOTE]
>  Endereços IP relacionados à infraestrutura do Exchange Online não estarão presentes na lista.  
  
> [!WARNING]
>  O Exchange Online atualmente suporta apenas a endereços IPV4; ele não oferece suporte a endereços IPV6.  
  
### <a name="x-ms-client-application"></a>X-MS-aplicativo de cliente  
 Tipo de declaração: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 Essa declaração do AD FS representa o protocolo usado pelo cliente final, que corresponde livremente ao aplicativo que está sendo usado.  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente só definidas pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. Dependendo do aplicativo, o valor da declaração será um dos seguintes:  
  
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
  
### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 Tipo de declaração: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 Essa declaração do AD FS fornece uma cadeia de caracteres para representar o tipo de dispositivo que o cliente está usando para acessar o serviço. Isso pode ser usado quando os clientes desejarem impedir o acesso para determinados dispositivos (como tipos específicos de Smartphones).  Valores de exemplo para essa declaração incluem (mas não estão limitados a) os valores a seguir.  
  
 A seguir estão exemplos de um cliente cujo x-ms-client-aplicativo é "Microsoft.Exchange.ActiveSync" pode conter o valor de x-ms-user-agent  
  
-   Vortex 1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android/0.3  
  
 Também é possível que esse valor está vazio.  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Tipo de declaração: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 Essa declaração do AD FS indica que a solicitação tiver passado por meio do proxy de aplicativo Web.  Essa declaração é preenchida com o proxy de aplicativo Web, que preenche o cabeçalho ao passar a solicitação de autenticação para o serviço de federação de back-end. O AD FS, em seguida, o converte em uma declaração.  
  
 O valor da declaração é o nome DNS do proxy de aplicativo Web que passou a solicitação.  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Tipo de declaração: `http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 Tipo de declaração, semelhante de x-ms-proxy acima, esse tipo de declaração que indica se a solicitação tiver passado por meio do proxy de aplicativo web. Ao contrário de x-ms-proxy insidecorporatenetwork é um valor booliano com uma solicitação diretamente para o serviço de federação de dentro da rede corporativa, indicando True.  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-ponto de extremidade-Absolute-Path (ativo vs passivo)  
 Tipo de declaração: `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 Esse tipo de declaração pode ser usado para determinar as solicitações provenientes de clientes (avançados) "ativos" versus "passivos" (browser-clientes baseados na web). Isso permite que as solicitações externas de aplicativos baseados em navegador, como o Outlook Web Access, o SharePoint Online ou o portal do Office 365 a serem permitidos enquanto as solicitações provenientes de clientes avançados, como o Microsoft Outlook são bloqueadas.  
  
 O valor da declaração é o nome do serviço do AD FS que recebeu a solicitação.  
  
## <a name="see-also"></a>Consulte também  
 [Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
