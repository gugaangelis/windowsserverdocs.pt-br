---
title: "As configurações do registro de Layer Security (TLS) de transporte"
description: "Segurança do Windows Server"
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
ms.date: 08/07/2017
ms.openlocfilehash: 8ccfacc367a5d32438bebf22798479a07f6cbdfc
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="transport-layer-security-tls-registry-settings"></a>As configurações do registro de Layer Security (TLS) de transporte

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista

Este tópico de referência para o profissional de TI contém informações de configuração do registro com suporte para a implementação do Windows do protocolo Transport Layer Security (TLS) e o protocolo Secure Sockets Layer (SSL) por meio de Schannel Security Support Provider (SSP). As subchaves do registro e entradas abordadas na Ajuda neste tópico administrar e solucionar o Schannel SSP, especificamente os protocolos TLS e SSL. 

>[!Caution]
>Essas informações são fornecidas como uma referência para usar quando você está solução de problemas ou verificando que as configurações necessárias sejam aplicadas. Recomendamos que você não diretamente editar o registro a menos que não haja nenhuma outra alternativa.
>Modificações no registro não são validadas pelo Editor do registro ou pelo sistema operacional Windows antes de serem aplicadas. Como resultado, valores incorretos podem ser armazenados, e isso pode resultar em erros irrecuperáveis no sistema. Quando possível, em vez de edição do registro diretamente, use política de grupo ou outras ferramentas do Windows como o Console de gerenciamento Microsoft (MMC) para realizar tarefas. Se você deve editar o registro, tenha muito cuidado. 

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Essa entrada não existir no registro por padrão. O valor padrão é que todos os métodos de quatro mapeamento de certificados, listados abaixo, são compatíveis.

Quando um aplicativo de servidor requer autenticação de cliente, Schannel tenta automaticamente mapear o certificado que é fornecido pelo computador cliente para uma conta de usuário. Você pode autenticar usuários que fazem logon com um certificado cliente Criando mapeamentos, que se relacionam as informações de certificado a uma conta de usuário do Windows. Depois de criar e habilitar um mapeamento de certificados, cada vez que um cliente apresentar um certificado de cliente, seu aplicativo de servidor associa automaticamente se o usuário a conta de usuário do Windows apropriada.

Na maioria dos casos, um certificado é mapeado para uma conta de usuário em uma de duas maneiras: 

- Um único certificado é mapeado para uma conta de usuário único (mapeamento individual).
- Vários certificados são mapeados para uma conta de usuário (mapeamento muitos-para-um).

Por padrão, o provedor de Schannel usará os seguintes métodos de quatro mapeamento de certificados, listados em ordem de preferência de:

1. Mapeamento de certificados do Kerberos service-for-user (S4U)
2. Mapeamento de nome da entidade do usuário
3. Mapeamento individual (também conhecido como assunto/emissor mapeando)
4. Mapeamento de muitos-para-um

Versões aplicáveis: conforme indicado no **aplica-se a** lista que está no início deste tópico.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Codificações de

