---
title: Visão geral da autenticação do Windows
description: Segurança do Windows Server
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
ms.openlocfilehash: 2021ccf1e0015cc910f43966f9400e6bd56a230c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870967"
---
# <a name="windows-authentication-overview"></a>Visão geral da autenticação do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de navegação para profissionais de TI lista os recursos de documentação para as tecnologias de logon e autenticação do Windows que incluem avaliação do produto, guias de introdução, procedimentos, guias de design e implantação, referências técnicas e referências de comandos.

## <a name="feature-description"></a>Descrição do recurso
A autenticação é um processo de verificação da identidade de um objeto, um serviço ou uma pessoa. Quando você autentica um objeto, a meta é verificar se ele é genuíno. Ao autenticar um serviço ou uma pessoa, a meta é verificar se as credenciais apresentadas são autênticas.

Em um contexto de rede, a autenticação é o ato de fornecer identidade para um aplicativo ou recurso de rede. Normalmente, identidade está comprovada por uma operação criptográfica que usa uma chave que somente os usuários conhecem – assim como acontece com criptografia de chave pública - ou uma chave compartilhada. O lado do servidor da troca de autenticação compara os dados assinados com uma chave de criptografia conhecida para validar a tentativa de autenticação.

O armazenamento das chaves criptográficas em um local central seguro torna o processo de autenticação escalonável e passível de manutenção. Serviços de domínio do Active Directory é a recomendada e a tecnologia padrão para armazenar informações de identidade \(incluindo as chaves criptográficas que são as credenciais do usuário\). O Active Directory é exigido para as implementações de Kerberos e NTLM padrão.

Técnicas de autenticação variam de um logon simple, que identifica os usuários com base em algo que apenas o usuário sabe - como uma senha, para os mecanismos de segurança mais eficientes que usar algo que o usuário tem - como tokens, certificados de chave pública, e biometria. Em um ambiente de negócios, os serviços ou usuários podem acessar vários aplicativos ou recursos em muitos tipos de servidores em um único local ou em vários locais. Por esses motivos, a autenticação deve dar suporte a ambientes de outras plataformas e de outros sistemas operacionais Windows.

O sistema operacional Windows implementa um conjunto padrão de protocolos de autenticação, incluindo Kerberos, NTLM, Transport Layer Security\/Secure Sockets Layer \(TLS\/SSL\)e o resumo como parte de um arquitetura extensível. Além disso, alguns protocolos são combinados com os pacotes de autenticação, como Negotiate e Credential Security Support Provider. Estes protocolos e pacotes permitem a autenticação de usuários, computadores e serviços; o processo de autenticação, por sua vez, permite que os usuários e os serviços autorizados acessem recursos de forma segura.

Para obter mais informações sobre a Autenticação do Windows, incluindo

-   [Conceitos de autenticação do Windows](windows-authentication-concepts.md)

-   [Cenários de Logon do Windows](windows-logon-scenarios.md)

-   [Arquitetura de autenticação do Windows](windows-authentication-architecture.md)

-   [Arquitetura de Interface de provedor de suporte de segurança](security-support-provider-interface-architecture.md)

-   [Processos de credenciais na autenticação do Windows](credentials-processes-in-windows-authentication.md)

-   [Configurações de diretiva de grupo usadas na autenticação do Windows](group-policy-settings-used-in-windows-authentication.md)

