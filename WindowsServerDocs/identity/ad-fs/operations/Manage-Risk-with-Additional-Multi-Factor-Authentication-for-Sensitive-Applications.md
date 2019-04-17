---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: "Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9ee275e6fe38005394cd071e9cfe9a7999350e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais

>Aplica-se a: Windows Server 2012 R2


-   [Configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guia passo a passo: Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurar métodos de autenticação adicional do AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>Neste guia
Este guia fornece as seguintes informações:

-   [Mecanismos de autenticação no AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -descrição dos mecanismos de autenticação disponíveis no serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2

-   [Visão geral do cenário](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) -uma descrição de um cenário em que você usar os serviços de Federação do Active Directory (AD FS) para habilitar a autenticação multifator (MFA) com base na associação de grupo do usuário.

    > [!NOTE]
    > No AD FS no Windows Server 2012 R2, você pode habilitar MFA com base na participação em grupo ou identidade do usuário, local de rede e identidade do dispositivo.

    Para obter instruções detalhadas passo a passo para configurar e verificar esse cenário, consulte [guia passo a passo: gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Principais conceitos - mecanismos de autenticação no AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Benefícios dos mecanismos de autenticação no AD FS
Serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2 fornece aos administradores de TI um conjunto mais rico e mais flexível de ferramentas para autenticação de usuários que desejam acesso a recursos corporativos. Ela permite que os administradores com um controle flexível principal e os métodos de autenticação adicional, oferece uma experiência de gerenciamento avançado para configurar a autenticação políticas (tanto por meio da interface do usuário e do Windows PowerShell) e melhora a experiência para os usuários finais que acessar aplicativos e serviços que são protegidos pelo AD FS. Estes são alguns dos benefícios de proteger seus aplicativos e serviços com o AD FS no Windows Server 2012 R2:

-   Política de autenticação global – uma funcionalidade de gerenciamento central, que um administrador de TI pode escolher quais métodos de autenticação são usados para autenticar usuários com base no local de rede do qual eles acessarem recursos protegidos. Isso permite que os administradores fazer o seguinte:

    -   Obriga o uso dos métodos de autenticação mais seguros para solicitações de acesso de extranet.

    -   Habilite a autenticação de dispositivo para autenticação de fator de segundo perfeita. Isso vincula a identidade do usuário para o dispositivo registrado que é usado para acessar o recurso, oferecendo assim a verificação de identidade composta mais segura antes de recursos protegidos são acessados.

        > [!NOTE]
        > Para obter mais informações sobre o objeto de dispositivo, serviço de registro do dispositivo, Workplace Join e o dispositivo como autenticação de fator de segundo perfeita e SSO, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Defina o requisito de MFA para todos os acessos extranet ou condicionalmente com base na identidade do usuário, local de rede ou um dispositivo que é usado para acessar recursos protegidos.

-   Maior flexibilidade na configuração de políticas de autenticação: você pode configurar políticas de autenticação personalizado para recursos do AD FS protegido com diversos valores de negócios. Por exemplo, você pode exigir MFA para aplicativos com alto impacto comercial.

-   Facilidade de uso: ferramentas de gerenciamento simples e intuitiva, como o snap-in baseado em GUI AD FS gerenciamento MMC e cmdlets do Windows PowerShell permitem aos administradores de TI configurar políticas de autenticação com relativa facilidade. Com o Windows PowerShell, você pode fazer script suas soluções para uso em escala e para automatizar tarefas administrativas comuns.

-   Maior controle sobre os ativos corporativos: como como um administrador, você pode usar o AD FS para configurar uma política de autenticação que se aplica a um recurso específico, você tem maior controle sobre recursos como corporativos estão protegidos. Aplicativos não podem substituir as políticas de autenticação especificadas pelos administradores de TI. Para aplicativos confidenciais e serviços, você pode habilitar o requisito MFA, autenticação de dispositivo e, opcionalmente, nova autenticação sempre que o recurso é acessado.

-   Suporte para provedores MFA personalizados: para organizações que aproveitam métodos MFA de terceiros, o AD FS oferece a capacidade de incorporar e usar esses métodos de autenticação perfeitamente.

### <a name="authentication-scope"></a>Escopo de autenticação
No AD FS no Windows Server 2012 R2, você pode especificar uma política de autenticação em um escopo global que se aplica a todos os aplicativos e serviços que são protegidos pelo AD FS.  Você também pode definir políticas de autenticação para aplicativos específicos e serviços (dependência relações de confiança de terceiros) que são protegidos pelo AD FS. Especificando uma política de autenticação para um aplicativo específico (por confiar confiança de terceiros) não substitui a política de autenticação global. Se global ou por confiar confiança de terceiros a política de autenticação exige MFA, MFA será acionada quando o usuário tentar autenticar nesta terceira relação de confiança de terceiros.  A política de autenticação global é um fallback para a terceira parte relações de confiança (aplicativos e serviços) que não têm uma política de autenticação específico configurada.

Uma política de autenticação global se aplica a todas as partes confiantes que são protegidas pelo AD FS. Você pode configurar as seguintes configurações como parte da política de autenticação global:

-   Métodos de autenticação a ser usado para autenticação principal

-   Configurações e métodos de MFA

-   Se a autenticação de dispositivo é habilitada. Para obter mais informações, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Políticas de autenticação de confiança de terceiros por confiar se aplicam especificamente a tentativas de acessar essa terceira confiança de terceiros (aplicativo ou serviço). Você pode configurar as seguintes configurações como parte da política de autenticação de confiança de terceiros por confiar:

-   Se os usuários são necessários para fornecer suas credenciais de cada vez para entrar

-   Configurações de MFA com base na solicitação de acesso, registro de dispositivo e usuário/grupo dados de localização

### <a name="primary-and-additional-authentication-methods"></a>Métodos de autenticação principal e adicionais
Com o AD FS no Windows Server 2012 R2, além do mecanismo de autenticação principal, os administradores podem configurar métodos de autenticação adicional. Métodos de autenticação principal são internos e servem para validar as identidades dos usuários. Você pode configurar os fatores de autenticação adicional para solicitar que são fornecidas mais informações sobre a identidade do usuário e, consequentemente, certifique-se de autenticação mais forte.

Com a autenticação principal no AD FS no Windows Server 2012 R2, você tem as seguintes opções:

-   Recursos publicados para ser acessados de fora da rede corporativa, autenticação de formulários é selecionada por padrão. Além disso, você também pode habilitar a autenticação de certificado (em outras palavras, autenticação com base em um cartão inteligente ou autenticação de certificado de cliente do usuário que funciona com o AD DS).

-   Recursos da intranet, autenticação do Windows é selecionada por padrão. Além disso, você pode habilitar também formulários e/ou autenticação de certificado.

Ao selecionar mais de um método de autenticação, você permite que os usuários tenham uma escolha de qual método autenticar com na página de entrada para seu aplicativo ou serviço.

Você também pode habilitar a autenticação de dispositivo para autenticação de fator de segundo perfeita. Isso vincula a identidade do usuário para o dispositivo registrado que é usado para acessar o recurso, oferecendo assim a verificação de identidade composta mais segura antes de recursos protegidos são acessados.

> [!NOTE]
> Para obter mais informações sobre o objeto de dispositivo, serviço de registro do dispositivo, Workplace Join e o dispositivo como autenticação de fator de segundo perfeita e SSO, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Se você especificar um método de autenticação do Windows (opção padrão) para seus recursos de intranet, solicitações de autenticação passar por esse método perfeitamente em navegadores que dão suporte à autenticação do Windows.

> [!NOTE]
> Autenticação do Windows não é compatível com todos os navegadores. O mecanismo de autenticação do AD FS em Windows Server 2012 R2 detecta o agente de usuário do navegador do usuário e usa uma configuração configurável para determinar se esse agente do usuário dá suporte para autenticação do Windows. Os administradores podem adicionar à lista de agentes do usuário (por meio do Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`comando, para especificar cadeias de caracteres de agente de usuário alternativo para navegadores que dão suporte à autenticação do Windows. Se o agente do usuário do cliente não dá suporte a autenticação do Windows, o método de fallback padrão é a autenticação de formulários.

### <a name="configuring-mfa"></a>Configurando MFA
Há duas partes para configurar o MFA do AD FS em Windows Server 2012 R2: especificando as condições sob as quais MFA é necessária e selecionando um método de autenticação adicional. Para obter mais informações sobre métodos de autenticação adicionais, consulte [configurar métodos de autenticação adicionais do AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Configurações de MFA**

As seguintes opções estão disponíveis para as configurações de MFA (condições sob as quais exigir MFA):

-   Você pode exigir MFA para usuários e grupos no domínio do AD que o servidor de Federação está associado a específicos.

-   Você pode exigir MFA para registrado (local de trabalho ingressado) ou cujo registro foi cancelado (não local de trabalho ingressado) dispositivos.

    Windows Server 2012 R2 adota uma abordagem centrada no usuário onde os objetos de dispositivo representam uma relação entre os dispositivos modernos user@devicee uma empresa. Objetos de dispositivo são uma nova classe no AD no Windows Server 2012 R2 que pode ser usado para oferecer compostos identidade ao fornecer acesso aos aplicativos e serviços. Um novo componente do AD FS - o dispositivo serviço de registro (DRS) - provisiona uma identidade do dispositivo no Active Directory e define um certificado no dispositivo consumidor que será usado para representar a identidade do dispositivo. Você pode usar este dispositivo identidade de empresa ingressar em seu dispositivo, em outras palavras, para conectar o dispositivo pessoal para o Active Directory de sua empresa. Quando você ingressar o dispositivo pessoal à área de trabalho, ele se torna um dispositivo conhecido e fornecerá autenticação de fator de segundo perfeita para recursos protegidos e aplicativos. Em outras palavras, depois que um dispositivo é ingressado do local de trabalho, a identidade do usuário está vinculada a esse dispositivo e pode ser usada para uma verificação de identidade composta perfeita antes de um recurso protegido é acessado.

    Para obter mais informações sobre o ingresso no local de trabalho e sair, consulte [ingresso para área de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   Você pode exigir MFA quando a solicitação de acesso a recursos protegidos vem de extranet ou a intranet.

## <a name="BKMK_2"></a>Visão geral do cenário
Nesse cenário, você deve habilitar MFA com base nos dados de associação de grupo do usuário para um aplicativo específico. Em outras palavras, você configurará uma política de autenticação no servidor de Federação exigir MFA quando os usuários que pertencem a um determinado grupo solicitarem acesso a um aplicativo específico que está hospedado em um servidor web.

Mais especificamente, nesse cenário, você habilitar uma política de autenticação para um aplicativo de teste baseada em declarações chamado **claimapp**, pelo qual um usuário AD **Robert Hatley** será necessária para passar por MFA, já que ele pertence a um grupo do AD **Finanças**.

As instruções de etapa por etapa para configurar e verificar esse cenário são fornecidas em [guia passo a passo: gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Para concluir as etapas neste passo a passo, você deve configurar um ambiente de laboratório e siga as etapas em [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Outros cenários de habilitar MFA no AD FS incluem o seguinte:

-   Habilite MFA, quando a solicitação de acesso é proveniente de extranet. Você pode modificar o código apresentado na seção "Definir a política de MFA" [guia passo a passo: gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) com o seguinte:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Habilite MFA, quando a solicitação de acesso é proveniente de um dispositivo associado ao local de trabalho não.  Você pode modificar o código apresentado na seção "Definir a política de MFA" [guia passo a passo: gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) com o seguinte:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Habilite MFA, se o acesso é proveniente de um usuário com um dispositivo que é o local de trabalho ingressado, mas não registrados para esse usuário. Você pode modificar o código apresentado na seção "Definir a política de MFA" [guia passo a passo: gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) com o seguinte:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Consulte também
[Guia passo a passo: Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)<ph x="2">
</ph>[configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



