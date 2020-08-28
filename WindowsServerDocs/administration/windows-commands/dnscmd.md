---
title: dnscmd
description: Artigo de referência para o comando dnscmd, que é uma interface de linha de comando para gerenciar servidores DNS.
ms.topic: reference
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e79d2cc2d5d014db197ab4bbeb0b24ff70fd145
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030804"
---
# <a name="dnscmd"></a>Dnscmd

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma interface de linha de comando para gerenciar servidores DNS. Esse utilitário é útil para gerar scripts de arquivos em lote para ajudar a automatizar tarefas rotineiras de gerenciamento de DNS ou para executar uma instalação autônoma simples e configuração de novos servidores DNS em sua rede.

## <a name="syntax"></a>Sintaxe

```
dnscmd <servername> <command> [<command parameters>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<servername>` | O endereço IP ou nome de host de um servidor DNS remoto ou local. |

## <a name="dnscmd-ageallrecords-command"></a>comando/AgeAllRecords do DNSCmd

Define a hora atual em um carimbo de data/hora nos registros de recursos em uma zona ou nó especificado em um servidor DNS.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /ageallrecords <zonename>[<nodename>] | [/tree]|[/f]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS que o administrador planeja gerenciar, representado pelo endereço IP, FQDN (nome de domínio totalmente qualificado) ou nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o FQDN da zona. |
| `<nodename>` | Especifica um nó ou subárvore específico na zona, usando o seguinte:<ul><li>**@** para a zona raiz ou FQDN</li><li>O FQDN de um nó (o nome com um ponto (.) no final)</li><li>Um único rótulo para o nome relativo à raiz da zona.</li></ul> |
| /tree | Especifica que todos os nós filho também recebem o carimbo de data/hora. |
| /f | Executa o comando sem pedir confirmação. |

##### <a name="remarks"></a>Comentários

- O comando **AgeAllRecords** destina-se à compatibilidade com versões anteriores entre a versão atual do DNS e as versões antigas do DNS nas quais a duração e a eliminação não têm suporte. Ele adiciona um carimbo de data/hora com a hora atual aos registros de recursos que não têm um carimbo de data/hora e define a hora atual nos registros de recursos que têm um carimbo de data/hora.

- A eliminação de registros não ocorre, a menos que os registros estejam com carimbo de data/hora. Registros de recursos de servidor de nomes (NS), registros de recursos de início de autoridade (SOA) e registros de recursos do WINS (serviço de cadastramento na Internet do Windows) não estão incluídos no processo de eliminação e não são marcados com o tempo mesmo quando o comando **AgeAllRecords** é executado.

- Esse comando falhará, a menos que a eliminação esteja habilitada para o servidor DNS e a zona. Para obter informações sobre como habilitar a eliminação para a zona, consulte o parâmetro **Aging** , dentro da sintaxe do `dnscmd /config` comando neste artigo.

- A adição de um carimbo de data/hora aos registros de recursos DNS os torna incompatíveis com os servidores DNS executados em sistemas operacionais diferentes do Windows Server. Um carimbo de data/hora adicionado usando o comando **AgeAllRecords** não pode ser revertido.

- Se nenhum dos parâmetros opcionais for especificado, o comando retornará todos os registros de recursos no nó especificado. Se um valor for especificado para pelo menos um dos parâmetros opcionais, o **dnscmd** enumerará somente os registros de recursos que correspondem ao valor ou aos valores especificados no parâmetro ou parâmetros opcionais.

### <a name="examples"></a>Exemplos

[Exemplo 1: definir a hora atual em um carimbo de data/hora para registros de recursos](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-1-set-the-current-time-on-a-time-stamp-to-resource-records)

## <a name="dnscmd-clearcache-command"></a>comando/ClearCache do DNSCmd

Limpa a memória cache do DNS dos registros de recursos no servidor DNS especificado.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /clearcache
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |

### <a name="example"></a>Exemplo

```
dnscmd dnssvr1.contoso.com /clearcache
```

## <a name="dnscmd-config-command"></a>comando dnscmd/config

Altera os valores no registro para o servidor DNS e zonas individuais. Esse comando também modifica a configuração do servidor especificado. Aceita configurações de nível de servidor e de zona.

> [!CAUTION]
> Não edite o registro diretamente, a menos que você não tenha nenhuma alternativa. O editor do registro ignora as proteções padrão, permitindo que as configurações possam prejudicar o desempenho, danificar o sistema ou até mesmo exigir que você reinstale o Windows. Você pode alterar com segurança a maioria das configurações do registro usando os programas no painel de controle ou no console de gerenciamento Microsoft (MMC). Se você precisar editar o registro diretamente, faça o backup primeiro. Leia a ajuda do editor do registro para obter mais informações.

### <a name="server-level-syntax"></a>Sintaxe de nível de servidor

