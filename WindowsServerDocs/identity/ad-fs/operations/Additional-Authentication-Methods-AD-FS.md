---
title: Métodos de autenticação adicionais no AD FS 2019
description: Este documento descreve os novos métodos de autenticação no AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.openlocfilehash: fed038c81b298036401004751aab46dba520eab3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940269"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurar provedores de autenticação de terceiros como autenticação primária no AD FS 2019


As organizações estão enfrentando ataques que tentam forçar a força bruta, comprometer ou bloquear contas de usuário, enviando solicitações de autenticação baseadas em senha.  Para ajudar a proteger as organizações contra o comprometimento, AD FS introduziu recursos como bloqueio "inteligente" de extranet e bloqueio baseado em endereço IP.

No entanto, essas atenuações são reativas.  Para fornecer uma maneira proativa, para reduzir a gravidade desses ataques, AD FS tem a capacidade de solicitar fatores que não sejam de senha antes de coletar a senha.

Por exemplo, AD FS 2016 introduziu o Azure MFA como autenticação primária para que os códigos de OTP do aplicativo autenticador pudessem ser usados como o primeiro fator.
Com base nisso, com AD FS 2019, você pode configurar provedores de autenticação externa como fatores de autenticação primária.

Há dois cenários principais que permite:

## <a name="scenario-1-protect-the-password"></a>Cenário 1: proteger a senha
Proteja o logon baseado em senha de ataques de força bruta e bloqueios solicitando primeiro um fator externo adicional.  Somente se a autenticação externa for concluída com êxito, o usuário verá um prompt de senha.  Isso elimina uma maneira conveniente de os invasores tentarem comprometer ou desabilitar contas.

Esse cenário consiste em dois componentes:
- Solicitando o Azure MFA (disponível em AD FS 2016 em diante) ou um fator de autenticação externo como autenticação primária
- Nome de usuário e senha como autenticação adicional no AD FS

## <a name="scenario-2-password-free"></a>Cenário 2: sem senha!
Elimine totalmente as senhas, mas conclua uma autenticação forte e multifator usando métodos totalmente não baseados em senha no AD FS
- Azure MFA com aplicativo autenticador
- Windows 10 Hello para empresas
- Autenticação de certificado
- Provedores de autenticação externos

## <a name="concepts"></a>Conceitos
O que a **autenticação primária** significa realmente é que é o método que o usuário é solicitado primeiro, antes de fatores adicionais.  Anteriormente, os únicos métodos primários disponíveis em AD FS eram métodos internos para Active Directory ou Azure MFA, ou outros repositórios de autenticação LDAP.  Os métodos externos podem ser configurados como autenticação "adicional", que ocorre após a conclusão bem-sucedida da autenticação primária.

No AD FS 2019, a autenticação externa como recurso primário significa que todos os provedores de autenticação externa registrados no farm de AD FS (usando Register-AdfsAuthenticationProvider) tornam-se disponíveis para autenticação primária, bem como autenticação "adicional". Eles podem ser habilitados da mesma maneira que os provedores internos, como autenticação de formulários e autenticação de certificado, para uso de intranet e/ou Extranet.

![autenticação](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Depois que um provedor externo é habilitado para extranet, intranet ou ambos, ele fica disponível para uso pelos usuários.  Se mais de um método estiver habilitado, os usuários verão uma página de escolha e poderão escolher um método primário, assim como fazem para autenticação adicional.

## <a name="pre-requisites"></a>Pré-requisitos
Antes de configurar provedores de autenticação externa como primário, verifique se você tem os seguintes pré-requisitos em vigor
- O nível de comportamento de farm de AD FS (FBL) foi gerado para ' 4 ' (esse valor é convertido em AD FS 2019)
    - Este é o valor padrão de FBL para novos farms de 2019 AD FS
    - Para AD FS farms com base no Windows Server 2012 R2 ou 2016, o FBL pode ser gerado usando o cmdlet do PowerShell Invoke-AdfsFarmBehaviorLevelRaise.  Para obter mais detalhes sobre como atualizar um farm de AD FS, consulte o artigo de atualização do farm para farms do SQL ou farms de WID
    - Você pode verificar o valor de FBL usando o cmdlet Get-AdfsFarmInformation
- O farm AD FS 2019 está configurado para usar as páginas voltadas para o usuário do novo 2019 ' paginado '
    - Esse é o comportamento padrão para novos farms de 2019 AD FS
    - Para AD FS farms atualizados do Windows Server 2012 R2 ou 2016, os fluxos paginados são habilitados automaticamente quando a autenticação externa como primária (o recurso descrito neste documento) é habilitada conforme descrito abaixo.

## <a name="enable-external-authentication-methods-as-primary"></a>Habilitar métodos de autenticação externa como primário
Depois de verificar os pré-requisitos, há duas maneiras de configurar AD FS provedores de autenticação adicionais como primário:

### <a name="using-powershell"></a>Usando o PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
```


O serviço de AD FS deve ser reiniciado depois de habilitar ou desabilitar a autenticação adicional como primário.

### <a name="using-the-ad-fs-management-console"></a>Usando o console de gerenciamento do AD FS
No console de gerenciamento do AD FS, **Service**em  ->  **métodos de autenticação**de serviço, em métodos de **autenticação primária**, clique em Editar

Clique na caixa de seleção **permitir provedores de autenticação adicionais como primário**.

O serviço de AD FS deve ser reiniciado depois de habilitar ou desabilitar a autenticação adicional como primário.

## <a name="enable-username-and-password-as-additional-authentication"></a>Habilitar nome de usuário e senha como autenticação adicional
Para concluir o cenário "proteger a senha", habilite o nome de usuário e a senha como autenticação adicional usando o PowerShell ou o console de gerenciamento do AD FS
### <a name="using-powershell"></a>Usando o PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
```

### <a name="using-the-ad-fs-management-console"></a>Usando o console de gerenciamento do AD FS
No console de gerenciamento do AD FS, **Service**em  ->  **métodos de autenticação**de serviço, em métodos de **autenticação adicionais**, clique em **Editar**

Clique na caixa de seleção de **autenticação de formulários** para habilitar o nome de usuário e a senha como autenticação adicional.
