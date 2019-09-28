---
title: Visão geral da autenticação do Windows
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 262a510e0b8484f3ee5cc28726f10857f92cbfd4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403284"
---
# <a name="windows-authentication-overview"></a>Visão geral da autenticação do Windows

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de navegação para profissionais de TI lista os recursos de documentação para as tecnologias de logon e autenticação do Windows que incluem avaliação do produto, guias de introdução, procedimentos, guias de design e implantação, referências técnicas e referências de comandos.

## <a name="feature-description"></a>Descrição do recurso
A autenticação é um processo de verificação da identidade de um objeto, um serviço ou uma pessoa. Quando você autentica um objeto, a meta é verificar se ele é genuíno. Ao autenticar um serviço ou uma pessoa, a meta é verificar se as credenciais apresentadas são autênticas.

Em um contexto de rede, a autenticação é o ato de fornecer identidade para um aplicativo ou recurso de rede. Normalmente, a identidade é comprovada por uma operação criptográfica que usa uma chave somente o usuário sabe-como com criptografia de chave pública – ou uma chave compartilhada. O lado do servidor da troca de autenticação compara os dados assinados com uma chave de criptografia conhecida para validar a tentativa de autenticação.

O armazenamento das chaves criptográficas em um local central seguro torna o processo de autenticação escalonável e passível de manutenção. Active Directory Domain Services é a tecnologia recomendada e padrão para armazenar informações de identidade \(including as chaves de criptografia que são as credenciais do usuário @ no__t-1. O Active Directory é exigido para as implementações de Kerberos e NTLM padrão.

As técnicas de autenticação variam de um logon simples, que identifica os usuários com base em algo que apenas o usuário sabe, como uma senha, para mecanismos de segurança mais poderosos que usam algo que o usuário tem tokens, certificados de chave pública e métrica. Em um ambiente de negócios, os serviços ou usuários podem acessar vários aplicativos ou recursos em muitos tipos de servidores em um único local ou em vários locais. Por esses motivos, a autenticação deve dar suporte a ambientes de outras plataformas e de outros sistemas operacionais Windows.

O sistema operacional Windows implementa um conjunto padrão de protocolos de autenticação, incluindo Kerberos, NTLM, Transport Layer Security @ no__t-0Secure Sockets Layer \(TLS @ no__t-2SSL @ no__t-3 e Digest, como parte de uma arquitetura extensível. Além disso, alguns protocolos são combinados com os pacotes de autenticação, como Negotiate e Credential Security Support Provider. Estes protocolos e pacotes permitem a autenticação de usuários, computadores e serviços; o processo de autenticação, por sua vez, permite que os usuários e os serviços autorizados acessem recursos de forma segura.

Para obter mais informações sobre a Autenticação do Windows, incluindo

-   [Conceitos de autenticação do Windows](windows-authentication-concepts.md)

-   [Cenários de logon do Windows](windows-logon-scenarios.md)

-   [Arquitetura de autenticação do Windows](windows-authentication-architecture.md)

-   [Arquitetura de Interface SSPI](security-support-provider-interface-architecture.md)

-   [Processos de credenciais na autenticação do Windows](credentials-processes-in-windows-authentication.md)

-   [Configurações da Política de Grupo usada na autenticação do Windows](group-policy-settings-used-in-windows-authentication.md)

consulte a [visão geral técnica da autenticação do Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Aplicações práticas
A Autenticação do Windows é usada para verificar se a informação vem de uma fonte confiável, de uma pessoa ou objeto de computador, como outro computador. O Windows fornece muitos métodos diferentes para alcançar essa meta conforme descrito abaixo.

|Para...|Recurso|Descrição|
|----|------|--------|
|Autenticar em um domínio do Active Directory|Kerberos|Os sistemas operacionais Microsoft Windows @ no__t-0Server implementam o protocolo de autenticação Kerberos versão 5 e as extensões para autenticação de chave pública. O cliente de autenticação Kerberos é implementado como um provedor de suporte de segurança \(SSP @ no__t-1 e pode ser acessado por meio da interface do provedor de suporte de segurança \(SSPI @ no__t-3. A autenticação de usuário inicial é integrada com a arquitetura de logon único do Winlogon @ no__t-0on. O Kerberos centro de distribuição de chaves \(KDC @ no__t-1 é integrado a outros serviços de segurança do Windows Server em execução no controlador de domínio. O KDC usa o banco de dados do serviço de diretório Active Directory do domínio como seu banco de dados de conta de segurança. O Active Directory é exigido para as implementações de Kerberos padrão.<br /><br />Para obter recursos adicionais, consulte [visão geral da autenticação Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticação segura na Web|TLS @ no__t-0SSL conforme implementado no provedor de suporte de segurança Schannel|O protocolo de segurança de camada de transporte \(TLS @ no__t-1 versões 1,0, 1,1 e 1,2, protocolo SSL \(SSL @ no__t-3, versões 2,0 e 3,0, protocolo de segurança da camada de transporte de datatransport versão 1,0 e o transporte de comunicações privadas o protocolo \(PCT @ no__t-5, versão 1,0, baseia-se na criptografia de chave pública. O pacote de protocolo de autenticação do provedor Secure Channel \(Schannel @ no__t-1 fornece esses protocolos. Todos os protocolos Schannel usam um modelo de cliente e de servidor.<br /><br />Para obter recursos adicionais, consulte [visão geral &#40;de TLS&#41; -SSL Schannel SSP](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticar em um aplicativo ou serviço Web|Autenticação Integrada do Windows<br /><br />Autenticação Digest|Para obter recursos adicionais, consulte autenticação [integrada do Windows](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx) e [autenticação Digest](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx)e [autenticação Digest avançada](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticar em aplicativos herdados|NTLM|NTLM é um protocolo de autenticação de estilo Challenge @ no__t-0response. Além da autenticação, o protocolo NTLM fornece opcionalmente a segurança da sessão, especificamente a integridade e a confidencialidade das mensagens por meio de funções de assinatura e lacre no NTLM.<br /><br />Para obter recursos adicionais, veja [Visão geral de NTLM](../kerberos/ntlm-overview.md).|
|Aproveitar autenticação multifator|Suporte de cartão inteligente<br /><br />Suporte biométrico|Os cartões inteligentes são um modo de violação @ no__t-0resistant e portátil para fornecer soluções de segurança para tarefas como autenticação de cliente, logon em domínios, assinatura de código e proteção de e @ no__t-1mail.<br /><br />A biometria depende da medição de uma característica física imutável de uma pessoa para identificá-la. As impressões digitais são uma das características biométricas mais usadas, com milhões de dispositivos biométricos de impressões digitais que são incorporados a computadores pessoais e periféricos.<br /><br />Para obter recursos adicionais, consulte [referência técnica do cartão inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Oferecer gerenciamento local, armazenamento e reutilização de credenciais|Gerenciamento de credenciais<br /><br />Autoridade de Segurança Local<br /><br />Senhas|O gerenciamento de credenciais no Windows garante que as credenciais sejam armazenadas com segurança. As credenciais são coletadas no Secure desktop \(for local ou no acesso ao domínio @ no__t-1, por meio de aplicativos ou por meio de sites para que as credenciais corretas sejam apresentadas sempre que um recurso for acessado.<br /><br />
|Estender a moderna proteção de autenticação aos sistemas herdados|Proteção estendida para autenticação|Esse recurso aprimora a proteção e a manipulação de credenciais ao autenticar conexões de rede usando a autenticação integrada do Windows \(IWA @ no__t-1.|

## <a name="software-requirements"></a>Requisitos de software
A Autenticação do Windows é projetada para ser compatível com as versões anteriores no sistema operacional Windows. Porém, os aprimoramentos de cada versão não são necessariamente aplicáveis às versões anteriores. Consulte a documentação sobre os recursos específicos para obter mais informações.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor
Muitos recursos de autenticação podem ser configurados usando a Política de Grupo, que pode ser instalada usando o Gerenciador do Servidor. O recurso Windows Biometric Framework é instalado usando o Gerenciador do Servidor. Outras funções de servidor que dependem de métodos de autenticação, como o servidor Web \(IIS @ no__t-1 e Active Directory Domain Services, também podem ser instaladas usando Gerenciador do Servidor.

## <a name="related-resources"></a>Recursos relacionados

|Tecnologias de autenticação|Recursos|
|----------------|-------|
|Autenticação do Windows|[Visão geral técnica de autenticação do Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Inclui tópicos que abordam diferenças entre versões, conceitos gerais de autenticação, cenários de logon, arquiteturas para versões com suporte e configurações aplicáveis.|
|Kerberos|[Visão geral da autenticação Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Visão geral da delegação restrita de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Referência técnica de autenticação Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003 @ no__t-2<br /><br />[Guia de sobrevivência Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet wiki @ no__t-2|
|TLS @ no__t-0SSL e DTLS \(Schannel provedor de suporte de segurança @ no__t-2|[Visão geral de TLS do Schannel do protocolo SSL &#40;&#41;](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Referência técnica do provedor de suporte de segurança Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticação digest|[Referência técnica de autenticação Digest](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003 @ no__t-2|
|NTLM|[Visão geral do NTLM](../kerberos/ntlm-overview.md)<br />Contém links para os recursos atuais e antigos|
|PKU2U|[Introdução ao PKU2U no Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Cartão inteligente|[Referência técnica do cartão inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenciais|[Proteção e gerenciamento de credenciais](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contém links para os recursos atuais e antigos<br /><br />[Visão geral de senhas](../kerberos/passwords-overview.md)<br />Contém links para os recursos atuais e antigos|