```
dnscmd [<servername>] /config <parameter>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS que você pretende gerenciar, representado pela sintaxe do computador local, endereço IP, FQDN ou nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<parameter>` | Especifique uma configuração e, como uma opção, um valor. Os valores de parâmetro usam esta sintaxe: *parâmetro* [*Value*]. |
| /addressanswerlimit`[0|5-28]` | Especifica o número máximo de registros de host que um servidor DNS pode enviar em resposta a uma consulta. O valor pode ser zero (0) ou pode estar no intervalo de 5 a 28 registros. O valor padrão é zero (0). |
| /bindsecondaries`[0|1]` | Altera o formato da transferência de zona para que ela possa atingir a máxima compactação e eficiência. Aceita os valores:<ul><li>**0** -usa compactação máxima e é compatível com as versões de ligação 4.9.4 e posteriores somente</li><li>**1** -envia apenas um registro de recurso por mensagem para servidores DNS que não são da Microsoft e é compatível com versões de ligação anteriores à 4.9.4. Essa é a configuração padrão.</li></ul> |
| /bootmethod`[0|1|2|3]` | Determina a origem da qual o servidor DNS obtém suas informações de configuração. Aceita os valores:<ul><li>**0** -limpa a fonte de informações de configuração.</li><li>**1** -carrega do arquivo de associação que está localizado no diretório DNS, que é `%systemroot%\System32\DNS` por padrão.</li><li>**2** -cargas do registro.</li><li>**3** -cargas de AD DS e do registro. Essa é a configuração padrão.</li></ul> |
| /defaultagingstate`[0|1]` | Determina se o recurso de limpeza de DNS está habilitado por padrão em zonas recém-criadas. Aceita os valores:<ul><li>**0** -desabilita a eliminação. Essa é a configuração padrão.</li><li>**1** -habilita a eliminação.</li></ul> |
| /defaultnorefreshinterval`[0x1-0xFFFFFFFF|0xA8]` | Define um período de tempo no qual nenhuma atualização é aceita para registros atualizados dinamicamente. As zonas no servidor herdam esse valor automaticamente.<p>Para alterar o valor padrão, digite um valor no intervalo de **0x1-0xFFFFFFFF**. O valor padrão do servidor é **0xA8**. |
| /defaultrefreshinterval `[0x1-0xFFFFFFFF|0xA8]` | Define um período de tempo permitido para atualizações dinâmicas para registros DNS. As zonas no servidor herdam esse valor automaticamente.<p>Para alterar o valor padrão, digite um valor no intervalo de **0x1-0xFFFFFFFF**. O valor padrão do servidor é **0xA8**. |
| /disableautoreversezones `[0|1]` | Habilita ou desabilita a criação automática de zonas de pesquisa inversa. As zonas de pesquisa inversa fornecem a resolução de endereços IP para nomes de domínio DNS. Aceita os valores:<ul><li>**0** – habilita a criação automática de zonas de pesquisa inversa. Essa é a configuração padrão.</li><li>**1** -desabilita a criação automática de zonas de pesquisa inversa.</li></ul> |
| /disablensrecordsautocreation `[0|1]` | Especifica se o servidor DNS cria automaticamente registros de recurso de servidor de nome (NS) para as zonas que hospeda. Aceita os valores:<ul><li>**0** -cria automaticamente registros de recurso de servidor de nomes (ns) para zonas que o servidor DNS hospeda.</li><li>**1** -não cria automaticamente registros de recurso de servidor de nomes (ns) para zonas que o servidor DNS hospeda.</li></ul> |
| /dspollinginterval `[0-30]` | Especifica com que frequência o servidor DNS sonda AD DS para alterações nas zonas integradas do Active Directory. |
| /dstombstoneinterval `[1-30]` |A quantidade de tempo, em segundos, para reter registros excluídos em AD DS. |
| /ednscachetimeout `[3600-15724800]` | Especifica o número de segundos que as informações de DNS estendido (EDNS) são armazenadas em cache. O valor mínimo é **3600**e o valor máximo é **15.724.800**. O valor padrão é **604.800** segundos (uma semana). |
| /enableednsprobes `[0|1]` | Habilita ou desabilita o servidor para investigar outros servidores para determinar se eles dão suporte ao EDNS. Aceita os valores:<ul><li>**0** -desabilita o suporte ativo para investigações de EDNS.</li><li>**1** -habilita o suporte ativo para investigações de EDNS.</li></ul> |
| /enablednssec `[0|1]` | Habilita ou desabilita o suporte para o DNSSEC (extensões de segurança do DNS). Aceita os valores:<ul><li>**0** -DESABILITA o DNSSEC.</li><li>**1** – HABILITA o DNSSEC.</li></ul> |
| /enableglobalnamessupport `[0|1]` | Habilita ou desabilita o suporte para a zona GlobalNames. A zona GlobalNames dá suporte à resolução de nomes DNS de rótulo único em uma floresta. Aceita os valores:<ul><li>**0** -desabilita o suporte para a zona GlobalNames. Quando você define o valor desse comando como 0, o serviço do servidor DNS não resolve nomes de rótulo único na zona GlobalNames.</li><li>**1** – habilita o suporte para a zona GlobalNames. Quando você define o valor desse comando como 1, o serviço do servidor DNS resolve nomes de rótulo único na zona GlobalNames.</li></ul> |
| /enableglobalqueryblocklist `[0|1]` | Habilita ou desabilita o suporte para a lista de blocos de consulta global que bloqueia a resolução de nomes de nome na lista. O serviço do servidor DNS cria e habilita a lista de blocos de consulta global por padrão quando o serviço é iniciado pela primeira vez. Para exibir a lista de blocos de consulta global atual, use o comando dnscmd/info **/GlobalQueryBlockList** . Aceita os valores:<ul><li>**0** -desabilita o suporte para a lista de blocos de consulta global. Quando você define o valor desse comando como 0, o serviço do servidor DNS responde às consultas de nomes na lista de blocos.</li><li>**1** – habilita o suporte para a lista de blocos de consulta global. Quando você define o valor desse comando como 1, o serviço do servidor DNS não responde a consultas de nomes na lista de blocos.</li></ul> |
| /eventloglevel `[0|1|2|4]` | Determina quais eventos são registrados no log do servidor DNS no Visualizador de Eventos. Aceita os valores:<ul><li>**0** -logs sem eventos.</li><li>**1** -registra apenas os erros.</li><li>**2** -registra apenas erros e avisos.</li><li>**4** -registra erros, avisos e eventos informativos. Essa é a configuração padrão.</li></ul> |
| /forwarddelegations `[0|1]` | Determina como o servidor DNS manipula uma consulta para uma subzona delegada. Essas consultas podem ser enviadas para a subzona que é referenciada na consulta ou à lista de encaminhadores que é nomeada para o servidor DNS. As entradas na configuração são usadas somente quando o encaminhamento está habilitado. Aceita os valores:<ul><li>**0** – envia automaticamente consultas que se referem a subzonas delegadas à subzona apropriada. Essa é a configuração padrão.</li><li>**1** – encaminha consultas que se referem à subzona delegada aos encaminhadores existentes.</li></ul> |
| /forwardingtimeout `[<seconds>]` | Determina por quantos segundos (**0x1-0xFFFFFFFF**) um servidor DNS aguarda um encaminhador responder antes de tentar outro encaminhador. O valor padrão é **0x5**, que é de 5 segundos. |
| /globalneamesqueryorder `[0|1]` | Especifica se o serviço do servidor DNS parece primeiro na zona GlobalNames ou nas zonas locais ao resolver nomes. Aceita os valores:<ul><li>**0** -o serviço do servidor DNS tenta resolver nomes consultando a zona GlobalNames antes de consultar as zonas para as quais ele é autoritativo.</li><li>**1** -o serviço do servidor DNS tenta resolver nomes consultando as zonas para as quais ele é autoritativo antes de consultar a zona GlobalNames.</li></ul> |
| /globalqueryblocklist`[[<name> [<name>]...]` | Substitui a lista de blocos de consulta global atual por uma lista dos nomes que você especificar. Se você não especificar nenhum nome, esse comando limpará a lista de blocos. Por padrão, a lista de blocos de consulta global contém os seguintes itens:<ul><li>ISATAP</li><li>proxy</li></ul>O serviço do servidor DNS pode remover um ou ambos os nomes quando ele for iniciado pela primeira vez, se encontrar esses nomes em uma zona existente. |
| /isslave `[0|1]` | Determina como o servidor DNS responde quando as consultas que ele encaminha não recebem nenhuma resposta. Aceita os valores:<ul><li>**0** -especifica que o servidor DNS não é subordinado. Se o encaminhador não responder, o servidor DNS tentará resolver a consulta em si. Essa é a configuração padrão.</li><li>**1** -especifica que o servidor DNS é um subordinado. Se o encaminhador não responder, o servidor DNS finalizará a pesquisa e enviará uma mensagem de falha para o resolvedor.</li></ul> |
| /localnetpriority `[0|1]` | Determina a ordem na qual os registros de host são retornados quando o servidor DNS tem vários registros de host para o mesmo nome. Aceita os valores:<ul><li>**0** -retorna os registros na ordem em que estão listados no banco de dados DNS.</li><li>**1** -retorna os registros que têm endereços de rede IP semelhantes primeiro. Essa é a configuração padrão.</li></ul> |
| /logfilemaxsize `[<size>]` | Especifica o tamanho máximo em bytes (**0x10000-0xFFFFFFFF**) do arquivo DNS. log. Quando o arquivo atinge seu tamanho máximo, o DNS substitui os eventos mais antigos. O tamanho padrão é **0x400000**, que é 4 megabytes (MB). |
| /LogFilePath `[<path+logfilename>]` | Especifica o caminho do arquivo DNS. log. O caminho padrão é `%systemroot%\System32\Dns\Dns.log`. Você pode especificar um caminho diferente usando o formato `path+logfilename` . |
| /logipfilterlist `<IPaddress> [,<IPaddress>...]` | Especifica quais pacotes são registrados no arquivo de log de depuração. As entradas são uma lista de endereços IP. Somente os pacotes que vão para e para os endereços IP na lista são registrados. |
| /loglevel `[<eventtype>]` | Determina quais tipos de eventos são registrados no arquivo DNS. log. Cada tipo de evento é representado por um número hexadecimal. Se você quiser mais de um evento no log, use adição hexadecimal para adicionar os valores e, em seguida, insira a soma. Aceita os valores:<ul><li>**0x0** -o servidor DNS não cria um log. Essa é a entrada padrão.</li><li>**0x10** -registra consultas e notificações.</li><li>**0x20** – registra atualizações.</li><li>**0xFE** -registra transações de não consulta.</li><li>**0x100** -registra as transações de pergunta.</li><li>**0x200** – respostas de logs.</li><li>**0x1000** -logs enviam pacotes.</li><li>**0x2000** -logs recebem pacotes.</li><li>**0x4000** -registra os pacotes UDP (User Datagram Protocol).</li><li>**0x8000** -registra pacotes TCP (Transmission Control Protocol).</li><li>**0xFFFF** -registra todos os pacotes.</li><li>**0x10000** -registra as transações de gravação do Active Directory.</li><li>**0x20000** -registra transações de atualização do Active Directory.</li><li>**0x1000000** -registra os pacotes completos.</li><li>**0x80000000** -registra transações de write-through.</li><li></ul> |
| /maxcachesize | Especifica o tamanho máximo, em kilobytes (KB), do cache de memória s do servidor DNS. |
| /maxcachettl `[<seconds>]` | Determina por quantos segundos (**0x0-0xFFFFFFFF**) um registro é salvo no cache. Se a configuração **0x0** for usada, o servidor DNS não armazenará em cache os registros. A configuração padrão é **0x15180** (86.400 segundos ou 1 dia). |
| /maxnegativecachettl `[<seconds>]` | Especifica o número de segundos (**0x1-0xFFFFFFFF**) que uma entrada que registra uma resposta negativa para uma consulta permanece armazenada no cache DNS. A configuração padrão é **0x384** (900 segundos). |
| /namecheckflag `[0|1|2|3]` | Especifica qual caractere padrão é usado ao verificar nomes DNS. Aceita os valores:<ul><li>**0** -usa caracteres ANSI que estão em conformidade com a solicitação de comentários (IETF) de comentário da Internet Engineering Task Force.</li><li>**1** -usa caracteres ANSI que não necessariamente estão em conformidade com as RFCs da IETF.</li><li>**2** -usa caracteres de formato de transformação UCS 8 (UTF-8) multibyte. Essa é a configuração padrão.</li><li>**3** -usa todos os caracteres.</li></ul> |
| /norecursion `[0|1]` | Determina se um servidor DNS executa a resolução de nomes recursivos. Aceita os valores:<ul><li>**0** -o servidor DNS executa a resolução de nomes recursiva se for solicitado em uma consulta. Essa é a configuração padrão.</li><li>**1** -o servidor DNS não executa a resolução de nome recursivo.</li></ul> |
| /notcp | Esse parâmetro é obsoleto e não tem efeito nas versões atuais do Windows Server. |
| /recursionretry `[<seconds>]` | Determina o número de segundos (**0x1-0xFFFFFFFF**) que um servidor DNS espera antes de tentar entrar novamente em contato com um servidor remoto. A configuração padrão é **0x3** (três segundos). Esse valor deve ser aumentado quando a recursão ocorre em um link de WAN (rede de longa distância) lento. |
| /recursiontimeout `[<seconds>]` | Determina o número de segundos (**0x1-0xFFFFFFFF**) que um servidor DNS espera antes de interromper as tentativas de entrar em contato com um servidor remoto. As configurações variam de **0x1** a **0xFFFFFFFF**. A configuração padrão é **0xF** (15 segundos). Esse valor deve ser aumentado quando a recursão ocorre em um link WAN lento. |
| /roundrobin `[0|1]` | Determina a ordem na qual os registros de host são retornados quando um servidor tem vários registros de host para o mesmo nome. Aceita os valores:<ul><li>**0** -o servidor DNS não usa Round Robin. Em vez disso, ele retorna o primeiro registro para cada consulta.</li><li>**1** -o servidor DNS gira entre os registros que ele retorna da parte superior para a parte inferior da lista de registros correspondentes. Essa é a configuração padrão.</li></ul> |
| /rpcprotocol `[0x0|0x1|0x2|0x4|0xFFFFFFFF]` | Especifica o protocolo que a chamada de procedimento remoto (RPC) usa quando faz uma conexão do servidor DNS. Aceita os valores:<ul><li>**0x0** -DESABILITA o RPC para DNS.</li><li>**0x01** – usa TCP/IP</li><li>**0x2** – usa pipes nomeados.</li><li>**0x4** -usa a LPC (chamada de procedimento local).</li><li>**0xFFFFFFFF** -todos os protocolos. Essa é a configuração padrão.</li></ul> |
| /scavenginginterval `[<hours>]` | Determina se o recurso de limpeza do servidor DNS está habilitado e define o número de horas (**0x0-0xFFFFFFFF**) entre os ciclos de eliminação. A configuração padrão é **0x0**, que desabilita a eliminação do servidor DNS. Uma configuração maior que **0x0** permite a eliminação do servidor e define o número de horas entre os ciclos de eliminação. |
| /secureresponses `[0|1]` | Determina se o DNS filtra os registros que são salvos em um cache. Aceita os valores:<ul><li>**0** -salva todas as respostas a consultas de nome em um cache. Essa é a configuração padrão.</li><li>**1** -salva somente os registros que pertencem à mesma subárvore DNS a um cache.</li></ul> |
| /sendport `[<port>]` | Especifica o número da porta (**0x0-0xFFFFFFFF**) que o DNS usa para enviar consultas recursivas para outros servidores DNS. A configuração padrão é **0x0**, o que significa que o número da porta é selecionado aleatoriamente. |
| /serverlevelplugindll`[<dllpath>]` | Especifica o caminho de um plug-in personalizado. Quando DllPath especifica o nome do caminho totalmente qualificado de um plug-in válido do servidor DNS, o servidor DNS chama funções no plug-in para resolver consultas de nome que estão fora do escopo de todas as zonas hospedadas localmente. Se um nome consultado estiver fora do escopo do plug-in, o servidor DNS executará a resolução de nomes usando encaminhamento ou recursão, conforme configurado. Se DllPath não for especificado, o servidor DNS deixará de usar um plug-in personalizado se um plug-in personalizado tiver sido configurado anteriormente. |
| /strictfileparsing `[0|1]` | Determina o comportamento de um servidor DNS quando ele encontra um registro errôneo ao carregar uma zona. Aceita os valores:<ul><li>**0** -o servidor DNS continua a carregar a zona mesmo que o servidor encontre um registro errôneo. O erro é registrado no log DNS. Essa é a configuração padrão.</li><li>**1** -o servidor DNS para de carregar a zona e registra o erro no log DNS.</li></ul> |
| /updateoptions `<RecordValue>` | Proíbe atualizações dinâmicas de tipos especificados de registros. Se você quiser que mais de um tipo de registro seja proibido no log, use adição hexadecimal para adicionar os valores e, em seguida, insira a soma. Aceita os valores:<ul><li>**0x0** -não restringe nenhum tipo de registro.</li><li>**0x1** -exclui registros de recursos de início de autoridade (SOA).</li><li>**0x2** -exclui registros de recursos do servidor de nomes (ns).</li><li>**0x4** -exclui a delegação de registros de recursos do servidor de nomes (ns).</li><li>**0x8** -exclui os registros de host do servidor.</li><li>**0x100** -durante a atualização dinâmica segura, o exclui registros de recursos de início de autoridade (SOA).</li><li>**0x200** -durante a atualização dinâmica segura, o exclui os registros de recurso do servidor de nomes raiz (ns).</li><li>**0x30F** -durante a atualização dinâmica padrão, o exclui registros de recursos do servidor de nomes (ns), registros de recursos de início de autoridade (SOA) e registros de host do servidor. Durante a atualização dinâmica segura, o exclui os registros de recurso do servidor de nomes raiz (NS) e os registros de recurso de início de autoridade (SOA). Permite delegações e atualizações de host do servidor.</li><li>**0x400** -durante a atualização dinâmica segura, o exclui os registros de recurso do servidor de nomes de delegação (ns).</li><li>**0x800** -durante a atualização dinâmica segura, o exclui os registros de host do servidor.</li><li>**0x1000000** -exclui registros DS (signatário de delegação).</li><li>**0x80000000** -desabilita a atualização dinâmica do DNS.</li></ul> |
| /writeauthorityns `[0|1]` | Determina quando o servidor DNS grava os registros de recurso do servidor de nomes (NS) na seção autoridade de uma resposta. Aceita os valores:<ul><li>**0** -grava os registros de recurso do servidor de nome (ns) na seção autoridade apenas de referências. Essa configuração está em conformidade com RFC 1034, conceitos de nomes de domínio e instalações e com RFC 2181, esclarecimentos para a especificação de DNS. Essa é a configuração padrão.</li><li>**1** -grava registros de recurso do servidor de nomes (ns) na seção autoridade de todas as respostas autoritativas bem-sucedidas.</li></ul> |
| /xfrconnecttimeout `[<seconds>]` | Determina o número de segundos (**0x0-0xFFFFFFFF**) que um servidor DNS primário aguarda uma resposta de transferência de seu servidor secundário. O valor padrão é **0x1E** (30 segundos). Depois que o valor de tempo limite expirar, a conexão será encerrada. |

