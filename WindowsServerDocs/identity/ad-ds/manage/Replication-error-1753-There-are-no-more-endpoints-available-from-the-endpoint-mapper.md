---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: Erro de replicação 1753 Não há mais pontos de extremidade disponíveis do mapeador de ponto de extremidade
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a280d540d09c6fdcb7846d1cf545856869be1152
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008962"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Erro de replicação 1753 Não há mais pontos de extremidade disponíveis do mapeador de ponto de extremidade

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Este tópico explica os sintomas, causas e como resolver o Active Directory replication erro 8524 a operação de DSA não conseguir continuar devido a uma falha de pesquisa DNS.</para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">Sintomas</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">Causa</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">Resoluções</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">Obter mais informações</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Sintomas</title>
    <content>
      <para>Este artigo descreve os sintomas, causa e etapas de resolução para operações do Active Directory que falham com o erro Win32 1753: "Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade."</para>
      <list class="ordered">
        <listItem>
          <para>Relatórios DCDIAG que o teste de conectividade, o teste de replicações do Active Directory ou o teste de KnowsOfRoleHolders falhou com o erro 1753: "Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade."</para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN. EXE relata que essa tentativa de replicação falhou com status 1753.</para><para>REPADMIN comandos que normalmente citar o status de 1753 podem incluir, mas não estão limitados a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Exemplo de saída do "REPADMIN /SHOWREPS" ilustrando a replicação de entrada de CONTOSO-DC2 para CONTOSO-DC1 falha com o erro "acesso negado à replicação" é mostrado abaixo:</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para>O <ui>verificar a topologia de replicação</ui> comando nos serviços e Sites do Active Directory retorna "Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade."</para><para>Clicando duas vezes no objeto de conexão de um DC de origem e escolhendo <ui>verificar a topologia de replicação</ui> falha com "Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade." A tela a mensagem de erro é mostrada abaixo:</para><para>Texto de título da caixa de diálogo: Verifique a topologia de replicação</para><para>Texto da mensagem de caixa de diálogo: </para><para>O seguinte erro ocorreu durante a tentativa de contatar o controlador de domínio: Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade.</para></listItem><listItem><para>O <ui>Replicate now</ui> comando nos serviços e Sites do Active Directory retorna "não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade."</para><para>Clicando duas vezes no objeto de conexão de um DC de origem e escolhendo <ui>Replicate now</ui> falha com "Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade." A tela a mensagem de erro é mostrada abaixo:</para><para>Texto de título da caixa de diálogo: Replicar agora</para><para>Texto da mensagem de caixa de diálogo: O seguinte erro ocorreu durante a tentativa de sincronizar o contexto de nomenclatura &lt;% % de nome de partição de diretório&gt; do controlador de domínio &lt;DC de origem&gt; ao controlador de domínio &lt;DCdedestino&gt;:</para><para>

Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade.</para><para>A operação não continuará</para></listItem><listItem><para>O KCC NTDS, NTDS geral ou Microsoft-Windows-Domainservice eventos com status-2146893022 são registrados no log de serviços de diretório no Visualizador de eventos.</para><para>Eventos do Active Directory que normalmente citam o status-2146893022 incluem, mas não estão limitados a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID de evento</para></TD><TD><para>Origem do Evento</para></TD><TD><para>Cadeia de caracteres do evento</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS geral</para></TD><TD><para>Active Directory tentou se comunicar com o seguinte catálogo global e as tentativas não foram bem-sucedidos.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Falha na tentativa de estabelecer um vínculo de replicação para a seguinte partição de diretório gravável.</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Falha na tentativa por Knowledge Consistency Checker (KCC) para adicionar um contrato de replicação para o seguinte diretório partição e a fonte de controlador de domínio.</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>Causa</title>
    <content>
      <para>O diagrama a seguir mostra o fluxo de trabalho do RPC iniciando com o registro do aplicativo para servidores com o mapeador de ponto de extremidade de RPC (EPM) na etapa 1 para a transferência de dados do cliente RPC para o aplicativo cliente na etapa 7. </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>Etapas 1 a 7 mapa às seguintes operações:</para>
      <list class="ordered">
        <listItem>
          <para>Aplicativo de servidor registra seus pontos de extremidade com o mapeador de ponto de extremidade RPC (EPM) </para>
        </listItem>
        <listItem>
          <para>Cliente faz uma chamada RPC (em nome do usuário, sistema operacional ou aplicativo iniciou operação) </para>
        </listItem>
        <listItem>
          <para>RPC do lado do cliente entra em contato com os computadores de destino EPM e peça para o ponto de extremidade completar a chamada do cliente </para>
        </listItem>
        <listItem>
          <para>Responde EPM de máquina do servidor com um ponto de extremidade </para>
        </listItem>
        <listItem>
          <para>RPC do lado do cliente entra em contato com o aplicativo de servidor </para>
        </listItem>
        <listItem>
          <para>Aplicativo de servidor executa a chamada, retorna o resultado para o cliente RPC </para>
        </listItem>
        <listItem>
          <para>RPC do lado do cliente passa o resultado de volta para o aplicativo cliente</para>
        </listItem>
      </list>
      <para>Falha de 1753 são gerados por uma falha entre as etapas de 3 # e #4. Especificamente, o erro 1753 significa que o cliente RPC (controlador de domínio de destino) foi capaz de contatar o servidor RPC (controlador de domínio de origem) pela porta 135, mas o EPM no servidor de RPC (controlador de domínio de origem) não pôde localizar o aplicativo de RPC de interesse e retornou o erro do lado do servidor de 1753. A presença do erro 1753 indica que o cliente RPC (controlador de domínio de destino) recebeu a resposta de erro do lado servidor do servidor de RPC (origem de replicação do AD DC) pela rede. </para>
      <para>Causas raiz específica para o erro de 1753 incluem: </para>
      <list class="ordered">
        <listItem>
          <para>O aplicativo de servidor nunca iniciado (ou seja, a etapa n º 1 no diagrama "obter mais informações" localizado acima nunca foi tentado).</para>
        </listItem>
        <listItem>
          <para>O aplicativo de servidor é iniciado, mas houve alguma falha durante a inicialização do que o impediu de Registrando com o mapeador de ponto de extremidade RPC (ou seja, etapa n º 1 no diagrama acima "obter mais informações" foi tentada, mas falha).</para>
        </listItem>
        <listItem>
          <para>O aplicativo de servidor iniciado, mas subsequentemente morreu. (ou seja, etapa n º 1 no diagrama acima "obter mais informações" foi concluída com êxito, mas foi desfeita posteriormente, porque o servidor morreu).</para>
        </listItem>
        <listItem>
          <para>O aplicativo de servidor cancelado manualmente seus pontos de extremidade (semelhantes a 3, mas intencional. Mas provavelmente não incluído para fins de integridade.)</para>
        </listItem>
        <listItem>
          <para>O cliente RPC (controlador de domínio de destino) contatar um servidor RPC diferente daquele devido a um nome para o erro de mapeamento de IP no DNS, WINS ou o arquivo Lmhosts/host pretendido.</para>
        </listItem>
      </list>
      <para>Erro de 1753 não é causado por: </para>
      <list class="bullet">
        <listItem>
          <para>A falta de conectividade de rede entre o cliente RPC (controlador de domínio de destino) e o servidor de RPC (controlador de domínio de origem) pela porta 135</para>
        </listItem>
        <listItem>
          <para>A falta de conectividade de rede entre o servidor RPC (controlador de domínio de origem) usando a porta 135 e o cliente RPC (controlador de domínio de destino) pela porta efêmera. </para>
        </listItem>
        <listItem>
          <para>Uma incompatibilidade de senha ou a incapacidade de descriptografar um pacote criptografado do Kerberos, o DC de origem </para>
        </listItem>
      </list>
      <para> </para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Resoluções</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>Verifique se que o registro do seu serviço com o mapeador de ponto de extremidade de serviço foi iniciado</embeddedLabel>
          </para>
          <para>Para o Windows 2000 e controladores de domínio do Windows Server 2003: Certifique-se de que o DC de origem é inicializado no modo normal. </para>
          <para>
