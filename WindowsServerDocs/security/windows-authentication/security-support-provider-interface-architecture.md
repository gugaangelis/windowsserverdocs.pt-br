---
title: "Arquitetura de Interface do provedor de suporte de segurança"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="security-support-provider-interface-architecture"></a>Arquitetura de Interface do provedor de suporte de segurança

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de referência para o profissional de TI descreve os protocolos de autenticação do Windows que são usados dentro da arquitetura de Interface de provedor de suporte de segurança (SSPI).

O Microsoft Security suporte a Interface do provedor (SSPI) é a base para autenticação do Windows. Aplicativos e serviços de infraestrutura que requerem autenticação usam SSPI fornecê-las.

SSPI é a implementação do genérico segurança serviço GSSAPI (API) em sistemas operacionais Windows Server. Para obter mais informações sobre GSSAPI, consulte RFC 2743 e RFC 2744 no banco de dados RFC IETF.

Provedores de suporte o padrão segurança (SSPs) que invocar protocolos de autenticação específicos no Windows são incorporados a SSPI como DLLs. Esses SSPs padrão são descritas nas seções a seguir. SSPs adicionais podem ser incorporadas se eles podem operar com a SSPI.

Conforme mostrado na imagem a seguir, a SSPI no Windows fornece um mecanismo que transporta tokens de autenticação no canal de comunicação existente entre o computador cliente e o servidor. Quando dois computadores ou dispositivos precisam ser autenticado para que eles podem se comunicar com segurança, as solicitações de autenticação são roteadas para SSPI, que conclui o processo de autenticação, independentemente do protocolo de rede atualmente em uso. A SSPI retorna objetos grandes binários transparentes. Estes são passados entre os aplicativos, momento em que eles podem ser passados para a camada SSPI. Assim, o SSPI permite que um aplicativo para usar vários modelos de segurança disponíveis em um computador ou rede sem alterar a interface para o sistema de segurança.

