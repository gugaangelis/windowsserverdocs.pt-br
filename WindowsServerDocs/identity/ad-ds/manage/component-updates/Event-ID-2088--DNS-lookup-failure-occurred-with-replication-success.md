---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: "ID de evento 2088 - DNS lookup falha com sucesso de replicação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4f223f075775f942f83a1962da28a77e85e89aa0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID de evento 2088: DNS lookup falha com sucesso de replicação

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

Configuração inválida de DNS pode estar afetando outras operações essenciais em computadores membro, controladores de domínio ou servidores de aplicativo nesta floresta do Active Directory Domain Services, incluindo a autenticação de logon ou acesso a recursos de rede. 

Você deve resolver esse erro de configuração do DNS imediatamente para que esse controlador de domínio pode resolver o endereço IP do controlador de domínio usando DNS. 

Nome de servidor alternativo: nome de host DNS de falha de DC1: 4a8717eb-8e58-456 c-995a-c92e4add7e8e._msdcs. contoso.com 

Observação: Por padrão, somente até 10 falhas DNS são mostradas para qualquer determinado 12 horas, mesmo que ocorram falhas mais de 10.  Para registrar todos os eventos de falha individuais, defina o diagnóstico seguinte valor do registro como 1: 

Caminho do registro: HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC cliente 

Ação do usuário: 

1) Se o controlador de domínio de origem for maior não funcionar ou do sistema operacional foi reinstalado com um nome de outro computador ou NTDSDSA objeto GUID, remover metadados do controlador de domínio de origem com ntdsutil.exe, usando as etapas descritas no artigo MSKB 216498. 

2) Confirme se o controlador de domínio de origem está executando o Active Directory e é acessível na rede digitando "net exibição \ \&lt;nome origem DC&gt;" ou "ping &lt;nome origem DC&gt;". 

3) Verifique se o controlador de domínio de origem está usando um servidor DNS válido de serviços DNS e o controlador de domínio de origem registro de host e CNAME gravam são registrado corretamente, usando a versão aprimorada do DNS do DCDIAG.EXE disponível em https://www.microsoft.com/dns 

dcdiag//test: dns 

4) Verificar se esse controlador de domínio de destino está usando um servidor DNS válido para serviços DNS, executando a versão aprimorada do DNS do DCDIAG.EXE comando no console do controlador de domínio de destino, da seguinte maneira: 

dcdiag//test: dns 

5) Para fazer uma análise adicional do DNS falhas de erro veja KB 824449: https://support.microsoft.com/?kbid=824449 

Valor de erro de dados adicional: 11004 o nome solicitado é válido, mas nenhum dado do tipo solicitado foi encontrado</code>
  </introduction>
  <section>
              
    <title>Diagnosis</title>
    <content>
      <para>Failure to resolve the source domain controller name by using the alias (CNAME) resource record in DNS can be due to DNS misconfigurations or delays in DNS data propagation.</para>
    </content>
  </section>
  <section>
              
    <title>Resolution</title>
    <content>
      <para>Proceed with DNS testing as described in "<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">Event ID 2087: DNS lookup failure caused replication to fail</link>."</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


