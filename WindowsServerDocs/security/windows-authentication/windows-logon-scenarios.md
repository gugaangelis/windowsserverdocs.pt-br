---
title: "Cenários de Logon do Windows"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="windows-logon-scenarios"></a>Cenários de Logon do Windows

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de referência para o profissional de TI resume comuns logon do Windows e cenários de entrada.

Os sistemas operacionais Windows exigem que todos os usuários para logon no computador com uma conta válida para acesso local e os recursos de rede. Computadores baseados em Windows seguro recursos Implementando o processo de logon, em que os usuários são autenticados. Depois que um usuário é autenticado, tecnologias de controle de acesso e autorização implementam a segunda fase de proteger os recursos: determinar se o usuário autenticado está autorizado a acessar um recurso.

O conteúdo deste tópico se aplicam a versões do Windows indicadas no **se aplica a** lista no início deste tópico.

Além disso, aplicativos e serviços podem exigir que os usuários efetuar login para acessar os recursos que são oferecidos pelo aplicativo ou serviço. O processo de entrada é semelhante ao processo de logon, que são necessárias uma conta válida e credenciais corretas, mas informações de logon são armazenadas no banco de dados Gerenciador de contas de segurança (SAM) no computador local e no Active Directory quando aplicável. Conta e credencial informações de entrada são gerenciadas pelo aplicativo ou serviço e, opcionalmente, podem ser armazenadas localmente no cofre de credenciais.

Para entender como a autenticação funciona, consulte [conceitos de autenticação do Windows](windows-authentication-concepts.md).

Este tópico descreve os cenários a seguir:

