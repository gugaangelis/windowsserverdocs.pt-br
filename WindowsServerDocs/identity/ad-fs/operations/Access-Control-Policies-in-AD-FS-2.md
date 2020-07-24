---
title: Políticas de controle de acesso de cliente no Serviços de Federação do Active Directory (AD FS) 2,0
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 110bc74d6b77c63fc6a9554049b5adb940f2641d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962668"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Políticas de controle de acesso de cliente no AD FS 2,0
As políticas de acesso para cliente no Serviços de Federação do Active Directory (AD FS) 2,0 permitem que você restrinja ou conceda acesso a recursos aos usuários.  Este documento descreve como habilitar as políticas de acesso do cliente no AD FS 2,0 e como configurar os cenários mais comuns.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Habilitando a política de acesso de cliente no AD FS 2,0

Para habilitar a política de acesso do cliente, siga as etapas abaixo.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Etapa 1: instalar o pacote cumulativo de atualizações 2 para AD FS pacotes 2,0 em seus servidores AD FS

Baixe o pacote [cumulativo de atualizações 2 para serviços de Federação do Active Directory (AD FS) (AD FS) 2,0](https://support.microsoft.com/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) e instale-o em todos os proxies do servidor de Federação e do servidor de Federação.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Etapa 2: adicionar cinco regras de declaração à Active Directory confiança do provedor de declarações

Após a instalação do pacote cumulativo de atualizações 2 em todos os servidores e proxies AD FS, use o procedimento a seguir para adicionar um conjunto de regras de declarações que torna os novos tipos de declaração disponíveis para o mecanismo de política.

Para fazer isso, você adicionará cinco regras de transformação de aceitação para cada um dos novos tipos de declaração de contexto de solicitação usando o procedimento a seguir.

Na Active Directory confiança do provedor de declarações, crie uma nova regra de transformação de aceitação para passar cada um dos novos tipos de declaração de contexto de solicitação.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Para adicionar uma regra de declaração à Active Directory confiança do provedor de declarações para cada um dos cinco tipos de declaração de contexto:


1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS gerenciamento de 2,0.
2. Na árvore de console, em AD FS 2.0 \ relações de confiança, clique em relações de confiança do provedor de declarações, clique com o botão direito do mouse em Active Directory e clique em Editar regras de declaração.
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de transformação de aceitação e clique em Adicionar regra para iniciar o assistente de regra.
4. Na página Selecionar modelo de regra, em modelo de regra de declaração, selecione passar ou filtrar uma declaração de entrada na lista e clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome de exibição para esta regra; em tipo de declaração de entrada, digite a seguinte URL de tipo de declaração e, em seguida, selecione passar todos os valores de declaração.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Para verificar a regra, selecione-a na lista e clique em Editar regra e, em seguida, clique em Exibir idioma da regra. O idioma da regra de declaração deve aparecer da seguinte maneira:`c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Clique em Concluir.
8. Na caixa de diálogo Editar regras de declaração, clique em OK para salvar as regras.
9. Repita as etapas de 2 a 6 para criar uma regra de declaração adicional para cada um dos quatro tipos de declaração restantes mostrados abaixo até que todas as cinco regras tenham sido criadas.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Etapa 3: atualizar a relação de confiança de terceira parte confiável da plataforma de identidade Microsoft Office 365

Escolha um dos cenários de exemplo abaixo para configurar as regras de declaração na relação de confiança de terceira parte confiável da plataforma de identidade Microsoft Office 365 que melhor atende às necessidades da sua organização.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Cenários de política de acesso de cliente para AD FS 2,0
As seções a seguir descrevem os cenários que existem para o AD FS 2,0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Cenário 1: bloquear todo o acesso externo ao Office 365

Esse cenário de política de acesso de cliente permite o acesso de todos os clientes internos e bloqueia todos os clientes externos com base no endereço IP do cliente externo. O conjunto de regras se baseia na regra de autorização de emissão padrão permitir acesso a todos os usuários. Você pode usar o procedimento a seguir para adicionar uma regra de autorização de emissão à relação de confiança de terceira parte confiável do Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS gerenciamento de 2,0.
2. Na árvore de console, em AD FS 2.0 \ relações de confiança, clique em confiança de terceira parte confiável, clique com o botão direito do mouse na relação de confiança da plataforma de identidade Microsoft Office 365 e clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e clique em Adicionar regra para iniciar o assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em modelo de regra de declaração, selecione enviar declarações usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome para exibição dessa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo da regra permitir acesso a todos os usuários na lista regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em OK.

>[!NOTE]
>Você precisará substituir o valor acima por "Regex de endereço IP público" por uma expressão de IP válida; consulte criando a expressão de intervalo de endereços IP para obter mais informações.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Cenário 2: bloquear todo o acesso externo ao Office 365, exceto o Exchange ActiveSync

O exemplo a seguir permite o acesso a todos os aplicativos do Office 365, incluindo o Exchange Online, de clientes internos, incluindo o Outlook. Ele bloqueia o acesso de clientes que residem fora da rede corporativa, conforme indicado pelo endereço IP do cliente, exceto para clientes do Exchange ActiveSync, como Smart Phones. O conjunto de regras se baseia na regra de autorização de emissão padrão chamada permitir acesso a todos os usuários. Use as etapas a seguir para adicionar uma regra de autorização de emissão à relação de confiança de terceira parte confiável do Office 365 usando o assistente de regra de declaração:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS gerenciamento de 2,0.
2. Na árvore de console, em AD FS 2.0 \ relações de confiança, clique em confiança de terceira parte confiável, clique com o botão direito do mouse na relação de confiança da plataforma de identidade Microsoft Office 365 e clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e clique em Adicionar regra para iniciar o assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em modelo de regra de declaração, selecione enviar declarações usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome para exibição dessa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo da regra permitir acesso a todos os usuários na lista regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em OK.

>[!NOTE]
>Você precisará substituir o valor acima por "Regex de endereço IP público" por uma expressão de IP válida; consulte criando a expressão de intervalo de endereços IP para obter mais informações.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Cenário 3: bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador

O conjunto de regras se baseia na regra de autorização de emissão padrão chamada permitir acesso a todos os usuários. Use as etapas a seguir para adicionar uma regra de autorização de emissão à Microsoft Office terceira parte confiável da plataforma de identidade 365 usando o assistente de regra de declaração:

>[!NOTE]
>Esse cenário não tem suporte com um proxy de terceiros devido a limitações nos cabeçalhos de política de acesso do cliente com solicitações passivas (baseadas na Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS gerenciamento de 2,0.
2. Na árvore de console, em AD FS 2.0 \ relações de confiança, clique em confiança de terceira parte confiável, clique com o botão direito do mouse na relação de confiança da plataforma de identidade Microsoft Office 365 e clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e clique em Adicionar regra para iniciar o assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em modelo de regra de declaração, selecione enviar declarações usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome para exibição dessa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo da regra permitir acesso a todos os usuários na lista regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em OK.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Cenário 4: bloquear todo o acesso externo ao Office 365 para grupos de Active Directory designados

O exemplo a seguir habilita o acesso de clientes internos com base no endereço IP. Ele bloqueia o acesso de clientes que residem fora da rede corporativa que têm um endereço IP de cliente externo, exceto aqueles indivíduos em um grupo de Active Directory especificado. o conjunto de regras se baseia na regra de autorização de emissão padrão chamada permitir acesso a todos os usuários. Use as etapas a seguir para adicionar uma regra de autorização de emissão à Microsoft Office terceira parte confiável da plataforma de identidade 365 usando o assistente de regra de declaração:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Para criar uma regra para bloquear todo o acesso externo ao Office 365 para grupos de Active Directory designados



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS gerenciamento de 2,0.
2. Na árvore de console, em AD FS 2.0 \ relações de confiança, clique em confiança de terceira parte confiável, clique com o botão direito do mouse na relação de confiança da plataforma de identidade Microsoft Office 365 e clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e clique em Adicionar regra para iniciar o assistente de regra de declaração.
4. Na página Selecionar modelo de regra, em modelo de regra de declaração, selecione enviar declarações usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra de declaração, digite o nome para exibição dessa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração:`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo da regra permitir acesso a todos os usuários na lista regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar regras de declaração, clique em OK.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descrições da sintaxe de linguagem de regra de declaração usada nos cenários acima

|                                                                                                   Descrição                                                                                                   |                                                                     Sintaxe de linguagem de regra de declaração                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              Regra de AD FS padrão para permitir o acesso a todos os usuários. Essa regra já deve existir na lista de regras de autorização de emissão de confiança de terceira parte confiável da plataforma de identidade Microsoft Office 365.              |                                  => problema (tipo = " <https://schemas.microsoft.com/authorization/claims/permit> ", valor = "verdadeiro");                                   |
|                               Adicionar essa cláusula a uma nova regra personalizada especifica que a solicitação vem do proxy do servidor de Federação (ou seja, tem o cabeçalho x-MS-Proxy)                                |                                                                                                                                                                    |
|                                                                                 É recomendável que todas as regras incluam isso.                                                                                  |                                    Exists ([type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy> "])                                    |
|                                                         Usado para estabelecer que a solicitação é de um cliente com um IP no intervalo aceitável definido.                                                         | NOT EXISTS ([type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip> ", valor = ~ "Regex de endereço IP público fornecido pelo cliente"]) |
|                                    Essa cláusula é usada para especificar que, se o aplicativo que está sendo acessado não for Microsoft. Exchange. ActiveSync, a solicitação deverá ser negada.                                     |       Não existe ([type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application> ", valor = = "Microsoft. Exchange. ActiveSync"])        |
|                                                      Essa regra permite que você determine se a chamada foi por meio de um navegador da Web e não será negada.                                                      |              Não existe ([type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path> ", valor = = "/adfs/ls/"])               |
| Essa regra declara que os únicos usuários em um determinado grupo de Active Directory (com base no valor de SID) devem ser negados. Adicionar não a essa instrução significa que um grupo de usuários será permitido, independentemente do local. |             Exists ([tipo = = " <https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid> ", valor = ~ "{valor de Sid de grupo do grupo do AD permitido}"])              |
|                                                                Essa é uma cláusula necessária para emitir uma negação quando todas as condições anteriores forem atendidas.                                                                 |                                   => problema (tipo = " <https://schemas.microsoft.com/authorization/claims/deny> ", valor = "verdadeiro");                                    |

### <a name="building-the-ip-address-range-expression"></a>Criando a expressão de intervalo de endereços IP

A declaração x-MS-encaminhar-Client-IP é populada a partir de um cabeçalho HTTP atualmente definido somente pelo Exchange Online, que popula o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da declaração pode ser um dos seguintes:

>[!Note] 
>Atualmente, o Exchange Online dá suporte apenas a endereços IPV4 e não IPV6.

Um único endereço IP: o endereço IP do cliente que está conectado diretamente ao Exchange Online

>[!Note] 
>O endereço IP de um cliente na rede corporativa será exibido como o endereço IP da interface externa do proxy ou gateway de saída da organização.

Os clientes que estão conectados à rede corporativa por uma VPN ou pelo Microsoft DirectAccess (DA) podem aparecer como clientes corporativos internos ou como clientes externos, dependendo da configuração da VPN ou DA.

Um ou mais endereços IP: quando o Exchange Online não pode determinar o endereço IP do cliente que está se conectando, ele definirá o valor com base no valor do cabeçalho x-Forwarded-for, um cabeçalho não padrão que pode ser incluído em solicitações baseadas em HTTP e tem suporte de vários clientes, balanceadores de carga e proxies no mercado.

>[!Note]
>Vários endereços IP, indicando o endereço IP do cliente e o endereço de cada proxy que passou na solicitação, serão separados por uma vírgula.

Os endereços IP relacionados à infraestrutura do Exchange Online não serão exibidos na lista.


#### <a name="regular-expressions"></a>Expressões regulares

Quando você precisa corresponder a um intervalo de endereços IP, é necessário construir uma expressão regular para executar a comparação. Na próxima série de etapas, forneceremos exemplos de como construir essa expressão para corresponder aos intervalos de endereços a seguir (Observe que você precisará alterar esses exemplos para corresponder ao seu intervalo de IP público):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

Primeiro, o padrão básico que corresponderá a um único endereço IP é o seguinte: \b # # # \. ### \. ### \. # # # \b

Estendendo isso, podemos corresponder dois endereços IP diferentes com uma expressão or da seguinte maneira: \b # # # \. ### \. ### \. # # # \b | \b # # # \. ### \. ### \. # # # \b

Portanto, um exemplo para corresponder apenas a dois endereços (como 192.168.1.1 ou 10.0.0.1) seria: \b192 \. 168 \. 1 \. 1 \ b | \b10 \. 0 \. 0 \. 1 \ b

Isso oferece a técnica pela qual você pode inserir qualquer número de endereços. Onde um intervalo de endereços precisa ser permitido, por exemplo, 192.168.1.1 – 192.168.1.25, a correspondência deve ser feita caractere por caractere: \b192 \. 168 \. 1 \. ([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>O endereço IP é tratado como cadeia de caracteres e não como um número.


A regra é dividida da seguinte maneira: \b192 \. 168 \. 1\.

Isso corresponde a qualquer valor que comece com 192.168.1.

O seguinte corresponde aos intervalos necessários para a parte do endereço após o último ponto decimal:


- ([1-9] corresponde a endereços que terminam em 1-9
- | 1 [0-9] corresponde a endereços que terminam em 10-19
- | 2 [0-5]) corresponde a endereços que terminam em 20-25

>[!Note]
>Os parênteses devem ser posicionados corretamente, para que você não comece a corresponder a outras partes de endereços IP.

Com o bloco 192 correspondente, podemos gravar uma expressão semelhante para o 10 bloco: \b10 \. 0 \. 0 \. ([1-9] | 1 [0-4]) \b

E colocando-os juntos, a expressão a seguir deve corresponder a todos os endereços de "192.168.1.1 ~ 25" e "10.0.0.1 ~ 14": \b192 \. 168 \. 1 \. ([1-9] | 1 [0-9] | 2 [0-5]) \b | \b10 \. 0 \. 0 \. ([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>Testando a expressão

As expressões Regex podem se tornar muito complicadas, portanto, é altamente recomendável usar uma ferramenta de verificação Regex. Se você fizer uma pesquisa na Internet por "Construtor de expressões de Regex online", encontrará vários bons utilitários online que permitirão experimentar suas expressões em relação aos dados de exemplo.

Ao testar a expressão, é importante que você entenda o que esperar deve corresponder. O sistema Exchange Online pode enviar vários endereços IP, separados por vírgulas. As expressões fornecidas acima funcionarão para isso. No entanto, é importante pensar nisso ao testar suas expressões Regex. Por exemplo, uma delas pode usar a seguinte entrada de exemplo para verificar os exemplos acima: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1 











## <a name="validating-the-deployment"></a>Validando a implantação

### <a name="security-audit-logs"></a>Logs de auditoria de segurança

Para verificar se as novas declarações de contexto de solicitação estão sendo enviadas e estão disponíveis para o AD FS pipeline de processamento de declarações, habilite o log de auditoria no servidor AD FS. Em seguida, envie algumas solicitações de autenticação e verifique os valores de declaração nas entradas do log de auditoria de segurança padrão. 

Para habilitar o log de eventos de auditoria para o log de segurança em um AD FS Server, siga as etapas em configurar a auditoria para AD FS 2,0.

### <a name="event-logging"></a>Registro de eventos em log

Por padrão, as solicitações com falha são registradas no log de eventos do aplicativo localizado em logs de aplicativos e serviços \ AD FS 2,0 \ admin. para obter mais informações sobre o log de eventos para AD FS, consulte [configurar AD FS log de eventos 2,0](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff641696(v=ws.10)).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configurando logs detalhados de rastreamento de AD FS

AD FS eventos de rastreamento são registrados no log de depuração do AD FS 2,0. Para habilitar o rastreamento, consulte [Configurar o rastreamento de depuração para AD FS 2,0](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff641696(v=ws.10)).

Depois de habilitar o rastreamento, use a seguinte sintaxe de linha de comando para habilitar o nível de log detalhado: wevtutil.exe SL "AD FS 2,0 rastreamento/depuração"/l: 5  

## <a name="related"></a>Relacionados
Para obter mais informações sobre os novos tipos de declaração, consulte [AD FS tipos de declarações](AD-FS-Claims-Types.md).