Codificações de TLS/SSL devem ser controladas pela configuração da ordem de pacote de codificação. Para obter detalhes, consulte [ordem do conjunto de codificação TLS configurando](manage-tls.md#configuring-tls-cipher-suite-order).

Para obter informações sobre a ordem padrão de pacotes de codificação que são usados pelo Schannel SSP, consulte [pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

##<a name="ciphersuites"></a>Conjuntos de codificação

Configurar os pacotes de codificação TLS/SSL deve ser feito usando a política de grupo, MDM ou o PowerShell, consulte [ordem do conjunto de codificação TLS configurando](manage-tls.md#configuring-tls-cipher-suite-order) para obter detalhes.

Para obter informações sobre a ordem padrão de pacotes de codificação que são usados pelo Schannel SSP, consulte [pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Essa entrada controla a quantidade de tempo que o sistema operacional leva em milissegundos para expirar entradas de cache do lado do cliente. Um valor de 0 desativa o cache de conexão segura. Essa entrada não existir no registro por padrão. 

Na primeira vez em que um cliente se conecta a um servidor por meio do Schannel SSP, completo handshake TLS/SSL é realizada. Quando isso for concluído, o segredo mestre, pacote de codificação e certificados são armazenados em cache da sessão no respectivo cliente e servidor.

A partir do Windows Server 2008 e Windows Vista, o tempo de cache do cliente padrão é 10 horas.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempo de cache do cliente padrão

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Essa entrada controla conformidade com FIPS Federal Information Processing (). O padrão é 0.

Versões aplicáveis: conforme indicado no **aplica-se a** lista que está no início deste tópico. 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\LSA

Pacotes de codificação do Windows Server FIPS: veja [pacotes de codificação com suporte e os protocolos no Schannel SSP](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hashes

Algoritmos de hash TLS/SSL devem ser controlados pela configuração da ordem de pacote de codificação. Consulte [ordem do conjunto de codificação TLS configurando](manage-tls.md#configuring-tls-cipher-suite-order) para obter detalhes.

## <a name="issuercachesize"></a>IssuerCacheSize

Essa entrada controla o tamanho do cache emissor e é usada com o emissor do mapeamento. O SSP Schannel tenta mapear todos os emissores na cadeia de certificados do cliente — não apenas o emissor direto do certificado do cliente. Quando os emissores não são mapeadas para uma conta, que é o caso comum, o servidor poderá tentar mapear o mesmo emissor repetidamente, nomeie centenas de vezes por segundo. 

Para evitar isso, o servidor tem um cache negativo, portanto, se um nome de emissor não é mapeado para uma conta, ele é adicionado ao cache e o Schannel SSP não tentará mapear o nome de emissor novamente até a entrada do cache expira. Essa entrada do registro Especifica o tamanho do cache. Essa entrada não existir no registro por padrão. O valor padrão é 100. 

Versões aplicáveis: conforme indicado no **aplica-se a** lista que está no início deste tópico.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Essa entrada controla o tamanho do intervalo de tempo limite de cache, em milissegundos. O SSP Schannel tenta mapear todos os emissores na cadeia de certificados do cliente — não apenas o emissor direto do certificado do cliente. No caso em que os emissores não são mapeadas para uma conta, que é o caso comum, o servidor poderá tentar mapear o mesmo emissor repetidamente, nomeie centenas de vezes por segundo.

Para evitar isso, o servidor tem um cache negativo, portanto, se um nome de emissor não é mapeado para uma conta, ele é adicionado ao cache e o Schannel SSP não tentará mapear o nome de emissor novamente até a entrada do cache expira. Esse cache é mantido por motivos de desempenho, para que o sistema não continuar tentando mapear os emissores mesmo. Essa entrada não existir no registro por padrão. O valor padrão é 10 minutos.

Versões aplicáveis: conforme indicado no **aplica-se a** lista que está no início deste tópico.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm - tamanhos de chave de cliente RSA

Essa entrada controla os tamanhos de chave de RSA de cliente. 

Uso de algoritmos de troca de chave deve ser controlado pela configuração da ordem de pacote de codificação.

Adicionado no Windows 10, versão 1507 e Windows Server 2016.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Para especificar um comprimento de bit de chave de intervalo com suporte mínimo de RSA para o cliente TLS, crie um **ClientMinKeyBitLength** entrada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, 1024 bits será o mínimo. 

Para especificar um comprimento de bit chave máxima variedade com suporte de RSA para o cliente TLS, crie um **ClientMaxKeyBitLength** entrada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, um máximo não é obrigatório.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm - tamanhos de chave Diffie-Hellman

Essa entrada controla os tamanhos de chave Diffie-Hellman. 

Uso de algoritmos de troca de chave deve ser controlado pela configuração da ordem de pacote de codificação.

Adicionado no Windows 10, versão 1507 e Windows Server 2016.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Para especificar um comprimento de bit chave mínimo com suporte intervalo de Diffie-Helman para o cliente TLS, crie um **ClientMinKeyBitLength** entrada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, 1024 bits será o mínimo. 
 
Para especificar um comprimento de bit de chave máximo com suporte intervalo de Diffie-Helman para o cliente TLS, crie um **ClientMaxKeyBitLength** entrada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, um máximo não é obrigatório. 
 
Para especificar o comprimento de bit de chave Diffie-Helman para o padrão de servidor TLS, crie um **ServerMinKeyBitLength** entrada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, 2048 bits será o padrão. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Essa entrada controla o número máximo de elementos de cache. Definindo MaximumCacheSize como 0 desabilita o cache de sessão de servidor e impede que reconexão. Aumentar MaximumCacheSize acima os valores padrão faz com que o Lsass.exe consumir memória adicional. Cada elemento de cache de sessão normalmente requer 2 a 4 KB de memória. Essa entrada não existir no registro por padrão. O valor padrão é 20.000 elementos. 

Versões aplicáveis: conforme indicado no **aplica-se a** lista que está no início deste tópico.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Mensagens – análise de fragmento

________________________________________
Essa entrada controla o tamanho máximo permitido de mensagens de handshake TLS fragmentados que serão aceitos. Mensagens maiores do que o tamanho permitido não serão aceitos e do handshake TLS falhará. Essas entradas não existir no registro por padrão. 

Quando você define o valor como 0x0, fragmentadas mensagens não são processadas e fará com que o handshake TLS falhar. Isso torna TLS clientes ou servidores no computador atual não compatível com as RFCs TLS. 

O número máximo permitido de tamanho pode ser aumentado até 2 ^ 24-1 bytes. Permitindo que um cliente ou servidor ler e armazenar grandes quantidades de dados não verificados da rede não é uma boa ideia e consumirá memória adicional para cada contexto de segurança. 

Adicionado no Windows 7 e Windows Server 2008 R2.
Há uma atualização que permite que o Internet Explorer no Windows XP, Windows Vista ou Windows Server 2008 para analisar fragmentadas mensagens de handshake TLS/SSL.

Caminho do registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Para especificar um tamanho máximo permitido de mensagens de handshake TLS fragmentados que o cliente TLS aceitará, crie um **MessageLimitClient** entrada. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, o valor padrão será 0x8000 bytes. 

Para especificar um tamanho máximo permitido de mensagens de handshake TLS fragmentados que o servidor TLS aceitará quando não há nenhuma autenticação de cliente, crie um **MessageLimitServer** entrada. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, o valor padrão será 0x4000 bytes. 

Para especificar um tamanho máximo permitido de mensagens de handshake TLS fragmentados que o servidor TLS aceitará quando há autenticação de cliente, crie um **MessageLimitServerClientAuth** entrada. Depois que tiver criado a entrada, altere o valor DWORD para o comprimento de bit desejado. Se não estiver configurado, o valor padrão será 0x8000 bytes. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Essa entrada controla o sinalizador é usado quando a lista de emissores confiáveis é enviada. No caso de servidores em que confia centenas de autoridades de certificação para autenticação do cliente, há muitos emissores para o servidor para poder enviá-los todos para o computador cliente ao solicitar a autenticação de cliente. Nessa situação, essa chave do registro pode ser definida e, em vez de enviar uma lista parcial, o Schannel SSP não enviará qualquer lista ao cliente.

Não enviando uma lista de emissores confiáveis pode afetar o que o cliente envia quando é solicitado para um certificado cliente. Por exemplo, quando o Internet Explorer recebe uma solicitação de autenticação de cliente, ele só exibe os certificados de cliente que encadear até uma das autoridades de certificação que é enviada pelo servidor. Se o servidor não enviou uma lista, o Internet Explorer exibe todos os certificados de cliente que estão instalados no cliente. 

Esse comportamento pode ser desejável. Por exemplo, quando os ambientes de PKI incluem certificados cruzados, os certificados de cliente e servidor não terá a mesma raiz da autoridade de certificação; Portanto, Internet Explorer não pode escolher um certificado que até uma das CAs do servidor é aprovado. Ao configurar o servidor para não enviar uma lista de emissor confiável, o Internet Explorer enviará todos os seus certificados.

Essa entrada não existir no registro por padrão.

Comportamento padrão de enviar a lista de emissor confiáveis

| Versão do Windows | Tempo |
|-----------------|------|
| Windows Server 2012 e Windows 8 e posterior | FALSE |
| Windows Server 2008 R2 e Windows 7 e anterior | TRUE |

Versões aplicáveis: conforme indicado no **aplica-se a** lista que está no início deste tópico.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Essa entrada controla a quantidade de tempo em milissegundos que o sistema operacional leva para expirar entradas de cache do lado do servidor. O valor de 0 desabilita o cache de sessão de servidor e impede que reconexão. Aumentar ServerCacheTime acima os valores padrão faz com que o Lsass.exe consumir memória adicional. Cada elemento de cache de sessão normalmente requer 2 a 4 KB de memória. Essa entrada não existir no registro por padrão. 

Versões aplicáveis: conforme indicado no **aplica-se a** lista que está no início deste tópico.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempo de cache do servidor padrão: 10 horas

## <a name="ssl-20"></a>SSL 2.0

Essa subchave controla o uso de SSL 2.0.

A partir do Windows 10, versão 1607 e Windows Server 2016, o SSL 2.0 foi removido e não é mais compatível.
Para uma configurações padrão de SSL 2.0, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo SSL 2.0, crie um **Enabled** entrada na subchave apropriada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 1. Para desabilitar o protocolo, altere o valor DWORD como 0.

Tabela subchave SSL 2.0

| Subchave | Descrição |
|--------|-------------|
| Cliente | Controla o uso de SSL 2.0 no cliente SSL. |
| Servidor | Controla o uso de SSL 2.0 no servidor SSL. |
| DisabledByDefault | Sinalizador para desabilitar o SSL 2.0 por padrão. |

## <a name="ssl-30"></a>SSL 3.0

Essa subchave controla o uso de SSL 3.0.

A partir do Windows 10, versão 1607 e Windows Server 2016, SSL 3.0 foi desabilitado por padrão. Para as configurações padrão de SSL 3.0, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar o protocolo SSL 3.0, crie uma entrada habilitada na subchave apropriada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 1. Para desabilitar o protocolo, altere o valor DWORD como 0.

Tabela subchave SSL 3.0

| Subchave | Descrição |
|--------|-------------|
| Cliente | Controla o uso de SSL 3.0 no cliente SSL. |
| Servidor | Controla o uso de SSL 3.0 no servidor SSL. |
| DisabledByDefault | Sinalizador para desabilitar SSL 3.0 por padrão. |

## <a name="tls-10"></a>TLS 1.0

Essa subchave controla o uso de TLS 1.0.

Para as configurações padrão de TLS 1.0, veja veja [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para desabilitar o protocolo TLS 1.0, crie um **Enabled** entrada na subchave apropriada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 0. Para habilitar o protocolo, altere o valor DWORD como 1.

Tabela subchave TLS 1.0

| Subchave | Descrição |
|--------|-------------|
| Cliente | Controla o uso de TLS 1.0 no cliente TLS. |
| Servidor | Controla o uso de TLS 1.0 no servidor TLS. |
| DisabledByDefault | Sinalizador para desabilitar TLS 1.0 por padrão. |

## <a name="tls-11"></a>TLS 1.1

Essa subchave controla o uso do protocolo TLS 1.1.

Para as configurações padrão de protocolo TLS 1.1, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>Para TLS 1.1 ser habilitado e negociada em servidores que executam o Windows Server 2008 R2, você deve criar o **DisabledByDefault** entrada na subchave apropriada (cliente, servidor) e defina-o como "0". A entrada não será vista no registro e está definido como "1" por padrão.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para desabilitar o protocolo TLS 1.1, crie um **Enabled** entrada na subchave apropriada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 0. Para habilitar o protocolo, altere o valor DWORD como 1.

Tabela subchave TLS 1.1

| Subchave | Descrição |
|--------|-------------|
| Cliente | Controla o uso do protocolo TLS 1.1 no cliente TLS. |
| Servidor | Controla o uso do protocolo TLS 1.1 no servidor TLS. |
| DisabledByDefault | Sinalizador para desabilitar o protocolo TLS 1.1 por padrão. |

## <a name="tls-12"></a>TLS 1.2

Essa subchave controla o uso de TLS 1.2.

Para as configurações padrão de TLS 1.2, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>Para o TLS 1.2 seja habilitada e negociada em servidores que executam o Windows Server 2008 R2, você deve criar o **DisabledByDefault** entrada na subchave apropriada (cliente, servidor) e defina-o como "0". A entrada não será vista no registro e está definido como "1" por padrão.

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para desabilitar o protocolo TLS 1.2, crie um **Enabled** entrada na subchave apropriada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 0. Para habilitar o protocolo, altere o valor DWORD como 1.

Tabela subchave TLS 1.2

| Subchave | Descrição |
|--------|-------------|
| Cliente | Controla o uso de TLS 1.2 no cliente TLS. |
| Servidor | Controla o uso de TLS 1.2 no servidor TLS. |
| DisabledByDefault | Sinalizador para desabilitar TLS 1.2 por padrão. |

## <a name="dtls-10"></a>DTLS 1.0

Essa subchave controla o uso de DTLS 1.0.

Para as configurações padrão de DTLS 1.0, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para desabilitar o protocolo DTLS 1.0, crie um **Enabled** entrada na subchave apropriada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 0. Para habilitar o protocolo, altere o valor DWORD como 1.

Tabela subchave DTLS 1.0

| Subchave | Descrição |
|--------|-------------|
| Cliente | Controla o uso de DTLS 1.0 no cliente DTLS. |
| Servidor | Controla o uso de DTLS 1.0 no servidor DTLS. |
| DisabledByDefault | Sinalizador para desabilitar DTLS 1.0 por padrão. |

## <a name="dtls-12"></a>DTLS 1.2

Essa subchave controla o uso de DTLS 1.2.

Para as configurações padrão de DTLS 1.2, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Caminho do registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para desabilitar o protocolo DTLS 1.2, crie um **Enabled** entrada na subchave apropriada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 0. Para habilitar o protocolo, altere o valor DWORD como 1.

Tabela subchave DTLS 1.2

| Subchave | Descrição |
|--------|-------------|
| Cliente | Controla o uso de DTLS 1.2 no cliente DTLS. |
| Servidor | Controla o uso de DTLS 1.2 no servidor DTLS. |
| DisabledByDefault | Sinalizador para desabilitar DTLS 1.2 por padrão. |

