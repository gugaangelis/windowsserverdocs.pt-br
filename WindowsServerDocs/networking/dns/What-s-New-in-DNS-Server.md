---
title: O que há de novo no servidor DNS no Windows Server
description: Este tópico fornece uma visão geral dos novos recursos no servidor DNS no Windows Server 2016 e versões posteriores
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ddb8920c045f231dcf5286283d9895ef6ffff47
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>O que há de novo no servidor DNS no Windows Server

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico descreve a funcionalidade de servidor de sistema de nome de domínio (DNS) que é novo ou alterado no Windows Server 2016.  
  
No Windows Server 2016, servidor DNS oferece suporte aprimorado nas seguintes áreas.  
  
|Funcionalidade|Novos ou melhorados|Descrição|  
|-----------------|-------------------|---------------|  
|Políticas DNS|Novo|Você pode configurar políticas DNS para especificar como um servidor DNS responde às consultas do DNS. Respostas DNS podem ser baseadas em endereço IP do cliente (local), hora do dia e vários outros parâmetros. As políticas DNS permitem DNS reconhecimento de local, gerenciamento de tráfego, balanceamento de carga, uma DNS e outros cenários.|  
|Taxa de resposta limitar (RRL)|Novo|Você pode habilitar a limitação de taxa de resposta em seus servidores DNS. Ao fazer isso, você deve evitar a possibilidade de sistemas mal-intencionado usando seus servidores DNS para iniciar um ataque de negação de serviço em um cliente DNS.|  
|Autenticação baseada em DNS de entidades nomeadas (DANE)|Novo|Você pode usar registros TLSA (Transport Layer Security autenticação) para fornecer informações aos clientes DNS afirma que CA devem esperar um certificado de seu nome de domínio. Isso impede que ataques man-in-the-middle, em que alguém pode corromper o cache do DNS para apontar para seu próprio site e fornecer um certificado que eles emitido por uma autoridade de certificação diferente.|  
|Suporte de registro desconhecido|Novo|Você pode adicionar os registros que não são suportados explicitamente pelo servidor DNS do Windows usando a funcionalidade de registro desconhecido.|  
|Dicas de raiz IPv6|Novo|Você pode usar o IPV6 nativo dicas de raiz dão suporte para executar a resolução de nomes de internet usando os servidores de raiz IPV6.|  
|Suporte do Windows PowerShell|Aprimorado|Novos cmdlets do Windows PowerShell estão disponíveis para o servidor DNS.|  
  
## <a name="dns-policies"></a>Políticas DNS

Você pode usar a política de DNS para gerenciamento de tráfego com base em localização geográfica, inteligentes respostas DNS com base na hora do dia, para gerenciar um único servidor DNS configurado para implantação split\ cérebro, a aplicação de filtros em consultas do DNS e muito mais. Os itens a seguir fornecem mais detalhes sobre esses recursos.

-   **Balanceamento de carga do aplicativo.** Quando você implantou várias instâncias de um aplicativo em locais diferentes, você pode usar a política DNS para equilibrar a carga de tráfego entre as instâncias de aplicativo diferente, alocar dinamicamente a carga do tráfego para o aplicativo.

-   **Geo\ local com base em gerenciamento de tráfego.** Você pode usar política de DNS para permitir que os servidores DNS primário e secundário responder a consultas do cliente DNS com base na localização geográfica do cliente e o recurso ao qual o cliente está tentando se conectar, fornecendo o cliente com o endereço IP do recurso mais próximo. 

-   **Divida cérebro DNS.** Com o cérebro split\ DNS, registros DNS são divididos em escopos zona diferentes no mesmo servidor DNS e clientes DNS recebem uma resposta com base em se os clientes são clientes internos ou externos. Você pode configurar o cérebro split\ DNS para zonas integrada ao Active Directory ou para zonas em servidores DNS autônomo.

-   **Filtragem.** Você pode configurar a política DNS para criar filtros de consulta que são baseados em critérios que você fornecer. Filtros de consulta na política DNS permitem que você configure o servidor DNS para responder de forma personalizada com base na consulta de DNS e cliente DNS que envia a consulta DNS. 
-   **Perícia.** Você pode usar a política DNS para redirecionar mal-intencionado clientes DNS para um endereço IP non\ existente em vez de direcionando-os para o computador que está tentando acessar.

-   **Hora do dia com base em redirecionamento.** Você pode usar a política DNS para distribuir o tráfego do aplicativo entre diferentes instâncias geograficamente distribuídas de um aplicativo usando políticas DNS que se baseiam na hora do dia. 
  
