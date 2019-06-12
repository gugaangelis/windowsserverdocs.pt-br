---
ms.assetid: ''
title: Políticas de controle de acesso de cliente no Active Directory Federation Services 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 036d6d0543687e7f82caf3dfd2c3bb0b4a981181
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445055"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Políticas de controle de acesso de cliente no AD FS 2.0
Uma política de acesso de cliente no Active Directory Federation Services 2.0 permitem restringir ou conceder aos usuários acesso aos recursos.  Este documento descreve como habilitar políticas de acesso do cliente no AD FS 2.0 e como configurar os cenários mais comuns.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Habilitar política de acesso de cliente no AD FS 2.0

Para habilitar a política de acesso do cliente, siga as etapas abaixo.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Etapa 1: Instalar o Update Rollup 2 para o AD FS 2.0 do pacote nos servidores do AD FS

Baixe o [Update Rollup 2 para os serviços de Federação do Active Directory (AD FS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) empacotar e instalá-lo em todos os servidores de Federação e proxies de servidor de Federação.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Etapa 2: Adicione que cinco regras de declaração para a relação de confiança do provedor de declarações do Active Directory

Depois que o Update Rollup 2 tiver sido instalado em todos os servidores do AD FS e proxies, use o procedimento a seguir para adicionar um conjunto de regras de declarações que disponibiliza os novos tipos de declaração para o mecanismo de políticas.

Para fazer isso, você adicionará cinco regras de transformação de aceitação para cada um dos novos tipos de declaração de contexto solicitação usando o procedimento a seguir.

Sobre a confiança do provedor de declarações do Active Directory, crie uma nova regra de transformação de aceitação de passagem, cada um dos novos tipos de declaração de contexto de solicitação.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Para adicionar uma regra de declaração para o Active Directory relação de confiança de provedor de declarações para cada contexto de cinco tipos de declaração:


1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e, em seguida, clique em AD FS 2.0 Management.
2. Na árvore de console, em AD FS 2.0 \ relações, clique em confianças do provedor de declarações do Active Directory com o botão direito e, em seguida, clique em Editar regras de declaração.
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de transformação de aceitação e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra.
4. Na página Selecionar modelo de regra, em um modelo de regra de declaração, selecione passar ou filtrar uma declaração de entrada na lista e, em seguida, clique em Avançar.
5. Na página Configurar regra, abaixo do nome da regra de declaração, digite o nome de exibição para essa regra; na entrada tipo de declaração, digite a URL de tipo de declaração a seguir e, em seguida, selecione passagem por todos os valores de declaração.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Para verificar se a regra, selecione-o na lista, clique em Editar regra e clique em Exibir linguagem da regra. A linguagem de regra de declaração deve aparecer da seguinte maneira: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Clique em Concluir.
8. Na caixa de diálogo Editar regras de declaração, clique em Okey para salvar as regras.
9. Repita as etapas 2 a 6 para criar uma regra de declaração adicional para cada um dos tipos de declaração de quatro restantes, mostrados abaixo até que todas as cinco regras foram criadas.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Etapa 3: Atualizar a plataforma de identidade do Microsoft Office 365 de confiança de terceira parte confiável

Escolha um dos cenários de exemplo abaixo para configurar as regras de declaração sobre a plataforma de identidade do Microsoft Office 365 de confiança que melhor atenda às necessidades da sua organização de terceira parte confiável.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Cenários de política de acesso de cliente do AD FS 2.0
As seções a seguir descrevem os cenários existentes do AD FS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Cenário 1: Bloquear todo o acesso externo ao Office 365

Nesse cenário de política de acesso de cliente permite o acesso de todos os clientes internos e blocos todos os clientes externos com base no endereço IP do cliente externo. O conjunto de regras baseia-se a regra de autorização de emissão padrão permitir o acesso a todos os usuários. Você pode usar o procedimento a seguir para adicionar uma regra de autorização de emissão para o objeto de confiança de terceira parte confiável do Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e, em seguida, clique em AD FS 2.0 Management.
2. Na árvore de console, em AD FS 2.0 \ relações, clique em confianças de terceiros confiável, a relação de confiança de plataforma de identidade do Microsoft Office 365 com o botão direito e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em um modelo de regra de declaração, selecione Enviar declarações usando uma regra personalizada e, em seguida, clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Clique em Concluir. Verifique se que a nova regra aparece imediatamente abaixo de permitir o acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em Okey.

>[!NOTE]
>Você precisará substituir o valor acima de "regex de endereço ip público" com uma expressão válida de IP; Consulte a compilar a expressão de intervalo de endereço IP para obter mais informações.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Cenário 2: Bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync

O exemplo a seguir permite o acesso a todos os aplicativos do Office 365, incluindo o Exchange Online, de clientes internos, incluindo o Outlook. Ele bloqueia o acesso de clientes que residem fora da rede corporativa, conforme indicado pelo endereço IP do cliente, exceto para clientes do Exchange ActiveSync, como Smartphones. O conjunto de regras baseia-se a regra de autorização de emissão padrão intitulada permitir o acesso a todos os usuários. Use as etapas a seguir para adicionar uma regra de autorização de emissão para o Office 365 usando o Assistente de regra de declaração de confiança de terceira parte confiável:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e, em seguida, clique em AD FS 2.0 Management.
2. Na árvore de console, em AD FS 2.0 \ relações, clique em confianças de terceiros confiável, a relação de confiança de plataforma de identidade do Microsoft Office 365 com o botão direito e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em um modelo de regra de declaração, selecione Enviar declarações usando uma regra personalizada e, em seguida, clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se que a nova regra aparece imediatamente abaixo de permitir o acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em Okey.

>[!NOTE]
>Você precisará substituir o valor acima de "regex de endereço ip público" com uma expressão válida de IP; Consulte a compilar a expressão de intervalo de endereço IP para obter mais informações.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Cenário 3: Bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador

O conjunto de regras baseia-se a regra de autorização de emissão padrão intitulada permitir o acesso a todos os usuários. Use as etapas a seguir para adicionar uma regra de autorização de emissão para a plataforma de identidade do Microsoft Office 365 usando o Assistente de regra de declaração de confiança de terceira parte confiável:

>[!NOTE]
>Não há suporte para esse cenário com um proxy de terceiros devido às limitações nos cabeçalhos de diretiva de acesso de cliente com solicitações passivos (baseado na Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e, em seguida, clique em AD FS 2.0 Management.
2. Na árvore de console, em AD FS 2.0 \ relações, clique em confianças de terceiros confiável, a relação de confiança de plataforma de identidade do Microsoft Office 365 com o botão direito e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em um modelo de regra de declaração, selecione Enviar declarações usando uma regra personalizada e, em seguida, clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se que a nova regra aparece imediatamente abaixo de permitir o acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em Okey.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Cenário 4: Bloquear todo o acesso externo ao Office 365 designados grupos do Active Directory

O exemplo a seguir habilita o acesso de clientes internos com base no endereço IP. Ele bloqueia o acesso de clientes que residem fora da rede corporativa com um endereço IP de cliente externo, exceto para as pessoas em uma regra especificada do grupo de diretório Active Directory set baseia-se a regra de autorização de emissão padrão intitulada permitir o acesso a Todos os usuários. Use as etapas a seguir para adicionar uma regra de autorização de emissão para a plataforma de identidade do Microsoft Office 365 usando o Assistente de regra de declaração de confiança de terceira parte confiável:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365 designados grupos do Active Directory



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e, em seguida, clique em AD FS 2.0 Management.
2. Na árvore de console, em AD FS 2.0 \ relações, clique em confianças de terceiros confiável, a relação de confiança de plataforma de identidade do Microsoft Office 365 com o botão direito e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em um modelo de regra de declaração, selecione Enviar declarações usando uma regra personalizada e, em seguida, clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se que a nova regra aparece imediatamente abaixo de permitir o acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em Okey.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descrições de sintaxe de linguagem de regra de declaração usada nos cenários acima

|                                                                                                   Descrição                                                                                                   |                                                                     Sintaxe de linguagem de regra de declaração                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              Padrão do AD FS regra permitir o acesso a todos os usuários. Essa regra já deve existir na plataforma de identidade do Microsoft Office 365 lista de regras de autorização de emissão de confiança de terceira parte confiável.              |                                  => issue(Type = "<https://schemas.microsoft.com/authorization/claims/permit>", Value = "true");                                   |
|                               Adicionar esta cláusula para uma regra nova e personalizada Especifica que a solicitação é proveniente de proxy do servidor de Federação (ou seja, ele tem o cabeçalho x-ms-proxy)                                |                                                                                                                                                                    |
|                                                                                 É recomendável que todas as regras de incluem isso.                                                                                  |                                    exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy>"])                                    |
|                                                         Usado para estabelecer que a solicitação é de um cliente com um IP no intervalo aceitável definido.                                                         | NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip>", Value=~"customer-provided public ip address regex"]) |
|                                    Essa cláusula é usada para especificar se o aplicativo que está sendo acessado não for Microsoft.Exchange.ActiveSync a solicitação deve ser negada.                                     |       NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application>", Value=="Microsoft.Exchange.ActiveSync"])        |
|                                                      Essa regra permite que você determine se a chamada for feita através de um navegador da Web e não será negada.                                                      |              NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path>", Value == "/adfs/ls/"])               |
| Esta regra indica que os únicos usuários em um grupo específico do Active Directory (com base no valor de SID) devem ser negados. Adicionando para essa instrução não significa que um grupo de usuários será permitido, independentemente do local. |             exists([Type == "<https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid>", Value =~ "{Group SID value of allowed AD group}"])              |
|                                                                Isso é uma cláusula necessária para emitir um deny quando todas as condições anteriores forem atendidas.                                                                 |                                   => issue(Type = "<https://schemas.microsoft.com/authorization/claims/deny>", Value = "true");                                    |

### <a name="building-the-ip-address-range-expression"></a>Compilar a expressão de intervalo de endereço IP

A declaração de x-ms-forwarded--ip do cliente é preenchida com um cabeçalho HTTP que está definido atualmente pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da declaração pode ser um dos seguintes:

>[!Note] 
>O Exchange Online atualmente suporta apenas endereços IPV4 e IPV6 não.

Um único endereço IP: O endereço IP do cliente que está conectado diretamente ao Exchange Online

>[!Note] 
>O endereço IP de um cliente na rede corporativa será exibido como o endereço IP de interface externa do gateway ou proxy de saída da organização.

Clientes que estão conectados à rede corporativa por uma VPN ou pela Microsoft DirectAccess (DA) podem aparecer como clientes corporativos internos ou clientes externos, dependendo da configuração de VPN ou DA.

Um ou mais endereços IP: Quando o Exchange Online não puder determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor do cabeçalho x-forwarded-for, um cabeçalho não padrão que pode ser incluído em solicitações com base em HTTP e é compatível com muitos clientes, balanceadores de carga, e proxies no mercado.

>[!Note]
>Vários endereços IP, que indica o endereço IP do cliente e o endereço de cada proxy que passou a solicitação serão ser separados por uma vírgula.

Endereços IP relacionados à infraestrutura do Exchange Online não aparecerá na lista.


#### <a name="regular-expressions"></a>Expressões regulares

Quando você tem que corresponder a um intervalo de endereços IP, ele se torna necessário construir uma expressão regular para executar a comparação. A próxima série de etapas, forneceremos exemplos de como construir uma expressão de acordo com os seguintes intervalos de endereços (Observe que você terá que alterar esses exemplos para coincidir com o intervalo de IP público):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

Primeiro, o padrão básico que corresponderá a um único endereço IP é da seguinte maneira: \b###\.###\.###\.\b # # #

Estendendo isso, podemos combinar dois endereços IP diferentes com uma expressão OR da seguinte maneira: \b###\.###\.###\.# # # \b|\b###\. ### \. ### \.\b # # #

Portanto, seria um exemplo para corresponder a apenas dois endereços (por exemplo, 192.168.1.1 ou 10.0.0.1): \b192\.168\.1\.1\b | \b10\.0\.0\.1\b

Isso lhe dá a técnica pela qual você pode inserir qualquer número de endereços. Sempre que um intervalo de endereço precisam permitidos, por exemplo, 192.168.1.1 – 192.168.1.25, a correspondência é necessário fazer caractere por caractere: \b192\.168\.1\.([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>O endereço IP é tratado como cadeia de caracteres e não é um número.


A regra é dividida da seguinte maneira: \b192\.168\.1\.

Isso corresponde a qualquer valor começando com 192.168.1.

A seguir corresponde os intervalos necessários para a parte do endereço após o ponto decimal final:


- ([1-9] correspondências terminados em 1 a 9
- | 1 [0-9] corresponde a endereços que terminam em 19 de 10
- |2[0-5]) correspondências terminados em 20 a 25

>[!Note]
>Os parênteses que devem ser posicionados corretamente, para que você não comece a outras partes de endereços IP correspondentes.

Com o bloco de 192 correspondido, podemos escrever uma expressão semelhante para o bloco de 10: \b10\.0\.0\.([1-9] | [0-4] de 1) \b

E colocá-las em conjunto, a expressão a seguir deve corresponder todos os endereços para "192.168.1.1~25" e "10.0.0.1~14": \b192\.168\.1\.([1-9] | 1 [0-9] | 2 [0-5]) \b|\b10\.0\.0\. ([1-9] | [0-4] de 1) \b

#### <a name="testing-the-expression"></a>Testar a expressão

Expressões de regex podem se tornar bastante complicadas, portanto, é altamente recomendável usar uma ferramenta de verificação de regex. Se você fizer uma pesquisa na internet para "construtor de expressão regex online", você encontrará vários utilitários online BOM que permitirá que você experimente as expressões em relação aos dados de exemplo.

Quando a expressão de teste, é importante entender o que esperar para devem corresponder. O sistema do Exchange online pode enviar muitos endereços IP, separados por vírgulas. As expressões fornecidas acima funcionará para isso. No entanto, é importante pensar sobre isso durante o teste suas expressões de regex. Por exemplo, um pode usar o exemplo a seguir para verificar se os exemplos acima de entrada: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>Validar a implantação

### <a name="security-audit-logs"></a>Logs de auditoria de segurança

Para verificar que o novo contexto de solicitação declarações estão sendo enviadas e estão disponíveis para o AD FS declarações pipeline de processamento, habilite a auditoria de registro em log no servidor do AD FS. Em seguida, envie algumas solicitações de autenticação e a seleção para os valores de declaração nas entradas de log de auditoria de segurança padrão. 

Para habilitar o registro em log de auditoria de eventos de segurança, faça logon em um servidor do AD FS, siga as etapas em configurar a auditoria do AD FS 2.0.

### <a name="event-logging"></a>Log de eventos

Por padrão, as solicitações com falha são registradas no log de eventos do aplicativo localizado em Applications and Services Logs \ AD FS 2.0 \ Admin.For obter mais informações no log de eventos do AD FS, consulte [configurar o AD FS 2.0 o log de eventos](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configurando o detalhado de AD FS nos Logs de rastreamento

Eventos de rastreamento do AD FS são registrados no log de depuração 2.0 do AD FS. Para habilitar o rastreamento, consulte [configurar o rastreamento de depuração para o AD FS 2.0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

Depois de habilitar o rastreamento, use a seguinte sintaxe de linha de comando para habilitar o nível de registro em log detalhado: wevtutil.exe sl "do AD FS 2.0 rastreamento/depuração" /l:5  

## <a name="related"></a>Relacionados
Para obter mais informações sobre os novos tipos de declaração, consulte [tipos de declarações do AD FS](AD-FS-Claims-Types.md).

