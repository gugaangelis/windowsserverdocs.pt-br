---
ms.assetid: ''
title: Tempo do Windows para rastreabilidade
description: As regulamentações em muitos setores exigem que os sistemas sejam rastreáveis para o UTC.  Isso significa que o deslocamento de um sistema pode ser atestado em relação ao UTC.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 307739042426088fa92c50e6ea4dc5d2a744f15a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405209"
---
# <a name="windows-time-for-traceability"></a>Tempo do Windows para rastreabilidade
>Aplica-se a: Windows Server 2016 versão 1709 ou posterior e Windows 10 versão 1703 ou posterior


As regulamentações em muitos setores exigem que os sistemas sejam rastreáveis para o UTC.  Isso significa que o deslocamento de um sistema pode ser atestado em relação ao UTC.  Para habilitar cenários de conformidade regulatória, o Windows 10 (versão 1703 ou superior) e o Windows Server 2016 (versão 1709 ou superior) fornecem novos logs de eventos para fornecer uma imagem da perspectiva do sistema operacional para formar uma compreensão das ações executadas em o relógio do sistema.  Esses logs de eventos são gerados continuamente para o serviço de tempo do Windows e podem ser examinados ou arquivados para análise posterior.

Esses novos eventos permitem que as seguintes perguntas sejam respondidas:

* O relógio do sistema foi alterado
* A frequência do relógio foi modificada
* A configuração do serviço de tempo do Windows foi modificada

## <a name="availability"></a>Disponibilidade

Esses aprimoramentos estão incluídos no Windows 10 versão 1703 ou superior e no Windows Server 2016 versão 1709 ou superior.

## <a name="configuration"></a>Configuração

Nenhuma configuração é necessária para realizar esse recurso.  Esses logs de eventos são habilitados por padrão e podem ser encontrados no Visualizador de eventos no canal **Log\Microsoft\Windows\Time-Service\Operational de aplicativos e serviços** .


## <a name="list-of-event-logs"></a>Lista de logs de eventos

A seção a seguir descreve os eventos registrados para uso em cenários de rastreabilidade.

# <a name="257tab257"></a>[257](#tab/257)
Esse evento é registrado quando o serviço de tempo do Windows (W32Time) é iniciado e registra as informações sobre a hora atual, a contagem de tiques atual, a configuração de tempo de execução, os provedores de tempo e a taxa de relógio atual.

|||
|---|---|
|Descrição do evento |Início do serviço |
|Detalhes |Ocorre na inicialização do W32time |
|Dados registrados |<ul><li>Hora atual em UTC</li><li>Contagem de tiques atual</li><li>Configuração do W32Time</li><li>Configuração do provedor de tempo</li><li>Taxa de relógio</li></ul> |
|Mecanismo de limitação  |nenhuma. Esse evento é acionado toda vez que o serviço é iniciado. |

**Exemplo:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

Essas informações também podem ser consultadas usando os seguintes comandos

*Configuração do provedor de tempo e do W32Time*
```
w32tm.exe /query /configuration
```

*Taxa de relógio*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Esse evento é registrado quando o serviço de tempo do Windows (W32Time) está parando e registra as informações sobre a hora atual e a contagem de tiques.

|||
|---|---|
|Descrição do evento |Interrupção de serviço |
|Detalhes |Ocorre no desligamento W32time |
|Dados registrados |<ul><li>Hora atual em UTC</li><li>Contagem de tiques atual</li></ul> |
|Mecanismo de limitação  |nenhuma. Esse evento é acionado toda vez que o serviço é interrompido. |

**Texto de exemplo:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Esse evento registra periodicamente sua lista atual de fontes de tempo e sua fonte de tempo escolhida.  Além disso, ele registra a contagem de tiques atual.  Esse evento não é disparado sempre que uma fonte de tempo é alterada.  Outros eventos listados posteriormente neste documento fornecem essa funcionalidade.

|||
|---|---|
|Descrição do evento |Status periódico do provedor de cliente NTP |
|Detalhes |Lista de fontes de tempo usadas pelo cliente NTP |
|Dados registrados |<ul><li>Fontes de tempo disponíveis</li><li>O servidor de tempo de referência escolhido no momento do registro em log</li><li>Contagem de tiques atual</li></ul>  |
|Mecanismo de limitação  |Registrado uma vez a cada 8 horas. |

**Texto de exemplo:** Status periódico do provedor de cliente NTP:

O cliente NTP está recebendo dados de tempo dos seguintes servidores NTP:

Server1. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123) Server2. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123);  e o servidor de tempo de referência escolhido é Server1. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123) (RefID: 0x08d6648e63). Contagem de tiques do sistema 13187937

