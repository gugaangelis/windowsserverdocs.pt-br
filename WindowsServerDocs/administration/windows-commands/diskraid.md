---
title: DiskRAID
description: Artigo de referência para a ferramenta de linha de comando do DiskRAID, que permite configurar e gerenciar a matriz redundante de subsistemas de armazenamento (RAID) de discos independentes (ou baratos).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0745c708878fa9da6571666b5702b4408976164
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924764"
---
# <a name="diskraid"></a>DiskRAID

O **DiskRAID** é uma ferramenta de linha de comando que permite configurar e gerenciar a matriz redundante de subsistemas de armazenamento (RAID) de discos independentes (ou baratos).

O RAID normalmente é usado em servidores para padronizar e categorizar sistemas de disco tolerantes a falhas. Os níveis de RAID fornecem várias combinações de desempenho, confiabilidade e custo. Alguns servidores fornecem três dos níveis de RAID: nível 0 (distribuição), nível 1 (espelhamento) e nível 5 (distribuição com paridade).

Um subsistema RAID de hardware distingue unidades de armazenamento endereçáveis fisicamente umas das outras usando um LUN (número de unidade lógica). Um objeto LUN deve ter pelo menos um plex e pode ter qualquer número de plexes adicionais. Cada plex contém uma cópia dos dados no objeto LUN. Os plexes podem ser adicionados e removidos de um objeto LUN.

A maioria dos comandos do DiskRAID opera em uma porta específica de HBA (adaptador de barramento de host), adaptador de iniciador, portal do iniciador, provedor, subsistema, controlador, porta, unidade, LUN, portal de destino, destino ou grupo do portal de destino. Use o comando **selecionar** para selecionar um objeto. O objeto selecionado é chamado de foco. O foco simplifica tarefas comuns de configuração, como a criação de vários LUNs no mesmo subsistema.

> [!NOTE]
> A ferramenta de linha de comando DiskRAID só funciona com subsistemas de armazenamento que dão suporte ao VDS (serviço de disco virtual).

## <a name="diskraid-commands"></a>Comandos do DiskRAID

Os comandos a seguir estão disponíveis de dentro da ferramenta DiskRAID.

### <a name="add"></a>add

Adiciona um LUN existente ao LUN selecionado no momento ou adiciona um portal de destino iSCSI ao grupo do portal de destino iSCSI selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| LUN de plex =`<n>` | Especifica o número de LUN a ser adicionado como um plex para o LUN selecionado no momento. Cuidado: todos os dados no LUN que estão sendo adicionados como um plex serão excluídos. |
| TPGROUP tportal =`<n>` | Especifica o número do portal de destino iSCSI a ser adicionado ao grupo do portal de destino iSCSI selecionado no momento. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskRAID continua processando comandos como se o erro não tivesse ocorrido. |

### <a name="associate"></a>sócio

Define a lista especificada de portas do controlador como ativas para o LUN selecionado atualmente (outras portas do controlador tornam-se inativas) ou adiciona as portas do controlador especificado à lista de portas do controlador ativo existentes para o LUN selecionado no momento ou associa o destino iSCSI especificado para o LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| controlador | Adiciona ou substitui a lista de controladores que estão associados ao LUN selecionado no momento. Use somente com provedores VDS 1,0. |
| ports | Adiciona ou substitui a lista de portas do controlador que estão associadas ao LUN selecionado no momento. Use somente com provedores VDS 1,1. |
| destinos | Adiciona ou substitui a lista de destinos iSCSI associados ao LUN selecionado no momento. Use somente com provedores VDS 1,1. |
| add | **Se estiver usando provedores VDS 1,0:** Adiciona os controladores especificados à lista existente de controladores associados ao LUN. Se esse parâmetro não for especificado, a lista de controladores substituirá a lista existente de controladores associados a esse LUN.<p>**Se estiver usando provedores VDS 1,1:** Adiciona as portas do controlador especificadas à lista existente de portas do controlador associadas ao LUN. Se esse parâmetro não for especificado, a lista de portas do controlador substituirá a lista existente de portas do controlador associadas a esse LUN. |
| `<n>[,<n> [, ...]]` | Use com o parâmetro **controladores** ou **destinos** . Especifica os números dos controladores ou destinos iSCSI a serem definidos como ativo ou associado. |
| `<n-m>[,<n-m>[,…]]` | Use com o parâmetro **ports** . Especifica as portas do controlador para definir o ativo usando um par de número de controlador (*n*) e número de porta (*m*). |

#### <a name="example"></a>Exemplo

Para associar e adicionar portas a um LUN que usa um provedor VDS 1,1:

```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)
```

### <a name="automagic"></a>automagic

Define ou limpa sinalizadores que dão dicas aos provedores sobre como configurar um LUN. Usado sem parâmetros, a operação **automagic** exibe uma lista de sinalizadores.

