---
title: "Visão geral de autenticação do Windows"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c2ec55ed6b09628d1d80a24be766259980e84d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-overview"></a>Visão geral de autenticação do Windows

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de navegação para o profissional de TI lista os recursos de documentação para tecnologias de logon e autenticação Windows que estão avaliação do produto, obtendo guias de Introdução, procedimentos, guias de design e implantação, referências técnicas e referências de comando.

## <a name="feature-description"></a>Descrição dos recursos
Autenticação é um processo para verificar a identidade de um objeto, o serviço ou a pessoa. Quando você autentica um objeto, o objetivo é verificar se o objeto é original. Quando você autentica um serviço ou a pessoa, o objetivo é verificar se as credenciais apresentadas são autênticas.

Em um contexto de rede, a autenticação é o ato de provar a identidade de um aplicativo de rede ou recurso. Normalmente, a identidade é comprovada por uma operação criptográfica que use qualquer uma chave que somente o usuário sabe - com criptografia de chave pública - ou uma chave compartilhada. O lado do servidor do exchange a autenticação compara os dados assinados com uma chave criptográfica conhecida para validar a tentativa de autenticação.

Armazenando as chaves de criptografia em um local central seguro torna o processo de autenticação pode ser mantido e escalonável. Serviços de domínio do Active Directory é recomendado e tecnologia padrão para armazenar informações de identidade \ (incluindo as chaves de criptografia que são credentials\ do usuário). Active Directory é necessário para implementações de Kerberos e NTLM padrão.

Variam de técnicas de autenticação de um logon simple, que identifica os usuários com base em algo que somente o usuário sabe - como uma senha, mais potentes mecanismos de segurança que usam algo que o usuário tenha - como tokens, certificados de chave pública e biometria. Em um ambiente de negócios, serviços ou os usuários podem acessar vários aplicativos ou recursos em vários tipos de servidores em um único local ou em vários locais. Por esses motivos, autenticação deve dar suporte a ambientes para outras plataformas e para outros sistemas operacionais Windows.

O sistema operacional Windows implementa um conjunto padrão de protocolos de autenticação, incluindo Kerberos, NTLM, Transport Layer Security\/Secure Sockets Layer \(TLS\/SSL\) e Digest, como parte de uma arquitetura extensível. Além disso, alguns protocolos são combinados em pacotes de autenticação como negociar e Credential Security Support Provider. Esses protocolos e pacotes de habilitar a autenticação de usuários, computadores e serviços; o processo de autenticação, por sua vez, permite que os usuários autorizados e serviços acessar recursos de maneira segura.

Para obter mais informações sobre a inclusão de autenticação do Windows

-   [Conceitos de autenticação do Windows](windows-authentication-concepts.md)

-   [Cenários de Logon do Windows](windows-logon-scenarios.md)

-   [Arquitetura de autenticação do Windows](windows-authentication-architecture.md)

-   [Arquitetura de Interface do provedor de suporte de segurança](security-support-provider-interface-architecture.md)

-   [Processos de credenciais de autenticação do Windows](credentials-processes-in-windows-authentication.md)

-   [Configurações de política de grupo usadas na autenticação do Windows](group-policy-settings-used-in-windows-authentication.md)