Você também pode usar políticas DNS para Active Directory integradas DNS zonas.

Para obter mais informações, consulte o [guia cenário da política de DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>A limitação de taxa de resposta

Você pode definir as configurações de RRL para controlar como responder a solicitações para um cliente DNS quando o servidor recebe várias solicitações direcionando o mesmo cliente. Ao fazer isso, você pode impedir que alguém enviando um ataque de negação de serviço (Dos) usando os servidores DNS. Por exemplo, um bot net pode enviar solicitações para o servidor DNS usando o endereço IP de um terceiro computador como o solicitante. Sem RRL, seus servidores DNS podem responder a todas as solicitações, saturação terceiro computador. Quando você usa RRL, você pode configurar as seguintes configurações:  
  
-   **Respostas por segundo**. Isso é o número máximo de vezes que a mesma resposta será fornecida para um cliente dentro de um segundo.  
  
-   **Erros por segundo**. Isso é o número máximo de vezes que uma resposta de erro será enviada para o mesmo cliente dentro de um segundo.  
  
-   **Janela**. Este é o número de segundos para o qual serão suspensa respostas para um cliente se forem feitas muitas solicitações.  
  
-   **Taxa de vazamento**. Isso é frequência o servidor DNS responderá a uma consulta durante o tempo respostas estão suspensos. Por exemplo, se o servidor de respostas para um cliente é suspenso por 10 segundos, e a taxa de vazamento é 5, o servidor ainda responderá a uma consulta para cada 5 consultas enviados. Isso permite que os clientes legítimos obter respostas, mesmo quando o servidor DNS está aplicando a taxa de resposta limitar no FQDN ou sub-rede.  
  
-   **Taxa de TC**. Isso é usado para informar o cliente para se conectar com TCP quando respostas para o cliente estiverem suspenso. Por exemplo, se a taxa de TC é 3 e o servidor suspende respostas para um determinado cliente, o servidor emitirá uma solicitação de conexão TCP para cada 3 consultas recebidas. Verifique se que o valor de taxa de TC é menor que a taxa de vazamento, para dar o cliente a opção de conectar via TCP antes de vazamento de respostas.  
  
-   **Respostas máximas**. Isso é o número máximo de respostas de que servidor emitirá para um cliente enquanto respostas são suspenso.  
  
-   **Lista branca domínios**. Esta é uma lista de domínios serem excluídos do configurações RRL.  
  
-   **Lista branca sub-redes**. Esta é uma lista de sub-redes serem excluídos do configurações RRL.  
  
-   **Interfaces de servidor lista branca**. Esta é uma lista das interfaces de servidor DNS a ser excluído por configurações RRL.  
  
## <a name="dane-support"></a>Suporte DANE

Você pode usar o suporte a DANE \ (RFC 6394 e 6698\) para especificar a seus clientes DNS que devem esperar certificados emitidos de nomes de domínios de CA hospedado no seu servidor DNS. Isso impede que uma forma de ataque de man-in-the-middle, em que alguém é capaz de danificar a um cache DNS e aponte um nome DNS para seus próprios endereços IP.  
  
Por exemplo, imagine que você hospedar um site seguro que usa SSL em www.contoso.com usando um certificado de uma autoridade bem conhecido chamado CA1. Alguém pode ainda ser capaz de obter um certificado para www.contoso.com um diferente, não-para-conhecido, certificado de autoridade denominado CA2. Em seguida, a entidade que está hospedando o site www.contoso.com falsos poderá danificar o cache do DNS de um cliente ou servidor para apontar www.contoto.com para seus sites falsos. O usuário final será apresentado um certificado de CA2 e pode simplesmente confirmá-la e se conectar ao site falso. Com DANE, o cliente faria faça uma solicitação para o servidor DNS para contoso.com solicitando o registro TLSA e saiba que o certificado para www.contoso.com foi problemas por CA1. Se apresentados com um certificado de autoridade de certificação outra, a conexão é anulada.  
  
## <a name="unknown-record-support"></a>Suporte de registro desconhecido

Um "registro desconhecido" é um registro de recurso cujo formato RDATA não é conhecido para o servidor DNS. O suporte recém-adicionado para tipos de registros desconhecido (RFC 3597) significa que você pode adicionar os tipos de registro sem suporte para as zonas de servidor DNS do Windows em formato binário na transmissão. O windows em cache do resolvedor já tem a capacidade de processar os tipos de registros desconhecido. Servidor DNS do Windows não fará nenhum registro processamento específico para os registros desconhecidos, mas enviará-lo novamente nas respostas se consultas são recebidas para ele.  
  
## <a name="ipv6-root-hints"></a>Dicas de raiz IPv6

As dicas de raiz IPV6, conforme publicado pelo IANA, foram adicionadas para o servidor DNS do windows. As consultas de nomes de internet agora podem usar servidores de raiz IPv6 para realizar resoluções de nome.

## <a name="windows-powershell-support"></a>Suporte do Windows PowerShell

Os seguintes novos cmdlets do Windows PowerShell e os parâmetros são introduzidos no Windows Server 2016.
  
-   **Adicionar DnsServerRecursionScope**. Este cmdlet cria um novo escopo de recursão no servidor DNS. Escopos de recursão são usados pelas políticas DNS para especificar uma lista de encaminhadores para ser usado em uma consulta DNS.  
  
-   **Remover DnsServerRecursionScope**. Este cmdlet Remove os escopos de recursão existentes.  
  
-   **Conjunto DnsServerRecursionScope**. Este cmdlet altera as configurações de um escopo de recursão existente.  
  
-   **Get-DnsServerRecursionScope**. Este cmdlet recupera informações sobre escopos de recursão existente.  
  
-   **Adicionar DnsServerClientSubnet**. Este cmdlet cria uma nova sub-rede cliente DNS. Sub-redes são usados pelas políticas DNS para identificar onde se encontra um cliente DNS.  
  
-   **Remover DnsServerClientSubnet**. Este cmdlet Remove sub-redes de cliente DNS existentes.  
  
-   **Conjunto DnsServerClientSubnet**. Este cmdlet altera as configurações de uma sub-rede existente do cliente DNS.  
  
-   **Get-DnsServerClientSubnet**. Este cmdlet recupera informações sobre sub-redes de cliente DNS existentes.  
  
-   **Adicionar DnsServerQueryResolutionPolicy**. Este cmdlet cria uma nova política de resolução de consulta DNS. Políticas de resolução de consulta DNS são usadas para especificar como, ou se, uma consulta é respondida a, com base em critérios diferentes.  
  
-   **Remover DnsServerQueryResolutionPolicy**. Este cmdlet remove as políticas DNS existentes.  
  
-   **Conjunto DnsServerQueryResolutionPolicy**. Este cmdlet altera as configurações de uma política existente do DNS.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Este cmdlet recupera informações sobre políticas DNS existentes.  
  
-   **Habilitar DnsServerPolicy**. Este cmdlet permite que as políticas DNS existentes.  
  
-   **Desabilitar DnsServerPolicy**. Este cmdlet desabilita as políticas DNS existentes.  
  
-   **Adicionar DnsServerZoneTransferPolicy**. Este cmdlet cria uma nova política de transferência de zona de servidor DNS. Políticas de transferência de zona DNS especificam se deseja negue ou ignorar uma transferência de zona com base em critérios diferentes.  
  
-   **Remover DnsServerZoneTransferPolicy**. Este cmdlet Remove existentes políticas de transferência de zona para servidor DNS.  
  
-   **Conjunto DnsServerZoneTransferPolicy**. Este cmdlet altera as configurações de uma política de transferência de zona de servidor DNS existente.  
  
-   **Get-DnsServerResponseRateLimiting**. Este cmdlet recupera RRL configurações.  
  
-   **Conjunto DnsServerResponseRateLimiting**. Este cmdlet muda RRL settigns.  
  
-   **Adicionar DnsServerResponseRateLimitingExceptionlist**. Este cmdlet cria uma lista de exceções RRL no servidor DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet recupera RRL excception listas.  
  
-   **Remover DnsServerResponseRateLimitingExceptionlist**. Este cmdlet Remove uma lista de exceções RRL existente.  
  
-   **Conjunto DnsServerResponseRateLimitingExceptionlist**. Este cmdlet muda RRL listas de exceção.  
  
-   **Adicionar DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido.  
  
-   **Get-DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido.  
  
-   **Remover DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido.  
  
-   **Conjunto DnsServerResourceRecord**. Este cmdlet foi atualizado para dar suporte ao tipo de registro desconhecido

Para obter mais informações, consulte os seguintes tópicos de referência de comando do Windows Server 2016 Windows PowerShell.

- [Módulo ServidorDNS](https://technet.microsoft.com/itpro/powershell/windows/dns-server/index)
- [Módulo DnsClient](https://technet.microsoft.com/itpro/powershell/windows/dns-client/index)

## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no cliente DNS](What-s-New-in-DNS-Client.md)  
  

  

