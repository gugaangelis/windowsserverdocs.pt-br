---
title: Usando o ETW para solucionar problemas de conexões LDAP
description: Como ativar e usar o ETW para rastrear conexões LDAP entre AD DS controladores de domínio.
author: Teresa-Motiv
manager: dcscontentpm
ms.prod: windows-server-dev
ms.technology: active-directory-lightweight-directory-services
audience: Admin
ms.author: v-tea
ms.topic: article
ms.date: 11/22/2019
ms.openlocfilehash: f7b7df714dbd02b15555fa20c70c1e995e121a48
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822929"
---
# <a name="using-etw-to-troubleshoot-ldap-connections"></a>Usando o ETW para solucionar problemas de conexões LDAP

O [ETW (rastreamento de eventos para Windows)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) pode ser uma importante ferramenta de solução de problemas para Active Directory Domain Services (AD DS). Você pode usar o ETW para rastrear as comunicações do protocolo[LDAP](https://docs.microsoft.com/previous-versions/windows/desktop/ldap/lightweight-directory-access-protocol-ldap-api)entre os clientes do Windows e os servidores LDAP, incluindo AD DS controladores de domínio.

## <a name="how-to-turn-on-etw-and-start-a-trace"></a>Como ativar o ETW e iniciar um rastreamento

**Para ativar o ETW**

1. Abra o editor do registro e crie a seguinte subchave do registro:

   **HKEY\_máquina de\_LOCAL\\sistema\\CurrentControlSet\\Services\\rastreamento de\\LDAP\\* processname***

   Nesta subchave, o *ProcessName* é o nome completo do processo que você deseja rastrear, incluindo sua extensão (por exemplo, "svchost. exe").

1. (**Opcional**) Nessa subchave, crie uma nova entrada denominada **pid**. Para usar essa entrada, atribua uma ID de processo como um valor DWORD.  

   Se você especificar uma ID de processo, o ETW rastreará apenas a instância do aplicativo que tem essa ID de processo.

**Para iniciar uma sessão de rastreamento**

- Abra uma janela de prompt de comando e execute o seguinte comando:

   ```cmd
   tracelog.exe -start <SessionName> -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f <FileName> -flag <TraceFlags>
   ```

   Os espaços reservados neste comando representam os valores a seguir.

  - \<*sessionname*> é um identificador arbitrário que é usado para rotular a sessão de rastreamento.  
  > [!NOTE]  
  > Você precisará se referir a esse nome de sessão posteriormente quando parar a sessão de rastreamento.
  - \<*nome de arquivo*> especifica o arquivo de log no qual os eventos serão gravados.
  - \<*sinalizadores*> deve ser um ou mais dos valores listados na tabela de sinalizadores de [rastreamento](#values-for-trace-flags).

## <a name="how-to-end-a-tracing-session-and-turn-off-event-tracing"></a>Como finalizar uma sessão de rastreamento e desativar o rastreamento de eventos

**Para interromper o rastreamento**

- No prompt de comando, execute o seguinte comando:

   ```cmd
   tracelog.exe -stop <SessionName>
   ```

   Neste comando, \<*sessionname*> é o mesmo nome que você usou no comando **tracelog. exe-start** .

**Para desativar o ETW**

- No editor do registro, exclua o **HKEY\_computador\_LOCAL\\sistema\\CurrentControlSet\\Services\\ldap\\Tracing\\* ProcessName*** subkey.

## <a name="values-for-trace-flags"></a>Valores para sinalizadores de rastreamento

Para usar um sinalizador, substitua o valor do sinalizador para o <*sinalizadores*> espaço reservado nos argumentos do comando **tracelog. exe-start** .

> [!NOTE]  
> Você pode especificar vários sinalizadores usando a soma dos valores de sinalizador apropriados. Por exemplo, para especificar os sinalizadores de *sinalizadores* **\_de pesquisa** (0X00000001) e de **cache\_de depuração** (0x00000010), o valor de > de \<apropriado é **0x00000011**.

|Nome do sinalizador |Valor do sinalizador |Descrição do sinalizador |
| --- | --- | --- |
|**DEBUG_SEARCH** |0x00000001 |Registra solicitações de pesquisa e os parâmetros que são passados para essas solicitações. As respostas não estão registradas aqui. Somente as solicitações de pesquisa são registradas. (Use **DEBUG_SPEWSEARCH** para registrar as respostas em solicitações de pesquisa.) |
|**DEBUG_WRITE** |0x00000002 |Registra solicitações de gravação e os parâmetros que são passados para essas solicitações. As solicitações de gravação incluem as operações adicionar, excluir, modificar e estendidas. |
|**DEBUG_REFCNT** |0x00000004 |Registra dados de contagem de referência e operações para conexões e solicitações. |
|**DEBUG_HEAP** |0x00000008 |Registra todas as alocações de memória e as versões de memória. |
|**DEBUG_CACHE** |0x00000010 |Registra a atividade de cache. Essa atividade inclui adições, remoções, ocorrências, erros e assim por diante. |
|**DEBUG_SSL** |0x00000020 |Registra erros e informações de SSL. |
|**DEBUG_SPEWSEARCH** |0x00000040 |Registra todas as respostas do servidor para solicitações de pesquisa. Essas respostas incluem os atributos que foram solicitados, além de todos os dados recebidos. |
|**DEBUG_SERVERDOWN** |0x00000080 |Registra erros de conexão e servidor. |
|**DEBUG_CONNECT** |0x00000100 |Registra os dados relacionados ao estabelecimento de uma conexão.<br />Use **DEBUG_CONNECTION** para registrar outros dados relacionados a conexões. |
|**DEBUG_RECONNECT** |0x00000200 |Registra a atividade de reconexão automática. Essa atividade inclui tentativas de reconexão, falhas e erros relacionados. |
|**DEBUG_RECEIVEDATA** |0x00000400 |Registra a atividade que está relacionada ao recebimento de mensagens do servidor. Essa atividade inclui eventos como "aguardando a resposta do servidor" e a resposta que é recebida do servidor. |
|**DEBUG_BYTES_SENT** |0x00000800 |Registra todos os dados enviados pelo cliente LDAP para o servidor. Essa função é essencialmente o registro em log de pacotes, mas ela sempre registra dados não criptografados. (Se um pacote for enviado por SSL, essa função registrará o pacote não criptografado.) Esse registro em log pode ser detalhado. Esse sinalizador provavelmente é melhor usado por si só ou combinado com **DEBUG_BYTES_RECEIVED**. |
|**DEBUG_EOM** |0x00001000 |Registra eventos relacionados a atingir o final de uma lista de mensagens. Esses eventos incluem informações como "lista de mensagens limpas" e assim por diante. |
|**DEBUG_BER** |0x00002000 |Registra as operações e os erros relacionados às regras de codificação básicas (BER). Essas operações e erros incluem problemas de codificação, problemas de tamanho de buffer e assim por diante. |
|**DEBUG_OUTMEMORY** |0x00004000 |Registra falhas para alocar memória. Também registra qualquer falha ao calcular a memória necessária (por exemplo, um estouro que ocorre ao calcular o tamanho do buffer necessário). |
|**DEBUG_CONTROLS** |0x00008000 |Registra dados relacionados a controles. Esses dados incluem controles que são inseridos, problemas que afetam controles, controles obrigatórios em uma conexão e assim por diante. |
|**DEBUG_BYTES_RECEIVED** |0x00010000 |Registra todos os dados recebidos pelo cliente LDAP. Esse comportamento é essencialmente o registro em log de pacotes, mas ele sempre registra dados não criptografados. (Se um pacote for enviado por SSL, essa opção registrará o pacote não criptografado.) Esse tipo de registro em log pode ser detalhado. Esse sinalizador provavelmente é melhor usado por si só ou combinado com **DEBUG_BYTES_SENT**. |
|**DEBUG_CLDAP** |0x00020000 |Registra eventos que são específicos para UDP e LDAP sem conexão. |
|**DEBUG_FILTER** |0x00040000 |Registra eventos e erros que são encontrados quando você cria um filtro de pesquisa.<br/>**Observação** Essa opção registra os eventos do cliente somente durante a construção do filtro. Ele não registra nenhuma resposta do servidor sobre um filtro. |
|**DEBUG_BIND** |0x00080000 |Registra eventos e erros de associação. Esses dados incluem informações de negociação, êxito de ligação, falha de ligação e assim por diante. |
|**DEBUG_NETWORK_ERRORS** |0x00100000 |Registra erros gerais de rede. Esses dados incluem erros de envio e recebimento.<br/>**Observação** Se uma conexão for perdida ou o servidor não puder ser acessado, **DEBUG_SERVERDOWN** será a marca preferida. |
|**DEBUG_VERBOSE** |0x00200000 |Registra mensagens gerais. Use essa opção para todas as mensagens que tendem a gerar uma grande quantidade de saída. Por exemplo, ele registra mensagens como "fim da mensagem atingida", "o servidor ainda não respondeu" e assim por diante. Essa opção também é útil para mensagens genéricas. |
|**DEBUG_PARSE** |0x00400000 |Registra eventos e erros de mensagens gerais, além de eventos de codificação e análise de pacotes e erros. |
|**DEBUG_REFERRALS** |0x00800000 |Registra dados sobre referências e caça referências. |
|**DEBUG_REQUEST** |0x01000000 |Registra o rastreamento de solicitações. |
|**DEBUG_CONNECTION** |0x02000000 |Registra erros e dados de conexão geral. |
|**DEBUG_INIT_TERM** |0x04000000 |Registra a inicialização e a limpeza do módulo (DLL principal e assim por diante). |
|**DEBUG_API_ERRORS** |0x08000000 |Dá suporte ao log de uso incorreto da API. Por exemplo, essa opção registrará dados se a operação de associação for chamada duas vezes na mesma conexão. |
|**DEBUG_ERRORS** |0x10000000 |Registra erros gerais. A maioria desses erros pode ser categorizada como erros de inicialização de módulo, erros de SSL ou erros de estouro ou Subfluxo. |
|**DEBUG_PERFORMANCE** |0x20000000 |Registra dados sobre estatísticas de atividade de LDAP global do processo depois de receber uma resposta do servidor para uma solicitação LDAP. |

## <a name="example"></a>{1&gt;Exemplo&lt;1}

Considere um aplicativo, App1. exe, que define senhas para contas de usuário. Suponha que App1. exe produz um erro inesperado. Para usar o ETW para ajudar a diagnosticar esse problema, siga estas etapas:

1. No editor do registro, crie a seguinte entrada de registro:

   **HKEY\_computador de\_LOCAL\\sistema\\CurrentControlSet\\Services\\rastreamento de\\LDAP\\App1. exe**

1. Para iniciar uma sessão de rastreamento, abra uma janela de prompt de comando e execute o seguinte comando:

   ```cmd
   tracelog.exe -start ldaptrace -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f .\ldap.etl -flag 0x80000
   ```

   Depois que esse comando é iniciado, **DEBUG\_BIND** garante que o ETW grave mensagens de rastreamento.\\LDAP. etl.

1. Inicie o App1. exe e reproduza o erro inesperado.

1. Para interromper a sessão de rastreamento, execute o seguinte comando no prompt de comando:

   ```cmd
    tracelog.exe -stop ldaptrace
   ```

1. Para impedir que outros usuários rastreamentom o aplicativo, exclua o **HKEY\_computador\_LOCAL**\\**sistema**\\**CurrentControlSet**\\**Services**\\**LDAP**\\**Tracing**\\a entrada do registro **App1. exe** .

1. Para examinar as informações no log de rastreamento, execute o seguinte comando no prompt de comando:

   ```cmd
    tracerpt.exe .\ldap.etl -o -report
    ```

   > [!NOTE]  
   > Neste comando, **Tracerpt. exe** é uma ferramenta de [consumidor de rastreamento](https://go.microsoft.com/fwlink/p/?linkid=83876) .