### <a name="zone-level-syntax"></a>Sintaxe de nível de zona

Modifica a configuração da zona especificada. O nome da zona deve ser especificado somente para parâmetros de nível de zona.

```
dnscmd /config <parameters>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<parameter>` | Especifique uma configuração, um nome de zona e, como uma opção, um valor. Os valores de parâmetro usam esta sintaxe: `zonename parameter [value]` . |
| /aging `<zonename>`| Habilita ou desabilita a limpeza em uma zona específica. |
| /allownsrecordsautocreation `<zonename>``[value]` | Substitui a configuração de autocriação de registro de recurso do servidor de nomes do servidor DNS (NS). Os registros de recursos do servidor de nomes (NS) que foram registrados anteriormente para essa zona não são afetados. Portanto, você deve removê-los manualmente se não quiser. |
| /allowupdate `<zonename>` | Determina se a zona especificada aceita atualizações dinâmicas. |
| /forwarderslave `<zonename>` | Substitui a configuração de **/isslave** do servidor DNS. |
| /forwardertimeout `<zonename>` | Determina por quantos segundos uma zona DNS aguarda um encaminhador responder antes de tentar outro encaminhador. Esse valor substitui o valor definido no nível do servidor. |
| /norefreshinterval `<zonename>` | Define um intervalo de tempo para uma zona durante a qual nenhuma atualização pode atualizar dinamicamente os registros DNS em uma zona especificada. |
| /refreshinterval `<zonename>` | Define um intervalo de tempo para uma zona durante a qual as atualizações podem atualizar dinamicamente os registros DNS em uma zona especificada. |
| /securesecondaries `<zonename>` | Determina quais servidores secundários podem receber atualizações de zona do servidor mestre para esta zona. |

