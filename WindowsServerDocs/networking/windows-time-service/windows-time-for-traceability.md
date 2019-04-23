---
ms.assetid: ''
title: Tempo do Windows para rastreamento
description: Regulamentos em muitos setores exigem sistemas para ser rastreadas para UTC.  Isso significa que o deslocamento do sistema pode ser Atestado em relação ao UTC.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: e25217feba45516cd0e9a3aa2bf1a2581d2087f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838037"
---
# <a name="windows-time-for-traceability"></a>Tempo do Windows para rastreamento
>Aplica-se a: Windows Server 2016 versão 1709 ou posterior e Windows 10 versão 1703 ou posterior


Regulamentos em muitos setores exigem sistemas para ser rastreadas para UTC.  Isso significa que o deslocamento do sistema pode ser Atestado em relação ao UTC.  Para habilitar cenários de conformidade a normas, Windows 10 (versão 1703 ou posterior) e o Windows Server 2016 (versão 1709 ou superior) fornece novos logs de eventos para fornecer uma imagem da perspectiva do sistema operacional para formar uma compreensão das ações tomadas o relógio do sistema.  Esses logs de eventos são gerados continuamente para o serviço de tempo do Windows e pode ser examinados ou arquivados para análise posterior.

Esses novos eventos permitem que as seguintes perguntas a serem respondidas:

* O relógio do sistema foi alterado
* A frequência do relógio foi modificada
* A configuração do serviço de tempo do Windows foi modificada

## <a name="availability"></a>Disponibilidade

Esses aprimoramentos estão incluídos no Windows 10 versão 1703 ou posterior e Windows Server 2016 versão 1709 ou superior.

## <a name="configuration"></a>Configuração

Nenhuma configuração é necessária para realizar esse recurso.  Esses logs de eventos são habilitados por padrão e pode ser encontrados no Visualizador de eventos sob o **aplicativos e serviços Log\Microsoft\Windows\Time-Service\Operational** channel.


## <a name="list-of-event-logs"></a>Lista de Logs de eventos

A seção a seguir descreve os eventos registrados para uso em cenários de rastreabilidade.

<!-- use tabs like the group policies -->
# <a name="257tab257"></a>[257](#tab/257)
Esse evento é registrado quando o serviço de tempo do Windows (W32Time) é iniciado e registra informações sobre a hora atual, a contagem de tiques atual, configuração de tempo de execução, provedores de tempo e taxa atual de relógio.

|||
|---|---|
|Descrição do evento |Início do serviço |
|Detalhes |Ocorre durante a inicialização de W32time |
|Dados registrados |<ul><li>Hora atual em UTC</li><li>Contagem de tiques atual</li><li>Configuração de W32Time</li><li>Configuração do provedor de tempo</li><li>Velocidade do relógio</li></ul> |
|Mecanismo de limitação  |Nenhum. Esse evento é acionado sempre que o serviço é iniciado. |

**Exemplo:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

Essas informações também podem ser consultadas usando os seguintes comandos

*Configuração de provedor de tempo W32Time*
```
w32tm.exe /query /configuration
```

*Velocidade do relógio*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Esse evento é registrado quando o serviço de tempo do Windows (W32Time) está sendo interrompido e registra informações sobre a contagem atual de tempo e escala.

|||
|---|---|
|Descrição do evento |Parar serviço |
|Detalhes |Ocorre durante o desligamento W32time |
|Dados registrados |<ul><li>Hora atual em UTC</li><li>Contagem de tiques atual</li></ul> |
|Mecanismo de limitação  |Nenhum. Esse evento é acionado sempre que o serviço é interrompido. |

**Texto de exemplo:**
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Esse evento registra periodicamente sua lista atual de fontes de tempo e sua fonte de tempo escolhido.  Além disso, ele registrará a contagem de tiques atual.  Esse evento não é disparado sempre que uma fonte de horário é alterado.  Outros eventos listados neste documento fornecem essa funcionalidade.

|||
|---|---|
|Descrição do evento |Status periódicas do provedor de cliente NTP |
|Detalhes |Lista de fontes de tempo (s) usado pelo cliente NTP |
|Dados registrados |<ul><li>Fontes de tempo disponíveis</li><li>O servidor de horário de referência escolhido no momento do registro em log</li><li>Contagem de tiques atual</li></ul>  |
|Mecanismo de limitação  |Registrado uma vez a cada 8 horas. |

**Texto de exemplo:** Status do provedor de cliente NTP periódicos:

Cliente NTP está recebendo dados de tempo dos servidores NTP seguintes:

Server1.Fabrikam.com, 0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]: 123);  e o servidor de horário de referência escolhido é Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]: 123) (RefID:0x08d6648e63). Contagem de tiques do sistema 13187937

