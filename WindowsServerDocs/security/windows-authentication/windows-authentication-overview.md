---
title: Visão geral da autenticação do Windows
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 4a2b5e6b48a56a1a2148df262d2785640ac6054d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638691"
---
# <a name="windows-authentication-overview"></a>Visão geral da autenticação do Windows

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de navegação para profissionais de TI lista os recursos de documentação para as tecnologias de logon e autenticação do Windows que incluem avaliação do produto, guias de introdução, procedimentos, guias de design e implantação, referências técnicas e referências de comandos.

## <a name="feature-description"></a>Descrição do recurso
A autenticação é um processo de verificação da identidade de um objeto, um serviço ou uma pessoa. Quando você autentica um objeto, a meta é verificar se ele é genuíno. Ao autenticar um serviço ou uma pessoa, a meta é verificar se as credenciais apresentadas são autênticas.

Em um contexto de rede, a autenticação é o ato de fornecer identidade para um aplicativo ou recurso de rede. Normalmente, a identidade é comprovada por uma operação criptográfica que usa uma chave somente o usuário sabe-como com criptografia de chave pública – ou uma chave compartilhada. O lado do servidor da troca de autenticação compara os dados assinados com uma chave de criptografia conhecida para validar a tentativa de autenticação.

O armazenamento das chaves criptográficas em um local central seguro torna o processo de autenticação escalonável e passível de manutenção. Active Directory Domain Services é a tecnologia recomendada e padrão para armazenar informações de identidade, \( incluindo as chaves de criptografia que são as credenciais do usuário \) . O Active Directory é exigido para as implementações de Kerberos e NTLM padrão.

As técnicas de autenticação variam de um logon simples, que identifica os usuários com base em algo que apenas o usuário sabe, como uma senha, a mecanismos de segurança mais poderosos que usam algo que o usuário tem, certificados de chave pública e biometria. Em um ambiente de negócios, os serviços ou usuários podem acessar vários aplicativos ou recursos em muitos tipos de servidores em um único local ou em vários locais. Por esses motivos, a autenticação deve dar suporte a ambientes de outras plataformas e de outros sistemas operacionais Windows.

