---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: ID do evento 2088-falha na pesquisa de DNS com êxito de replicação
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f84fd7be45995e9e0b318b42c8b4152af244a9da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823049"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID do evento 2088: falha de pesquisa de DNS ocorreu com êxito na replicação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

    
    <developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
      <introduction>
    <para>When a destination domain controller running Windows Server 2003 with Service Pack 1 (SP1) receives Event ID 2088 in the Directory Service event log, attempts to resolve the globally unique identifier (GUID) in the alias (CNAME) resource record to an IP address for the source domain controller failed. However, the destination domain controller tried other means to resolve the name and succeeded by using either the fully qualified domain name (FQDN) or the NetBIOS name of the source domain controller. Although replication was successful, the Domain Name System (DNS) problem should be diagnosed and resolved. </para>
    <para>The following is an example of the event text: </para>
    <code>Log Name: Directory Service

    Source: Microsoft-Windows-ActiveDirectory_DomainService
    Date: 3/15/2008  9:20:11 AM
    Event ID: 2088
    Task Category: DS RPC Client 
    Level: Warning
    Keywords: Classic
    User: ANONYMOUS LOGON
    Computer: DC3.contoso.com
    Description:
    Active Directory could not use DNS to resolve the IP address of the 
    source domain controller listed below. To maintain the consistency 
    of Security groups, group policy, users and computers and their passwords, 
    Active Directory Domain Services successfully replicated using the NetBIOS 
    or fully qualified computer name of the source domain controller. 

A configuração de DNS inválida pode estar afetando outras operações essenciais em computadores membros, controladores de domínio ou servidores de aplicativos nessa Active Directory Domain Services floresta, incluindo autenticação de logon ou acesso a recursos de rede. 

Você deve resolver imediatamente esse erro de configuração de DNS para que esse controlador de domínio possa resolver o endereço IP do controlador de domínio de origem usando DNS. 

Nome de servidor alternativo: DC1 falha no nome de host DNS: 4a8717eb-8e58-456c-995a-c92e4add7e8e. _msdcs. contoso. com 

Observação: por padrão, apenas até 10 falhas de DNS são mostradas para um determinado período de 12 horas, mesmo se ocorrerem mais de 10 falhas.  Para registrar em log todos os eventos de falha individuais, defina o seguinte valor de registro de diagnóstico como 1: 

Caminho do registro: cliente RPC do HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS 

Ação do usuário: 

1) Se o controlador de domínio de origem não estiver mais funcionando ou se seu sistema operacional tiver sido reinstalado com um nome de computador ou GUID de objeto NTDSDSA diferente, remova os metadados do controlador de domínio de origem com Ntdsutil. exe, usando as etapas descritas no artigo 216498 do MSKB. 

2) Confirme se o controlador de domínio de origem está executando Active Directory e está acessível na rede digitando "net view \\&lt;nome do DC de origem&gt;" ou "ping &lt;nome do DC de origem&gt;". 

3) Verifique se o controlador de domínio de origem está usando um servidor DNS válido para os serviços DNS e se o registro de host do controlador de domínio de origem e o registro CNAME estão registrados corretamente, usando a versão aprimorada do DNS do DCDIAG. EXE disponível em <https://www.microsoft.com/dns> 

Dcdiag/test: DNS 

4) Verifique se esse controlador de domínio de destino está usando um servidor DNS válido para serviços DNS, executando a versão aprimorada do DNS do DCDIAG. EXE no console do controlador de domínio de destino, da seguinte maneira: 

Dcdiag/test: DNS 

5) Para análise adicional de falhas de erro de DNS, consulte KB 824449: <https://support.microsoft.com/?kbid=824449> 

Valor de erro de dados adicional: 11004 o nome solicitado é válido, mas nenhum dado do tipo solicitado foi encontrado</code> </introduction>
  <section>
    <content>de 
    <title>diagnóstico</title> 
      <para>Falha ao resolver o nome do controlador de domínio de origem usando o registro de recurso de alias (CNAME) no DNS pode ser devido a configurações incorretas de DNS ou atrasos na propagação de dados DNS.</para>
    </content>
  </section>
  <section>
    <content>de 
    <title>resolução</title> 
      <para>Continue com o teste de DNS conforme descrito em &quot;<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID do evento 2087: falha na pesquisa de DNS causou a falha da replicação</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