## <a name="dnscmd-createbuiltindirectorypartitions-command"></a>comando/CreateBuiltinDirectoryPartitions do DNSCmd

Cria uma partição de diretório de aplicativo DNS. Quando o DNS é instalado, uma partição de diretório de aplicativo para o serviço é criada nos níveis de floresta e domínio. Use este comando para criar partições de diretório de aplicativo DNS que foram excluídas ou nunca criadas. Sem nenhum parâmetro, esse comando cria uma partição de diretório DNS interna para o domínio.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /createbuiltindirectorypartitions [/forest] [/alldomains]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| /Forest | Cria uma partição de diretório DNS para a floresta. |
| /alldomains | Cria partições DNS para todos os domínios na floresta. |

## <a name="dnscmd-createdirectorypartition-command"></a>comando/createdirectorypartition do DNSCmd

Cria uma partição de diretório de aplicativo DNS. Quando o DNS é instalado, uma partição de diretório de aplicativo para o serviço é criada nos níveis de floresta e domínio. Esta operação cria partições de diretório de aplicativo DNS adicionais.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /createdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<partitionFQDN>` | O FQDN da partição de diretório do aplicativo DNS que será criada. |

## <a name="dnscmd-deletedirectorypartition-command"></a>comando/deletedirectorypartition do DNSCmd

Remove uma partição de diretório de aplicativo DNS existente.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /deletedirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<partitionFQDN>` | O FQDN da partição de diretório do aplicativo DNS que será removida. |

## <a name="dnscmd-directorypartitioninfo-command"></a>comando/directorypartitioninfo do DNSCmd

Lista informações sobre uma partição de diretório de aplicativo DNS especificada.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /directorypartitioninfo <partitionFQDN> [/detail]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<partitionFQDN>` | O FQDN da partição de diretório do aplicativo DNS. |
| /detail | Lista todas as informações sobre a partição de diretório de aplicativos. |

## <a name="dnscmd-enlistdirectorypartition-command"></a>comando/enlistdirectorypartition do DNSCmd

Adiciona o servidor DNS ao conjunto de réplicas da partição de diretório especificada.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /enlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<partitionFQDN>` | O FQDN da partição de diretório do aplicativo DNS. |

## <a name="dnscmd-enumdirectorypartitions-command"></a>comando/EnumDirectoryPartitions do DNSCmd

