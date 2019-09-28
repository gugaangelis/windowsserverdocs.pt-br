---
title: Configurações de registro de TLS (segurança de camada de transporte)
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 02/28/2019
ms.openlocfilehash: 60202e537093bd21515043ba56f70f3895c91d42
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403403"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Configurações de registro de TLS (segurança de camada de transporte)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows 10

Este tópico de referência para o profissional de ti contém informações de configuração de registro com suporte para a implementação do Windows do protocolo TLS e o protocolo protocolo SSL (SSL) por meio do suporte de segurança Schannel Provedor (SSP). As subchaves do registro e as entradas abordadas neste tópico ajudam você a administrar e solucionar problemas do SSP do Schannel, especificamente os protocolos TLS e SSL. 

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

As codificações TLS/SSL devem ser controladas com a configuração da ordem do conjunto de codificação. Para obter detalhes, consulte [Configuring TLS Cipher Suite Order](manage-tls.md#configuring-tls-cipher-suite-order).

Para obter informações sobre a ordem dos conjuntos de codificação padrão que são usados pelo Schannel SSP, consulte [pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

## <a name="ciphersuites"></a>Conjuntos de criptografia

Configurar conjuntos de criptografia TLS/SSL deve ser feito usando a política de grupo, o MDM ou o PowerShell, consulte [Configurando a ordem do conjunto de criptografia TLS](manage-tls.md#configuring-tls-cipher-suite-order) para obter detalhes.

Para obter informações sobre a ordem dos conjuntos de codificação padrão que são usados pelo Schannel SSP, consulte [pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Essa entrada controla a quantidade de tempo que o sistema operacional leva em milissegundos para finalizar entradas de cache do lado do cliente. Um valor 0 desativa o cache de conexão segura. Essa entrada não existe no Registro por padrão. 

Na primeira vez que um cliente se conecta a um servidor por meio do SSP Schannel, um handshake de TLS/SSL completo é executado. Quando isso é concluído, o segredo mestre, o conjunto de criptografias e os certificados são armazenados no cache da sessão, no respectivo cliente e servidor.

A partir do Windows Server 2008 e do Windows Vista, o tempo de cache do cliente padrão é de 10 horas.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempo de cache de cliente padrão

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

O grampeamento do protocolo OCSP permite que um servidor Web, como o Serviços de Informações da Internet (IIS), forneça o status de revogação atual de um certificado de servidor ao enviar o certificado do servidor a um cliente durante o handshake de TLS. Esse recurso reduz a carga em servidores OCSP porque o servidor Web pode armazenar em cache o status OCSP atual do certificado do servidor e enviá-lo a vários clientes Web. Sem esse recurso, cada cliente Web tentaria recuperar o status OCSP atual do certificado do servidor do servidor OCSP. Isso gerará uma carga alta nesse servidor OCSP. 

Além do IIS, os serviços da Web sobre http. sys também podem se beneficiar dessa configuração, incluindo Serviços de Federação do Active Directory (AD FS) (AD FS) e o WAP (proxy de aplicativo Web). 

Por padrão, o suporte a OCSP está habilitado para sites do IIS que têm uma associação segura (SSL/TLS) simples. No entanto, esse suporte não será habilitado por padrão se o site do IIS estiver usando um ou ambos os seguintes tipos de associações seguras (SSL/TLS):
- Exigir Indicação de Nome de Servidor
- Usar repositório de certificados centralizados

Nesse caso, a resposta de saudação do servidor durante o handshake TLS não incluirá um status grampeado do OCSP por padrão. Esse comportamento melhora o desempenho: A implementação de grampeamento do Windows OCSP é dimensionada para centenas de certificados de servidor. Como o SNI e o CCS permitem que o IIS seja dimensionado para milhares de sites que potencialmente têm milhares de certificados de servidor, a definição desse comportamento como habilitado por padrão pode causar problemas de desempenho.

Versões aplicáveis: Todas as versões que começam com o Windows Server 2012 e o Windows 8. 

Caminho do registro: [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

Adicione a seguinte chave:

"EnableOcspStaplingForSni" = DWORD: 00000001

Para desabilitar, defina o valor DWORD como 0:

"EnableOcspStaplingForSni" = DWORD: 00000000

> [!NOTE] 
> A habilitação dessa chave do registro tem um impacto potencial no desempenho.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Esta entrada controla a conformidade do padrão FIPS. O padrão é 0.

Versões aplicáveis: Todas as versões que começam com o Windows Server 2012 e o Windows 8. 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\LSA

Conjuntos de codificação FIPS do Windows Server: Consulte [pacotes de criptografia e protocolos com suporte no SSP do Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hashes

Os algoritmos de hash TLS/SSL devem ser controlados pela configuração da ordem do conjunto de codificação. Consulte [Configurando a ordem do conjunto de codificação TLS](manage-tls.md#configuring-tls-cipher-suite-order) para obter detalhes.

## <a name="issuercachesize"></a>IssuerCacheSize

Essa entrada controla o tamanho do cache emissor e é usada com o mapeamento do emissor. O SSP do Schannel tenta mapear todos os emissores na cadeia de certificados do cliente — não apenas o emissor direto do certificado do cliente. Quando os emissores não são mapeados para uma conta, que é o caso típico, o servidor pode tentar mapear o mesmo nome do emissor repetidamente, centenas de vezes por segundo. 

Para evitar isso, o servidor tem um cache negativo, portanto, se um nome de emissor não é mapeado para uma conta, ele é adicionado ao cache e o SSP Schannel não tentará mapear o nome do emissor novamente, até que a entrada de cache expira. Essa entrada de Registro especifica o tamanho do cache. Essa entrada não existe no Registro por padrão. O valor padrão é 100. 

Versões aplicáveis: Todas as versões que começam com o Windows Server 2008 e o Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Essa entrada controla a duração do intervalo de tempo limite do cache em milissegundos. O SSP do Schannel tenta mapear todos os emissores na cadeia de certificados do cliente — não apenas o emissor direto do certificado do cliente. Caso os emissores não sejam mapeados para uma conta, que é o caso típico, o servidor pode tentar mapear o mesmo nome do emissor repetidamente, centenas de vezes por segundo.

Para evitar isso, o servidor tem um cache negativo, portanto, se um nome de emissor não é mapeado para uma conta, ele é adicionado ao cache e o SSP Schannel não tentará mapear o nome do emissor novamente, até que a entrada de cache expira. Esse cache é mantido por motivos de desempenho, para que o sistema não continue tentando mapear os mesmos emissores. Essa entrada não existe no Registro por padrão. O valor padrão é 10 minutos.

Versões aplicáveis: Todas as versões que começam com o Windows Server 2008 e o Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm-tamanhos de chave RSA de cliente

Essa entrada controla os tamanhos de chave RSA do cliente. 

O uso de algoritmos de troca de chaves deve ser controlado pela configuração da ordem do conjunto de codificação.

Adicionado no Windows 10, versão 1507 e Windows Server 2016.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Para especificar um intervalo mínimo com suporte de comprimento de bit de chave RSA para o cliente TLS, crie uma entrada **ClientMinKeyBitLength** . Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, 1024 bits será o mínimo. 

Para especificar um intervalo máximo com suporte de comprimento de bit de chave RSA para o cliente TLS, crie uma entrada **ClientMaxKeyBitLength** . Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, um máximo não será imposto.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>Tamanhos de chave KeyExchangeAlgorithm-Diffie-Hellman

Essa entrada controla os tamanhos de chave Diffie-Hellman. 

O uso de algoritmos de troca de chaves deve ser controlado pela configuração da ordem do conjunto de codificação.

Adicionado no Windows 10, versão 1507 e Windows Server 2016.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Para especificar um intervalo mínimo com suporte de comprimento de bit de chave Diffie-Helman para o cliente TLS, crie uma entrada **ClientMinKeyBitLength** . Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, 1024 bits será o mínimo. 
 
Para especificar um intervalo máximo com suporte de comprimento de bit de chave Diffie-Helman para o cliente TLS, crie uma entrada **ClientMaxKeyBitLength** . Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, um máximo não será imposto. 
 
Para especificar o comprimento de bit de chave Diffie-Helman para o padrão do servidor TLS, crie uma entrada **ServerMinKeyBitLength** . Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, 2048 bits será o padrão. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Essa entrada controla o número máximo de elementos de cache. A configuração de MaximumCacheSize como 0 desabilita o cache de sessão do servidor e impede a reconexão. O aumento de MaximumCacheSize acima dos valores padrão faz com que o Lsass.exe consuma memória adicional. Cada elemento de cache de sessão geralmente requer de 2 a 4 KB de memória. Essa entrada não existe no Registro por padrão. O valor padrão é 20.000 elementos. 

Versões aplicáveis: Todas as versões que começam com o Windows Server 2008 e o Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Mensagens – análise de fragmento

________________________________________
Essa entrada controla o tamanho máximo permitido de mensagens de handshake de TLS fragmentadas que serão aceitas. Mensagens maiores que o tamanho permitido não serão aceitas e o handshake TLS falhará. Essas entradas não existem no registro por padrão. 

Quando você define o valor como 0x0, as mensagens fragmentadas não são processadas e farão com que o handshake TLS falhe. Isso torna os clientes ou servidores TLS no computador atual não compatíveis com as RFCs do TLS. 

O tamanho máximo permitido pode ser aumentado para até 2 ^ 24-1 bytes. Permitir que um cliente ou servidor Leia e armazene grandes quantidades de dados não verificados da rede não é uma boa ideia e consumirá memória adicional para cada contexto de segurança. 

Adicionado ao Windows 7 e ao Windows Server 2008 R2.
Uma atualização que habilita o Internet Explorer no Windows XP, no Windows Vista ou no Windows Server 2008 para analisar as mensagens de handshake TLS/SSL fragmentadas está disponível.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Para especificar um tamanho máximo permitido de mensagens de handshake de TLS fragmentadas que o cliente TLS aceitará, crie uma entrada **MessageLimitClient** . Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, o valor padrão será 0x8000 bytes. 

Para especificar um tamanho máximo permitido de mensagens de handshake de TLS fragmentadas que o servidor TLS aceitará quando não houver autenticação de cliente, crie uma entrada **MessageLimitServer** . Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, o valor padrão será 0x4000 bytes. 

Para especificar um tamanho máximo permitido de mensagens de handshake de TLS fragmentadas que o servidor TLS aceitará quando houver autenticação de cliente, crie uma entrada **MessageLimitServerClientAuth** . Depois de criar a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, o valor padrão será 0x8000 bytes. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Essa entrada controla o sinalizador que é usado quando a lista de emissores confiáveis é enviada. No caso de servidores que confiam centenas de autoridades de certificação para autenticação de cliente, há muitos emissores de servidor capaz de enviá-los todos para o computador cliente ao solicitar a autenticação do cliente. Nessa situação, essa chave do Registro pode ser definida e, em vez de enviar uma lista parcial, o SSP Schannel não enviar nenhuma lista para o cliente.

O não envio de uma lista de emissores confiáveis pode afetar o que o cliente envia quando lhe é solicitado um certificado de cliente. Por exemplo, quando o Internet Explorer recebe uma solicitação de autenticação de cliente, ele exibe apenas os certificados de cliente que encadeiam até uma das autoridades de certificação enviadas pelo servidor. Se o servidor não tiver enviado a uma lista, o Internet Explorer exibirá todos os certificados de cliente que estão instalados no cliente. 

Esse comportamento pode ser desejável. Por exemplo, quando os ambientes PKI incluem certificados cruzados, os certificados de cliente e servidor não terão a mesma AC raiz; Portanto, o Internet Explorer não pode escolher um certificado que se encadeia com uma das CAs do servidor. Ao configurar o servidor para não enviar uma lista de emissores confiáveis, o Internet Explorer enviará todos os seus certificados.

Essa entrada não existe no Registro por padrão.

Comportamento padrão de enviar lista de emissores confiáveis

| Versão do Windows | Time |
|-----------------|------|
| Windows Server 2012 e Windows 8 e posterior | FALSE |
| Windows Server 2008 R2 e Windows 7 e anterior | TRUE |

Versões aplicáveis: Todas as versões que começam com o Windows Server 2008 e o Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Essa entrada controla a quantidade de tempo que o sistema operacional leva em milissegundos para finalizar entradas de cache do lado do servidor. Um valor de 0 desabilita o cache de sessão do lado do servidor e impede a reconexão. O aumento de ServerCacheTime acima dos valores padrão faz com que o Lsass.exe consuma memória adicional. Cada elemento de cache de sessão geralmente requer de 2 a 4 KB de memória. Essa entrada não existe no Registro por padrão. 

Versões aplicáveis: Todas as versões que começam com o Windows Server 2008 e o Windows Vista.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempo de cache do servidor padrão: 10 horas

## <a name="ssl-20"></a>SSL 2.0

Essa subchave controla o uso do SSL 2,0.

A partir do Windows 10, versão 1607 e Windows Server 2016, o SSL 2,0 foi removido e não tem mais suporte.
Para obter as configurações padrão de SSL 2,0, consulte [protocolos no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo SSL 2,0, crie uma entrada **habilitada** na subchave cliente ou servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves SSL 2,0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do SSL 2,0 no cliente SSL. |
| Servidor | Controla o uso do SSL 2,0 no servidor SSL. |

Para desabilitar o SSL 2,0 para cliente ou servidor, altere o valor de DWORD para 0. Se um aplicativo SSPI solicitar o uso do SSL 2,0, ele será negado. 

Para desabilitar o SSL 2,0 por padrão, crie uma entrada **DisabledByDefault** e altere o valor DWORD para 1. Se um aplicativo SSPI explicitamente solicitações para usar SSL 2,0, ele poderá ser negociado. 

O exemplo a seguir mostra o SSL 2,0 desabilitado no registro:

![SSL 2,0 desabilitado](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Essa subchave controla o uso do SSL 3,0.

A partir do Windows 10, versão 1607 e Windows Server 2016, o SSL 3,0 foi desabilitado por padrão. Para configurações padrão de SSL 3,0, consulte [protocolos no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo SSL 3,0, crie uma entrada **habilitada** na subchave cliente ou servidor, conforme descrito na tabela a seguir.  
Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves SSL 3,0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do SSL 3,0 no cliente SSL. |
| Servidor | Controla o uso do SSL 3,0 no servidor SSL. |

Para desabilitar o SSL 3,0 para cliente ou servidor, altere o valor de DWORD para 0.
Se um aplicativo SSPI solicitar o uso do SSL 3,0, ele será negado. 

Para desabilitar o SSL 3,0 por padrão, crie uma entrada **DisabledByDefault** e altere o valor DWORD para 1. Se um aplicativo SSPI solicitar explicitamente o uso do SSL 3,0, ele poderá ser negociado. 

O exemplo a seguir mostra o SSL 3,0 desabilitado no registro:

![SSL 3,0 desabilitado](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Essa subchave controla o uso de TLS 1,0.

Para configurações padrão do TLS 1,0, consulte [protocolos no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo TLS 1,0, crie uma entrada **habilitada** na subchave cliente ou servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves TLS 1,0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do TLS 1,0 no cliente TLS. |
| Servidor | Controla o uso do TLS 1,0 no servidor TLS. |

Para desabilitar o TLS 1,0 para cliente ou servidor, altere o valor de DWORD para 0.
Se um aplicativo SSPI solicitar o uso do TLS 1,0, ele será negado. 

Para desabilitar o TLS 1,0 por padrão, crie uma entrada **DisabledByDefault** e altere o valor DWORD para 1. Se um aplicativo SSPI solicitar explicitamente o uso do TLS 1,0, ele poderá ser negociado. 

O exemplo a seguir mostra o TLS 1,0 desabilitado no registro:

![TLS 1,0 desabilitado](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Essa subchave controla o uso de TLS 1,1.

Para configurações padrão do TLS 1,1, consulte [protocolos no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo TLS 1,1, crie uma entrada **habilitada** na subchave cliente ou servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves TLS 1,1

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do TLS 1,1 no cliente TLS. |
| Servidor | Controla o uso do TLS 1,1 no servidor TLS. |

Para desabilitar o TLS 1,1 para cliente ou servidor, altere o valor de DWORD para 0.
Se um aplicativo SSPI solicitar o uso do TLS 1,1, ele será negado. 

Para desabilitar o TLS 1,1 por padrão, crie uma entrada **DisabledByDefault** e altere o valor DWORD para 1. Se um aplicativo SSPI solicitar explicitamente o uso do TLS 1,1, ele poderá ser negociado. 

O exemplo a seguir mostra o TLS 1,1 desabilitado no registro:

![TLS 1,1 desabilitado](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Essa subchave controla o uso de TLS 1,2.

Para configurações padrão do TLS 1,2, consulte [protocolos no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo TLS 1,2, crie uma entrada **habilitada** na subchave cliente ou servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves TLS 1,2

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do TLS 1,2 no cliente TLS. |
| Servidor | Controla o uso do TLS 1,2 no servidor TLS. |

Para desabilitar o TLS 1,2 para cliente ou servidor, altere o valor de DWORD para 0.
Se um aplicativo SSPI solicitar o uso do TLS 1,2, ele será negado. 

Para desabilitar o TLS 1,2 por padrão, crie uma entrada **DisabledByDefault** e altere o valor DWORD para 1. Se um aplicativo SSPI solicitar explicitamente o uso do TLS 1,2, ele poderá ser negociado. 

O exemplo a seguir mostra o TLS 1,2 desabilitado no registro:

![TLS 1,2 desabilitado](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Essa subchave controla o uso do DTLS 1,0.

Para configurações padrão do DTLS 1,0, consulte [protocolos no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo DTLS 1,0, crie uma entrada **habilitada** na subchave cliente ou servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves do DTLS 1,0

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do DTLS 1,0 no cliente do DTLS. |
| Servidor | Controla o uso do DTLS 1,0 no servidor DTLS. |

Para desabilitar o DTLS 1,0 para cliente ou servidor, altere o valor de DWORD para 0.
Se um aplicativo SSPI solicitar o uso do DTLS 1,0, ele será negado. 

Para desabilitar o DTLS 1,0 por padrão, crie uma entrada **DisabledByDefault** e altere o valor DWORD para 1. Se um aplicativo SSPI solicitar explicitamente o uso do DTLS 1,0, ele poderá ser negociado. 

O exemplo a seguir mostra o DTLS 1,0 desabilitado no registro:

![DTLS 1,0 desabilitado](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1,2

Essa subchave controla o uso do DTLS 1,2.

Para configurações padrão do DTLS 1,2, consulte [protocolos no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo DTLS 1,2, crie uma entrada **habilitada** na subchave cliente ou servidor, conforme descrito na tabela a seguir. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. 

Tabela de subchaves do DTLS 1,2

| Subchave | Descrição |
|--------|-------------|
| Remota | Controla o uso do DTLS 1,2 no cliente do DTLS. |
| Servidor | Controla o uso do DTLS 1,2 no servidor DTLS. |


Para desabilitar o DTLS 1,2 para cliente ou servidor, altere o valor de DWORD para 0.
Se um aplicativo SSPI solicitar o uso do DTLS 1,0, ele será negado. 

Para desabilitar o DTLS 1,2 por padrão, crie uma entrada **DisabledByDefault** e altere o valor DWORD para 1. Se um aplicativo SSPI solicitar explicitamente o uso do DTLS 1,2, ele poderá ser negociado. 

O exemplo a seguir mostra o DTLS 1,1 desabilitado no registro:

![DTLS 1,1 desabilitado](images/dtls-11-registry-setting.png)


