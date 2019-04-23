---
title: Arquitetura de Interface SSPI
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8b0a74089c5d8a88a380f8a56e3b9e03b84081c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827017"
---
# <a name="security-support-provider-interface-architecture"></a>Arquitetura de Interface SSPI

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de referência para profissionais de TI descreve os protocolos de autenticação do Windows que são usados dentro da arquitetura de Interface de provedor de suporte de segurança (SSPI).

A Interface de provedor de suporte (SSPI) do Microsoft segurança é a base para a autenticação do Windows. Aplicativos e serviços de infraestrutura que exigem autenticação usam a SSPI para fornecê-la.

SSPI é a implementação do genérico segurança serviço GSSAPI (API) em sistemas operacionais Windows Server. Para obter mais informações sobre a GSSAPI, consulte RFC 2743 e RFC 2744 no banco de dados de RFC da IETF.

O padrão suporte SSPs (provedores segurança) que invocam os protocolos de autenticação específico no Windows são incorporados a SSPI como DLLs. Esses SSPs padrão são descritos nas seções a seguir. SSPs adicionais podem ser incorporadas se eles podem operar com o SSPI.

Conforme mostrado na imagem a seguir, o SSPI no Windows fornece um mecanismo que transporta os tokens de autenticação sobre o canal de comunicação existente entre o computador cliente e o servidor. Quando dois computadores ou dispositivos precisam ser autenticados para que elas possam se comunicar com segurança, as solicitações de autenticação são roteadas à SSPI, que conclui o processo de autenticação, independentemente do protocolo de rede em uso no momento. A SSPI retorna transparentes objetos binários grandes. Eles são passados entre os aplicativos, no ponto em que eles podem ser passados para a camada SSPI. Portanto, o SSPI permite que um aplicativo para usar vários modelos de segurança disponíveis em um computador ou rede sem alterar a interface para o sistema de segurança.

