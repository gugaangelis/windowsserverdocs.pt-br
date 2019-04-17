---
title: "TLS - visão geral SSL (Schannel SSP)"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b846ed54a1f7c8ef7a85ea9f836ffa75d0036ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Visão geral de TLS - SSL (Schannel SSP)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve as alterações na funcionalidade no Schannel segurança Support Provider (SSP), que incluem a Transport Layer Security (TLS), o Secure Sockets Layer (SSL) e os protocolos de autenticação de datagrama Transport Layer Security (DTLS), para o Windows Server 2012 R2, Windows Server 2012, Windows 8.1 e Windows 8.

Schannel é um Security Support Provider (SSP) que implementa os protocolos de autenticação padrão SSL, TLS e DTLS Internet. A Interface de provedor de suporte de segurança (SSPI) é uma API usada pelos sistemas do Windows para executar funções relacionadas à segurança, incluindo a autenticação. A SSPI funciona como uma interface comum para vários provedores de suporte de segurança (SSPs), incluindo o Schannel SSP.

Para obter mais informações sobre a implementação da Microsoft de TLS e SSL no Schannel SSP, consulte o [referência técnica de TLS/SSL (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


##<a name="tlsssl-schannel-ssp-features"></a>Recursos TLS/SSL (Schannel SSP)
A seguir descreve os recursos de TLS no Schannel SSP.

### <a name="tls-session-resumption"></a>Continuidade da sessão TLS
O protocolo Transport Layer Security (TLS), um componente do Schannel Security Support Provider, é usado para proteger os dados que são enviados entre aplicativos em uma rede não confiável. TLS/SSL pode ser usado para autenticar servidores e computadores cliente e também para criptografar as mensagens entre as partes autenticadas.

Dispositivos que se conectam TLS aos servidores com frequência precisam se reconectar devido a expiração de sessão. Windows 8.1 e Windows Server 2012 R2 agora dão suporte RFC 5077 (TLS continuidade da sessão sem estado do lado do servidor). Essa modificação fornece dispositivos do Windows Phone e Windows RT com:

-   Uso de recursos reduzida no servidor

-   Reduzido de largura de banda, o que aumenta a eficiência das conexões de cliente

-   Reduzimos o tempo gasto para do handshake TLS devido continuações da conexão.

> [!NOTE]
> A implementação do lado do cliente do RFC 5077 foi adicionada no Windows 8.

Para obter informações sobre continuidade sem estado da sessão TLS, consulte o documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>Negociação do protocolo de aplicativo
 Windows Server 2012 R2 e Windows 8.1 dar suporte a negociação de protocolo de aplicativo do lado do cliente TLS para que os aplicativos podem aproveitar protocolos como parte do desenvolvimento padrão HTTP 2.0 e os usuários podem acessar os serviços online, como Google e Twitter usando aplicativos que estejam executando o protocolo SPDY.

**Como funciona**

Aplicativos cliente e servidor habilitar a extensão de negociação do protocolo de aplicativo, fornecendo listas de protocolo de aplicativo com suporte IDs, em ordem decrescente de preferência. O cliente TLS indica que dá suporte a negociação do protocolo de aplicativo, incluindo a extensão de negociação de protocolo de camada de aplicativo (ALPN) com uma lista dos protocolos com suporte de cliente na mensagem ClientHello.

Quando o cliente TLS faz a solicitação para o servidor, o servidor TLS lê é lista de protocolo com suporte para o protocolo de aplicativo mais preferenciais que o cliente também dá suporte. Se tal um protocolo for encontrado, o servidor responde com a ID do protocolo selecionado e continuará com o handshake como de costume. Se não houver nenhum protocolo de aplicativo comum, o servidor envia um alerta de falha de handshake fatal.

### <a name="BKMK_TrustedIssuers"></a>Gerenciamento de emissores confiáveis para autenticação de cliente
Quando a autenticação do computador cliente é necessária usando SSL ou TLS, o servidor pode ser configurado para enviar uma lista de emissores de certificados confiáveis. Essa lista contém o conjunto de emissores de certificado que o servidor confiará e uma dica para o computador cliente sobre qual certificado cliente e selecione se houver vários certificados presentes. Além disso, a cadeia de certificados que do computador cliente envia para o servidor deve ser validada em relação à lista de emissores confiáveis configurado.

Antes do Windows Server 2012 e Windows 8, aplicativos ou processos que usou o SSP Schannel (incluindo HTTP.sys e IIS) podem fornecer uma lista dos emissores confiáveis que eles têm suporte para autenticação de cliente por meio de um certificado confiável lista (lista de certificados confiáveis).

No Windows Server 2012 e Windows 8, as alterações foram feitas para o processo de autenticação subjacente para que:

-   Gerenciamento de lista com base em lista de certificados confiáveis emissor confiável não é mais compatível.

-   O comportamento para enviar a lista de emissor confiável por padrão é desativado: o valor padrão da chave do registro SendTrustedIssuerList agora é 0 (desativado por padrão) em vez de 1.

-   Compatibilidade com versões anteriores dos sistemas operacionais Windows é preservada.

**O valor isso adicionar?**

A partir do Windows Server 2012, o uso da lista de certificados confiáveis foi substituído por uma implementação de baseado em armazenamento de certificado. Isso possibilita a capacidade de gerenciamento mais familiar por meio do commandlets do gerenciamento certificado existente do provedor do PowerShell, bem como ferramentas de linha de comando, como certutil.exe.

Embora o tamanho máximo da lista de autoridades de certificação confiável que o Schannel SSP dá suporte (KB 16) permanece os mesmos que no Windows Server 2008 R2, no Windows Server 2012, há um certificado novo dedicado armazenar para emissores de autenticação de cliente para que não relacionados certificados não estão incluídos na mensagem.

**Como ele funciona?**

No Windows Server 2012, a lista de emissores confiáveis é configurada usando repositórios de certificados; repositório de certificados do padrão de um computador global e um que é opcional por site. A origem da lista será determinada da seguinte maneira:

-   Se houver um repositório de credencial específica configurado para o site, ela será usada como a fonte

-   Se nenhum certificado existe no repositório definida pelo aplicativo, Schannel verifica o **emissores de autenticação de cliente** armazenam no computador local e, se houver certificados, usa esse repositório como a origem. Se nenhum certificado for encontrado na loja, no repositório raízes confiáveis será verificado.

-   Se nenhuma das lojas globais ou locais contiverem certificados, o provedor Schannel usará o **autoridades de Certifictation raiz confiáveis** armazenar como a origem da lista de emissores confiáveis. (Esse é o comportamento para o Windows Server 2008 R2).

Se o **Certifictation autoridades confiáveis** repositório que foi usado contém uma mistura de raiz (autoassinados) e certificados de emissor de autoridade de certificação, somente os certificados CA emissor serão enviados ao servidor por padrão.

**Como configurar Schannel para usar o repositório de certificados para emissores confiáveis**

A arquitetura Schannel SSP no Windows Server 2012 por padrão usará as lojas, conforme descrito acima para gerenciar a lista de emissores confiáveis. Você ainda pode usar o commandlets do gerenciamento certificado existente do provedor do PowerShell, bem como ferramentas de linha de comando, como Certutil para gerenciar certificados.

Para obter informações sobre o gerenciamento de certificados usando o provedor do PowerShell, consulte [Cmdlets de administração do AD CS no Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

Para obter informações sobre o gerenciamento de certificados usando o utilitário de certificado, consulte [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

Para obter informações sobre quais dados, incluindo a store definida pelo aplicativo, são definidos para uma credencial Schannel, consulte [estrutura SCHANNEL_CRED (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Padrões para modos de confiança**

Há três cliente confiar em modos de autenticação aceitos pelo provedor de Schannel. O modo de confiança controla como a validação da cadeia de certificados do cliente é realizada e é uma configuração de todo o sistema controlada pelo REG_DWORD "ClientAuthTrustMode" em HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel.

|Valor|Confiar em modo|Descrição|
|-----|-------|--------|
|0|Confiança de máquina (padrão)|Requer que o certificado de cliente é emitido por um certificado na lista de emissores confiáveis.|
|1|Relação de confiança raiz exclusivos|Requer que um cliente de cadeias de um certificado raiz contidos no repositório especificadas pelo chamador confiável emissor de certificado. O certificado também deve ser emitido por um emissor na lista de emissores confiáveis|
|2|CA exclusivos confiança|Requer que uma cadeia de certificados de cliente para um certificado da CA intermediário ou o certificado raiz no especificado chamador confiável store emissor.|

Para obter informações sobre falhas de autenticação devido a problemas de configuração de emissores confiáveis, consulte o artigo do Knowledge Base [280256](https://support.microsoft.com/kb/2802568).

### <a name="BKMK_SNI"></a>Suporte a TLS extensões de indicador de nome de servidor (SNI)
Recurso de indicação de nome de servidor estende os protocolos SSL e TLS para permitir que a identificação apropriada do servidor quando várias imagens de virtual estão em execução em um único servidor. Para proteger a comunicação entre um computador cliente e um servidor corretamente, o computador cliente solicitar um certificado digital do servidor. Depois que o servidor responde à solicitação e envia o certificado, o computador cliente examina a ele, usa para criptografar a comunicação e prossegue com a troca de solicitação-resposta normal. No entanto, em um cenário de hospedagem virtual, vários domínios, cada um com seu próprio certificado potencialmente distinto, são hospedados em um servidor. Nesse caso, o servidor tem há maneira de saber de antemão qual certificado para enviar para o computador cliente. SNI permite que o computador cliente informar o domínio de destino anteriormente no protocolo, e isso permite que o servidor selecionar corretamente o certificado apropriado.

**O valor isso adicionar?**

Essa funcionalidade adicional:

-   Permite que você hospedar vários sites SSL em um único IP e a combinação de porta

-   Reduz o uso da memória quando vários sites SSL são hospedados em um servidor web único

-   Permite que os usuários mais para se conectar ao meu sites SSL simultaneamente

-   Permite que você forneça dicas para usuários finais por meio da interface do computador para selecionar o certificado correto durante um processo de autenticação de cliente.

**Como funciona**

O SSP Schannel mantém um cache na memória dos Estados de conexão do cliente permitida para clientes. Isso permite que os computadores cliente reconectá-lo rapidamente para o servidor SSL sem sujeitos a um handshake SSL completo em visitas subsequentes.  Esse uso eficiente de gerenciamento de certificados permite que sites mais sejam hospedados em um único Windows Server 2012 em comparação com versões anteriores do sistema operacional.

Seleção de certificado pelo usuário final foi aprimorada, permitindo que você construa uma lista de nomes de emissor de certificado provável que fornecem ao usuário final com dicas em qual delas escolher. Essa lista é configurável usando política de grupo.

### <a name="BKMK_DTLS"></a>Datagrama Transport Layer Security (DTLS)
O protocolo de versão 1.0 DTLS foi adicionado para o provedor de suporte de segurança Schannel. O protocolo DTLS fornece privacidade de comunicações para os protocolos de datagrama. O protocolo permite cliente/servidor aplicativos se comuniquem de maneira que foi projetada para impedir a interceptação, adulteração ou falsificação de mensagem. O protocolo DTLS é baseado no protocolo Transport Layer Security (TLS) e fornece garantias de segurança equivalente, reduzindo a necessidade de usar IPsec ou a criação de um protocolo de segurança de camada de aplicativo personalizado.

**O valor isso adicionar?**

Datagramas são comuns em streaming de mídia, como jogo ou protegida videoconferência. Adicionar o protocolo DTLS para o provedor Schannel oferece a capacidade de usar o modelo Windows SSPI familiar em proteger a comunicação entre computadores cliente e servidores. DTLS é projetada para ser mais semelhante ao TLS quanto possível, para minimizar invenção de segurança e para maximizar a quantidade de reutilização de código e infraestrutura.

**Como funciona**

Aplicativos que usam DTLS via UDP podem usar o modelo de SSPI no Windows Server 2012 e Windows 8. Determinados pacotes de codificação estão disponíveis para configuração, semelhante a como você pode configurar o TLS. Schannel continua a usar o provedor criptográfico CNG, que aproveita as vantagens da certificação FIPS 140, que foi introduzida no Windows Vista.

### <a name="BKMK_Deprecated"></a>Funcionalidade substituída
No Schannel SSP para Windows Server 2012 e Windows 8, não há nenhuma recursos preteridos ou funcionalidade.

## <a name="see-also"></a>Consulte também
-   [Modelo de segurança de nuvem privada - funcionalidade de Wrapper](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



