---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar AD FS proteção contra bloqueio de extranet
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f49e4a7e27d5b224a86655e48f07df741f03e7b0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962638"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurar AD FS proteção contra bloqueio de extranet

Em AD FS no Windows Server 2012 R2, apresentamos um recurso de segurança chamado bloqueio de extranet.  Com esse recurso, AD FS irá "parar" de autenticar a conta de usuário "mal-intencionado" de fora por um período de tempo.  Isso impede que suas contas de usuário sejam bloqueadas no Active Directory.  Além de proteger seus usuários de um bloqueio de conta do AD, AD FS bloqueio de extranet também protege contra ataques de adivinhação de senha de força bruta

> [!NOTE]
> Esse recurso funciona apenas para o **cenário de extranet** em que as solicitações de autenticação são feitas pelo proxy de aplicativo Web e aplicam-se somente a **autenticação de nome de usuário e senha**.

## <a name="advantages-of-extranet-lockout"></a>Vantagens do bloqueio de extranet
O bloqueio de extranet fornece as seguintes vantagens principais:
- Ele protege suas contas de usuário contra **ataques de força bruta** em que um invasor tenta adivinhar a senha de um usuário enviando solicitações de autenticação continuamente. Nesse caso, AD FS bloqueará a conta de usuário mal-intencionado para acesso à extranet
- Ele protege suas contas de usuário contra o **bloqueio de conta mal-intencionado** , em que um invasor deseja bloquear uma conta de usuário enviando solicitações de autenticação com senhas erradas. Nesse caso, embora a conta de usuário seja bloqueada pelo AD FS para acesso à extranet, a conta de usuário real no AD não é bloqueada e o usuário ainda pode acessar recursos corporativos na organização. Isso é conhecido como **bloqueio flexível**.

## <a name="how-it-works"></a>Como funciona
Há três configurações em AD FS que você precisa configurar para habilitar esse recurso: 
- **EnableExtranetLockout &lt; Booliano &gt; ** defina esse valor booliano como true se você quiser habilitar o bloqueio de extranet.
- **ExtranetLockoutThreshold &lt; Inteiro &gt; ** isso define o número máximo de tentativas de senha inválidos. Depois que o limite for atingido, AD FS rejeitará imediatamente as solicitações da extranet sem tentar entrar em contato com o controlador de domínio para autenticação, não importa se a senha é boa ou ruim, até que a janela de observação da extranet seja passada. Isso significa que o valor do atributo **badPwdCount** de uma conta do AD não aumentará enquanto a conta estiver bloqueada por software.
- **ExtranetObservationWindow &lt; TimeSpan &gt; ** determina por quanto tempo a conta de usuário será bloqueada por software. AD FS começará a executar a autenticação de nome de usuário e senha novamente quando a janela for passada. AD FS usa o atributo badPasswordTime do AD como referência para determinar se a janela de observação da extranet foi aprovada ou não. A janela foi aprovada se a hora atual > badPasswordTime + ExtranetObservationWindow. 

> [!NOTE]
> AD FS funções de bloqueio de extranet independentemente das políticas de bloqueio do AD. No entanto, é altamente recomendável que você defina o valor do parâmetro **ExtranetLockoutThreshold** para um valor que seja menor que o limite de bloqueio de conta do AD. A falha ao fazer isso resultaria na AD FS não conseguir proteger as contas de serem bloqueadas no Active Directory. 

Um exemplo de habilitação do recurso de bloqueio de extranet com o máximo de 15 tentativas de senha inadequadas e duração de bloqueio flexível de 30 minutos é o seguinte:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Essas configurações serão aplicadas a todos os domínios que o serviço de AD FS pode autenticar. A maneira como ele funciona é que, quando AD FS receber uma solicitação de autenticação, ele acessará o controlador de domínio primário (PDC) por meio de uma chamada LDAP e executará uma pesquisa para o atributo **badPwdCount** para o usuário no PDC. Se AD FS encontrar o valor da configuração **badPwdCount** >= ExtranetLockoutThreshold e o tempo definido na janela de observação de extranet ainda não tiver passado, AD FS rejeitará a solicitação imediatamente, o que significa que, independentemente de o usuário inserir uma senha boa ou incorreta da extranet, o logon falhará porque o AD FS não envia as credenciais ao AD. AD FS não mantém nenhum estado em relação a **badPwdCount** ou a contas de usuário bloqueadas. AD FS usa o AD para todo o rastreamento de estado. 

> [!warning]
> Quando AD FS bloqueio de extranet no servidor 2012 R2 é habilitado, todas as solicitações de autenticação por meio do WAP são validadas pelo AD FS no PDC. Quando o PDC não estiver disponível, os usuários não poderão se autenticar a partir da extranet.

O servidor 2016 oferece um parâmetro adicional que permite que AD FS fallback para outro controlador de domínio quando o PDC não estiver disponível:

- **ExtranetLockoutRequirePDC &lt; Booliano &gt; ** -quando habilitado: o bloqueio de extranet requer um controlador de domínio primário (PDC). Quando desabilitado: o bloqueio de extranet fará fallback para outro controlador de domínio, caso o PDC não esteja disponível.