![Diagrama que mostra a arquitetura de Interface de provedor de suporte de segurança](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

As seções a seguir descrevem os SSPs padrão que interagem com o SSPI. Os SSPs são usados de maneiras diferentes nos sistemas operacionais do Windows para promover uma comunicação segura em um ambiente de rede não segura.

-   [Provedor de suporte de segurança do Kerberos](#BKMK_KerbSSP)

-   [Provedor de suporte de segurança NTLM](#BKMK_NTLMSSP)

-   [Digest Security Support Provider](#BKMK_DigestSSP)

-   [Provedor de suporte de segurança Schannel](#BKMK_SchannelSSP)

-   [Negociar Security Support Provider](#BKMK_NegoSSP)

-   [Credential Security Support Provider](#BKMK_CredSSP)

-   [Negociar extensões Security Support Provider](#BKMK_NegoExtsSSP)

-   [PKU2U Security Support Provider](#BKMK_PKU2USSP)

Também incluídos neste tópico:

[Seleção de provedor de suporte de segurança](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Provedor de suporte de segurança do Kerberos
Esse SSP usa apenas o protocolo Kerberos versão 5, conforme implementado pela Microsoft. Esse protocolo baseia-se em RFC 4120 e revisões de rascunho da rede do grupo de trabalho. É um protocolo padrão da indústria que é usado com uma senha ou um cartão inteligente para um logon interativo. Também é o método de autenticação preferido para serviços no Windows.

Porque o protocolo Kerberos tem sido o protocolo de autenticação padrão desde o Windows 2000, todos os serviços de domínio dão suporte a Kerberos SSP. Esses serviços incluem:

-   Active Directory consultas que usam o LDAP Lightweight Directory Access Protocol)

-   Gerenciamento remoto de servidor ou estação de trabalho que usa o serviço de chamada de procedimento remoto

-   Serviços de impressão

-   Autenticação de cliente-servidor

-   Acesso de arquivo remoto que usa o protocolo de bloco de mensagens de servidor (SMB) (também conhecido como Common Internet File System ou CIFS)

-   Referência e gerenciamento do sistema de arquivos distribuído

-   Autenticação de intranet para os serviços de informações da Internet (IIS)

-   Autenticação de autoridade de segurança para Internet Protocol security (IPsec)

-   Solicitações de certificado para serviços de certificados do Active Directory para computadores e usuários de domínio

Location: %windir%\Windows\System32\kerberos.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** lista no início deste tópico, além do Windows Server 2003 e o Windows XP.

**Recursos adicionais para o protocolo Kerberos e o SSP do Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: Extensões do protocolo Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: Extensões do protocolo Kerberos: Serviço para usuário e especificação do protocolo de delegação restrita](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Aperfeiçoamentos de Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) para o Windows Vista

-   [Alterações na autenticação Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) para Windows 7 

-   [Referência técnica da autenticação Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Provedor de suporte de segurança NTLM
O NTLM Security Support Provider (SSP NTLM) é um binário de protocolo usado pela Interface de provedor de suporte para segurança (SSPI) para permitir a autenticação de desafio / resposta NTLM e negociar opções de integridade e confidencialidade de mensagens. NTLM é usado sempre que a autenticação SSPI é usada, incluindo para autenticação do protocolo SMB ou CIFS, HTTP negociar a autenticação (por exemplo, autenticação da Web da Internet) e o serviço de chamada de procedimento remoto. O NTLM SSP inclui o NTLM e NTLM versão 2 (NTLMv2) protocolos de autenticação.

Os sistemas operacionais Windows com suporte pode usar o SSP do NTLM para o seguinte:

-   Autenticação de cliente/servidor

-   Serviços de impressão

-   Acesso a arquivos usando o protocolo CIFS (SMB)

-   Proteger o serviço de chamada de procedimento remoto ou DCOM

Location: %windir%\Windows\System32\msv1_0.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** lista no início deste tópico, além do Windows Server 2003 e o Windows XP.

**Recursos adicionais para o protocolo NTLM e o SSP do NTLM**

-   [Pacote de autenticação Msv1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Alterações na autenticação NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) no Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Auditoria e restringir o guia de uso do NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Digest Security Support Provider
Autenticação Digest é um padrão da indústria que é usado para autenticação de web e acesso protocolo LDAP (Lightweight Directory). Autenticação resumida transmite credenciais pela rede como um resumo de hash ou mensagem MD5.

SSP Digest (WDigest) é usado para o seguinte:

-   Acesso do Internet Explorer e Internet Information Services (IIS)

-   Consultas LDAP

Location: %windir%\Windows\System32\Digest.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** lista no início deste tópico, além do Windows Server 2003 e o Windows XP.

**Recursos adicionais para o protocolo de resumo e o SSP Digest**

-   [Autenticação Digest da Microsoft (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: Extensões do protocolo Digest](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Provedor de suporte de segurança Schannel
O canal seguro (Schannel) é usado para autenticação de servidor baseados na web, como quando um usuário tenta acessar um servidor web seguro.

O protocolo TLS, protocolo SSL, o protocolo de tecnologia PCT (Private Communications) e o protocolo de camada de transporte do datagrama (DTLS) são baseados em criptografia de chave pública. Schannel fornece todos esses protocolos. Todos os protocolos Schannel usam um modelo de cliente/servidor. O SSP Schannel usa certificados de chave pública para autenticar as partes. Ao autenticar terceiros, o SSP Schannel seleciona um protocolo na seguinte ordem de preferência:

-   Transporte Layer Security (TLS) versão 1.0

-   Transporte Layer Security (TLS) versão 1.1

-   Transporte Layer Security (TLS) versão 1.2

-   Secure Socket Layer (SSL) versão 2.0

-   Secure Socket Layer (SSL) versão 3.0

-   PCT PCT)

    **Observação** PCT é desabilitado por padrão.

O protocolo selecionado é o protocolo de autenticação preferencial que o cliente e o servidor podem dar suporte. Por exemplo, se um servidor dá suporte a todos os protocolos Schannel e o cliente oferece suporte a apenas o SSL 3.0 e o SSL 2.0, o processo de autenticação usa o SSL 3.0.

DTLS é usado quando chamada explicitamente pelo aplicativo. Para obter mais informações sobre o DTLS e outros protocolos que são usados pelo provedor de Schannel, consulte [referência de provedor de suporte técnico segurança Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Location: %windir%\Windows\System32\Schannel.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** lista no início deste tópico, além do Windows Server 2003 e o Windows XP.

> [!NOTE]
> TLS 1.2 foi introduzido nesse provedor no Windows Server 2008 R2 e Windows 7. DTLS foi introduzido nesse provedor no Windows Server 2012 e Windows 8.

**Recursos adicionais para os protocolos TLS e SSL e SSP Schannel**

-   [Canal seguro (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Referência técnica de TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: Perfil do Transport Layer Security (TLS)](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negociar Security Support Provider
O simples e o mecanismo de negociação GSS-API protegidas (SPNEGO) constitui a base para o SSP negociar, whichcan ser usado para negociar um protocolo de autenticação específico. Quando um aplicativo chama SSPI para fazer logon em uma rede, ele pode especificar um SSP para processar a solicitação. Se o aplicativo especifica Negotiate SSP, ele analisa a solicitação e escolhe o provedor apropriado para lidar com a solicitação, com base nas políticas de segurança de cliente configurado.

SPNEGO é especificado no RFC 2478.

Nas versões com suporte dos sistemas operacionais Windows, negociar segurança dão suporte a provedor seleciona entre o protocolo Kerberos e NTLM. Negociar seleciona do protocolo Kerberos por padrão, a menos que esse protocolo não pode ser usado por um dos sistemas envolvidos na autenticação ou o aplicativo de chamada não forneceu informações suficientes para usar o protocolo Kerberos.

Local: %windir%\Windows\System32\lsasrv.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** lista no início deste tópico, além do Windows Server 2003 e o Windows XP.

**Recursos adicionais para o Negotiate SSP**

-   [Microsoft Negotiate (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: Extensões (SPNEGO) do mecanismo de negociação GSS-API simples e protegido](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: Negociar e especificação do protocolo de autenticação de HTTP Nego2](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Credential Security Support Provider
O Credential Security Service Provider (CredSSP) fornece uma único logon (SSO) experiência do usuário ao iniciar novas sessões de serviços de Terminal e serviços de área de trabalho remota. CredSSP permite que os aplicativos delegar credenciais de usuários do computador cliente (usando o SSP do lado do cliente) para o servidor de destino (por meio do SSP no servidor), com base nas políticas do cliente. As políticas do CredSSP são configuradas por meio da política de grupo e a delegação de credenciais é desativada por padrão.

Location: %windir%\Windows\System32\credssp.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** no início deste tópico.

**Recursos adicionais para o SSP de credenciais**

-   [\[MS-CSSP\]: Especificação do protocolo do Credential Security suporte Provider (CredSSP)](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Credential Security Service Provider e o SSO para o Terminal Services Logon](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Negociar extensões Security Support Provider
Negociar extensões (NegoExts) é um pacote de autenticação que negocia o uso de SSPs diferentes de NTLM ou o protocolo Kerberos, para aplicativos e cenários implementado pela Microsoft e outros softwares de empresas.

Essa extensão para o pacote Negotiate permite que os seguintes cenários:

-   **Disponibilidade de cliente avançada dentro de um sistema federado.** Documentos podem ser acessados nos sites do SharePoint, e eles podem ser editados usando um aplicativo do Microsoft Office completos.

-   **Suporte a cliente avançado para os serviços do Microsoft Office.** Os usuários podem entrar nos serviços do Microsoft Office e usar um aplicativo do Microsoft Office completos.

-   **Microsoft Exchange Server hospedado e o Outlook.** Não há nenhuma relação de confiança domínio estabelecida como o Exchange Server está hospedado na web. O Outlook usa o serviço Windows Live para autenticar usuários.

-   **Disponibilidade de cliente rico entre servidores e computadores cliente.** Componentes de rede e de autenticação do sistema operacional são usados.

O pacote Negotiate Windows trata o SSP NegoExts da mesma maneira como faz para Kerberos e NTLM. NegoExts.dll é carregado na autoridade de sistema Local (LSA) na inicialização. Quando uma solicitação de autenticação é recebida, com base na origem da solicitação, NegoExts negocia entre os SSPs com suporte. Ele reúne as credenciais e as políticas, criptografa-os e envia essas informações para o SSP apropriado, em que o token de segurança é criado.

Os SSPs NegoExts com suporte não são SSPs autônomos como o Kerberos e NTLM. Portanto, dentro do SSP NegoExts, quando o método de autenticação falha por algum motivo, uma mensagem de falha de autenticação será ser exibida ou registrada. Nenhum método de autenticação de fallback ou renegociação é possível.

Location: %windir%\Windows\System32\negoexts.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** no início deste tópico, exceto o Windows Server 2008 e Windows Vista.

### <a name="BKMK_PKU2USSP"></a>PKU2U Security Support Provider
O protocolo de PKU2U foi introduzido e é implementado como um SSP no Windows 7 e Windows Server 2008 R2. Esse SSP habilita a autenticação de ponto a ponto, especialmente por meio do recurso chamado grupo doméstico, que foi introduzido no Windows 7 de compartilhamento de arquivos e mídia. O recurso permite o compartilhamento entre computadores que não são membros de um domínio.

Local: %windir%\Windows\System32\pku2u.dll

Esse provedor está incluído por padrão nas versões designadas na **aplica-se a** no início deste tópico, exceto o Windows Server 2008 e Windows Vista.

**Recursos adicionais para o protocolo PKU2U e o SSP PKU2U**

-   [Apresentando a integração de identidade Online](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Seleção de provedor de suporte de segurança
A SSPI do Windows pode usar qualquer um dos protocolos que têm suporte por meio de provedores de suporte de segurança instalados. No entanto, como nem todos os sistemas operacionais dão suporte os mesmos pacotes SSP como um determinado computador executando o Windows Server, os clientes e servidores devem negociar a usar um protocolo que dão suporte a ambos. Windows Server prefere computadores cliente e aplicativos para usar o protocolo Kerberos, um protocolo baseado em padrões forte, quando possível, mas o sistema operacional continua permitir que computadores cliente e cliente de aplicativos que não dão suporte a Kerberos protocolo para autenticar.

Antes de autenticação pode levar a comunicação local os dois computadores devem concordar com um protocolo que os dois podem dar suporte. Para qualquer protocolo ser usado por meio de SSPI, cada computador deve ter o SSP de apropriado. Por exemplo, para um computador cliente e servidor para usar o protocolo de autenticação Kerberos, eles devem ambos oferecem suporte Kerberos v5. Windows Server usa a função **EnumerateSecurityPackages** para identificar quais SSPs têm suporte em um computador e o que são os recursos desses SSPs.

A seleção de um protocolo de autenticação pode ser manipulada em uma das duas maneiras a seguir:

1.  [Protocolo de autenticação único](#BKMK_SingleAuth)

2.  [Opção de negociação](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocolo de autenticação único
Quando um único protocolo aceitável é especificado no servidor, o computador cliente deve oferecer suporte o protocolo especificado ou a falha de comunicação. Quando um único protocolo aceitável é especificado, a troca de autenticação ocorre da seguinte maneira:

1.  O computador cliente solicita acesso a um serviço.

2.  O servidor responde à solicitação e especifica o protocolo que será usado.

3.  O computador cliente examina o conteúdo da resposta e verificações para determinar se ele dá suporte ao protocolo especificado. Se o computador cliente dá suporte ao protocolo especificado, a autenticação prossegue. Se o computador cliente não dá suporte ao protocolo, a autenticação falhará, independentemente se o computador cliente está autorizado a acessar o recurso.

### <a name="BKMK_Negotiate"></a>Opção de negociação
A opção negotiate pode ser usada para permitir que o cliente e servidor tentar localizar um protocolo aceitável. Isso se baseia no mecanismo de negociação GSS-API protegidas (SPNEGO) e simples. Quando a autenticação é iniciada com a opção de negociar um protocolo de autenticação, o exchange SPNEGO ocorre da seguinte maneira:

1.  O computador cliente solicita acesso a um serviço.

2.  O servidor responde com uma lista de protocolos de autenticação que ele pode dar suporte e um desafio de autenticação ou resposta, com base no protocolo que é sua primeira opção. Por exemplo, o servidor pode listar o protocolo Kerberos e NTLM e enviar uma resposta de autenticação Kerberos.

3.  O computador cliente examina o conteúdo da resposta e verificações para determinar se ele dá suporte a qualquer um dos protocolos especificados.

    -   Se o computador cliente suporta o protocolo preferencial, a autenticação continuará.

    -   Se o computador cliente não suporta o protocolo preferencial, mas ele oferece suporte a um dos outros protocolos listados pelo servidor, o computador cliente permite que o servidor Saiba que ele oferece suporte de protocolo de autenticação e a autenticação prossegue.

    -   Se o computador cliente não oferece suporte a qualquer um dos protocolos listados, a troca de autenticação falhará.

## <a name="see-also"></a>Consulte também
[Arquitetura de autenticação do Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


