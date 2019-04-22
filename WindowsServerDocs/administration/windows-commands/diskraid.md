---
title: diskraid
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a565a1d5fa1bc3ff57d1578fb54cfa4553e3bb26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818867"
---
# <a name="diskraid"></a>diskraid



DiskRAID é uma ferramenta de linha de comando que lhe permite configurar e gerenciar a matriz redundante de subsistemas de armazenamento de discos independentes (ou de baixo custo) (RAID).

RAID é um método usado para padronizar e categorizar sistemas tolerantes a falhas de disco. Níveis de RAID fornecem várias combinações de custo, confiabilidade e desempenho. RAID é geralmente usado em servidores. Alguns servidores fornecem três níveis de RAID: Nível 0 (distribuição), nível 1 (espelhamento) e 5 (particionamento com paridade).

Um subsistema RAID de hardware distingue as unidades de armazenamento endereçável fisicamente uns dos outros por meio de um número de unidade lógica (LUN). Um objeto LUN deve ter pelo menos um plex e pode ter qualquer número de plex adicionais. Cada plex contém uma cópia dos dados no objeto de LUN. Plex pode ser adicionados ao e removidos de um objeto LUN.

A maioria dos comandos DiskRAID operam em uma porta de controladora (HBA) do barramento de host específico, adaptador do iniciador, portal do iniciador, provedor, subsistema, controlador, porta, unidade, LUN, portal de destino, destino ou grupo do portal de destino. Você pode usar o comando SELECT para selecionar um objeto. O objeto selecionado deve ter o foco. Foco simplifica tarefas comuns de configuração, como a criação de vários LUNs dentro do mesmo subsistema.

> [!NOTE]
> A ferramenta de linha de comando DiskRAID funciona apenas com subsistemas de armazenamento que dão suporte ao serviço de disco Virtual (VDS).

## <a name="diskraid-commands"></a>Comandos DiskRAID

