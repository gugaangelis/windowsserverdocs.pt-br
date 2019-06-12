---
title: Configurações do registro Layer Security (TLS) de transporte
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 02/28/2019
ms.openlocfilehash: 32068319aae7545675e126eed6e1ab4c914bcbcf
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812639"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Configurações do registro Layer Security (TLS) de transporte

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10

Este tópico de referência para profissionais de TI contém informações de configuração do registro com suporte para a implementação do Windows do protocolo Transport Layer Security (TLS) e o protocolo Secure Sockets Layer (SSL) por meio do suporte de segurança Schannel SSP (provedor). As subchaves do registro e entradas abordadas nesta Ajuda do tópico, administrar e solucionar problemas de SSP Schannel, especificamente os protocolos TLS e SSL. 

> [!CAUTION]
> Essas informações são fornecidas como uma referência a ser usada quando você estiver solucionando problemas ou verificando se as configurações necessárias estão aplicadas.
> É recomendável não editar diretamente o Registro, a menos que não haja outra alternativa.
> Modificações no Registro não são validadas pelo editor de Registro nem pelo sistema operacional Windows antes de serem aplicadas.
> Como resultado, valores incorretos podem ser armazenados, e isso pode resultar em erros irrecuperáveis no sistema.
> Quando possível, em vez de editar o Registro diretamente, use a Política de Grupo ou outras ferramentas do Windows, como o MMC (Console de Gerenciamento Microsoft) para realizar tarefas.
> Se você deve editar o Registro, tenha muito cuidado.

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Essa entrada não existe no Registro por padrão. O valor padrão é que todos os quatro métodos de mapeamento de certificado listados abaixo têm suporte.

Quando um aplicativo para servidores requer autenticação de cliente, o Schannel automaticamente tenta mapear o certificado fornecido pelo computador cliente para uma conta de usuário. Você pode autenticar os usuários que se conectam com um certificado de cliente, criando mapeamentos que se relacionam com as informações de certificado de uma conta de usuário do Windows. Depois de criar e habilitar um mapeamento de certificado, sempre que um cliente apresentar um certificado, o aplicativo para servidores associará automaticamente esse usuário à conta de usuário do Windows apropriada.

Na maioria dos casos, um certificado é mapeado para uma conta de usuário em uma de duas maneiras: 

- Um único certificado é mapeado para uma única conta de usuário (mapeamento um a um).
- Vários certificados são mapeados para várias contas de usuário (mapeamento muitos para um).

Por padrão, o provedor Schannel usará os seguintes quatro métodos de mapeamento de certificados, listados em ordem de preferência:

1. Mapeamento de certificado Kerberos S4U(Service-for-User)
2. Mapeamento de nome UPN
3. Mapeamento um a um (também conhecido como mapeamento de assunto/emissor)
4. Mapeamento muitos-para-um

Versões aplicáveis: Conforme indicado na lista **Aplica-se a** no começo deste tópico.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Criptografias

