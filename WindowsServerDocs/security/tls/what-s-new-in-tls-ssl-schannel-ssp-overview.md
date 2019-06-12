---
title: TLS / SSL (Schannel SSP) Overview
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: b1af556200c9dd497bac835f1480479cca075dab
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447287"
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Visão geral do TLS / SSL (Schannel SSP)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Este tópico para profissionais de TI descreve as alterações na funcionalidade em segurança suporte Provider (SSP Schannel), que inclui a segurança de camada de transporte (TLS), o protocolo (SSL) e a camada de segurança DTLS (Datagram Transport) protocolos de autenticação para o Windows Server 2012 R2, Windows Server 2012, Windows 8.1 e Windows 8.

Schannel é um SSP (Provedor de Suporte de Segurança) que implementa os protocolos de autenticação padrão da Internet SSL, TLS e DTLS. A Interface SSPI é uma API usada por sistemas Windows para executar funções relacionadas à segurança, incluindo autenticação. A SSPI funciona como uma interface comum para vários SSPS, incluindo o SSP Schannel.

Para obter mais informações sobre a implementação da Microsoft de TLS e SSL no SSP Schannel, consulte o [referência técnica de TLS/SSL (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


## <a name="tlsssl-schannel-ssp-features"></a>Recursos TLS/SSL (Schannel SSP)
O exemplo a seguir descreve os recursos do TLS em SSP. o Schannel

### <a name="tls-session-resumption"></a>Retomada da sessão TLS
O protocolo TLS (Transport Layer Security), um componente do Provedor de Suporte de Segurança Schannel, é usado para proteger os dados enviados entre os aplicativos em uma rede não confiável. Você pode usar o TLS/SSL para autenticar servidores e computadores cliente, e também para criptografar mensagens entre as partes autenticadas.

Os dispositivos que conectam o TLS aos servidores frequentemente precisam se reconectar devido à expiração de sessão. Windows 8.1 e Windows Server 2012 R2 agora dão suporte a RFC 5077 (retomada da sessão TLS sem estado do lado do servidor). Essa modificação fornece dispositivos Windows Phone e Windows RT com:

-   Uso reduzido de recursos do servidor

-   Redução de largura de banda, o que melhora a eficiência das conexões de clientes

-   Menos tempo gasto para o handshake do TLS devido às retomadas de conexão.

> [!NOTE]
> A implementação do lado do cliente do RFC 5077 foi adicionada no Windows 8.

Para obter informações sobre a retomada da sessão TLS sem monitoração de estado, consulte o documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>Negociação de protocolos de aplicativos
 Windows Server 2012 R2 e Windows 8.1 dar suporte a negociação de protocolo de aplicativo do lado do cliente TLS para que os aplicativos possam aproveitar os protocolos como parte do desenvolvimento padrão HTTP 2.0 e os usuários podem acessar serviços online, como Google e Twitter usando aplicativos em execução o protocolo SPDY.

**Como ele funciona**

Os aplicativos de cliente e servidor permitem a extensão da negociação de protocolos de aplicativos fornecendo listas das IDs de protocolos de aplicativos com suporte, em ordem decrescente de preferência. O cliente do TLS indica que ele dá suporte à negociação de protocolos de aplicativos incluindo a extensão da negociação de protocolos de camada de aplicativos (ALPN) com uma lista de protocolos com suporte cliente na mensagem ClientHello.

Quando o cliente do TLS faz a solicitação ao servidor, o servidor do TLS lê sua lista de protocolos com suporte em busca do protocolo de aplicativos mais preferencial ao qual o cliente também dá suporte. Se esse protocolo for encontrado, o servidor responde com a ID do protocolo selecionado e continua com o handshake como de costume. Se não houver nenhum protocolo de aplicativos em comum, o servidor envia um alerta de falha fatal do handshake.

### <a name="BKMK_TrustedIssuers"></a>Gerenciamento de emissores confiáveis para autenticação de cliente
Quando a autenticação do computador cliente é necessária usando SSL ou TLS, você pode configurar o servidor para enviar uma lista de emissores de certificado confiáveis. Essa lista contém o conjunto de emissores de certificado em que o servidor vai confiar e fornece uma dica para o computador cliente quanto a qual certificado de cliente selecionar se houver vários certificados presentes. Além disso, a cadeia de certificados que o computador cliente envia ao servidor deve ser validada em relação à lista de emissores confiáveis configurada.

Antes do Windows Server 2012 e Windows 8, aplicativos ou processos que usavam o SSP Schannel (incluindo http. sys e IIS) pode fornecer uma lista de emissores confiáveis aos quais eles davam suporte para autenticação de cliente por meio de uma lista de confiança de certificado (CTL).

No Windows Server 2012 e Windows 8, foram feitas alterações para o processo de autenticação subjacente, de modo que:

-   Não há mais suporte para o gerenciamento da lista de emissores confiáveis com base em CTL.

-   O comportamento para enviar a lista de emissores confiáveis é desabilitado de modo padrão: Agora, o valor padrão da chave do Registro SendTrustedIssuerList é 0 (desabilitado de modo padrão), em vez de 1.

-   A compatibilidade com versões anteriores dos sistemas operacionais Windows é preservada.

**Qual valor isso adicionar?**

Começando com o Windows Server 2012, o uso da CTL foi substituído por uma implementação baseada em repositório de certificados. Isso permite uma capacidade de gerenciamento mais familiar por meio dos commandlets existentes de gerenciamento de certificados do provedor do Windows PowerShell e das ferramentas de linha de comando como certutil.exe.

Embora o tamanho máximo da lista de autoridades de certificação confiáveis que o SSP Schannel dá suporte (16 KB) continue o mesmo do Windows Server 2008 R2, no Windows Server 2012 há um novo repositório de certificados dedicado para emissores de autenticação de cliente, de modo que certificados não relacionados não são incluídos na mensagem.

**Como ele funciona?**

No Windows Server 2012, a lista de emissores confiáveis é configurada usando repositórios de certificados; repositório de certificados de computador global um padrão e outro que é opcional por site. A origem da lista será determinada da seguinte maneira:

-   Se houver um repositório de credenciais específico configurado para o site, ele será usado como a origem

-   Se nenhum certificado existir no repositório definido pelo aplicativo, o Schannel verificará o repositório **Emissores de autenticação de clientes** do computador local e, se os certificados estiverem presentes, usará esse repositório como a origem. Se nenhum certificado for encontrado em um repositório, o repositório de Raízes Confiáveis será verificado.

-   Se nenhum dos repositórios locais ou globais contiverem certificados, o provedor Schannel usará o **autoridades de certificação raiz confiáveis** armazenar como a origem da lista de emissores confiáveis. (Esse é o comportamento do Windows Server 2008 R2).

Se o **autoridades de certificação raiz confiáveis** repositório que foi usado contiver uma mistura de raiz (autoassinado) e certificados do emissor de autoridade de certificação, somente os certificados de emissor de autoridade de certificação serão enviados ao servidor por padrão.

**Como configurar o Schannel para usar o repositório de certificados de emissores confiáveis**

A arquitetura do SSP do Schannel no Windows Server 2012 por padrão usará os repositórios conforme descrito acima para gerenciar a lista de emissores confiáveis. Você ainda pode usar os commandlets existentes de gerenciamento de certificados do provedor do PowerShell, bem como as ferramentas de linha de comando como Certutil, para gerenciar certificados.

Para obter informações sobre como gerenciar certificados usando o provedor do PowerShell, consulte [Cmdlets de administração do AD CS no Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

Para obter informações sobre como gerenciar certificados usando o utilitário de certificado, consulte [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

Para obter informações sobre quais dados, incluindo o armazenamento definido pelo aplicativo, são definidos para uma credencial do Schannel, consulte [estrutura SCHANNEL_CRED (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Padrões dos modos de confiança**

O provedor Schannel dá suporte a três modos de confiança de autenticação de clientes. O modo de confiança controla como a validação da cadeia de certificados do cliente é executada e é uma configuração de todo o sistema controlada pelo REG_DWORD "ClientAuthTrustMode" em hkey_local_machine\system\currentcontrolset\control\securityproviders\schannel. .

|Valor|Modo de confiança|Descrição|
|-----|-------|--------|
|0|Confiança do computador (padrão)|Requer que o certificado do cliente seja emitido por um certificado da lista de emissores confiáveis.|
|1|Confiança raiz exclusiva|Requer que um certificado de cliente esteja ligado a um certificado raiz contido no repositório de emissor confiável especificado pelo chamador. O certificado também deve ser emitido por um emissor da lista de emissores confiáveis|
|2|Confiança de AC exclusiva|Requer que a cadeia de certificados de um cliente esteja ligado a um Certificado de Autoridade de Certificação intermediário ou a um certificado raiz do repositório do emissor confiável especificado pelo chamador.|

Para obter informações sobre falhas de autenticação devido a problemas de configuração de emissores confiáveis, consulte o artigo Knowledge Base [280256](https://support.microsoft.com/kb/2802568).

### <a name="BKMK_SNI"></a>Suporte do TLS para extensões de indicador de nome de servidor (SNI)
O recurso de Indicação do Nome do Servidor estende os protocolos SSL e TLS para permitir identificação adequada do servidor quando diversas imagens virtuais são executadas em um único servidor. Para proteger corretamente a comunicação entre um computador cliente e um servidor, o computador cliente solicita um certificado digital do servidor. Após o servidor responder ao pedido e enviar o certificado, o computador cliente o analisa, usa para criptografar a comunicação e prossegue com a troca normal de solicitação e resposta. No entanto, em um cenário de hospedagem virtual, vários domínios, cada um com o seu próprio certificado potencialmente distinto, são hospedados em um servidor. Nesse caso, o servidor não tem como saber previamente qual certificado enviar para o computador cliente. O SNI permite que o computador cliente informe o domínio de destino antecipadamente no protocolo, e isso permite que o servidor selecione corretamente o certificado adequado.

**Qual valor isso adicionar?**

Esta funcionalidade adicional:

-   Permite hospedar vários sites SSL em uma única combinação de IP e porta

-   Reduz o uso de memória quando diversos sites SSL são hospedados em um único servidor Web

-   Permite que mais usuários se conectem aos meus sites SSL simultaneamente

-   Permite que você forneça dicas para usuários finais através da interface do computador para selecionar o certificado correto durante um processo de autenticação do cliente.

**Como ele funciona**

O SSP Schannel mantém um cache na memória dos estados de conexão permitidos para clientes. Isso permite que os computadores cliente se reconectem rapidamente ao servidor SSL sem se sujeitarem a um handshake SSL completo em visitas subsequentes.  Esse uso eficiente do gerenciamento de certificados permite que mais sites sejam hospedados em um único Windows Server 2012 em comparação com versões anteriores do sistema operacional.

A seleção do certificado por parte do usuário final foi melhorada, ao permitir que você construa uma lista de nomes prováveis de emissores de certificado que fornecem dicas ao usuário final sobre qual escolher. Essa lista é configurável usando a Política de Grupo.

### <a name="BKMK_DTLS"></a>Segurança da camada de transporte do datagrama (DTLS)
O protocolo DTLS versão 1.0 foi adicionado ao Provedor de Suporte de Segurança Schannel. O protocolo DTLS proporciona privacidade nas comunicações para protocolos de datagrama. O protocolo permite que os aplicativos cliente/servidor se comuniquem de uma maneira concebida para impedir a espionagem, sabotagem ou falsificação de mensagem. O protocolo DTLS baseia-se no protocolo Transport Layer Security (TLS) e fornece garantias de segurança equivalentes, reduzindo a necessidade de usar o IPsec ou criar um protocolo personalizado de segurança de camada de aplicativos.

**Qual valor isso adicionar?**

Datagramas são comuns em streaming de mídia, como jogo ou videoconferência segura. Adicionar o protocolo DTLS ao provedor Schannel possibilita usar o modelo de SSPI familiar do Windows para proteger as comunicações entre computadores cliente e servidores. O DTLS foi deliberadamente concebido para ser o mais semelhante possível ao TLS, tanto para minimizar novas invenções de segurança quanto para maximizar a reutilização de código e infraestrutura.

**Como ele funciona**

Aplicativos que usam DTLS em UDP podem usar o modelo SSPI no Windows Server 2012 e Windows 8. Alguns pacotes de codificações estão disponíveis para configuração, de maneira semelhante à forma como se configura o TLS. O Schannel continua a usar o provedor de criptografia CNG, que usa a certificação FIPS 140, introduzida no Windows Vista.

### <a name="BKMK_Deprecated"></a>Funcionalidade preterida
No SSP Schannel para Windows Server 2012 e Windows 8, não há nenhum recurso preterido ou a funcionalidade.

## <a name="see-also"></a>Consulte também
-   [Modelo de segurança de nuvem privada - funcionalidade Wrapper](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



