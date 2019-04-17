---
ms.assetid: 
title: "Políticas de controle de acesso do cliente no Active Directory federação Services 2.0"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00b9cd17c7a5c206e06bea12fd90762adb336715
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Políticas de controle de acesso do cliente no AD FS 2.0
Um políticas de acesso do cliente no Active Directory federação Services 2.0 permitem que você restringir ou conceder acesso de usuários aos recursos.  Este documento descreve como habilitar a políticas de acesso do cliente no AD FS 2.0 e como configurar os cenários mais comuns.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Habilitar a política de acesso do cliente no AD FS 2.0

Para habilitar a política de acesso do cliente, siga as etapas abaixo.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Etapa 1: Instalar a atualização de Rollup 2 para o AD FS 2.0 empacotar em seus servidores AD FS

Baixe o [Update Rollup 2 para os serviços de Federação do Active Directory (AD FS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) empacotar e instalá-lo em todos os servidores de Federação e proxies de servidor de Federação.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Etapa 2: Adicionar que cinco reivindicar regras para a relação de confiança do provedor de declarações do Active Directory

Depois que a atualização de Rollup 2 tiver sido instalado em todos os servidores do AD FS e proxies, use o procedimento a seguir para adicionar um conjunto de regras de declarações que disponibiliza novos tipos de declaração para o mecanismo de política.

Para fazer isso, você adicionará cinco regras de transformação de aceitação para cada um dos novos tipos de declaração de contexto solicitação usando o procedimento a seguir.

Na confiança do provedor de declarações do Active Directory, crie uma nova regra de transformação de aceitação para passar por meio de cada um dos novos tipos de declaração de contexto de solicitação.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Para adicionar uma regra de declaração no Active Directory requerimentos judiciais ou Extrajudiciais provedor confiança para cada contexto de cinco tipos de declaração:


1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS 2.0 gerenciamento.
2. Na árvore de console, em AD FS 2.0\Trust relações, clique em relações de confiança do provedor de declarações, clique com botão direito do Active Directory e, em seguida, clique em Editar regras de declaração.
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de transformação de aceitação e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra.
4. Na página Selecionar modelo de regra, sob o modelo de regra de declaração, selecione passagem ou filtrar uma declaração de entrada na lista e clique em Avançar.
5. Na página Configurar regra, em nome da regra reivindicação, digite o nome de exibição para essa regra; em entrada tipo de declaração, digite a URL de tipo de declaração a seguir e, em seguida, selecione Pass por meio de todos os valores de declaração.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Para verificar a regra, selecione-o na lista, clique em Editar regra e clique em idioma de regra de modo de exibição. O idioma de regra reivindicação deverá aparecer como segue: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Clique em Concluir.
8. Na caixa de diálogo Editar reivindicação regras, clique em Okey para salvar as regras.
9. Repita as etapas 2 a 6 para criar uma regra de declaração adicionais para cada um dos tipos de quatro declaração restantes mostrados abaixo até que todas as cinco regras foram criadas.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Etapa 3: Atualizar a plataforma de identidade do Microsoft Office 365 dependência confiança de terceiros

Escolha um dos cenários de exemplo a seguir para configurar as regras de declaração sobre a plataforma de identidade do Microsoft Office 365 dependência confiança de terceiros que melhor atenda às necessidades da sua organização.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Cenários de política de acesso do cliente do AD FS 2.0
As seções a seguir descreve os cenários existentes para AD FS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Cenário 1: Bloquear todos os acessos externo para o Office 365

Esse cenário de política de acesso do cliente permite o acesso de todos os clientes internos e blocos de todos os clientes externos com base no endereço IP do cliente externo. O conjunto de regras complementa a regra de autorização de emissão padrão permitir acesso a todos os usuários. Você pode usar o procedimento a seguir para adicionar uma regra de emissão autorização para o Office 365 dependência confiança de terceiros.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para criar uma regra para bloquear todos os acessos externo para o Office 365



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS 2.0 gerenciamento.
2. Na árvore de console, em AD FS 2.0\Trust relações, clique em confie dependência de terceiros, clique com botão direito a relação de confiança de plataforma de identidade do Microsoft Office 365 e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, sob o modelo de regra de declaração, selecione Enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra reivindicação, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo permitir acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar reivindicação regras, clique em Okey.

>[!NOTE]
>Você terá que substituir o valor acima para "regex de endereço ip pública" por uma expressão de IP válida; Veja criando a expressão de intervalo de endereços IP para obter mais informações.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Cenário 2: Bloquear todos os acessos externo para o Office 365, exceto do Exchange ActiveSync

O exemplo a seguir permite o acesso a todos os aplicativos do Office 365, inclusive Exchange Online, de clientes internos, incluindo Outlook. Ele bloqueia o acesso de clientes localizados fora da rede corporativa, conforme indicado pelo endereço IP do cliente, exceto para os clientes do Exchange ActiveSync, como Smartphones. O conjunto de regras complementa a regra de autorização de emissão padrão intitulada permitir acesso a todos os usuários. Use as seguintes etapas para adicionar uma regra de emissão autorização para o Office 365 dependência usando o Assistente de regra de declaração de confiança de terceiros:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Para criar uma regra para bloquear todos os acessos externo para o Office 365



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS 2.0 gerenciamento.
2. Na árvore de console, em AD FS 2.0\Trust relações, clique em confie dependência de terceiros, clique com botão direito a relação de confiança de plataforma de identidade do Microsoft Office 365 e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, sob o modelo de regra de declaração, selecione Enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra reivindicação, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo permitir acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar reivindicação regras, clique em Okey.

>[!NOTE]
>Você terá que substituir o valor acima para "regex de endereço ip pública" por uma expressão de IP válida; Veja criando a expressão de intervalo de endereços IP para obter mais informações.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Cenário 3: Bloquear todos os acessos externo para o Office 365 exceto aplicativos baseados em navegador

O conjunto de regras complementa a regra de autorização de emissão padrão intitulada permitir acesso a todos os usuários. Use as seguintes etapas para adicionar uma regra de autorização de emissão para a plataforma de identidade do Microsoft Office 365 dependência usando o Assistente de regra de declaração de confiança de terceiros:

>[!NOTE]
>Não há suporte para esse cenário com um proxy de terceiros devido a limitações em cabeçalhos de política de acesso do cliente com solicitações passivos (com base na Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Para criar uma regra para bloquear o acesso externo para o Office 365 exceto aplicativos baseados em navegador



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS 2.0 gerenciamento.
2. Na árvore de console, em AD FS 2.0\Trust relações, clique em confie dependência de terceiros, clique com botão direito a relação de confiança de plataforma de identidade do Microsoft Office 365 e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, sob o modelo de regra de declaração, selecione Enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra reivindicação, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo permitir acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar reivindicação regras, clique em Okey.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Cenário 4: Bloquear todos os acessos externo para o Office 365 para designado grupos do Active Directory

O exemplo a seguir permite acesso de clientes internos com base no endereço IP. Ele bloqueia o acesso de clientes localizados fora da rede corporativa que possuem um endereço IP do cliente externo, exceto para as pessoas em uma regra do Active Directory Group.The especificada conjunto complementa a regra de autorização de emissão padrão intitulada permitir acesso a todos os usuários. Use as seguintes etapas para adicionar uma regra de autorização de emissão para a plataforma de identidade do Microsoft Office 365 dependência usando o Assistente de regra de declaração de confiança de terceiros:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Para criar uma regra para bloquear todos os acessos externo para o Office 365 para designado grupos do Active Directory



1. Clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em AD FS 2.0 gerenciamento.
2. Na árvore de console, em AD FS 2.0\Trust relações, clique em confie dependência de terceiros, clique com botão direito a relação de confiança de plataforma de identidade do Microsoft Office 365 e, em seguida, clique em Editar regras de declaração. 
3. Na caixa de diálogo Editar regras de declaração, selecione a guia regras de autorização de emissão e, em seguida, clique em Adicionar regra para iniciar o Assistente de regra de declaração.
4. Na página Selecionar modelo de regra, sob o modelo de regra de declaração, selecione Enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada e clique em Avançar.
5. Na página Configurar regra, em nome da regra reivindicação, digite o nome de exibição para essa regra. Em regra personalizada, digite ou cole a seguinte sintaxe de linguagem de regra de declaração: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Clique em Concluir. Verifique se a nova regra aparece imediatamente abaixo permitir acesso a regra de todos os usuários na lista de regras de autorização de emissão.
7. Para salvar a regra, na caixa de diálogo Editar reivindicação regras, clique em Okey.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descrições da declaração de regra de sintaxe da linguagem usada nos cenários acima

|Descrição|Declaração de sintaxe da linguagem de regra|
|-----|-----| 
|Padrão AD FS regra permitir acesso a todos os usuários. Essa regra deve existir na plataforma de identidade do Microsoft Office 365 contar a lista de regras de autorização de emissão de confiança de terceiros.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/permit", valor = "true");| 
|Adicionar esta cláusula a uma regra de nova e personalizada Especifica que a solicitação tem provenientes do proxy do servidor de Federação (isto é, ele tem o cabeçalho x-ms-proxy)
É recomendável que todas as regras de incluem-lo.|Existe ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"])| 
|Usado para estabelecer que a solicitação é de um cliente com um IP no intervalo aceitável definido.|NÃO existe ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", valor = ~ "regex de endereço ip pública fornecida pelo cliente"])| 
|Esta cláusula é usada para especificar se o aplicativo está sendo acessado não for Microsoft.Exchange.ActiveSync a solicitação deve ser negada.|NÃO existe ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value=="Microsoft.Exchange.ActiveSync"])| 
|Essa regra permite que você determinar se a chamada foi por meio de um navegador da Web e não será negada.|NÃO existe ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", valor = = "/ adfs/ls /"])| 
|Essa regra estados que os únicos usuários em um grupo específico do Active Directory (com base no valor do SID) devem ser negados. Adicionando não a esta declaração significa que um grupo de usuários será permitido, independentemente do local.|Existe ([tipo = = "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", valor = ~ "{grupo valor SID de grupo do AD permitido}"])| 
|Isso é uma cláusula necessária para emitir uma negação quando todas as condições acima são atendidas.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/deny", valor = "true");|