Codificações TLS/SSL devem ser controladas pela configuração da ordem do cipher suite. Para obter detalhes, consulte [Configurando o TLS Cipher Suite Order](manage-tls.md#configuring-tls-cipher-suite-order).

Para obter informações sobre a ordem de conjuntos de codificação padrão que são usadas pelo SSP Schannel, consulte [conjuntos de codificação no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

## <a name="ciphersuites"></a>Conjuntos de criptografia

Configurar conjuntos de codificação TLS/SSL deve ser feito usando a diretiva de grupo, MDM ou o PowerShell, consulte [Configurando o TLS Cipher Suite Order](manage-tls.md#configuring-tls-cipher-suite-order) para obter detalhes.

Para obter informações sobre a ordem de conjuntos de codificação padrão que são usadas pelo SSP Schannel, consulte [conjuntos de codificação no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Essa entrada controla a quantidade de tempo que o sistema operacional leva em milissegundos para finalizar entradas de cache do lado do cliente. Um valor 0 desativa o cache de conexão segura. Essa entrada não existe no Registro por padrão. 

Na primeira vez que um cliente se conecta a um servidor por meio do SSP Schannel, um handshake de TLS/SSL completo é executado. Quando isso é concluído, o segredo mestre, o conjunto de criptografias e os certificados são armazenados no cache da sessão, no respectivo cliente e servidor.

Começando com o Windows Server 2008 e Windows Vista, o tempo de cache do cliente padrão é de 10 horas.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempo de cache de cliente padrão

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

Grampeamento de protocolo de Status de certificado OCSP online permite que um servidor web, como o Internet Information Services (IIS), para fornecer o atual status de revogação de um certificado de servidor quando ele envia o certificado do servidor para um cliente durante o handshake TLS. Esse recurso reduz a carga nos servidores OCSP porque o servidor web pode armazenar em cache o status atual do OCSP do certificado do servidor e enviá-lo para vários clientes da web. Sem esse recurso, cada cliente da web tentar recuperar o status atual do OCSP do certificado do servidor do servidor OCSP. Isso geraria uma alta carga no servidor OCSP. 

Além do IIS, serviços web sobre HTTP. sys também podem aproveitar essa configuração, incluindo os serviços de Federação do Active Directory (AD FS) e Proxy de aplicativo da Web (WAP). 

Por padrão, o suporte OCSP está ativado para sites do IIS que têm uma associação simple de (SSL/TLS) segura. No entanto, esse suporte não está habilitado por padrão, se o site do IIS está usando um ou ambos os seguintes tipos de associações de seguro (SSL/TLS):
- Exigir a indicação de nome de servidor
- Usar repositório de certificados centralizados

Nesse caso, a resposta de saudação do servidor durante o handshake TLS não incluirá um status OCSP grampeado por padrão. Esse comportamento melhora o desempenho: A implementação de grampeamento OCSP do Windows pode ser dimensionado para centenas de certificados de servidor. Porque o SNI e CCS habilita o IIS dimensionar para milhares de sites que têm potencialmente milhares de certificados de servidor, a configurar esse comportamento a ser habilitado por padrão pode causar problemas de desempenho.

Versões aplicáveis: Todas as versões começando com o Windows Server 2012 e Windows 8. 

Caminho do registro: [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

Adicione a seguinte chave:

"EnableOcspStaplingForSni" = DWORD: 00000001

Para desabilitar, defina o valor DWORD para 0:

"EnableOcspStaplingForSni" = DWORD: 00000000

> [!NOTE] 
> Habilitar essa chave do registro tem um impacto no desempenho.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Esta entrada controla a conformidade do padrão FIPS. O padrão é 0.

Versões aplicáveis: Todas as versões começando com o Windows Server 2012 e Windows 8. 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\LSA

Conjuntos de criptografia FIPS do Windows Server: Ver [conjuntos de criptografia e protocolos com suporte no SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hashes

Algoritmos de hash TLS/SSL devem ser controlados pela configuração da ordem do cipher suite. Ver [Configurando o TLS Cipher Suite Order](manage-tls.md#configuring-tls-cipher-suite-order) para obter detalhes.

## <a name="issuercachesize"></a>IssuerCacheSize

Essa entrada controla o tamanho do cache emissor e é usada com o mapeamento do emissor. O SSP Schannel tenta mapear todos os emissores na cadeia de certificados do cliente — não apenas o emissor direto do certificado do cliente. Quando os emissores não são mapeados para uma conta, que é o caso típico, o servidor pode tentar mapear o mesmo nome do emissor repetidamente, centenas de vezes por segundo. 

Para evitar isso, o servidor tem um cache negativo, portanto, se um nome de emissor não é mapeado para uma conta, ele é adicionado ao cache e o SSP Schannel não tentará mapear o nome do emissor novamente, até que a entrada de cache expira. Essa entrada de Registro especifica o tamanho do cache. Essa entrada não existe no Registro por padrão. O valor padrão é 100. 

Versões aplicáveis: Todas as versões começando com o Windows Server 2008 e Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Essa entrada controla a duração do intervalo de tempo limite do cache em milissegundos. O SSP Schannel tenta mapear todos os emissores na cadeia de certificados do cliente — não apenas o emissor direto do certificado do cliente. Caso os emissores não sejam mapeados para uma conta, que é o caso típico, o servidor pode tentar mapear o mesmo nome do emissor repetidamente, centenas de vezes por segundo.

Para evitar isso, o servidor tem um cache negativo, portanto, se um nome de emissor não é mapeado para uma conta, ele é adicionado ao cache e o SSP Schannel não tentará mapear o nome do emissor novamente, até que a entrada de cache expira. Esse cache é mantido por motivos de desempenho, para que o sistema não continue tentando mapear os mesmos emissores. Essa entrada não existe no Registro por padrão. O valor padrão é 10 minutos.

Versões aplicáveis: Todas as versões começando com o Windows Server 2008 e Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm - tamanhos de chave RSA do cliente

Essa entrada controla os tamanhos de chave RSA cliente. 

Uso de algoritmos de troca de chaves deve ser controlado pela configuração da ordem do cipher suite.

Adicionado no Windows 10, versão 1507 e Windows Server 2016.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Para especificar um comprimento de bit de chave do intervalo com suporte mínimo de RSA para o cliente do TLS, crie uma **ClientMinKeyBitLength** entrada. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, 1.024 bits será o valor mínimo. 

Para especificar um comprimento de bit de chave máximo intervalo com suporte do RSA para o cliente do TLS, crie uma **ClientMaxKeyBitLength** entrada. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, um máximo não é imposto.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm - tamanhos de chave Diffie-Hellman

Essa entrada controla os tamanhos de chave Diffie-Hellman. 

Uso de algoritmos de troca de chaves deve ser controlado pela configuração da ordem do cipher suite.

Adicionado no Windows 10, versão 1507 e Windows Server 2016.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Para especificar um comprimento de bit de chave mínimo com suporte intervalo de Diffie-Helman para o cliente do TLS, crie uma **ClientMinKeyBitLength** entrada. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, 1.024 bits será o valor mínimo. 
 
Para especificar um comprimento de bit de chave máximo com suporte intervalo de Diffie-Helman para o cliente do TLS, crie uma **ClientMaxKeyBitLength** entrada. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, um máximo não é imposto. 
 
Para especificar o comprimento de bit de chave Diffie-Helman para o padrão do servidor TLS, crie uma **ServerMinKeyBitLength** entrada. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, 2048 bits será o padrão. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Essa entrada controla o número máximo de elementos de cache. A configuração de MaximumCacheSize como 0 desabilita o cache de sessão do servidor e impede a reconexão. O aumento de MaximumCacheSize acima dos valores padrão faz com que o Lsass.exe consuma memória adicional. Cada elemento de cache de sessão normalmente requer de 2 a 4 KB de memória. Essa entrada não existe no Registro por padrão. O valor padrão é 20.000 elementos. 

Versões aplicáveis: Todas as versões começando com o Windows Server 2008 e Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Sistema de mensagens – análise de fragmento

________________________________________
Essa entrada controla o tamanho máximo permitido de mensagens de handshake TLS fragmentados que serão aceitos. Mensagens maiores do que o tamanho permitido não serão aceito e o handshake TLS falhará. Essas entradas não existem no registro por padrão. 

Quando você define o valor como 0x0, mensagens fragmentadas não são processadas e fará com que o handshake TLS falhe. Isso torna TLS clientes ou servidores no computador atual não compatível com as RFCs do TLS. 

O número máximo permitido de tamanho pode ser aumentado até 2 ^ 24-1 bytes. Permitindo que um cliente ou servidor ler e armazenar grandes quantidades de dados não verificados da rede não é uma boa ideia e consumirá mais memória para cada contexto de segurança. 

Adicionado no Windows 7 e Windows Server 2008 R2.
Uma atualização que habilita o Internet Explorer no Windows XP, Windows Vista ou Windows Server 2008 para analisar mensagens de handshake TLS/SSL fragmentadas está disponível.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Para especificar um tamanho máximo permitido de mensagens de handshake TLS fragmentados que aceita o cliente do TLS, crie uma **MessageLimitClient** entrada. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, o valor padrão será 0x8000 bytes. 

Para especificar um tamanho máximo permitido de mensagens de handshake TLS fragmentados que o servidor TLS aceitará quando não há nenhuma autenticação de cliente, crie uma **MessageLimitServer** entrada. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, o valor padrão será 0x4000 bytes. 

Para especificar um tamanho máximo permitido de mensagens de handshake TLS fragmentados que o servidor TLS aceitará quando há autenticação de cliente, crie uma **MessageLimitServerClientAuth** entrada. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não configurado, o valor padrão será 0x8000 bytes. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Essa entrada controla o sinalizador que é usado quando a lista de emissores confiáveis é enviada. No caso de servidores que confiam centenas de autoridades de certificação para autenticação de cliente, há muitos emissores de servidor capaz de enviá-los todos para o computador cliente ao solicitar a autenticação do cliente. Nessa situação, essa chave do Registro pode ser definida e, em vez de enviar uma lista parcial, o SSP Schannel não enviar nenhuma lista para o cliente.

O não envio de uma lista de emissores confiáveis pode afetar o que o cliente envia quando lhe é solicitado um certificado de cliente. Por exemplo, quando o Internet Explorer recebe uma solicitação de autenticação de cliente, ele exibe apenas os certificados de cliente que encadeiam até uma das autoridades de certificação enviadas pelo servidor. Se o servidor não tiver enviado a uma lista, o Internet Explorer exibirá todos os certificados de cliente que estão instalados no cliente. 

Esse comportamento pode ser desejável. Por exemplo, quando os ambientes de PKI incluem certificados cruzados, os certificados de cliente e servidor não terão a mesma AC raiz; portanto, o Internet Explorer não pode escolher um certificado vinculado a uma das autoridades de certificação do servidor. Ao configurar o servidor para não enviar uma lista de emissores confiáveis, o Internet Explorer enviará todos os seus certificados.

Essa entrada não existe no Registro por padrão.

Comportamento de envio de lista de emissores confiáveis padrão

| Versão do Windows | Time |
|-----------------|------|
| Windows Server 2012 e Windows 8 e posterior | FALSE |
| Windows Server 2008 R2 e Windows 7 e anteriores | TRUE |

Versões aplicáveis: Todas as versões começando com o Windows Server 2008 e Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Essa entrada controla a quantidade de tempo que o sistema operacional leva em milissegundos para finalizar entradas de cache do lado do servidor. Um valor de 0 desabilita o cache de sessão do lado do servidor e impede a reconexão. O aumento de ServerCacheTime acima dos valores padrão faz com que o Lsass.exe consuma memória adicional. Cada elemento de cache de sessão normalmente requer de 2 a 4 KB de memória. Essa entrada não existe no Registro por padrão. 

Versões aplicáveis: Todas as versões começando com o Windows Server 2008 e Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempo de cache do servidor padrão: 10 horas

## <a name="ssl-20"></a>SSL 2.0

Essa subchave controla o uso do SSL 2.0.

Começando com o Windows 10, versão 1607 e Windows Server 2016, o SSL 2.0 foi removido e não é mais suportada.
Para configurações de um padrão de SSL 2.0, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo SSL 2.0, crie uma **Enabled** entrada na subchave do cliente ou do servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves de SSL 2.0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do SSL 2.0 no cliente SSL. |
| Servidor | Controla o uso do SSL 2.0 no servidor SSL. |

Para desabilitar o SSL 2.0 para o cliente ou servidor, altere o valor DWORD para 0. Se um aplicativo SSPI solicitações para usar o SSL 2.0, ele será negado. 

Para desabilitar o SSL 2.0 por padrão, crie uma **DisabledByDefault** valor de entrada e altere o valor DWORD como 1. Se um explicitamente de aplicativo SSPI solicitações para usar o SSL 2.0, pode ser negociado. 

O exemplo a seguir mostra o SSL 2.0 desabilitados no registro:

![SSL 2.0 desabilitado](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Essa subchave controla o uso do SSL 3.0.

Começando com o Windows 10, versão 1607 e Windows Server 2016, SSL 3.0 foi desabilitado por padrão. Para as configurações padrão do SSL 3.0, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo SSL 3.0, crie uma **Enabled** entrada na subchave do cliente ou do servidor, conforme descrito na tabela a seguir.  
Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves de SSL 3.0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do SSL 3.0 no cliente SSL. |
| Servidor | Controla o uso do SSL 3.0 no servidor SSL. |

Para desabilitar o SSL 3.0 para o cliente ou servidor, altere o valor DWORD para 0.
Se um aplicativo SSPI solicitações para usar o SSL 3.0, ele será negado. 

Para desabilitar o SSL 3.0 por padrão, crie uma **DisabledByDefault** valor de entrada e altere o valor DWORD como 1. Se um aplicativo SSPI solicita explicitamente para usar o SSL 3.0, ele pode ser negociado. 

O exemplo a seguir mostra o SSL 3.0 desabilitados no registro:

![SSL 3.0 desabilitado](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Essa subchave controla o uso do TLS 1.0.

Para as configurações padrão de TLS 1.0, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo TLS 1.0, crie uma **Enabled** entrada na subchave do cliente ou do servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves do TLS 1.0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do TLS 1.0 no cliente TLS. |
| Servidor | Controla o uso do TLS 1.0 no servidor do TLS. |

Para desabilitar o TLS 1.0 para o cliente ou servidor, altere o valor DWORD para 0.
Se um aplicativo SSPI solicitações para usar o TLS 1.0, ele será negado. 

Para desabilitar o TLS 1.0 por padrão, crie uma **DisabledByDefault** valor de entrada e altere o valor DWORD como 1. Se um aplicativo SSPI solicita explicitamente para usar TLS 1.0, ele pode ser negociado. 

O exemplo a seguir mostra o TLS 1.0 desabilitada no registro:

![TLS 1.0 desabilitada](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Essa subchave controla o uso do TLS 1.1.

Para as configurações padrão de TLS 1.1, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo TLS 1.1, crie uma **Enabled** entrada na subchave do cliente ou do servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves do TLS 1.1

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do TLS 1.1 no cliente TLS. |
| Servidor | Controla o uso do TLS 1.1 no servidor do TLS. |

Para desabilitar o TLS 1.1 para o cliente ou servidor, altere o valor DWORD para 0.
Se um aplicativo SSPI solicitações para usar o TLS 1.1, ele será negado. 

Para desabilitar o TLS 1.1 por padrão, crie uma **DisabledByDefault** valor de entrada e altere o valor DWORD como 1. Se um aplicativo SSPI solicita explicitamente para usar o TLS 1.1, ele pode ser negociado. 

O exemplo a seguir mostra o TLS 1.1 desabilitados no registro:

![TLS 1.1 desabilitado](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Essa subchave controla o uso do TLS 1.2.

Para as configurações padrão de TLS 1.2, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo TLS 1.2, crie uma **Enabled** entrada na subchave do cliente ou do servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves do TLS 1.2

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do TLS 1.2 no cliente TLS. |
| Servidor | Controla o uso do TLS 1.2 no servidor do TLS. |

Para desabilitar o TLS 1.2 para o cliente ou servidor, altere o valor DWORD para 0.
Se um aplicativo SSPI solicitações para usar o TLS 1.2, ele será negado. 

Para desabilitar o TLS 1.2 por padrão, crie uma **DisabledByDefault** valor de entrada e altere o valor DWORD como 1. Se um aplicativo SSPI solicita explicitamente para usar o TLS 1.2, ele pode ser negociado. 

O exemplo a seguir mostra o TLS 1.2 desabilitados no registro:

![TLS 1.2 desabilitado](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Essa subchave controla o uso do DTLS 1.0.

Para as configurações padrão do DTLS 1.0, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo DTLS 1.0, crie uma **Enabled** entrada na subchave do cliente ou do servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves do DTLS 1.0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do DTLS 1.0 no cliente DTLS. |
| Servidor | Controla o uso do DTLS 1.0 no servidor DTLS. |

Para desabilitar o DTLS 1.0 para o cliente ou servidor, altere o valor DWORD para 0.
Se um aplicativo SSPI solicitações para usar o DTLS 1.0, ele será negado. 

Para desabilitar o DTLS 1.0 por padrão, crie uma **DisabledByDefault** valor de entrada e altere o valor DWORD como 1. Se um aplicativo SSPI solicita explicitamente para usar o DTLS 1.0, ele pode ser negociado. 

O exemplo a seguir mostra 1.0 DTLS desabilitados no registro:

![1.0 DTLS desabilitado](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1.2

Essa subchave controla o uso do DTLS 1.2.

Para as configurações padrão de DTLS 1.2, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo DTLS 1.2, crie uma **Enabled** entrada na subchave do cliente ou do servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves DTLS 1.2

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do DTLS 1.2 no cliente DTLS. |
| Servidor | Controla o uso do DTLS 1.2 no servidor DTLS. |


Para desabilitar o DTLS 1.2 para o cliente ou servidor, altere o valor DWORD para 0.
Se um aplicativo SSPI solicitações para usar o DTLS 1.0, ele será negado. 

Para desabilitar o DTLS 1.2 por padrão, crie uma **DisabledByDefault** valor de entrada e altere o valor DWORD como 1. Se um aplicativo SSPI solicita explicitamente para usar o DTLS 1.2, ele pode ser negociado. 

O exemplo a seguir mostra o 1.1 DTLS desabilitados no registro:

![1.1 DTLS desabilitado](images/dtls-11-registry-setting.png)


