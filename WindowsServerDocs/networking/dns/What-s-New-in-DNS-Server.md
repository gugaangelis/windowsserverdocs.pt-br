---
title: O que há de novo no servidor DNS no Windows Server
description: Este tópico fornece uma visão geral dos novos recursos no servidor DNS no Windows Server 2016 e versões posteriores
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 26d9a532f8c2276a81e8718e76290d41c78f6633
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317971"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>O que há de novo no servidor DNS no Windows Server

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade de servidor DNS (sistema de nomes de domínio) que é nova ou alterada no Windows Server 2016.  
  
No Windows Server 2016, o servidor DNS oferece suporte aprimorado nas seguintes áreas.  
  
|Funcionalidade|Novo ou melhorado|Descrição|  
|-----------------|-------------------|---------------|  
|Políticas de DNS|Novo|Você pode configurar as políticas de DNS para especificar como um servidor DNS responde a consultas DNS. As respostas DNS podem ser baseadas no endereço IP do cliente (local), na hora do dia e em vários outros parâmetros. As políticas de DNS habilitam o DNS com reconhecimento de local, o gerenciamento de tráfego, o balanceamento de carga, o DNS de divisão e outros cenários.|  
|Limitação de taxa de resposta (RRL)|Novo|Você pode habilitar a limitação da taxa de resposta em seus servidores DNS. Ao fazer isso, você evita a possibilidade de sistemas mal-intencionados que usam seus servidores DNS para iniciar um ataque de negação de serviço em um cliente DNS.|  
|Autenticação baseada em DNS de entidades nomeadas (SUNDANÊS)|Novo|Você pode usar os registros TLSA (autenticação de segurança de camada de transporte) para fornecer informações aos clientes DNS que afirmam a qual AC eles devem esperar um certificado para seu nome de domínio. Isso impede ataques man-in-the-Middle, em que alguém pode corromper o cache DNS para apontar para seu próprio site e fornecer um certificado emitido por uma autoridade de certificação diferente.|  
|Suporte a registro desconhecido|Novo|Você pode adicionar registros que não são explicitamente suportados pelo servidor DNS do Windows usando a funcionalidade de registro desconhecida.|  
|Dicas de raiz IPv6|Novo|Você pode usar o suporte de dicas de raiz IPV6 nativo para executar a resolução de nomes da Internet usando os servidores raiz IPV6.|  
|Suporte do Windows PowerShell|Melhorado|Novos cmdlets do Windows PowerShell estão disponíveis para o servidor DNS.|  
  
## <a name="dns-policies"></a>Políticas de DNS

Você pode usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica, respostas de DNS inteligente com base na hora do dia, para gerenciar um único servidor DNS configurado para a implantação de divisão\-Brain, aplicando filtros em consultas DNS e muito mais. Os itens a seguir fornecem mais detalhes sobre esses recursos.

-   **Balanceamento de carga do aplicativo.** Quando você implantou várias instâncias de um aplicativo em locais diferentes, você pode usar a política DNS para balancear a carga de tráfego entre as diferentes instâncias de aplicativo, alocando dinamicamente a carga de tráfego para o aplicativo.

-   **Gerenciamento de tráfego baseado na localização de\-geográfica.** Você pode usar a política DNS para permitir que servidores DNS primários e secundários respondam a consultas de cliente DNS com base na localização geográfica do cliente e do recurso ao qual o cliente está tentando se conectar, fornecendo ao cliente o endereço IP do mais próximo Kit. 

-   **DNS de divisão de cérebro.** Com a divisão\-o DNS Brain, os registros DNS são divididos em escopos de zona diferentes no mesmo servidor DNS, e os clientes DNS recebem uma resposta com base no fato de os clientes serem clientes internos ou externos. Você pode configurar o DNS dividido\-cérebro para Active Directory zonas integradas ou para zonas em servidores DNS autônomos.

-   **Aplica.** Você pode configurar a política DNS para criar filtros de consulta baseados em critérios fornecidos por você. Os filtros de consulta na política DNS permitem configurar o servidor DNS para responder de uma maneira personalizada com base na consulta DNS e no cliente DNS que envia a consulta DNS. 
-   **Análise forense.** Você pode usar a política DNS para redirecionar clientes DNS mal-intencionados para um endereço IP não\-existente em vez de direcioná-los para o computador que estão tentando acessar.

-   **Hora do redirecionamento com base no dia.** Você pode usar a política DNS para distribuir o tráfego de aplicativos em diferentes instâncias distribuídas geograficamente de um aplicativo usando políticas de DNS com base na hora do dia. 
  