Para Windows Server 2008 ou Windows Server 2008 R2: no console do DC de origem, inicie o Gerenciador de serviços (Services. msc) e verifique se que o <embeddedLabel>Active Directory Domain Services</embeddedLabel> serviço está em execução. </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>Verifique se esse cliente RPC (controlador de domínio de destino) conectada ao servidor de RPC pretendido (controlador de domínio de origem)</embeddedLabel>
          </para>
          <para>Todos os controladores de domínio em uma floresta do Active Directory comuns registrar um controlador de domínio registro CNAME na zona MSDCS. &lt;domínio raiz da floresta&gt; zona DNS, independentemente de qual domínio residem na floresta. O registro CNAME do controlador de domínio é derivado de <embeddedLabel>objectGUID</embeddedLabel> atributo do objeto Configurações NTDS para cada controlador de domínio. </para>
          <para>Ao executar operações baseadas em replicação, um controlador de domínio de destino consulta DNS para a registro CNAME de controladores de domínio de origem. O registro CNAME contém o nome de computador totalmente qualificado do DC de origem que é usado para derivar o endereço IP de controladores de domínio de origem por meio de pesquisa de cache do cliente DNS, Host / pesquisa, um host do arquivo Lmhosts / AAAA registrar no DNS ou WINS. </para>
          <para>Obsoleto objetos de configurações NTDS e ruins mapeamentos de nome-para-IP no DNS, WINS, Host e Lmhosts arquivos podem fazer com que o cliente RPC (controlador de domínio de destino) para se conectar ao servidor RPC incorreto (controlador de domínio de origem). Além disso, o mapeamento de nome-para-IP incorreto pode causar o cliente RPC (controlador de domínio de destino) para se conectar a um computador que não tem nem mesmo o aplicativo de servidor de RPC de interesse (a função do Active Directory, neste caso) instalado. (Exemplo: um registro de host obsoletos para o DC2 contém o endereço IP do DC3 ou em um computador membro). </para>
          <para>Verifique se o objectGUID que existe no destino de cópia de controladores de domínio do Active Directory para o DC de origem corresponde o objectGUID do controlador de domínio de origem armazenado na fonte de cópia de controladores de domínio do Active Directory. Se houver uma discrepância, use repadmin. exe /showobjmeta no objeto de configurações ntds para ver qual deles corresponde ao último promoção do DC de origem (Dica: comparar os carimbos de data para a data de criação de objeto de configurações NTDS do. exe /showobjmeta contra a última data de promoção no arquivo de Dcpromo. log de controladores de domínio de origem. Talvez você precise usar a última modificar / criar data do DCPROMO. Arquivo de LOG em si). Se o objeto GUIDs não forem idêntico, o probabilidade de controlador de domínio de destino tem um objeto de configurações NTDS obsoleto para o DC de origem cujo registro CNAME se refere a um registro de host com um nome inválido para mapeamento de IP. </para>
          <para>No controlador de domínio de destino, execute IPCONFIG /ALL para determinar quais servidores DNS de destino que usando o controlador de domínio para resolução de nome:</para>
          <code>c:&gt;ipconfig /all</code>
          <para>No controlador de domínio de destino, execute NSLOOKUP em relação a controladores de domínio totalmente qualificado do registro CNAME do controlador de domínio de origem:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>Verificar se o endereço IP retornado pelo NSLOOKUP "possui" o nome do host / identidade de segurança do DC de origem:</para>
          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>ou</para>
          <para>Fazer logon no console do DC de origem, executar "IPCONFIG" no prompt de comando e verificar se o DC de origem possui o endereço IP retornado pelo comando NSLOOKUP acima</para>
          <para>Verifique para mapeamentos do IP no DNS do host duplicado / obsoletos</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>Se existirem endereços IP inválidos em registros de host, investigue se a eliminação de DNS é habilitado e configurado corretamente. </para><para>Se os testes acima ou um rastreamento de rede não mostra uma consulta de nome, retornando um endereço IP inválido, considere as entradas obsoletas em arquivos HOST, arquivos LMHOSTS e servidores WINS. Observe que os servidores DNS também pode ser configurados para executar a resolução de nome de fallback do WINS.</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>Verifique se que o aplicativo de servidor (Active Directory e outros) foi registrado com o mapeador de ponto de extremidade no servidor de RPC (controlador de domínio de origem)</embeddedLabel>
          </para>
          <para>O Active Directory usa uma combinação de portas registradas dinamicamente e bem conhecidas. Esta tabela lista as conhecidas portas e protocolos usados pelos controladores de domínio do Active Directory.</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>Aplicativo de servidor RPC</para>
                </TD>
                <TD>
                  <para>Port</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>Servidor DNS</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Servidor LDAP</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft-DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP SSL</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Servidor de Catálogo Global</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Servidor de Catálogo Global</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>As conhecidas portas não estão registradas com o mapeador de ponto de extremidade. </para>
          <para>Active Directory e outros aplicativos também registrem os serviços que recebem portas atribuídas dinamicamente no intervalo de portas efêmeras RPC. Esses aplicativos de servidor RPC são atribuídos dinamicamente portas TCP entre 1024 e 5000 em computadores com Windows 2000 e Windows Server 2003 e as portas entre o intervalo de 49152 e 65535 em computadores com Windows Server 2008 e Windows Server 2008 R2. A porta RPC usada pela replicação pode ser embutido em código no registro usando as etapas documentadas no <externalLink> <linkText>KB artigo 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>. Active Directory continua a registrar com o EPM quando configurado para usar uma porta embutida. </para>
          <para>Verifique se que o aplicativo do servidor de RPC de interesse se registrou com o mapeador de ponto de extremidade RPC no servidor de RPC (o DC de origem no caso de replicação do AD). </para>
          <para>Há várias maneiras de realizar essa tarefa, mas um é instalar e executar o PORTQRY em um prompt de comando com privilégios de administrador no console do usando a sintaxe de DC de origem: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>Na saída do portqry, observe os números de porta registrados dinamicamente pelo "MS NT Directory DRS Interface" (UUID = 351...) para o <embeddedLabel>protocolo ncacn_ip_tcp</embeddedLabel>. O trecho a seguir mostra o exemplo de saída usando o portquery de um controlador de domínio do Windows Server 2008 R2 e o UUID / par de protocolo usado especificamente pelo Active Directory realçado na <embeddedLabel>negrito</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>Outras maneiras possíveis para resolver esse erro:</para>
          <list class="ordered">
            <listItem>
              <para>Verifique se que o DC de origem é inicializado no modo normal e se a função de sistema operacional e o controlador de domínio em que o DC de origem tiver iniciado totalmente.</para>
            </listItem>
            <listItem>
              <para>Verifique se o serviço de domínio do Active Directory está em execução. Se o serviço está parado ou não foi configurado com valores de inicialização padrão, redefina os valores de inicialização padrão, reinicialize o controlador de domínio modificado e repita a operação.</para>
            </listItem>
            <listItem>
              <para>Verifique se o status de serviço e o valor de inicialização para o serviço RPC e Localizador RPC correto para a versão do sistema operacional do cliente de RPC (controlador de domínio de destino) e o servidor de RPC (controlador de domínio de origem). Se o serviço está parado ou não foi configurado com valores de inicialização padrão, redefina os valores de inicialização padrão, reinicialize o controlador de domínio modificado e repita a operação.</para>
              <para>Além disso, certifique-se de que o contexto de serviço coincide com configurações padrão listadas na tabela a seguir.</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>Fornecer manutenção</para>
                    </TD>
                    <TD>
                      <para>Status padrão (tipo de inicialização) no Windows Server 2003 e posterior </para>
                    </TD>
                    <TD>
                      <para>Status padrão (tipo de inicialização) no Windows Server 2000</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>Chamada de procedimento remoto</para>
                    </TD>
                    <TD>
                      <para>Iniciado (automático)</para>
                    </TD>
                    <TD>
                      <para>Iniciado (automático)</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>Localizador de chamada de procedimento remoto</para>
                    </TD>
                    <TD>
                      <para>Nulo ou interrompido (Manual)</para>
                    </TD>
                    <TD>
                      <para>Iniciado (automático)</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>Verifique se o tamanho do intervalo de porta dinâmica não tiver sido restrito. A sintaxe do Windows Server 2008 e Windows Server 2008 R2 NETSH para enumerar o intervalo de portas RPC é mostrada abaixo:</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>Verifique se que as definições de porta embutida definidas no KB 224196 caiam dentro do intervalo de porta dinâmica para a versão do sistema operacional de controladores de domínio de origem.</para>
              <para>Revisão <externalLink> <linkText>KB artigo 224196</linkText> <linkUri> https://support.microsoft.com/kb/224196 </linkUri> </externalLink> e certifique-se de que a porta embutida esteja dentro do intervalo de porta efêmera para a versão do sistema operacional do DC de origem.</para>
            </listItem>
            <listItem>
              <para>Verifique se a chave ClientProtocols existe sob HKLM\Software\Microsoft\Rpc e contém os seguintes valores padrão de 5:</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>Obter mais informações</title>
    <content>
      <para>
        <embeddedLabel>Exemplo de um nome inválido para IP mapeando causando erro de RPC de 1753 versus -2146893022: o nome da entidade de destino está incorreto</embeddedLabel>
      </para>
      <para>O domínio contoso.com consiste em DC1 e DC2 com x.x.1.1 de endereços IP e x.x.1.2. O host "A" registros "AAAA" para o DC2 estão corretamente registrado em todos os servidores DNS configurados para DC1. Além disso, o arquivo de HOSTS no DC1 contém uma entrada de mapeamento de nome de host totalmente qualificado de DC2s para x.x.1.2 de endereço IP. Posteriormente, alterações de endereço IP do DC2 de X.X.1.2 X.X.1.3 e um novo computador membro é ingressado no domínio com x.x.1.2 de endereço IP. Replicação do AD tentativas disparada pela <ui>Replicate now</ui> comando no snap-in Serviços e Sites do Active Directory falha com erro 1753, conforme mostrado no rastreamento abaixo:</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>No quadro <embeddedLabel>10</embeddedLabel>, o destino do controlador de domínio consulta o mapeador de ponto de extremidade de controladores de domínio de origem pela porta 135 para a classe de serviço de replicação do Active Directory UUID E351... </para>
      <para>No quadro <embeddedLabel>11</embeddedLabel>, a fonte de controlador de domínio, nesse caso, um computador que não hospeda a função de controlador de domínio ainda e, portanto, não registrou o E351 membro... UUID para o serviço de replicação com seu local EPM responde com o erro simbólico EP_S_NOT_REGISTERED que mapeia para o erro decimal 1753, erro hexa 0x6d9 e erro amigável "não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade".</para>
      <para>Posteriormente, o computador do membro com x.x.1.2 de endereço IP é promovido como uma réplica "MayberryDC" no domínio contoso.com. Novamente, o <ui>Replicate now</ui> comando é usado para disparar a replicação, mas desta vez falha com na tela erro "o nome da entidade de destino está incorreto." O computador cujo adaptador de rede é atribuído o IP endereço x.x.1.2 <placeholder>é</placeholder> um controlador de domínio, no momento é inicializado no modo normal e registrou o E351... o serviço de replicação UUID com EPM seu local, mas ele não possui a identidade de segurança ou o nome do DC2 e não é possível descriptografar a solicitação de Kerberos do DC1; portanto, a solicitação agora falhará com o erro "o nome da entidade de destino está incorreto". O erro é mapeado para o erro decimal -2146893022 / hex erro 0x80090322. </para>
      <para>Tais mapeamentos do IP do host inválidos pode ser causados por entradas obsoletas no host / arquivos Lmhosts, host um / registros AAAA no DNS ou WINS. </para>
      <para>Resumo: Este exemplo falhou porque um mapeamento de IP do host inválido (no arquivo de HOST, neste caso) causou o controlador de domínio de destino resolver a uma "fonte" controlador de domínio que não tem o Active Directory Domain Services serviço está em execução (ou até mesmo instaladas para esse fim) portanto, a replicação SPN ainda não foi registrado e o DC de origem retornou erro 1753. No segundo caso, um mapeamento de IP do host inválido (novamente no arquivo de HOST) causou o controlador de domínio para se conectar a um controlador de domínio que tinha registrado o E351 de destino... replicação SPN, mas essa fonte tinha uma identidade diferente do nome de host e de segurança que o controlador de domínio de origem pretendido para que as tentativas de falharam com erro -2146893022: O nome da entidade de destino está incorreto.</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Solucionando problemas de operações do Active Directory que falham com o erro 1753: Não há mais nenhum ponto de extremidade disponíveis do Mapeador do ponto de extremidade. </linkText> 
      <linkUri> https://support.microsoft.com/kb/2089874 </linkUri> 
    </externalLink> 
