---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar a proteção de bloqueio de Extranet do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6612c05e664b50c5a50b10b712b91715cc85d230
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189885"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurar a proteção de bloqueio de Extranet do AD FS

No AD FS no Windows Server 2012 R2, apresentamos um recurso de segurança chamado bloqueio de Extranet.  Com esse recurso, o AD FS "interromperá" autenticar a conta de usuário "mal-intencionado" de fora para um período de tempo.  Isso impede que as contas de usuário que está sendo bloqueado no Active Directory.  Além de proteger os usuários de um bloqueio de conta do AD, o bloqueio de extranet do AD FS também protege contra ataques de adivinhação de senha de força bruta

> [!NOTE]
> Esse recurso funciona somente para o **cenário de extranet** onde a autenticação de solicitações são fornecidos por meio do Proxy de aplicativo Web e só se aplica ao **autenticação de nome de usuário e senha**.

## <a name="advantages-of-extranet-lockout"></a>Vantagens de bloqueio de Extranet
Bloqueio de extranet fornece as seguintes vantagens essenciais:
- Ele protege suas contas de usuário do **ataques de força bruta** em que um invasor tentar adivinhar a senha do usuário enviando continuamente as solicitações de autenticação. Nesse caso, o AD FS irá bloquear a conta de usuário mal-intencionado para acesso à extranet
- Ele protege suas contas de usuário do **bloqueio de conta mal-intencionado** em que um invasor deseja bloquear uma conta de usuário por meio do envio de solicitações de autenticação com senhas erradas. Nesse caso, embora a conta de usuário será bloqueada pelo AD FS para acesso à extranet, a conta de usuário real do AD não está bloqueada e o usuário ainda pode acessar recursos corporativos dentro da organização. Isso é conhecido como um **bloqueio reversível**.

## <a name="how-it-works"></a>Como funciona
Há 3 configurações no AD FS que você precisa configurar para habilitar esse recurso: 
- **EnableExtranetLockout &lt;Boolean&gt;**  definir esse valor booliano para ser verdadeiro se desejar habilitar o bloqueio de Extranet.
- **ExtranetLockoutThreshold &lt;inteiro&gt;**  define o número máximo de tentativas de senha incorreta. Quando o limite é atingido, o AD FS imediatamente rejeita as solicitações da extranet sem tentar contatar o controlador de domínio para autenticação, não importa se a senha é bom ou ruim, até que a janela de Observação extranet é passada. Isso significa que o valor de **badPwdCount** atributo de uma conta do AD não aumentará enquanto a conta está bloqueada soft out.
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  Isso determina quanto tempo o usuário conta será reversível-bloqueada. O AD FS será iniciado realizar a autenticação de nome de usuário e senha novamente quando a janela é passada. O AD FS usa o badPasswordTime de atributo do AD como referência para determinar se a janela de Observação extranet passou ou não. A janela tiver passado se o atual tempo > badPasswordTime + ExtranetObservationWindow. 

> [!NOTE]
> Funções de bloqueio de extranet do AD FS independentemente das políticas de bloqueio do AD. No entanto, é altamente recomendável que você defina as **ExtranetLockoutThreshold** valor do parâmetro para um valor que é menor que o limite de bloqueio de conta do AD. Falha ao fazer isso resultaria no AD FS não ser capaz de proteger as contas de serem bloqueados no Active Directory. 

Um exemplo de como habilitar o recurso de bloqueio de Extranet com 15 número máximo de tentativas de senha e a duração de bloqueio flexível de 30 minutos é da seguinte maneira:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Essas configurações serão aplicadas a todos os domínios que o serviço AD FS pode autenticar. A maneira que ele funciona é que quando o AD FS recebe uma solicitação de autenticação, ele acessar o controlador de domínio primário (PDC) por meio de uma chamada LDAP e realizar uma pesquisa para o **badPwdCount** atributo para o usuário no controlador de domínio primário. Se o AD FS encontra o valor de **badPwdCount** > = ExtranetLockoutThreshold configuração e a hora definida na janela de observação da Extranet não passou ainda, o AD FS rejeitará a solicitação imediatamente, o que significa que não importa se a usuário insere uma senha de BOM ou ruim da extranet, o logon falhará porque o AD FS não envia as credenciais para o AD. O AD FS não mantém nenhum estado em relação ao **badPwdCount** ou as contas de usuário bloqueadas. O AD FS usa o AD para todos os estados de controle. 

> [!warning]
> Quando o bloqueio de Extranet do AD FS no Server 2012 R2 esteja habilitado todas as solicitações de autenticação por meio de WAP são validadas pelo AD FS no PDC. Quando o controlador de domínio primário não estiver disponível, os usuários poderão autenticar da extranet.

Server 2016 oferece um parâmetro adicional que permite que o AD FS para o fallback para outro controlador de domínio quando o controlador de domínio primário não está disponível:

- **ExtranetLockoutRequirePDC &lt;Boolean&gt;**  – quando habilitado: bloqueio de extranet requer um controlador de domínio primário (PDC). Quando desabilitado: bloqueio de extranet fará fallback para outro controlador de domínio, caso o controlador de domínio primário não está disponível.

