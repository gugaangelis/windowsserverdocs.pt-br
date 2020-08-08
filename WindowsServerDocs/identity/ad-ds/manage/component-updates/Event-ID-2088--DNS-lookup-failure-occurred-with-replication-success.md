---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: ID do evento 2088-falha na pesquisa de DNS com êxito de replicação
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9dbb7debbca8d1625ebe975a051ed8b607d1ddd0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943281"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID do evento 2088: falha de pesquisa de DNS ocorreu com êxito na replicação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando um controlador de domínio de destino que executa o Windows Server 2003 com Service Pack 1 (SP1) recebe a ID de evento 2088 no log de eventos do serviço de diretório, as tentativas de resolver o identificador global exclusivo (GUID) no registro de recurso de alias (CNAME) para um endereço IP para o controlador de domínio de origem falharam. No entanto, o controlador de domínio de destino tentou outros meios para resolver o nome e teve êxito usando o FQDN (nome de domínio totalmente qualificado) ou o nome NetBIOS do controlador de domínio de origem. Embora a replicação tenha sido bem-sucedida, o problema do DNS (sistema de nomes de domínio) deve ser diagnosticado e resolvido.

Segue um exemplo do texto do evento:

```
Log Name: Directory Service
Source: Microsoft-Windows-ActiveDirectory_DomainService
Date: 3/15/2008  9:20:11 AM
Event ID: 2088
Task Category: DS RPC Client
Level: Warning
Keywords: Classic
User: ANONYMOUS LOGON
Computer: DC3.contoso.com
Description:
Active Directory could not use DNS to resolve the IP address of the source domain controller listed below. To maintain the consistency of Security groups, group policy, users and computers and their passwords, Active Directory Domain Services successfully replicated using the NetBIOS or fully qualified computer name of the source domain controller.
```

A configuração de DNS inválida pode estar afetando outras operações essenciais em computadores membros, controladores de domínio ou servidores de aplicativos nessa Active Directory Domain Services floresta, incluindo autenticação de logon ou acesso a recursos de rede.

Você deve resolver imediatamente esse erro de configuração de DNS para que esse controlador de domínio possa resolver o endereço IP do controlador de domínio de origem usando DNS.

Nome de servidor alternativo: DC1 falha no nome de host DNS: 4a8717eb-8e58-456c-995a-c92e4add7e8e. _msdcs. contoso. com

Observação: por padrão, apenas até 10 falhas de DNS são mostradas para um determinado período de 12 horas, mesmo se ocorrerem mais de 10 falhas.  Para registrar em log todos os eventos de falha individuais, defina o seguinte valor de registro de diagnóstico como 1:

Caminho do registro: cliente RPC do HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS

Ação do usuário:

1) Se o controlador de domínio de origem não estiver mais funcionando ou se seu sistema operacional tiver sido reinstalado com um nome de computador ou GUID de objeto NTDSDSA diferente, remova os metadados do controlador de domínio de origem com ntdsutil.exe, usando as etapas descritas no artigo 216498 do MSKB.

2) Confirme se o controlador de domínio de origem está executando Active Directory e está acessível na rede digitando "net view \\ <source DC name> " ou "ping <source DC name> ".

3) Verifique se o controlador de domínio de origem está usando um servidor DNS válido para os serviços DNS e se o registro de host do controlador de domínio de origem e o registro CNAME estão registrados corretamente, usando a versão aprimorada do DNS do DCDIAG.EXE disponível em<https://www.microsoft.com/dns>

Dcdiag/test: DNS

4) Verifique se esse controlador de domínio de destino está usando um servidor DNS válido para serviços DNS, executando a versão aprimorada do DNS do DCDIAG.EXE comando no console do controlador de domínio de destino, da seguinte maneira:

Dcdiag/test: DNS

5) Para análise adicional de falhas de erro de DNS, consulte KB 824449:<https://support.microsoft.com/?kbid=824449>

Valor de erro de dados adicional: 11004 o nome solicitado é válido, mas nenhum dado do tipo solicitado foi encontrado </code></introduction>
  <section>
    <title>Diagnóstico</title>
    <content>
      <para>Falha ao resolver o nome do controlador de domínio de origem usando o registro de recurso de alias (CNAME) no DNS pode ser devido a configurações incorretas de DNS ou atrasos na propagação de dados DNS.</para>
    </content>
  </section>
  <section>
    <title>Resolução</title>
    <content>
      <para>Continue com o teste de DNS conforme descrito em &quot; <link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID do evento 2087: falha na pesquisa de DNS causou a falha da replicação</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>