Lista as partições de diretório de aplicativo DNS para o servidor especificado.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /enumdirectorypartitions [/custom]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| /custom | Lista somente partições de diretório criadas pelo usuário. |

## <a name="dnscmd-enumrecords-command"></a>comando/enumrecords do DNSCmd

Lista os registros de recursos de um nó especificado em uma zona DNS.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /enumrecords <zonename> <nodename> [/type <rrtype> <rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<childname>] [/continue | /detail]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| /enumrecords | Lista os registros de recursos na zona especificada. |
| `<zonename>` | Especifica o nome da zona à qual os registros de recursos pertencem. |
| `<nodename>` | Especifica o nome do nó dos registros de recursos. |
| `[/type <rrtype> <rrdata>]` | Especifica o tipo de registros de recursos a serem listados e o tipo de dados que é esperado. Aceita os valores:<ul><li>`<rrtype>` -Especifica o tipo de registros de recursos a serem listados.</li><li>`<rrdata>` -Especifica o tipo de dados que é o registro esperado.</li></ul> |
| /authority | Inclui dados autoritativos. |
| /glue | Inclui dados de cola. |
| /additional | Inclui todas as informações adicionais sobre os registros de recursos listados. |
| /node | Lista somente os registros de recursos do nó especificado. |
| /child | Lista somente os registros de recursos de um domínio filho especificado. |
| /startchild`<childname>` | Inicia a lista no domínio filho especificado. |
| /continue | Lista somente os registros de recursos com o tipo e os dados. |
| /detail | Lista todas as informações sobre os registros de recursos. |

#### <a name="example"></a>Exemplo

```
dnscmd /enumrecords test.contoso.com test /additional
```

## <a name="dnscmd-enumzones-command"></a>comando/enumzones do DNSCmd

Lista as zonas que existem no servidor DNS especificado. Os parâmetros **enumzones** atuam como filtros na lista de zonas. Se nenhum filtro for especificado, uma lista completa de zonas será retornada. Quando um filtro é especificado, somente as zonas que atendem aos critérios do filtro são incluídas na lista de zonas retornada.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <partitionFQDN>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| /primary | Lista todas as zonas que são zonas primárias padrão ou zonas integradas do Active Directory. |
| /secondary | Lista todas as zonas secundárias padrão. |
| /forwarder | Lista as zonas que encaminham consultas não resolvidas para outro servidor DNS. |
| /stub | Lista todas as zonas de stub. |
| /cache | Lista somente as zonas que são carregadas no cache. |
| /auto-created] | Lista as zonas que foram criadas automaticamente durante a instalação do servidor DNS. |
| /forward | Lista as zonas de pesquisa direta. |
| /reverse | Lista as zonas de pesquisa inversa. |
| /ds | Lista as zonas integradas do Active Directory. |
| /File | Lista as zonas que são apoiadas por arquivos. |
| /domaindirectorypartition | Lista as zonas que são armazenadas na partição de diretório de domínio. |
| /forestdirectorypartition | Lista as zonas que são armazenadas na partição de diretório de aplicativos DNS da floresta. |
| /customdirectorypartition | Lista todas as zonas que são armazenadas em uma partição de diretório de aplicativo definida pelo usuário. |
| /legacydirectorypartition | Lista todas as zonas que estão armazenadas na partição de diretório de domínio. |
| /directorypartition `<partitionFQDN>` | Lista todas as zonas que estão armazenadas na partição de diretório especificada. |

#### <a name="examples"></a>Exemplos

- [Exemplo 2: exibir uma lista completa de zonas em um servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-2-display-a-complete-list-of-zones-on-a-dns-server))

- [Exemplo 3: exibir uma lista de zonas autocriadas em um servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-3-display-a-list-of-autocreated-zones-on-a-dns-server)

## <a name="dnscmd-exportsettings-command"></a>comando/ExportSettings do DNSCmd

Cria um arquivo de texto que lista os detalhes de configuração de um servidor DNS. O arquivo de texto é nomeado *DnsSettings.txt*. Ele está localizado no `%systemroot%\system32\dns` diretório do servidor. Você pode usar as informações no arquivo que o **dnscmd/ExportSettings** cria para solucionar problemas de configuração ou para garantir que você tenha configurado vários servidores de forma idêntica.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /exportsettings
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |

## <a name="dnscmd-info-command"></a>comando/info do DNSCmd

Exibe as configurações da seção DNS do registro do servidor especificado `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters` . Para exibir as configurações do registro no nível da zona, use o `dnscmd zoneinfo` comando.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /info [<settings>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<settings>` | Qualquer configuração que o comando **info** retorna pode ser especificada individualmente. Se uma configuração não for especificada, um relatório de configurações comuns será retornado. |

#### <a name="example"></a>Exemplo

- [Exemplo 4: exibir a configuração isslave de um servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-4-display-the-isslave-setting-from-a-dns-server)

- [Exemplo 5: exibir a configuração de RecursionTimeout de um servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-5-display-the-recursiontimeout-setting-from-a-dns-server)

## <a name="dnscmd-ipvalidate-command"></a>comando/ipvalidate do DNSCmd

Testa se um endereço IP identifica um servidor DNS funcional ou se o servidor DNS pode atuar como encaminhador, um servidor de dica de raiz ou um servidor mestre para uma zona específica.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /ipvalidate <context> [<zonename>] [[<IPaddress>]]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<context>` | Especifica o tipo de teste a ser executado. Você pode especificar qualquer um dos seguintes testes:<ul><li>**/dnsservers** -testa se os computadores com os endereços especificados são servidores DNS em funcionamento.</li><li>**/forwarders** -testa se os endereços que você especifica identificam servidores DNS que podem atuar como encaminhadores.</li><li>**/roothints** -testa se os endereços que você especifica identificam servidores DNS que podem atuar como servidores de nomes de dica de raiz.</li><li>**/zonemasters** -testa se os endereços que você especifica identificam servidores DNS que são servidores mestres para *zonename*. |
| `<zonename>` | Identifica a zona. Use esse parâmetro com o parâmetro **/zonemasters** . |
| `<IPaddress>` | Especifica os endereços IP que o comando testa. |

#### <a name="examples"></a>Exemplos

```
nscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2
```

## <a name="dnscmd-nodedelete-command"></a>comando/nodedelete do DNSCmd

Exclui todos os registros de um host especificado.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /nodedelete <zonename> <nodename> [/tree] [/f]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona. |
| `<nodename>` | Especifica o nome do host do nó a ser excluído. |
| /tree | Exclui todos os registros filho. |
| /f | Executa o comando sem pedir confirmação. |

#### <a name="example"></a>Exemplo

[Exemplo 6: excluir os registros de um nó](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-6-delete-the-records-from-a-node)

## <a name="dnscmd-recordadd-command"></a>comando/RecordAdd do DNSCmd

Adiciona um registro a uma zona especificada em um servidor DNS.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /recordadd <zonename> <nodename> <rrtype> <rrdata>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica a zona na qual o registro reside. |
| `<nodename>` | Especifica um nó específico na zona. |
| `<rrtype>` | Especifica o tipo de registro a ser adicionado. |
| `<rrdata>` | Especifica o tipo de dados que é esperado. |

> [!NOTE]
> Depois de adicionar um registro, certifique-se de usar o tipo de dados e o formato de dados corretos. Para obter uma lista de tipos de registro de recurso e os tipos de dados apropriados, consulte [exemplos do dnscmd](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)).

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-recorddelete-command"></a>comando/recorddelete do DNSCmd

