---
title: Horário do Windows para rastreabilidade
description: As regulamentações em muitos setores exigem que os sistemas sejam rastreáveis para o UTC.  Isso significa que a diferença de um sistema pode ser atestada com respeito ao UTC.
author: dahavey
ms.author: dahavey
ms.date: 10/17/2018
ms.topic: article
ms.openlocfilehash: 297e6b3716c03c37089daef1b4658850479ae0e8
ms.sourcegitcommit: b5b040a47cf48c94852de9aad8b91475f891d2f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88563336"
---
# <a name="windows-time-for-traceability"></a>Horário do Windows para rastreabilidade
>Aplica-se a: Windows Server 2016 versão 1709 ou posterior e Windows 10 versão 1703 ou posterior

As regulamentações em muitos setores exigem que os sistemas sejam rastreáveis para o UTC.  Isso significa que a diferença de um sistema pode ser atestada com respeito ao UTC.  Para habilitar cenários de conformidade regulatória, o Windows 10 (versão 1703 ou posterior) e o Windows Server 2016 (versão 1709 ou posterior) fornecem novos logs de eventos para oferecer uma visão da perspectiva do Sistema Operacional a fim de criar uma compreensão das ações executadas no relógio do sistema.  Esses logs de evento são gerados continuamente para o Serviço de Horário do Windows e podem ser examinados ou arquivados para análise posterior.

Esses novos eventos permitem que as seguintes perguntas sejam respondidas:

* O relógio do sistema foi alterado
* A frequência do relógio foi modificada
* A configuração do Serviço de Horário do Windows foi modificada

## <a name="availability"></a>Disponibilidade

Esses aprimoramentos estão incluídos no Windows 10 versão 1703 ou posterior e no Windows Server 2016 versão 1709 ou posterior.

## <a name="configuration"></a>Configuração

Nenhuma configuração é necessária para usar este recurso.  Esses logs de eventos são habilitados por padrão e podem ser encontrados no visualizador de eventos no canal **Applications and Services Log\Microsoft\Windows\Time-Service\Operational**.

## <a name="list-of-event-logs"></a>Lista de logs de eventos

A seção a seguir descreve os eventos registrados para uso em cenários de rastreabilidade.

# <a name="257"></a>[257](#tab/257)

Esse evento é registrado em log quando o Serviço de Horário do Windows (W32Time) é iniciado e registra informações em log sobre a hora atual, a contagem atual de tiques, a configuração do runtime, os provedores de horário e a taxa atual do relógio.

|Descrição do evento |Início do serviço |
|---|---|
|Detalhes |Ocorre na inicialização do W32time |
|Dados registrados |<ul><li>Hora atual em UTC</li><li>Contagem atual de tiques</li><li>Configuração do W32Time</li><li>Configuração do Provedor de Tempo</li><li>Taxa do relógio</li></ul> |
|Mecanismo de limitação  |Nenhum. Esse evento é disparado sempre que o serviço é iniciado. |

**Exemplo:**