Você pode usar o seguinte comando do Windows PowerShell para configurar o bloqueio de extranet AD FS no servidor 2016:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Trabalhando com a política de bloqueio de Active Directory
O recurso de bloqueio de extranet no AD FS funciona independentemente da política de bloqueio do AD. No entanto, você precisa verificar se as configurações do bloqueio de extranet estão configuradas corretamente para que ele possa atender à sua finalidade de segurança com a política de bloqueio do AD.
Vamos dar uma olhada na política de bloqueio do AD primeiro. Há três configurações sobre a política de bloqueio no AD:
- **Limite de bloqueio de conta**: essa configuração é semelhante à configuração ExtranetLockoutThreshold em AD FS. Ele determina o número de tentativas de logon com falha que farão com que uma conta de usuário seja bloqueada. Para proteger suas contas de usuário de um ataque de bloqueio de conta mal-intencionado, você deseja definir o valor de ExtranetLockoutThreshold em AD FS &lt; o valor limite de bloqueio de conta no AD
- **Duração do bloqueio de conta**: essa configuração determina por quanto tempo uma conta de usuário é bloqueada. Essa configuração não importa muito nessa conversa, pois o bloqueio de extranet deve sempre acontecer antes que o bloqueio do AD ocorra se configurado corretamente
- **Redefinir contador de bloqueios de conta após**: essa configuração determina quanto tempo deve decorrer da falha do último logon do usuário antes que **badPwdCount** seja redefinido como 0. Para que o recurso de bloqueio de extranet no AD FS funcione bem com a política de bloqueio do AD, você deseja certificar-se de que o valor de ExtranetObservationWindow em AD FS &gt; a redefinição do contador de bloqueios de conta após o valor no AD. Os exemplos a seguir explicarão o porquê.  

Vamos dar uma olhada em dois exemplos e ver como o **badPwdCount** muda ao longo do tempo com base em diferentes configurações e Estados. Vamos supor nos dois exemplos de **limite de bloqueio de conta** = 4 e **ExtranetLockoutThreshold** = 2. A seta **vermelha** representa uma tentativa de senha incorreta, a seta **verde** representa uma boa tentativa de senha. No exemplo #1, **ExtranetObservationWindow** &gt; **redefina o contador de bloqueio de conta após**. No exemplo #2, **ExtranetObservationWindow** &lt; **redefina o contador de bloqueio de conta após**. 

### <a name="example-1"></a>Exemplo 1
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Exemplo 2
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Como você pode ver no acima, há duas condições quando **badPwdCount** será redefinido como 0. Uma é quando há um logon bem-sucedido. A outra é quando é hora de redefinir esse contador, conforme definido em **Redefinir contador de bloqueios de conta após** a configuração. Ao **redefinir o contador de bloqueios de conta após** &lt; **ExtranetObservationWindow**, uma conta não tem nenhum risco de ser bloqueada pelo AD. No entanto, se o **contador de bloqueio de conta for redefinido após** &gt; **ExtranetObservationWindow**, haverá uma chance de que uma conta possa ser bloqueada pelo AD, mas de forma atrasada. Pode levar algum tempo para que uma conta seja bloqueada pelo AD, dependendo de sua configuração, pois AD FS só permitirá uma tentativa de senha incorreta durante sua janela de observação até que **badPwdCount** atinja o **limite de bloqueio de conta**.

Para obter mais informações, consulte [Configuring Account Lock](/archive/blogs/secguide/configuring-account-lockout). 

## <a name="known-issues"></a>Problemas Conhecidos
Há um problema conhecido em que a conta de usuário do AD não pode autenticar com AD FS porque o atributo **badPwdCount** não é replicado para o controlador de domínio que o ADFS está consultando. Consulte [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) para obter mais detalhes. Você pode encontrar todos os AD FS QFEs que foram lançados até [aqui](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Pontos principais a serem lembrados
- O recurso de bloqueio de extranet funciona apenas para o **cenário de extranet** em que as solicitações de autenticação são feitas pelo proxy de aplicativo Web
- O recurso de bloqueio de extranet aplica-se somente ao **nome de usuário & autenticação de senha**
- AD FS não mantém nenhum controle de **badPwdCount** ou de usuários que são bloqueados de maneira flexível. AD FS usa o AD para todo o rastreamento de estado
- AD FS executa uma pesquisa para o atributo **badPwdCount** por meio de chamada LDAP para o usuário no PDC para cada tentativa de autenticação  
- AD FS com mais de 2016 falharão se não for possível acessar o PDC. AD FS 2016 introduziu melhorias que permitirão que AD FS retorne a outros controladores de domínio no caso do PDC não esteja disponível. 
- AD FS permitirá solicitações de autenticação da extranet, se badPwdCount < ExtranetLockoutThreshold 
- Se **badPwdCount**  >=  **ExtranetLockoutThreshold** e **badPasswordTime**  +  **ExtranetObservationWindow** < hora atual, AD FS rejeitará as solicitações de autenticação da extranet
- Para evitar o bloqueio de conta mal-intencionado, você deve **ExtranetLockoutThreshold**verificar se o  <  **limite de bloqueio de conta** do ExtranetLockoutThreshold e o contador de bloqueio de **ExtranetObservationWindow**  >  **conta** de ExtranetObservationWindow


## <a name="additional-references"></a>Referências adicionais  
- [Práticas recomendadas para proteger Serviços de Federação do Active Directory (AD FS)](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Delegar acesso do Powershell Commandlet do AD FS para usuários não administradores](delegate-ad-fs-pshell-access.md)
- [Set-Adfsproperties](/powershell/module/adfs/set-adfsproperties?view=win10-ps)

[Operações do AD FS](../ad-fs-operations.md)

    
