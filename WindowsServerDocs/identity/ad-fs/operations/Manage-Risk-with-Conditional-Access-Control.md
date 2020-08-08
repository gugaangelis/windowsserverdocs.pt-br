---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: Gerenciar riscos com Controle de Acesso Condicional
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 97466c5f7d0a6c89980195d7b71e6697748db334
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954212"
---
# <a name="manage-risk-with-conditional-access-control"></a>Gerenciar riscos com Controle de Acesso Condicional




-   [Principais conceitos-controle de acesso condicional no AD FS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Gerenciamento de riscos com Controle de Acesso Condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="key-concepts---conditional-access-control-in-ad-fs"></a><a name="BKMK_1"></a>Principais conceitos – controle de acesso condicional no AD FS
A função geral do AD FS é emitir um token de acesso que contenha um conjunto de declarações. A decisão sobre o que as declarações AD FS aceita e os problemas são governadas por regras de declaração.

O controle de acesso no AD FS é implementado com regras de declaração de autorização de emissão que são usadas para emitir uma permissão ou negar declarações que determinarão se um usuário ou grupo de usuários terá permissão para acessar recursos protegidos por AD FS ou não. As regras de autorização só podem ser definidas em objetos de confiança da terceira parte confiável.

|Opção de regras|Lógica de regras|
|---------------|--------------|
|Permitir todos os usuários|Se o tipo de declaração de entrada for igual a *qualquer tipo de declaração* e o valor for igual a *qualquer valor*, emita declarações com valores iguais a *Permitir*|
|Permitir o acesso a usuários com esta declaração de entrada|Se o tipo de declaração de entrada for igual a *tipo de declaração específico* e o valor for igual a *valor de declaração específico*, emita declarações com valores iguais a *Permitir*|
|Negar o acesso a usuários com esta declaração de entrada|Se o tipo de declaração de entrada for igual a *tipo de declaração específico* e o valor for igual a *valor de declaração específico*, emita declarações com valores iguais a *Negar*|

Para obter mais informações sobre a lógica e asa opções dessas regras, consulte [Quando usar uma regra de autorização de declaração](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913560(v=ws.11)).

No AD FS no Windows Server 2012 R2, o controle de acesso é aprimorado com vários fatores, incluindo dados de usuário, dispositivo, local e autenticação. Isso é possibilitado por uma maior variedade de tipos de declaração disponíveis para as regras de declaração de autorização.  Em outras palavras, em AD FS no Windows Server 2012 R2, você pode impor o controle de acesso condicional com base na identidade do usuário ou na associação de grupo, local de rede, dispositivo (independentemente de ser associado ao local de trabalho, para obter mais informações, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e autenticação de segundo fator contínuo em aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)) e o estado de autenticação

O controle de acesso condicional no AD FS no Windows Server 2012 R2 oferece os seguintes benefícios:

-   Políticas de autorização por aplicação flexíveis e expressivas, através das quais é possível permitir ou negar acesso com base em usuário, dispositivo, local de rede e estado de autenticação

-   Criação de regras de autorização de emissão para aplicativos de terceira parte confiável

-   Experiência rica de UI para os cenários comuns de controle de acesso condicional

-   Suporte a Windows PowerShell e linguagem de declarações avançada para cenários avançados de controle de acesso condicional

