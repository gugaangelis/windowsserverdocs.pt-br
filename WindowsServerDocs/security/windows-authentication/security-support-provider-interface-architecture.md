---
title: Arquitetura de Interface SSPI
description: Segurança do Windows Server
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: da7c390427a3f0f2348d91e14d0affef905db390
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766949"
---
# <a name="security-support-provider-interface-architecture"></a>Arquitetura de Interface SSPI

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de referência para o profissional de ti descreve os protocolos de autenticação do Windows que são usados na arquitetura do SSPI (Security Support Provider interface).

A interface de provedor de suporte de segurança da Microsoft (SSPI) é a base para a autenticação do Windows. Aplicativos e serviços de infraestrutura que exigem autenticação usam SSPI para fornecê-lo.

SSPI é a implementação da API de serviço de segurança (GSSAPI) genérica em sistemas operacionais Windows Server. Para obter mais informações sobre o GSSAPI, consulte RFC 2743 e RFC 2744 no banco de dados RFC da IETF.

Os SSPs (provedores de suporte de segurança) padrão que invocam protocolos de autenticação específicos no Windows são incorporados ao SSPI como DLLs. Esses SSPs padrão são descritos nas seções a seguir. Os SSPs adicionais poderão ser incorporados se puderem operar com o SSPI.

Conforme mostrado na imagem a seguir, o SSPI no Windows fornece um mecanismo que transporta tokens de autenticação no canal de comunicação existente entre o computador cliente e o servidor. Quando dois computadores ou dispositivos precisam ser autenticados para que possam se comunicar com segurança, as solicitações de autenticação são roteadas para o SSPI, o que conclui o processo de autenticação, independentemente do protocolo de rede em uso no momento. O SSPI retorna objetos binários grandes transparentes. Eles são passados entre os aplicativos, no ponto em que podem ser passados para a camada SSPI. Assim, o SSPI permite que um aplicativo use vários modelos de segurança disponíveis em um computador ou rede sem alterar a interface para o sistema de segurança.