**Comando** essas informações também podem ser consultadas usando os seguintes comandos

*Identificar pares*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Descrição do evento |Status e a configuração do serviço de tempo |
|Detalhes |W32Time registra periodicamente sua configuração e status. Isso é o equivalente a chamar:<br><br>`w32tm /query /configuration /verbose`<br>OU<br>`w32tm /query /status /verbose` |
|Mecanismo de limitação  |Registrado uma vez a cada 8 horas. |

# <a name="261tab261"></a>[261](#tab/261)
Isso registra cada instância quando a hora do sistema é modificada usando a API de SetSystemTime.

|||
|---|---|
|Descrição do evento |Hora do sistema está definida |
|Mecanismo de limitação  |Nenhum.<br><br>Isso deve ocorrer raramente em sistemas com a sincronização de tempo razoável, e queremos fazer logon toda vez que ocorrer. Podemos ignorar configuração TimeJumpAuditOffset ao registrar esse evento, pois essa configuração destina-se a limitação de eventos no log de eventos do sistema do Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Descrição do evento |Frequência do relógio do sistema ajustada |
|Detalhes |Frequência de relógio do sistema é modificada constantemente por W32time quando o relógio está em sincronização próxima. Queremos capturar "razoavelmente significativos" ajustes feitos a frequência do relógio sem saturar o log de eventos. |
|Mecanismo de limitação  |Todos os ajustes abaixo TimeAdjustmentAuditThreshold do relógio (min = 128 parte por milhão, padrão = 800 parte por milhão) não são registradas.<br><br>Alteração do PPM 2 na frequência do relógio com granularidade atual resulta em alteração de 120 µsec/s em precisão do relógio.<br><br>Em um sistema sincronizado, a maioria dos ajustes estão abaixo desse nível. Se você quiser mais refinado de controle, essa configuração pode ser ajustada para baixo ou você pode usar PerfCounters, ou você pode usar ambos. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Descrição do evento |Alterar na lista de provedores de tempo carregado ou configurações de serviço de tempo. |
|Detalhes |A nova leitura W32time configurações pode causar determinadas configurações críticas a ser modificado na memória, que pode afetar a precisão geral da sincronização de tempo.<br><br>W32Time registra cada ocorrência quando reler suas configurações, que fornece o possível impacto no tempo de sincronização. |
|Mecanismo de limitação  |Nenhum.<br><br>Esse evento ocorre somente quando um administrador ou a atualização da GP altera os provedores de tempo e, em seguida, dispara W32time. Queremos registrar cada instância de alteração de configurações. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Descrição do evento |Alterar fonte (s) de tempo usado pelo cliente NTP |
|Detalhes |Cliente NTP registra um evento com o estado atual dos servidores/computadores tempo quando um servidor de tempo/par muda de estado (**pendentes-> Sync**, **-> sincronização inacessível**, ou outras transições) |
|Mecanismo de limitação  |Frequência máxima – apenas uma vez a cada 5 minutos para proteger o log de problemas transitórios e implementação de provedor incorreta. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Descrição do evento |Tempo fonte ou stratum número alterações de serviço |
|Detalhes |Fonte de tempo W32Time e o número de Stratum são fatores importantes na capacidade de rastreamento de tempo e todas as alterações a essas devem estar conectadas. Se W32time não tem uma fonte de tempo e você não tiver configurado como uma fonte de horário confiável, em seguida, ele deixará de publicidade como um servidor de tempo e, por design responder às solicitações com alguns parâmetros inválidos. Esse evento é essencial para acompanhar as alterações de estado em uma topologia de NTP. |
|Mecanismo de limitação  |Nenhum. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Descrição do evento |Ressincronização de tempo é solicitada |
|Detalhes |Esta operação é disparada:<ul><li>Quando ocorrem alterações de rede</li><li>Sistema retorna do modo de espera conectada/hibernação</li><li>Quando nós não foram sincronizados por um longo tempo</li><li>Admin emite o comando de ressincronização</li></ul>Essa operação resulta em perda de imediata de precisão de sincronização de hora refinado porque ele faz com que o cliente NTP limpar seus filtros. |
|Mecanismo de limitação  |Frequência máxima - uma vez a cada 5 minutos.<br><br>É possível que uma placa de rede incorreta (ou um script ruim) pode disparar essa operação repetidamente e resultar em logs obtendo sobrecarregados. Portanto, a necessidade de restringir esse evento.<br><br>Observe que a sincronização de horário com precisão leva muito mais do que 5 minutos para atingir e a limitação não perde informações sobre o evento original que resultaram em perda de precisão de tempo.  |

---