<externalLink> <linkText>839880 erros de mapeador de ponto de extremidade de RPC de solução de problemas usando as ferramentas de suporte do Windows Server 2003 do CD do produtodoartigoKB</linkText> <linkUri> https://support.microsoft.com/kb/839880 </linkUri> </externalLink> 
<externalLink> <linkText>832017 serviço rede e visão geral de requisitos de porta para o sistema Windows Server do artigo KB</linkText> <linkUri> https://support.microsoft.com/kb/832017/ </linkUri> </externalLink> 
<externalLink> <linkText>224196 tráfego de replicação de restringir o Active Directory e o tráfego RPC do cliente para uma porta específica do artigo KB</linkText> <linkUri> https://support.microsoft.com/kb/224196/ </linkUri> </externalLink> 
<externalLink> <linkText>KB artigo 154596 como configurar a alocação de porta dinâmica da RPC para funcionar com os firewalls</linkText> <linkUri> https://support.microsoft.com/kb/154596 </linkUri> </externalLink> <externalLink> <linkText>Como funciona o RPC</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>como o servidor prepara para uma Conexão</linkText> <linkUri> https://msdn.microsoft.com/library/aa373938(VS.85).aspx </linkUri> </externalLink> 
<externalLink> <linkText>Como o cliente estabelece uma Conexão</linkText> <linkUri> https://msdn.microsoft.com/library/aa373937(VS.85).aspx </linkUri> </externalLink> <externalLink> <linkText>Registrando a Interface</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>fazendo o servidor Disponível na rede</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>registrando pontos de extremidade</linkText> <linkUri> https://msdn.microsoft.com/library/aa375255(VS.85).aspx </linkUri> </externalLink> <externalLink> <linkText>Escuta de chamadas do cliente</linkText> <linkUri> https://msdn.microsoft.com/library/aa373966(VS.85).aspx </linkUri> </externalLink> <externalLink> <linkText>Como o cliente estabelece uma Conexão</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>restringindo Ativa o tráfego de replicação de diretório e RPC do cliente o tráfego para uma porta específica</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>SPN para um controlador de domínio de destino no AD DS</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