#### <a name="syntax"></a>Sintaxe

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| set | Define os sinalizadores especificados para os valores especificados. |
| clear | Limpa os sinalizadores especificados. A palavra-chave **All** apaga todos os sinalizadores automagic. |
| aplicar | Aplica os sinalizadores atuais ao LUN selecionado. |
| `<flag>` | Os sinalizadores são identificados por acrônimos de três letras, incluindo:<ul><li>**FCR** -recuperação de falha rápida necessária</li><li>**FTL** -tolerante a falhas</li><li>**MSR** -leituras mais exdominantes</li><li>**MXD** -máximo de unidades</li><li>**MXS** -tamanho máximo esperado</li><li>**Ora** -alinhamento de leitura ideal</li><li>**ORS** -tamanho de leitura ideal</li><li>**OSR** -otimizar para leituras sequenciais</li><li>**OSW** -otimizar para gravações sequenciais</li><li> **Owa** -alinhamento ideal de gravação</li><li>**OWS** -tamanho de gravação ideal</li><li>**RBP** -recriação de prioridade</li><li>**RBV** -verificação de leitura regressiva habilitada</li><li>**RMP** -remapeamento habilitado</li><li>**STS** -tamanho da Strip</li><li>**WTC** -cache de gravação habilitado</li><li>**YNK** – removível</li></ul> |

### <a name="break"></a>break

Remove o Plex do LUN selecionado no momento. O Plex e os dados contidos nela não são retidos e as extensões de unidade podem ser recuperadas.

> [!CAUTION]
> Primeiro, você deve selecionar um LUN espelhado antes de usar este comando. Todos os dados no Plex serão excluídos. Não há garantia de que todos os dados contidos no LUN original sejam consistentes.

#### <a name="syntax"></a>Sintaxe

