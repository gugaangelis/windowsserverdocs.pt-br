---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: Gerenciar os riscos com controle de acesso condicional
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2ad7d1467abd6d69077b515b8c69a65f7e70f19
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-conditional-access-control"></a>Gerenciar os riscos com controle de acesso condicional

>Aplica-se a: Windows Server 2012 R2


-   [Controle de acesso condicional conceitos no AD FS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Gerenciamento de risco com controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>Principais conceitos - controle de acesso condicional no AD FS
A função do AD FS geral é emitir um token de acesso que contém um conjunto de declarações. A decisão sobre quais declarações do AD FS aceita e depois, emite é regida pelas regras de declaração.

Controle de acesso no AD FS é implementado com regras de declaração de autorização de emissão que são usadas para emitir uma permissão ou negação requerimentos judiciais ou Extrajudiciais determinarão se um usuário ou um grupo de usuários terão permissão para acessar recursos do AD FS protegido ou não. Regras de autorização só podem ser definidas na terceira relações de confiança de terceiros.

|Opção de regra|Lógica de regra|
|---------------|--------------|
|Permitir que todos os usuários|Se for igual a entrada tipo de declaração *qualquer reivindicação tipo* e o valor é igual a *qualquer valor*, em seguida, o problema reivindicar com o valor é igual a *permitir*|
|Permitir o acesso aos usuários com essa declaração de entrada|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *especificado reivindicação valor*, em seguida, o problema reivindicar com o valor é igual a *permitir*|
|Negar acesso aos usuários com essa declaração de entrada|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *especificado reivindicação valor*, em seguida, o problema reivindicar com o valor é igual a *negar*|

Para saber mais sobre essas opções de regra e lógica, consulte [quando usar uma regra de declaração de autorização](https://technet.microsoft.com/library/ee913560.aspx).

No AD FS no Windows Server 2012 R2, o controle de acesso é avançado com diversos fatores, incluindo o usuário, dispositivo, localização e dados de autenticação. Isso se torna possível por uma variedade maior de tipos de declaração disponível para as regras de declaração de autorização.  Em outras palavras, no AD FS no Windows Server 2012 R2, você pode impor o controle de acesso condicional com base na participação em grupo ou identidade do usuário, o local de rede, o dispositivo (se ele é o local de trabalho ingressado, para obter mais informações, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)) e o estado de autenticação (se a autenticação multifator (MFA) foi executada).

Controle de acesso condicional do AD FS em Windows Server 2012 R2, oferece os seguintes benefícios:

-   Políticas de autorização por aplicativo flexível e expressivo, por meio do qual você pode permitir ou negar acesso com base em usuário, o dispositivo, o local de rede e o estado de autenticação

-   Criação de regras de autorização para aplicativos de terceiros confiante de emissão

-   Experiência de Rich UI para os cenários comuns de controle de acesso condicional

-   Suportam a linguagem avançada requerimentos judiciais ou Extrajudiciais & Windows PowerShell para cenários de controle de acesso condicional avançada

-   Personalizado (por confiar aplicativo de terceiros) mensagens 'Acesso negado'. Para obter mais informações, consulte [Personalizando as páginas do AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx). Por poder personalizar essas mensagens, você pode explicar por que um usuário está sendo acesso negado e também facilitam a correção de autoatendimento onde é possível, por exemplo, os usuários prompt para área de trabalho ingressar em seus dispositivos. Para obter mais informações, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

A tabela a seguir inclui todos os tipos de declaração disponíveis no AD FS no Windows Server 2012 R2 para ser usado para a implementação de controle de acesso condicional.

|Tipo de declaração|Descrição|
|--------------|---------------|
|Endereço de email|O endereço de email do usuário.|
|Nome determinado|O nome determinado do usuário.|
|Nome|O nome exclusivo do usuário,|
|UPN|O UPN (UPN) do usuário.|
|Nome comum|O nome comum do usuário.|
|AD FS 1 x o endereço de email|O endereço de email do usuário quando interoperar com o AD FS 1.1 ou AD FS 1.0.|
|Grupo|Um grupo que o usuário é um membro do.|
|AD FS 1 x UPN|O UPN do usuário quando interoperar com o AD FS 1.1 ou AD FS 1.0.|
|Função|Uma função que o usuário tem.|
|Sobrenome|O sobrenome do usuário.|
|PPID|O identificador privado do usuário.|
|ID do nome|O identificador SAML do nome do usuário.|
|Carimbo de data / hora de autenticação|Usado para exibir a hora e data que o usuário foi autenticado.|
|Método de autenticação|O método usado para autenticar o usuário.|
|Negar único grupo SID|Somente negação grupo SID do usuário.|
|Negar apenas SID principal|O somente negação principal SID do usuário.|
|Negar somente grupo primário SID|A principal somente negação agrupar SID do usuário.|
|Grupo SID|O grupo SID do usuário.|
|Grupo primário SID|O grupo primário SID do usuário.|
|SID principal|O SID principal do usuário.|
|Nome de conta do Windows|O nome da conta de domínio do usuário na forma de domínio \ usuário.|
|É registrado do usuário|Usuário está registrado para usar este dispositivo.|
|Identificador de dispositivo|Identificador do dispositivo.|
|Identificador de registro do dispositivo|Identificador de registro de dispositivo.|
|Nome de exibição de registro do dispositivo|Nome de exibição do registro de dispositivo.|
|Tipo de sistema operacional do dispositivo|Tipo de sistema operacional do dispositivo.|
|Versão do sistema operacional de dispositivo|Versão do sistema operacional do dispositivo.|
|É dispositivo gerenciado|Dispositivo é gerenciado por um serviço de gerenciamento.|
|Encaminhado cliente IP|Endereço IP do usuário.|
|Aplicativo cliente|Tipo do aplicativo cliente.|
|Agente de usuário do cliente|Tipo de dispositivo do cliente está usando para acessar o aplicativo.|
|Cliente de IP|Endereço IP do cliente.|
|Caminho de ponto de extremidade|Caminho absoluto do ponto de extremidade que pode ser usado para determinar o ativo em relação a clientes passivos.|
|Proxy|Nome DNS do proxy do servidor de Federação passado a solicitação.|
|Identificador de aplicativo|Identificador para o terceiro.|
|Políticas de aplicativo|Políticas de aplicativos do certificado.|
|Identificador da chave da autoridade|A extensão de identificador da chave da autoridade do certificado assinado um certificado emitido.|
|Restrição básica|Uma das restrições básicas do certificado.|
|Uso avançado de chave|Descreve um dos usos avançados de chave do certificado.|
|Emissor|O nome da autoridade de certificação que emitiu o certificado x. 509.|
|Nome de emissor|O nome diferenciado do emissor do certificado.|
|Uso de chave|Um dos usos de chave do certificado.|
|Não após|Data no horário local após o qual um certificado não é mais válido.|
|Não antes|A data no horário local em que um certificado se torna válido.|
|Políticas de certificado|As políticas sob as quais o certificado foi emitido.|
|Chave pública|Chave pública do certificado.|
|Dados brutos de certificado|Os dados brutos do certificado.|
|Nome de entidade alternativo|Um dos nomes alternativos do certificado.|
|Número de série|O número de série do certificado.|
|Algoritmo de assinatura|O algoritmo usado para criar a assinatura de um certificado.|
|Assunto|O assunto do certificado.|
|Identificador da chave da entidade|O identificador da chave da entidade do certificado.|
|Nome do requerente|O nome diferenciado assunto de um certificado.|
|V2 Nome do modelo|O nome do modelo de certificado 2 versão usada quando a emissão ou renovando um certificado. Esse é um valor específico da Microsoft.|
|V1 Nome do modelo|O nome do modelo de certificado de versão 1 usado quando a emissão ou renovando um certificado. Esse é um valor específico da Microsoft.|
|Impressão digital|Impressão digital do certificado.|
|X 509 versão|A versão de formato x. 509 do certificado.|
|Rede corporativa interna|Usado para indicar se uma solicitação originados dentro da rede corporativa.|
|Tempo de expiração de senha|Usado para exibir a hora em que a senha expira.|
|Dias de expiração de senha|Usado para exibir o número de dias de expiração de senha.|
|Atualizar a URL de senha|Usado para exibir o endereço web do serviço de atualização de senha.|
|Referências de métodos de autenticação|Usado para indicar al métodos de autenticação usados para autenticar o usuário.|

## <a name="BKMK_2"></a>Gerenciamento de risco com controle de acesso condicional
Usando as configurações disponíveis, há muitas maneiras em que você pode gerenciar o risco com a implementação de controle de acesso condicional.

### <a name="common-scenarios"></a>Cenários comuns
Por exemplo, imagine um cenário simples de implementar o controle de acesso condicional com base nos dados de associação de grupo do usuário para um aplicativo específico (terceira confiança de terceiros). Em outras palavras, você pode configurar uma regra de autorização de emissão em seu servidor de federação para permitir que os usuários que pertencem a um determinado grupo em seu anúncio acesso ao domínio para um aplicativo específico que está protegido pelo AD FS.  Passo a passo instruções detalhadas (usando a interface do usuário e do Windows PowerShell) para implementar esse cenário são abordadas em [guia passo a passo: gerenciar o risco com controle de acesso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Para concluir as etapas neste passo a passo, você deve configurar um ambiente de laboratório e siga as etapas em [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Cenários avançados
Outros exemplos de implementação de controle de acesso condicional do AD FS em Windows Server 2012 R2 incluem o seguinte:

-   Permitir acesso a um aplicativo protegido pelo AD FS somente se a identidade do usuário foi validada com MFA

    Você pode usar o código a seguir:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir acesso a um aplicativo protegido pelo AD FS somente se a solicitação de acesso é proveniente de um dispositivo associado ao local de trabalho que está registrado para o usuário

    Você pode usar o código a seguir:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir acesso a um aplicativo protegido pelo AD FS somente se a solicitação de acesso é proveniente de um dispositivo associado ao local de trabalho que está registrado para um usuário cuja identidade foi validada com MFA

    Você pode usar o código a seguir

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir acesso extranet a um aplicativo protegido pelo AD FS somente se a solicitação de acesso é proveniente de um usuário cuja identidade foi validada com MFA.

    Você pode usar o código a seguir:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Consulte também
[Guia passo a passo: Gerenciar o risco com controle de acesso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)<ph x="2">
</ph>[configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