-   Mensagens de ' acesso negado ' personalizadas (por aplicativo de terceira parte confiável). Para obter mais informações, consulte [Personalizando as páginas de entrada do AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280950(v=ws.11)). Ao conseguir personalizar essas mensagens, é possível explicar por que um usuário está tendo seu acesso negado e também facilitar a atualização de autoatendimento onde é possível, por exemplo, alertando usuários para que ingressem seus dispositivos no local de trabalho. Para obter mais informações, consulte [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

A tabela a seguir inclui todos os tipos de declaração disponíveis em AD FS no Windows Server 2012 R2 a serem usados para implementar o controle de acesso condicional.

|Tipo de declaração|Descrição|
|--------------|---------------|
|Endereço de Email|O endereço de email do usuário.|
|Nome|O nome fornecido do usuário.|
|Nome|O nome exclusivo do usuário.|
|UPN|O UPN (nome principal do usuário) do usuário.|
|Nome comum|O nome comum do usuário.|
|Endereço de email do AD FS 1 x|O endereço de email do usuário ao interoperar com AD FS 1.1 ou AD FS 1.0.|
|Agrupar|Um grupo do qual o usuário é membro.|
|UPN do AD FS 1 x|O UPN do usuário ao interoperar com AD FS 1.1 ou AD FS 1.0.|
|Função|Uma função que o usuário tenha.|
|Sobrenome|O sobrenome do usuário.|
|PPID|O identificador privado do usuário.|
|ID do nome|O identificador de nome SAML do usuário.|
|Carimbo de data/hora da autenticação|Usado para exibir a data e a hora em que o usuário foi autenticado.|
|Método de autenticação|O método usado para autenticar o usuário.|
|SID de grupo somente negar|O SID de grupo somente negar do usuário.|
|SID primário somente negar|O SID primário somente negar do usuário.|
|SID de grupo primário somente negar|O SID de grupo primário somente negar do usuário.|
|SID de grupo|O SID de grupo do usuário.|
|SID de grupo primário|O SID de grupo primário do usuário.|
|SID primário|O SID primário do usuário.|
|Nome da conta Windows|O nome da conta de domínio do usuário sob a forma de domínio\usuário.|
|O usuário está registrado|O usuário está registrado para usar esse dispositivo.|
|Identificador do dispositivo|O identificador do dispositivo.|
|Identificador de registro do dispositivo|Identificador para registro do dispositivo.|
|Nome de exibição do registro do dispositivo|O nome de exibição do registro do dispositivo.|
|Tipo de SO do dispositivo|Tipo do sistema operacional do dispositivo.|
|Versão do SO do dispositivo|A versão do sistema operacional do dispositivo.|
|O dispositivo é gerenciado|O dispositivo é gerenciado por um serviço de gerenciamento.|
|IP do cliente encaminhado|O endereço IP do usuário.|
|Aplicativo cliente|Tipo do aplicativo cliente.|
|Agente de usuário do cliente|Tipo de dispositivo que o cliente está usando para acessar o aplicativo.|
|IP do cliente|Endereço IP do cliente.|
|Caminho do ponto de extremidade|Caminho do ponto de extremidade absoluto que pode ser usado para determinar clientes ativos em relação a passivos.|
|Proxy|Nome DNS do proxy do servidor de federação que passou a solicitação.|
|Identificador do aplicativo|Identificador da terceira parte confiável.|
|Políticas de aplicativo|Diretivas de aplicativo do certificado.|
|Identificador da chave da autoridade|A extensão do identificador da chave da autoridade do certificado que assinou um certificado emitido.|
|Restrição básica|Uma das restrições básicas do certificado.|
|Uso avançado de chave|Descreve um dos usos avançados de chave do certificado.|
|Emissor|O nome da autoridade de certificação que emitiu o certificado X.509.|
|Nome do emissor|O nome diferenciado do emissor do certificado.|
|Uso de chave|Um dos usos de chave do certificado.|
|Não Após|Data, na hora local, depois que um certificado não é mais válido.|
|Não Antes De|A data, na hora local, em que um certificado se torna válido.|
|Políticas de certificado|As políticas sob as quais o certificado foi emitido.|
|Chave pública|A chave pública do certificado.|
|Dados não processados do certificado|Os dados não processados do certificado.|
|Nome alternativo da entidade|Um dos nomes alternativos do certificado.|
|Número de Série|O número de série do certificado.|
|Algoritmo de assinatura|O algoritmo usado para criar a assinatura de um certificado.|
|Assunto|O assunto do certificado.|
|Identificador da chave de assunto|O identificador da chave de assunto do certificado.|
|Nome do assunto|O nome diferenciado do assunto de um certificado.|
|Nome do modelo V2|O nome do modelo do certificado de versão 2 usado durante a emissão ou renovação de um certificado. Este é um valor específico da Microsoft.|
|Nome do modelo V1|O nome do modelo do certificado de versão 1 usado durante a emissão ou renovação de um certificado. Este é um valor específico da Microsoft.|
|Impressão digital|Impressão digital do certificado.|
|Versão X 509|A versão do formato X.509 do certificado.|
|Dentro da Rede Corporativa|Usado para indicar se uma solicitação originou-se de dentro da rede corporativa.|
|Hora da Expiração da Senha|Usado para exibir a hora quando a senha expira.|
|Dias de expiração da senha|Usado para exibir o número de dias para a expiração da senha.|
|URL de atualização da senha|Usado para exibir o endereço Web do serviço de atualização da senha.|
|Referências de métodos de autenticação|Usado para indicar todos os métodos de autenticação usados para autenticar o usuário.|

## <a name="managing-risk-with-conditional-access-control"></a><a name="BKMK_2"></a>Gerenciamento de riscos com Controle de Acesso Condicional
Usando as configurações disponíveis, há muitas maneiras de gerenciar o risco implementando controle de acesso condicional.

### <a name="common-scenarios"></a>Cenários comuns
Por exemplo, imagine um cenário simples de implementação do controle de acesso condicional com base nos dados de associação de grupo do usuário para um aplicativo específico (confiança de terceira parte confiável). Em outras palavras, você pode configurar uma regra de autorização de emissão no servidor de Federação para permitir que os usuários que pertencem a um determinado grupo em seu domínio do AD acessem um aplicativo específico protegido pelo AD FS.  As instruções passo a passo detalhadas (usando a interface do usuário e o Windows PowerShell) para implementar esse cenário são abordadas em [Walkthrough Guide: Manage Risk with Conditional Access Control](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Para concluir as etapas neste passo a passos, você deve configurar um ambiente de laboratório e seguir as etapas em [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Cenários avançados
Outros exemplos de implementação do controle de acesso condicional no AD FS no Windows Server 2012 R2 incluem o seguinte:

-   Permitir acesso a um aplicativo protegido por AD FS somente se a identidade desse usuário tiver sido validada com MFA

    É possível usar o seguinte código:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir acesso a um aplicativo protegido por AD FS somente se a solicitação de acesso for proveniente de um dispositivo ingressado no local de trabalho que está registrado para o usuário

    É possível usar o seguinte código:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir o acesso a um aplicativo protegido por AD FS somente se a solicitação de acesso for proveniente de um dispositivo ingressado no local de trabalho que está registrado para um usuário cuja identidade foi validada com MFA

    É possível usar o seguinte código

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir acesso de extranet a um aplicativo protegido por AD FS somente se a solicitação de acesso for proveniente de um usuário cuja identidade tenha sido validada com MFA.

    É possível usar o seguinte código:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Consulte Também
[Guia de instruções: gerenciar riscos com o controle](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md) 
 de acesso condicional [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
