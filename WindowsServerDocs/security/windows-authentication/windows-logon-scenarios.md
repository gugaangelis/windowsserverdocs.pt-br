---
title: Cenários de logon do Windows
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d6a1d52bee3c2d14ea83ccb794ecea1872868df5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828177"
---
# <a name="windows-logon-scenarios"></a>Cenários de logon do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de referência para profissionais de TI resume o logon do Windows comuns e cenários de logon.

Os sistemas operacionais Windows exigem que todos os usuários fizerem logon no computador com uma conta válida para acesso local e recursos de rede. Computadores baseados em Windows proteger recursos Implementando o processo de logon, em que os usuários são autenticados. Depois que um usuário é autenticado, tecnologias de controle de acesso e autorização implementam a segunda fase de proteção dos recursos: determinar se o usuário autenticado está autorizado a acessar um recurso.

O conteúdo deste tópico se aplicam a versões do Windows designadas na **aplica-se a** no início deste tópico.

Além disso, aplicativos e serviços podem exigir que os usuários entrar para acessar esses recursos que são oferecidos pelo aplicativo ou serviço. O processo de logon é semelhante ao processo de logon, em que uma conta válida e credenciais corretas são necessárias, mas as informações de logon armazenadas no banco de dados Gerenciador de conta de segurança (SAM) no computador local e no Active Directory, onde aplicável. Entrar conta e informações de credenciais são gerenciadas pelo aplicativo ou serviço e, opcionalmente, podem ser armazenadas localmente no cofre de credenciais.

Para entender como funciona a autenticação, consulte [conceitos de autenticação do Windows](windows-authentication-concepts.md).

Este tópico descreve os cenários a seguir:

-   [Logon interativo](#BKMK_InteractiveLogon)

-   [Logon de rede](#BKMK_NetworkLogon)

-   [Logon de cartão inteligente](#BKMK_SmartCardLogon)

-   [Logon biométrico](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Logon interativo
O processo de logon começa quando um usuário insere as credenciais na caixa de diálogo entrada de credenciais, ou quando o usuário insere um cartão inteligente no leitor de cartão inteligente, ou quando o usuário interage com um dispositivo biométrico. Os usuários podem executar um logon interativo, usando uma conta de usuário local ou uma conta de domínio para fazer logon um computador.

O diagrama a seguir mostra os elementos de logon interativo e o processo de logon.

![Diagrama que mostra os elementos de logon interativo e o processo de logon](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Arquitetura de autenticação de cliente do Windows**

### <a name="BKMK_LocaDomainLogon"></a>Logon local e de domínio
As credenciais que o usuário apresente para um logon de domínio contêm todos os elementos necessários para um logon local, como o nome da conta e senha ou certificado e informações de domínio do Active Directory. O processo confirma a identificação do usuário para o banco de dados de segurança no computador local do usuário ou para um domínio do Active Directory. Esse processo de logon obrigatório não pode ser desativado para os usuários em um domínio.

Os usuários podem executar um logon interativo a um computador em qualquer uma das duas maneiras:

-   Localmente, quando o usuário tem acesso físico direto para o computador, ou quando o computador fizer parte de uma rede de computadores.

    Um logon local concede uma permissão de usuário para acessar os recursos do Windows no computador local. Um logon local requer que o usuário tem uma conta de usuário no Gerenciador de contas de segurança (SAM) no computador local. O SAM protege e gerencia informações de usuário e grupo na forma de contas de segurança armazenadas no registro do computador local. O computador pode ter acesso à rede, mas não é necessária. Informações de associação de grupo e de conta de usuário local são usadas para gerenciar o acesso aos recursos locais.

    Um logon de rede concede uma permissão de usuário para acessar recursos do Windows no computador local, além de todos os recursos em computadores da rede, conforme definido pelo token de acesso da credencial. Um logon local e um logon de rede exigem que o usuário tem uma conta de usuário no Gerenciador de contas de segurança (SAM) no computador local. Informações de associação de grupo e de conta de usuário local são usadas para gerenciar o acesso a recursos locais e o token de acesso para o usuário define quais recursos podem ser acessados em computadores da rede.

    Um logon local e um logon de rede não são suficientes para conceder a permissão de usuário e computador para acessar e usar recursos de domínio.

-   Remotamente, por meio dos serviços de Terminal ou serviços de área de trabalho remota (RDS), nesse caso o logon é mais qualificado como remota interativa.

Após um logon interativo, Windows executa aplicativos em nome do usuário e o usuário pode interagir com esses aplicativos.

Um logon local concede uma permissão de usuário para acessar recursos no computador local ou recursos em computadores da rede. Se o computador tiver ingressado em um domínio, a funcionalidade de Winlogon tenta fazer logon no domínio.

Um logon de domínio concede uma permissão de usuário para acessar o local e recursos do domínio. Um logon de domínio requer que o usuário tem uma conta de usuário no Active Directory. O computador deve ter uma conta no domínio do Active Directory e estar fisicamente conectado à rede. Os usuários também devem ter os direitos de usuário para fazer logon um computador local ou em um domínio. Informações de conta de usuário de domínio e informações de associação de grupo são usados para gerenciar o acesso a recursos locais e de domínio.

### <a name="BKMK_RemoteLogon"></a>Logon remoto
No Windows, o acesso a outro computador por meio de logon remoto se baseia no protocolo de área de trabalho remota (RDP). Porque o usuário deve já ter com êxito conectado ao computador cliente antes de tentar uma conexão remota, os processos de logon interativo tem concluído com êxito.

RDP gerencia as credenciais inseridas pelo usuário usando o cliente de área de trabalho remota. Essas credenciais são destinadas ao computador de destino e o usuário deve ter uma conta no computador de destino. Além disso, o computador de destino deve ser configurado para aceitar uma conexão remota. As credenciais do computador de destino são enviadas para a tentativa de executar o processo de autenticação. Se a autenticação for bem-sucedida, o usuário está conectado ao local e recursos de rede que estão acessíveis usando as credenciais fornecidas.

## <a name="BKMK_NetworkLogon"></a>Logon de rede
Um logon de rede só pode ser usado após a autenticação de usuário, o serviço ou o computador tiver sido feita. Durante o logon de rede, o processo não usa as caixas de diálogo de entrada de credenciais para coletar dados. Em vez disso, as credenciais estabelecida anteriormente ou outro método para coletar credenciais é usado. Esse processo confirma a identidade do usuário para qualquer serviço de rede que o usuário está tentando acessar. Esse processo normalmente é invisível para o usuário, a menos que as credenciais alternativas precisam ser fornecidos.

Para fornecer esse tipo de autenticação, o sistema de segurança inclui esses mecanismos de autenticação:

-   Protocolo Kerberos versão 5

-   Certificados de chave pública

-   Secure Sockets Layer/Transport Layer Security (SSL/TLS)

-   Digest

-   NTLM, para compatibilidade com sistemas baseados em Microsoft Windows NT 4.0

Para obter informações sobre os elementos e os processos, consulte o diagrama de logon interativo acima.

## <a name="BKMK_SmartCardLogon"></a>Logon de cartão inteligente
Cartões inteligentes podem ser usados para fazer logon apenas para contas de domínio, contas não locais. Autenticação de cartão inteligente requer o uso do protocolo de autenticação Kerberos. Introduzidos no Windows 2000 Server, em sistemas operacionais baseados em Windows uma extensão de chave pública para a solicitação de autenticação inicial do protocolo é implementada do Kerberos. Em contraste com a criptografia de chave secreta compartilhada, a criptografia de chave pública é assimétrica, ou seja, duas chaves diferentes são necessários: uma para criptografar, outra para descriptografar. Juntas, as chaves que são necessárias para executar as duas operações formam um par de chaves pública/privada.

Para iniciar uma sessão de logon típico, um usuário deve provar sua identidade, fornecendo informações conhecidas somente o usuário e a infraestrutura subjacente do protocolo Kerberos. As informações de segredo são uma chave de criptografia compartilhada derivada da senha do usuário. Uma chave secreta compartilhada é simétrica, que significa que a mesma chave é usada para criptografia e descriptografia.

O diagrama a seguir mostra os elementos e os processos necessários para logon de cartão inteligente.

![Diagrama que mostra os elementos e os processos necessários para logon de cartão inteligente](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Arquitetura de provedor de credenciais de cartão inteligente**

Quando um cartão inteligente é usado em vez de uma senha, um par de chaves pública/privada armazenado no cartão inteligente do usuário é substituído para a chave secreta compartilhada, que é derivada da senha do usuário. A chave privada é armazenada somente em cartão inteligente. A chave pública pode ser disponibilizada para qualquer pessoa com quem o proprietário deseja que a troca de informações confidenciais.

Para obter mais informações sobre o processo de logon do cartão inteligente do Windows, consulte [como logon do cartão inteligente funciona no Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Logon biométrico
Um dispositivo é usado para capturar e criar uma característica digital de um artefato, como uma impressão digital. Essa representação digital é comparada a uma amostra do artefato mesmo, e quando as duas são comparadas com êxito, a autenticação pode ocorrer. Computadores que executam qualquer um dos sistemas operacionais designados na **aplica-se a** no início deste tópico pode ser configurado para aceitar esse formulário de logon. No entanto, se o logon biométrico está configurado somente para logon local, o usuário precisará apresentar credenciais de domínio ao acessar um domínio do Active Directory.

## <a name="additional-resources"></a>Recursos adicionais
Para obter informações sobre como o Windows gerencia as credenciais enviadas durante o processo de logon, consulte [gerenciamento de credenciais na autenticação do Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Visão geral técnica de Logon do Windows e autenticação](https://technet.microsoft.com/library/dn169029.aspx)


