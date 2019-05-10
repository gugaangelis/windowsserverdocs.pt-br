---
title: O que há de novo no servidor DNS no Windows Server
description: Este tópico fornece uma visão geral dos novos recursos do servidor DNS no Windows Server 2016 e versões posteriores
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 665e411eda834a59c6dbe3581611b9b58bd006f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833567"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>O que há de novo no servidor DNS no Windows Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade do servidor de sistema de nome de domínio (DNS) é nova ou alterada no Windows Server 2016.  
  
No Windows Server 2016, o servidor DNS oferece suporte aprimorado nas seguintes áreas.  
  
|Funcionalidade|Novo ou melhorado|Descrição|  
|-----------------|-------------------|---------------|  
|Políticas de DNS|Novo|Você pode configurar políticas DNS para especificar como um servidor DNS responde a consultas DNS. As respostas DNS podem ser baseadas em endereço IP do cliente (local), hora do dia e vários outros parâmetros. Políticas de DNS permitem DNS reconhecimento de local, o gerenciamento de tráfego, balanceamento de carga, o DNS com partição de rede e outros cenários.|  
|Taxa de resposta de limitação (RRL)|Novo|Você pode habilitar a limitação de taxa de resposta nos servidores DNS. Ao fazer isso, você deve evitar a possibilidade de sistemas mal-intencionado usando seus servidores DNS para iniciar um ataque de negação de serviço em um cliente DNS.|  
|Autenticação baseada em DNS de entidades nomeadas (painel)|Novo|Você pode usar registros TLSA (autenticação de segurança de camada de transporte) para fornecer informações para os clientes DNS que o que eles devem esperar um certificado de nome de domínio da autoridade de certificação de estado. Isso impede ataques man-in-the-middle, onde alguém pode corromper o cache do DNS para apontar para seu próprio site e fornecer um certificado emitidos por eles de uma autoridade de certificação diferente.|  
|Suporte de registro desconhecido|Novo|Você pode adicionar os registros que não são explicitamente suportados pelo servidor DNS do Windows usando a funcionalidade de registro desconhecido.|  
|Dicas de raízes de IPv6|Novo|Você pode usar o IPV6 nativo, suporte a dicas de raiz para executar a resolução de nome de internet usando os servidores raiz de IPV6.|  
|Suporte do Windows PowerShell|Melhorado|Novos cmdlets do Windows PowerShell estão disponíveis para o servidor DNS.|  
  
## <a name="dns-policies"></a>Políticas de DNS

Você pode usar a política de DNS para o gerenciamento de tráfego, com base na hora do dia, para gerenciar um único servidor DNS configurado para dividir as respostas DNS inteligentes com base de localização geográfica\-implantação cérebro, aplicação de filtros em consultas DNS e muito mais. Os itens a seguir fornecem mais detalhes sobre esses recursos.

-   **Balanceamento de carga do aplicativo.** Quando você tiver implantado várias instâncias de um aplicativo em locais diferentes, você pode usar a política de DNS para balancear a carga de tráfego entre as instâncias de aplicativo diferente, alocar dinamicamente a carga de tráfego para o aplicativo.

-   **Geográfica\-o gerenciamento de tráfego baseados na localização.** Você pode usar a política de DNS para permitir que os servidores DNS primário e secundário responder a consultas de cliente DNS baseadas na localização geográfica do cliente e o recurso ao qual o cliente está tentando se conectar, fornecendo o cliente com o endereço IP do mais próximo recurso. 

-   **Divida o cérebro DNS.** Com divisão\-cérebro DNS, registros de DNS são divididos em escopos de zona diferente no mesmo servidor DNS e clientes DNS recebem uma resposta com base em se os clientes são clientes internos ou externos. Você pode configurar a divisão\-cérebro DNS para zonas integrada ao Active Directory ou para as zonas em servidores DNS autônomos.

-   **A filtragem.** Você pode configurar a política de DNS para criar filtros de consulta que são baseados em critérios fornecidos por você. Filtros de consulta na política de DNS permitem que você configure o servidor DNS para responder de maneira personalizada com base na consulta DNS e do cliente DNS que envia a consulta DNS. 
-   **Análise forense.** Você pode usar a política de DNS para redirecionar os clientes mal-intencionados de DNS para um não\-endereço IP existente em vez de direcioná-las para o computador que estão tentando alcançar.

-   **Hora do dia com base em redirecionamento.** Você pode usar a política de DNS para distribuir o tráfego de aplicativo entre diferentes instâncias distribuídas geograficamente de um aplicativo por meio de políticas DNS com base na hora do dia. 
  
