---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: ID do evento 2088 - Falha na pesquisa de DNS ocorreu com êxito da replicação
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: cc090fa749a601e53b4347cce43245f22badc8ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840707"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID do evento 2088: Falha na pesquisa de DNS ocorreu com êxito da replicação

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

Outras operações essenciais em computadores de membro, controladores de domínio ou servidores de aplicativos nesta floresta do Active Directory Domain Services, incluindo o acesso aos recursos de rede ou autenticação de logon pode estar afetando a configuração de DNS inválido. 

Imediatamente, você deve resolver esse erro de configuração de DNS para que esse controlador de domínio possa resolver o endereço IP do controlador de domínio de origem usando o DNS. 

Nome do servidor alternativo: Nome do host de DC1 falhando DNS: 4a8717eb-8e58-456c-995a-c92e4add7e8e._msdcs.contoso.com 

OBSERVAÇÃO: Por padrão, apenas 10 falhas DNS são mostradas para qualquer período de 12 horas determinado, mesmo que ocorram falhas mais de 10.  Para registrar todos os eventos de falha individuais, defina o seguinte diagnóstico valor do registro como 1: 

Caminho do registro: HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC Client 

Ação do usuário: 

1) Se o controlador de domínio de origem tem não mais funcionando ou seu sistema operacional foi reinstalado com um nome de computador diferente ou NTDSDSA objeto GUID, remover metadados do controlador de domínio de origem com ntdsutil.exe, usando as etapas descritas no artigo MSKB 216498. 

2) Confirme se o controlador de domínio de origem executando o Active Directory e está acessível na rede digitando "net view \\ &lt;nome de DC de origem&gt;" ou "ping &lt;nome do DC de origem&gt;". 

3) Verifique se que o controlador de domínio de origem está usando um servidor DNS válido para os serviços DNS e que registro host CNAME do controlador de domínio de origem registro e estão registrados corretamente, usando a versão aprimorada do DNS de DCDIAG. EXE disponível em https://www.microsoft.com/dns 

dcdiag /test:dns 

4) Verificar se esse controlador de domínio de destino está usando um servidor DNS válido para os serviços DNS, executando a versão aprimorada do DNS de DCDIAG. Comando EXE no console do controlador de domínio de destino, da seguinte maneira: 

dcdiag /test:dns 

5) Para análise posterior das falhas de erro do DNS Consulte 824449 KB: https://support.microsoft.com/?kbid=824449 

Valor adicional do erro de dados: 11004 o nome solicitado é válido, mas nenhum dado do tipo solicitado foi encontrado</code> </introduction>
  <section>
    <title>Diagnóstico</title>
    <content>
      <para>Falha ao resolver o nome de controlador de domínio de origem usando o registro de recurso de alias (CNAME) no DNS pode ser devido a configurações incorretas do DNS ou atrasos de propagação de dados DNS.</para>
    </content>
  </section>
  <section>
    <title>Resolução</title>
    <content>
      <para>Continuar com o teste de DNS, conforme descrito em "<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID do evento 2087: Falha na pesquisa de DNS causou falha na replicação</link>. "</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