**Comando** Essas informações também podem ser consultadas usando os seguintes comandos

*Identificar pares*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Descrição do evento |Status e configuração de serviço de tempo |
|Detalhes |O W32time registra periodicamente sua configuração e seu status. Esse é o equivalente de chamar:<br><br>`w32tm /query /configuration /verbose`<br>OU<br>`w32tm /query /status /verbose` |
|Mecanismo de limitação  |Registrado uma vez a cada 8 horas. |

# <a name="261tab261"></a>[261](#tab/261)
Isso registra cada instância quando a hora do sistema é modificada usando a API SetSystemTime.

|||
|---|---|
|Descrição do evento |A hora do sistema está definida |
|Mecanismo de limitação  |nenhuma.<br><br>Isso deve acontecer raramente em sistemas com sincronização de tempo razoável e queremos registrar em log a cada vez que ocorrer. Ignoramos a configuração de TimeJumpAuditOffset ao registrar esse evento, pois essa configuração foi destinada a restringir eventos no log de eventos do sistema Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Descrição do evento |Frequência de relógio do sistema ajustada |
|Detalhes |A frequência do relógio do sistema é modificada constantemente pelo W32time quando o relógio está em uma sincronização próxima. Queremos capturar ajustes "razoavelmente significativos" feitos na frequência do relógio sem sobreexecutar o log de eventos. |
|Mecanismo de limitação  |Todos os ajustes de relógio abaixo de TimeAdjustmentAuditThreshold (min = 128 parte por milhão, padrão = 800 parte por milhão) não são registrados.<br><br>2 PPM alteradas na frequência de relógio com a granularidade atual gera 120 μsec/s de alteração na precisão do relógio.<br><br>Em um sistema sincronizado, a maioria dos ajustes está abaixo desse nível. Se você quiser um controle mais preciso, essa configuração pode ser ajustada ou você pode usar PerfCounters, ou você pode fazer ambos. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Descrição do evento |Alteração nas configurações de serviço de tempo ou na lista de provedores de tempo carregados. |
|Detalhes |A releitura das configurações do W32time pode fazer com que determinadas configurações críticas sejam modificadas na memória, o que pode afetar a precisão geral da sincronização de tempo.<br><br>O W32time registra cada ocorrência ao reler suas configurações, o que proporciona o possível impacto na sincronização de tempo. |
|Mecanismo de limitação  |nenhuma.<br><br>Esse evento ocorre somente quando uma atualização de administrador ou GP altera os provedores de tempo e, em seguida, dispara o W32time. Queremos registrar cada instância de alteração das configurações. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Descrição do evento |Alteração nas fontes de tempo usadas pelo cliente NTP |
|Detalhes |O cliente NTP registra um evento com o estado atual dos servidores/pares de tempo quando um servidor/ponto de um tempo altera o estado (**sincronização de > pendente**, **sincronização > inacessível**ou outras transições) |
|Mecanismo de limitação  |Frequência máxima – apenas uma vez a cada 5 minutos para proteger o log de problemas transitórios e implementação de provedor inadequada. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Descrição do evento |Alterações de origem de serviço de tempo ou de número de estrato |
|Detalhes |A fonte de tempo W32time e o número de estrato são fatores importantes na rastreabilidade de tempo e as alterações a elas devem ser registradas em log. Se W32time não tiver nenhuma fonte de tempo e você não tiver configurado como uma fonte de tempo confiável, ele interromperá o anúncio como um servidor de horário e responderá a solicitações com alguns parâmetros inválidos. Esse evento é essencial para acompanhar as alterações de estado em uma topologia de NTP. |
|Mecanismo de limitação  |nenhuma. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Descrição do evento |A nova sincronização de tempo é solicitada |
|Detalhes |Esta operação foi disparada:<ul><li>Quando ocorrem alterações na rede</li><li>O sistema retorna do modo de espera/hibernação conectado</li><li>Quando não sincronizamos há muito tempo</li><li>O administrador emite o comando Ressync</li></ul>Essa operação resulta em perda imediata de precisão de sincronização de tempo refinado porque faz com que o cliente NTP Limpe seus filtros. |
|Mecanismo de limitação  |Frequência máxima-uma vez a cada 5 minutos.<br><br>É possível que uma placa de rede incorreta (ou um script ruim) possa disparar essa operação repetidamente e fazer com que os logs fiquem sobrecarregados. Portanto, a necessidade de restringir esse evento.<br><br>Observe que a sincronização de tempo precisa leva muito mais do que 5 minutos para ser alcançada e a limitação não perde informações sobre o evento original que resultou em perda de precisão de tempo.  |

---
