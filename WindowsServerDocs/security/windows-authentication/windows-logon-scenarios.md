---
title: Cenários de logon do Windows
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9a953b22b39a20557103fa84a5d6d5e42e753444
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861699"
---
# <a name="windows-logon-scenarios"></a>Cenários de logon do Windows

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico de referência para o profissional de ti resume os cenários comuns de logon e entrada do Windows.

Os sistemas operacionais Windows exigem que todos os usuários façam logon no computador com uma conta válida para acessar os recursos locais e de rede. Os computadores baseados no Windows protegem recursos implementando o processo de logon, no qual os usuários são autenticados. Depois que um usuário é autenticado, as tecnologias de autorização e controle de acesso implementam a segunda fase de proteger recursos: determinando se o usuário autenticado está autorizado a acessar um recurso.

O conteúdo deste tópico se aplica às versões do Windows designadas na lista **aplica-se a** no início deste tópico.

Além disso, os aplicativos e serviços podem exigir que os usuários entrem para acessar os recursos que são oferecidos pelo aplicativo ou serviço. O processo de entrada é semelhante ao processo de logon, pois uma conta válida e as credenciais corretas são necessárias, mas as informações de logon são armazenadas no banco de dados SAM (Gerenciador de contas de segurança) no computador local e no Active Directory quando aplicável. A conta de entrada e as informações de credenciais são gerenciadas pelo aplicativo ou serviço e, opcionalmente, podem ser armazenadas localmente no armário de credenciais.

Para entender como funciona a autenticação, consulte [conceitos de autenticação do Windows](windows-authentication-concepts.md).

Este tópico descreve os seguintes cenários:

-   [Logon interativo](#BKMK_InteractiveLogon)

-   [Logon de rede](#BKMK_NetworkLogon)

-   [Logon de cartão inteligente](#BKMK_SmartCardLogon)

-   [Logon biométrico](#BKMK_BioLogon)

## <a name="interactive-logon"></a><a name="BKMK_InteractiveLogon"></a>Logon interativo
O processo de logon começa quando um usuário digita as credenciais na caixa de diálogo entrada de credenciais ou quando o usuário insere um cartão inteligente no leitor de cartão inteligente ou quando o usuário interage com um dispositivo biométrico. Os usuários podem executar um logon interativo usando uma conta de usuário local ou uma conta de domínio para fazer logon em um computador.

O diagrama a seguir mostra os elementos de logon interativos e o processo de logon.

![Diagrama mostrando os elementos de logon interativos e o processo de logon](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Arquitetura de autenticação de cliente do Windows**

### <a name="local-and-domain-logon"></a><a name="BKMK_LocaDomainLogon"></a>Logon local e de domínio
As credenciais que o usuário apresenta para um logon de domínio contêm todos os elementos necessários para um logon local, como nome da conta, senha ou certificado, e Active Directory informações de domínio. O processo confirma a identificação do usuário para o banco de dados de segurança no computador local do usuário ou em um domínio Active Directory. Esse processo de logon obrigatório não pode ser desativado para usuários em um domínio.

Os usuários podem executar um logon interativo em um computador de duas maneiras:

-   Localmente, quando o usuário tem acesso físico direto ao computador ou quando o computador faz parte de uma rede de computadores.

    Um logon local concede a um usuário permissão para acessar recursos do Windows no computador local. Um logon local requer que o usuário tenha uma conta de usuário no SAM (Gerenciador de contas de segurança) no computador local. O SAM protege e gerencia informações de usuário e grupo na forma de contas de segurança armazenadas no registro do computador local. O computador pode ter acesso à rede, mas não é necessário. As informações de conta de usuário local e Associação de grupo são usadas para gerenciar o acesso aos recursos locais.

    Um logon de rede concede a um usuário permissão para acessar recursos do Windows no computador local, além de quaisquer recursos em computadores em rede, conforme definido pelo token de acesso da credencial. Um logon local e um logon de rede exigem que o usuário tenha uma conta de usuário no SAM (Gerenciador de contas de segurança) no computador local. As informações de conta de usuário local e Associação de grupo são usadas para gerenciar o acesso a recursos locais, e o token de acesso para o usuário define quais recursos podem ser acessados em computadores em rede.

    Um logon local e um logon de rede não são suficientes para conceder ao usuário e ao computador permissão para acessar e usar recursos de domínio.

-   Remotamente, por meio de serviços de terminal ou Serviços de Área de Trabalho Remota (RDS), nesse caso, o logon é ainda mais qualificado como interativo remoto.

Após um logon interativo, o Windows executa aplicativos em nome do usuário, e o usuário pode interagir com esses aplicativos.

Um logon local concede a um usuário permissão para acessar recursos no computador local ou recursos em computadores em rede. Se o computador tiver ingressado em um domínio, a funcionalidade do Winlogon tentará fazer logon nesse domínio.

Um logon de domínio concede a um usuário permissão para acessar recursos locais e de domínio. Um logon de domínio requer que o usuário tenha uma conta de usuário no Active Directory. O computador deve ter uma conta no domínio Active Directory e estar fisicamente conectado à rede. Os usuários também devem ter direitos de usuário para fazer logon em um computador local ou em um domínio. As informações de conta de usuário de domínio e informações de associação de grupo são usadas para gerenciar o acesso a recursos locais e de domínio.

### <a name="remote-logon"></a><a name="BKMK_RemoteLogon"></a>Logon remoto
No Windows, o acesso a outro computador por meio de logon remoto depende do protocolo RDP (RDP). Como o usuário já deve ter feito logon com êxito no computador cliente antes de tentar uma conexão remota, os processos de logon interativos foram concluídos com êxito.

O RDP gerencia as credenciais que o usuário insere usando o cliente Área de Trabalho Remota. Essas credenciais são destinadas ao computador de destino e o usuário deve ter uma conta nesse computador de destino. Além disso, o computador de destino deve ser configurado para aceitar uma conexão remota. As credenciais do computador de destino são enviadas para tentar executar o processo de autenticação. Se a autenticação for bem-sucedida, o usuário será conectado a recursos locais e de rede que são acessíveis usando as credenciais fornecidas.

## <a name="network-logon"></a><a name="BKMK_NetworkLogon"></a>Logon de rede
Um logon de rede só pode ser usado após o usuário, o serviço ou a autenticação do computador ter ocorrido. Durante o logon da rede, o processo não usa as caixas de diálogo de entrada de credenciais para coletar dados. Em vez disso, as credenciais estabelecidas anteriormente ou outro método para coletar credenciais são usadas. Esse processo confirma a identidade do usuário para qualquer serviço de rede que o usuário está tentando acessar. Esse processo é normalmente invisível para o usuário, a menos que credenciais alternativas precisem ser fornecidas.

Para fornecer esse tipo de autenticação, o sistema de segurança inclui estes mecanismos de autenticação:

-   Protocolo Kerberos versão 5

-   Certificados de chave pública

-   Segurança de camada de protocolo SSL/transporte (SSL/TLS)

-   Digest

-   NTLM, para compatibilidade com sistemas baseados no Microsoft Windows NT 4,0

Para obter informações sobre os elementos e processos, consulte o diagrama de logon interativo acima.

## <a name="smart-card-logon"></a><a name="BKMK_SmartCardLogon"></a>Logon de cartão inteligente
Os cartões inteligentes podem ser usados para fazer logon somente em contas de domínio, não em contas locais. A autenticação de cartão inteligente requer o uso do protocolo de autenticação Kerberos. Introduzido no Windows 2000 Server, em sistemas operacionais baseados no Windows, uma extensão de chave pública para a solicitação de autenticação inicial do protocolo Kerberos é implementada. Ao contrário da criptografia de chave secreta compartilhada, a criptografia de chave pública é assimétrica, ou seja, duas chaves diferentes são necessárias: uma para criptografar, outra para descriptografar. Juntas, as chaves que são necessárias para executar ambas as operações compõem um par de chaves privada/pública.

Para iniciar uma sessão de logon típica, um usuário deve provar sua identidade fornecendo informações conhecidas apenas para o usuário e a infraestrutura de protocolo Kerberos subjacente. As informações secretas são uma chave compartilhada criptográfica derivada da senha do usuário. Uma chave secreta compartilhada é simétrica, o que significa que a mesma chave é usada para criptografia e descriptografia.

O diagrama a seguir mostra os elementos e processos necessários para o logon do cartão inteligente.

![Diagrama mostrando os elementos e os processos necessários para o logon do cartão inteligente](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Arquitetura do provedor de credenciais de cartão inteligente**

Quando um cartão inteligente é usado em vez de uma senha, um par de chaves privada/pública armazenado no cartão inteligente do usuário é substituído pela chave secreta compartilhada, que é derivada da senha do usuário. A chave privada é armazenada somente no cartão inteligente. A chave pública pode ser disponibilizada para qualquer pessoa com a qual o proprietário deseja trocar informações confidenciais.

Para obter mais informações sobre o processo de logon do cartão inteligente no Windows, consulte [como o logon do cartão inteligente funciona no Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="biometric-logon"></a><a name="BKMK_BioLogon"></a>Logon biométrico
Um dispositivo é usado para capturar e criar uma característica digital de um artefato, como uma impressão digital. Em seguida, essa representação digital é comparada a uma amostra do mesmo artefato e quando as duas são comparadas com êxito, a autenticação pode ocorrer. Os computadores que executam qualquer um dos sistemas operacionais designados na lista **aplica-se a ao** início deste tópico podem ser configurados para aceitar essa forma de logon. No entanto, se o logon biométrico for configurado apenas para logon local, o usuário precisará apresentar as credenciais de domínio ao acessar um domínio de Active Directory.

## <a name="additional-resources"></a>Recursos adicionais
Para obter informações sobre como o Windows gerencia as credenciais enviadas durante o processo de logon, consulte [Gerenciamento de credenciais na autenticação do Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Visão geral técnica de logon e autenticação do Windows](https://technet.microsoft.com/library/dn169029.aspx)