### <a name="building-the-ip-address-range-expression"></a>Criando a expressão de intervalo de endereços IP

A declaração de x-ms-encaminhado-client-ip é preenchida com um cabeçalho HTTP que atualmente é definido pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da reivindicação pode ser um destes procedimentos:

>[!Note] 
>Exchange Online atualmente dá suporte apenas endereços IPV4 e IPV6 não.

Um único endereço IP: O endereço IP do cliente que está conectado diretamente ao Exchange Online

>[!Note] 
>O endereço IP de um cliente na rede corporativa aparecerá como o endereço IP da interface externa do proxy de saída da organização ou gateway.

Os clientes que estão conectados à rede corporativa por uma VPN ou pela Microsoft DirectAccess (DA) podem aparecer como clientes corporativos internos ou externos clientes dependendo da configuração de VPN ou DA.

Um ou mais endereços IP: ao Exchange Online não pode determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor do cabeçalho x-encaminhado-para, um cabeçalho não padrão que pode ser incluído em solicitações de HTTP e é compatível com muitos clientes, balanceadores de carga e proxies do mercado.

>[!Note]
>Vários endereços IP, indicando que o endereço IP do cliente e o endereço de cada proxy que passado a solicitação serão ser separados por vírgula.

Endereços IP relacionada à infraestrutura Exchange Online não aparecerá na lista.