Você também pode usar políticas de DNS para Active Directory zonas DNS integradas.

Para obter mais informações, consulte o [Guia de cenários de política de DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitação da taxa de resposta

Você pode definir configurações de RRL para controlar como responder a solicitações para um cliente DNS quando o servidor recebe várias solicitações direcionadas ao mesmo cliente. Ao fazer isso, você pode impedir que alguém envie um ataque de negação de serviço (dos) usando seus servidores DNS. Por exemplo, uma rede de bot pode enviar solicitações para o servidor DNS usando o endereço IP de um terceiro computador como o solicitante. Sem o RRL, os servidores DNS podem responder a todas as solicitações, inundando o terceiro computador. Ao usar o RRL, você pode definir as seguintes configurações:  
  
-   **Respostas por segundo**. Este é o número máximo de vezes que a mesma resposta será dada a um cliente dentro de um segundo.  
  
-   **Erros por segundo**. Este é o número máximo de vezes que uma resposta de erro será enviada para o mesmo cliente dentro de um segundo.  
  
-   **Janela**. Este é o número de segundos para o qual as respostas a um cliente serão suspensas se muitas solicitações forem feitas.  
  
-   **Taxa de perda**. Essa é a frequência com que o servidor DNS responderá a uma consulta durante as respostas de tempo que são suspensas. Por exemplo, se o servidor suspender as respostas a um cliente por 10 segundos e a taxa de vazamento for 5, o servidor ainda responderá a uma consulta para cada 5 consultas enviadas. Isso permite que os clientes legítimos obtenham respostas mesmo quando o servidor DNS estiver aplicando a limitação de taxa de resposta em sua sub-rede ou FQDN.  
  
-   **Taxa de TC**. Isso é usado para instruir o cliente a tentar se conectar com TCP quando as respostas para o cliente são suspensas. Por exemplo, se a taxa de TC for 3 e o servidor suspender as respostas para um determinado cliente, o servidor emitirá uma solicitação de conexão TCP para cada 3 consultas recebidas. Verifique se o valor da taxa de TC é menor que a taxa de vazamento, para dar ao cliente a opção de se conectar via TCP antes de vazar as respostas.  
  
-   **Máximo de respostas**. Esse é o número máximo de respostas que o servidor emitirá para um cliente enquanto as respostas são suspensas.  
  
-   **Domínios de lista branca**. Esta é uma lista de domínios a serem excluídos das configurações de RRL.  
  
-   **Sub-redes de lista branca**. Esta é uma lista de sub-redes a serem excluídas das configurações de RRL.  
  
-   **Interfaces de servidor de lista**de permissões. Esta é uma lista de interfaces do servidor DNS a ser excluída das configurações de RRL.  
  
## <a name="dane-support"></a>Suporte ao SUNDANÊS

Você pode usar o suporte a SUNDANÊS \(RFC 6394 e 6698\) para especificar para seus clientes DNS qual AC eles devem esperar que os certificados sejam emitidos para os nomes de domínios hospedados no seu servidor DNS. Isso impede uma forma de ataque man-in-the-Middle, em que alguém é capaz de corromper um cache DNS e apontar um nome DNS para seu próprio endereço IP.  
  
Por exemplo, imagine que você hospede um site seguro que usa SSL em www.contoso.com usando um certificado de uma autoridade bem conhecida chamada CA1. Alguém ainda poderá obter um certificado para www.contoso.com de uma autoridade de certificação diferente, e não tão conhecida, denominada CA2. Em seguida, a entidade que hospeda o site da www.contoso.com falsa pode ser capaz de corromper o cache DNS de um cliente ou servidor para apontar www.contoto.com para seu site falso. O usuário final receberá um certificado de CA2 e poderá simplesmente refirmá-lo e conectar-se ao site falso. Com o SUNDANÊS, o cliente fará uma solicitação ao servidor DNS para contoso.com solicitando o registro TLSA e aprenderia que o certificado para www.contoso.com foi emitido por CA1. Se for apresentado um certificado de outra CA, a conexão será anulada.  
  
## <a name="unknown-record-support"></a>Suporte a registro desconhecido

Um "registro desconhecido" é um RR cujo formato RDATA não é conhecido pelo servidor DNS. O suporte recém-adicionado para tipos de registro desconhecido (RFC 3597) significa que você pode adicionar os tipos de registro sem suporte nas zonas de servidor DNS do Windows no formato binário durante a transmissão. O resolvedor de cache do Windows já tem a capacidade de processar tipos de registros desconhecidos. O servidor DNS do Windows não fará nenhum processamento específico de registro para os registros desconhecidos, mas o enviará de volta em respostas se as consultas forem recebidas para ele.  
  
## <a name="ipv6-root-hints"></a>Dicas de raiz IPv6

As dicas de raiz IPV6, conforme publicado pela IANA, foram adicionadas ao servidor DNS do Windows. As consultas de nome de Internet agora podem usar servidores raiz IPv6 para executar resoluções de nomes.

## <a name="windows-powershell-support"></a>Suporte ao Windows PowerShell

Os novos cmdlets e parâmetros do Windows PowerShell a seguir são introduzidos no Windows Server 2016.
  
-   **Add-DnsServerRecursionScope**. Esse cmdlet cria um novo escopo de recursão no servidor DNS. Os escopos de recursão são usados pelas políticas de DNS para especificar uma lista de encaminhadores a serem usados em uma consulta DNS.  
  
-   **Remove-DnsServerRecursionScope**. Esse cmdlet Remove escopos de recursão existentes.  
  
-   **Set-DnsServerRecursionScope**. Esse cmdlet altera as configurações de um escopo de recursão existente.  
  
-   **Get-DnsServerRecursionScope**. Esse cmdlet recupera informações sobre escopos de recursão existentes.  
  
-   **Add-DnsServerClientSubnet**. Esse cmdlet cria uma nova sub-rede de cliente DNS. As sub-redes são usadas pelas políticas de DNS para identificar onde um cliente DNS está localizado.  
  
-   **Remove-DnsServerClientSubnet**. Esse cmdlet Remove as sub-redes de cliente DNS existentes.  
  
-   **Set-DnsServerClientSubnet**. Esse cmdlet altera as configurações de uma sub-rede de cliente DNS existente.  
  
-   **Get-DnsServerClientSubnet**. Esse cmdlet recupera informações sobre sub-redes de cliente DNS existentes.  
  
-   **Add-DnsServerQueryResolutionPolicy**. Esse cmdlet cria uma nova política de resolução de consulta DNS. As políticas de resolução de consulta DNS são usadas para especificar como, ou se, uma consulta é respondida, com base em critérios diferentes.  
  
-   **Remove-DnsServerQueryResolutionPolicy**. Esse cmdlet Remove as políticas DNS existentes.  
  
-   **Set-DnsServerQueryResolutionPolicy**. Esse cmdlet altera as configurações de uma política DNS existente.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Esse cmdlet recupera informações sobre as políticas de DNS existentes.  
  
-   **Enable-DnsServerPolicy**. Esse cmdlet habilita as políticas DNS existentes.  
  
-   **Disable-DnsServerPolicy**. Esse cmdlet desabilita as políticas de DNS existentes.  
  
-   **Add-DnsServerZoneTransferPolicy**. Esse cmdlet cria uma nova política de transferência de zona do servidor DNS. Políticas de transferência de zona DNS especifique se deseja negar ou ignorar uma transferência de zona com base em critérios diferentes.  
  
-   **Remove-DnsServerZoneTransferPolicy**. Este cmdlet Remove as políticas de transferência de zona do servidor DNS existentes.  
  
-   **Set-DnsServerZoneTransferPolicy**. Este cmdlet altera as configurações de uma política de transferência de zona de servidor DNS existente.  
  
-   **Get-DnsServerResponseRateLimiting**. Esse cmdlet recupera as configurações de RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Esse cmdlet altera a configurações de RRL.  
  
-   **Add-DnsServerResponseRateLimitingExceptionlist**. Esse cmdlet cria uma lista de exceções de RRL no servidor DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Esse cmdlet recupera as listas de excception de RRL.  
  
-   **Remove-DnsServerResponseRateLimitingExceptionlist**. Esse cmdlet Remove uma lista de exceções de RRL existente.  
  
-   **Set-DnsServerResponseRateLimitingExceptionlist**. Esse cmdlet altera as listas de exceções de RRL.  
  
-   **Add-DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido.  
  
-   **Get-DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido.  
  
-   **Remove-DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido.  
  
-   **Set-DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido

Para obter mais informações, consulte os seguintes tópicos de referência de comando do Windows Server 2016 do Windows PowerShell.

- [Módulo DnsServer](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Módulo DnsClient](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no cliente DNS](What-s-New-in-DNS-Client.md)  
  

  