O sistema operacional Windows implementa um conjunto padrão de protocolos de autenticação, incluindo Kerberos, NTLM, segurança de camada de transporte \/ protocolo SSL \( TLS \/ SSL \) e Digest, como parte de uma arquitetura extensível. Além disso, alguns protocolos são combinados com os pacotes de autenticação, como Negotiate e Credential Security Support Provider. Estes protocolos e pacotes permitem a autenticação de usuários, computadores e serviços; o processo de autenticação, por sua vez, permite que os usuários e os serviços autorizados acessem recursos de forma segura.

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
|Autenticar em um domínio do Active Directory|Kerberos|Os &nbsp; sistemas operacionais Microsoft Windows Server implementam o protocolo de autenticação Kerberos versão 5 e as extensões para autenticação de chave pública. O cliente de autenticação Kerberos é implementado como um SSP do provedor de suporte de segurança \( \) e pode ser acessado por meio da interface de provedor de suporte de segurança \( SSPI \) . A autenticação inicial do usuário é integrada com a arquitetura de logon único do Winlogon \- . O Kerberos centro de distribuição de chaves \( KDC \) é integrado a outros serviços de segurança do Windows Server em execução no controlador de domínio. O KDC usa o banco de dados do serviço de diretório Active Directory do domínio como seu banco de dados de conta de segurança. O Active Directory é exigido para as implementações de Kerberos padrão.<p>Para obter recursos adicionais, veja [Visão geral da autenticação Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticação segura na Web|TLS \/ SSL conforme implementado no provedor de suporte de segurança Schannel|O protocolo TLS de segurança de camada de transporte \( \) versões 1,0, 1,1 e 1,2, protocolo SSL \( \) protocolo SSL, versões 2,0 e 3,0, protocolo de segurança de camada de transporte de datatransport versão 1,0 e o protocolo PCT de transporte de comunicações privada \( \) , versão 1,0, são baseados na criptografia de chave pública. O conjunto de \( protocolos de autenticação do provedor Schannel do canal seguro \) fornece esses protocolos. Todos os protocolos Schannel usam um modelo de cliente e de servidor.<p>Para obter recursos adicionais, confira [TLS-SSL &#40;Schannel SSP&#41; visão geral](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticar em um aplicativo ou serviço Web|Autenticação Integrada do Windows<p>Autenticação Digest|Para obter recursos adicionais, consulte [Autenticação integrada do Windows](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) e [Autenticação Digest](/previous-versions/windows/it-pro/windows-server-2003/cc738318(v=ws.10)) e [Autenticação Digest avançada](/previous-versions/windows/it-pro/windows-server-2003/cc783131(v=ws.10)).|
|Autenticar em aplicativos herdados|NTLM|NTLM é um protocolo de autenticação de estilo de resposta de desafio \- . Além da autenticação, o protocolo NTLM fornece opcionalmente a segurança da sessão, especificamente a integridade e a confidencialidade das mensagens por meio de funções de assinatura e lacre no NTLM.<p>Para obter recursos adicionais, veja [Visão geral de NTLM](../kerberos/ntlm-overview.md).|
|Aproveitar autenticação multifator|Suporte de cartão inteligente<p>Suporte biométrico|Os cartões inteligentes são uma \- maneira portátil e resistente a adulterações para fornecer soluções de segurança para tarefas como autenticação de cliente, logon em domínios, assinatura de código e segurança e \- email.<p>A biometria depende da medição de uma característica física imutável de uma pessoa para identificá-la. As impressões digitais são uma das características biométricas mais usadas, com milhões de dispositivos biométricos de impressões digitais que são incorporados a computadores pessoais e periféricos.<p>Para obter recursos adicionais, consulte [referência técnica do cartão inteligente](/windows/security/identity-protection/smart-cards/smart-card-windows-smart-card-technical-reference). |
|Oferecer gerenciamento local, armazenamento e reutilização de credenciais|Gerenciamento de credenciais<p>Autoridade de Segurança Local<p>Senhas|O gerenciamento de credenciais no Windows garante que as credenciais sejam armazenadas com segurança. As credenciais são coletadas na área de trabalho segura \( para acesso local ou de domínio \) , por meio de aplicativos ou por meio de sites, para que as credenciais corretas sejam apresentadas sempre que um recurso for acessado.<p>
|Estender a moderna proteção de autenticação aos sistemas herdados|Proteção Estendida para Autenticação|Esse recurso aprimora a proteção e a manipulação de credenciais ao autenticar conexões de rede usando IWA de autenticação integrada do Windows \( \) .|

## <a name="software-requirements"></a>Requisitos de software
A Autenticação do Windows é projetada para ser compatível com as versões anteriores no sistema operacional Windows. Porém, os aprimoramentos de cada versão não são necessariamente aplicáveis às versões anteriores. Consulte a documentação sobre os recursos específicos para obter mais informações.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor
Muitos recursos de autenticação podem ser configurados usando a Política de Grupo, que pode ser instalada usando o Gerenciador do Servidor. O recurso Windows Biometric Framework é instalado usando o Gerenciador do Servidor. Outras funções de servidor que dependem de métodos de autenticação, como o servidor Web \( IIS \) e Active Directory Domain Services, também podem ser instaladas usando Gerenciador do servidor.

## <a name="related-resources"></a>Recursos relacionados

|Tecnologias de autenticação|Recursos|
|----------------|-------|
|Autenticação do Windows|[Visão geral técnica de autenticação do Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Inclui tópicos que abordam as diferenças entre versões, os conceitos gerais de autenticação, cenários de logon, arquiteturas para versões com suporte e configurações aplicáveis.|
|Kerberos|[Kerberos Authentication Overview](../kerberos/kerberos-authentication-overview.md)<p>[Visão geral da Delegação Restrita de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<p>Referência técnica de [autenticação Kerberos](/previous-versions/windows/it-pro/windows-server-2003/cc739058(v=ws.10)) \( 2003\)<p>[Fórum do Kerberos](/answers/topics/windows-server-security.html)|
|\/Provedor de suporte de segurança do DTLS Schannel SSL e TLS \(\)|[Visão geral do &#40;do SSP do Schannel do protocolo TLS&#41;](../tls/tls-ssl-schannel-ssp-overview.md)<p>[Referência técnica do provedor de suporte de segurança Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticação digest|Referência técnica da [autenticação Digest](/previous-versions/windows/it-pro/windows-server-2003/cc782794(v=ws.10)) \( 2003\)|
|NTLM|[NTLM Overview](../kerberos/ntlm-overview.md)<br />Contém links para recursos atuais e anteriores|
|PKU2U|[Introdução ao PKU2U no Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560634(v=ws.10))|
|Cartão inteligente|[Referência técnica do cartão inteligente](/windows/security/identity-protection/smart-cards/smart-card-windows-smart-card-technical-reference)<p>
|Credenciais|[Proteção e gerenciamento de credenciais](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contém links para recursos atuais e anteriores<p>[Visão geral de senhas](../kerberos/passwords-overview.md)<br />Contém links para recursos atuais e anteriores|