![Diagrama mostrando a arquitetura de Interface de provedor de suporte de segurança](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

As seções a seguir descrevem os SSPs padrão que interagem com o SSPI. Os SSPs são usados de maneiras diferentes em sistemas operacionais Windows a promoção de comunicação seguro em um ambiente de rede não segura.

-   [Provedor de suporte de segurança Kerberos](#BKMK_KerbSSP)

-   [Provedor de suporte de segurança NTLM](#BKMK_NTLMSSP)

-   [Provedor de suporte de segurança de resumo](#BKMK_DigestSSP)

-   [Provedor de suporte de segurança Schannel](#BKMK_SchannelSSP)

-   [Negotiate Security Support Provider](#BKMK_NegoSSP)

-   [Provedor de suporte de segurança de credencial](#BKMK_CredSSP)

-   [Negociar extensões Security Support Provider](#BKMK_NegoExtsSSP)

-   [Provedor de suporte de segurança de PKU2U](#BKMK_PKU2USSP)

Também incluído neste tópico:

[Seleção de provedor de suporte de segurança](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Provedor de suporte de segurança Kerberos
Este SSP usa apenas o protocolo Kerberos versão 5, conforme implementado pela Microsoft. Esse protocolo é baseado em RFC 4120 e revisões de rascunho a rede do grupo de trabalho. É um protocolo padrão do setor que é usado com uma senha ou um cartão inteligente para um logon interativo. Também é o método de autenticação preferencial para serviços do Windows.

Como o protocolo Kerberos tem sido o protocolo de autenticação padrão desde o Windows 2000, todos os serviços de domínio oferecer suporte a Kerberos SSP. Esses serviços incluem:

-   No Active Directory as consultas que usam o LDAP Lightweight Directory Access Protocol)

-   Gerenciamento remoto do servidor ou estação de trabalho que usa o serviço de chamada de procedimento remoto

-   Serviços de impressão

-   Autenticação de cliente-servidor

-   Acesso remoto a arquivos que usa o protocolo SMB Server Message Block () (também conhecido como Common Internet File System ou CIFS)

-   Gerenciamento de sistema de arquivos distribuído e referência

-   Autenticação de intranet para o Internet Information Services (IIS)

-   Autenticação de autoridade de segurança de protocolo IPSec (IPsec)

-   Solicitações de certificados nos serviços de certificados do Active Directory para usuários de domínio e computadores

Local: %windir%\Windows\System32\kerberos.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico, além do Windows Server 2003 e Windows XP.

**Recursos adicionais para o protocolo Kerberos e o Kerberos SSP**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: extensões do protocolo Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: extensões do protocolo Kerberos: serviço de usuário e a especificação do protocolo de delegação restrita](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/PA (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Aprimoramentos de Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) para o Windows Vista

-   [Alterações na autenticação Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) para Windows 7 

-   [Referência técnica de autenticação Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Provedor de suporte de segurança NTLM
O provedor de suporte de segurança NTLM (NTLM SSP) é um binário protocolo usado pela Interface de provedor de suporte de segurança (SSPI) para permitir a autenticação de desafio / resposta NTLM e negociar opções de integridade e a confidencialidade de mensagens. NTLM é usada sempre que a autenticação SSPI é usada, incluindo para autenticação de protocolo Server Message Block ou CIFS, autenticação HTTP negociar (por exemplo, autenticação da Web Internet) e o serviço de chamada de procedimento remoto. O NTLM SSP inclui o NTLM e NTLM versão 2 (NTLMv2) protocolos de autenticação.

Os sistemas operacionais Windows compatíveis pode usar o NTLM SSP para o seguinte:

-   Autenticação de cliente/servidor

-   Serviços de impressão

-   Acesso a arquivos usando CIFS (SMB)

-   Serviço de chamada de procedimento remoto seguro ou DCOM

Local: %windir%\Windows\System32\msv1_0.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico, além do Windows Server 2003 e Windows XP.

**Recursos adicionais para o protocolo NTLM e NTLM SSP**

-   [Pacote de autenticação Msv1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Alterações na autenticação NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) no Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Auditoria e restringir o guia de uso NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Provedor de suporte de segurança de resumo
Autenticação Digest é um padrão do setor que é usado para Lightweight Directory Access Protocol (LDAP) e autenticação da web. A autenticação Digest transmite credenciais através da rede como um resumo de hash ou mensagem MD5.

Resumo SSP (WDigest) é usada para o seguinte:

-   Acesso à Internet Explorer e o Internet Information Services (IIS)

-   Consultas LDAP

Local: %windir%\Windows\System32\Digest.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico, além do Windows Server 2003 e Windows XP.

**Recursos adicionais para o protocolo de resumo e o SSP de resumo**

-   [Autenticação Digest Microsoft (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: digest extensões de protocolo](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Provedor de suporte de segurança Schannel
O canal seguro (Schannel) é usado para autenticação de servidor baseados na web, como quando um usuário tenta acessar um servidor web seguro.

O protocolo TLS, protocolo SSL, o protocolo de tecnologia de comunicações privadas (PCT) e o protocolo de camada de transporte de datagrama (DTLS) são baseadas em criptografia de chave pública. Schannel fornece todos esses protocolos. Todos os protocolos Schannel usam um modelo de cliente/servidor. O SSP Schannel usa certificados de chave pública para autenticar partes. Durante a autenticação de partes, Schannel SSP seleciona um protocolo na seguinte ordem de preferência de:

-   Transporte Layer Security (TLS) versão 1.0

-   Transporte Layer Security (TLS) versão 1.1

-   Transporte Layer Security (TLS) versão 1.2

-   Secure Socket Layer (SSL) versão 2.0

-   Secure Socket Layer (SSL) versão 3.0

-   Tecnologia de comunicações privadas (PCT)

    **Observação** PCT está desabilitado por padrão.

O protocolo selecionado é o protocolo de autenticação preferencial que o cliente e o servidor podem dar suporte. Por exemplo, se todos os protocolos Schannel dá suporte a um servidor e cliente suporta apenas SSL 3.0 e SSL 2.0, o processo de autenticação usa SSL 3.0.

DTLS é usado quando chamado explicitamente pelo aplicativo. Para obter mais informações sobre DTLS e os protocolos que são usados pelo provedor de Schannel, consulte [referência de provedor de suporte técnico segurança Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Local: %windir%\Windows\System32\Schannel.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico, além do Windows Server 2003 e Windows XP.

> [!NOTE]
> TLS 1.2 foi introduzido nesse provedor no Windows Server 2008 R2 e Windows 7. DTLS foi introduzido nesse provedor no Windows Server 2012 e Windows 8.

**Recursos adicionais para os protocolos TLS e SSL e o Schannel SSP**

-   [Canal seguro (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Referência técnica de TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: Layer Security (TLS) perfil de transporte](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negotiate Security Support Provider
O mecanismo de negociação GSS-API protegido (SPNEGO) e simples forma a base para o Negotiate SSP, whichcan ser usado para negociar um protocolo de autenticação específico. Quando um aplicativo chama SSPI para fazer logon uma rede, ele pode especificar um SSP para processar a solicitação. Se o aplicativo especifica o Negotiate SSP, ele analisa a solicitação e seleciona o provedor apropriado para lidar com a solicitação, com base nas políticas de segurança configuradas pelo cliente.

SPNEGO é especificado em RFC 2478.

Em versões compatíveis dos sistemas operacionais Windows, a segurança negociar suporte provedor seleciona entre o protocolo Kerberos e NTLM. Negociar seleciona as Kerberos protocolo por padrão, a menos que esse protocolo não pode ser usado por um dos sistemas envolvidos na autenticação, ou o aplicativo de chamada não forneceu informações suficientes para usar o protocolo Kerberos.

Local: %windir%\Windows\System32\lsasrv.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico, além do Windows Server 2003 e Windows XP.

**Recursos adicionais para o Negotiate SSP**

-   [Microsoft negociar (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: extensões de mecanismo (SPNEGO) de negociação GSS-API simples e protegido](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: negociar e especificação de protocolo de autenticação de HTTP Nego2](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Provedor de suporte de segurança de credencial
O provedor de serviços de segurança de credencial (CredSSP) fornece uma único (SSO) usuário experiência de logon ao iniciar novas sessões de serviços de Terminal e serviços de área de trabalho remota. CredSSP permite que aplicativos delegar as credenciais dos usuários do computador cliente (usando o SSP de cliente) no servidor de destino (por meio do SSP de servidor), com base nas políticas do cliente. CredSSP políticas são configuradas usando a política de grupo e a delegação das credenciais está desativada por padrão.

Local: %windir%\Windows\System32\credssp.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico.

**Recursos adicionais para o SSP de credenciais**

-   [\[MS-CSSP\]: especificação de protocolo de CredSSP (provedor) de suporte de segurança de credenciais](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Provedor de serviços de segurança e SSO de credenciais para Terminal Services Logon](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Negociar extensões Security Support Provider
Negociar extensões (NegoExts) é um pacote de autenticação que negocia o uso de SSPs diferente NTLM ou o protocolo Kerberos, para cenários e aplicativos implementados pela Microsoft e outros softwares empresas.

Essa extensão para o pacote de negociação permite os seguintes cenários:

-   **Disponibilidade de cliente rich dentro de um sistema federado.** Documentos podem ser acessados nos sites do SharePoint, e eles podem ser editados usando um aplicativo do Microsoft Office completo.

-   **Cliente de rich suporte para serviços do Microsoft Office.** Os usuários podem entrar em serviços do Microsoft Office e usar um aplicativo do Microsoft Office completo.

-   **Hospedado do Microsoft Exchange Server e do Outlook.** Não há nenhuma confiança entre domínios estabelecida como servidor do Exchange está hospedado na web. Outlook usa o serviço Windows Live para autenticar os usuários.

-   **Disponibilidade de cliente rich entre computadores cliente e servidores.** Componentes de rede e autenticação do sistema operacional são usados.

O pacote Windows negociar trata o NegoExts SSP da mesma maneira como o faz para Kerberos e NTLM. NegoExts.dll é carregado na autoridade de sistema Local (LSA) no momento da inicialização. Quando uma solicitação de autenticação é recebida, com base na origem da solicitação, NegoExts negocia entre os SSPs com suporte. Ela coleta as credenciais e políticas, os criptografa e envia essas informações para o SSP apropriado, onde o token de segurança é criado.

Os SSPs compatíveis com NegoExts não são SSPs autônomos, como Kerberos e NTLM. Portanto, dentro do NegoExts SSP, quando o método de autenticação falha por qualquer motivo, uma mensagem de falha de autenticação será exibida ou registrada. Nenhuma renegociação ou fallback métodos de autenticação são possíveis.

Local: %windir%\Windows\System32\negoexts.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico, excluindo o Windows Server 2008 e Windows Vista.

### <a name="BKMK_PKU2USSP"></a>Provedor de suporte de segurança de PKU2U
O protocolo PKU2U foi introduzido e implementado como um SSP no Windows 7 e Windows Server 2008 R2. Este SSP permite a autenticação de ponto a ponto, principalmente por meio de mídia e recurso chamado grupo doméstico, que foi introduzido no Windows 7 de compartilhamento de arquivos. O recurso permite o compartilhamento entre computadores que não sejam membros de um domínio.

Local: %windir%\Windows\System32\pku2u.dll

Esse provedor está incluído por padrão nas versões designadas no **se aplica a** lista no início deste tópico, excluindo o Windows Server 2008 e Windows Vista.

**Recursos adicionais para o protocolo PKU2U e o SSP PKU2U**

-   [Apresentando a integração de identidade Online](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Seleção de provedor de suporte de segurança
O SSPI do Windows pode usar qualquer um dos protocolos com suporte por meio de provedores de suporte de segurança instalados. No entanto, porque nem todos os sistemas operacionais compatíveis com os mesmos pacotes de SSP qualquer determinado computador executando o Windows Server, clientes e servidores devem negociar usar um protocolo que dão suporte a ambas. Windows Server prefere computadores cliente e os aplicativos usam o protocolo Kerberos, um protocolo forte baseada em padrões, quando possível, mas o sistema operacional continua permitir que cliente e os computadores cliente aplicativos não compatíveis com o protocolo Kerberos para se autenticar.

Antes de autenticação pode levar a comunicação de lugar os dois computadores devem concordar em um protocolo que ambas podem dar suporte. Para qualquer protocolo ser usado por meio do SSPI, cada computador deve ter o SSP de apropriado. Por exemplo, para um computador cliente e servidor para usar o protocolo de autenticação Kerberos, eles precisam ambos suportar Kerberos v5. Windows Server usa a função **EnumerateSecurityPackages** para identificar quais SSPs têm suporte em um computador e o que são os recursos desses SSPs.

A seleção de um protocolo de autenticação pode ser manipulada em uma das seguintes maneiras:

1.  [Protocolo de autenticação única](#BKMK_SingleAuth)

2.  [Negociar opção](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocolo de autenticação única
Quando um único protocolo aceitável é especificado no servidor, o computador cliente deve suportar o protocolo especificado ou a falha de comunicação. Quando um único protocolo aceitável for especificado, a troca de autenticação ocorre da seguinte maneira:

1.  O computador cliente solicita acesso a um serviço.

2.  O servidor responde à solicitação e especifica o protocolo que será usado.

3.  O computador cliente examina o conteúdo da resposta e verificações para determinar se ele dá suporte para o protocolo especificado. Se o computador cliente dá suporte ao protocolo especificado, a autenticação continua. Se o computador cliente não dá suporte ao protocolo, a autenticação falha, independentemente se o computador cliente está autorizado a acessar o recurso.

### <a name="BKMK_Negotiate"></a>Negociar opção
A opção Negociar pode ser usada para permitir que o cliente e servidor tentar encontrar um protocolo aceitável. Isso é baseado no mecanismo de negociação GSS-API protegido (SPNEGO) e simples. Quando a autenticação começa com a opção Negociar para um protocolo de autenticação, o exchange SPNEGO ocorre da seguinte maneira:

1.  O computador cliente solicita acesso a um serviço.

2.  O servidor responde com uma lista de protocolos de autenticação que ele pode oferecer suporte e um desafio de autenticação ou a resposta, com base no protocolo que é sua primeira opção. Por exemplo, o servidor pode listar o protocolo Kerberos e NTLM e enviar uma resposta de autenticação Kerberos.

3.  O computador cliente examina o conteúdo da resposta e verificações para determinar se ele dá suporte para qualquer um dos protocolos especificados.

    -   Se o computador cliente dá suporte ao protocolo preferencial, autenticação de receita.

    -   Se o computador cliente não dá suporte ao protocolo preferencial, mas ele dá suporte a um dos outros protocolos listados pelo servidor, o computador cliente permite que o servidor sabe qual protocolo de autenticação que ela suporta e a autenticação de receita.

    -   Se o computador cliente não oferece suporte a qualquer um dos protocolos listados, a troca de autenticação falhará.

## <a name="see-also"></a>Consulte também
[Arquitetura de autenticação do Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