Exclui um registro de recurso para uma zona especificada.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /recorddelete <zonename> <nodename> <rrtype> <rrdata> [/f]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica a zona na qual reside o registro de recurso. |
| `<nodename>` | Especifica um nome do host. |
| `<rrtype>` | Especifica o tipo de registro de recurso a ser excluído. |
| `<rrdata>` | Especifica o tipo de dados que é esperado. |
| /f | Executa o comando sem pedir confirmação. Como os nós podem ter mais de um registro de recurso, esse comando exige que você seja muito específico sobre o tipo de registro de recurso que deseja excluir. Se você especificar um tipo de dados e não especificar um tipo de dados de registro de recurso, todos os registros com esse tipo de dados específico para o nó especificado serão excluídos. |

#### <a name="examples"></a>Exemplos

```
dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-resetforwarders-command"></a>comando/ResetForwarders do DNSCmd

Seleciona ou redefine os endereços IP para os quais o servidor DNS encaminha as consultas DNS quando ele não pode resolvê-las localmente.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /resetforwarders <IPaddress> [,<IPaddress>]...][/timeout <timeout>] [/slave | /noslave]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<IPaddress>` | Lista os endereços IP para os quais o servidor DNS encaminha consultas não resolvidas. |
| /Timeout `<timeout>` | Define o número de segundos que o servidor DNS espera por uma resposta do encaminhador. Por padrão, esse valor é cinco segundos. |
| /slave | Impede que o servidor DNS execute suas próprias consultas iterativas se o encaminhador falhar ao resolver uma consulta. |
| /noslave | Permite que o servidor DNS execute suas próprias consultas iterativas se o encaminhador não resolver uma consulta. Essa é a configuração padrão. |
| /f | Executa o comando sem pedir confirmação. Como os nós podem ter mais de um registro de recurso, esse comando exige que você seja muito específico sobre o tipo de registro de recurso que deseja excluir. Se você especificar um tipo de dados e não especificar um tipo de dados de registro de recurso, todos os registros com esse tipo de dados específico para o nó especificado serão excluídos. |

##### <a name="remarks"></a>Comentários

- Por padrão, um servidor DNS executa consultas iterativas quando não é possível resolver uma consulta.

- A configuração de endereços IP usando o comando **ResetForwarders** faz com que o servidor DNS execute consultas recursivas para os servidores DNS nos endereços IP especificados. Se os encaminhadores não resolverem a consulta, o servidor DNS poderá executar suas próprias consultas iterativas.

- Se o parâmetro **/slave** for usado, o servidor DNS não executará suas próprias consultas iterativas. Isso significa que o servidor DNS encaminha consultas não resolvidas somente para os servidores DNS na lista e não tentará consultas iterativas se os encaminhadores não os resolverem. É mais eficiente definir um endereço IP como um encaminhador para um servidor DNS. Você pode usar o comando **ResetForwarders** para servidores internos em uma rede para encaminhar suas consultas não resolvidas para um servidor DNS que tenha uma conexão externa.

- A listagem do endereço IP de um encaminhador duas vezes faz com que o servidor DNS tente encaminhar para esse servidor duas vezes.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave
dnscmd dnssvr1.contoso.com /resetforwarders /noslave
```

## <a name="dnscmd-resetlistenaddresses-command"></a>comando/ResetListenAddresses do DNSCmd

Especifica os endereços IP em um servidor que escuta solicitações de cliente DNS. Por padrão, todos os endereços IP em um servidor DNS escutam solicitações de DNS do cliente.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /resetlistenaddresses <listenaddress>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<listenaddress>` | Especifica um endereço IP no servidor DNS que escuta solicitações de cliente DNS. Se nenhum endereço de escuta for especificado, todos os endereços IP no servidor escutarão as solicitações do cliente. |

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1
```

## <a name="dnscmd-startscavenging-command"></a>comando/startscavenging do DNSCmd

Informa um servidor DNS para tentar uma pesquisa imediata de registros de recursos obsoletos em um servidor DNS especificado.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /startscavenging
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |

##### <a name="remarks"></a>Comentários

- A conclusão bem-sucedida desse comando inicia uma limpeza imediatamente. Se a limpeza falhar, nenhuma mensagem de aviso será exibida.

- Embora o comando para iniciar a limpeza pareça ser concluído com êxito, a limpeza não é iniciada a menos que as seguintes condições sejam atendidas:

    - A eliminação é habilitada tanto para o servidor quanto para a zona.

    - A zona é iniciada.

    - Os registros de recursos têm um carimbo de data/hora.

- Para obter informações sobre como habilitar a eliminação para o servidor, consulte o parâmetro **ScavengingInterval** em **sintaxe de nível de servidor** na seção **/config** .

- Para obter informações sobre como habilitar a eliminação para a zona, consulte o parâmetro **Aging** em **sintaxe no nível da zona** na seção **/config** .

- Para obter informações sobre como reiniciar uma zona pausada, consulte o parâmetro **zoneresume** neste artigo.

- Para obter informações sobre como verificar os registros de recursos para um carimbo de data/hora, consulte o parâmetro **AgeAllRecords** neste artigo.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /startscavenging
```

## <a name="dnscmd-statistics-command"></a>comando/statistics do DNSCmd

Exibe ou limpa os dados de um servidor DNS especificado.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /statistics [<statid>] [/clear]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<statid>` | Especifica qual estatística ou combinação de estatísticas exibir. O comando **Statistics** exibe os contadores que começam no servidor DNS quando ele é iniciado ou retomado. Um número de identificação é usado para identificar uma estatística. Se nenhum número de ID de estatística for especificado, todas as estatísticas serão exibidas. Os números que podem ser especificados, juntamente com a estatística correspondente que exibe, podem incluir:<ul><li>**00000001** -tempo</li><li>**00000002** -consulta</li><li>**00000004** -Query2</li><li>**00000008** -recurse</li><li>**00000010** -mestre</li><li>**00000020** -secundário</li><li>**00000040** -WINS</li><li>**00000100** -atualização</li><li>**00000200** -SkwanSec</li><li>**00000400** – DS</li><li>**00010000** -memória</li><li>**00100000** -PacketMem</li><li>**00040000** -dBASE</li><li>**00080000** -registros</li><li>**00200000** -NbstatMem</li><li>**/Clear** -redefine o contador de estatísticas especificado como zero.</li></ul> |

#### <a name="examples"></a>Exemplos

- [Exemplo 7: ](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-7-display-time-statistics-for-a-dns-server)

- [Exemplo 8: exibir estatísticas de NbstatMem para um servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-8-display-nbstatmem-statistics-for-a-dns-server)

## <a name="dnscmd-unenlistdirectorypartition-command"></a>comando/unenlistdirectorypartition do DNSCmd

Remove o servidor DNS do conjunto de réplicas da partição de diretório especificada.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /unenlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<partitionFQDN>` | O FQDN da partição de diretório do aplicativo DNS que será removida. |