Para exibir a sintaxe de comando, clique no comando:
-   [add](#BKMK_1)
-   [associate](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [chap](#BKMK_5)
-   [create](#BKMK_6)
-   [delete](#BKMK_7)
-   [detail](#BKMK_8)
-   [dissociate](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [maintenance](#BKMK_22)
-   [name](#BKMK_23)
-   [offline](#BKMK_24)
-   [online](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerate](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [remove](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [select](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [standby](#BKMK_35)
-   [unmask](#BKMK_36)

### <a name="BKMK_1"></a>add

Adiciona um LUN existente para o LUN selecionado no momento ou adiciona um portal de destino iSCSI para o grupo de portais de destino iSCSI atualmente selecionada.

#### <a name="syntax"></a>Sintaxe

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Parâmetros

**plex lun**=*n*

Especifica o número LUN para adicionar como um plex ao LUN selecionado no momento.

> [!CAUTION]
> Todos os dados no LUN que está sendo adicionado como um plex serão excluídos.

**tpgroup tportal=***n*

Especifica o número de portal de destino iSCSI para adicionar ao grupo de portais de destino iSCSI atualmente selecionada.

**noerr**

Especifica que as falhas que ocorrem ao executar esta operação serão ignoradas. Isso é útil no modo de script.

### <a name="BKMK_2"></a>associate

Define a lista especificada do controlador de portas como ativa para o LUN atualmente selecionado (outra portas ficam inativas controlador), adiciona as portas de controlador especificado à lista de portas existentes do controlador ativo para o LUN selecionado no momento ou associa o destino de iSCSI especificado para o LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Parâmetros

**Controladores**

Para uso com provedores de VDS 1.0 apenas. Adiciona ou substitui a lista de controladores que estão associados com o LUN selecionado no momento.

**Portas**

Para uso com provedores de VDS 1.1 apenas. Adiciona ou substitui a lista de portas de controlador que estão associados com o LUN selecionado no momento.

**targets**

Para uso com provedores de VDS 1.1 apenas. Adiciona ou substitui a lista de destinos iSCSI que estão associados com o LUN selecionado no momento.

**add**

Para provedores de VDS 1.0, adiciona os controladores especificados à lista existente de controladores associado ao LUN. Se esse parâmetro não for especificado, a lista de controladores substitui a lista existente de controladores associados a esse LUN.

Para provedores de VDS 1.1, adiciona as portas de controlador especificado à lista de portas do controlador associado ao LUN existente. Se esse parâmetro não for especificado, a lista de portas de controlador substitui a lista de portas do controlador associado com esse LUN existente.
```
<n>[,<n> [, ...]]
```
Para uso com o **controladores** ou **destinos** parâmetro. Especifica os números dos controladores ou destinos iSCSI para definir como ativo ou associar.
```
<n-m>[,<n-m>[,…]]
```
Para uso com o **portas** parâmetro. Especifica as portas de controlador para definir o Active Directory usando um número de controlador (*n*) e o número da porta (*m*) par.

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra como associar e adicionar portas a um LUN que usa um provedor de VDS 1.1:
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

### <a name="BKMK_3"></a>automagic

Define ou limpa os sinalizadores que dar dicas para provedores sobre como configurar um LUN. Usado sem parâmetros, o **automagic** operação exibe uma lista dos sinalizadores.

#### <a name="syntax"></a>Sintaxe

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Parâmetros

**set**

Define os sinalizadores especificados para os valores especificados.

**clear**

Limpa os sinalizadores especificados. O **todos os** palavra-chave limpa todos os sinalizadores automagic.

**apply**

Aplica os sinalizadores atuais ao LUN selecionado.

\<flag>

Sinalizadores são identificados por acrônimos de três letras.

|Flag|Descrição|
|----|-----------|
|FCR|Recuperação de pane rápida necessária|
|FTL|Tolerante a falhas|
|MSR|Principalmente leituras|
|MXD|Número máximo de Drives|
|MXS|Tamanho máximo esperado|
|ORA|Alinhamento de leitura ideal|
|ORS|Tamanho ideal de leitura|
|OSR|Otimizar para leituras sequenciais|
|OSW|Otimizar para gravações sequenciais|
|OWA|Alinhamento de gravação ideal|
|OWS|Tamanho da gravação ideal|
|RBP|Reconstruir prioridade|
|RBV|Verificar novamente a leitura habilitado|
|RPM|Remapeamento habilitado|
|STS|Tamanho de distribuição|
|WTC|Cache de gravação habilitado|
|YNK|Removível|

### <a name="BKMK_4"></a>quebra

Remove o plex do LUN selecionado no momento. O plex e os dados que ele continha não são mantidos, e as extensões da unidade podem ser recuperadas.

#### <a name="syntax"></a>Sintaxe

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Parâmetros

**plex**

Especifica o número de plex a ser removido. O plex e os dados que ele continha não serão mantidos e os recursos usados por este plex serão recuperados. Os dados contidos no LUN não são garantidos para ser consistente. Se você deseja manter este plex, use o Volume Shadow Copy Service (VSS).

**noerr**

Especifica que as falhas que ocorrem ao executar esta operação serão ignoradas. Isso é útil no modo de script.

#### <a name="remarks"></a>Comentários

> [!NOTE]
> Você deve primeiro selecionar um LUN espelhado antes de usar o **quebra** comando.

> [!CAUTION]
> Todos os dados sobre o plex serão excluídos.

> [!CAUTION]
> Todos os dados contidos no LUN original não é garantida para ser consistente.

### <a name="BKMK_5"></a>chap

Define o Challenge Handshake Authentication Protocol (CHAP) compartilhado secreto para que os iniciadores iSCSI e destinos iSCSI podem se comunicar entre si.

#### <a name="syntax"></a>Sintaxe

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Parâmetros

**conjunto de iniciador**

Define o segredo compartilhado no serviço do iniciador iSCSI local usado para autenticação de CHAP mútua quando o iniciador autentica o destino.

**Lembre-se de iniciador**

Comunica-se o segredo do CHAP de um destino iSCSI para o serviço iniciador iSCSI local para que o serviço do iniciador pode usar o segredo para se autenticar para o destino durante a autenticação CHAP.

**conjunto de destino**

Define o segredo compartilhado no destino iSCSI selecionada atualmente usado para autenticação de CHAP quando o destino autentica o iniciador.

**Lembre-se de destino**

Comunica-se o segredo do CHAP do iniciador iSCSI para o destino de iSCSI em foco atual para que o destino possa usar o segredo para se autenticar para o iniciador durante a autenticação de CHAP mútua.

**secret**

Especifica o segredo para usar. Se estiver vazio, o segredo será limpo.

**target**

Especifica um destino no subsistema selecionado no momento para associar o segredo. Isso é opcional durante a configuração de um segredo no iniciador e deixando de fora indica que o segredo será usado para todos os destinos que ainda não tiver um segredo associado.

**initiatorname**

Especifica um nome do iniciador iSCSI para associar o segredo. Isso é opcional ao definir um segredo em um destino e deixando de fora indica que o segredo será usado para todos os iniciadores que ainda não tiver um segredo associado.

### <a name="BKMK_6"></a>create

Cria um novo LUN ou o destino iSCSI no subsistema de selecionado no momento, ou cria um grupo do portal de destino no destino selecionado no momento. Você pode exibir a associação real usando o **DiskRAID lista** comando.

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

**simple**

Cria um LUN simple.

**stripe**

Cria um LUN distribuído.

**RAID**

Cria um LUN distribuído com paridade.

**mirror**

Cria um LUN espelhado.

**automagic**

Cria um LUN usando o *automagic* dicas atualmente em vigor. Consulte a **automagic** subcomandos para obter mais informações.

**size**=

Especifica o tamanho total do LUN em megabytes. Se o **tamanho =** parâmetro não for especificado, o LUN criado será o maior tamanho possível permitido por todas as unidades especificadas.

Um provedor normalmente cria um LUN, pelo menos, tão grande quanto o tamanho solicitado, mas o provedor pode ter que arredondar até o próximo tamanho maior em alguns casos. Por exemplo, se o tamanho é especificado como.99 GB e o provedor podem alocar apenas as extensões de disco GB, o LUN resultante seria 1 GB.

Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:
-   **B** de byte.
-   **KB** para quilobytes.
-   **MB** para megabyte.
-   **GB** para gigabyte.
-   **TB** para terabyte.
-   **PB** para petabyte.

**Unidades**=

Especifica o *drive_number* para as unidades a ser usado para criar um LUN. Se o **tamanho =** parâmetro não for especificado, o LUN criado é o maior tamanho possível permitido por todas as unidades especificadas. Se o **tamanho =** parâmetro for especificado, provedores irá selecionar unidades na lista unidade especificada para criar o LUN. Provedores tentará usar as unidades na ordem especificada quando possível.

**stripesize**=

Especifica o tamanho em megabytes para um *stripe* ou *RAID* LUN. O stripesize não pode ser alterado depois que o LUN é criado.

Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:
-   **B** de byte.
-   **KB** para quilobytes.
-   **MB** para megabyte.
-   **GB** para gigabyte.
-   **TB** para terabyte.
-   **PB** para petabyte.

**target**

Cria um novo destino iSCSI no subsistema de selecionado no momento.

**name**

Fornece o nome amigável para o destino.

**iscsiname**

Fontes de nome para o destino de iSCSI e podem ser omitidas para que o provedor de geram um nome.

**tpgroup**

Cria um novo grupo de portal de destino do iSCSI no destino selecionado no momento.

**noerr**

Especifica que as falhas que ocorrem ao executar esta operação serão ignoradas. Isso é útil no modo de script.

#### <a name="remarks"></a>Comentários

-   Ambos os **tamanho**= ou o **unidades**= parâmetro deve ser especificado. Eles também podem ser usados juntos.
-   O tamanho da distribuição para um LUN não pode ser alterado após a criação.

### <a name="BKMK_7"></a>delete

Exclui o LUN selecionado no momento, o destino iSCSI (contanto que existem não os LUNs associadas ao destino iSCSI) ou grupo de portais de destino iSCSI.

#### <a name="syntax"></a>Sintaxe

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Parâmetros

**lun**

Exclui o LUN selecionado no momento e todos os dados nele.

**uninstall**

Especifica que o disco no sistema local associado ao LUN será limpo antes do LUN é excluído.

**target**

Exclui o destino iSCSI atualmente selecionado se nenhum LUN está associado ao destino.

**tpgroup**

Exclui o grupo de portais de destino iSCSI atualmente selecionada.

**noerr**

Especifica que as falhas que ocorrem ao executar esta operação serão ignoradas. Isso é útil no modo de script.

### <a name="BKMK_8"></a>detail

Exibe informações detalhadas sobre o objeto atualmente selecionado do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Parâmetros

**hbaport**

Lista informações detalhadas sobre a porta de controladora (HBA) de barramento do host selecionado no momento.

**iadapter**

Lista informações detalhadas sobre o adaptador do iniciador iSCSI atualmente selecionada.

**iportal**

Lista informações detalhadas sobre o portal do iniciador iSCSI atualmente selecionada.

**provider**

Lista informações detalhadas sobre o provedor selecionado no momento.

**subsystem**

Lista informações detalhadas sobre o subsistema selecionado no momento.

**controller**

Lista informações detalhadas sobre o controlador selecionado no momento.

**port**

Lista informações detalhadas sobre a porta do controlador selecionado no momento.

**drive**

Lista informações detalhadas sobre a unidade selecionada no momento, incluindo os LUNs occupying.

**lun**

Lista informações detalhadas sobre o LUN selecionado no momento, incluindo a contribuição de unidades. A saída difere ligeiramente dependendo se o LUN faz parte de um subsistema de Fibre Channel ou iSCSI. Se a lista de Hosts sem máscara contiver apenas um asterisco, isso significa que o LUN está desmascarado para todos os hosts.

**tportal**

Lista informações detalhadas sobre o portal de destino iSCSI atualmente selecionada.

**target**

Lista informações detalhadas sobre o destino iSCSI atualmente selecionado.

**tpgroup**

Lista informações detalhadas sobre o grupo de portais de destino iSCSI atualmente selecionada.

**verbose**

Para uso apenas com o parâmetro LUN. Lista informações adicionais, incluindo seu plex.

### <a name="BKMK_9"></a>dissociate

Conjuntos especificados lista de portas do controlador como inativa para o LUN atualmente selecionado (outro controlador de portas não são afetadas), ou dissocia a lista especificada de destinos iSCSI para o LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Parâmetro

**Controladores**

Para uso com provedores de VDS 1.0 apenas. Remove os controladores na lista de controladores que estão associados com o LUN selecionado no momento.

**Portas**

Para uso com provedores de VDS 1.1 apenas. Remove as portas do controlador da lista de portas do controlador que estão associados com o LUN selecionado no momento.

**targets**

Para uso com provedores de VDS 1.1 apenas. Remove destinos da lista de destinos iSCSI que estão associados com o LUN selecionado no momento.
```
<n> [,<n> [,…]]
```
Para uso com o **controladores** ou **destinos** parâmetro. Especifica os números dos controladores ou destinos iSCSI para definir como inativo ou desassociar.
```
<n-m>[,<n-m>[,…]]
```
Para uso com o **portas** parâmetro. Especifica as portas de controlador para definir como inativo, usando um número de controlador (*n*) e o número da porta (*m*) par.

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

### <a name="BKMK_10"></a>exit

Sai DiskRAID.

#### <a name="syntax"></a>Sintaxe

```
exit
```

### <a name="BKMK_11"></a>extend

Estende o LUN selecionado no momento com a adição de setores até o final do LUN. Nem todos os provedores dão suporte estendendo LUNs. Não se estende quaisquer volumes ou sistemas de arquivos contidos no LUN. Depois de estender o LUN, você deve estender as estruturas em disco associadas usando o **DiskPart estender** comando.

#### <a name="syntax"></a>Sintaxe

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Parâmetros

**size=**

Especifica o tamanho em megabytes para estender o LUN. Se o **tamanho =** parâmetro não for especificado, o LUN é estendido pelo maior tamanho possível permitido por todas as unidades especificadas. Se o **tamanho =** parâmetro for especificado, os provedores de selecionar unidades na lista especificada pela **unidades =** parâmetro para criar o LUN.

Para especificar o tamanho usando outras unidades, use um dos seguintes sufixos reconhecidos imediatamente após o tamanho:
-   **B** de byte.
-   **KB** para quilobytes.
-   **MB** para megabyte.
-   **GB** para gigabyte.
-   **TB** para terabytes
-   **PB** para petabyte

**drives=**

Especifica o \<drive_number > para as unidades a serem usadas ao criar um LUN. Se o **tamanho =** parâmetro não for especificado, o LUN criado é o maior tamanho possível permitido por todas as unidades especificadas. Provedores de usam as unidades na ordem especificada quando possível.

**noerr**

Especifica que as falhas que ocorrem ao executar esta operação devem ser ignoradas. Isso é útil no modo de script.

#### <a name="remarks"></a>Comentários

Ambos os *tamanho* ou o \<unidade > parâmetro deve ser especificado. Eles também podem ser usados juntos.

### <a name="BKMK_12"></a>flushcache

Limpa o cache no controlador selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
flushcache controller
```

### <a name="BKMK_13"></a>Ajuda

Exibe uma lista de todos os comandos de DiskRAID.

#### <a name="syntax"></a>Sintaxe

```
help
```

### <a name="BKMK_14"></a>importtarget

Recupera ou define o destino de importação atual do Volume Shadow Copy Service (VSS) que é definido para o subsistema selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Parâmetro

**conjunto de destino**

Se especificado, define o destino selecionado atualmente para o destino de importação do VSS para o subsistema selecionado no momento. Se não for especificado, o comando recupera o destino de importação VSS atual que está definido para o subsistema selecionado no momento.

### <a name="BKMK_15"></a>Iniciador

Recupera informações sobre o iniciador iSCSI local.

#### <a name="syntax"></a>Sintaxe

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

Invalida o cache no controlador selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

Define a política de balanceamento de carga no LUN selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Parâmetros

**type**

Especifica a política de balanceamento de carga. Se o tipo não for especificado, o **caminho** parâmetro deve ser especificado. Tipo pode ser um dos seguintes:

**FAILOVER**: Usa um caminho principal com outros caminhos que estão sendo caminhos de backup.

**ROUNDROBIN**: Usa todos os caminhos no estilo round-robin, que tenta cada caminho sequencialmente.

**SUBSETROUNDROBIN**: Usa todos os caminhos principais no estilo round-robin; caminhos de backup são usados somente se todos os caminhos principais falham.

**DYNLQD**: Usa o caminho com o menor número de solicitações ativas.

**PONDERADO**: Usa o caminho com o peso mínimo (cada caminho deve ser atribuído um peso).

**LEASTBLOCKS**: Usa o caminho com o mínimo de blocos.

**VENDORSPECIFIC**: Usa uma política específica de fornecedor.

**paths**

Especifica se um caminho é **primário** ou tem um determinado \<peso >. Todos os caminhos não especificados são definidos implicitamente como backup. Todos os caminhos listados devem ser um dos caminhos de LUN selecionado no momento.

### <a name="BKMK_19"></a>list

Exibe uma lista de objetos do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Parâmetros

**hbaports**

Lista informações resumidas sobre todas as portas HBA conhecido ao VDS. A porta HBA selecionada no momento é marcada por um asterisco (*).

**você deseja selecionar**

Lista informações de resumo sobre todos os adaptadores do iniciador iSCSI conhecido ao VDS. O adaptador do iniciador selecionado no momento é marcado por um asterisco (*).

**iportals**

Lista informações de resumo sobre todos os portais do iniciador iSCSI no adaptador do iniciador selecionado no momento. O portal do iniciador selecionado no momento é marcado por um asterisco (*).

**provedores**

Lista informações de resumo sobre cada provedor conhecido ao VDS. O provedor selecionado no momento é marcado por um asterisco (*).

**subsystems**

Lista informações de resumo sobre cada subsistema no sistema. O subsistema selecionado no momento é marcado por um asterisco (*).

**Controladores**

Lista informações de resumo sobre cada controlador no subsistema selecionado no momento. O controlador selecionado no momento é marcado por um asterisco (*).

**Portas**

Lista informações de resumo sobre cada porta do controlador no controlador selecionado no momento. A porta selecionada no momento é marcada por um asterisco (*).

**drives**

Lista informações de resumo sobre cada unidade no subsistema selecionado no momento. A unidade selecionada atualmente é marcada por um asterisco (*).

**luns**

Lista informações de resumo sobre cada LUN no subsistema selecionado no momento. LUN selecionado no momento é marcado por um asterisco (*).

**tportals**

Lista informações de resumo sobre todos os portais de destino iSCSI no subsistema selecionado no momento. O portal de destino selecionado no momento é marcado por um asterisco (*).

**targets**

Lista informações de resumo sobre todos os destinos iSCSI no subsistema selecionado no momento. O destino selecionado atualmente é marcado por um asterisco (*).

**tpgroups**

Lista informações resumidas sobre todas as portal grupos de destino iSCSI no destino selecionado no momento. O grupo de portal selecionado no momento é marcado por um asterisco (*).

### <a name="BKMK_20"></a>logon

Registra o adaptador do iniciador iSCSI especificado no destino iSCSI atualmente selecionada.

#### <a name="syntax"></a>Sintaxe

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Parâmetros

**type**

Especifica o tipo de logon para executar: **manual**, **persistente**, ou **inicialização**. Se não for especificado, um logon manual será executado.

**manual** -logon manualmente.

**persistente** - automaticamente usar o mesmo logon quando o computador for reiniciado.

**inicialização** -(essa opção é para desenvolvimento futuro e não é usada atualmente *.*)

**chap**

Especifica o tipo de autenticação de CHAP para usar: **none**, **oneway** CHAP, ou **mútua** CHAP; se não for especificado, nenhuma autenticação será usada.

**tportal**

Especifica um portal de destino opcional no subsistema a ser usado para o logon selecionado no momento.

**iportal**

Especifica um portal do iniciador opcional no adaptador do iniciador especificado a ser usado para o logon.

\<flag>

Identificado pelo acrônimo de três letras:

**IPS**: Exige o protocolo IPsec

**EMP**: Habilitar múltiplos caminhos

**EHD**: Habilitar o resumo de cabeçalho

**EDD**: Habilitar o resumo dos dados

### <a name="BKMK_21"></a>logoff

Registra o adaptador do iniciador iSCSI especificado fora do destino iSCSI atualmente selecionado.

#### <a name="syntax"></a>Sintaxe

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Parâmetros

**iadapter**

Especifica o adaptador do iniciador com uma sessão de logon para efetuar logout do.

### <a name="BKMK_22"></a>maintenance

Executa operações de manutenção no objeto selecionado no momento do tipo especificado.

#### <a name="syntax"></a>Sintaxe

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Parâmetros

\<object>

Especifica o tipo de objeto no qual executar a operação. O *objeto* tipo pode ser um **subsistema**, **controlador**, **da porta, unidade** ou **LUN**.

\<operation>

Especifica a operação de manutenção a ser executada. O *operação* tipo pode ser **rotação**, **spindown**, **piscar**, **bipe** ou **ping** . Uma *operação* deve ser especificado.

**count=**

Especifica o número de vezes para repetir a *operação*. Isso normalmente é usado com **piscar**, **bipe**, ou **ping**.

### <a name="BKMK_23"></a>name

Define o nome amigável do destino iSCSI, LUN ou subsistema selecionado no momento para o nome especificado.

#### <a name="syntax"></a>Sintaxe

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Parâmetro

\<name>

Especifica um nome para o subsistema de LUN ou o destino. O nome deve ser menor que 64 caracteres de comprimento. Se nenhum nome for fornecido, o nome existente, se houver, será excluído.

### <a name="BKMK_24"></a>offline

Define o estado do objeto selecionado no momento do tipo especificado para **offline**.

#### <a name="syntax"></a>Sintaxe

```
offline <object>
```

#### <a name="parameter"></a>Parâmetro

\<object>

Especifica o tipo de objeto no qual executar essa operação. O \<objeto >

o tipo pode ser **subsistema**, **controlador**, **unidade**, **LUN**, ou **Portal**.

### <a name="BKMK_25"></a>online

Define o estado do objeto selecionado do tipo especificado para **online**. Se o objeto é **hbaport**, altera o status dos caminhos para a porta HBA selecionada no momento **online**.

#### <a name="syntax"></a>Sintaxe

```
online <object> 
```

#### <a name="parameter"></a>Parâmetro

\<object>

Especifica o tipo de objeto no qual executar essa operação. O \<objeto >

o tipo pode ser **hbaport**, **subsistema**, **controlador**, **unidade**, **LUN**, ou  **Portal**.

### <a name="BKMK_26"></a>recover

Executa operações necessárias, como a ressincronização ou reserva quente, reparar o LUN selecionado no momento e tolerante a falhas. Por exemplo, a recuperação pode causar um sobressalente ativo a ser associado a um conjunto RAID que tem um disco com falha ou outra realocação de extensão de disco.

#### <a name="syntax"></a>Sintaxe

```
recover <lun>
```

### <a name="BKMK_27"></a>reenumerate

Reenumerates objetos do tipo especificado. Se você usar a extensão de comando LUN, você deve usar o comando de atualização para atualizar o tamanho do disco antes de usar o comando reenumerate.

#### <a name="syntax"></a>Sintaxe

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Parâmetros

**subsystems**

Consulta o provedor para descobrir quaisquer novos subsistemas que foram adicionados no provedor selecionado no momento.

**drives**

Consulta de barramentos de e/s internos para descobrir todas as unidades que foram adicionadas no subsistema selecionado no momento.

### <a name="BKMK_28"></a>refresh

Atualiza os dados internos para o provedor selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
refresh provider
```

### <a name="BKMK_29"></a>rem

Usado para scripts de comentário.

#### <a name="syntax"></a>Sintaxe

```
Rem <comment>
```

### <a name="BKMK_30"></a>remove

Remove o portal de destino iSCSI especificado a partir do grupo do portal de destino selecionado no momento.

#### <a name="syntax"></a>Sintaxe

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Parâmetro

**tpgroup tportal=** \<tportal>

Especifica o portal de destino iSCSI a ser removido.

**noerr**

Especifica que as falhas que ocorrem ao executar esta operação devem ser ignoradas. Isso é útil no modo de script.

### <a name="BKMK_31"></a>replace

Substitui a unidade especificada com a unidade selecionada no momento.

#### <a name="syntax"></a>Sintaxe

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Parâmetro

**drive=**

Especifica o \<drive_number > para a unidade a ser substituído.

#### <a name="remarks"></a>Comentários

-   A unidade especificada não pode ser a unidade selecionada no momento.

### <a name="BKMK_32"></a>reset

Redefine o controlador selecionado no momento ou a porta.

#### <a name="syntax"></a>Sintaxe

```
Reset {controller | port}
```

#### <a name="parameters"></a>Parâmetros

**controller**

Redefine o controlador.

**port**

Redefine a porta.

### <a name="BKMK_33"></a>select

Exibe ou altera o objeto atualmente selecionado.

#### <a name="syntax"></a>Sintaxe

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Parâmetros

**object**

Especifica o tipo de objeto para selecionar. O \<objeto > tipo pode ser **provedor**, **subsistema**, **controlador**, **unidade**, ou **LUN**.

**hbaport** [\<n>]

Define o foco para a porta HBA local especificada. Se nenhuma porta HBA for especificada, o comando exibe a porta HBA selecionada no momento (se houver). Especificar um índice inválido de porta HBA resulta em nenhuma porta HBA de em foco. Selecionando uma porta HBA desmarca quaisquer adaptadores de iniciador selecionado e os portais do iniciador.

**iadapter** [\<n>]

Define o foco para o adaptador do iniciador iSCSI local especificado. Se nenhum adaptador de iniciador for especificado, o comando exibe o adaptador do iniciador selecionado no momento (se houver). Especificar um índice do adaptador de iniciador inválido resulta em nenhum adaptador de iniciador de em foco. Selecionar um adaptador do iniciador desmarca quaisquer portas do HBA selecionadas e os portais do iniciador.

**iportal** [\<n>]

Define o foco para o portal do iniciador iSCSI local especificado dentro do adaptador do iniciador iSCSI selecionada. Se nenhum portal iniciador for especificado, o comando exibe o portal do iniciador selecionado no momento (se houver). Especificar um índice de portal iniciador inválido resulta em nenhum portal iniciador selecionado.

**provider** [\<n>]

Define o foco para o provedor especificado. Se nenhum provedor for especificado, o comando exibe o provedor selecionado no momento (se houver). Especificar um índice de provedor inválido resulta em nenhum provedor de em foco.

**subsystem** [\<n>]

Define o foco para o subsistema especificado. Se nenhum subsistema for especificado, o comando exibe o subsistema com foco (se houver). Especificar um índice de subsistema inválido resulta em nenhum subsistema em foco. A seleção de um subsistema implicitamente seleciona seu provedor associado.

**controller** [\<n>]

Define o foco para o controlador especificado dentro do subsistema selecionado no momento. Se nenhum controlador for especificado, o comando exibe o controlador selecionado no momento (se houver). Especificar um índice de controlador inválido resulta em nenhum controlador de em foco. Selecionar um controlador desmarca quaisquer portas do controlador selecionado, unidades, LUNs, portais de destino, destinos e grupos do portal de destino.

**port** [\<n>]

Define o foco para a porta de controlador especificado dentro do controlador selecionado no momento. Se nenhuma porta for especificada, o comando exibe a porta selecionada no momento (se houver). Especificar um índice de porta inválido resulta em nenhuma porta selecionada.

**drive** [\<n>]

Define o foco para a unidade especificada, ou eixo físico, dentro do subsistema selecionado no momento. Se nenhuma unidade for especificada, o comando exibe a unidade selecionada no momento (se houver). Especificar um índice de unidade inválida resulta em nenhuma unidade de em foco. Selecionar uma unidade anula a seleção de qualquer controladores selecionadas, as portas do controlador, LUNs, portais de destino, destinos e grupos do portal de destino.

**lun** [\<n>]

Define o foco para o LUN especificado dentro do subsistema selecionado no momento. Se nenhum LUN for especificado, o comando exibe o LUN selecionado no momento (se houver). Especificar um índice inválido do LUN resulta em nenhum LUN selecionado. Selecionar um LUN anula a seleção de qualquer controladores selecionadas, as portas do controlador, unidades, portais de destino, destinos e grupos do portal de destino.

**tportal** [\<n>]

Define o foco para o portal de destino iSCSI especificado dentro do subsistema selecionado no momento. Se nenhum portal de destino for especificado, o comando exibe o portal de destino selecionado no momento (se houver). Especificar um índice do portal de destino inválido resulta em nenhum portal de destino selecionado. Selecionar um portal de destino anula a seleção de qualquer controladores, portas de controlador, unidades, LUNs, destinos e grupos do portal de destino.

**target** [\<n>]

Define o foco para o destino de iSCSI especificado dentro do subsistema selecionado no momento. Se nenhum destino for especificado, o comando exibe o destino selecionado no momento (se houver). Especificar um índice de destino inválido resulta em nenhum destino selecionado. Selecionando um destino anula a seleção de qualquer controladores, portas de controlador, unidades, LUNs, portais de destino e grupos do portal de destino.

**tpgroup** [\<n>]

Define o foco para o grupo de portais de destino iSCSI especificado dentro do destino iSCSI atualmente selecionado. Se nenhum grupo do portal de destino for especificado, o comando exibe o grupo de portais de destino selecionado no momento (se houver). Especificar um índice do grupo de portais de destino inválido resulta em nenhum grupo do portal de destino em foco.

[\<n>]

Especifica o \<número do objeto > para selecionar. Se o <object number> especificado não é válido, todas as seleções existentes para objetos do tipo especificado serão limpas. Se nenhum <object number> for especificado, o objeto atual é exibido.

### <a name="BKMK_34"></a>setflag

Define a unidade selecionada atualmente como um sobressalente ativo.

#### <a name="syntax"></a>Sintaxe

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Parâmetros

**true**

Seleciona a unidade selecionada atualmente como um sobressalente ativo.

**false**

Desmarca a unidade selecionada atualmente como um sobressalente ativo.

#### <a name="remarks"></a>Comentários

Sobressalentes não podem ser usados para operações comuns de associação de LUN. Elas estão reservadas para tratamento de falhas apenas. A unidade não deve ser atualmente associada a qualquer LUN existente.

### <a name="BKMK_shrink"></a>reduzir

Reduz o tamanho do LUN selecionado.

#### <a name="syntax"></a>Sintaxe

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Parâmetros

**size=**

Especifica a quantidade de espaço em megabytes (MB) para reduzir o tamanho do LUN desejada pelo. Para especificar o tamanho usando outras unidades, use um dos sufixos reconhecidos (B, KB, MB, GB, TB e PB) imediatamente após o tamanho.

**noerr**

Especifica que as falhas que ocorrem ao executar esta operação serão ignoradas. Isso é útil no modo de script.

### <a name="BKMK_35"></a>modo de espera

Altera o status dos caminhos para a porta de controladora (HBA) do barramento de host selecionado no momento para em espera.

#### <a name="syntax"></a>Sintaxe

```
standby hbaport
```

#### <a name="parameters"></a>Parâmetros

**hbaport**

Altera o status dos caminhos para a porta de controladora (HBA) do barramento de host selecionado no momento para em espera.

### <a name="BKMK_36"></a>unmask

Disponibilizar os LUNs selecionados no momento de hosts especificados.

#### <a name="syntax"></a>Sintaxe

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Parâmetros

**all**

Especifica que o LUN deve ser feito acessível de todos os hosts. No entanto, você não pode retirar máscara de LUN para todos os destinos em um subsistema iSCSI.

> [!IMPORTANT]
> Faça o logoff do destino antes de executar o comando MÁSCARA tudo.

**None**

Especifica que o LUN não deve ser acessível para qualquer host.

> [!IMPORTANT]
> Você deve efetuar logout do destino antes de executar o comando DESMASCARAR o LUN NONE.

**add**

Especifica que os hosts especificados devem ser adicionados à lista existente de hosts que esse LUN está acessível. Se esse parâmetro não for especificado, a lista de hosts fornecidos substitui a lista existente de hosts que esse LUN está acessível.

**WWN=**

Especifica uma lista de números hexadecimais que representam os nomes de todo o mundo do qual o LUN ou hosts devem se tornar acessíveis. Para a máscara/máscara a um conjunto específico de hosts em um subsistema de Fibre Channel, você pode digitar uma lista separada por vírgulas do WWN para as portas nos computadores host de interesse.

**initiator=**

Especifica uma lista de iniciadores iSCSI ao qual o LUN selecionado no momento deve tornar acessível. Para a máscara/máscara a um conjunto específico de hosts em um subsistema iSCSI, você pode digitar uma lista separada por vírgulas de nomes de iniciador de iSCSI para iniciadores nos computadores host de interesse.

**uninstall**

Se especificado, desinstala o disco associado ao LUN no sistema local, antes do LUN é mascarado.

## <a name="scripting-diskraid"></a>Script DiskRAID

DiskRAID possível gerar o script em qualquer computador executando o Windows Server 2008 ou Windows Server 2003 com um provedor de hardware VDS associado. Para chamar um script de DiskRAID, no prompt de comando digite:
```
diskraid /s <script.txt>
```
Por padrão, o DiskRAID interrompe o processamento de comandos e retorna um código de erro se houver um problema no script. Para continuar a execução do script e ignorar erros, inclua o GPT no comando. Isso permite que essas práticas úteis como usando um único script para excluir todos os LUNs em um subsistema, independentemente do número total de LUNs. Nem todos os comandos oferecem suporte a GPT. Erros são sempre retornados sobre erros de sintaxe de comando, independentemente se você incluiu o GPT

### <a name="diskraid-error-codes"></a>Códigos de erro DiskRAID

|Código de erro|Descrição do erro|
|----------|-----------------|
|0|Nenhum erro tiver ocorrido. Todo o script foi executado sem falhas.|
|1|Ocorreu uma exceção fatal.|
|2|Os argumentos especificados na linha de comando DiskRAID estavam incorretos.|
|3|O DiskRAID não pôde abrir o script especificado ou o arquivo de saída.|
|4|Um dos serviços que Diskraid usa retornou uma falha.|
|5|Ocorreu um erro de sintaxe de comando. O script falhou porque um objeto foi selecionado incorretamente ou era inválido para uso com o comando.|

## <a name="example-interactively-view-status-of-subsystem"></a>Exemplo: Exibir o Status do subsistema de forma interativa

Se você quiser exibir o status do subsistema de 0 em seu computador, digite o seguinte na linha de comando:
```
diskraid
```
Pressione ENTER. O seguinte é exibido:
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Para selecionar o subsistema de 0, digite o seguinte no prompt de DiskRAID:
```
select subsystem 0
```
Pressione ENTER. Saída semelhante à seguinte será exibida:
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
Para sair do DiskRAID, digite o seguinte no prompt de DiskRAID:
```
exit
```