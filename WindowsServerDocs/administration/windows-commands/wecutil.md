---
title: wecutil
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 04fb86d813049dc5f0aa6d4fba51e45dccbd1b80
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440186"
---
# <a name="wecutil"></a>wecutil



Permite que você criar e gerenciar assinaturas de eventos que são encaminhados de computadores remotos. O computador remoto deve oferecer suporte o protocolo WS-Management. Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).


## <a name="syntax"></a>Sintaxe

```
wecutil  [{es | enum-subscription}] 
[{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]] 
[{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]] 
[{ss | set-subscription} [<Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]] [/c:<Configfile> [/cun:<Username> /cup:<Password>]]] 
[{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]] 
[{ds | delete-subscription} <Subid>] 
[{rs | retry-subscription} <Subid> [<Eventsource>…]] 
[{qc | quick-config} [/q:[<Quiet>]]].
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|{es \| enum-subscription}|Exibe os nomes de todas as assinaturas de evento remoto que existem.|
|{gs \| get-subscription} \<Subid > [/ f\<formato >] [/ uni:\<Unicode >]|Exibe informações de configuração de assinatura remotos. \<Subid > é uma cadeia de caracteres que identifica exclusivamente uma assinatura. \<Subid > é o mesmo que a cadeia de caracteres que foi especificada no \<SubscriptionId > marca do arquivo de configuração XML, que era usado para criar a assinatura.|
|{gr \| get-subscriptionruntimestatus} \<Subid > [\<Eventsource >...]|Exibe o status de tempo de execução de uma assinatura. \<Subid > é uma cadeia de caracteres que identifica exclusivamente uma assinatura. \<Subid > é o mesmo que a cadeia de caracteres que foi especificada no \<SubscriptionId > marca do arquivo de configuração XML, que era usado para criar a assinatura. \<EventSource > é uma cadeia de caracteres que identifica um computador que serve como uma origem de eventos. \<EventSource > deve ser um nome de domínio totalmente qualificado, um nome NetBIOS ou endereço IP.|
|{ss \| set-subscription} \<Subid > [/ e: [\<Subenabled >]] [/ esa:\<endereço >] [/ ese: [\<Srcenabled >]] [/aes] [/res] [/ Cancelar:\<Username >] [/ até:\< Senha >] [/ unidade d:\<Desc >] [/ uri:\<Uri >] [/ cm:\<Configmode >] [/ ex:\<Expires >] [/ p:\<consulta >] [/ dia:\<dialeto >] [/ tn:\< TransportName >] [/ tp:\<Transportport >] [/ dm:\<Deliverymode >] [/ dmi:\<Deliverymax >] [/ dmlt:\<Deliverytime >] [/ Olá:\<pulsação >] [/ cf:\< Conteúdo >] [/ l:\<localidade >] [/ ree: [\<Readexist >]] [/ lf:\<arquivo de log >] [/ pn:\<Publishername >] [/ essp:\<Enableport >] [/ hn:\< Nome do host >] [/ ct:\<tipo >]</br>ou</br>{ss \| /c: a assinatura de conjunto\<Configfile > [/ cun:\<Comusername >/copo:\<Compassword >]|Altera a configuração de assinatura. Você pode especificar a ID da assinatura e as opções apropriadas para alterar os parâmetros de assinatura, ou você pode especificar um arquivo de configuração XML para alterar os parâmetros de assinatura.|
|{cs \| Criar assinatura} \<Configfile > [/ cun:\<nome de usuário >/copo:\<senha >]|Cria uma assinatura remota. \<ConfigFile > especifica o caminho para o arquivo XML que contém a configuração de assinatura. O caminho pode ser absoluto ou relativo ao diretório atual.|
|{ds \| excluir-assinatura} \<Subid >|Exclui uma assinatura e cancelamento de assinatura de todas as fontes de evento que entregam eventos no log de eventos para a assinatura. Todos os eventos já recebidos e registrados não serão excluídos. \<Subid > é uma cadeia de caracteres que identifica exclusivamente uma assinatura. \<Subid > é o mesmo que a cadeia de caracteres que foi especificada no \<SubscriptionId > marca do arquivo de configuração XML, que era usado para criar a assinatura.|
|{rs \| repetição-subscription} \<Subid > [\<Eventsource >...]|Tentativas para estabelecer uma conexão e enviar uma solicitação de assinatura remota para uma assinatura inativa. Tentar se reativar a todas as fontes de evento ou fontes de eventos específicas. Fontes desabilitadas não são repetidas. \<Subid > é uma cadeia de caracteres que identifica exclusivamente uma assinatura. \<Subid > é o mesmo que a cadeia de caracteres que foi especificada no \<SubscriptionId > marca do arquivo de configuração XML, que era usado para criar a assinatura. \<EventSource > é uma cadeia de caracteres que identifica um computador que serve como uma origem de eventos. \<EventSource > deve ser um nome de domínio totalmente qualificado, um nome NetBIOS ou endereço IP.|
|{qc \| quick-config} [/q:[\<Quiet>]]|Configura o serviço coletor de eventos do Windows para garantir que uma assinatura pode ser criada e mantida por meio de reinicializações. Isso inclui as seguintes etapas:</br>1.  Habilite o canal ForwardedEvents se ele estiver desabilitado.</br>2.  Defina o serviço coletor de eventos do Windows para atrasar o início.</br>3.  Inicie o serviço do coletor de eventos do Windows se ele não está em execução.|

## <a name="options"></a>Opções

|Opção|Descrição|
|------|-----------|
|/f:\<Format>|Especifica o formato das informações que são exibidos. \<Formato > pode ser Terse ou XML. Se <Format> é XML, a saída é exibida no formato XML. Se \<formato > é Terse, a saída é exibida em pares de nome-valor. O padrão é Terse.|
|/c:\<Configfile>|Especifica o caminho para o arquivo XML que contém uma configuração de assinatura. O caminho pode ser absoluto ou relativo ao diretório atual. Essa opção só pode ser usada com o **/cun** e **/copo** opções e é mutuamente exclusivo com todas as outras opções.|
|/e:[\<Subenabled>]|Habilita ou desabilita uma assinatura. \<Subenabled > pode ser true ou false. O valor padrão dessa opção é verdadeiro.|
|/esa:\<Address>|Especifica o endereço de uma origem do evento. \<Endereço > é uma cadeia de caracteres que contém um nome de domínio totalmente qualificado, um nome NetBIOS ou endereço IP, que identifica um computador que serve como uma origem de eventos. Essa opção deve ser usada com o **/ese**, **/aes**, **/res**, ou **/un** e **/up** opções.|
|/ESE: [\<Srcenabled >]|Habilita ou desabilita uma origem do evento. \<Srcenabled > pode ser true ou false. Essa opção só será permitida se o **/esa** opção for especificada. O valor padrão dessa opção é verdadeiro.|
|/aes|Adiciona a origem do evento que é especificada pelo **/esa** opção se já não é uma parte da assinatura. Se o endereço especificado o **/esa** opção já é uma parte da assinatura, um erro será relatado. Essa opção só será permitida se o **/esa** opção for especificada.|
|/res|Remove a origem do evento que é especificada pelo **/esa** opção se ele já é uma parte da assinatura. Se o endereço especificado o **/esa** opção não é uma parte da assinatura, um erro será relatado. Essa opção só será permitida se **/esa** opção for especificada.|
|/un:\<Username >|Especifica a credencial do usuário a ser usado com a origem do evento especificada pelo **/esa** opção. Essa opção só será permitida se o **/esa** opção for especificada.|
|/ acima:\<senha >|Especifica a senha que corresponde à credencial do usuário. Essa opção só será permitida se o **/un** opção for especificada.|
|/d:\<Desc>|Fornece uma descrição para a assinatura.|
|/uri:\<Uri>|Especifica o tipo dos eventos que são consumidos pela assinatura. \<URI > contém uma cadeia de caracteres URI que é combinada com o endereço do computador de origem de evento para identificar exclusivamente a origem dos eventos. A cadeia de caracteres do URI é usada para todos os endereços de origem do evento na assinatura.|
|/cm:\<Configmode>|Define o modo de configuração. \<Configmode > pode ser uma das seguintes cadeias de caracteres: Normal, personalizado, MinLatency ou MinBandwidth. Os modos Normal, MinLatency e MinBandwidth definir o modo de entrega, itens máxima de entrega, intervalo de pulsação e tempo de latência máxima de entrega. O **/dm**, **/dmi**, **/hi** ou **/dmlt** opções só podem ser especificado se o modo de configuração estiver definido como personalizado.|
|/ex:\<Expires>|Define a hora em que a assinatura expira. \<Expira > deve ser definido no formato de data e hora padrão de XML ou ISO8601: yyyy-MM-ddTHH [.sss] [Z], onde T é o separador de hora e Z indica a hora UTC.|
|/q:\<Query>|Especifica a cadeia de caracteres de consulta para a assinatura. O formato do \<consulta > podem ser diferentes para diferentes valores URI e se aplica a todas as fontes na assinatura.|
|/dia:\<dialeto >|Define o dialeto que usa a cadeia de caracteres de consulta.|
|/tn:\<Transportname>|Especifica o nome do transporte que é usado para se conectar a uma fonte de evento remoto.|
|/TP:\<Transportport >|Define o número da porta que é usado pelo transporte ao se conectar a uma fonte de evento remoto.|
|/DM:\<Deliverymode >|Especifica o modo de entrega. \<DeliveryMode > pode ser pull ou push. Essa opção só é válida se o **/cm** opção for definida como personalizado.|
|/dmi:\<Deliverymax>|Define o número máximo de itens para entrega em lote. Essa opção é válida somente se **/cm** estiver definido como personalizado.|
|/dmlt:\<Deliverytime >|Define a latência máxima no fornecimento de um lote de eventos. \<Deliverytime > é o número de milissegundos. Essa opção é válida somente se **/cm** estiver definido como personalizado.|
|/hi:\<Heartbeat>|Define o intervalo de pulsação. \<Pulsação > é o número de milissegundos. Essa opção é válida somente se **/cm** estiver definido como personalizado.|
|/cf:\<Content>|Especifica o formato dos eventos que são retornados. \<Conteúdo > pode ser RenderedText ou eventos. Quando o valor for RenderedText, os eventos são retornados com as cadeias de caracteres localizadas (como a descrição do evento) anexadas ao evento. O valor padrão é RenderedText.|
|/l:\<Locale>|Especifica a localidade para a entrega das cadeias de caracteres localizadas no formato RenderedText. \<Localidade > é um identificador de idioma e país/região, por exemplo, "EN-us". Essa opção só é válida se o **/CF** opção for definida como RenderedText.|
|/ree:[\<Readexist>]|Identifica os eventos que são entregues para a assinatura. \<Readexist > pode true ou false. Quando o <Readexist> for true, todos os eventos existentes são lidos de origens de evento de assinatura. Quando o <Readexist> é false, apenas eventos futuros (que chegam) são entregues. O valor padrão é verdadeiro para um **/ree** opção sem um valor. Se nenhum **/ree** opção for especificada, o valor padrão é false.|
|/LF:\<Logfile >|Especifica o log de eventos local que é usado para armazenar eventos recebidos de fontes de evento.|
|/pn:\<Publishername>|Especifica o nome do publicador. Ele deve ser um publicador que possui ou importa o log especificado pelo **/lf** opção.|
|/essp:\<Enableport>|Especifica que o número da porta deve ser anexado ao nome do serviço principal do serviço remoto. \<Enableport > pode ser true ou false. O número da porta é acrescentado ao <Enableport> é verdadeiro. Quando o número da porta for anexado, alguma configuração talvez precise impedir o acesso a fontes de evento de que está sendo negada.|
|/HN:\<Hostname >|Especifica o nome DNS do computador local. Esse nome é usado pela fonte do evento remoto ser recuado eventos e deve ser usado apenas para uma assinatura push.|
|/CT:\<tipo >|Define o tipo de credencial para o acesso remoto de origem. \<Tipo > deve ser um dos seguintes valores: padrão, negotiate, digest, basic ou localmachine. O valor padrão é o padrão.|
|/cun:\<Comusername >|Define a credencial de usuário compartilhada a ser usado para fontes de evento que não têm suas próprias credenciais de usuário. Se essa opção for especificada com o **/c** , nome de usuário e UserPassword as configurações de opções para fontes de evento individuais do arquivo de configuração são ignorados. Se você quiser usar uma credencial diferente para uma fonte de evento específico, você deve substituir esse valor, especificando o **/un** e **/up** opções para uma fonte de evento específico na linha de comando de outra **ss** comando.|
|/ copo:\<Compassword >|Define a senha do usuário para a credencial de usuário compartilhadas. Quando \<Compassword > é definida como * (asterisco), a senha é lido do console. Essa opção é válida somente quando o **/cun** opção for especificada.|
|/q:[\<Quiet>]|Especifica se o procedimento de configuração solicita confirmação. \<Silencioso > pode ser true ou false. Se <Quiet> for true, o procedimento de configuração não solicita confirmação. O valor padrão dessa opção é false.|

## <a name="remarks"></a>Comentários

> [!IMPORTANT]
> Se você receber a mensagem "o servidor RPC não está disponível? Quando você tenta executar wecutil, você precisará iniciar o serviço coletor de eventos do Windows (wecsvc). Para iniciar wecsvc, em um prompt de comando com privilégios elevados, digite net iniciar wecsvc.

- O exemplo a seguir mostra o conteúdo de um arquivo de configuração:  
  ```
  <Subscription xmlns="https://schemas.microsoft.com/2006/03/windows/events/subscription">
  <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
  <ConfigurationMode>Normal</ConfigurationMode>
  <Description>Forward Sample Subscription</Description>
  <SubscriptionId>SampleSubscription</SubscriptionId>
  <Query><![CDATA[
  <QueryList>
  <Query Path="Application">
  <Select>*</Select>
  </Query>
  </QueryList>
  ]]></Query>
  <EventSources>
  <EventSource Enabled="true">
  <Address>mySource.myDomain.com</Address>
  <UserName>myUserName</UserName>
  <Password>*</Password>
  </EventSource>
  </EventSources>
  <CredentialsType>Default</CredentialsType>
  <Locale Language="EN-US"></Locale>
  </Subscription>
  ```

## <a name="BKMK_examples"></a>Exemplos

Informações de configuração para uma assinatura denominada sub1 de saída:
```
wecutil gs sub1
```
Exemplo de saída:
```
EventSource[0]:
Address: localhost
Enabled: true
Description: Subscription 1
Uri: wsman:microsoft/logrecord/sel
DeliveryMode: pull
DeliveryMaxSize: 16000
DeliveryMaxItems: 15
DeliveryMaxLatencyTime: 1000
HeartbeatInterval: 10000
Locale:
ContentFormat: renderedtext
LogFile: HardwareEvents
```
Exiba o status de tempo de execução de uma assinatura denominada sub1:
```
wecutil gr sub1
```
Atualize a configuração de assinatura de nome sub1 de um novo arquivo XML chamado WsSelRg2.xml:
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
Atualize a configuração de assinatura chamada sub2 com vários parâmetros:
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
Criar uma assinatura para encaminhar eventos de um log de eventos do aplicativo do Windows Vista de um computador remoto em mySource.myDomain.com no log ForwardedEvents (consulte comentários para obter um exemplo de um arquivo de configuração):
```
wecutil cs subscription.xml
```
Exclua uma assinatura denominada sub1:
```
wecutil ds sub1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