Você também pode usar políticas DNS para o DNS integrado ao Active Directory zonas.

Para obter mais informações, consulte o [guia de cenário de política de DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitação de taxa de resposta

Você pode configurar as configurações de RRL para controlar como responder a solicitações para um cliente DNS quando o servidor recebe várias solicitações de direcionamento do mesmo cliente. Ao fazer isso, você pode impedir que alguém envie um ataque dos (negação de serviço) usando seus servidores DNS. Por exemplo, um bot net pode enviar solicitações para seu servidor DNS usando o endereço IP de um terceiro computador como o solicitante. Sem RRL, seus servidores DNS podem responder a todas as solicitações, inundação o terceiro computador. Quando você usa RRL, você pode configurar as seguintes configurações:  
  
-   **Respostas por segundo**. Isso é o número máximo de vezes que a mesma resposta será dado a um cliente dentro de um segundo.  
  
-   **Erros por segundo**. Isso é o número máximo de vezes que uma resposta de erro será enviada ao cliente mesmo dentro de um segundo.  
  
-   **Janela**. Isso é o número de segundos para o qual as respostas a um cliente serão suspenso se muitas solicitações são feitas.  
  
-   **Taxa de vazamento**. Essa é a frequência com que o servidor DNS responderá a uma consulta durante o tempo que as respostas são suspensos. Por exemplo, se o servidor suspende as respostas a um cliente por 10 segundos, e a taxa de vazamento for 5, o servidor ainda responderá a uma consulta para cada 5 consultas enviadas. Isso permite que os clientes legítimos para obter respostas, mesmo quando o servidor DNS está sendo aplicada na sua sub-rede ou o FQDN de limitação de taxa de resposta.  
  
-   **Taxa de TC**. Isso é usado para informar o cliente tentar se conectar com o TCP quando respostas para o cliente são suspensos. Por exemplo, se a taxa de TC é 3 e o servidor suspende as respostas para um determinado cliente, o servidor emite uma solicitação de conexão TCP para cada 3 consultas recebidas. Verifique se que o valor de taxa de TC é menor do que a taxa de vazamento, dar a opção para se conectar via TCP antes de vazamento de respostas de cliente.  
  
-   **Respostas máximo**. Isso é o número máximo de respostas, que o servidor emitirá a um cliente, enquanto as respostas são suspensos.  
  
-   **Domínios de permissões**. Esta é uma lista de domínios a serem excluídos das configurações de RRL.  
  
-   **Lista de permissões de sub-redes**. Esta é uma lista de sub-redes a serem excluídos das configurações de RRL.  
  
-   **Interfaces de servidor da lista de permissões**. Esta é uma lista de interfaces de servidor DNS a serem excluídos das configurações de RRL.  
  
## <a name="dane-support"></a>Suporte de painel

Você pode usar o suporte do painel \(RFC 6394 e 6698\) especificar para os clientes DNS que eles devem esperar que certificados sejam emitidos de para nomes de domínios da autoridade de certificação hospedado no seu servidor DNS. Isso evita uma forma de ataque man-in-the-middle onde alguém é capaz de corromper um cache DNS e um nome DNS para seus próprios endereços IP.  
  
Por exemplo, imagine que você hospedar um site seguro que usa o SSL em www.contoso.com, usando um certificado de uma autoridade bem conhecido chamado CA1. Alguém ainda poderá obter um certificado para www.contoso.com de um diferente, não-para-conhecido, certificado de autoridade chamada CA2. Em seguida, a entidade que hospeda o site falso www.contoso.com poderá corromper o cache DNS de um cliente ou servidor para apontar www.contoto.com para seu site falso. O usuário final verá um certificado de CA2 e pode simplesmente confirmá-la e conectar-se ao site falso. Com o painel, o cliente seria fazer uma solicitação para o servidor DNS de contoso.com pedindo para o registro TLSA e saber se o certificado para www.contoso.com era problemas por CA1. Se obtiver um certificado de autoridade de certificação de outra, a conexão será anulada.  
  
## <a name="unknown-record-support"></a>Suporte de registro desconhecido

Um "registro desconhecido" é um registro de recurso cujo formato RDATA não é conhecido para o servidor DNS. O suporte adicionado recentemente para tipos de registro desconhecido (RFC 3597) significa que você pode adicionar os tipos de registro sem suporte para as zonas de servidor DNS do Windows no formato binário durante a transmissão. As janelas de resolvedor de cache já tem a capacidade de processar tipos de registro desconhecido. Servidor DNS do Windows não fará qualquer processamento específico registro para os registros de desconhecido, mas será enviá-lo novamente nas respostas se a consulta for recebida para ele.  
  
## <a name="ipv6-root-hints"></a>Dicas de raízes de IPv6

As dicas de raiz de IPV6, conforme publicado pelo IANA, foram adicionadas para o servidor DNS do windows. As consultas de nome de internet agora podem usar servidores raiz de IPv6 para a execução de resoluções de nome.

## <a name="windows-powershell-support"></a>Suporte ao Windows PowerShell

Os seguintes novos cmdlets do Windows PowerShell e os parâmetros são introduzidos no Windows Server 2016.
  
-   **Add-DnsServerRecursionScope**. Esse cmdlet cria um novo escopo de recursão no servidor DNS. Escopos de recursão são usados pelas políticas DNS para especificar uma lista de encaminhadores a ser usado em uma consulta DNS.  
  
-   **Remove-DnsServerRecursionScope**. Esse cmdlet Remove os escopos de recursão existentes.  
  
-   **Set-DnsServerRecursionScope**. Esse cmdlet altera as configurações de um escopo de recursão existente.  
  
-   **Get-DnsServerRecursionScope**. Este cmdlet recupera informações sobre escopos de recursão existente.  
  
-   **Add-DnsServerClientSubnet**. Esse cmdlet cria uma nova sub-rede do cliente DNS. Subredes são usados pelas políticas DNS para identificar onde um cliente DNS está localizado.  
  
-   **Remove-DnsServerClientSubnet**. Esse cmdlet Remove sub-redes existentes de cliente DNS.  
  
-   **Set-DnsServerClientSubnet**. Esse cmdlet altera as configurações de uma sub-rede de cliente DNS existente.  
  
-   **Get-DnsServerClientSubnet**. Este cmdlet recupera informações sobre sub-redes existentes de cliente DNS.  
  
-   **Add-DnsServerQueryResolutionPolicy**. Esse cmdlet cria uma nova política de resolução de consulta DNS. Políticas de resolução de consulta DNS são usadas para especificar como, ou se, uma consulta está respondida, com base em critérios diferentes.  
  
-   **Remove-DnsServerQueryResolutionPolicy**. Este cmdlet removerá as políticas DNS existentes.  
  
-   **Set-DnsServerQueryResolutionPolicy**. Esse cmdlet altera as configurações de uma política DNS existente.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Este cmdlet recupera informações sobre as políticas DNS existentes.  
  
-   **Enable-DnsServerPolicy**. Esse cmdlet permite que as políticas DNS existentes.  
  
-   **Disable-DnsServerPolicy**. Esse cmdlet desativa as políticas DNS existentes.  
  
-   **Add-DnsServerZoneTransferPolicy**. Esse cmdlet cria uma nova política de transferência de zona de servidor DNS. Políticas de transferência de zona DNS especificam se deseja negar ou ignorar uma transferência de zona com base em critérios diferentes.  
  
-   **Remove-DnsServerZoneTransferPolicy**. Esse cmdlet Remove existentes políticas de transferência de zona para servidor DNS.  
  
-   **Set-DnsServerZoneTransferPolicy**. Esse cmdlet altera as configurações de uma política de transferência de zona de servidor DNS existente.  
  
-   **Get-DnsServerResponseRateLimiting**. Este cmdlet recupera as configurações de RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Esse cmdlet altera as configurações de RRL.  
  
-   **Add-DnsServerResponseRateLimitingExceptionlist**. Esse cmdlet cria uma lista de exceções RRL no servidor DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet recupera RRL excception listas.  
  
-   **Remove-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet removerá uma lista de exceção RRL existente.  
  
-   **Set-DnsServerResponseRateLimitingExceptionlist**. Esse cmdlet altera RRL listas de exceção.  
  
-   **Add-DnsServerResourceRecord**. Esse cmdlet foi atualizado para dar suporte a tipo de registro desconhecido.  
  
-   **Get-DnsServerResourceRecord**. Esse cmdlet foi atualizado para dar suporte a tipo de registro desconhecido.  
  
-   **Remove-DnsServerResourceRecord**. Esse cmdlet foi atualizado para dar suporte a tipo de registro desconhecido.  
  
-   **Set-DnsServerResourceRecord**. Esse cmdlet foi atualizado para dar suporte a tipo de registro desconhecido

Para obter mais informações, consulte os seguintes tópicos de referência de comando do Windows PowerShell do Windows Server 2016.

- [Módulo de DnsServer](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Módulo DnsClient](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no cliente DNS](What-s-New-in-DNS-Client.md)  
  

  