![Diagrama mostrando a arquitetura da interface do provedor de suporte de segurança](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

As seções a seguir descrevem os SSPs padrão que interagem com o SSPI. Os SSPs são usados de maneiras diferentes nos sistemas operacionais Windows para promover a comunicação segura em um ambiente de rede não seguro.

-   [Provedor de suporte de segurança Kerberos](#BKMK_KerbSSP)

-   [Provedor de suporte de segurança do NTLM](#BKMK_NTLMSSP)

-   [Provedor de suporte de segurança Digest](#BKMK_DigestSSP)

-   [Provedor de suporte de segurança Schannel](#BKMK_SchannelSSP)

-   [Negociar provedor de suporte de segurança](#BKMK_NegoSSP)

-   [Provedor de suporte à segurança de credenciais](#BKMK_CredSSP)

-   [Provedor de suporte de segurança de extensões de negociação](#BKMK_NegoExtsSSP)

-   [Provedor de suporte de segurança do PKU2U](#BKMK_PKU2USSP)

Também incluído neste tópico:

[Seleção do provedor de suporte de segurança](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="kerberos-security-support-provider"></a><a name="BKMK_KerbSSP"></a>Provedor de suporte de segurança Kerberos
Esse SSP usa apenas o protocolo Kerberos versão 5, conforme implementado pela Microsoft. Esse protocolo é baseado nas revisões RFC 4120 e draft do grupo de trabalho de rede. É um protocolo padrão do setor que é usado com uma senha ou um cartão inteligente para um logon interativo. Ele também é o método de autenticação preferencial para serviços no Windows.

Como o protocolo Kerberos foi o protocolo de autenticação padrão desde o Windows 2000, todos os serviços de domínio dão suporte ao SSP do Kerberos. Esses serviços incluem:

-   Active Directory consultas que usam o protocolo LDAP (Lightweight Directory Access Protocol)

-   Gerenciamento de servidor remoto ou estação de trabalho que usa o serviço de chamada de procedimento remoto

-   Serviços de impressão

-   Autenticação de cliente-servidor

-   Acesso remoto a arquivos que usa o protocolo SMB (também conhecido como sistema de arquivos da Internet comum ou CIFS)

-   Gerenciamento e referência do sistema de arquivos distribuído

-   Autenticação de intranet para Serviços de Informações da Internet (IIS)

-   Autenticação de autoridade de segurança para IPsec (Internet Protocol Security)

-   Solicitações de certificado para Active Directory serviços de certificados para usuários e computadores do domínio

Local:% WINDIR% \Windows\System32\kerberos.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico, além do windows Server 2003 e do Windows XP.

**Recursos adicionais para o protocolo Kerberos e o SSP do Kerberos**

-   [Microsoft Kerberos (Windows)](/windows/win32/secauthn/microsoft-kerberos)

-   [\[MS-KILE \] : extensões de protocolo Kerberos](/openspecs/windows_protocols/ms-kile/2a32282e-dd48-4ad9-a542-609804b02cc9)

-   [\[MS-SFU \] : extensões de protocolo Kerberos: serviço para especificação de protocolo de delegação restrita e de usuário](/openspecs/windows_protocols/ms-sfu/3bff5864-8135-400e-bdd9-33b552051d94)

-   [SSP/PA Kerberos (Windows)](/windows/win32/secauthn/kerberos-ssp-ap)

-   [Aprimoramentos do Kerberos](/previous-versions/windows/it-pro/windows-vista/cc749438(v=ws.10)) para o Windows Vista

-   [Alterações na autenticação Kerberos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560670(v=ws.10)) para o Windows 7

-   [Referência técnica de autenticação Kerberos](/previous-versions/windows/it-pro/windows-server-2003/cc739058(v=ws.10))

### <a name="ntlm-security-support-provider"></a><a name="BKMK_NTLMSSP"></a>Provedor de suporte de segurança do NTLM
O provedor de suporte de segurança NTLM (NTLM SSP) é um protocolo de mensagens binários usado pela SSPI (Security Support Provider interface) para permitir a autenticação de desafio/resposta NTLM e para negociar as opções de integridade e confidencialidade. O NTLM é usado sempre que a autenticação SSPI é usada, incluindo o bloqueio de mensagens do servidor ou a autenticação CIFS, a autenticação de negociação HTTP (por exemplo, autenticação na Web da Internet) e o serviço de chamada de procedimento remoto. O SSP do NTLM inclui os protocolos de autenticação NTLM e NTLM versão 2 (NTLMv2).

Os sistemas operacionais Windows com suporte podem usar o SSP do NTLM para o seguinte:

-   Autenticação de cliente/servidor

-   Serviços de impressão

-   Acesso a arquivos usando CIFS (SMB)

-   Serviço de chamada de procedimento remoto seguro ou serviço DCOM

Local:% WINDIR% \Windows\System32\msv1_0.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico, além do windows Server 2003 e do Windows XP.

**Recursos adicionais para o protocolo NTLM e o SSP do NTLM**

-   [Pacote de autenticação MSV1_0 (Windows)](/windows/win32/secauthn/msv1-0-authentication-package)

-   [Alterações na autenticação NTLM](/previous-versions/windows/it-pro/windows-7/dd566199(v=ws.10)) no Windows 7

-   [Microsoft NTLM (Windows)](/windows/win32/secauthn/microsoft-ntlm)

-   [Auditing and restricting NTLM usage guide (Guia de uso do NTLM para auditoria e restrição)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/jj865674(v=ws.10))

### <a name="digest-security-support-provider"></a><a name="BKMK_DigestSSP"></a>Provedor de suporte de segurança Digest
A autenticação Digest é um padrão da indústria que é usado para o protocolo LDAP e a autenticação na Web. A autenticação Digest transmite as credenciais pela rede como um hash MD5 ou um resumo de mensagem.

O SSP de resumo (Wdigest.dll) é usado para o seguinte:

-   Acesso ao Internet Explorer e Serviços de Informações da Internet (IIS)

-   Consultas LDAP

Local:% WINDIR% \Windows\System32\Digest.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico, além do windows Server 2003 e do Windows XP.

**Recursos adicionais para o protocolo Digest e o SSP de resumo**

-   [Autenticação de Microsoft Digest (Windows)](/windows/win32/secauthn/microsoft-digest-ssp)

-   [\[MS-DPSP \] : extensões de protocolo Digest](/openspecs/windows_protocols/ms-dpsp/3e44be62-2067-472a-9ef0-e937298b68fb)

### <a name="schannel-security-support-provider"></a><a name="BKMK_SchannelSSP"></a>Provedor de suporte de segurança Schannel
O canal seguro (Schannel) é usado para autenticação de servidor baseada na Web, como quando um usuário tenta acessar um servidor Web seguro.

O protocolo TLS, o protocolo SSL, o protocolo PCT (Private Communications Technology) e o protocolo DTLS (camada de transporte de datagrama) baseiam-se na criptografia de chave pública. O Schannel fornece todos esses protocolos. Todos os protocolos Schannel usam um modelo de cliente/servidor. O SSP Schannel usa certificados de chave pública para autenticar as partes. Ao autenticar partes, o Schannel SSP seleciona um protocolo na seguinte ordem de preferência:

-   Segurança de camada de transporte (TLS) versão 1,0

-   Segurança de camada de transporte (TLS) versão 1,1

-   Segurança de camada de transporte (TLS) versão 1,2

-   Camada de soquete de segurança (SSL) versão 2,0

-   Camada de soquete de segurança (SSL) versão 3,0

-   PCT (tecnologia de comunicação privada)

    **Observação** O PCT está desabilitado por padrão.

O protocolo selecionado é o protocolo de autenticação preferencial ao qual o cliente e o servidor podem dar suporte. Por exemplo, se um servidor oferecer suporte a todos os protocolos Schannel e o cliente der suporte apenas a SSL 3,0 e SSL 2,0, o processo de autenticação usará SSL 3,0.

DTLS é usado quando chamado explicitamente pelo aplicativo. Para obter mais informações sobre o DTLS e os outros protocolos usados pelo provedor Schannel, consulte [referência técnica do provedor de suporte de segurança Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Local:% WINDIR% \Windows\System32\Schannel.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico, além do windows Server 2003 e do Windows XP.

> [!NOTE]
> O TLS 1,2 foi introduzido nesse provedor no Windows Server 2008 R2 e no Windows 7. O DTLS foi introduzido neste provedor no Windows Server 2012 e no Windows 8.

**Recursos adicionais para os protocolos TLS e SSL e o SSP do Schannel**

-   [Canal seguro (Windows)](/windows/win32/secauthn/secure-channel)

-   [Referência técnica de TLS/SSL](/previous-versions/windows/it-pro/windows-server-2003/cc784149(v=ws.10))

-   [\[MS-TLSP \] : perfil de protocolo TLS](/openspecs/windows_protocols/ms-tlsp/58aba05b-62b0-4cd1-b88b-dc8a24920346)

### <a name="negotiate-security-support-provider"></a><a name="BKMK_NegoSSP"></a>Negociar provedor de suporte de segurança
O mecanismo de negociação de GSS-API simples e protegido (SPNEGO) constitui a base para o Negotiate SSP, whichcan ser usado para negociar um protocolo de autenticação específico. Quando um aplicativo chama o SSPI para fazer logon em uma rede, ele pode especificar um SSP para processar a solicitação. Se o aplicativo especificar o Negotiate SSP, ele analisará a solicitação e selecionará o provedor apropriado para lidar com a solicitação, com base nas políticas de segurança configuradas pelo cliente.

SPNEGO é especificado no RFC 2478.

Em versões com suporte dos sistemas operacionais Windows, o provedor de suporte Negotiate Security seleciona entre o protocolo Kerberos e o NTLM. Negotiate seleciona o protocolo Kerberos por padrão, a menos que esse protocolo não possa ser usado por um dos sistemas envolvidos na autenticação ou o aplicativo de chamada não forneceu informações suficientes para usar o protocolo Kerberos.

Local:% WINDIR% \Windows\System32\lsasrv.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico, além do windows Server 2003 e do Windows XP.

**Recursos adicionais para o Negotiate SSP**

-   [Microsoft Negotiate (Windows)](/windows/win32/secauthn/microsoft-negotiate)

-   [\[MS-SPNG \] : extensões do mecanismo de negociação do GSS-API (SPNEGO) simples e protegidas](/openspecs/windows_protocols/ms-spng/f377a379-c24f-4a0f-a3eb-0d835389e28a)

-   [\[MS-N2HT \] : especificação de protocolo de autenticação http Negotiate e Nego2](/openspecs/windows_protocols/ms-n2ht/4b88aa77-4b12-4933-8740-0f32d8f4eacf)

### <a name="credential-security-support-provider"></a><a name="BKMK_CredSSP"></a>Provedor de suporte à segurança de credenciais
O Credential Security Service Provider (CredSSP) fornece uma experiência de usuário SSO (logon único) ao iniciar novos serviços de terminal e Serviços de Área de Trabalho Remota sessões. O CredSSP permite que os aplicativos deleguem as credenciais dos usuários do computador cliente (usando o SSP do lado do cliente) para o servidor de destino (por meio do SSP do lado do servidor), com base nas políticas do cliente. As políticas de CredSSP são configuradas usando Política de Grupo, e a delegação de credenciais é desativada por padrão.

Local:% WINDIR% \Windows\System32\credssp.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico.

**Recursos adicionais para o SSP de credenciais**

-   [\[MS-CSSP \] : especificação do protocolo CredSSP (provedor de suporte à segurança de credencial)](/openspecs/windows_protocols/ms-cssp/85f57821-40bb-46aa-bfcb-ba9590b8fc30)

-   [Provedor de serviços de segurança de credenciais e SSO para logon de serviços de terminal](/previous-versions/windows/it-pro/windows-vista/cc749211(v=ws.10))

### <a name="negotiate-extensions-security-support-provider"></a><a name="BKMK_NegoExtsSSP"></a>Provedor de suporte de segurança de extensões de negociação
O Negotiate Extensions (NegoExts) é um pacote de autenticação que negocia o uso de SSPs, além do NTLM ou do protocolo Kerberos, para aplicativos e cenários implementados pela Microsoft e por outras empresas de software.

Essa extensão para o pacote Negotiate permite os seguintes cenários:

-   **Disponibilidade avançada do cliente em um sistema federado.** Os documentos podem ser acessados em sites do SharePoint e podem ser editados usando um aplicativo de Microsoft Office completo.

-   **Suporte avançado ao cliente para serviços de Microsoft Office.** Os usuários podem entrar no Microsoft Office Services e usar um aplicativo de Microsoft Office completo.

-   **Microsoft Exchange Server e Outlook hospedados.** Não há confiança de domínio estabelecida porque o Exchange Server está hospedado na Web. O Outlook usa o serviço Windows Live para autenticar usuários.

-   **Disponibilidade de cliente avançada entre computadores cliente e servidores.** Os componentes de autenticação e de rede do sistema operacional são usados.

O pacote de negociação do Windows trata o SSP do NegoExts da mesma maneira que faz para Kerberos e NTLM. NegoExts.dll é carregado na autoridade de sistema local (LSA) na inicialização. Quando uma solicitação de autenticação é recebida, com base na origem da solicitação, o NegoExts negocia entre os SSPs com suporte. Ele coleta as credenciais e as políticas, criptografa-as e as envia para o SSP apropriado, no qual o token de segurança é criado.

Os SSPs com suporte do NegoExts não são SSPs autônomos, como Kerberos e NTLM. Portanto, no SSP do NegoExts, quando o método de autenticação falhar por algum motivo, uma mensagem de falha de autenticação será exibida ou registrada. Nenhum método de autenticação de renegociação ou de fallback é possível.

Local:% WINDIR% \Windows\System32\negoexts.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico, excluindo o windows Server 2008 e o Windows Vista.

### <a name="pku2u-security-support-provider"></a><a name="BKMK_PKU2USSP"></a>Provedor de suporte de segurança do PKU2U
O protocolo PKU2U foi introduzido e implementado como um SSP no Windows 7 e no Windows Server 2008 R2. Esse SSP habilita a autenticação ponto a ponto, especialmente por meio do recurso de compartilhamento de mídia e de arquivos chamado grupo doméstico, que foi introduzido no Windows 7. O recurso permite o compartilhamento entre computadores que não são membros de um domínio.

Local:% WINDIR% \Windows\System32\pku2u.dll

Esse provedor é incluído por padrão nas versões designadas na lista **aplica-se a** no início deste tópico, excluindo o windows Server 2008 e o Windows Vista.

**Recursos adicionais para o protocolo PKU2U e o SSP do PKU2U**

-   [Introdução à integração de identidade online](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560662(v=ws.10))

## <a name="security-support-provider-selection"></a><a name="BKMK_SecuritySupportProviderSelection"></a>Seleção do provedor de suporte de segurança
O SSPI do Windows pode usar qualquer um dos protocolos com suporte por meio dos provedores de suporte de segurança instalados. No entanto, como nem todos os sistemas operacionais dão suporte aos mesmos pacotes do SSP que qualquer computador que esteja executando o Windows Server, os clientes e servidores devem negociar para usar um protocolo que ambos dão suporte. O Windows Server prefere que computadores cliente e aplicativos usem o protocolo Kerberos, um protocolo forte baseado em padrões, quando possível, mas o sistema operacional continua a permitir computadores cliente e aplicativos cliente que não dão suporte ao protocolo Kerberos para autenticação.

Antes que a autenticação possa ocorrer, os dois computadores de comunicação devem concordar com um protocolo que ambos podem dar suporte. Para que qualquer protocolo seja utilizável por meio do SSPI, cada computador deve ter o SSP apropriado. Por exemplo, para que um computador cliente e um servidor usem o protocolo de autenticação Kerberos, eles devem dar suporte ao Kerberos v5. O Windows Server usa a função **EnumerateSecurityPackages** para identificar quais SSPs têm suporte em um computador e quais são os recursos desses SSPs.

A seleção de um protocolo de autenticação pode ser manipulada de uma das duas maneiras a seguir:

1.  [Protocolo de autenticação única](#BKMK_SingleAuth)

2.  [Opção Negotiate](#BKMK_Negotiate)

### <a name="single-authentication-protocol"></a><a name="BKMK_SingleAuth"></a>Protocolo de autenticação única
Quando um único protocolo aceitável é especificado no servidor, o computador cliente deve dar suporte ao protocolo especificado ou a comunicação falha. Quando um único protocolo aceitável é especificado, a troca de autenticação ocorre da seguinte maneira:

1.  O computador cliente solicita acesso a um serviço.

2.  O servidor responde à solicitação e especifica o protocolo que será usado.

3.  O computador cliente examina o conteúdo da resposta e verifica para determinar se ele oferece suporte ao protocolo especificado. Se o computador cliente oferecer suporte ao protocolo especificado, a autenticação continuará. Se o computador cliente não oferecer suporte ao protocolo, a autenticação falhará, independentemente de o computador cliente estar autorizado a acessar o recurso.

### <a name="negotiate-option"></a><a name="BKMK_Negotiate"></a>Opção Negotiate
A opção Negotiate pode ser usada para permitir que o cliente e o servidor tentem encontrar um protocolo aceitável. Isso se baseia no mecanismo de negociação GSS-API simples e protegido (SPNEGO). Quando a autenticação começa com a opção de negociar um protocolo de autenticação, o intercâmbio SPNEGO ocorre da seguinte maneira:

1.  O computador cliente solicita acesso a um serviço.

2.  O servidor responde com uma lista de protocolos de autenticação que ele pode dar suporte e um desafio de autenticação ou resposta, com base no protocolo que é sua primeira opção. Por exemplo, o servidor pode listar o protocolo Kerberos e NTLM e enviar uma resposta de autenticação Kerberos.

3.  O computador cliente examina o conteúdo da resposta e verifica para determinar se ele oferece suporte a qualquer um dos protocolos especificados.

    -   Se o computador cliente der suporte ao protocolo preferencial, a autenticação continuará.

    -   Se o computador cliente não oferecer suporte ao protocolo preferencial, mas ele oferecer suporte a um dos outros protocolos listados pelo servidor, o computador cliente permitirá que o servidor saiba qual protocolo de autenticação ele suporta e a autenticação continuará.

    -   Se o computador cliente não oferecer suporte a nenhum dos protocolos listados, a troca de autenticação falhará.

## <a name="additional-references"></a>Referências adicionais
[Arquitetura de autenticação do Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169024(v=ws.10))