```
break plex=<plex_number> [noerr]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| plexos | Especifica o número do plex a ser removido. O Plex e os dados contidos nela não serão retidos e os recursos usados por esse Plex serão recuperados. Não há garantia de que os dados contidos no LUN sejam consistentes. Se você quiser manter esse Plex, use o Serviço de Cópias de Sombra de Volume (VSS). |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskRAID continua processando comandos como se o erro não tivesse ocorrido. |

### <a name="chap"></a>via

Define o segredo compartilhado do CHAP (Challenge Handshake Authentication Protocol) para que os iniciadores iSCSI e os destinos iSCSI possam se comunicar uns com os outros.

#### <a name="syntax"></a>Sintaxe

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| conjunto de iniciadores | Define o segredo compartilhado no serviço do iniciador iSCSI local usado para autenticação CHAP mútua quando o iniciador autentica o destino. |
| Remember do iniciador | Comunica o segredo CHAP de um destino iSCSI para o serviço do iniciador iSCSI local para que o serviço iniciador possa usar o segredo para se autenticar no destino durante a autenticação CHAP. |
| conjunto de destino | Define o segredo compartilhado no destino iSCSI atualmente selecionado usado para autenticação CHAP quando o destino autentica o iniciador. |
| Lembre-se de destino | Comunica o segredo CHAP de um iniciador iSCSI para o destino iSCSI atual em foco para que o destino possa usar o segredo para se autenticar no iniciador durante a autenticação de CHAP mútuo. |
| segredo | Especifica o segredo a ser usado. Se estiver vazio, o segredo será limpo. |
| destino | Especifica um destino no subsistema selecionado no momento para associar ao segredo. Isso é opcional ao definir um segredo no iniciador e deixá-lo fora indica que o segredo será usado para todos os destinos que ainda não têm um segredo associado. |
| initiatorname | Especifica um nome iSCSI do iniciador a ser associado ao segredo. Isso é opcional ao definir um segredo em um destino e deixá-lo fora indica que o segredo será usado para todos os iniciadores que ainda não têm um segredo associado. |

### <a name="create"></a>create

Cria um novo LUN ou destino iSCSI no subsistema selecionado no momento ou cria um grupo de portais de destino no destino selecionado no momento. Você pode exibir a associação real usando o comando de **lista do DiskRAID** .

#### <a name="syntax"></a>Sintaxe

```
create lun simple [size=<n>] [drives=<n>] [noerr]
create lun stripe [size=<n>] [drives=<n, n> [,...]]  [stripesize=<n>] [noerr]
create lun raid [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun mirror [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun automagic size=<n> [noerr]
create target name=<name> [iscsiname=<iscsiname>] [noerr]
create tpgroup [noerr]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| simple | Cria um LUN simples. |
| faixa | Cria um LUN distribuído. |
| RAID | Cria um LUN distribuído com paridade. |
| mirror | Cria um LUN espelhado. |
| automagic | Cria um LUN usando as dicas *automagic* atualmente em vigor. Para obter mais informações, consulte o subcomando **automagic** neste artigo. |
| tamanho = | Especifica o tamanho total do LUN em megabytes. O parâmetro **size**= ou **drives**= deve ser especificado. Eles também podem ser usados juntos. Se o parâmetro **size =** não for especificado, o LUN criado será o maior tamanho possível permitido por todas as unidades especificadas.<p>Um provedor normalmente cria um LUN pelo menos tão grande quanto o tamanho solicitado, mas o provedor pode ter que arredondar para o próximo maior tamanho em alguns casos. Por exemplo, se o tamanho for especificado como. 99 GB e o provedor só puder alocar extensões de disco de GB, o LUN resultante será de 1 GB. Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:<ul><li>**B** -byte</li><li>**KB** -kilobyte</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte.</li></ul> |
| unidades = | Especifica o *drive_number* para as unidades a serem usadas para criar um LUN. O parâmetro **size**= ou **drives**= deve ser especificado. Eles também podem ser usados juntos. Se o parâmetro **size =** não for especificado, o LUN criado será o maior tamanho possível permitido por todas as unidades especificadas. Se o parâmetro **size =** for especificado, os provedores selecionarão unidades da lista de unidades especificadas para criar o LUN. Os provedores tentarão usar as unidades na ordem especificada, quando possível. |
| listras = | Especifica o tamanho em megabytes para uma *distribuição* ou LUN *RAID* . As listras não podem ser alteradas após a criação do LUN. Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:<ul><li>**B** -byte</li><li>**KB** -kilobyte</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte.</li></ul> |
| destino | Cria um novo destino iSCSI no subsistema selecionado no momento. |
| name | Fornece o nome amigável para o destino. |
| iscsiname | Fornece o nome iSCSI para o destino e pode ser omitido para que o provedor gere um nome. |
| TPGROUP | Cria um novo grupo do portal de destino iSCSI no destino selecionado no momento. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskRAID continua processando comandos como se o erro não tivesse ocorrido. |

### <a name="delete"></a>excluir

Exclui o LUN selecionado no momento, o destino iSCSI (desde que não haja LUNs associados ao destino iSCSI) ou o grupo do portal de destino iSCSI.

#### <a name="syntax"></a>Sintaxe

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| lun | Exclui o LUN selecionado no momento e todos os dados nele. |
| uninstall | Especifica que o disco no sistema local associado ao LUN será limpo antes que o LUN seja excluído. |
| destino | Excluirá o destino iSCSI atualmente selecionado se nenhum LUN estiver associado ao destino. |
| TPGROUP | Exclui o grupo do portal de destino iSCSI selecionado no momento. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskRAID continua processando comandos como se o erro não tivesse ocorrido. |

### <a name="detail"></a>detalhes

Exibe informações detalhadas sobre o objeto selecionado no momento do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| destes | Lista informações detalhadas sobre a porta do adaptador de barramento de host (HBA) selecionada no momento. |
| IADAPTER | Lista informações detalhadas sobre o adaptador do iniciador iSCSI selecionado no momento. |
| iportal | Lista informações detalhadas sobre o portal do iniciador iSCSI selecionado no momento. |
| provider | Lista informações detalhadas sobre o provedor selecionado no momento. |
| subsystem | Lista informações detalhadas sobre o subsistema selecionado no momento. |
| controlador | Lista informações detalhadas sobre o controlador selecionado no momento. |
| porta | Lista informações detalhadas sobre a porta do controlador selecionada no momento. |
| unidade | Lista informações detalhadas sobre a unidade selecionada no momento, incluindo os LUNs que ocupam. |
| lun | Lista informações detalhadas sobre o LUN selecionado no momento, incluindo as unidades de contribuição. A saída difere ligeiramente, dependendo se o LUN faz parte de um subsistema Fibre Channel ou iSCSI. Se a lista de hosts sem máscara contiver apenas um asterisco, isso significará que o LUN está sem máscara para todos os hosts. |
| tportal | Lista informações detalhadas sobre o portal de destino iSCSI selecionado no momento. |
| destino | Lista informações detalhadas sobre o destino iSCSI selecionado no momento. |
| TPGROUP | Lista informações detalhadas sobre o grupo do portal de destino iSCSI selecionado no momento. |
| verbose | Para uso somente com o parâmetro LUN. Lista informações adicionais, incluindo seus plexes. |

### <a name="dissociate"></a>desassociar

Define a lista especificada de portas do controlador como inativas para o LUN selecionado atualmente (outras portas do controlador não são afetadas) ou dissocia a lista especificada de destinos iSCSI para o LUN selecionado no momento.

#### <a name="syntax"></a>Syntax

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

##### <a name="parameter"></a>Parâmetro

| Parâmetro | Descrição |
| --------- | ----------- |
| controladores | Remove controladores da lista de controladores que estão associados ao LUN selecionado no momento. Use somente com provedores VDS 1,0. |
| ports | Remove portas do controlador da lista de portas do controlador que estão associadas ao LUN selecionado no momento. Use somente com provedores VDS 1,1. |
| destinos | Remove destinos da lista de destinos iSCSI associados ao LUN selecionado no momento. Use somente com provedores VDS 1,1. |
| `<n> [,<n> [,…]]` | Para uso com o parâmetro **controladores** ou **destinos** . Especifica os números dos controladores ou destinos iSCSI para definir como inativo ou dissociar. |
| `<n-m>[,<n-m>[,…]]` | Para uso com o parâmetro **ports** . Especifica as portas do controlador a serem definidas como inativas usando um par de número de controlador (*n*) e número de porta (*m*). |

#### <a name="example"></a>Exemplo

```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)

DISKRAID> DISSOCIATE PORTS 0-0,1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 1)
```

### <a name="exit"></a>exit

Sai do DiskRAID.

#### <a name="syntax"></a>Syntax

```
exit
```

### <a name="extend"></a>extend

Estende o LUN selecionado no momento adicionando setores ao final do LUN. Nem todos os provedores dão suporte à extensão de LUNs. Não estende os volumes ou sistemas de arquivos contidos no LUN. Depois de estender o LUN, você deve estender as estruturas em disco associadas usando o comando de **extensão do DiskPart** .

#### <a name="syntax"></a>Sintaxe

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho | Especifica o tamanho em megabytes para estender o LUN. O *tamanho* ou o `<drive>` parâmetro deve ser especificado. Eles também podem ser usados juntos. Se o parâmetro **size =** não for especificado, o LUN será estendido pelo maior tamanho possível permitido por todas as unidades especificadas. Se o parâmetro **size =** for especificado, os provedores selecionarão unidades na lista especificada pelo parâmetro **drives =** para criar o LUN. Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:<ul><li>**B** -byte</li><li>**KB** -kilobyte</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte.</li></ul> |
| unidades = | Especifica o `<drive_number>` para as unidades a serem usadas ao criar um LUN. O *tamanho* ou o `<drive>` parâmetro deve ser especificado. Eles também podem ser usados juntos. Se o parâmetro **size =** não for especificado, o LUN criado será o maior tamanho possível permitido por todas as unidades especificadas. Os provedores usam as unidades na ordem especificada quando possível. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskRAID continua processando comandos como se o erro não tivesse ocorrido. |

### <a name="flushcache"></a>flushcache

Limpa o cache no controlador selecionado no momento.

#### <a name="syntax"></a>Syntax

```
flushcache controller
```

### <a name="help"></a>ajuda

Exibe uma lista de todos os comandos do DiskRAID.

#### <a name="syntax"></a>Syntax

```
help
```

### <a name="importtarget"></a>importtarget

Recupera ou define o destino de importação de Serviço de Cópias de Sombra de Volume (VSS) atual definido para o subsistema selecionado no momento.

#### <a name="syntax"></a>Syntax

```
importtarget subsystem [set target]
```

##### <a name="parameter"></a>Parâmetro

| Parâmetro | Descrição |
| --------- | ----------- |
| definir destino | Se especificado, define o destino selecionado no momento para o destino de importação do VSS para o subsistema selecionado no momento. Se não for especificado, o comando recuperará o destino de importação VSS atual que está definido para o subsistema selecionado no momento. |

### <a name="initiator"></a>initiator

Recupera informações sobre o iniciador iSCSI local.

#### <a name="syntax"></a>Syntax

```
initiator
```

### <a name="invalidatecache"></a>invalidatecache

Invalida o cache no controlador selecionado no momento.

#### <a name="syntax"></a>Syntax

```
invalidatecache controller
```

### <a name="lbpolicy"></a>lbpolicy

Define a política de balanceamento de carga no LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| type | Especifica a política de balanceamento de carga. Se o tipo não for especificado, o parâmetro **path** deverá ser especificado. Tipo pode ser um dos seguintes:<ul><li>**Failover** – usa um caminho primário com outros caminhos sendo caminhos de backup.</li><li>**RoundRobin** -usa todos os caminhos no modo Round Robin, que tenta cada caminho sequencialmente.</li><li>**SUBSETROUNDROBIN** -usa todos os caminhos primários no modo Round Robin; os caminhos de backup serão usados somente se todos os caminhos primários falharem.</li><li>**DYNLQD** -usa o caminho com o número mínimo de solicitações ativas.<li><li>**Ponderado** – usa o caminho com o mínimo de peso (cada caminho deve ser atribuído a um peso).</li><li>**LEASTBLOCKS** -usa o caminho com os blocos mínimos.</li><li>**VENDORSPECIFIC** -usa uma política específica do fornecedor.</li></ul> |
| path | Especifica se um caminho é **primário** ou tem um específico `<weight>` . Todos os caminhos não especificados são definidos implicitamente como backup. Todos os caminhos listados devem ser um dos caminhos de LUN atualmente selecionados. |

### <a name="list"></a>list

Exibe uma lista de objetos do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
list {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| hbaports | Lista informações de resumo sobre todas as portas do HBA conhecidas como VDS. A porta do HBA selecionada no momento é marcada por um asterisco (*). |
| iadapters | Lista informações de resumo sobre todos os adaptadores de iniciador iSCSI conhecidos como VDS. O adaptador do iniciador selecionado atualmente é marcado por um asterisco (*). |
| iportals | Lista informações de resumo sobre todos os portais do iniciador iSCSI no adaptador do iniciador selecionado no momento. O portal do iniciador selecionado atualmente é marcado por um asterisco (*). |
| providers | Lista informações de resumo sobre cada provedor conhecido como VDS. O provedor selecionado no momento é marcado por um asterisco (*). |
| subsistemas | Lista informações de resumo sobre cada subsistema no sistema. O subsistema selecionado no momento é marcado por um asterisco (*). |
| controladores | Lista informações de resumo sobre cada controlador no subsistema selecionado no momento. O controlador selecionado atualmente é marcado por um asterisco (*). |
| ports | Lista informações de resumo sobre cada porta do controlador no controlador selecionado no momento. A porta atualmente selecionada é marcada por um asterisco (*). |
| unidades | Lista informações de resumo sobre cada unidade no subsistema selecionado no momento. A unidade atualmente selecionada é marcada por um asterisco (*). |
| melhor | Lista informações de resumo sobre cada LUN no subsistema selecionado no momento. O LUN selecionado atualmente é marcado por um asterisco (*). |
| tportals | Lista informações de resumo sobre todos os portais de destino iSCSI no subsistema selecionado no momento. O portal de destino selecionado atualmente é marcado por um asterisco (*). |
| destinos | Lista informações de resumo sobre todos os destinos iSCSI no subsistema selecionado no momento. O destino selecionado atualmente é marcado por um asterisco (*). |
| tpgroups | Lista informações de resumo sobre todos os grupos do portal de destino iSCSI no destino selecionado no momento. O grupo do portal selecionado atualmente é marcado por um asterisco (*). |

### <a name="login"></a>login

Registra o adaptador do iniciador iSCSI especificado no destino iSCSI selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| type | Especifica o tipo de logon a ser executado: **manual** ou **persistente**. Se não for especificado, um logon manual será executado. |
| manual | Faça logon manualmente. Há também uma opção de **inicialização** destinada ao desenvolvimento futuro e não é usada no momento. |
| persistente | Use o mesmo logon automaticamente quando o computador for reiniciado. |
| via | Especifica o tipo de autenticação CHAP a ser usado **none**: nenhum **, unichap ou CHAP** **mútuo** ; Se não for especificado, nenhuma autenticação será usada. |
| tportal | Especifica um portal de destino opcional no subsistema selecionado no momento para ser usado para o logon. |
| iportal | Especifica um portal do iniciador opcional no adaptador do iniciador especificado a ser usado para o logon. |
| `<flag>` | Identificado por acrônimos de três letras:<ul><li>**IPS** -exigir IPSec</li><li>**EMP** -habilitar vários caminhos</li><li>**EHD** -habilitar Resumo de cabeçalho</li><li>**EDD** -habilitar Resumo de dados</li></ul> |

### <a name="logout"></a>logout

Registra o adaptador do iniciador iSCSI especificado fora do destino iSCSI selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
logout target iadapter= <iadapter>
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| IADAPTER | Especifica o adaptador do iniciador com uma sessão de logon da qual fazer logoff. |

### <a name="maintenance"></a>manutenção

Executa operações de manutenção no objeto selecionado no momento do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
maintenance <object operation> [count=<iteration>]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<object>` | Especifica o tipo de objeto no qual executar a operação. O tipo de *objeto* pode ser um **subsistema**, **controlador**, **porta, unidade** ou **LUN**. |
| `<operation>` | Especifica a operação de manutenção a ser executada. O tipo de *operação* pode ser **SPINUP**, **spindown**, **piscar**, **emitir aviso sonoro** ou **ping**. Uma *operação* deve ser especificada. |
| contagem = | Especifica o número de vezes para repetir a *operação*. Normalmente, isso é usado com **intermitência**, **alarme sonoro**ou **ping**. |

### <a name="name"></a>name

Define o nome amigável do subsistema, LUN ou destino iSCSI atualmente selecionado para o nome especificado.

#### <a name="syntax"></a>Syntax

```
name {subsystem | lun | target} [<name>]
```

##### <a name="parameter"></a>Parâmetro

| Parâmetro | Descrição |
| --------- | ----------- |
| `<name>` | Especifica um nome para o subsistema, LUN ou destino. O nome deve ter menos de 64 caracteres de comprimento. Se nenhum nome for fornecido, o nome existente, se houver, será excluído. |

### <a name="offline"></a>offline

Define o estado do objeto selecionado no momento do tipo especificado como **offline**.

#### <a name="syntax"></a>Syntax

```
offline <object>
```

##### <a name="parameter"></a>Parâmetro

| Parâmetro | Descrição |
| --------- | ----------- |
| `<object>` | Especifica o tipo de objeto no qual executar esta operação. O tipo pode ser: **subsistema**, **controlador**, **unidade**, **LUN**ou **tportal**. |

### <a name="online"></a>online

Define o estado do objeto selecionado do tipo especificado como **online**. Se Object for **hbaport**, o alterará o status dos caminhos para a porta de HBA selecionada no momento para **online**.

#### <a name="syntax"></a>Syntax

```
online <object>
```

##### <a name="parameter"></a>Parâmetro

| Parâmetro | Descrição |
| --------- | ----------- |
| `<object>` | Especifica o tipo de objeto no qual executar esta operação. O tipo pode ser: **hbaport**, **Subsystem**, **Controller**, **drive**, **LUN**ou **tportal**. |

### <a name="recover"></a>recover

Executa as operações necessárias, como ressincronização ou Hot Sparing, para reparar o LUN tolerante a falhas selecionado no momento. Por exemplo, a recuperação pode fazer com que um hot spare seja associado a um conjunto de RAID que tenha um disco com falha ou outra realocação de extensão de disco.

#### <a name="syntax"></a>Syntax

```
recover <lun>
```

### <a name="reenumerate"></a>reenumerar

Reenumera objetos do tipo especificado. Se você usar o comando Extend LUN, deverá usar o comando Refresh para atualizar o tamanho do disco antes de usar o comando reenumerate.

#### <a name="syntax"></a>Sintaxe

```
reenumerate {subsystems | drives}
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| subsistemas | Consulta o provedor para descobrir novos subsistemas que foram adicionados ao provedor selecionado no momento. |
| unidades | Consulta os barramentos de e/s internos para descobrir as novas unidades que foram adicionadas no subsistema selecionado no momento. |

### <a name="refresh"></a>refresh

Atualiza os dados internos do provedor selecionado no momento.

#### <a name="syntax"></a>Syntax

```
refresh provider
```

### <a name="rem"></a>rem

Usado para comentar scripts.

#### <a name="syntax"></a>Syntax

```
Rem <comment>
```

### <a name="remove"></a>remove

Remove o portal de destino iSCSI especificado do grupo do portal de destino selecionado no momento.

#### <a name="syntax"></a>Syntax

```
remove tpgroup tportal=<tportal> [noerr]
```

##### <a name="parameter"></a>Parâmetro

| Parâmetro | Descrição |
| --------- | ----------- |
| TPGROUP tportal =`<tportal>` | Especifica o portal de destino iSCSI a ser removido. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskRAID continua processando comandos como se o erro não tivesse ocorrido. |

### <a name="replace"></a>substituir

Substitui a unidade especificada pela unidade selecionada no momento. A unidade especificada pode não ser a unidade selecionada no momento.

#### <a name="syntax"></a>Syntax

```
replace drive=<drive_number>
```

##### <a name="parameter"></a>Parâmetro

| Parâmetro | Descrição |
| --------- | ----------- |
| unidade = | Especifica o `<drive_number>` para a unidade a ser substituída. |

### <a name="reset"></a>reset

Redefine a porta ou o controlador selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
reset {controller | port}
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| controlador | Redefine o controlador. |
| porta | Redefine a porta. |

### <a name="select"></a>select

Exibe ou altera o objeto selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| objeto | Especifica o tipo de objeto a ser selecionado, incluindo: **provedor**, **subsistema**, **controlador**, **unidade**ou **LUN**. |
| destes`[<n>]` | Define o foco para a porta HBA local especificada. Se nenhuma porta HBA for especificada, o comando exibirá a porta HBA selecionada no momento (se houver). A especificação de um índice de porta HBA inválido resulta em nenhuma porta HBA em foco. A seleção de uma porta HBA anula a seleção de todos os adaptadores iniciadores e portais iniciadores selecionados. |
| IADAPTER`[<n>]` | Define o foco para o adaptador do iniciador iSCSI local especificado. Se nenhum adaptador do iniciador for especificado, o comando exibirá o adaptador do iniciador selecionado no momento (se houver). A especificação de um índice de adaptador iniciador inválido resulta em nenhum adaptador de iniciador em foco. A seleção de um adaptador de iniciador anula a seleção de portas HBA selecionadas e portais do iniciador. |
| iportal`[<n>]` | Define o foco para o portal do iniciador iSCSI local especificado no adaptador do iniciador iSCSI selecionado. Se nenhum portal do iniciador for especificado, o comando exibirá o portal do iniciador selecionado no momento (se houver). A especificação de um índice inválido do portal do iniciador resulta em nenhum portal do iniciador selecionado. |
| operador`[<n>]` | Define o foco para o provedor especificado. Se nenhum provedor for especificado, o comando exibirá o provedor selecionado no momento (se houver). A especificação de um índice de provedor inválido resulta em nenhum provedor em foco. |
| subsistema`[<n>]` | Define o foco para o subsistema especificado. Se nenhum subsistema for especificado, o comando exibirá o subsistema com foco (se houver). A especificação de um índice de subsistema inválido resulta em nenhum subsistema em foco. A seleção de um subsistema seleciona implicitamente seu provedor associado. |
| controle`[<n>]` | Define o foco para o controlador especificado no subsistema selecionado no momento. Se nenhum controlador for especificado, o comando exibirá o controlador selecionado no momento (se houver). A especificação de um índice de controlador inválido resulta em nenhum controlador em foco. A seleção de um controlador anula a seleção de quaisquer portas de controlador, unidades, LUNs, portais de destino, destinos e grupos do portal de destino selecionados. |
| Porto`[<n>]` | Define o foco para a porta do controlador especificado no controlador selecionado no momento. Se nenhuma porta for especificada, o comando exibirá a porta selecionada no momento (se houver). A especificação de um índice de porta inválido resulta em nenhuma porta selecionada. |
| Dirigir`[<n>]` | Define o foco para a unidade especificada ou o eixo físico no subsistema selecionado no momento. Se nenhuma unidade for especificada, o comando exibirá a unidade selecionada no momento (se houver). A especificação de um índice de unidade inválido resulta em nenhuma unidade em foco. A seleção de uma unidade anula a seleção de todos os controladores, portas do controlador, LUNs, portais de destino, destinos e grupos do portal de destino selecionados. |
| LUN`[<n>]` | Define o foco para o LUN especificado no subsistema selecionado no momento. Se nenhum LUN for especificado, o comando exibirá o LUN selecionado no momento (se houver). A especificação de um índice LUN inválido resulta em nenhum LUN selecionado. A seleção de um LUN anula a seleção de todos os controladores, portas do controlador, unidades, portais de destino, destinos e grupos do portal de destino selecionados. |
| tportal`[<n>]` | Define o foco para o portal de destino iSCSI especificado no subsistema selecionado no momento. Se nenhum portal de destino for especificado, o comando exibirá o portal de destino selecionado no momento (se houver). A especificação de um índice do portal de destino inválido resulta em nenhum portal de destino selecionado. A seleção de um portal de destino desmarca quaisquer controladores, portas do controlador, unidades, LUNs, destinos e grupos do portal de destino. |
| alvo`[<n>]` | Define o foco para o destino iSCSI especificado no subsistema selecionado no momento. Se nenhum destino for especificado, o comando exibirá o destino selecionado no momento (se houver). A especificação de um índice de destino inválido resulta em nenhum destino selecionado. A seleção de um destino desmarca quaisquer controladores, portas do controlador, unidades, LUNs, portais de destino e grupos do portal de destino. |
| TPGROUP`[<n>]` | Define o foco para o grupo do portal de destino iSCSI especificado no destino iSCSI selecionado no momento. Se nenhum grupo do portal de destino for especificado, o comando exibirá o grupo do portal de destino selecionado no momento (se houver). A especificação de um índice de grupo do portal de destino inválido resulta em nenhum grupo de portal de destino em foco. |
|`[<n>]` | Especifica o a `<object number>` ser selecionado. Se o `<object number>` especificado não for válido, todas as seleções existentes para objetos do tipo especificado serão limpas. Se não `<object number>` for especificado, o objeto atual será exibido.

### <a name="setflag"></a>SetFlag

Define a unidade atualmente selecionada como um hot spare. Os sobressalentes não podem ser usados para operações de associação de LUN comuns. Elas são reservadas apenas para tratamento de falhas. A unidade não deve estar atualmente associada a nenhum LUN existente.

#### <a name="syntax"></a>Sintaxe

```
setflag drive hotspare={true | false}
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| true | Seleciona a unidade atualmente selecionada como um hot spare. |
| false | Desmarca a unidade selecionada no momento como um hot spare. |

### <a name="shrink"></a>shrink

Reduz o tamanho do LUN selecionado.

#### <a name="syntax"></a>Sintaxe

```
shrink lun size=<n> [noerr]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho | Especifica a quantidade desejada de espaço em megabytes (MB) para reduzir o tamanho do LUN pelo. Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:<ul><li>**B** -byte</li><li>**KB** -kilobyte</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskRAID continua processando comandos como se o erro não tivesse ocorrido. |

### <a name="standby"></a>em espera

Altera o status dos caminhos para a porta do adaptador de barramento de host (HBA) atualmente selecionada para em espera.

#### <a name="syntax"></a>Sintaxe

```
standby hbaport
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| destes | Altera o status dos caminhos para a porta do adaptador de barramento de host (HBA) atualmente selecionada para em espera. |

### <a name="unmask"></a>unmask

Torna os LUNs atualmente selecionados acessíveis a partir dos hosts especificados.

#### <a name="syntax"></a>Sintaxe

```
unmask lun {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

##### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| all | Especifica que o LUN deve se tornar acessível de todos os hosts. No entanto, não é possível remover a máscara do LUN para todos os destinos em um subsistema iSCSI.<P>Você deve fazer logoff do destino antes de executar o `unmask lun all` comando. |
| nenhum | Especifica que o LUN não deve ser acessível a nenhum host.<P>Você deve fazer logoff do destino antes de executar o `unmask lun none` comando. |
| add | Especifica que os hosts especificados devem ser adicionados à lista existente de hosts do qual esse LUN está acessível. Se esse parâmetro não for especificado, a lista de hosts fornecida substituirá a lista existente de hosts dos quais esse LUN pode ser acessado. |
| WWN = | Especifica uma lista de números hexadecimais que representam os nomes de todo o mundo dos quais o LUN ou os hosts devem se tornar acessíveis. Para mascarar/remover a máscara para um conjunto específico de hosts em um subsistema Fibre Channel, você pode digitar uma lista separada por ponto-e-vírgula de WWN para as portas nos computadores host de seu interesse. |
| iniciador = | Especifica uma lista de iniciadores iSCSI para os quais o LUN selecionado no momento deve tornar-se acessível. Para mascarar/remover a máscara para um conjunto específico de hosts em um subsistema iSCSI, você pode digitar uma lista separada por ponto-e-vírgula de nomes de iniciadores iSCSI para os iniciadores nos computadores host de seu interesse. |
| uninstall | Se especificado, desinstala o disco associado ao LUN no sistema local antes que o LUN seja mascarado. |

## <a name="scripting-diskraid"></a>Script de DiskRAID

O DiskRAID pode ser incluído no script em qualquer computador que esteja executando uma versão com suporte do Windows Server, com um provedor de hardware VDS associado. Para invocar um script do DiskRAID, no prompt de comando, digite:

```
diskraid /s <script.txt>
```

Por padrão, o DiskRAID para o processamento de comandos e retorna um código de erro se houver um problema no script. Para continuar a executar o script e ignorar os erros, inclua o parâmetro **noerr** no comando. Isso permite práticas úteis como usar um único script para excluir todos os LUNs em um subsistema, independentemente do número total de LUNs. Nem todos os comandos dão suporte ao parâmetro **noerr** . Os erros sempre são retornados em erros de sintaxe de comando, independentemente de você ter incluído o parâmetro **noerr** .

## <a name="diskraid-error-codes"></a>Códigos de erro do DiskRAID

| Código do Erro | Descrição do erro |
| ---------- | ----------------- |
| 0 | Não ocorreu nenhum erro. Todo o script foi executado sem falha. |
| 1 | Ocorreu uma exceção fatal. |
| 2 | Os argumentos especificados em uma linha de comando do DiskRAID estavam incorretos. |
| 3 | O DiskRAID não pôde abrir o script ou o arquivo de saída especificado. |
| 4 | Um dos serviços que o DiskRAID usa retornou uma falha. |
| 5 | Ocorreu um erro de sintaxe de comando. O script falhou porque um objeto foi selecionado incorretamente ou era inválido para uso com esse comando. |

## <a name="example"></a>Exemplo

Para exibir o status do subsistema 0 no computador, digite:

```
diskraid
```

Pressione ENTER e uma saída semelhante à seguinte será exibida:

```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```

Para selecionar subsistema 0, digite o seguinte no prompt do DiskRAID:

```
select subsystem 0
```

Pressione ENTER e uma saída semelhante à seguinte será exibida:

```
Subsystem 0 is now the selected subsystem.

DISKRAID> list drives

  Drive ###  Status      Health          Size      Free    Bus  Slot  Flags
  ---------  ----------  ------------  --------  --------  ---  ----  -----
  Drive 0    Online      Healthy         107 GB    107 GB    0     1
  Drive 1    Offline     Healthy          29 GB     29 GB    1     0
  Drive 2    Online      Healthy         107 GB    107 GB    0     2
  Drive 3    Not Ready   Healthy          19 GB     19 GB    1     1
```

Para sair do DiskRAID, digite o seguinte no prompt do DiskRAID:

```
exit
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)