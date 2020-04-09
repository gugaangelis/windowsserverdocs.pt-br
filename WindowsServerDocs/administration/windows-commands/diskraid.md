---
title: diskraid
description: O tópico de comandos do Windows para o DiskRAID, que é uma ferramenta de linha de comando que permite que você configure e gerencie a matriz redundante de subsistemas de armazenamento (RAID) de discos independentes (ou baratos).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea71fc67420700527a3a14494c947aed7a2ec747
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845399"
---
# <a name="diskraid"></a>diskraid

O DiskRAID é uma ferramenta de linha de comando que permite configurar e gerenciar a matriz redundante de subsistemas de armazenamento (RAID) de discos independentes (ou baratos).

O RAID é um método usado para padronizar e categorizar sistemas de disco tolerantes a falhas. Os níveis de RAID fornecem várias combinações de desempenho, confiabilidade e custo. O RAID é geralmente usado em servidores. Alguns servidores fornecem três dos níveis de RAID: nível 0 (distribuição), nível 1 (espelhamento) e nível 5 (distribuição com paridade).

Um subsistema RAID de hardware distingue unidades de armazenamento endereçáveis fisicamente umas das outras usando um LUN (número de unidade lógica). Um objeto LUN deve ter pelo menos um plex e pode ter qualquer número de plexes adicionais. Cada plex contém uma cópia dos dados no objeto LUN. Os plexes podem ser adicionados e removidos de um objeto LUN.

A maioria dos comandos do DiskRAID opera em uma porta específica de HBA (adaptador de barramento de host), adaptador de iniciador, portal do iniciador, provedor, subsistema, controlador, porta, unidade, LUN, portal de destino, destino ou grupo do portal de destino. Use o comando selecionar para selecionar um objeto. O objeto selecionado é chamado de foco. O foco simplifica tarefas comuns de configuração, como a criação de vários LUNs no mesmo subsistema.

> [!NOTE]
> A ferramenta de linha de comando DiskRAID só funciona com subsistemas de armazenamento que dão suporte ao VDS (serviço de disco virtual).

## <a name="diskraid-commands"></a>Comandos do DiskRAID