## <a name="dnscmd-writebackfiles-command"></a>comando/writebackfiles do DNSCmd

Verifica a memória do servidor DNS em busca de alterações e as grava no armazenamento persistente. O comando **writebackfiles** atualiza todas as zonas sujas ou uma zona especificada. Uma zona é suja quando há alterações na memória que ainda não foram gravadas no armazenamento persistente. Essa é uma operação em nível de servidor que verifica todas as zonas. Você pode especificar uma zona nesta operação ou pode usar a operação **zonewriteback** .

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /writebackfiles <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser atualizada. |

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /writebackfiles
```

## <a name="dnscmd-zoneadd-command"></a>comando/ZoneAdd do DNSCmd

Adiciona uma zona ao servidor DNS.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneadd <zonename> <zonetype> [/dp <FQDN> | {/domain | enterprise | legacy}]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona. |
| `<zonetype>` | Especifica o tipo de zona a ser criada. A especificação de um tipo de zona de **/forwarder** ou **/DsForwarder** cria uma zona que executa o encaminhamento condicional. Cada tipo de zona tem diferentes parâmetros necessários:<ul><li>**/DsPrimary** – cria uma zona integrada do Active Directory.</li><li>**/Primary/File `<filename>` ** -Cria uma zona primária padrão e especifica o nome do arquivo que irá armazenar as informações de zona.</li><li>**/Secondary `<masterIPaddress> [<masterIPaddress>...]` ** -Cria uma zona secundária padrão.</li><li>**/stub `<masterIPaddress> [<masterIPaddress>...]` /File `<filename>` ** – cria uma zona de stub com backup em arquivo.</li><li>**/DsStub `<masterIPaddress> [<masterIPaddress>...]` ** -Cria uma zona de stub integrada do Active Directory.</li><li>**/forwarder `<masterIPaddress> [<masterIPaddress>]` .../File `<filename>` ** -especifica que a zona criada encaminha consultas não resolvidas para outro servidor DNS.</li><li>**/DsForwarder** -especifica que a zona integrada do Active Directory criada encaminha consultas não resolvidas para outro servidor DNS.</li></ul> |
| `<FQDN>` | Especifica o FQDN da partição de diretório. |
| /Domain | Armazena a zona na partição de diretório de domínio. |
| /enterprise | Armazena a zona na partição de diretório empresarial. |
| /legacy | Armazena a zona em uma partição de diretório herdada. |

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zonechangedirectorypartition-command"></a>comando/ZoneChangeDirectoryPartition do DNSCmd

Altera a partição de diretório na qual a zona especificada reside.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zonechangedirectorypartition <zonename> {[<newpartitionname>] | [<zonetype>]}
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | O FQDN da partição de diretório atual na qual a zona reside. |
| `<newpartitionname>` | O FQDN da partição de diretório para a qual a zona será movida. |
| `<zonetype>` | Especifica o tipo de partição de diretório para a qual a zona será movida. |
| /Domain | Move a zona para a partição de diretório de domínio interna. |
| /Forest | Move a zona para a partição de diretório de floresta interna. |
| /legacy | Move a zona para a partição de diretório que é criada para controladores de domínio anteriores do Active Directory. Essas partições de diretório não são necessárias para o modo nativo. |

## <a name="dnscmd-zonedelete-command"></a>comando/zonedelete do DNSCmd

Exclui uma zona especificada.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zonedelete <zonename> [/dsdel] [/f]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser excluída. |
| /dsdel | Exclui a zona do Azure Directory Domain Services (AD DS). |
| /f | Executa o comando sem pedir confirmação. |

#### <a name="examples"></a>Exemplos

- [Exemplo 9: excluir uma zona de um servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-9-delete-a-zone-from-a-dns-server)

## <a name="dnscmd-zoneexport-command"></a>comando/zoneexport do DNSCmd

Cria um arquivo de texto que lista os registros de recursos de uma zona especificada. A operação **zoneexport** cria um arquivo de registros de recursos para uma zona integrada do Active Directory para fins de solução de problemas. Por padrão, o arquivo que esse comando cria é colocado no diretório DNS, que é, por padrão, o `%systemroot%/System32/Dns` diretório.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneexport <zonename> <zoneexportfile>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona. |
| `<zoneexportfile>` | Especifica o nome do arquivo a ser criado. |

#### <a name="examples"></a>Exemplos

- [Exemplo 10: exportar a lista de registros de recursos da zona para um arquivo](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-10-export-zone-resource-records-list-to-a-file)

## <a name="dnscmd-zoneinfo"></a>/zoneinfo do DNSCmd

Exibe as configurações da seção do registro da zona especificada: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\<zonename>`

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneinfo <zonename> [<setting>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona. |
| `<setting>` | Você pode especificar individualmente qualquer configuração que o comando **zoneinfo** retornar. Se você não especificar uma configuração, todas as configurações serão retornadas. |

##### <a name="remarks"></a>Comentários

- Para exibir as configurações do registro no nível do servidor, use o comando **/info** .

- Para ver uma lista de configurações que você pode exibir com esse comando, consulte o comando **/config** .

#### <a name="examples"></a>Exemplos

- [Exemplo 11: exibir a configuração de RefreshInterval do registro](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-11-display-refreshinterval-setting-from-the-registry)

- [Exemplo 12: exibir a configuração de envelhecimento do registro](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-12-display-aging-setting-from-the-registry)

## <a name="dnscmd-zonepause-command"></a>comando/zonepause do DNSCmd

Pausa a zona especificada, que, em seguida, ignora as solicitações de consulta.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zonepause <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser pausada. |

##### <a name="remarks"></a>Comentários

- Para retomar uma zona e torná-la disponível depois que ela tiver sido pausada, use o comando **/zoneresume** .

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zonepause test.contoso.com
```

## <a name="dnscmd-zoneprint-command"></a>comando/zoneprint do DNSCmd

Lista os registros em uma zona.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneprint <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser listada. |







## <a name="dnscmd-zonerefresh-command"></a>comando/zonerefresh do DNSCmd

Força uma zona DNS secundária a atualizar a partir da zona mestre.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zonerefresh <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser atualizada. |

##### <a name="remarks"></a>Comentários

- O comando **zonerefresh** força uma verificação do número de versão no registro de recurso de início de autoridade (SOA) do servidor mestre. Se o número de versão no servidor mestre for maior que o número de versão do servidor secundário, uma transferência de zona será iniciada para atualizar o servidor secundário. Se o número de versão for o mesmo, nenhuma transferência de zona ocorrerá.

- A verificação forçada ocorre por padrão a cada 15 minutos. Para alterar o padrão, use o `dnscmd config refreshinterval` comando.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com
```

## <a name="dnscmd-zonereload-command"></a>comando/ZoneReload do DNSCmd

Copia informações de zona de sua origem.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zonereload <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser recarregada. |

##### <a name="remarks"></a>Comentários

- Se a zona estiver integrada ao Active Directory, ela será recarregada de Active Directory Domain Services (AD DS).

- Se a zona for uma zona com backup em arquivo padrão, ela será recarregada de um arquivo.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zonereload test.contoso.com
```

## <a name="dnscmd-zoneresetmasters-command"></a>comando/ZoneResetMasters do DNSCmd

Redefine os endereços IP do servidor mestre que fornece informações de transferência de zona para uma zona secundária.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneresetmasters <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser redefinida. |
| /local | Define uma lista mestra local. Esse parâmetro é usado para zonas integradas ao Active Directory. |
| `<IPaddress>` | Os endereços IP dos servidores mestres da zona secundária. |

##### <a name="remarks"></a>Comentários

- Esse valor é originalmente definido quando a zona secundária é criada. Use o comando **ZoneResetMasters** no servidor secundário. Esse valor não terá nenhum efeito se estiver definido no servidor DNS mestre.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local
```

## <a name="dnscmd-zoneresetscavengeservers-command"></a>comando/ZoneResetScavengeServers do DNSCmd

Altera os endereços IP dos servidores que podem recuperar a zona especificada.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneresetscavengeservers <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica a zona a ser reparada. |
| /local | Define uma lista mestra local. Esse parâmetro é usado para zonas integradas ao Active Directory. |
| `<IPaddress>` | Lista os endereços IP dos servidores que podem executar a limpeza. Se esse parâmetro for omitido, todos os servidores que hospedam essa zona poderão eliminá-lo. |

##### <a name="remarks"></a>Comentários

- Por padrão, todos os servidores que hospedam uma zona podem recuperar essa zona.

- Se uma zona estiver hospedada em mais de um servidor DNS, você poderá usar esse comando para reduzir o número de vezes que uma zona é recuperada.

- A eliminação deve ser habilitada no servidor DNS e na zona afetada por esse comando.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2
```

## <a name="dnscmd-zoneresetsecondaries-command"></a>comando/zoneresetsecondaries do DNSCmd

Especifica uma lista de endereços IP de servidores secundários para os quais um servidor mestre responde quando é solicitado uma transferência de zona.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneresetsecondaries <zonename> {/noxfr | /nonsecure | /securens | /securelist <securityIPaddresses>} {/nonotify | /notify | /notifylist <notifyIPaddresses>}
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona que terá seus servidores secundários redefinidos. |
| /local | Define uma lista mestra local. Esse parâmetro é usado para zonas integradas ao Active Directory. |
| /noxfr | Especifica que nenhuma transferência de zona é permitida. |
| /nonsecure | Especifica que todas as solicitações de transferência de zona são concedidas. |
| /securens | Especifica que somente o servidor listado no registro de recurso do servidor de nomes (NS) para a zona recebe uma transferência. |
| /securelist | Especifica que as transferências de zona são concedidas somente à lista de servidores. Esse parâmetro deve ser seguido por um endereço IP ou endereços que o servidor mestre usa. |
| `<securityIPaddresses>` | Lista os endereços IP que recebem transferências de zona do servidor mestre. Esse parâmetro é usado somente com o parâmetro **/Securelist** . |
| /nonotify | Especifica que nenhuma notificação de alteração é enviada aos servidores secundários. |
| /notify | Especifica que as notificações de alteração são enviadas para todos os servidores secundários. |
| /notifylist | Especifica que as notificações de alteração são enviadas somente para a lista de servidores. Esse comando deve ser seguido por um endereço IP ou endereços que o servidor mestre usa. |
| `<notifyIPaddresses>` | Especifica o endereço IP ou os endereços do servidor secundário ou servidores para os quais as notificações de alteração são enviadas. Essa lista é usada somente com o parâmetro **/NotifyList** . |

##### <a name="remarks"></a>Comentários

- Use o comando **zoneresetsecondaries** no servidor mestre para especificar como ele responde a solicitações de transferência de zona de servidores secundários.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2
```

## <a name="dnscmd-zoneresettype-command"></a>comando/zoneresettype do DNSCmd

Altera o tipo da zona.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneresettype <zonename> <zonetype> [/overwrite_mem | /overwrite_ds]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Identifica a zona na qual o tipo será alterado. |
| `<zonetype>` | Especifica o tipo de zona a ser criada. Cada tipo tem diferentes parâmetros necessários, incluindo:<ul><li>**/DsPrimary** – cria uma zona integrada do Active Directory.</li><li>**/Primary/File `<filename>` ** -Cria uma zona primária padrão.</li><li>**/Secondary `<masterIPaddress> [,<masterIPaddress>...]` ** -Cria uma zona secundária padrão.</li><li>**/stub `<masterIPaddress>[,<masterIPaddress>...]` /File `<filename>` ** – cria uma zona de stub com backup em arquivo.</li><li>**/DsStub `<masterIPaddress>[,<masterIPaddress>...]` ** -Cria uma zona de stub integrada do Active Directory.</li><li>**/forwarder `<masterIPaddress[,<masterIPaddress>]` .../File `<filename>` ** -especifica que a zona criada encaminha consultas não resolvidas para outro servidor DNS.</li><li>**/DsForwarder** -especifica que a zona integrada do Active Directory criada encaminha consultas não resolvidas para outro servidor DNS.</li></ul> |
| /overwrite_mem | Substitui dados DNS de dados em AD DS. |
| /overwrite_ds | Substitui os dados existentes no AD DS. |

##### <a name="remarks"></a>Comentários

- Definir o tipo de zona como **/DsForwarder** cria uma zona que executa o encaminhamento condicional.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zoneresume-command"></a>comando/zoneresume do DNSCmd

Inicia uma zona especificada que foi pausada anteriormente.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneresume <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser retomada. |

##### <a name="remarks"></a>Comentários

- Você pode usar essa operação para reiniciar a partir da operação **/zonepause** .

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com
```

## <a name="dnscmd-zoneupdatefromds-command"></a>comando/zoneupdatefromds do DNSCmd

Atualiza a zona integrada do Active Directory especificada do AD DS.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zoneupdatefromds <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser atualizada. |

##### <a name="remarks"></a>Comentários

- As zonas integradas do Active Directory executam essa atualização por padrão a cada cinco minutos. Para alterar esse parâmetro, use o `dnscmd config dspollinginterval` comando.

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zoneupdatefromds
```

## <a name="dnscmd-zonewriteback-command"></a>comando/zonewriteback do DNSCmd

Verifica a memória do servidor DNS em busca de alterações que sejam relevantes para uma zona especificada e as grava no armazenamento persistente.

### <a name="syntax"></a>Sintaxe

```
dnscmd [<servername>] /zonewriteback <zonename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `<servername>` | Especifica o servidor DNS a ser gerenciado, representado pelo endereço IP, pelo FQDN ou pelo nome do host. Se esse parâmetro for omitido, o servidor local será usado. |
| `<zonename>` | Especifica o nome da zona a ser atualizada. |

##### <a name="remarks"></a>Comentários

- Essa é uma operação em nível de zona. Você pode atualizar todas as zonas em um servidor DNS usando a operação **/writebackfiles** .

#### <a name="examples"></a>Exemplos

```
dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
