---
title: Métodos de autenticação adicionais do AD FS de 2019
description: Este documento descreve os novos métodos de autenticação do AD FS de 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 68cd67dc14d3407985579a49e2f8603634fafdb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824427"
---
# <a name="configure-3rd-party-authenticaiton-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurar provedores de autenticação de terceiros 3ª como autenticação principal no AD FS de 2019

>Aplica-se a: Windows Server 2019

As organizações estão enfrentando ataques que tentam força bruta, comprometer ou caso contrário, bloquear as contas de usuário por meio do envio de solicitações de autenticação baseada em senha.  Para ajudar a proteger as organizações contra o comprometimento, o AD FS introduziu recursos, como bloqueio de extranet do "inteligente" e o bloqueio de baseadas em endereço IP.  

No entanto, essas reduções são reativas.  Para fornecer uma maneira proativa, reduzir a gravidade desses ataques, o AD FS tem a capacidade de solicitar fatores diferente de senha antes de coletar a senha.  

Por exemplo, o AD FS 2016 introduziu Azure MFA como autenticação primária para que os códigos de OTP do aplicativo Authenticator pode ser usados como o primeiro fator.
Com o AD FS 2019 com base nisso, você pode configurar provedores de autenticação externa como fatores de autenticação primária.

Há dois cenários principais, que isso permite:

## <a name="scenario-1-protect-the-password"></a>Cenário 1: proteger a senha
Proteger o logon baseado em senha contra ataques de força bruta e bloqueios, solicitando um fator adicional e externo primeiro.  Somente se a autenticação externa for concluída com êxito o usuário, em seguida, vê um prompt de senha.  Isso elimina uma maneira conveniente de invasores tentando comprometer ou desabilitar contas.

Este cenário consiste em dois componentes:
- Avisar para MFA do Azure ou um fator de autenticação externa, como autenticação primária
- Nome de usuário e senha como autenticação adicional no AD FS

## <a name="scenario-2-password-free"></a>Cenário 2: sem senha!
Eliminar senhas totalmente mas concluir um forte, a autenticação multifator usando a senha não inteiramente com base em métodos no AD FS
- Azure MFA com o aplicativo autenticador
- Windows 10 Hello para empresas
- Autenticação de certificado
- Provedores de autenticação externa

## <a name="concepts"></a>Conceitos
O que **autenticação primária** realmente significa é que é o método que o usuário é solicitado para o primeiro, antes de fatores adicionais.  Anteriormente primary apenas métodos disponíveis no AD FS foram criados nos métodos do Active Directory ou Azure MFA ou outra autenticação LDAP armazenamentos.  Métodos externos pode ser configurados como "adicional" autenticação, o que acontece após a autenticação primária for concluída com êxito.

No AD FS 2019, a autenticação externa, como o principal recurso significa que qualquer provedor de autenticação externa registrados no farm do AD FS (usando o Register-AdfsAuthenticationProvider) ficam disponíveis para autenticação primária, bem como "adicional" autenticação. Elas podem ser habilitadas da mesma forma que os provedores internos, como autenticação de formulários e autenticação de certificado, para intranet e/ou o uso de extranet.

![autenticação](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Depois que um provedor externo está habilitado para a extranet, intranet, ou ambos, ele é disponibilizado para os usuários usem.  Se mais de um método estiver habilitado, os usuários serão ver uma página de opção e poder escolher um método primário, assim como autenticação adicional.

## <a name="pre-requisites"></a>Pré-requisitos
Antes de configurar provedores de autenticação externa como primário, verifique se que você tem os seguintes pré-requisitos em vigor
- Nível de comportamento de farm do AD FS (FBL) foi aumentado para '4' (esse valor converte para o AD FS 2019)
    - Esse é o valor FBL padrão para novos farms do AD FS de 2019
    - Para farms do AD FS com base no Windows Server 2012 R2 ou 2016, o FBL pode ser gerado usando o PowerShell commandlet Invoke-AdfsFarmBehaviorLevelRaise.  Para obter mais detalhes sobre como atualizar um farm do AD FS, consulte o farm do artigo para farms de servidores SQL ou farms de servidores WID de atualização 
    - Você pode verificar o valor FBL usando o cmdlet Get-AdfsFarmInformation
- O farm do AD FS 2019 está configurado para usar o novo usuário 'paginados' 2019 páginas opostas
    - Esse é o comportamento padrão para novos farms do AD FS de 2019
    - Para farms do AD FS atualizados do Windows Server 2012 R2 ou 2016, os fluxos paginados são habilitados automaticamente quando a autenticação externa como primário (o recurso descrito neste documento) está habilitada conforme descrito abaixo.

## <a name="enable-external-authentication-methods-as-primary"></a>Habilitar métodos de autenticação externa como primário
Depois de verificar os pré-requisitos, há duas maneiras de configurar provedores de autenticação adicionais do AD FS como primário:

### <a name="using-powershell"></a>Usando o PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


O serviço AD FS deve ser reiniciado depois de habilitar ou desabilitar a autenticação adicional como primário.

### <a name="using-the-ad-fs-management-console"></a>Usando o console de gerenciamento do AD FS
No console de gerenciamento do AD FS, sob **Service** -> **métodos de autenticação**, em **métodos de autenticação primária**, clique em Editar

Clique na caixa de seleção para **permitem que provedores de autenticação adicionais como primário**.

O serviço AD FS deve ser reiniciado depois de habilitar ou desabilitar a autenticação adicional como primário.

## <a name="enable-username-and-password-as-additional-authentication"></a>Habilitar o nome de usuário e senha como autenticação adicional
Para concluir o cenário de "proteger a senha", habilitar o nome de usuário e senha como autenticação adicional usando o PowerShell ou o console de gerenciamento do AD FS
### <a name="using-powershell"></a>Usando o PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>Usando o console de gerenciamento do AD FS
No console de gerenciamento do AD FS, sob **Service** -> **métodos de autenticação**, em **métodos de autenticação adicionais**, clique em  **Editar**

Clique na caixa de seleção para **autenticação de formulários** para habilitar o nome de usuário e senha como autenticação adicional.
