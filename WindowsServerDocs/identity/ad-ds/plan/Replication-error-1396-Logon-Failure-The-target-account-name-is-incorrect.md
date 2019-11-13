---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: Erro de replicação 1396 Falha de Logon. O nome da conta de destino está incorreto
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a8af10fd54f557e4f4a2127dbd1cc178d53d93a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402483"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Erro de replicação 1396 Falha de Logon. O nome da conta de destino está incorreto

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Este artigo descreve os sintomas, a causa e como resolver Active Directory falha de replicação com o erro do Win32 1396: falha de logon do &quot;: o nome da conta de destino está incorreto.&quot; </para>
    <list class="bullet"> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Sintomas</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">Causa</link>
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
<listItem><para>O DCDIAG relata que o teste de replicação do Active Directory falhou com o erro 1396: falha de logon: o nome da conta de destino está incorreto.&quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN. EXE relata que a última tentativa de replicação falhou com o status 1396.</para><para>Os comandos REPADMIN que normalmente citam o status 1396 incluem, mas não estão limitados a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN/ADD</para></listItem><listItem><para>REPADMIN/REPLSUM</para></listItem><listItem><para>REPADMIN/REHOST</para></listItem><listItem><para>REPADMIN/SHOWVECTOR/LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN/SHOWREPS</para></listItem><listItem><para>REPADMIN/SHOWREPL</para></listItem><listItem><para>REPADMIN/SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Saída de exemplo de &quot;REPADMIN/SHOWREPS&quot; ilustrando a replicação de entrada de CONTOSO-DC2 para CONTOSO-DC1 falhando com a falha de logon de &quot;: o nome da conta de destino está incorreto.&quot; erro é mostrado abaixo::</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para>O comando <ui>replicate Now</ui> no Active Directory sites e serviços retorna &quot;falha de logon: o nome da conta de destino está incorreto.&quot;</para><para>Clicar com o botão direito do mouse no objeto de conexão de um DC de origem e escolher <ui>replicar agora</ui> falha com &quot;falha de logon: o nome da conta de destino está incorreto.&quot; a mensagem de erro na tela é mostrada abaixo:</para><para>Texto do título do diálogo:</para><para>Replicar agora</para><para>Texto da mensagem de caixa de diálogo: </para><para>O erro a seguir ocorreu durante a tentativa de sincronizar o contexto de nomenclatura &lt;o caminho DNS da partição&gt; do controlador de domínio &lt;&gt; do DC de origem para o controlador de domínio &lt;DC de destino&gt;: falha de logon: o nome da conta de destino está incorreto. Esta operação não continuará. </para></listItem><listItem><para>Os eventos NTDS KCC, NTDS General ou Microsoft-Windows-ActiveDirectory_DomainService com o status 1396 são registrados no log dos serviços de diretório Visualizador de Eventos.</para><para>Active Directory eventos que normalmente citem o status 1396 incluem, mas não estão limitados a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID de evento</para></TD><TD><para>Origem do Evento</para></TD><TD><para>Cadeia de eventos</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>O Assistente para Instalação do Active Directory Domain Services (Dcpromo) não pôde estabelecer conexão com o controlador de domínio a seguir.</para></TD></tr><tr><TD><para>1645</para><para>Esse evento lista o SPN de três partes.</para></TD><TD><para>Replicação de NTDS</para></TD><TD><para>O Active Directory não executou uma chamada de procedimento remoto (RPC) autenticada para outro controlador de domínio porque no nome de entidade de serviço (SPN) desejado para o controlador de domínio de destino não está registrado no controlador de domínio do Centro de Distribuição de Chaves (KDC) que resolve o SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory Domain Services tentou se comunicar com o catálogo global a seguir e as tentativas não foram bem-sucedidas.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>O Knowledge Consistency Checker localizado uma conexão de replicação para o serviço de diretório somente leitura local e tentou atualizá-lo remotamente na instância do serviço de diretório a seguir. Falha na operação. Ele será repetido.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC DO NTDS</para></TD><TD><para>Falha na tentativa de estabelecer um link de replicação para a seguinte partição de diretório gravável.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>KCC DO NTDS</para></TD><TD><para>Falha na tentativa de estabelecer um link de replicação para uma partição de diretório somente leitura com os seguintes parâmetros.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>IDÊNTICO</para></TD><TD><para> O servidor não pode registrar seu nome no DNS.</para></TD></tr></tbody></table></listItem><listItem><para>O DCPROMO falha com um erro na tela</para><para>Texto do título do diálogo:</para><para>Falha na instalação do Active Directory</para><para>Texto da mensagem de caixa de diálogo:</para><para>A operação falhou porque: o serviço de diretório não pôde criar o objeto de servidor para CN = NTDS Settings, CN = ServerBeingPromoted, CN = Servers, CN = site, CN = sites, CN = Configuration, DC = contoso, DC = com no servidor ReplicationSourceDC.contoso.com. </para><para>Verifique se as credenciais de rede fornecidas têm acesso suficiente para adicionar uma réplica. </para><para>
Falha de logon do &quot;: o nome da conta de destino está incorreto. &quot;</para><para>Nesse caso, a ID de evento 1645, 1168 e 1125 são registradas no servidor que está sendo promovido.</para></listItem><listItem><para>Mapeie uma unidade usando <embeddedLabel>net use</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>Nesse caso, o servidor também pode registrar a ID de evento 333 no log de eventos do sistema e usar uma grande quantidade de memória virtual para um aplicativo, como SQL Server.</para></listItem><listItem><para>A hora do controlador de domínio está incorreta.</para></listItem><listItem><para>O KDC não será iniciado em um RODC após uma restauração da conta krbtgt para o RODC, que foi excluído. Por exemplo, após uma restauração, o erro 1396 é exibido. </para><para>
A ID do evento 1645 está registrada no RODC. </para><para>
O Dcdiag também relata um erro de que ele não pode atualizar a conta do RODC krbtgt. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Causa</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>O SPN não existe no catálogo global pesquisado pelo KDC em nome do cliente que está tentando se autenticar usando o Kerberos.</para>
          <para>No contexto de replicação de Active Directory, o cliente Kerberos é o DC de destino, o KDC que executa a pesquisa de SPN é provavelmente o próprio DC de destino, mas pode ser um controlador de domínio remoto.</para>
        </listItem>
        <listItem>
          <para>A conta de usuário ou serviço que deve conter o nome da entidade de serviço que está sendo pesquisada não existe no catálogo global pesquisado pelo KDC em nome do DC de destino que está tentando replicar.</para>
          <para>No contexto de replicação de Active Directory, a conta de computador DC de origem não existe no catálogo global pesquisado pelo controlador de domínio em nome do DC de destino que executa a replicação de entrada.</para>
        </listItem>
        <listItem>
          <para>O DC de destino não tem um segredo de LSA para o domínio DCs de origem.</para>
        </listItem>
        <listItem>
          <para>O SPN que está sendo pesquisado existe em uma conta de computador diferente do DC de origem.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Resoluções</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Verifique o log de eventos do serviço de diretório no DC de destino para o evento de replicação NTDS 1645 e observe o seguinte:</para>
          <para>O nome do DC de destino</para>
          <para>O SPN que está sendo pesquisado (E3514235-4B06-11D1-AB04-00C04FC2DCD2/&lt;GUID do objeto para o objeto de configurações NTDS de origem&gt;/&lt;domínio de destino&amp;amp; gt;.&amp;amp; lt; TLD&amp;amp; gt; @&lt;domínio de destino&gt;.&lt;TLD&gt;</para>
          <para>O KDC que está sendo usado pelo DC de destino</para>
        </listItem>
        <listItem>
          <para>No console do KDC identificado na etapa 1, digite: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>Execute o teste do localizador NLTEST imediatamente após uma tentativa de replicação que falha com o erro 1396 no DC de destino. </para>
          <para>Isso deve identificar o GC em relação ao qual o KDC está realizando pesquisas de SPN. </para>
          <para>O GC que está sendo pesquisado pelo KDC também pode ser capturado no evento 1655 do Microsoft-Windows-ActiveDirectory_DomainService.</para>
        </listItem>
        <listItem>
          <para>Pesquise o SPN descoberto na etapa 1 no catálogo global descoberto na etapa 2.</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>OU</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Verifique se o objeto de host para o SPN existe.</para>
          <para>Verifique o caminho DN para o objeto de host, incluindo se o objeto é CNF/conflito danificado ou reside no contêiner perdido e encontrado.</para>
          <para>Verifique se os DCs de origem Active Directory o SPN de replicação está registrado somente na conta de computador dos DCs de origem.</para>
          <para>Se o SPN de replicação estiver ausente, determine se o controlador de domínio de origem registrou seu SPN com ele mesmo e se o SPN está ausente no GC usado pelo KDC devido à latência de replicação simples ou uma falha de replicação.</para>
        </listItem>
        <listItem>
          <para>Verifique a integridade do canal seguro e a integridade da confiança.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink> 
      <linkText>Solução de problemas Active Directory operações que falham com o erro 1396: falha de logon: o nome da conta de destino está incorreto.</linkText> 
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri> 
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


