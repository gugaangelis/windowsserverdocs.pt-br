---
title: Dnscmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39478e9b7dd8e8c69ed07f5d431486a7ed96b9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815497"
---
# <a name="dnscmd"></a>Dnscmd

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma interface de linha de comando para gerenciar servidores DNS. Esse utilitário é útil em arquivos de script em lotes para ajudar a automatizar tarefas de gerenciamento de DNS de rotina ou para executar uma instalação autônoma simples e a configuração de novos servidores DNS na sua rede.  
## <a name="syntax"></a>Sintaxe  
```  
dnscmd <ServerName> <command> [<command parameters>]  
```  
## <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<ServerName>|O IP endereço ou nome do host de um servidor DNS local ou remoto.|  
## <a name="commands"></a>Comandos  
|Comando|Descrição|  
|------|--------|  
|[dnscmd /ageallrecords](#BKMK_1)|Define a hora atual em todos os carimbos de hora em uma zona ou nó.|  
|[dnscmd /clearcache](#BKMK_2)|Limpa o cache do servidor DNS.|  
|[dnscmd /config](#BKMK_3)|Redefine a configuração do servidor ou zona DNS.|  
|[dnscmd /createbuiltindirectorypartitions](#BKMK_4)|cria as partições de diretório de aplicativo DNS internos.|  
|[dnscmd /createdirectorypartition](#BKMK_5)|cria uma partição de diretório de aplicativo.|  
|[dnscmd /deletedirectorypartition](#BKMK_6)|Exclui uma partição de diretório de aplicativo.|  
|[dnscmd /directorypartitioninfo](#BKMK_7)|lista informações sobre uma partição de diretório de aplicativo.|  
|[dnscmd /enlistdirectorypartition](#BKMK_8)|Adiciona um servidor DNS para o conjunto de replicação de uma partição de diretório de aplicativo.|  
|[DNSCmd /enumdirectorypartitions](#BKMK_9)|lista as partições de diretório de aplicativo do DNS para um servidor.|  
|[dnscmd /enumrecords](#BKMK_10)|lista os registros de recursos em uma zona.|  
|[DNSCmd /enumzones](#BKMK_11)|lista as zonas hospedadas pelo servidor especificado.|  
|[dnscmd /exportsettings](#BKMK_25a)|Grava informações de configuração de servidor em um arquivo de texto.|  
|[dnscmd /info](#BKMK_12)|Obtém informações do servidor.|  
|[dnscmd /ipvalidate](#BKMK_29a)|Valida os servidores DNS remotos.|  
|[dnscmd /nodedelete](#BKMK_13)|Exclui todos os registros para um nó em uma zona.|  
|[dnscmd /recordadd](#BKMK_14)|Adiciona um registro de recurso a uma zona.|  
|[dnscmd /recorddelete](#BKMK_15)|Remove um registro de recurso de uma zona.|  
|[dnscmd /resetforwarders](#BKMK_16)|Define os servidores DNS para encaminhar consultas recursivas.|  
|[dnscmd /resetlistenaddresses](#BKMK_17)|Define os endereços IP do servidor para atender às solicitações DNS.|  
|[dnscmd /startscavenging](#BKMK_18)|Inicia a eliminação do servidor.|  
|[/ DNSCmd estatísticas](#BKMK_19)|As consultas ou limpa os dados de estatísticas do servidor.|  
|[dnscmd /unenlistdirectorypartition](#BKMK_20)|Remove um servidor DNS do conjunto de replicação de uma partição de diretório de aplicativo.|  
|[dnscmd /writebackfiles](#BKMK_21)|Salva todos os dados de zona ou dica de raiz em um arquivo.|  
|[dnscmd /zoneadd](#BKMK_22)|cria uma nova zona no servidor DNS.|  
|[dnscmd /zonechangedirectorypartition](#BKMK_23)|Altera a partição de diretório no qual reside uma zona.|  
|[dnscmd /zonedelete](#BKMK_24)|Exclui uma zona do servidor DNS.|  
|[dnscmd /zoneexport](#BKMK_25)|Grava os registros de recursos de uma zona em um arquivo de texto.|  
|[dnscmd /zoneinfo](#BKMK_26)|Exibe informações sobre a zona.|  
|[dnscmd /zonepause](#BKMK_27)|pausa uma zona.|  
|[dnscmd /zoneprint](#BKMK_28)|Exibe todos os registros na zona.|  
|[dnscmd /zonerefresh](#BKMK_30)|força uma atualização da zona secundária da zona mestre.|  
|[dnscmd /zonereload](#BKMK_31)|Recarrega uma região do seu banco de dados.|  
|[dnscmd /zoneresetmasters](#BKMK_32)|Altera os servidores mestres que fornecem informações de transferência de zona para uma zona secundária.|  
|[dnscmd /zoneresetscavengeservers](#BKMK_33)|Altera os servidores que podem eliminar uma zona.|  
|[dnscmd /zoneresetsecondaries](#BKMK_34)|Redefine a informação secundária para uma zona.|  
|[dnscmd /zoneresettype](#BKMK_29)|Altera o tipo de zona.|  
|[dnscmd /zoneresume](#BKMK_35)|Retoma uma zona.|  
|[dnscmd /zoneupdatefromds](#BKMK_36)|Atualiza uma zona integrada do active directory com os dados dos serviços de domínio do active directory (AD DS).|  
|[dnscmd /zonewriteback](#BKMK_37)|Salva os dados da zona em um arquivo.|  
### <a name="BKMK_1"></a>dnscmd /ageallrecords  
Define a hora atual em um carimbo de hora nos registros de recursos em um nó em um servidor DNS ou a região especificada.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /ageallrecords <ZoneName>[<NodeName>] | [/tree]|[/f]  
```  
#### <a name="parameters"></a>Parâmetros  
<ServerName>  
Especifica o servidor DNS que o administrador planeja gerenciar, representado pelo endereço IP, o nome de domínio totalmente qualificado (FQDN) ou o nome do Host. Se esse parâmetro for omitido, o servidor local será usado.  
<ZoneName>  
Especifica o FQDN da zona.  
<NodeName>  
Especifica um nó específico ou uma subárvore na zona. *NodeName* Especifica o nó ou uma subárvore na zona usando o seguinte:  
-   @ para a zona raiz ou o FQDN  
-   O FQDN de um nó (o nome com um ponto (.) no final)  
-   Um único rótulo para o nome relativo à raiz de zona  
/Tree  
Especifica que todos os nós filho também recebem o carimbo de data / hora.  
/f  
Executa o comando sem pedir confirmação.  
#### <a name="remarks"></a>Comentários  
-   O **ageallrecords** comando é para fins de compatibilidade entre versões anteriores do DNS no qual duração e eliminação não tinham suporte e a versão atual do DNS. Ele adiciona um carimbo de data / hora com a hora atual para registros de recursos que não têm um carimbo de hora e define a hora atual em registros de recursos que têm um carimbo de data / hora.  
-   Limpeza de registro não ocorre, a menos que os registros são com carimbo de hora. Registros de recurso de início de registros de recursos de autoridade (SOA), do servidor (NS) de nomes e registros de recursos do serviço de nome na Internet do Windows (WINS) não estão incluídos no processo de eliminação e eles não são com carimbo de hora, mesmo quando o **ageallrecords**execuções de comando.  
-   Este comando falhará, a menos que a eliminação está habilitada para o servidor DNS e a zona. Para obter informações sobre como habilitar a eliminação da zona, consulte a **envelhecimento** parâmetro em sintaxe de nível de zona na [config](#BKMK_3) comando.  
-   A adição de um carimbo de hora para registros de recursos DNS torna incompatível com os servidores DNS que são executados em sistemas operacionais diferentes do Windows 2000, Windows XP ou Windows Server 2003. Um carimbo de hora que você adiciona usando o **ageallrecords** comando não pode ser revertido.  
-   Se nenhum dos parâmetros opcionais for especificado, o comando retorna todos os registros de recursos no nó especificado. Se for especificado um valor para pelo menos um dos parâmetros opcionais, **dnscmd** enumera apenas os registros de recursos que correspondem ao valor ou valores que são especificados no parâmetro opcional ou parâmetros.  
#### <a name="example"></a>Exemplo  
Consulte [exemplo 1: Definir a hora atual em um carimbo de hora para registros de recurso](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_2"></a>dnscmd /clearcache  
Limpa a memória de cache DNS de registros de recursos no servidor DNS especificado.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /clearcache  
```  
#### <a name="parameters"></a>Parâmetros  
<ServerName>  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
#### <a name="sample-usage"></a>Exemplo de uso  
`dnscmd dnssvr1.contoso.com /clearcache`  
### <a name="BKMK_3"></a>dnscmd /config  
Altera os valores no registro para o servidor DNS e zonas individuais. Aceita as configurações de nível de servidor e as configurações de nível de zona.  
> [!CAUTION]  
> Não edite o registro diretamente, a menos que você não tenha nenhuma alternativa. O editor do Registro ignora proteções padrão, permitindo configurações que podem prejudicar o desempenho, danificar o sistema ou até mesmo exigir a reinstalação do Windows. Você pode alterar a maioria das configurações de registro com segurança por meio de programas no painel de controle ou o Console de gerenciamento Microsoft (mmc). Se você deve editar o registro diretamente, faça backup primeiro. Ler a Ajuda do editor do registro para obter mais informações.  
#### <a name="server-level-syntax"></a>Sintaxe de nível de servidor  
```  
dnscmd [<ServerName>] /config <Parameter>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifica a configuração do servidor especificado.  
#### <a name="parameters"></a>Parâmetros  
<ServerName>  
Especifica o servidor DNS que você planeja gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
<Parameter>  
Especifique uma configuração e, como opção, um valor. Valores de parâmetro usam esta sintaxe: *Parâmetro* [*valor*]  
Os seguintes valores de parâmetro são descritos no restante desta seção:  
-   **/addressanswerlimit**  
-   **/bindsecondaries**  
-   **/bootmethod**  
-   **/defaultagingstate**  
-   **/defaultnorefreshinterval**  
-   **/defaultrefreshinterval**  
-   **/disableautoreversezones**  
-   **/disablensrecordsautocreation**  
-   **/dspollinginterval**  
-   **/dstombstoneinterval**  
-   **/ednscachetimeout**  
-   **/enablednsprobes**  
-   **/enablednssec**  
-   **/enableglobalnamessupport**  
-   **/enableglobalqueryblocklist**  
-   **/eventloglevel**  
-   **/forwarddelegations**  
-   **/forwardingtimeout**  
-   **/globalnamesqueryorder**  
-   **/globalqueryblocklist**  
-   **/isslave**  
-   **/localnetpriority**  
-   **/logfilemaxsize**  
-   **/logfilepath**  
-   **/logipfilterlist**  
-   **/loglevel**  
-   **/maxcachesize**  
-   **/maxcachettl**  
-   **/namecheckflag**  
-   **/notcp**  
-   **/norecursion**  
-   **/recursionretry**  
-   **/recursiontimeout**  
-   **/roundrobin**  
-   **/rpcprotocol**  
-   **/scavenginginterval**  
-   **/secureresponses**  
-   **/sendport**  
-   **/strictfileparsing**  
-   **/updateoptions**  
-   **/writeauthorityns**  
-   **/xfrconnecttimeout**  
**/addressanswerlimit [0 | 5-28]**  
Especifica o número máximo de registros de host que um servidor DNS pode enviar em resposta a uma consulta. O valor pode ser zero (0), ou ele pode estar no intervalo de 5 por meio de registros de 28. O valor padrão é zero (0).  
**/bindsecondaries[0|1]**  
Altera o formato da transferência de zona para que ela pode atingir eficiência e máximas de compactação. No entanto, esse formato não é compatível com versões anteriores de Berkeley Internet Name Domain (BIND).  
**0**  
Usa compactação máxima. Esse formato é compatível com versões de ligação 4.9.4 e versões posteriores.  
**1**  
Envia o registro de apenas um recurso por mensagem para os servidores DNS não - Microsoft. Esse formato é compatível com versões anteriores do BIND que 4.9.4. Essa é a configuração padrão.  
**/bootmethod [0 | 1 | 2 | 3].**  
Determina a origem da qual o servidor DNS obtém suas informações de configuração.  
**0**  
Limpa a origem das informações de configuração.  
**1**  
Carrega a partir do arquivo de associação que está localizado no diretório do DNS, que é %SystemRoot%\System32\Dns. por padrão.  
**2**  
Carrega a partir do registro.  
**3**  
Cargas de AD DS e o registro. Essa é a configuração padrão.  
**/defaultagingstate[0|1]**  
Determina se o recurso de eliminação de DNS é habilitado por padrão nas zonas recém-criado.  
**0**  
Desabilita a eliminação. Essa é a configuração padrão.  
**1**  
Habilita a eliminação.  
**/defaultnorefreshinterval[0x1-0xFFFFFFFF|0xA8]**  
Define um período de tempo em que as atualizações não são aceitos para registros atualizados dinamicamente. As zonas no servidor herdam esse valor automaticamente. Para alterar o valor padrão, digite um valor no intervalo de **0x1 0xFFFFFFFF**. O valor padrão do servidor é **0xA8**.  
**/defaultrefreshinterval [0xFFFFFFFF 0x1 | 0xA8]**  
Define um período de tempo permitido para atualizações dinâmicas para registros de DNS. As zonas no servidor herdam esse valor automaticamente. Para alterar o valor padrão, digite um valor no intervalo de **0x1 0xFFFFFFFF**. O valor padrão do servidor é **0xA8**.  
**/disableautoreversezones [0 | 1]**  
Habilita ou desabilita a criação automática de zonas de pesquisa inversa. Zonas de pesquisa inversa fornecem resolução de endereços IP (Internet Protocol) para nomes de domínio DNS.  
**0**  
Permite a criação automática de zonas de pesquisa inversa. Essa é a configuração padrão.  
**1**  
Desabilita a criação automática de zonas de pesquisa inversa.  
**/DisableNSRecordsAutoCreation {0 | 1}**  
Especifica se o servidor DNS cria automaticamente o servidor de nomes (NS) registros de recursos para as zonas que ele hospeda.  
**0**  
Automaticamente cria registros de recursos de NS (servidor) de nomes para as zonas que os hosts de servidor DNS.  
**1**  
Não criar automaticamente registros de recursos de NS (servidor) de nomes para as zonas que os hosts de servidor DNS.  
**/dspollinginterval 0 a 30**  
Especifica a frequência com que o DNS server sondar os AD DS para que as alterações no active directory de zonas integradas.  
**/dstombstoneinterval [1-30]**  
A quantidade de tempo em segundos para manter os registros excluídos no AD DS.  
**/ednscachetimeout [<seconds>]**  
Especifica o número de segundos em que informações estendidas de DNS (EDNS) são armazenados em cache. O valor mínimo é 3600 e o valor máximo é 15,724,800. O valor padrão é de 604.800 segundos (uma semana).  
**/enableednsprobes {0 | 1}**  
Habilita ou desabilita o servidor para outros servidores para determinar se eles dão suporte a EDNS de investigação.  
**0**  
Desabilita o suporte de Active Directory para investigações EDNS.  
**1**  
Habilita o suporte de Active Directory para investigações EDNS.  
**/enablednssec {0|1}**  
Habilita ou desabilita o suporte para extensões de segurança DNS (DNSSEC).  
**0**  
Desabilita o suporte a DNSSEC.  
**1**  
Habilita o suporte a DNSSEC.  
**/enableglobalnamessupport {0 | 1}**  
Habilita ou desabilita o suporte para a zona GlobalNames. A zona GlobalNames dá suporte a resolução de nomes DNS de rótulo único em uma floresta.  
**0**  
Desabilita o suporte para a zona GlobalNames. Quando você define o valor desse comando ao **0**, o serviço servidor DNS não resolve nomes de rótulo único na zona GlobalNames.  
**1**  
Habilita o suporte para a zona GlobalNames. Quando você define o valor desse comando ao **1**, o serviço servidor DNS resolve nomes de rótulo único na zona GlobalNames.  
**/enableglobalqueryblocklist {0|1}**  
Habilita ou desabilita o suporte para a lista de bloqueios de consulta global que bloqueia a resolução de nomes na lista. O serviço servidor DNS cria e ativa a lista global de consultas não autorizadas por padrão, quando o serviço é iniciado na primeira vez. Para exibir a lista atual de blocos de consulta global, use o **dnscmd /info /globalqueryblocklist** comando.  
**0**  
Desabilita o suporte para a lista global de consultas não autorizadas. Quando você define o valor desse comando ao **0**, o serviço servidor DNS responde a consultas de nomes na lista de bloqueios.  
**1**  
Habilita o suporte para a lista global de consultas não autorizadas. Quando você define o valor desse comando ao **1**, o serviço servidor DNS não responde a consultas de nomes na lista de bloqueios.  
**/EventLogLevel [0 | 1 | 2 | 4]**  
Determina quais eventos são registrados no log do servidor DNS no Visualizador de eventos.  
**0**  
Não registra nenhum evento.  
**1**  
Registra apenas erros.  
**2**  
Registra apenas erros e avisos.  
**4**  
Registra erros, avisos e eventos informativos. Essa é a configuração padrão.  
**/forwarddelegations [0 | 1]**  
Determina como o servidor DNS lida com uma consulta para uma subzona delegada. Essas consultas podem ser enviadas para a subzona que é referenciada na consulta ou para a lista de encaminhadores que é chamada para o servidor DNS. Entradas de configuração são usadas somente quando o encaminhamento está habilitado.  
**0**  
Envia automaticamente consultas que se referem a subzonas delegadas para a subzona apropriada. Essa é a configuração padrão.  
**1**  
encaminha consultas que fizerem referência a subzona delegada para os encaminhadores existentes.  
**/ForwardingTimeout [<seconds>]**  
Determina quantos segundos (0x1 0xFFFFFFFF) espera que um servidor DNS encaminhador responder antes de tentar outro encaminhador. O valor padrão é **0x5**, que é de 5 segundos.  
**/globalneamesqueryorder** {**0|1**}  
Especifica se o serviço servidor DNS procura primeiro na zona GlobalNames ou zonas locais quando ele resolver nomes.  
**0**  
O serviço servidor DNS tenta resolver nomes consultando a zona GlobalNames antes de consultar as zonas para as quais tem autoridade.  
**1**  
O serviço servidor DNS tenta resolver nomes consultando as zonas para as quais tem autoridade antes de consultar a zona GlobalNames.  
**/globalqueryblocklist[[<name> [<name>]...]**  
substitui a lista de bloqueios de consulta global atual com uma lista dos nomes que você especificar. Se você não especificar quaisquer nomes, este comando limpa a lista de bloqueios. Por padrão, a lista de bloqueios de consulta global contém os seguintes itens:  
-   isatap  
-   wpad  
O serviço servidor DNS pode remover um ou ambos esses nomes quando ele é iniciado na primeira vez, se ele encontrar esses nomes em uma zona existente.  
**/isslave [0 | 1]**  
Determina como o servidor DNS responde quando consultas que ele encaminha não recebem nenhuma resposta.  
**0**  
Especifica que o servidor DNS não é um subordinado (também conhecido como um *subordinado*). Se o encaminhador não responder, o servidor DNS tenta resolver a consulta em si. Essa é a configuração padrão.  
**1**  
Especifica que o servidor DNS é um subordinado. Se o encaminhador não responder, o servidor DNS encerra a pesquisa e envia uma mensagem de falha para o resolvedor.  
**/localnetpriority [0 | 1]**  
Determina a ordem na qual os registros de host são retornados quando o servidor DNS tiver vários registros de host para o mesmo nome.  
**0**  
Retorna os registros na ordem em que são listados no banco de dados DNS.  
**1**  
Retorna os registros que têm IP semelhante endereços de rede pela primeira vez. Essa é a configuração padrão.  
**/logfilemaxsize [<size>]**  
Especifica o tamanho máximo em bytes (0x10000 0xFFFFFFFF) do arquivo DNS. log. Quando o arquivo atingir seu tamanho máximo, o DNS substitui os eventos mais antigos. O tamanho padrão é 0x400000, que é 4 megabytes (MB).  
**/logfilepath [<path+LogFileName>]**  
Especifica o caminho do arquivo DNS. log. O caminho padrão é % systemroot%\System32\Dns\Dns.log. Você pode especificar um caminho diferente usando o formato *caminho + LogFileName*.  
**/logipfilterlist <IPaddress> [,<IPaddress>...]**  
Especifica quais pacotes são registrados no arquivo de log de depuração. As entradas são uma lista de endereços IP. Apenas os pacotes que vão de e para os endereços IP na lista são registrados.  
**/loglevel [<Eventtype>]**  
Determina quais tipos de eventos são registrados no arquivo de DNS. Cada tipo de evento é representado por um número hexadecimal. Se você quiser mais de um evento no log, use hexadecimal adição para adicionar os valores e, em seguida, insira a soma.  
**0x0**  
O servidor DNS não cria um log. Esta é a entrada padrão.  
**0x10**  
Registra as consultas.  
**0x10**  
Registra as notificações.  
**0x20**  
Registra as atualizações.  
**0xFE**  
Logs de transações nonquery.  
**0x100**  
Logs de transações da pergunta.  
**0x200**  
Registra as respostas.  
**0x1000**  
Logs de enviam pacotes.  
**0x2000**  
Logs de recebem pacotes.  
**0x4000**  
Registra pacotes do protocolo UDP (User Datagram).  
**0x8000**  
Registra pacotes do protocolo TCP (Transmission Control).  
**0xFFFF**  
Registra todos os pacotes.  
**0x10000**  
Logs de transações de gravação do Active Directory.  
**0x20000**  
Registra em log de transações de atualização do Active Directory.  
**0x1000000**  
Pacotes completos de logs.  
**0x80000000**  
Logs write-through transações.  
**/maxcachesize**  
Especifica o tamanho máximo, em quilobytes (KB), o servidor s memória do cache do DNS.  
**/maxcachettl [<seconds>]**  
Determina o número de segundos (0x0 0xFFFFFFFF) em um registro é salvo no cache. Se o **0x0** for usada, o servidor DNS não armazena em cache registros. A configuração padrão é **0x15180** (86.400 segundos ou 1 dia).  
**/maxnegativecachettl [<seconds>]**  
Especifica o número de segundos (0x1 0xFFFFFFFF) uma entrada que registra uma resposta negativa para uma consulta permanece armazenada no cache do DNS. A configuração padrão é **0x384** (900 segundos).  
**/namecheckflag [0 | 1 | 2 | 3].**  
Especifica qual padrão de caracteres é usado durante a verificação de nomes DNS.  
**0**  
Usa caracteres ANSI que estão em conformidade com a Internet Engineering Task forçam (IETF) Rfcs Request for Comments ().  
**1**  
Usa caracteres ANSI que necessariamente não são compatíveis com a IETF Rfcs.  
**2**  
Transformação de UCS multibyte usa formato 8 caracteres (UTF-8). Essa é a configuração padrão.  
**3**  
Usa todos os caracteres.  
**/norecursion [0 | 1]**  
Determina se um servidor DNS executa a resolução de nomes recursivos.  
**0**  
O servidor DNS executa a resolução de nomes recursivos se eles forem solicitados em uma consulta. Essa é a configuração padrão.  
**1**  
O servidor DNS não executa resolução de nomes recursivos.  
**/notcp**  
Esse parâmetro é obsoleto e não tem nenhum efeito nas versões atuais do Windows Server.  
**/recursionretry [<seconds>]**  
Determina o número de segundos (0x1 0xFFFFFFFF) que um servidor DNS aguarda antes de tentar novamente entre em contato com um servidor remoto. A configuração padrão é 0x3 (três segundos). Esse valor deve ser aumentado quando recursão ocorre em um link de rede de (longa distância WAN) lenta de longa distância.  
**/RecursionTimeout [<seconds>]**  
Determina o número de segundos (0x1 0xFFFFFFFF) que um servidor DNS deve aguardar antes de interromper tentativas de contatar um servidor remoto. As configurações variam de **0x1** por meio **0xFFFFFFFF**. A configuração padrão é **0xF** (15 segundos). Esse valor deve ser aumentado quando recursão ocorre em um link WAN lento.  
**/RoundRobin [0 | 1]**  
Determina a ordem na qual os registros de host são retornados quando um servidor tiver vários registros de host para o mesmo nome.  
0  
O servidor DNS não usa rodízio. Em vez disso, ele retorna o primeiro registro a cada consulta.  
**1**  
O servidor DNS gira entre os registros retornados da parte superior até a parte inferior da lista de registros correspondentes. Essa é a configuração padrão.  
**/RpcProtocol [0x0 | 0x1 | 0x2 | 0x4 | 0xFFFFFFFF]**  
Especifica o protocolo de chamada de procedimento remoto (RPC) usa quando ele faz uma conexão do servidor DNS.  
**0x0**  
Desabilita o RPC de DNS.  
**0x1**  
Usa o TCP/IP.  
**0x2**  
Usa pipes nomeadas.  
**0x4**  
Usa a chamada de procedimento local (LPC).  
**0xFFFFFFFF**  
Todos os protocolos. Essa é a configuração padrão.  
**/ScavengingInterval [<hours>]**  
Determina se o recurso de eliminação para o servidor DNS está habilitado e define o número de horas (0x0 0xFFFFFFFF) entre os ciclos de eliminação. A configuração padrão é **0x0**, que desabilita a eliminação para o servidor DNS. Uma configuração maior que **0x0** habilita a eliminação para o servidor e define o número de horas entre os ciclos de eliminação.  
**/secureresponses [0 | 1]**  
Determina se o DNS filtra os registros que são salvos em um cache.  
**0**  
Salva todas as respostas a consultas de nome de um cache. Essa é a configuração padrão.  
**1**  
Salva apenas os registros que pertencem à mesma subárvore DNS para um cache.  
**/SendPort [<port>]**  
Especifica o número da porta (0x0 0xFFFFFFFF) que usa o DNS para enviar consultas recursivas a outros servidores DNS. A configuração padrão é **0x0**, que significa que o número da porta é selecionado aleatoriamente.  
**/serverlevelplugindll[<Dllpath>]**  
Especifica o caminho de um plug-in personalizado. Quando *Dllpath* Especifica o nome de caminho totalmente qualificado de um servidor DNS válido plug-in, o servidor DNS chama funções no plug-in para resolver consultas de nomes que estão fora do escopo de todas as localmente hospedado zonas. Se um nome consultado está fora do escopo do plug-in, o servidor DNS executa resolução de nomes usando encaminhamento ou recursão, conforme configurado. Se *Dllpath* não for especificado, o servidor DNS deixa de usar um plug-in personalizado, se um plug-in personalizado foi configurado anteriormente.  
**/strictfileparsing [0 | 1]**  
Determina o comportamento do servidor DNS quando encontra um registro incorreto durante o carregamento de uma zona.  
**0**  
O servidor DNS continua a carregar a zona, mesmo se o servidor encontrar um registro incorreto. O erro é registrado no log de DNS. Essa é a configuração padrão.  
**1**  
O servidor DNS para carregar a zona, e ele registra o erro no log de DNS.  
**/UpdateOptions <RecordValue>**  
Proíbe atualizações dinâmicas de tipos especificados de registros. Se você quiser mais de um tipo de registro para ser proibido no log, use hexadecimal adição para adicionar os valores e, em seguida, insira a soma.  
**0x0**  
Não restringe quaisquer tipos de registro.  
**0x1**  
Exclui o início de registros de recursos de autoridade (SOA).  
**0x2**  
Exclui registros de recursos de NS (servidor) de nome.  
**0x4**  
Exclui a delegação de registros de recursos de NS (servidor) de nome.  
**0x8**  
Exclui registros de host do servidor.  
**0x100**  
Durante a atualização dinâmica segura, exclui o início de registros de recursos de autoridade (SOA).  
**0x200**  
Durante a atualização dinâmica segura, exclui registros de recursos de NS (servidor) de nome de raiz.  
**0x30F**  
Durante a atualização dinâmica padrão, exclui registros de recursos de NS (servidor) de nome, início de registros de recurso de autoridade (SOA) e registros de host do servidor. Durante a atualização dinâmica segura, exclui registros de recursos de NS (servidor) de nome raiz e o início de registros de recursos de autoridade (SOA). Permite que as delegações e o servidor de atualizações de host.  
**0x400**  
Durante a atualização dinâmica segura, exclui registros de recursos de NS (servidor) de nomes de delegação.  
**0x800**  
Durante a atualização dinâmica segura, exclui registros de host do servidor.  
**0x1000000**  
Exclui os registros de delegação DS (signatário).  
**0x80000000**  
Desabilita a atualização dinâmica de DNS.  
**/writeauthorityns [0 | 1]**  
Determina quando o servidor DNS grava registros de recursos de NS (servidor) de nomes na seção de autoridade de uma resposta.  
**0**  
Grava registros de recursos de NS (servidor) de nomes na seção de referências somente de autoridade. Essa configuração está em conformidade com Rfc 1.034, conceitos de nomes de domínio e instalações e com a Rfc 2181, esclarecimentos para a especificação de DNS. Essa é a configuração padrão.  
**1**  
Grava registros de recursos de NS (servidor) de nomes na seção de autoridade de todas as respostas de autorização bem-sucedida.  
**/xfrconnecttimeout [<seconds>]**  
Determina o número de segundos (0x0 0xFFFFFFFF) um servidor DNS primário aguarda uma resposta de transferência do servidor secundário. O valor padrão é **0x1E** (30 segundos). Depois que o valor de tempo limite expira, a conexão é encerrada.  
#### <a name="zone-level-syntax"></a>Sintaxe de nível de zona  
```  
dnscmd /config <Parameters>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifica a configuração da zona especificada.  
#### <a name="parameters"></a>Parâmetros  
**<Parameters>**  
Especifica uma configuração, uma zona, o nome e, como opção, um valor. Valores de parâmetro usam esta sintaxe: *Parâmetro ZoneName* [*valor*]  
Os seguintes valores de parâmetro são documentados no restante desta seção:  
-   **/aging**  
-   **/allownsrecordsautocreation**  
-   **/allowupdate**  
-   **/forwarderslave**  
-   **/forwardertimeout**  
-   **/norefreshinterval**  
-   **/refreshinterval**  
-   **/securesecondaries**  
**/aging <ZoneName>**  
Habilita ou desabilita a eliminação em uma zona específica.  
**/allownsrecordsautocreation <ZoneName> [<Value>]**  
Substitui nome NS (servidor) recursos registros criação automática do servidor DNS de configuração. Registros de recursos de NS (servidor) de nomes que foram registrados anteriormente para essa zona não são afetados. Portanto, você deve removê-los manualmente se não quiser que eles.  
**/allowupdate <ZoneName>**  
Determina se a região especificada aceita atualizações dinâmicas.  
**/forwarderslave <ZoneName>**  
Substitui o servidor DNS **/isslave** configuração.  
**/forwardertimeout <ZoneName>**  
Determina quantos segundos uma zona DNS aguarda um encaminhador responder antes de tentar outro encaminhador. Esse valor substitui o valor é definido no nível do servidor.  
**/NoRefreshInterval <ZoneName>**  
Define um intervalo de tempo para uma zona durante o qual não há atualizações podem atualizar dinamicamente registros DNS em uma região especificada.  
**/refreshinterval <ZoneName>**  
Define um intervalo de tempo para uma zona durante o qual as atualizações podem atualizar dinamicamente registros DNS em uma região especificada.  
**/securesecondaries <ZoneName>**  
Determina quais servidores secundários podem receber atualizações de zona do servidor mestre para esta zona.  
#### <a name="remarks"></a>Comentários  
-   O nome da zona deve ser especificado apenas para parâmetros de nível de zona.  
### <a name="BKMK_4"></a>dnscmd /createbuiltindirectorypartitions  
cria uma partição de diretório de aplicativo. Quando o DNS é instalado, uma partição de diretório de aplicativo para o serviço é criada nos níveis de floresta e domínio. Use este comando para criar partições de diretório de aplicativo DNS que foram excluídas ou nunca foi criadas. Sem nenhum parâmetro, esse comando cria uma partição de diretório interna do DNS para o domínio.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /createbuiltindirectorypartitions [/forest] [/alldomains]   
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**/forest**  
cria uma partição de diretório DNS da floresta.  
**/alldomains**  
cria partições DNS para todos os domínios na floresta.  
### <a name="BKMK_5"></a>dnscmd /createdirectorypartition  
cria uma partição de diretório de aplicativo. Quando o DNS é instalado, uma partição de diretório de aplicativo para o serviço é criada nos níveis de floresta e domínio. Esta operação cria partições de diretório de aplicativo DNS adicionais.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /createdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<PartitionFQDN>**  
O FQDN da partição de diretório de aplicativo a DNS que será criado.  
### <a name="BKMK_6"></a>dnscmd /deletedirectorypartition  
Remove uma partição de diretório de aplicativo DNS existente.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /deletedirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<PartitionFQDN>**  
O FQDN da partição de diretório de aplicativo a DNS que será removido.  
### <a name="BKMK_7"></a>dnscmd /directorypartitioninfo  
lista informações sobre uma partição de diretório de aplicativo DNS especificada.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /directorypartitioninfo <PartitionFQDN> [/detail]   
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<PartitionFQDN>**  
O FQDN da partição de diretório de aplicativo DNS.  
**/detail**  
lista todas as informações sobre a partição de diretório de aplicativo.  
### <a name="BKMK_8"></a>dnscmd /enlistdirectorypartition  
Adiciona o servidor DNS para o conjunto de réplicas da partição de diretório especificado.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /enlistdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<PartitionFQDN>**  
O FQDN da partição de diretório de aplicativo DNS.  
### <a name="BKMK_9"></a>DNSCmd /enumdirectorypartitions  
lista as partições de diretório de aplicativos do DNS para o servidor especificado.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /enumdirectorypartitions [/custom]   
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**/custom**  
lista apenas as partições de diretório criados pelo usuário.  
### <a name="BKMK_10"></a>dnscmd /enumrecords  
lista os registros de recursos de um nó especificado em uma zona DNS.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /enumrecords <ZoneName> <NodeName> [/type <RRtype> <Rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<ChildName>] [/continue | /detail]   
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS que você planeja gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**/enumrecords**  
lista os registros de recursos na zona especificada.  
**<ZoneName>**  
Especifica o nome da zona à qual pertencem os registros de recursos.  
**<NodeName>**  
Especifica o nome do nó dos registros de recursos.  
**/tipo <RRtype> <Rrdata>**  
Especifica o tipo de registro de recurso a ser listado e o tipo de dados que são esperados:  
**<RRtype>**  
Especifica o tipo de registro de recurso a ser listado.  
**<Rrdata>**  
Especifica o tipo de dados de registro esperado.  
**/authority**  
Inclui dados autoritativos.  
**/glue**  
Inclui os dados de associação.  
**/additional**  
Inclui todas as informações adicionais sobre os registros de recursos listados.  
**{/node | /child | /startchild <ChildName>}**  
Filtra ou adiciona informações para a exibição do registro de recurso:  
**/node**  
lista apenas os registros de recursos do nó especificado.  
**/child**  
lista apenas os registros de recursos de um domínio filho especificado.  
**/startchild <ChildName>**  
Inicia a lista no domínio filho especificado.  
**/continue | /detail**  
Especifica como os dados retornados são exibidos.  
**/continue**  
lista apenas os registros de recursos com seu tipo e dados.  
**/detail**  
lista todas as informações sobre os registros de recursos.  
#### <a name="sample-usage"></a>Exemplo de uso  
`dnscmd /enumrecords test.contoso.com test /additional`  
### <a name="BKMK_11"></a>DNSCmd /enumzones  
lista as zonas que existem no servidor DNS especificado.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <PartitionFQDN>]  
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**/primary | /secondary | /forwarder | /stub | /cache | /auto-created**  
Filtra os tipos de zonas a serem exibidos:  
**/primary**  
lista todas as zonas que são zonas primárias padrão ou zonas integrada ao active directory.  
**/secondary**  
lista todas as zonas secundárias padrão.  
**/forwarder**  
lista as zonas que encaminham consultas não resolvidas para outro servidor DNS.  
**/stub**  
lista todas as zonas de stub.  
**/cache**  
lista apenas as zonas que são carregadas no cache.  
**/auto-created**  
lista as zonas que foram criadas automaticamente durante a instalação do servidor DNS.  
**/forward | /reverse | /ds | /file**  
Especifica filtros adicionais dos tipos de zonas a serem exibidos:  
**/forward**  
listas de zonas de pesquisa direta.  
**/reverse**  
listas de zonas de pesquisa inversa.  
**/ds**  
lista de zonas integrada ao active directory.  
**/file**  
lista as zonas que são apoiadas por arquivos.  
**/domaindirectorypartition**  
lista as zonas que são armazenadas na partição de diretório do domínio.  
**/forestdirectorypartition**  
lista as zonas que são armazenadas na floresta de partição de diretório de aplicativo.  
**/customdirectorypartition**  
lista todas as zonas que são armazenadas em uma partição de diretório de aplicativo definidas pelo usuário.  
**/legacydirectorypartition**  
lista todas as zonas são armazenadas na partição de diretório do domínio.  
**/DirectoryPartition <PartitionFQDN>**  
lista todas as zonas são armazenadas na partição de diretório especificado.  
#### <a name="remarks"></a>Comentários  
-   O **enumzones** parâmetros atuam como filtros na lista de zonas. Se nenhum filtro for especificado, uma lista completa das regiões será retornada. Quando um filtro for especificado, apenas as zonas que atendem aos critérios do filtro são incluídas na lista retornada de zonas.  
#### <a name="example"></a>Exemplo  
Consulte [exemplo 2: Exibir uma lista completa das zonas em um servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemplo 3: Exibir uma lista de zonas criado automaticamente em um servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25a"></a>dnscmd /exportsettings  
cria um arquivo de texto que lista os detalhes de configuração de um servidor DNS. O arquivo de texto chamado DnsSettings.txt. Ele está localizado na pasta %systemroot%\system32\dns do servidor.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /exportsettings   
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
#### <a name="remarks"></a>Comentários  
-   Você pode usar as informações no arquivo que **/exportsettings dnscmd** cria para solucionar problemas de configuração ou para garantir que você tenha configurado vários servidores de forma idêntica.  
### <a name="BKMK_12"></a>dnscmd /info  
Exibe as configurações da seção de DNS do registro do servidor especificado: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters**  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /info [<Setting>]  
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<Setting>**  
Qualquer configuração que o **info** comando retorna pode ser especificada individualmente. Se uma configuração não for especificada, será retornado um relatório de configurações comuns.  
#### <a name="remarks"></a>Comentários  
-   Esse comando exibe as configurações do registro no nível do servidor DNS. Para exibir as configurações do registro de nível de zona, use o [zoneinfo](#BKMK_26) comando. Para ver uma lista de configurações que podem ser exibidos com este comando, consulte o [config](#BKMK_3) descrição.  
#### <a name="example"></a>Exemplo  
Consulte [exemplo 4: Exibir a configuração de um servidor DNS de IsSlave](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemplo 5: Exibir a configuração de um servidor DNS de Recursiontimeout](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_29a"></a>dnscmd /ipvalidate  
Testa se um endereço IP identifica um servidor DNS em funcionamento ou se o servidor DNS pode agir como um encaminhador, um servidor de dica de raiz ou um servidor mestre para uma zona específica.  
#### <a name="syntax"></a>Sintaxe  
```  
dnscmd [<ServerName>] /ipvalidate <Context> [<ZoneName>] [[<IPaddress>]]  
```  
#### <a name="parameters"></a>Parâmetros  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
**<Context>**  
Especifica o tipo de teste para executar. Você pode especificar qualquer um dos seguintes testes:  
-   **/dnsservers** testa se os computadores com os endereços que você especificar estão funcionando servidores DNS.  
-   **/forwarders** testes que os endereços que você especificar identificam os servidores DNS que podem atuar como encaminhadores.  
-   **/roothints** testes que os endereços que você especificar identificam os servidores DNS que podem atuar como servidores de nomes de dica de raiz.  
-   **/zonemasters** testes que os endereços que você especificar servidores DNS que são servidores mestres para identificam *ZoneName*.  
**<ZoneName>**  
Identifica a zona. Use este parâmetro com o **/zonemasters** parâmetro.  
**<IPaddress>**  
Especifica os endereços IP que o comando testa.  
#### <a name="sample-usage"></a>Exemplo de uso  
<pre>dnscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2  
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2</pre>  
### <a name="BKMK_13"></a>dnscmd /nodedelete  
Exclui todos os registros para um host especificado. # # # Sintaxe ```  
dnscmd [<ServerName>] /nodedelete <ZoneName> <NodeName> [/tree] [/f] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona.  
**<NodeName>**  
Especifica o nome do host do nó a excluir.  
**/tree**  
Exclui todos os registros filho.  
**/f**  
executa o comando sem pedir confirmação. # # # Consulte o exemplo [exemplo 6: excluir os registros de um nó](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_14"></a>dnscmd /recordadd  
Adiciona um registro a uma região especificada em um servidor DNS. #### Syntax ```  
dnscmd [<ServerName>] /recordadd <ZoneName> <NodeName> <RRtype> <Rrdata> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica a zona na qual reside o registro.  
**<NodeName>**  
Especifica um nó específico na zona.  
**<RRtype>**  
Especifica o tipo de registro a ser adicionado.  
**<Rrdata>**  
Especifica o tipo de dados que são esperados.  
> [!NOTE]  
Quando você adiciona um registro, certifique-se de que você use o formato de dados e tipo de dados correto. Para obter uma lista de tipos de registro de recurso e os tipos de dados apropriado, consulte [registros de recursos de referência](https://technet.microsoft.com/library/cc758321(v=ws.10).aspx). # # # De exemplo de uso
<pre>dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5  
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com</pre>  
### <a name="BKMK_15"></a>dnscmd /recorddelete  
Exclui um registro de recurso de uma região especificada. #### Syntax ```  
dnscmd <ServerName> /recorddelete <ZoneName> <NodeName> <RRtype> <Rrdata>[/f] ```  
#### Parameters  
**<ServerName>**  
> Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica a zona na qual reside o registro de recurso.  
**<NodeName>**  
Especifica o nome do host.  
**<RRtype>**  
Especifica o tipo de registro de recurso a ser excluído.  
**<Rrdata>**  
Especifica o tipo de dados que são esperados.  
**/f**  
executa o comando sem pedir confirmação:-porque nós podem ter mais de um registro de recurso, este comando requer que você seja muito específico sobre o tipo de registro de recurso que você deseja excluir. – Se você especificar um tipo de dados e você não especificar um tipo de dados de registro de recurso, todos os registros com esse tipo de dados específico para o nó especificado serão excluídos. # # # De exemplo de uso `dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com`  
### <a name="BKMK_16"></a>dnscmd /resetforwarders  
Seleciona ou redefine os endereços IP para o qual o servidor DNS encaminha consultas DNS quando ele não é possível resolvê-los localmente. #### Syntax ```  
dnscmd [<ServerName>] /resetforwarders [<IPaddress> [,<IPaddress>]...][/timeout <timeOut>] [/slave|/noslave] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<IPaddress>**  
lista os endereços IP para o qual o servidor DNS encaminha consultas não resolvidas.  
**/tempo limite <timeOut>**  
Define o número de segundos que o servidor DNS aguarda uma resposta do encaminhador. Por padrão, esse valor é de cinco segundos.  
**/slave|/noslave**  
Determina se o servidor DNS executa suas próprias consultas interativas, se o encaminhador não conseguir resolver uma consulta:  
**/slave**  
Impede que o servidor DNS execute suas próprias consultas interativas, se o encaminhador não conseguir resolver uma consulta.  
**/noslave**  
Permite que o servidor DNS executar suas próprias consultas interativas, se o encaminhador não conseguir resolver uma consulta. Essa é a configuração padrão. # # # Comentários - por padrão, um servidor DNS executa consultas iterativas quando ele não pode resolver uma consulta. – Configurando endereços IP usando o **resetforwarders** comando faz com que o servidor DNS executar consultas recursivas a servidores de DNS de endereços IP especificados. Se os encaminhadores não resolver a consulta, o servidor DNS, em seguida, pode executar suas próprias consultas interativas. -Se a **servidorsubordinado/** parâmetro é usado, o servidor DNS não executa suas próprias consultas interativas. Isso significa que o servidor DNS encaminha consultas não resolvidas somente para os servidores DNS na lista, e ele não tente consultas iterativas se encaminhadores não resolvê-los. Ele é mais eficiente para definir um endereço IP como um encaminhador para um servidor DNS. Você pode usar o **resetforwarders** comando para servidores internos em uma rede para encaminhar suas consultas não resolvidas para um servidor DNS que tenha uma conexão externa. -a listagem de um endereço IP do encaminhador duas vezes faz com que o servidor DNS tentar encaminhar para esse servidor duas vezes. # # # De exemplo de uso
<pre>dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave  
dnscmd dnssvr1.contoso.com /resetforwarders /noslave</pre>  
### <a name="BKMK_17"></a>dnscmd /resetlistenaddresses  
Especifica os endereços IP em um servidor que escuta solicitações de cliente DNS. # # # Sintaxe ```  
dnscmd [<ServerName>] /resetlistenaddresses [<listenaddress>] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<listenaddress>**  
Especifica um endereço IP no servidor DNS que escuta solicitações de cliente DNS. Se nenhum endereço de escuta for especificado, todos os endereços IP no servidor escutam solicitações de cliente. # # # Comentários - por padrão, todos os endereços IP em um servidor DNS escutam solicitações de cliente DNS. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1`  
### <a name="BKMK_18"></a>dnscmd /startscavenging  
Informa a um servidor DNS, tente uma pesquisa de imediata para registros de recursos obsoletos em um servidor DNS especificado. # # # Sintaxe ```  
dnscmd [<ServerName>] /startscavenging ```  
#### Parameter  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado. # # # Comentários - conclusão bem-sucedida desse comando inicia um eliminar imediatamente. -Embora o comando para iniciar a eliminar pareça ser concluída com êxito, a eliminar não for iniciado, a menos que as pré-condições a seguir forem atendidas:-eliminação está habilitada para o servidor e a zona. -A zona é iniciada. -Os registros de recursos tem um carimbo de data / hora. -Para obter informações sobre como habilitar a eliminação para o servidor, consulte o **scavenginginterval** parâmetro em sintaxe de nível de servidor na [config](#BKMK_3) seção. -Para obter informações sobre como habilitar a eliminação da zona, consulte a **envelhecimento** parâmetro em sintaxe de nível de zona na [config](#BKMK_3) seção. -Para obter informações sobre como iniciar uma zona que está em pausa, consulte o [zoneresume](#BKMK_35) seção. -Para obter informações sobre como verificar registros de recursos para um carimbo de hora, consulte a [ageallrecords](#BKMK_1) seção. -Se a eliminar falhar, nenhuma mensagem de aviso será exibida. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /startscavenging`  
### <a name="BKMK_19"></a>/ DNSCmd estatísticas  
Exibe ou limpa os dados para um servidor DNS especificado. # # # Sintaxe ```  
dnscmd [<ServerName>] / estatísticas [<StatID>] [/ limpar] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<StatID>**  
Especifica qual estatística ou uma combinação de estatísticas a serem exibidas. Um número de identificação é usado para identificar uma estatística. Se nenhum número de identificação de estatística é especificado, exibem todas as estatísticas.  
A seguir está uma lista de números que podem ser especificados e a estatística correspondente que exibe:  
**00000001**  
time  
**00000002**  
consulta  
**00000004**  
query2  
**00000008**  
Recurse  
**00000010**  
Mestre  
**00000020**  
Secundário  
**00000040**  
WINS  
**00000100**  
Atualização  
**00000200**  
SkwanSec  
**00000400**  
Ds  
**00010000**  
Memória  
**00100000**  
PacketMem  
**00040000**  
DBASE  
**00080000**  
Registros  
**00200000**  
NbstatMem  
**/clear**  
Redefine o contador de estatística especificado como zero. # # # Comentários - a **estatísticas** comando exibe os contadores que começam no servidor DNS quando ele é iniciado ou retomado. # # # Consulte exemplos de [exemplo 7: Exibir estatísticas de tempo para um servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemplo 8: Exibir estatísticas de NbstatMem para um servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_20"></a>dnscmd /unenlistdirectorypartition  
Remove o servidor DNS de conjunto de réplicas da partição de diretório especificado. # # # Sintaxe ```  
dnscmd [<ServerName>] /unenlistdirectorypartition <PartitionFQDN> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<PartitionFQDN>**  
O FQDN da partição de diretório de aplicativo a DNS que será removido.  
### <a name="BKMK_21"></a>dnscmd /writebackfiles  
Verifica a memória do servidor DNS para que as alterações e os grava no armazenamento persistente. # # # Sintaxe ```  
dnscmd [<ServerName>] /writebackfiles [<ZoneName>] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona a ser atualizada. # # # Comentários - a **writebackfiles** comando atualiza todas as zonas de falta de limpeza ou de uma região especificada. Uma zona está suja quando há alterações na memória que ainda não foram gravadas no armazenamento persistente. Essa é uma operação de nível de servidor que verifica todas as regiões. Você pode especificar uma zona nessa operação ou você pode usar o [zonewriteback](#BKMK_37) operação. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /writebackfiles`  
### <a name="BKMK_22"></a>dnscmd /zoneadd  
Adiciona uma zona para o servidor DNS. # # # Sintaxe ```  
dnscmd [<ServerName>] /zoneadd <ZoneName> <Zonetype> [/dp <FQDN>| {/ domínio | / enterprise | / herdados}] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona.  
**<Zonetype>**  
Especifica o tipo de zona a ser criada. Cada tipo de zona tem diferentes parâmetros obrigatórios:  
**/dsprimary**  
cria uma zona integrada do active directory.  
**//file primário <FileName>**  
cria uma zona primária padrão e especifica o nome do arquivo que armazenará as informações de zona.  
**/secundário <MasterIPaddress> [<MasterIPaddress>...]**  
cria uma zona secundária padrão.  
**/stub <MasterIPaddress> [<MasterIPaddress>...] de arquivos <FileName>**  
cria uma zona de stub de backup em arquivo.  
**//DsStub <MasterIPaddress> [<MasterIPaddress>...]**  
cria uma zona de stub integrada do active directory.  
**/encaminhador <MasterIPaddress> [<MasterIPaddress>]... / arquivo <FileName>**  
Especifica que a zona criada encaminha consultas não resolvidas para outro servidor DNS.  
**/dsforwarder**  
Especifica que a zona integrada ao diretório do Active Directory criado encaminha consultas não resolvidas para outro servidor DNS.  
**/dp <FQDN> {/domain | /enterprise | /legacy}**  
Especifica a partição de diretório no qual armazenar a zona.  
**<FQDN>**  
Especifica o FQDN da partição de diretório.  
**/domain**  
Armazena a zona na partição de diretório de domínio.  
**/enterprise**  
Armazena a zona na partição de diretório corporativo.  
**/legacy**  
Armazena a zona em uma partição de diretório herdado. # # # Comentários - especificando um tipo de zona de **/forwarder** ou **/dsforwarder** cria uma zona que executa o encaminhamento condicional. # # # De exemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary  
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_23"></a>dnscmd /zonechangedirectorypartition  
Altera a partição de diretório no qual reside a região especificada. # # # Sintaxe ```  
dnscmd [<ServerName>] /zonechangedirectorypartition <ZoneName>] {[<NewPartitionName>] | [<Zonetype>] } ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
O FQDN da partição de diretório atual no qual reside a zona.  
**<NewPartitionName>**  
O FQDN da partição de diretório será movida para a zona.  
**<Zonetype>**  
Especifica o tipo de partição de diretório será movida para a zona.  
**/domain**  
Move a zona para a partição de diretório de domínio interno.  
**/forest**  
Move a zona para a partição de diretório de floresta interna.  
**/legacy**  
Move a zona para a partição de diretório é criada para controladores de domínio do Active Directory versão anterior. Essas partições de diretório não são necessárias para o modo nativo.  
### <a name="BKMK_24"></a>dnscmd /zonedelete  
Exclui uma região especificada. #### Syntax ```  
dnscmd [<ServerName>] /zonedelete <ZoneName> [/dsdel] [/f] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona a ser excluído.  
**/dsdel**  
Exclui a zona do AD DS.  
**/f**  
Executa o comando sem pedir confirmação. # # # Consulte o exemplo [exemplo 9: excluir uma zona de um servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25"></a>dnscmd /zoneexport  
cria um arquivo de texto que lista os registros de recursos de uma região especificada. # # # Sintaxe de ' dnscmd [<ServerName>] /zoneexport <ZoneName> <ZoneExportFile>' # # # parâmetros **<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona.  
**<ZoneExportFile>**  
Especifica o nome do arquivo a ser criado. # # # Comentários - a **zoneexport** operação cria um arquivo de registros de recursos para uma zona integrada do active directory para fins de solução de problemas. Por padrão, o arquivo que este comando cria é colocado no diretório do DNS, que é, por padrão, o diretório %systemroot%/System32/Dns. # # # Consulte o exemplo [exemplo 10: Exportar lista de registros de recurso de zona para um arquivo](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_26"></a>dnscmd /zoneinfo  
Exibe as configurações da seção do registro da zona especificada: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>** # # # sintaxe ```  
dnscmd [<ServerName>] /zoneinfo <ZoneName> [<Setting>] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona.  
**<Setting>**  
Você pode especificar individualmente qualquer configuração que o **zoneinfo** comando retorna. Se você não especificar uma configuração, todas as configurações serão retornadas. # # # Comentários - a **zoneinfo** comando exibe as configurações do registro na zona DNS níveis em **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>**. -Para exibir as configurações do registro de nível de servidor, use o [info](#BKMK_12) comando. -Para ver uma lista de configurações que você pode exibir com este comando, consulte o [config](#BKMK_3) comando. # # # Consulte o exemplo [exemplo 11: Exibe a configuração de RefreshInterval do registro](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemplo 12: Exibir configuração do registro de classificação por vencimento](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_27"></a>dnscmd /zonepause  
pausa a zona especificada, o que, em seguida, ignora solicitações de consulta. # # # Sintaxe ```  
dnscmd [<ServerName>] /zonepause <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona a ser pausado. # # # Comentários - retomar uma zona e disponibilizá-lo depois que ele foi pausado, use o [zoneresume](#BKMK_35) comando. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /zonepause test.contoso.com`  
### <a name="BKMK_28"></a>dnscmd /zoneprint  
lista os registros em uma zona. # # # Sintaxe ```  
dnscmd [<ServerName>] /zoneprint <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Identifica a zona a ser listado.  
### <a name="BKMK_30"></a>dnscmd /zonerefresh  
força uma zona DNS secundária para atualizar a partir da zona principal. #### Syntax ```  
dnscmd <ServerName> /zonerefresh <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona a ser atualizada. # # # Comentários - a **zonerefresh** comando força uma verificação do número de versão no início de registro de recurso de autoridade (SOA) s do servidor mestre. Se o número de versão no servidor mestre é maior do que o número de versão do servidor secundário, uma transferência de zona é iniciada que atualiza o servidor secundário. Se o número de versão é o mesmo, ocorre a transferência de zona. – A verificação forçada ocorre por padrão a cada 15 minutos. Para alterar o padrão, use o **dnscmd config refreshinterval** comando. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com`  
### <a name="BKMK_31"></a>dnscmd /zonereload  
Copia informações de sua origem de zona. # # # Sintaxe de ```  
dnscmd <ServerName> /zonereload <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona a ser recarregado. # # # Comentários - se a zona é integrada ao active directory, ele recarrega do AD DS. -Se a zona é uma zona padrão de backup em arquivo, ele recarregará de um arquivo. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /zonereload test.contoso.com`  
### <a name="BKMK_32"></a>dnscmd /zoneresetmasters  
Redefine os endereços IP do servidor mestre que fornece informações de transferência de zona para uma zona secundária. # # # Sintaxe de ```  
dnscmd <ServerName> /zoneresetmasters <ZoneName> [/ local] [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona a ser recarregado.  
**/local**  
Define uma lista mestre local. Esse parâmetro é usado para zonas integrada ao active directory.  
**<IPaddress>**  
Os endereços IP dos servidores mestres da zona secundária. # # # Comentários - esse valor é definido originalmente quando a zona secundária é criada. Use o **zoneresetmasters** comando no servidor secundário. Esse valor não tem efeito se ele for definido no servidor DNS mestre. # # # De exemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1  
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local</pre>  
### <a name="BKMK_33"></a>dnscmd /zoneresetscavengeservers  
Altera os endereços IP dos servidores que podem eliminar a zona especificada. # # # Sintaxe ```  
dnscmd [<ServerName>] /zoneresetscavengeservers <ZoneName> [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Identifica a região para eliminar.  
**<IPaddress>**  
lista os endereços IP dos servidores que podem executar a eliminar. Se esse parâmetro for omitido, todos os servidores que hospedam essa zona poderá eliminá-lo. # # # Comentários - por padrão, todos os servidores que hospedam uma zona podem eliminar dessa zona. -Se uma zona é hospedada em mais de um servidor DNS, você pode usar esse comando para reduzir o número de vezes que uma zona é eliminada. -Eliminação deve estar habilitado no servidor DNS e zona que é afetada por este comando. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2`  
### <a name="BKMK_34"></a>dnscmd /zoneresetsecondaries  
Especifica uma lista de endereços IP dos servidores secundários para o qual um servidor mestre responde quando ele é solicitado para uma transferência de zona. # # # Sintaxe ```  
dnscmd [<ServerName>] /zoneresetsecondaries <ZoneName> {/ noxfr | / seguras | /securens | /securelist <SecurityIPaddresses>} {/ nonotify | / notificar | /notifylist <NotifyIPaddresses>} ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se o é o parâmetro for omitido, o servidor local é usado.  
**<ZoneName>**  
Especifica o nome da zona que terá seus servidores secundários redefinir.  
**/noxfr | / não seguras | /securens | /SecureList <SecurityIPaddresses>**  
Especifica se todos ou apenas alguns dos servidores secundários solicitando uma atualização de obtenção de uma atualização.  
**/noxfr**  
Especifica que nenhuma transferência de zona é permitida.  
**/nonsecure**  
Especifica que todas as solicitações de transferência de zona são concedidas.  
**/securens**  
Especifica que somente o servidor que está listado no registro de recurso do servidor (NS) de nome para a zona é concedido a uma transferência.  
**/securelist**  
Especifica que as transferências de zona são concedidas apenas à lista de servidores. Esse parâmetro deve ser seguido por um endereço IP ou endereços que usa o servidor mestre.  
**<SecurityIPaddresses>**  
lista os endereços IP que recebem transferências de zona do servidor mestre. Esse parâmetro é usado apenas com o **/securelist** parâmetro.  
**/nonotify | / Notificar | /notifylist <NotifyIPaddresses>**  
Especifica que uma notificação de alteração é enviada somente para determinados servidores secundários:  
**/nonotify**  
Especifica que nenhuma notificação de alteração é enviadas para os servidores secundários.  
**/notify**  
Especifica que as notificações de alteração são enviadas para todos os servidores secundários.  
**/notifylist**  
Especifica que as notificações de alteração sejam enviadas para apenas a lista de servidores. Esse comando deve ser seguido por um endereço IP ou endereços que usa o servidor mestre.  
**<NotifyIPaddresses>**  
Especifica o endereço ou endereços IP do servidor secundário ou servidores que mudam de notificações são enviadas. Essa lista é usada apenas com o **/notifylist** parâmetro. # # # Comentários - Use o **zoneresetsecondaries** comando no servidor mestre para especificar como ele responde a solicitações de transferência de zona de servidores secundários. # # # De exemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify  
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2</pre>  
### <a name="BKMK_29"></a>dnscmd /zoneresettype  
Altera o tipo da zona. #### Syntax ```  
dnscmd [<ServerName>] /zoneresettype <ZoneName> <Zonetype> [/overwrite_mem | /overwrite_ds] ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome de host. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Identifica a zona em que o tipo será alterado.  
**<Zonetype>**  
Especifica o tipo de zona a ser criada. Cada tipo tem diferentes parâmetros obrigatórios:  
**/dsprimary**  
cria uma zona integrada do active directory.  
**//file primário <FileName>**  
cria uma zona primária padrão.  
**/secundário <MasterIPaddress> [,<MasterIPaddress>...]**  
cria uma zona secundária padrão.  
**/stub <MasterIPaddress>[,<MasterIPaddress>...] de arquivos <FileName>**  
cria uma zona de stub de backup em arquivo.  
**//DsStub <MasterIPaddress>[,<MasterIPaddress>...]**  
cria uma zona de stub integrada do active directory.  
**/encaminhador <MasterIPaddress[,<MasterIPaddress>]... / arquivo<FileName>**  
Especifica que a zona criada encaminha consultas não resolvidas para outro servidor DNS.  
**/dsforwarder**  
Especifica que a zona integrada ao diretório do Active Directory criado encaminha consultas não resolvidas para outro servidor DNS.  
**/overwrite_mem | /overwrite_ds**  
Especifica como substituir dados existentes:  
**/overwrite_mem**  
Substitui os dados DNS dos dados no AD DS.  
**/overwrite_ds**  
Substitui os dados existentes no AD DS. # # # Comentários - configurar a zona de tipo como **/dsforwarder** cria uma zona que executa o encaminhamento condicional. # # # De exemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns  
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_35"></a>dnscmd /zoneresume  
inicia uma região especificada que anteriormente estava em pausa. #### Syntax ```  
dnscmd <ServerName> /zoneresume <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona para retomar. # # # Comentários - você pode usar essa operação para reverter a [zonepause](#BKMK_27) operação. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com`  
### <a name="BKMK_36"></a>dnscmd /zoneupdatefromds  
Atualiza a zona integrada ao Active Directory especificado do AD DS. # # # Sintaxe de ```  
dnscmd <ServerName> /zoneupdatefromds <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona para atualizar. # # # Comentários - active directory integradas zonas executam essa atualização por padrão a cada cinco minutos. Para alterar esse parâmetro, use o **dnscmd config dspollinginterval** comando. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /zoneupdatefromds`  
### <a name="BKMK_37"></a>dnscmd /zonewriteback  
Verifica a memória do servidor DNS para que as alterações que são relevantes para uma região especificada e os grava no armazenamento persistente. # # # Sintaxe de ```  
dnscmd <ServerName> /zonewriteback <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica o servidor DNS para gerenciar, representado pelo nome do host, FQDN ou endereço IP. Se esse parâmetro for omitido, o servidor local será usado.  
**<ZoneName>**  
Especifica o nome da zona para atualizar. # # # Comentários – essa é uma operação de nível de zona. Você pode atualizar todas as zonas em um servidor DNS com o [writebackfiles](#BKMK_21) operação. # # # De exemplo de uso `dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com`  