-   [Logon interativo](#BKMK_InteractiveLogon)

-   [Logon de rede](#BKMK_NetworkLogon)

-   [Logon com cartão inteligente](#BKMK_SmartCardLogon)

-   [Logon biométrico](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Logon interativo
O processo de logon começa quando um usuário insere as credenciais na caixa de diálogo entrada de credenciais, ou quando o usuário insere um cartão inteligente no leitor de cartão inteligente ou quando o usuário interage com um dispositivo biométrico. Os usuários podem realizar um logon interativo, usando uma conta de usuário local ou uma conta de domínio para fazer logon um computador.

O diagrama a seguir mostra os elementos de logon interativo e o processo de logon.

![Diagrama mostrando os elementos de logon interativo e o processo de logon](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Arquitetura de autenticação de cliente do Windows**

### <a name="BKMK_LocaDomainLogon"></a>Logon local e de domínio
As credenciais que apresenta ao usuário para um logon de domínio contenham todos os elementos necessários para um logon local, como o nome da conta e senha ou certificado e informações de domínio do Active Directory. O processo confirma a identificação do usuário para o banco de dados de segurança no computador local do usuário ou para um domínio do Active Directory. Esse processo de logon obrigatório não pode ser desativado para os usuários em um domínio.

Os usuários podem realizar um logon interativo em um computador em uma destas duas maneiras:

-   Localmente, quando o usuário tem acesso físico direto para o computador, ou quando o computador fizer parte de uma rede de computadores.

    Um logon local concede uma permissão do usuário para acessar recursos do Windows no computador local. Um logon local requer que o usuário tenha uma conta de usuário no Gerenciador de contas de segurança (SAM) no computador local. O SAM protege e gerencia o usuário e informações de grupo na forma de contas de segurança armazenadas no registro do computador local. O computador pode ter acesso à rede, mas não é necessária. Informações de associação de grupo e de conta de usuário local são usadas para gerenciar o acesso a recursos locais.

    Um logon de rede concede uma permissão do usuário para acessar recursos do Windows no computador local, além de todos os recursos em computadores em rede, conforme definido pelo token de acesso da credencial. Um logon local e um logon de rede exigem que o usuário tenha uma conta de usuário no Gerenciador de contas de segurança (SAM) no computador local. Informações de associação de grupo e de conta de usuário local são usadas para gerenciar o acesso a recursos locais e o token de acesso para o usuário define quais recursos podem ser acessados em computadores em rede.

    Um logon local e um logon de rede não são suficientes para conceder a permissão do usuário e o computador para acessar e usar recursos do domínio.

-   Remotamente, usando os serviços de Terminal ou serviços de área de trabalho remota (RDS), em cujo caso a configuração logon é mais qualificados como remoto interativo.

Após um logon interativo, o Windows executa aplicativos em nome do usuário e o usuário pode interagir com os aplicativos.

Um logon local concede uma permissão do usuário para acessar recursos no computador local ou recursos em computadores em rede. Se o computador tiver ingressado em um domínio, a funcionalidade do Winlogon tenta fazer logon no domínio.

Um logon de domínio concede uma permissão do usuário para acessar locais e recursos do domínio. Um logon de domínio requer que o usuário tenha uma conta de usuário no Active Directory. O computador deve ter uma conta no domínio do Active Directory e ser fisicamente conectado à rede. Os usuários também devem ter os direitos de usuário fazer logon um computador local ou de um domínio. Informações de conta de usuário de domínio e informações de associação de grupo são usados para gerenciar o acesso ao domínio e recursos locais.

### <a name="BKMK_RemoteLogon"></a>Logon remoto
No Windows, acessar outro computador por meio de logon remoto se baseia no protocolo de área de trabalho remota (RDP). Porque o usuário deve já ter logon com êxito para o computador cliente antes de tentar uma conexão remota, processos de logon interativo tiverem concluído com êxito.

RDP gerencia as credenciais que o usuário insere usando o cliente de área de trabalho remota. Essas credenciais destinam-se para o computador de destino e o usuário deve ter uma conta no computador de destino. Além disso, o computador de destino deve ser configurado para aceitar uma conexão remota. As credenciais do computador de destino são enviadas para tentar realizar o processo de autenticação. Se a autenticação for bem-sucedida, o usuário está conectado ao local e recursos de rede que estão acessíveis usando as credenciais fornecidas.

## <a name="BKMK_NetworkLogon"></a>Logon de rede
Um logon de rede só pode ser usado após a autenticação de usuário, o serviço ou o computador tiver sido feita. Durante o logon de rede, o processo não usa as caixas de diálogo de entrada de credenciais para coletar dados. Em vez disso, estabelecida anteriormente credenciais ou outro método para coletar as credenciais é usado. Esse processo confirma a identidade do usuário em qualquer serviço de rede que o usuário está tentando acessar. Esse processo é geralmente invisível para o usuário, a menos que tem credenciais alternativas ser fornecido.

Para fornecer esse tipo de autenticação, o sistema de segurança inclui esses mecanismos de autenticação:

-   Protocolo Kerberos versão 5

-   Certificados de chave pública

-   Secure Sockets Layer/Transport Layer Security (TLS/SSL)

-   Resumo

-   NTLM, para compatibilidade com sistemas baseados em Microsoft Windows NT 4.0

Para obter informações sobre os elementos e processos, consulte o diagrama de logon interativo acima.

## <a name="BKMK_SmartCardLogon"></a>Logon com cartão inteligente
Cartões inteligentes podem ser usados para fazer logon somente para contas de domínio, as contas locais não. Autenticação de cartão inteligente exige o uso do protocolo de autenticação Kerberos. Introduzido no Windows 2000 Server, em sistemas operacionais baseados em Windows uma extensão de chave pública para a solicitação de autenticação inicial do protocolo é implementada de Kerberos. Em contraste com criptografia de chave secreta compartilhada, criptografia de chave pública é assimétrica, ou seja, são necessárias duas chaves diferentes: um para criptografar, outra para descriptografar. Juntos, as chaves que são necessárias para executar as duas operações formam um par de chaves privada/pública.

Para iniciar uma sessão de logon típico, um usuário deve provar sua identidade, fornecendo informações conhecidas somente para o usuário e a infraestrutura de protocolo Kerberos subjacente. As informações secretas são uma chave compartilhada criptográfica derivada de senha do usuário. Uma chave secreta compartilhada é simétrica, o que significa que a mesma chave é usada para criptografia e descriptografia.

O diagrama a seguir mostra os elementos e os processos necessários para logon com cartão inteligente.

![Diagrama mostrando os elementos e os processos necessários para logon com cartão inteligente](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Arquitetura de provedor de credenciais de cartão inteligente**

Quando um cartão inteligente é usado em vez de uma senha, um par de chaves privada/pública armazenado em um cartão inteligente do usuário é substituído pela chave secreta compartilhada, que é derivada de senha do usuário. A chave privada é armazenada apenas no cartão inteligente. A chave pública pode ser disponibilizada para qualquer pessoa com quem deseja que o proprietário trocar informações confidenciais.

Para saber mais sobre o processo de logon do cartão inteligente no Windows, consulte [como entrada do cartão inteligente funciona no Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Logon biométrico
Um dispositivo é usado para capturar e criar uma característica digital de um artefato, como uma impressão digital. Essa representação digital, em seguida, é comparado com uma amostra do artefato mesmo e quando os dois são comparados com êxito, a autenticação pode ocorrer. Computadores que executam qualquer um dos sistemas operacionais designados no **se aplica a** lista no início deste tópico pode ser configurada para aceitar este formulário de logon. No entanto, se logon biométrico configurado apenas para logon local, o usuário precisa apresentar credenciais de domínio ao acessar um domínio do Active Directory.

## <a name="additional-resources"></a>Recursos adicionais
Para obter informações sobre como o Windows gerencia credenciais enviadas durante o processo de logon, consulte [gerenciamento de credenciais de autenticação do Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Visão geral técnica de Logon do Windows e autenticação](https://technet.microsoft.com/library/dn169029.aspx)


