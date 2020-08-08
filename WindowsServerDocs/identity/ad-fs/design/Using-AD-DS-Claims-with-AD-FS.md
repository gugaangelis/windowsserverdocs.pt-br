---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Usando declarações do AD DS com o AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 942b7d88196fa32dd70fd554d76547cc5feadb26
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967543"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Usando declarações do AD DS com o AD FS


Você pode habilitar o controle de acesso mais rico para aplicativos federados usando Active Directory Domain Services \( AD DS \) \- emitir declarações de usuário e dispositivo junto com serviços de Federação do Active Directory (AD FS) \( AD FS \) .

## <a name="about-dynamic-access-control"></a>Sobre o controle de acesso dinâmico
No Windows Server &reg; 2012, o recurso de controle de acesso dinâmico permite que as organizações concedam acesso a arquivos com base em declarações de usuário \( que são originadas por atributos de conta de usuário \) e declarações de dispositivo \( que são originadas por atributos de conta de computador \) emitidos pelo Active Directory Domain Services \( AD DS \) . AD DS declarações emitidas são integradas à autenticação integrada do Windows por meio do protocolo de autenticação Kerberos.

Para obter mais informações sobre o controle de acesso dinâmico, consulte [roteiro de conteúdo de controle de acesso dinâmico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).

### <a name="whats-new-in-ad-fs"></a>O que há de novo no AD FS?
Como uma extensão para o cenário de controle de acesso dinâmico, AD FS no Windows Server 2012 agora pode:

-   Acesse atributos de conta de computador, além dos atributos de conta de usuário de dentro de AD DS. Nas versões anteriores do AD FS, o Serviço de Federação não pôde acessar atributos de conta de computador de AD DS.

-   Consuma AD DS declarações de usuário ou dispositivo emitidas que residem em um tíquete de autenticação Kerberos. Nas versões anteriores do AD FS, o mecanismo de declarações foi capaz de ler SIDs de IDs de segurança de usuário e grupo \( \) do Kerberos, mas não foi capaz de ler nenhuma informação de declaração contida em um tíquete Kerberos.

-   Transforme AD DS declarações de usuário ou dispositivo emitidas em tokens SAML que aplicativos confiáveis podem usar para executar um controle de acesso mais rico.

## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Benefícios do uso de declarações de AD DS com AD FS
Essas declarações emitidas AD DS podem ser inseridas em Tíquetes de autenticação Kerberos e usadas com AD FS para fornecer os seguintes benefícios:

-   As organizações que exigem políticas de controle de acesso mais avançadas podem habilitar o \- acesso baseado em declarações a aplicativos e recursos usando AD DS declarações emitidas com base nos valores de atributo armazenados em AD DS para um determinado usuário ou conta de computador. Isso pode ajudar os administradores a reduzirem a sobrecarga adicional associada à criação e ao gerenciamento:

    -   AD DS grupos de segurança que, de outra forma, seriam usados para controlar o acesso a aplicativos e recursos acessíveis por meio da autenticação integrada do Windows.

    -   Relações de confiança de floresta que, de outra forma, seriam usadas para controlar o acesso a \- \- \( \) \/ aplicativos e recursos acessíveis à Internet B2B entre empresas.

-   As organizações agora podem impedir o acesso não autorizado a recursos de rede de computadores cliente com base no fato de um valor de atributo de conta de computador específico armazenado em AD DS \( por exemplo, o nome DNS de um computador \) corresponde à política de controle de acesso do recurso, \( por exemplo, um servidor de arquivos que foi ACLd com declarações \) ou a política de terceira parte confiável \( , por exemplo \- \) Isso pode ajudar os administradores a definir políticas de controle de acesso mais refinadas para recursos ou aplicativos que são:

    -   Acessível somente por meio da autenticação integrada do Windows.

    -   Internet acessível por meio de mecanismos de autenticação AD FS. AD FS pode ser usado para transformar AD DS declarações de dispositivo emitidas em AD FS declarações que podem ser encapsuladas em tokens SAML que podem ser consumidos por um recurso acessível pela Internet ou por um aplicativo de terceira parte confiável.

## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Diferenças entre AD DS e AD FS declarações emitidas
Há dois fatores diferenciais que são importantes para entender sobre as declarações que são emitidas de AD DS versus AD FS. Entre elas, podemos incluir:

-   AD DS só pode emitir declarações que são encapsuladas em tíquetes Kerberos, não tokens SAML. Para obter mais informações sobre como AD DS emite declarações, consulte [roteiro de conteúdo de controle de acesso dinâmico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).

-   AD FS só pode emitir declarações encapsuladas em tokens SAML, não com tíquetes Kerberos. Para obter mais informações sobre como AD FS emite declarações, consulte [a função do mecanismo de declarações](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).

## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Como as declarações emitidas pelo AD DS funcionam com o AD FS
AD DS declarações emitidas podem ser usadas com AD FS para acessar declarações de usuário e dispositivo diretamente do contexto de autenticação do usuário, em vez de fazer uma chamada LDAP separada para Active Directory. A ilustração a seguir e as etapas correspondentes discutem como esse processo funciona em mais detalhes para habilitar \- o controle de acesso baseado em declarações para o cenário de controle de acesso dinâmico.

![usando declarações](media/UsingADDSClaimswithADFS.gif)

1.  Um administrador de AD DS usa o console do Centro Administrativo do Active Directory ou cmdlets do PowerShell para habilitar objetos de tipo de declaração específicos no esquema de AD DS.

2.  Um administrador de AD FS usa o console de gerenciamento de AD FS para criar e configurar o provedor de declarações e as relações de confiança de terceira parte confiável com regras de declaração de passagem \- ou de transformação.

3.  Um cliente do Windows tenta acessar a rede. Como parte do processo de autenticação Kerberos, o cliente apresenta seu tíquete de usuário e computador \- concedendo \( TGT de tíquete \) que ainda não contém nenhuma declaração para o controlador de domínio. O controlador de domínio examina AD DS para tipos de declaração habilitados e inclui quaisquer declarações resultantes no tíquete Kerberos retornado.

4.  Quando o cliente do usuário \/ tenta acessar um recurso de arquivo que é ACLd para exigir as declarações, ele pode acessar o recurso porque a ID composta do Kerberos tem essas declarações.

5.  Quando o mesmo cliente tenta acessar um site ou aplicativo Web configurado para AD FS autenticação, o usuário é redirecionado para um servidor de Federação AD FS configurado para autenticação integrada do Windows. O cliente envia uma solicitação para o controlador de domínio usando o Kerberos. O controlador de domínio emite um tíquete Kerberos que contém as declarações solicitadas que o cliente pode apresentar ao servidor de Federação.

6.  Com base na maneira como as regras de declarações foram configuradas no provedor de declarações e nas confianças de terceira parte confiável que o administrador configurou anteriormente, AD FS lê as declarações do tíquete Kerberos e as inclui em um token SAML que ele emite para o cliente.

7.  O cliente recebe o token SAML que contém as declarações corretas e, em seguida, é redirecionado para o site.

Para obter mais informações sobre como criar as regras de declaração necessárias para AD DS declarações emitidas trabalharem com AD FS, consulte [criar uma regra para transformar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