Consulte o [visão geral técnica do Windows autenticação](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Aplicativos práticos
Autenticação do Windows é usada para verificar se as informações provém de uma fonte confiável, seja a partir de um objeto pessoa ou computador, como outro computador. O Windows fornece vários métodos diferentes para atingir esse meta, conforme descrito a seguir.

|Para...|Recurso|Descrição|
|----|------|--------|
|Autenticar em um domínio do Active Directory|Kerberos|O Microsoft Windows&nbsp;sistemas operacionais de servidor implementar o protocolo de autenticação Kerberos versão 5 e extensões para autenticação de chave pública. O cliente de autenticação Kerberos é implementado como um provedor de suporte de segurança \(SSP\) e podem ser acessados por meio de \(SSPI\) a Interface do provedor de suporte de segurança. Autenticação de usuário inicial é integrada com a único Winlogon sign\ na arquitetura. O Centro de distribuição de chaves Kerberos \(KDC\) é integrado com outros serviços de segurança do Windows Server em execução no controlador de domínio. O KDC usa o banco de dados do serviço diretório do domínio do Active Directory como seu banco de dados de conta de segurança. Active Directory é necessário para implementações de Kerberos padrão.<br /><br />Para obter recursos adicionais, consulte [visão geral de autenticação Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticação segura na web|TLS\/SSL conforme implementado no Schannel Security Support Provider|A Transport Layer Security \(TLS\) protocolo versões 1.0, 1.1 e 1.2, protocolo Secure Sockets Layer \(SSL\), versão do protocolo de versões 2.0 e 3.0, datagrama Transport Layer Security 1.0 e o protocolo de transporte de comunicações privadas \(PCT\), versão 1.0, são baseadas em criptografia de chave pública. O conjunto de protocolos de autenticação do canal seguro \(Schannel\) provedor fornece esses protocolos. Todos os protocolos Schannel usam um modelo de cliente e servidor.<br /><br />Para obter recursos adicionais, consulte [TLS - SSL & #40; Schannel SSP & #41; Visão geral](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticar em um serviço web ou aplicativo|Autenticação integrada do Windows<br /><br />Autenticação Digest|Para obter recursos adicionais, consulte [autenticação integrada do Windows] (https://technet.microsoft.com/library/cc758557(v=WS.10.aspx and [Digest Authentication](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), and [Advanced Digest Authentication](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticar em aplicativos herdados|NTLM|O protocolo NTLM NTLM é um acréscimo de autenticação protocol.In de estilo challenge\ resposta para autenticação, opcionalmente fornece para a segurança de sessão – especificamente a integridade da mensagem e a confidencialidade por meio de assinatura e o fechamento funções em NTLM.<br /><br />Para obter recursos adicionais, consulte [visão geral de NTLM](../kerberos/ntlm-overview.md).|
|Autenticação multifator aproveitamento|Suporte a cartões inteligentes<br /><br />Suporte a biometria|Cartões inteligentes são uma maneira tamper\ resistente e portátil para fornecer soluções de segurança para tarefas como autenticação de cliente, fazer logon em domínios, assinatura do código e proteger e\ mail.<br /><br />Biometria depende de medir uma característica física sem alteração de uma pessoa para identificar exclusivamente essa pessoa. Impressões digitais são um das características biométricas usadas com mais frequência, com milhões de dispositivos biométricos de impressão digital que estão incorporados em computadores pessoais e periféricos.<br /><br />Para obter recursos adicionais, consulte [referência técnica do cartão inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Fornecer gerenciamento local, armazenamento e reutilização de credenciais|Gerenciamento de credenciais<br /><br />Autoridade de segurança local<br /><br />Senhas|Gerenciamento de credenciais no Windows garante que as credenciais são armazenadas com segurança. As credenciais são coletadas na área de trabalho segura \ (para acesso local ou de domínio), por meio de aplicativos ou sites para que as credenciais corretas são apresentadas sempre que um recurso é acessado.<br /><br />
|Estender a proteção de autenticação modernos para sistemas herdados|Proteção estendida para autenticação|Esse recurso aumenta a proteção e manipulação de credenciais durante a autenticação de conexões de rede usando \(IWA\) autenticação integrada do Windows.|

## <a name="software-requirements"></a>Requisitos de software
Autenticação do Windows é projetada para ser compatível com versões anteriores do sistema operacional Windows. No entanto, melhorias com cada lançamento não são necessariamente aplicáveis a versões anteriores. Consulte a documentação sobre recursos específicos para obter mais informações.

## <a name="server-manager-information"></a>Informações do Gerenciador do servidor
Muitos recursos de autenticação podem ser configurados usando a política de grupo, que podem ser instalados usando o Gerenciador do servidor. O recurso Windows Biometric Framework é instalado usando o Gerenciador do servidor. Outras funções de servidor que variam de acordo com os métodos de autenticação, como o servidor Web \(IIS\) e serviços de domínio do Active Directory, também podem ser instaladas usando o Gerenciador do servidor.

## <a name="related-resources"></a>Recursos relacionados

|Tecnologias de autenticação|Recursos|
|----------------|-------|
|Autenticação do Windows|[Visão geral técnica de autenticação do Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Inclui tópicos de endereçamento diferenças entre versões, conceitos gerais de autenticação, cenários de logon, arquiteturas para as versões com suporte e as configurações aplicáveis.|
|Kerberos|[Visão geral de autenticação Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Visão geral de delegação restrita de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Referência técnica de autenticação Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Guia de sobrevivência Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL e DTLS \ (Schannel provider\ de suporte de segurança)|[TLS - SSL & #40; Schannel SSP & #41; Visão geral](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Referência de provedor de suporte técnico segurança Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticação Digest|[Referência técnica de autenticação de Digest](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Visão geral NTLM](../kerberos/ntlm-overview.md)<br />Contém links para recursos atuais e anteriores|
|PKU2U|[Apresentando PKU2U no Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Cartão inteligente|[Referência técnica do cartão inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenciais|[Gerenciamento e proteção de credenciais](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contém links para recursos atuais e anteriores<br /><br />[Visão geral de senhas](../kerberos/passwords-overview.md)<br />Contém links para recursos atuais e anteriores|