Você pode usar o seguinte comando do Windows PowerShell para configurar o bloqueio de extranet do AD FS no Server 2016:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Trabalhando com a política de bloqueio do Active Directory
O recurso de bloqueio de Extranet do AD FS funciona independentemente da política de bloqueio do AD. No entanto, é necessário garantir que as configurações para o bloqueio de Extranet está configurado corretamente para que ele possa atender sua finalidade de segurança com a política de bloqueio do AD.
Vamos dar uma olhada na diretiva de bloqueio do AD primeiro. Há três configurações em relação à diretiva de bloqueio do AD:
- **Limite de bloqueio de conta**: essa configuração é semelhante à configuração ExtranetLockoutThreshold no AD FS. Ele determina o número de tentativas de logon com falha que fará com que uma conta de usuário seja bloqueada. Para proteger suas contas de usuário de um ataque de bloqueio de conta mal-intencionado, você deseja definir o valor de ExtranetLockoutThreshold no AD FS &lt; o valor de limite de bloqueio de conta do AD
- **Duração do bloqueio de conta**: essa configuração determina quanto tempo um usuário conta está bloqueada. Essa configuração não importa muito nesta conversa como bloqueio de Extranet sempre deve ocorrer antes de bloqueio do AD acontece se configurado corretamente
- **Redefinir contador de bloqueios de conta após**: essa configuração determina quanto tempo deve decorrer da última falha de logon do usuário antes de **badPwdCount** é redefinido como 0. Para que o recurso de bloqueio de Extranet do AD FS para funcionar bem com a política de bloqueio do AD, você deseja garantir que o valor de ExtranetObservationWindow no AD FS &gt; o valor de Redefinir contador de bloqueios de conta após do AD. Os exemplos a seguir explicará o porquê.  

Vamos dar uma olhada em dois exemplos e ver como **badPwdCount** muda ao longo do tempo com base em configurações diferentes e estados. Vamos supor que nos dois exemplos **limite de bloqueio de conta** = 4 e **ExtranetLockoutThreshold** = 2. O **vermelho** seta representa a tentativa de senha incorreta, o **verde** seta representa uma tentativa de senha válida. No exemplo 1 de # **ExtranetObservationWindow** &gt; **Redefinir contador de bloqueios de conta após**. No exemplo 2 de # **ExtranetObservationWindow** &lt; **Redefinir contador de bloqueios de conta após**. 

### <a name="example-1"></a>Exemplo 1
![Exemplo 1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Exemplo 2
![Exemplo 1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Como você pode ver acima, há duas condições quando **badPwdCount** será redefinido como 0. Um é quando há um logon bem-sucedido. O outro é quando é hora de redefinir esse contador conforme definido em **Redefinir contador de bloqueios de conta após** configuração. Quando **Redefinir contador de bloqueios de conta após** &lt; **ExtranetObservationWindow**, uma conta não tem nenhum risco de serem bloqueados pelo AD. No entanto, se **Redefinir contador de bloqueios de conta após** &gt; **ExtranetObservationWindow**, há uma chance de que uma conta pode ser bloqueada pelo AD, mas em uma "forma atrasada". Ele pode demorar um pouco para obter uma conta bloqueada pelo AD dependendo da sua configuração como o AD FS só permitirá que uma tentativa de senha incorreta durante sua janela de Observação até **badPwdCount** atinge **limite de bloqueio de conta** .

Para obter mais informações, consulte [Configurando o bloqueio de conta](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/). 

## <a name="known-issues"></a>Problemas conhecidos
Há um problema conhecido em que a conta de usuário do AD não pode autenticar com o AD FS porque o **badPwdCount** atributo não é replicado para o controlador de domínio ADFS está consultando. Ver [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) para obter mais detalhes. Você pode encontrar todos os QFEs FS AD que foram lançadas até o momento [aqui](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Principais pontos a serem lembrados
- O recurso de bloqueio de Extranet somente funciona para o **cenário de extranet** onde as solicitações de autenticação são fornecidos por meio do Proxy de aplicativo Web
- O recurso de bloqueio de Extranet somente se aplica a **autenticação de nome de usuário e senha**
- O AD FS não controlar qualquer **badPwdCount** ou usuários que são o soft-bloqueada. O AD FS usa o AD para todos os estados de controle
- O AD FS executa uma pesquisa para o **badPwdCount** atributo por meio de chamada LDAP para o usuário no controlador de domínio primário para cada tentativa de autenticação  
- O AD FS anterior a 2016 falhará se não puder acessar o controlador de domínio primário. AD FS 2016 introduziu melhorias que permitirá que o AD FS para o fallback para outros controladores de domínio no caso de controlador de domínio primário não está disponível. 
- O AD FS permitirá que as solicitações de autenticação do extranet se badPwdCount < ExtranetLockoutThreshold 
- Se **badPwdCount** >= **ExtranetLockoutThreshold** AND **badPasswordTime** + **ExtranetObservationWindow** < Hora atual, o AD FS rejeitará as solicitações de autenticação da extranet
- Para evitar o bloqueio de conta mal-intencionado, você deve se certificar **ExtranetLockoutThreshold** < **limite de bloqueio de conta** AND **ExtranetObservationWindow**  >  **Zerar contador de bloqueio de conta**


## <a name="additional-references"></a>Referências adicionais  
- [Práticas recomendadas para proteger os serviços de Federação do Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Delegar acesso do Powershell Commandlet do AD FS para usuários não administradores](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