Para exibir a sintaxe do comando, clique em um comando:
-   [add](#BKMK_1)
-   [sócio](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [via](#BKMK_5)
-   [criada](#BKMK_6)
-   [delete](#BKMK_7)
-   [detalhes](#BKMK_8)
-   [desassociar](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [Configure](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [lista](#BKMK_19)
-   [entrar](#BKMK_20)
-   [logout](#BKMK_21)
-   [manter](#BKMK_22)
-   [name](#BKMK_23)
-   [está](#BKMK_24)
-   [conectar](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerar](#BKMK_27)
-   [Nova](#BKMK_28)
-   [rem](#BKMK_29)
-   [exclu](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [Não](#BKMK_33)
-   [SetFlag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [Espera](#BKMK_35)
-   [unmask](#BKMK_36)

### <a name="add"></a><a name=BKMK_1></a>agrega

Adiciona um LUN existente ao LUN selecionado no momento ou adiciona um portal de destino iSCSI ao grupo do portal de destino iSCSI selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

##### <a name="parameters"></a>Parâmetros

**LUN de plex**=*n*

Especifica o número de LUN a ser adicionado como um plex para o LUN selecionado no momento.

> [!CAUTION]
> Todos os dados no LUN que está sendo adicionado como um plex serão excluídos.

**TPGROUP tportal =** <em>n</em>

Especifica o número do portal de destino iSCSI a ser adicionado ao grupo do portal de destino iSCSI selecionado no momento.

**NOERR**

Especifica que qualquer falha que ocorra durante a execução desta operação será ignorada. Isso é útil no modo de script.

### <a name="associate"></a><a name=BKMK_2></a>sócio

Define a lista especificada de portas do controlador como ativas para o LUN selecionado atualmente (outras portas do controlador tornam-se inativas) ou adiciona as portas do controlador especificado à lista de portas do controlador ativo existentes para o LUN selecionado no momento ou associa o destino iSCSI especificado para o LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

##### <a name="parameters"></a>Parâmetros

**controladores**

Para uso somente com provedores VDS 1,0. Adiciona ou substitui a lista de controladores que estão associados ao LUN selecionado no momento.

**porta**

Para uso somente com provedores VDS 1,1. Adiciona ou substitui a lista de portas do controlador que estão associadas ao LUN selecionado no momento.

**aos**

Para uso somente com provedores VDS 1,1. Adiciona ou substitui a lista de destinos iSCSI associados ao LUN selecionado no momento.

**add**

Para provedores VDS 1,0, o adiciona os controladores especificados à lista existente de controladores associados ao LUN. Se esse parâmetro não for especificado, a lista de controladores substituirá a lista existente de controladores associados a esse LUN.

Para provedores VDS 1,1, o adiciona as portas do controlador especificado à lista existente de portas do controlador associadas ao LUN. Se esse parâmetro não for especificado, a lista de portas do controlador substituirá a lista existente de portas do controlador associadas a esse LUN.
```
<n>[,<n> [, ...]]
```
Para uso com o parâmetro **controladores** ou **destinos** . Especifica os números dos controladores ou destinos iSCSI a serem definidos como ativo ou associado.
```
<n-m>[,<n-m>[,…]]
```
Para uso com o parâmetro **ports** . Especifica as portas do controlador para definir o ativo usando um par de número de controlador (*n*) e número de porta (*m*).

#### <a name="example"></a>{1&gt;Exemplo&lt;1}

O exemplo a seguir mostra como associar e adicionar portas a um LUN que usa um provedor VDS 1,1:
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

### <a name="automagic"></a><a name=BKMK_3></a>automagic

Define ou limpa sinalizadores que dão dicas aos provedores sobre como configurar um LUN. Usado sem parâmetros, a operação **automagic** exibe uma lista de sinalizadores.

#### <a name="syntax"></a>Sintaxe

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

##### <a name="parameters"></a>Parâmetros

**set**

Define os sinalizadores especificados para os valores especificados.

**formatação**

Limpa os sinalizadores especificados. A palavra-chave **All** apaga todos os sinalizadores automagic.

**usar**

Aplica os sinalizadores atuais ao LUN selecionado.

sinalizador de \<>

Os sinalizadores são identificados por acrônimos de três letras.

|Flag|Descrição|
|----|-----------|
|FCR|Recuperação rápida de falhas necessária|
|FTL|Tolerante a falhas|
|MSR|Maioria das leituras|
|MXD|Máximo de unidades|
|MXS|Tamanho máximo esperado|
|ORA|Alinhamento de leitura ideal|
|ORS|Tamanho de leitura ideal|
|OSR|Otimizar para leituras sequenciais|
|OSW|Otimizar para gravações sequenciais|
|OWA|Alinhamento de gravação ideal|
|OWS|Tamanho de gravação ideal|
|RBP|Recriar prioridade|
|RBV|Verificação de leitura regressiva habilitada|
|RMP|Remapeamento habilitado|
|STS|Tamanho da distribuição|
|WTC|Cache de gravação habilitado|
|YNK|Removível|

### <a name="break"></a><a name=BKMK_4></a>interrupção

Remove o Plex do LUN selecionado no momento. O Plex e os dados contidos nela não são retidos e as extensões de unidade podem ser recuperadas.

#### <a name="syntax"></a>Sintaxe

```
break plex=<plex_number> [noerr]
```

##### <a name="parameters"></a>Parâmetros

**plexos**

Especifica o número do plex a ser removido. O Plex e os dados contidos nela não serão retidos e os recursos usados por esse Plex serão recuperados. Não há garantia de que os dados contidos no LUN sejam consistentes. Se você quiser manter esse Plex, use o Serviço de Cópias de Sombra de Volume (VSS).

**NOERR**

Especifica que qualquer falha que ocorra durante a execução desta operação será ignorada. Isso é útil no modo de script.

#### <a name="remarks"></a>Comentários

> [!NOTE]
> Primeiro, você deve selecionar um LUN espelhado antes de usar o comando **Break** .

> [!CAUTION]
> Todos os dados no Plex serão excluídos.

> [!CAUTION]
> Não há garantia de que todos os dados contidos no LUN original sejam consistentes.

### <a name="chap"></a><a name=BKMK_5></a>via

Define o segredo compartilhado do CHAP (Challenge Handshake Authentication Protocol) para que os iniciadores iSCSI e os destinos iSCSI possam se comunicar uns com os outros.

#### <a name="syntax"></a>Sintaxe

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

##### <a name="parameters"></a>Parâmetros

**conjunto de iniciadores**

Define o segredo compartilhado no serviço do iniciador iSCSI local usado para autenticação CHAP mútua quando o iniciador autentica o destino.

**Remember do iniciador**

Comunica o segredo CHAP de um destino iSCSI para o serviço do iniciador iSCSI local para que o serviço iniciador possa usar o segredo para se autenticar no destino durante a autenticação CHAP.

**conjunto de destino**

Define o segredo compartilhado no destino iSCSI atualmente selecionado usado para autenticação CHAP quando o destino autentica o iniciador.

**Lembre-se de destino**

Comunica o segredo CHAP de um iniciador iSCSI para o destino iSCSI atual em foco para que o destino possa usar o segredo para se autenticar no iniciador durante a autenticação de CHAP mútuo.

**RADIUS**

Especifica o segredo a ser usado. Se estiver vazio, o segredo será limpo.

**alvo**

Especifica um destino no subsistema selecionado no momento para associar ao segredo. Isso é opcional ao definir um segredo no iniciador e deixá-lo fora indica que o segredo será usado para todos os destinos que ainda não têm um segredo associado.

**initiatorname**

Especifica um nome iSCSI do iniciador a ser associado ao segredo. Isso é opcional ao definir um segredo em um destino e deixá-lo fora indica que o segredo será usado para todos os iniciadores que ainda não têm um segredo associado.

### <a name="create"></a><a name=BKMK_6></a>criada

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

#### <a name="parameter"></a>Parâmetro

**único**

Cria um LUN simples.

**Stripe**

Cria um LUN distribuído.

**RAID**

Cria um LUN distribuído com paridade.

**espelho**

Cria um LUN espelhado.

**automagic**

Cria um LUN usando as dicas *automagic* atualmente em vigor. Consulte o subcomando **automagic** para obter mais informações.

= de **tamanho**

Especifica o tamanho total do LUN em megabytes. Se o parâmetro **size =** não for especificado, o LUN criado será o maior tamanho possível permitido por todas as unidades especificadas.

Um provedor normalmente cria um LUN pelo menos tão grande quanto o tamanho solicitado, mas o provedor pode ter que arredondar para o próximo maior tamanho em alguns casos. Por exemplo, se o tamanho for especificado como. 99 GB e o provedor só puder alocar extensões de disco de GB, o LUN resultante será de 1 GB.

Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:
-   **B** para byte.
-   **KB** para kilobyte.
-   **MB** para megabyte.
-   **GB** para Gigabyte.
-   **TB** para terabyte.
-   **PB** para petabyte.

**unidades**=

Especifica o *drive_number* para as unidades a serem usadas para criar um LUN. Se o parâmetro **size =** não for especificado, o LUN criado será o maior tamanho possível permitido por todas as unidades especificadas. Se o parâmetro **size =** for especificado, os provedores selecionarão unidades da lista de unidades especificadas para criar o LUN. Os provedores tentarão usar as unidades na ordem especificada, quando possível.

**listrar**=

Especifica o tamanho em megabytes para uma *distribuição* ou LUN *RAID* . As listras não podem ser alteradas após a criação do LUN.

Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:
-   **B** para byte.
-   **KB** para kilobyte.
-   **MB** para megabyte.
-   **GB** para Gigabyte.
-   **TB** para terabyte.
-   **PB** para petabyte.

**alvo**

Cria um novo destino iSCSI no subsistema selecionado no momento.

**name**

Fornece o nome amigável para o destino.

**iscsiname**

Fornece o nome iSCSI para o destino e pode ser omitido para que o provedor gere um nome.

**TPGROUP**

Cria um novo grupo do portal de destino iSCSI no destino selecionado no momento.

**NOERR**

Especifica que qualquer falha que ocorra durante a execução desta operação será ignorada. Isso é útil no modo de script.

#### <a name="remarks"></a>Comentários

-   O parâmetro **size**= ou **drives**= deve ser especificado. Eles também podem ser usados juntos.
-   O tamanho da distribuição para um LUN não pode ser alterado após a criação.

### <a name="delete"></a><a name=BKMK_7></a>apagar

Exclui o LUN selecionado no momento, o destino iSCSI (desde que não haja LUNs associados ao destino iSCSI) ou o grupo do portal de destino iSCSI.

#### <a name="syntax"></a>Sintaxe

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

##### <a name="parameters"></a>Parâmetros

**LUN**

Exclui o LUN selecionado no momento e todos os dados nele.

**desinstalar**

Especifica que o disco no sistema local associado ao LUN será limpo antes que o LUN seja excluído.

**alvo**

Excluirá o destino iSCSI atualmente selecionado se nenhum LUN estiver associado ao destino.

**TPGROUP**

Exclui o grupo do portal de destino iSCSI selecionado no momento.

**NOERR**

Especifica que qualquer falha que ocorra durante a execução desta operação será ignorada. Isso é útil no modo de script.

### <a name="detail"></a><a name=BKMK_8></a>detalhes

Exibe informações detalhadas sobre o objeto selecionado no momento do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

##### <a name="parameters"></a>Parâmetros

**destes**

Lista informações detalhadas sobre a porta do adaptador de barramento de host (HBA) selecionada no momento.

**IADAPTER**

Lista informações detalhadas sobre o adaptador do iniciador iSCSI selecionado no momento.

**iportal**

Lista informações detalhadas sobre o portal do iniciador iSCSI selecionado no momento.

**operador**

Lista informações detalhadas sobre o provedor selecionado no momento.

**subsistema**

Lista informações detalhadas sobre o subsistema selecionado no momento.

**controle**

Lista informações detalhadas sobre o controlador selecionado no momento.

**Porto**

Lista informações detalhadas sobre a porta do controlador selecionada no momento.

**Dirigir**

Lista informações detalhadas sobre a unidade selecionada no momento, incluindo os LUNs que ocupam.

**LUN**

Lista informações detalhadas sobre o LUN selecionado no momento, incluindo as unidades de contribuição. A saída difere ligeiramente, dependendo se o LUN faz parte de um subsistema Fibre Channel ou iSCSI. Se a lista de hosts sem máscara contiver apenas um asterisco, isso significará que o LUN está sem máscara para todos os hosts.

**tportal**

Lista informações detalhadas sobre o portal de destino iSCSI selecionado no momento.

**alvo**

Lista informações detalhadas sobre o destino iSCSI selecionado no momento.

**TPGROUP**

Lista informações detalhadas sobre o grupo do portal de destino iSCSI selecionado no momento.

**extensa**

Para uso somente com o parâmetro LUN. Lista informações adicionais, incluindo seus plexes.

### <a name="dissociate"></a><a name=BKMK_9></a>desassociar

Define a lista especificada de portas do controlador como inativas para o LUN selecionado atualmente (outras portas do controlador não são afetadas) ou dissocia a lista especificada de destinos iSCSI para o LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Parâmetro

**controladores**

Para uso somente com provedores VDS 1,0. Remove controladores da lista de controladores que estão associados ao LUN selecionado no momento.

**porta**

Para uso somente com provedores VDS 1,1. Remove portas do controlador da lista de portas do controlador que estão associadas ao LUN selecionado no momento.

**aos**

Para uso somente com provedores VDS 1,1. Remove destinos da lista de destinos iSCSI associados ao LUN selecionado no momento.
```
<n> [,<n> [,…]]
```
Para uso com o parâmetro **controladores** ou **destinos** . Especifica os números dos controladores ou destinos iSCSI para definir como inativo ou dissociar.
```
<n-m>[,<n-m>[,…]]
```
Para uso com o parâmetro **ports** . Especifica as portas do controlador a serem definidas como inativas usando um par de número de controlador (*n*) e número de porta (*m*).

#### <a name="example"></a>{1&gt;Exemplo&lt;1}

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

### <a name="exit"></a><a name=BKMK_10></a>sido

Sai do DiskRAID.

#### <a name="syntax"></a>Sintaxe

```
exit
```

### <a name="extend"></a><a name=BKMK_11></a>estender

Estende o LUN selecionado no momento adicionando setores ao final do LUN. Nem todos os provedores dão suporte à extensão de LUNs. Não estende os volumes ou sistemas de arquivos contidos no LUN. Depois de estender o LUN, você deve estender as estruturas em disco associadas usando o comando de **extensão do DiskPart** .

#### <a name="syntax"></a>Sintaxe

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

##### <a name="parameters"></a>Parâmetros

**tamanho =**

Especifica o tamanho em megabytes para estender o LUN. Se o parâmetro **size =** não for especificado, o LUN será estendido pelo maior tamanho possível permitido por todas as unidades especificadas. Se o parâmetro **size =** for especificado, os provedores selecionarão unidades na lista especificada pelo parâmetro **drives =** para criar o LUN.

Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:
-   **B** para byte.
-   **KB** para kilobyte.
-   **MB** para megabyte.
-   **GB** para Gigabyte.
-   **TB** para terabyte
-   **PB** para petabyte

**unidades =**

Especifica o > de drive_number \<para as unidades a serem usadas ao criar um LUN. Se o parâmetro **size =** não for especificado, o LUN criado será o maior tamanho possível permitido por todas as unidades especificadas. Os provedores usam as unidades na ordem especificada quando possível.

**NOERR**

Especifica que qualquer falha que ocorra durante a execução desta operação deve ser ignorada. Isso é útil no modo de script.

#### <a name="remarks"></a>Comentários

O *tamanho* ou o parâmetro da unidade de \<> deve ser especificado. Eles também podem ser usados juntos.

### <a name="flushcache"></a><a name=BKMK_12></a>flushcache

Limpa o cache no controlador selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
flushcache controller
```

### <a name="help"></a><a name=BKMK_13></a>Ajuda

Exibe uma lista de todos os comandos do DiskRAID.

#### <a name="syntax"></a>Sintaxe

```
help
```

### <a name="importtarget"></a><a name=BKMK_14></a>importtarget

Recupera ou define o destino de importação de Serviço de Cópias de Sombra de Volume (VSS) atual definido para o subsistema selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Parâmetro

**definir destino**

Se especificado, define o destino selecionado no momento para o destino de importação do VSS para o subsistema selecionado no momento. Se não for especificado, o comando recuperará o destino de importação VSS atual que está definido para o subsistema selecionado no momento.

### <a name="initiator"></a><a name=BKMK_15></a>Configure

Recupera informações sobre o iniciador iSCSI local.

#### <a name="syntax"></a>Sintaxe

```
initiator
```

### <a name="invalidatecache"></a><a name=BKMK_16></a>invalidatecache

Invalida o cache no controlador selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
invalidatecache controller
```

### <a name="lbpolicy"></a><a name=BKMK_18></a>lbpolicy

Define a política de balanceamento de carga no LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

##### <a name="parameters"></a>Parâmetros

**type**

Especifica a política de balanceamento de carga. Se o tipo não for especificado, o parâmetro **path** deverá ser especificado. O tipo pode ser um dos seguintes:

**Failover**: usa um caminho primário com outros caminhos sendo caminhos de backup.

**RoundRobin**: usa todos os caminhos no modo Round Robin, que tenta cada caminho sequencialmente.

**SUBSETROUNDROBIN**: usa todos os caminhos primários no modo Round Robin; os caminhos de backup serão usados somente se todos os caminhos primários falharem.

**DYNLQD**: usa o caminho com o número mínimo de solicitações ativas.

**Ponderado**: usa o caminho com o mínimo de peso (cada caminho deve ser atribuído a um peso).

**LEASTBLOCKS**: usa o caminho com os blocos mínimos.

**VENDORSPECIFIC**: usa uma política específica do fornecedor.

**estruturas**

Especifica se um caminho é **primário** ou tem um determinado peso de \<>. Todos os caminhos não especificados são definidos implicitamente como backup. Todos os caminhos listados devem ser um dos caminhos de LUN atualmente selecionados.

### <a name="list"></a><a name=BKMK_19></a>lista

Exibe uma lista de objetos do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

##### <a name="parameters"></a>Parâmetros

**hbaports**

Lista informações de resumo sobre todas as portas do HBA conhecidas como VDS. A porta do HBA selecionada no momento é marcada por um asterisco (*).

**iadapters**

Lista informações de resumo sobre todos os adaptadores de iniciador iSCSI conhecidos como VDS. O adaptador do iniciador selecionado atualmente é marcado por um asterisco (*).

**iportals**

Lista informações de resumo sobre todos os portais do iniciador iSCSI no adaptador do iniciador selecionado no momento. O portal do iniciador selecionado atualmente é marcado por um asterisco (*).

**fornecedor**

Lista informações de resumo sobre cada provedor conhecido como VDS. O provedor selecionado no momento é marcado por um asterisco (*).

**subsistemas**

Lista informações de resumo sobre cada subsistema no sistema. O subsistema selecionado no momento é marcado por um asterisco (*).

**controladores**

Lista informações de resumo sobre cada controlador no subsistema selecionado no momento. O controlador selecionado atualmente é marcado por um asterisco (*).

**porta**

Lista informações de resumo sobre cada porta do controlador no controlador selecionado no momento. A porta atualmente selecionada é marcada por um asterisco (*).

**controla**

Lista informações de resumo sobre cada unidade no subsistema selecionado no momento. A unidade atualmente selecionada é marcada por um asterisco (*).

**melhor**

Lista informações de resumo sobre cada LUN no subsistema selecionado no momento. O LUN selecionado atualmente é marcado por um asterisco (*).

**tportals**

Lista informações de resumo sobre todos os portais de destino iSCSI no subsistema selecionado no momento. O portal de destino selecionado atualmente é marcado por um asterisco (*).

**aos**

Lista informações de resumo sobre todos os destinos iSCSI no subsistema selecionado no momento. O destino selecionado atualmente é marcado por um asterisco (*).

**tpgroups**

Lista informações de resumo sobre todos os grupos do portal de destino iSCSI no destino selecionado no momento. O grupo do portal selecionado atualmente é marcado por um asterisco (*).

### <a name="login"></a><a name=BKMK_20></a>entrar

Registra o adaptador do iniciador iSCSI especificado no destino iSCSI selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

##### <a name="parameters"></a>Parâmetros

**type**

Especifica o tipo de logon a ser executado: **manual**, **persistente**ou **inicialização**. Se não for especificado, um logon manual será executado.

**manual** -fazer logon manualmente.

**persistente** – use o mesmo logon automaticamente quando o computador for reiniciado.

**inicialização** – (essa opção é para desenvolvimento futuro e não é usada no momento<em>.</em>)

**via**

Especifica o tipo de autenticação CHAP a ser usado **none**: nenhum **, unichap ou CHAP** **mútuo** ; Se não for especificado, nenhuma autenticação será usada.

**tportal**

Especifica um portal de destino opcional no subsistema selecionado no momento para ser usado para o logon.

**iportal**

Especifica um portal do iniciador opcional no adaptador do iniciador especificado a ser usado para o logon.

sinalizador de \<>

Identificado por três acrônimos de letra:

**IPS**: exigir IPSec

**EMP**: habilitar vários caminhos

**EHD**: habilitar Resumo de cabeçalho

**EDD**: habilitar o resumo de dados

### <a name="logout"></a><a name=BKMK_21></a>logout

Registra o adaptador do iniciador iSCSI especificado fora do destino iSCSI selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
logout target iadapter= <iadapter>
```

##### <a name="parameters"></a>Parâmetros

**IADAPTER**

Especifica o adaptador do iniciador com uma sessão de logon da qual fazer logoff.

### <a name="maintenance"></a><a name=BKMK_22></a>manter

Executa operações de manutenção no objeto selecionado no momento do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
maintenance <object operation> [count=<iteration>]
```

##### <a name="parameters"></a>Parâmetros

objeto \<>

Especifica o tipo de objeto no qual executar a operação. O tipo de *objeto* pode ser um **subsistema**, **controlador**, **porta, unidade** ou **LUN**.

\<operação >

Especifica a operação de manutenção a ser executada. O tipo de *operação* pode ser **SPINUP**, **spindown**, **piscar**, **emitir aviso sonoro** ou **ping**. Uma *operação* deve ser especificada.

**contagem =**

Especifica o número de vezes para repetir a *operação*. Normalmente, isso é usado com **intermitência**, **alarme sonoro**ou **ping**.

### <a name="name"></a><a name=BKMK_23></a>nomes

Define o nome amigável do subsistema, LUN ou destino iSCSI atualmente selecionado para o nome especificado.

#### <a name="syntax"></a>Sintaxe

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Parâmetro

nome do \<>

Especifica um nome para o subsistema, LUN ou destino. O nome deve ter menos de 64 caracteres de comprimento. Se nenhum nome for fornecido, o nome existente, se houver, será excluído.

### <a name="offline"></a><a name=BKMK_24></a>está

Define o estado do objeto selecionado no momento do tipo especificado como **offline**.

#### <a name="syntax"></a>Sintaxe

```
offline <object>
```

#### <a name="parameter"></a>Parâmetro

objeto \<>

Especifica o tipo de objeto no qual executar esta operação. O objeto \<>

o tipo pode ser **subsistema**, **controlador**, **unidade**, **LUN**ou **tportal**.

### <a name="online"></a><a name=BKMK_25></a>conectar

Define o estado do objeto selecionado do tipo especificado como **online**. Se Object for **hbaport**, o alterará o status dos caminhos para a porta de HBA selecionada no momento para **online**.

#### <a name="syntax"></a>Sintaxe

```
online <object> 
```

#### <a name="parameter"></a>Parâmetro

objeto \<>

Especifica o tipo de objeto no qual executar esta operação. O objeto \<>

o tipo pode ser **hbaport**, **Subsystem**, **Controller**, **drive**, **LUN**ou **tportal**.

### <a name="recover"></a><a name=BKMK_26></a>Recupera

Executa as operações necessárias, como ressincronização ou Hot Sparing, para reparar o LUN tolerante a falhas selecionado no momento. Por exemplo, a recuperação pode fazer com que um hot spare seja associado a um conjunto de RAID que tenha um disco com falha ou outra realocação de extensão de disco.

#### <a name="syntax"></a>Sintaxe

```
recover <lun>
```

### <a name="reenumerate"></a><a name=BKMK_27></a>reenumerar

Reenumera objetos do tipo especificado. Se você usar o comando Extend LUN, deverá usar o comando Refresh para atualizar o tamanho do disco antes de usar o comando reenumerate.

#### <a name="syntax"></a>Sintaxe

```
reenumerate {subsystems | drives}
```

##### <a name="parameters"></a>Parâmetros

**subsistemas**

Consulta o provedor para descobrir novos subsistemas que foram adicionados ao provedor selecionado no momento.

**controla**

Consulta os barramentos de e/s internos para descobrir as novas unidades que foram adicionadas no subsistema selecionado no momento.

### <a name="refresh"></a><a name=BKMK_28></a>Nova

Atualiza os dados internos do provedor selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
refresh provider
```

### <a name="rem"></a><a name=BKMK_29></a>Trab

Usado para comentar scripts.

#### <a name="syntax"></a>Sintaxe

```
Rem <comment>
```

### <a name="remove"></a><a name=BKMK_30></a>exclu

Remove o portal de destino iSCSI especificado do grupo do portal de destino selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Parâmetro

**TPGROUP tportal =** \<tportal >

Especifica o portal de destino iSCSI a ser removido.

**NOERR**

Especifica que qualquer falha que ocorra durante a execução desta operação deve ser ignorada. Isso é útil no modo de script.

### <a name="replace"></a><a name=BKMK_31></a>Substitua

Substitui a unidade especificada pela unidade selecionada no momento.

#### <a name="syntax"></a>Sintaxe

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Parâmetro

**unidade =**

Especifica o > de drive_number \<para a unidade a ser substituída.

#### <a name="remarks"></a>Comentários

-   A unidade especificada pode não ser a unidade selecionada no momento.

### <a name="reset"></a><a name=BKMK_32></a>definido

Redefine a porta ou o controlador selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
Reset {controller | port}
```

##### <a name="parameters"></a>Parâmetros

**controle**

Redefine o controlador.

**Porto**

Redefine a porta.

### <a name="select"></a><a name=BKMK_33></a>Não

Exibe ou altera o objeto selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

##### <a name="parameters"></a>Parâmetros

**objeto**

Especifica o tipo de objeto a ser selecionado. O tipo de objeto de \<> pode ser **provedor**, **subsistema**, **controlador**, **unidade**ou **LUN**.

**hbaport** [\<n >]

Define o foco para a porta HBA local especificada. Se nenhuma porta HBA for especificada, o comando exibirá a porta HBA selecionada no momento (se houver). A especificação de um índice de porta HBA inválido resulta em nenhuma porta HBA em foco. A seleção de uma porta HBA anula a seleção de todos os adaptadores iniciadores e portais iniciadores selecionados.

**IADAPTER** [\<n >]

Define o foco para o adaptador do iniciador iSCSI local especificado. Se nenhum adaptador do iniciador for especificado, o comando exibirá o adaptador do iniciador selecionado no momento (se houver). A especificação de um índice de adaptador iniciador inválido resulta em nenhum adaptador de iniciador em foco. A seleção de um adaptador de iniciador anula a seleção de portas HBA selecionadas e portais do iniciador.

**IPORTAL** [\<n >]

Define o foco para o portal do iniciador iSCSI local especificado no adaptador do iniciador iSCSI selecionado. Se nenhum portal do iniciador for especificado, o comando exibirá o portal do iniciador selecionado no momento (se houver). A especificação de um índice inválido do portal do iniciador resulta em nenhum portal do iniciador selecionado.

**provedor** [\<n >]

Define o foco para o provedor especificado. Se nenhum provedor for especificado, o comando exibirá o provedor selecionado no momento (se houver). A especificação de um índice de provedor inválido resulta em nenhum provedor em foco.

**subsistema** [\<n >]

Define o foco para o subsistema especificado. Se nenhum subsistema for especificado, o comando exibirá o subsistema com foco (se houver). A especificação de um índice de subsistema inválido resulta em nenhum subsistema em foco. A seleção de um subsistema seleciona implicitamente seu provedor associado.

**controlador** [\<n >]

Define o foco para o controlador especificado no subsistema selecionado no momento. Se nenhum controlador for especificado, o comando exibirá o controlador selecionado no momento (se houver). A especificação de um índice de controlador inválido resulta em nenhum controlador em foco. A seleção de um controlador anula a seleção de quaisquer portas de controlador, unidades, LUNs, portais de destino, destinos e grupos do portal de destino selecionados.

**porta** [\<n >]

Define o foco para a porta do controlador especificado no controlador selecionado no momento. Se nenhuma porta for especificada, o comando exibirá a porta selecionada no momento (se houver). A especificação de um índice de porta inválido resulta em nenhuma porta selecionada.

**unidade** [\<n >]

Define o foco para a unidade especificada ou o eixo físico no subsistema selecionado no momento. Se nenhuma unidade for especificada, o comando exibirá a unidade selecionada no momento (se houver). A especificação de um índice de unidade inválido resulta em nenhuma unidade em foco. A seleção de uma unidade anula a seleção de todos os controladores, portas do controlador, LUNs, portais de destino, destinos e grupos do portal de destino selecionados.

**LUN** [\<n >]

Define o foco para o LUN especificado no subsistema selecionado no momento. Se nenhum LUN for especificado, o comando exibirá o LUN selecionado no momento (se houver). A especificação de um índice LUN inválido resulta em nenhum LUN selecionado. A seleção de um LUN anula a seleção de todos os controladores, portas do controlador, unidades, portais de destino, destinos e grupos do portal de destino selecionados.

**tportal** [\<n >]

Define o foco para o portal de destino iSCSI especificado no subsistema selecionado no momento. Se nenhum portal de destino for especificado, o comando exibirá o portal de destino selecionado no momento (se houver). A especificação de um índice do portal de destino inválido resulta em nenhum portal de destino selecionado. A seleção de um portal de destino desmarca quaisquer controladores, portas do controlador, unidades, LUNs, destinos e grupos do portal de destino.

**destino** [\<n >]

Define o foco para o destino iSCSI especificado no subsistema selecionado no momento. Se nenhum destino for especificado, o comando exibirá o destino selecionado no momento (se houver). A especificação de um índice de destino inválido resulta em nenhum destino selecionado. A seleção de um destino desmarca quaisquer controladores, portas do controlador, unidades, LUNs, portais de destino e grupos do portal de destino.

**TPGROUP** [\<n >]

Define o foco para o grupo do portal de destino iSCSI especificado no destino iSCSI selecionado no momento. Se nenhum grupo do portal de destino for especificado, o comando exibirá o grupo do portal de destino selecionado no momento (se houver). A especificação de um índice de grupo do portal de destino inválido resulta em nenhum grupo de portal de destino em foco.

[\<n >]

Especifica o número do objeto de \<> a ser selecionado. Se o <object number> especificado não for válido, todas as seleções existentes para objetos do tipo especificado serão limpas. Se nenhum <object number> for especificado, o objeto atual será exibido.

### <a name="setflag"></a><a name=BKMK_34></a>SetFlag

Define a unidade atualmente selecionada como um hot spare.

#### <a name="syntax"></a>Sintaxe

```
setflag drive hotspare={true | false}
```

##### <a name="parameters"></a>Parâmetros

**true**

Seleciona a unidade atualmente selecionada como um hot spare.

**false**

Desmarca a unidade selecionada no momento como um hot spare.

#### <a name="remarks"></a>Comentários

Os sobressalentes não podem ser usados para operações de associação de LUN comuns. Eles são reservados apenas para tratamento de falhas. A unidade não deve estar atualmente associada a nenhum LUN existente.

### <a name="shrink"></a><a name=BKMK_shrink></a>PodeReduzir

Reduz o tamanho do LUN selecionado.

#### <a name="syntax"></a>Sintaxe

```
shrink lun size=<n> [noerr]
```

##### <a name="parameters"></a>Parâmetros

**tamanho =**

Especifica a quantidade desejada de espaço em megabytes (MB) para reduzir o tamanho do LUN pelo. Para especificar o tamanho usando outras unidades, use um dos sufixos reconhecidos (B, KB, MB, GB, TB e PB) imediatamente após o tamanho.

**NOERR**

Especifica que qualquer falha que ocorra durante a execução desta operação será ignorada. Isso é útil no modo de script.

### <a name="standby"></a><a name=BKMK_35></a>Espera

Altera o status dos caminhos para a porta do adaptador de barramento de host (HBA) atualmente selecionada para em espera.

#### <a name="syntax"></a>Sintaxe

```
standby hbaport
```

##### <a name="parameters"></a>Parâmetros

**destes**

Altera o status dos caminhos para a porta do adaptador de barramento de host (HBA) atualmente selecionada para em espera.

### <a name="unmask"></a><a name=BKMK_36></a>unmask

Torna os LUNs atualmente selecionados acessíveis a partir dos hosts especificados.

#### <a name="syntax"></a>Sintaxe

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

##### <a name="parameters"></a>Parâmetros

**os**

Especifica que o LUN deve se tornar acessível de todos os hosts. No entanto, não é possível remover a máscara do LUN para todos os destinos em um subsistema iSCSI.

> [!IMPORTANT]
> Você deve fazer logoff do destino antes de executar o comando remover máscara de tudo.

**None**

Especifica que o LUN não deve ser acessível a nenhum host.

> [!IMPORTANT]
> Você deve fazer logoff do destino antes de executar o comando unmask LUN NONE.

**add**

Especifica que os hosts especificados devem ser adicionados à lista existente de hosts do qual esse LUN está acessível. Se esse parâmetro não for especificado, a lista de hosts fornecida substituirá a lista existente de hosts dos quais esse LUN pode ser acessado.

**WWN =**

Especifica uma lista de números hexadecimais que representam os nomes de todo o mundo dos quais o LUN ou os hosts devem se tornar acessíveis. Para mascarar/remover a máscara para um conjunto específico de hosts em um subsistema Fibre Channel, você pode digitar uma lista separada por ponto-e-vírgula de WWN para as portas nos computadores host de seu interesse.

**iniciador =**

Especifica uma lista de iniciadores iSCSI para os quais o LUN selecionado no momento deve tornar-se acessível. Para mascarar/remover a máscara para um conjunto específico de hosts em um subsistema iSCSI, você pode digitar uma lista separada por ponto-e-vírgula de nomes de iniciadores iSCSI para os iniciadores nos computadores host de seu interesse.

**desinstalar**

Se especificado, desinstala o disco associado ao LUN no sistema local antes que o LUN seja mascarado.

## <a name="scripting-diskraid"></a>Script de DiskRAID

O DiskRAID pode ser incluído no script em qualquer computador que esteja executando o Windows Server 2008 ou o Windows Server 2003 com um provedor de hardware VDS associado. Para invocar um script do DiskRAID, no prompt de comando, digite:
```
diskraid /s <script.txt>
```
Por padrão, o DiskRAID para o processamento de comandos e retorna um código de erro se houver um problema no script. Para continuar a executar o script e ignorar os erros, inclua o parâmetro NOERR no comando. Isso permite práticas úteis como usar um único script para excluir todos os LUNs em um subsistema, independentemente do número total de LUNs. Nem todos os comandos dão suporte ao parâmetro NOERR. Os erros sempre são retornados em erros de sintaxe de comando, independentemente de você ter incluído o parâmetro NOERR,

### <a name="diskraid-error-codes"></a>Códigos de erro do DiskRAID

|Código de erro|Descrição do Erro|
|----------|-----------------|
|0|Não ocorreu nenhum erro. Todo o script foi executado sem falha.|
|1|Ocorreu uma exceção fatal.|
|2|Os argumentos especificados em uma linha de comando do DiskRAID estavam incorretos.|
|3|O DiskRAID não pôde abrir o script ou o arquivo de saída especificado.|
|4|Um dos serviços que o DiskRAID usa retornou uma falha.|
|5|Ocorreu um erro de sintaxe de comando. O script falhou porque um objeto foi selecionado incorretamente ou era inválido para uso com esse comando.|

## <a name="example-interactively-view-status-of-subsystem"></a>Exemplo: exibir o status de subsistema de forma interativa

Se você quiser exibir o status do subsistema 0 em seu computador, digite o seguinte na linha de comando:
```
diskraid
```
Pressione ENTER. O seguinte é exibido:
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Para selecionar subsistema 0, digite o seguinte no prompt do DiskRAID:
```
select subsystem 0
```
Pressione ENTER. Uma saída semelhante à seguinte é exibida:
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