```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

Essas informações também podem ser consultadas usando os comandos a seguir

*Configuração do W32Time e do Provedor de Tempo*

```
w32tm.exe /query /configuration
```

*Taxa do relógio*
```
w32tm.exe /query /status /verbose
```

# <a name="258"></a>[258](#tab/258)

Esse evento é registrado em log quando o Serviço de Horário do Windows (W32Time) está parando e registra em logs as informações sobre a contagem atual de tempo e de tiques.

|Descrição do evento |Interrupção do serviço |
|---|---|
|Detalhes |Ocorre no desligamento do W32time |
|Dados registrados |<ul><li>Hora atual em UTC</li><li>Contagem atual de tiques</li></ul> |
|Mecanismo de limitação  |Nenhum. Esse evento é disparado sempre que o serviço é interrompido. |

**Texto de exemplo:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259"></a>[259](#tab/259)

Esse evento registra periodicamente em log a lista atual de fontes de horário e a fonte de horário escolhida dele.  Além disso, ele registra em log a contagem atual de tiques.  Esse evento não é disparado sempre que uma fonte de horário é alterada.  Outros eventos listados mais adiante neste documento oferecem essa funcionalidade.

|Descrição do evento |Status periódico do provedor de cliente NTP |
|---|---|
|Detalhes |Lista de fontes de horário usadas pelo cliente NTP |
|Dados registrados |<ul><li>Fontes de tempo disponíveis</li><li>O servidor de tempo de referência escolhido no momento do registro em log</li><li>Contagem atual de tiques</li></ul> |
|Mecanismo de limitação  |Registrado a cada 8 horas. |

**Texto de exemplo:** status periódico do provedor de cliente NTP:

O cliente NTP está recebendo dados de tempo dos seguintes servidores NTP:

server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123); e o servidor de horário de referência escolhido é o Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123) (RefID:0x08d6648e63). Contagem de tiques do sistema 13187937

**Comando** Essas informações também podem ser consultadas usando os comandos a seguir

*Identificar pares*
`w32tm.exe /query /peers`

# <a name="260"></a>[260](#tab/260)

|Descrição do evento |Configuração e status do serviço de horário |
|---|---|
|Detalhes |O W32time registra em log periodicamente a configuração e o status dele. Esse é o equivalente de chamar:<br><br>`w32tm /query /configuration /verbose`<br>OU<br>`w32tm /query /status /verbose` |
|Mecanismo de limitação  |Registrado a cada 8 horas. |

# <a name="261"></a>[261](#tab/261)

Isso registra em log cada instância quando a Hora do Sistema é modificada usando a API SetSystemTime.

|Descrição do evento |A Hora do Sistema foi definida |
|---|---|
|Mecanismo de limitação  |Nenhum.<p>Isso deve acontecer raramente em sistemas com sincronização de tempo razoável e quando queremos registrar em log sempre que ocorrer. Ignoramos a configuração TimeJumpAuditOffset ao registrar esse evento em log porque essa configuração era destinada a limitar eventos no log de eventos do sistema do Windows. |

# <a name="262"></a>[262](#tab/262)

|Descrição do evento |Frequência do relógio do sistema ajustada |
|---|---|
|Detalhes |A frequência do relógio do sistema é modificada constantemente pelo W32time quando o relógio está em uma sincronização próxima. Queremos capturar ajustes "razoavelmente significativos" feitos na frequência do relógio sem executar o log de eventos excessivamente. |
|Mecanismo de limitação  |Todos os ajustes de relógio abaixo de TimeAdjustmentAuditThreshold (mín. = 128 partes por milhão, padrão = 800 partes por milhão) não são registrados em log.<p>A alteração de 2 PPM na frequência do relógio com granularidade atual gera uma alteração de 120 µsec/s na precisão do relógio.<p>Em um sistema sincronizado, a maioria dos ajustes fica abaixo desse nível. Se você quiser um acompanhamento mais preciso, essa configuração poderá ser ajustada, você poderá usar PerfCounters ou poderá fazer ambos. |

# <a name="263"></a>[263](#tab/263)

|Descrição do evento |Alteração nas configurações do Serviço de Horário ou em uma lista de provedores de horário carregados. |
|--|--|
|Detalhes |A releitura das configurações do W32time pode fazer determinadas configurações críticas serem modificadas na memória, o que pode afetar a precisão geral da sincronização de tempo.<br><br>O W32time registra cada ocorrência em log ao reler as configurações, o que oferece o impacto potencial na sincronização de tempo. |
|Mecanismo de limitação  |Nenhum.<br><br>Esse evento ocorre somente quando uma atualização de administrador ou GP altera os provedores de horário e dispara o W32time. Queremos registrar cada instância de alteração de configurações. |

# <a name="264"></a>[264](#tab/264)

|Descrição do evento |Alteração nas fontes de horário usadas pelo cliente NTP |
|---|---|
|Detalhes |O cliente NTP registra um evento com o estado atual dos servidores de tempo/pares quando o estado de um servidor de tempo/par é alterado (**Pendente ->Sincronização**, **Sincronizar -> inacessível** ou outras transições) |
|Mecanismo de limitação  |Frequência máxima – apenas uma vez a cada 5 minutos para proteger o log contra problemas transitórios e uma implementação de provedor inadequada. |

# <a name="265"></a>[265](#tab/265)

|Descrição do evento |Alterações no número de estratos ou na fonte do Serviço de Horário |
|---|---|
|Detalhes |A fonte de horário e o número de estratos do W32time são fatores importantes na rastreabilidade de tempo. As alterações delas devem ser registradas em log. Se o W32time não tiver nenhuma fonte de horário e você não tiver configurado como uma fonte de horário confiável, ele interromperá a publicidade como um servidor de horário e responderá a solicitações com alguns parâmetros inválidos, por padrão. Esse evento é essencial para acompanhar as alterações de estado em uma topologia de NTP. |
|Mecanismo de limitação  |Nenhum. |

# <a name="266"></a>[266](#tab/266)

|Descrição do evento |Uma nova sincronização de tempo é solicitada |
|---|---|
|Detalhes |Essa operação é disparada:<ul><li>Quando ocorrem alterações na rede</li><li>O sistema retorna do modo de espera/hibernação conectado</li><li>Quando não sincronizamos há muito tempo</li><li>O administrador emite o comando resync</li></ul>Essa operação resulta em perda imediata de precisão de sincronização de tempo refinada porque faz o cliente NTP apagar seus filtros. |
|Mecanismo de limitação  |Frequência máxima – uma vez a cada cinco minutos.<br><br>É possível que uma placa de rede incorreta (ou um script inadequado) possa disparar essa operação repetidamente e causar a sobrecarga dos logs. Daí a necessidade de restringir esse evento.<br><br>Observe que a sincronização de horário preciso leva muito mais do que 5 minutos para ser atingida e a limitação não perde informações sobre o evento original que resultou em perda de precisão de horário.  |