#### <a name="regular-expressions"></a>Expressões regulares

Quando você tem que corresponder a um intervalo de endereços IP, é necessário construir uma expressão regular para executar a comparação. Na próxima série de etapas, fornecemos exemplos de como construir uma expressão como essa para coincidir com os seguintes intervalos de endereços (Observe que você precisará alterar esses exemplos para corresponder o intervalo IP público):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

Primeiro, o padrão básico que corresponderá a um único endereço IP é o seguinte: \b###\.###\.###\.###\b

Estendendo isso, podemos combinar dois endereços IP com uma expressão ou da seguinte forma: \b###\.###\.###\.###\b|\b###\.###\.###\.###\b

Portanto, um exemplo para corresponder a apenas dois endereços (como 192.168.1.1 ou 10.0.0.1) seria: \b192\.168\.1\.1\b|\b10\.0\.0\.1\b

Isso lhe dá a técnica pela qual você pode inserir qualquer número de endereços. Quando um intervalo de endereços precisa permitido, por exemplo 192.168.1.1 – 192.168.1.25, a correspondência deve ser feita apenas caractere em caractere: \b192\.168\.1\. ([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>O endereço IP é tratado como cadeia de caracteres e não é um número.


A regra é dividida da seguinte forma: \b192\.168\.1\.

Isso corresponde a qualquer 192.168.1 a partir do valor.

O seguinte corresponde os intervalos necessários para a parte do endereço após o ponto decimal final:


- ([1-9] correspondências endereços terminados em 1-9
- | 1 [0-9] corresponde endereços terminando em 10 19
- Endereços de correspondências |2[0-5]) terminados em 20-25

>[!Note]
>Os parênteses devem ser posicionados corretamente, para que você não iniciar a correspondência com outras partes do endereços IP.

Com o bloco 192 correspondido, podemos escrever uma expressão semelhante para o bloco de 10: \b10\.0\.0\. ([1-9] | 1 [0-4]) \b

E colocá-los juntos, a seguinte expressão deve corresponder a todos os endereços de "192.168.1.1~25" e "10.0.0.1~14": \b192\.168\.1\. ([1-9]|1[0-9]|2[0-5])\b|\b10\.0\.0\. ([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>Testando a expressão

Regex expressões podem ficar bem complicadas, portanto, é altamente recomendável usando uma ferramenta de verificação de regex. Se você fizer uma pesquisa na internet para "construtor de expressão regex online", você encontrará vários utilitários online BOM que permitirá que você experimente suas expressões contra dados de amostra.

Ao testar a expressão, é importante entender o que esperar para deve ser correspondido. O sistema de online do Exchange pode enviar vários endereços IP, separados por vírgulas. As expressões fornecidas acima funcionará para isso. No entanto, é importante pensar ao testar suas expressões de regex. Por exemplo, um pode usar o exemplo a seguir para verificar os exemplos acima de entrada: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>Validando a implantação

### <a name="security-audit-logs"></a>Logs de auditoria de segurança

Para verificar que o novo contexto de solicitação requerimentos judiciais ou Extrajudiciais estão sendo enviados e estão disponíveis para o AD FS declarações do pipeline de processamento, habilite auditoria para fazer logon no servidor do AD FS. Envie algumas solicitações de autenticação e verificação para os valores de declaração nas entradas de log de auditoria de segurança padrão. 

Para habilitar o registro em log de auditoria de registro de eventos para a segurança em um servidor do AD FS, siga as etapas em Configurar auditoria do AD FS 2.0.

### <a name="event-logging"></a>Log de eventos

Por padrão, as solicitações com falha são registradas no log de eventos do aplicativo localizado em Logs de aplicativos e serviços \ AD FS 2.0 \ Admin.For obter mais informações no log de eventos do AD FS, consulte [configurar o AD FS 2.0 log de eventos](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configurando detalhada AD FS Logs de rastreamento

AD FS rastreamento eventos são registrados no log de depuração 2.0 do AD FS. Para habilitar o rastreamento, consulte [configurar o rastreamento de depuração do AD FS 2.0](https://technet.microsoft.com/en-us/library/adfs2-troubleshooting-configuring-computers.aspx).

Depois que você tiver ativado o rastreamento, use a seguinte sintaxe de linha de comando para habilitar o nível de log detalhado: wevtutil.exe sl "AD FS 2.0 rastreamento/depuração" /l: 5  

## <a name="related"></a>Relacionados
Para obter mais informações sobre os novos tipos de declaração veja [tipos de declarações do AD FS](AD-FS-Claims-Types.md).
 
