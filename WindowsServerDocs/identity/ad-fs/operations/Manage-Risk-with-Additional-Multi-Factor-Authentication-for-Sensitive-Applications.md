---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: gerencie riscos com Multi-Factor Authentication adicional para aplicativos confidenciais
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a37c0b0a1de06b65e0cb867ec4d160bbb0373a15
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189066"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>gerencie riscos com Multi-Factor Authentication adicional para aplicativos confidenciais




-   [Configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurar métodos de autenticação adicionais do AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>Neste guia
Esse guia contém as seguintes informações:

-   [Mecanismos de autenticação no AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -descrição dos mecanismos de autenticação disponíveis no Active Directory Federation Services (AD FS) no Windows Server 2012 R2

-   [Visão geral do cenário](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) -uma descrição de um cenário onde você usa os serviços de Federação do Active Directory (AD FS) para habilitar a autenticação multifator (MFA) com base na associação de grupo do usuário.

    > [!NOTE]
    > No AD FS no Windows Server 2012 R2, você pode habilitar o MFA com base no local de rede, identidade do dispositivo e associação de grupo ou identidade do usuário.

    Para obter instruções detalhadas passo a passo para configurar e verificar esse cenário, consulte [guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Principais conceitos - mecanismos de autenticação no AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Benefícios dos mecanismos de autenticação no AD FS
Serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2 fornece aos administradores de TI com um conjunto de ferramentas mais avançado, mais flexível para autenticar os usuários que desejam acessar recursos corporativos. Ele capacita os administradores com controle flexível sobre o primário e os métodos de autenticação adicionais, oferece uma experiência avançada de gerenciamento para configurar a autenticação de políticas (por meio da interface do usuário e do Windows PowerShell) e aprimora a experiência para os usuários finais que acessam aplicativos e serviços que são protegidos pelo AD FS. A seguir está alguns dos benefícios de proteger seus aplicativos e serviços com o AD FS no Windows Server 2012 R2:

-   Política de autenticação global – um recurso de gerenciamento central, no qual um administrador de TI pode escolher quais métodos de autenticação são usados para autenticar usuários com base no local de rede do qual eles acessam recursos protegidos. Isso permite que os administradores façam o seguinte:

    -   Impor o uso de métodos de autenticação mais seguros para solicitações de acesso da extranet.

    -   Habilitar a autenticação de dispositivos para autenticação de dois fatores contínua. Isso vincula a identidade do usuário para o dispositivo registrado que é usado para acessar o recurso, oferecendo assim a verificação de identidade composta mais segura antes que recursos protegidos sejam acessados.

        > [!NOTE]
        > Para obter mais informações sobre o objeto de dispositivo, serviço de registro de dispositivo, ingresso no local e o dispositivo como autenticação de dois fatores contínua e SSO, consulte [ingresse no local de trabalho de qualquer dispositivo para SSO e autenticação de dois fatores contínua Em aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Definir o requisito de MFA para todos os acessos extranet ou condicionalmente com base na identidade do usuário, local de rede ou um dispositivo que é usado para acessar recursos protegidos.

-   Maior flexibilidade na configuração de políticas de autenticação: você pode configurar políticas de autenticação personalizadas para recursos do AD FS-protegido com diferentes valores de negócio. Por exemplo, é possível exigir MFA para aplicativo com alto impacto nos negócios.

-   Facilidade de uso: ferramentas de gerenciamento simples e intuitivas, como o snap-in baseado em GUI MMC de gerenciamento do AD FS e os cmdlets do Windows PowerShell permitem que os administradores de TI configurem políticas de autenticação com relativa facilidade. Com o Windows PowerShell, é possível fazer scripts de suas soluções para usar em escala e para automatizar tarefas administrativas rotineiras.

-   Maior controle sobre os ativos corporativos: já que como um administrador, você pode usar o AD FS para configurar uma política de autenticação que se aplica a um recurso específico, você tem maior controle sobre como os recursos corporativos estão protegidos. Os aplicativos não podem substituir as políticas de autenticação especificadas por administradores de TI. Para serviços e aplicativos confidenciais, é possível habilitar a exigência de MFA, a autenticação de dispositivos e, opcionalmente, uma autenticação nova toda vez que o recurso for acessado.

-   Suporte para provedores personalizados de MFA: para organizações que utilizam métodos de MFA de terceiros, o AD FS oferece a capacidade de incorporar e usar esses métodos de autenticação sem problemas.

### <a name="authentication-scope"></a>Escopo de autenticação
No AD FS no Windows Server 2012 R2, você pode especificar uma política de autenticação em um escopo global que é aplicável a todos os aplicativos e serviços que são protegidos pelo AD FS.  Você também pode definir políticas de autenticação para aplicativos específicos e serviços (objetos de confiança da terceira parte confiável) que são protegidos pelo AD FS. Especificar uma política de autenticação para um aplicativo específico (por objeto de confiança da terceira parte confiável) não substitui a política de autenticação global. Se a política de autenticação global ou por objeto de confiança da terceira parte confiável exigir MFA, a MFA será disparada quando o usuário tentar fazer autenticação a esse objeto de confiança da terceira parte confiável.  A política de autenticação global é um fallback para objetos de confiança da terceira parte confiável (aplicativos e serviços) que não têm uma política específica de autenticação configurada.

Uma política de autenticação global se aplica a todas as terceiras partes confiáveis que são protegidas pelo AD FS. É possível configurar as seguintes definições como parte da política de autenticação global:

-   Métodos de autenticação a serem usados para autenticação principal

-   Definições e métodos para a MFA

-   Se a autenticação de dispositivo está habilitada. Para obter mais informações, consulte [ingresse no local de trabalho de qualquer dispositivo para SSO e contínuo segundo fator de autenticação em aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

As políticas de autenticação por objeto de confiança da terceira parte confiável aplicam-se especificamente às tentativas de acessar esse objeto de confiança da terceira parte confiável (aplicativo ou serviço). É possível configurar as seguintes definições como parte da política de autenticação por objeto de confiança da terceira parte confiável:

-   Se os usuários são obrigados a fornecer suas credenciais cada vez que entram

-   As configurações de MFA baseiam-se em usuário/grupo, registro de dispositivos e dados de localização de solicitação de acesso

### <a name="primary-and-additional-authentication-methods"></a>Métodos de autenticação principal e adicionais
Com o AD FS no Windows Server 2012 R2, além do mecanismo de autenticação primária, os administradores podem configurar métodos de autenticação adicionais. Métodos de autenticação principal são internos e destinam-se para validar as identidades de usuários. Você pode configurar fatores de autenticação adicionais para solicitar que obter mais informações sobre a identidade do usuário são fornecidas e, consequentemente, garantir uma autenticação mais forte.

Com a autenticação principal no AD FS no Windows Server 2012 R2, você tem as seguintes opções:

-   Para recursos publicados para serem acessados de fora da rede corporativa, a Autenticação de Formulários é selecionada por padrão. Além disso, também é possível habilitar a Autenticação de Certificado (em outras palavras, a autenticação baseada em cartão inteligente ou autenticação de certificado do cliente do usuário que funciona com o AD DS).

-   Para os recursos de intranet, a autenticação do Windows está selecionada por padrão. Além disso, você também pode habilitar Autenticação de Certificado e/ou Formulários.

Ao selecionar mais de um método de autenticação, você habilita seus usuários a terem uma escolha de qual método com o qual autenticar para a página de entrada para o aplicativo ou serviço.

Você habilita também a autenticação de dispositivos para autenticação de dois fatores contínua. Isso vincula a identidade do usuário para o dispositivo registrado que é usado para acessar o recurso, oferecendo assim a verificação de identidade composta mais segura antes que recursos protegidos sejam acessados.

> [!NOTE]
> Para obter mais informações sobre o objeto de dispositivo, serviço de registro de dispositivo, ingresso no local e o dispositivo como autenticação de dois fatores contínua e SSO, consulte [ingresse no local de trabalho de qualquer dispositivo para SSO e autenticação de dois fatores contínua Em aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Se você especificar o método de autenticação do Windows (opção padrão) para seus recursos de intranet, as solicitações de autenticação serão submetidas a esse método sem problemas em navegadores que oferecem suporte a autenticação do Windows.

> [!NOTE]
> A autenticação do Windows não é compatível com todos os navegadores. O mecanismo de autenticação no AD FS no Windows Server 2012 R2 detecta o agente do usuário de navegador do usuário e usa uma definição configurável para determinar se o agente de usuário dá suporte à autenticação do Windows. Os administradores podem adicionar a essa lista de agentes do usuário (através do Windows PowerShell) o comando `Set-AdfsProperties -WIASupportedUserAgents`, a fim de especificar cadeias de caracteres de agente de usuário alternativas para navegadores que oferecem suporte à autenticação do Windows. Se o agente do usuário do cliente não oferece suporte a autenticação do Windows, o método de fallback padrão é a autenticação de formulários.

### <a name="configuring-mfa"></a>Configuração da MFA
Há duas partes para configurar a MFA no AD FS no Windows Server 2012 R2: especificar as condições sob as quais a MFA é necessária e selecionar um método de autenticação adicional. Para obter mais informações sobre métodos de autenticação adicionais, consulte [configurar métodos de autenticação adicionais para o AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Configurações do MFA**

As seguintes opções estão disponíveis para configurações de MFA (condições sob as quais exigir a MFA):

-   É possível exigir MFA para usuários e grupos específicos no domínio do AD a qual o servidor de federação está ingressado.

-   É possível exigir MFA para dispositivos registrados (ingressados no local de trabalho) ou não registrados (não ingressados no local de trabalho).

    Windows Server 2012 R2 utiliza uma abordagem centrada no usuário para dispositivos modernos em que os objetos de dispositivo representam uma relação entre user@device e uma empresa. Objetos de dispositivo são uma nova classe no AD no Windows Server 2012 R2 que pode ser usado para oferecer identidades compostas ao fornecer acesso a aplicativos e serviços. Um novo componente do AD FS - o DRS (Serviço de Registro de Dispositivos) - propicia uma identidade de dispositivo no Active Directory e define um certificado no dispositivo do consumidor que será usado para representar a identidade do dispositivo. É possível usar essa identidade de dispositivo para ingressar seu dispositivo no local de trabalho; em outras palavras, para conectar seu dispositivo pessoal no Active Directory do local de trabalho. Quando você ingressa seu dispositivo pessoal no local de trabalho, ele se torna um dispositivo conhecido e fornece autenticação de dois fatores contínua para aplicativos e recursos protegidos. Em outras palavras, depois que um dispositivo é ingressado no local de trabalho, a identidade do usuário está vinculada a esse dispositivo e pode ser usada para uma verificação de identidade composta contínua antes de um recurso protegido é acessado.

    Para obter mais informações sobre ingresso no local e de saída, consulte [ingresso no local de trabalho de qualquer dispositivo para SSO e contínuo segundo fator de autenticação em aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   É possível exigir MFA quando a solicitação de acesso para os recursos protegidos é proveniente da extranet ou da intranet.

## <a name="BKMK_2"></a>Visão geral do cenário
Nesse cenário, você habilita a MFA com base em dados de associação de grupo do usuário para um aplicativo específico. Em outras palavras, você definirá uma política de autenticação no servidor de federação para exigir MFA quando os usuários que pertencem a um determinado grupo solicitarem acesso a um aplicativo específico que está hospedado em um servidor Web.

Mais especificamente, nesse cenário, você habilita uma política de autenticação para um aplicativo de teste baseado em declarações chamado **claimapp**, pela qual um usuário do AD de **Robert Hatley** será requisitado a passar por MFA porque pertence ao **Finance** do grupo AD.

A passo a passo instruções passo a passo para configurar e verificar esse cenário é fornecida no [guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Para concluir as etapas neste passo a passo, você deve configurar um ambiente de laboratório e siga as etapas em [configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Outros cenários de habilitação de MFA no AD FS incluem o seguinte:

-   Habilitar a MFA, se a solicitação de acesso provir da extranet. Você pode modificar o código apresentado na seção "Configurar MFA a política" [guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) com o seguinte:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Habilitar a MFA, se a solicitação de acesso provir de um dispositivo que não está ingressado no local de trabalho.  Você pode modificar o código apresentado na seção "Configurar MFA a política" [guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) com o seguinte:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Habilitar a MFA, se o acesso estiver vindo de um usuário com um dispositivo que está ingressado no local de trabalho, mas não está registrado para esse usuário. Você pode modificar o código apresentado na seção "Configurar MFA a política" [guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) com o seguinte:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Consulte também
[Guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