Consulte a [visão geral técnica do Windows autenticação](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Aplicações práticas
A Autenticação do Windows é usada para verificar se a informação vem de uma fonte confiável, de uma pessoa ou objeto de computador, como outro computador. O Windows fornece muitos métodos diferentes para alcançar essa meta conforme descrito abaixo.

|Para...|Recurso|Descrição|
|----|------|--------|
|Autenticar em um domínio do Active Directory|Kerberos|O Microsoft Windows&nbsp;sistemas operacionais Server implementam o protocolo de autenticação Kerberos versão 5 e extensões para autenticação de chave pública. O cliente de autenticação Kerberos é implementado como um provedor de suporte de segurança \(SSP\) e pode ser acessado por meio da Interface de provedor de suporte de segurança \(SSPI\). Autenticação de usuário inicial é integrada com o logon único do Winlogon\-na arquitetura. O Kerberos Key Distribution Center \(KDC\) é integrado com outros serviços de segurança do Windows Server em execução no controlador de domínio. O KDC usa o banco de dados de serviço diretório de domínio do Active Directory como seu banco de dados de conta de segurança. O Active Directory é exigido para as implementações de Kerberos padrão.<br /><br />Para obter recursos adicionais, consulte [visão geral da autenticação do Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticação segura na Web|TLS\/SSL conforme implementado no provedor de suporte de segurança Schannel|A segurança de camada de transporte \(TLS\) versões 1.0, 1.1 e 1.2, protocolo SSL do protocolo \(SSL\) protocol, versão 1.0 do protocolo versões 2.0 e 3.0, Datagram Transport Layer Security e a privada Transporte de comunicações \(PCT\) protocol, versão 1.0, são baseados em criptografia de chave pública. O canal seguro \(Schannel\) conjunto de protocolos de autenticação de provedor fornece esses protocolos. Todos os protocolos Schannel usam um modelo de cliente e de servidor.<br /><br />Para obter recursos adicionais, consulte [TLS / SSL &#40;SSP Schannel&#41; visão geral](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticar em um aplicativo ou serviço Web|Autenticação Integrada do Windows<br /><br />Autenticação Digest|Para obter recursos adicionais, consulte [autenticação integrada do Windows](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx) e [autenticação Digest](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), e [autenticação Digest avançada](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticar em aplicativos herdados|NTLM|NTLM é um desafio\-protocolo de autenticação de estilo de resposta. Além da autenticação, o protocolo NTLM, opcionalmente, fornece para a segurança de sessão; especificamente a integridade da mensagem e a confidencialidade por meio da assinatura e o fechamento das funções em NTLM.<br /><br />Para obter recursos adicionais, veja [Visão geral de NTLM](../kerberos/ntlm-overview.md).|
|Aproveitar autenticação multifator|Suporte de cartão inteligente<br /><br />Suporte biométrico|Os cartões inteligentes são uma violação\-meio portátil e resistente para fornecer soluções de segurança para tarefas como autenticação de cliente, registro em domínios, assinatura do código e protegendo e\-mail.<br /><br />A biometria depende da medição de uma característica física imutável de uma pessoa para identificá-la. As impressões digitais são uma das características biométricas mais usadas, com milhões de dispositivos biométricos de impressões digitais que são incorporados a computadores pessoais e periféricos.<br /><br />Para obter recursos adicionais, consulte [referência técnica do cartão inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Oferecer gerenciamento local, armazenamento e reutilização de credenciais|Gerenciamento de credenciais<br /><br />Autoridade de Segurança Local<br /><br />Senhas|O gerenciamento de credenciais no Windows garante que as credenciais sejam armazenadas com segurança. As credenciais são coletadas na área de trabalho protegida \(para acesso local ou domínio\), por meio de aplicativos ou sites para que as credenciais corretas sejam apresentadas sempre que um recurso é acessado.<br /><br />
|Estender a moderna proteção de autenticação aos sistemas herdados|Proteção estendida para autenticação|Este recurso aprimora a proteção e a manipulação de credenciais para autenticar conexões de rede usando a autenticação integrada do Windows \(IWA\).|

## <a name="software-requirements"></a>Requisitos de software
A Autenticação do Windows é projetada para ser compatível com as versões anteriores no sistema operacional Windows. Porém, os aprimoramentos de cada versão não são necessariamente aplicáveis às versões anteriores. Consulte a documentação sobre os recursos específicos para obter mais informações.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor
Muitos recursos de autenticação podem ser configurados usando a Política de Grupo, que pode ser instalada usando o Gerenciador do Servidor. O recurso Windows Biometric Framework é instalado usando o Gerenciador do Servidor. Outras funções de servidor que são dependentes de métodos de autenticação, como o servidor Web \(IIS\) e serviços de domínio do Active Directory, também pode ser instalado usando o Gerenciador do servidor.

## <a name="related-resources"></a>Recursos relacionados

|Tecnologias de autenticação|Recursos|
|----------------|-------|
|Autenticação do Windows|[Visão geral técnica de autenticação do Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Inclui tópicos que abordam as diferenças entre versões, os conceitos gerais de autenticação, cenários de logon, arquiteturas para versões com suporte e configurações aplicáveis.|
|Kerberos|[Visão geral da autenticação do Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Visão geral da delegação restrita de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Referência técnica da autenticação Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Guia de sobrevivência Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(Wiki do TechNet\)|
|TLS\/SSL e DTLS \(provedor de suporte de segurança Schannel\)|[TLS / SSL &#40;Schannel SSP&#41; visão geral](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Referência de provedor de suporte técnico segurança Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticação digest|[Referência técnica da autenticação de resumo](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Visão geral de NTLM](../kerberos/ntlm-overview.md)<br />Contém links para recursos atuais e anteriores|
|PKU2U|[Introdução ao PKU2U no Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Cartão inteligente|[Referência técnica do cartão inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenciais|[Proteção e gerenciamento de credenciais](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contém links para recursos atuais e anteriores<br /><br />[Visão geral de senhas](../kerberos/passwords-overview.md)<br />Contém links para recursos atuais e anteriores|


