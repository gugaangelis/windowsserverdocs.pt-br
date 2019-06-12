---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: Erro de replicação 1396 Falha de Logon. O nome da conta de destino está incorreto
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 55e93eb24707fb1a583d060f44131ac794b00d22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442549"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Erro de replicação 1396 Falha de Logon. O nome da conta de destino está incorreto

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Este artigo descreve os sintomas, causas e como resolver a replicação do Active Directory que apresentaram falha com erro Win32 1396: &quot;Logon failure: O nome da conta de destino está incorreto.&quot; </para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Sintomas</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">Faz com que</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">Resoluções</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Sintomas</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>Relatórios DCDIAG que o teste de replicações do Active Directory falhou com o erro 1396: Falha no logon: O nome da conta de destino está incorreto.&quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN. EXE relata que a última tentativa de replicação falhou com status 1396.</para><para>REPADMIN comandos que normalmente citar o status 1396 incluem, mas não estão limitados a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN/LATÊNCIA DE /SHOWVECTOR</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Saída de exemplo do &quot;REPADMIN /SHOWREPS&quot; ilustrando a replicação de entrada de CONTOSO-DC2 para CONTOSO-DC1 falhando com o &quot;falha no Logon: O nome da conta de destino está incorreto. &quot; erro é mostrado abaixo:</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1396 (0x574):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.
</code></listItem><listItem><para>O <ui>Replicate now</ui> comando nos serviços e Sites do Active Directory retorna &quot;falha no Logon: O nome da conta de destino está incorreto.&quot;</para><para>Clicando duas vezes no objeto de conexão de um DC de origem e escolhendo <ui>Replicate now</ui> falha com &quot;falha no Logon: O nome da conta de destino está incorreto.&quot; A tela a mensagem de erro é mostrada abaixo:</para><para>Texto de título da caixa de diálogo:</para><para>Replicar agora</para><para>Texto da mensagem de caixa de diálogo: </para><para>O seguinte erro ocorreu durante a tentativa de sincronizar o contexto de nomenclatura &lt;caminho DNS da partição&gt; do controlador de domínio &lt;DC de origem&gt; ao controlador de domínio &lt;destino DC&gt;: Falha no logon: O nome da conta de destino está incorreto. Esta operação não continuará. </para></listItem><listItem><para>O KCC NTDS, NTDS geral ou Microsoft-Windows-Domainservice eventos com o status de 1396 são registrados no log de serviços de diretório no Visualizador de eventos.</para><para>Eventos do Active Directory que normalmente citam o status de 1396 incluem, mas não estão limitados a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID de evento</para></TD><TD><para>Origem do Evento</para></TD><TD><para>Cadeia de caracteres do evento</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>O Active Directory Assistente de instalação do serviços de domínio Active (Dcpromo) não pôde estabelecer conexão com o seguinte controlador de domínio.</para></TD></tr><tr><TD><para>1645</para><para>Esse evento lista o SPN de três partes.</para></TD><TD><para>Replicação de NTDS</para></TD><TD><para>O Active Directory não executou uma chamada de procedimento remoto (RPC) autenticada para outro controlador de domínio porque no nome de entidade de serviço (SPN) desejado para o controlador de domínio de destino não está registrado no controlador de domínio do Centro de Distribuição de Chaves (KDC) que resolve o SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Serviços de domínio do Active Directory tentou se comunicar com o seguinte catálogo global e as tentativas não foram bem-sucedidos.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>O Knowledge Consistency Checker localizado de uma conexão de replicação para o serviço de diretório somente leitura local e tentou atualizá-lo remotamente na seguinte instância de serviço de diretório. A operação falhou. Ele será repetido.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Falha na tentativa de estabelecer um vínculo de replicação para a seguinte partição de diretório gravável.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>A tentativa de estabelecer um link de replicação para uma partição de diretório somente leitura com os seguintes parâmetros falhou.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> O servidor não pode registrar seu nome no DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO falhar com um erro na tela</para><para>Texto de título da caixa de diálogo:</para><para>Falha na instalação do Active Directory</para><para>Texto da mensagem de caixa de diálogo:</para><para>A operação falhou porque: O serviço de diretório Falha ao criar o objeto de servidor para CN = NTDS Settings, CN = ServerBeingPromoted, CN = Servers, CN = sites, CN = Sites, CN = Configuration, DC = contoso, DC = no servidor ReplicationSourceDC.contoso.com. </para><para>Verifique se as credenciais de rede fornecidas têm acesso suficiente para adicionar uma réplica. </para><para>
&quot;Falha no logon: O nome da conta de destino está incorreto. &quot;</para><para>Nesse caso, a ID do evento 1645 e 1168 1125 são registradas no servidor que está sendo promovido.</para></listItem><listItem><para>Mapear uma unidade usando <embeddedLabel>net use</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>Nesse caso, o servidor pode também log 333 de ID de evento no log de eventos do sistema e use uma grande quantidade de memória virtual para um aplicativo como o SQL Server.</para></listItem><listItem><para>A hora do controlador de domínio está incorreta.</para></listItem><listItem><para>O KDC não será iniciado em um RODC após uma restauração da conta krbtgt para o RODC, que tinha sido excluído. Por exemplo, após uma restauração, erro 1396 é exibido. </para><para>
ID do evento 1645 é registrado no RODC. </para><para>
Dcdiag também relata um erro que ele não é possível atualizar a conta krbtgt do RODC. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Faz com que</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>O SPN não existe no catálogo global de pesquisados pelo KDC em nome do cliente tentar autenticar usando Kerberos.</para>
          <para>No contexto de replicação do Active Directory, o cliente Kerberos é o destino do controlador de domínio, o KDC executa a pesquisa SPN é provavelmente o destino do controlador de domínio em si, mas pode ser um controlador de domínio remoto.</para>
        </listItem>
        <listItem>
          <para>O usuário ou a conta de serviço que deve conter o nome de entidade de serviço que estão sendo pesquisado não existe no catálogo global de pesquisados pelo KDC em nome de destino tentar replicar do controlador de domínio.</para>
          <para>No contexto de replicação do Active Directory, a conta de computador do controlador de domínio de origem não existe no catálogo global de pesquisados pelo controlador de domínio em nome de replicação de entrada executando o controlador de domínio de destino.</para>
        </listItem>
        <listItem>
          <para>O controlador de domínio de destino não tem um segredo LSA para o domínio de controladores de domínio de origem.</para>
        </listItem>
        <listItem>
          <para>O SPN que estão sendo pesquisado existe em uma conta de computador diferente que o controlador de domínio de origem.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Resoluções</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Verifique o log de eventos do serviço de diretório em que o controlador de domínio de destino para o evento de replicação de NTDS 1645 e observe o seguinte:</para>
          <para>O nome do controlador de domínio de destino</para>
          <para>O SPN que estão sendo pesquisados (e3514235-4b06-11d1-ab04-00c04fc2dcd2 /&lt;guid do objeto de configurações NTDS de controladores de domínio de origem do objeto&gt;/&lt;domínio de destino&amp;amp; gt;. &amp;amp; lt; tld&amp;amp; gt; @&lt;domínio de destino&gt;.&lt; TLD&gt;</para>
          <para>O KDC que está sendo usado pelo controlador de domínio de destino</para>
        </listItem>
        <listItem>
          <para>No console do KDC identificado na etapa 1, digite: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>Execute o teste de localizador NLTEST imediatamente após uma tentativa de replicação falha com o erro 1396 no controlador de domínio de destino. </para>
          <para>Isso deve identificar esse GC que o KDC está executando pesquisas de SPN no. </para>
          <para>O GC que está sendo pesquisado pelo KDC também pode ser capturado nos eventos Microsoft-Windows-Domainservice 1655.</para>
        </listItem>
        <listItem>
          <para>Pesquisar o SPN descoberto na etapa 1 no catálogo global descoberto na etapa 2.</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>OU</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Verifique se o objeto de host para o SPN existe.</para>
          <para>Verifique se o caminho do DN para o objeto de host incluindo se o objeto é CNF / reside no contêiner Achados e perdidos ou danificados de conflito.</para>
          <para>Verifique se a fonte de controladores de domínio Active Directory Replication SPN é registrada somente em controladores de domínio conta de computador de origem.</para>
          <para>Se a replicação SPN estiver ausente, determine se o DC de origem tiver registrado seu SPN com ele próprio, e se o SPN está ausente no GC usado pelo KDC devido à latência de replicação simples ou uma falha de replicação.</para>
        </listItem>
        <listItem>
          <para>Verifique a integridade do canal de segurança e integridade de confiança.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Solucionando problemas de operações do Active Directory que falham com o erro 1396: Falha no logon: O nome da conta de destino está incorreto.</linkText>
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


