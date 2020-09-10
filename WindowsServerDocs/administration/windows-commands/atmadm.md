---
title: atmadm
description: Artigo de referência para o comando atmadm, que monitora conexões e endereços que são registrados pelo Gerenciador de chamadas atM em uma rede atM (modo de transferência assíncrona).
ms.topic: reference
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c77ab10ce7ec628d3a1c820bc644f4b117d82953
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633406"
---
# <a name="atmadm"></a>atmadm

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Monitora conexões e endereços registrados pelo Gerenciador de chamadas atM em uma rede atM (modo de transferência assíncrona). Você pode usar **atmadm** para exibir estatísticas de chamadas de entrada e saída em adaptadores atM. Usado sem parâmetros, **atmadm** exibe estatísticas para monitorar o status de conexões atM ativas.

## <a name="syntax"></a>Sintaxe

```
atmadm [/c][/a][/s]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| /c | Exibe informações de chamada para todas as conexões atuais com o adaptador de rede atM instalado neste computador. |
| /a | Exibe o endereço do ponto de acesso do serviço de rede atM (NSAP) registrado para cada adaptador instalado neste computador. |
| /s | Exibe estatísticas para monitorar o status de conexões atM ativas. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="remarks"></a>Comentários

- O comando **atmadm/c** produz uma saída semelhante à seguinte:

    ```
    Windows atM call Manager Statistics
    atM Connections on Interface : [009] Olicom atM PCI 155 Adapter
       Connection   VPI/VCI   remote address/
                              Media Parameters (rates in bytes/sec)
       In  PMP SVC    0/193   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/192   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  PMP SVC    0/191   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/190   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  P-P SVC    0/475   47000580FFE1000000F21A2E180000C110081501
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9188
       Out PMP SVC    0/194   47000580FFE1000000F21A2E180000C110081501 (0)
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9180
                              Rx:UBR,Peak 0,Avg 0,MaxSdu 0
       Out P-P SVC    0/474   4700918100000000613E5BFE010000C110081500
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
       In  PMP SVC    0/195   47000580FFE1000000F21A2E180000C110081500
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 0
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9180
    ```

    A tabela a seguir contém descrições de cada elemento na saída de exemplo de **atmadm/c** .

    | Tipo de dados | Exibição de tela | Descrição |
    | -------- | --------- | -------- |
    | Informações de Conexão | Entrada/saída | Direção da chamada. O **no** é o adaptador de rede atM de outro dispositivo.  **Out** é do adaptador de rede atM para outro dispositivo. |
    | PMP | Chamada ponto a multiponto. |
    | P-P | Chamada ponto a ponto. |
    | SVC | A conexão está em um circuito virtual comutado. |
    | PVC | A conexão está em um circuito virtual permanente. |
    | Informações de VPI/VCI | VPI/VCI | O caminho virtual e o canal virtual da chamada de entrada ou saída. |
    | Parâmetros de mídia/endereço remoto | 47000580FFE1000000F21A2E180000C110081500 | Endereço NSAP do dispositivo atM de chamada **(in)** ou chamado **(out)** . |
    | TX | O parâmetro **TX** inclui os três seguintes elementos:<ul><li>Tipo de taxa de bits padrão ou especificada (UBR, CBR, VBR ou ABR)</li><li>Velocidade de linha padrão ou especificada</li><li>Tamanho de SDU (unidade de dados de serviço) especificado.</li></ul> |
    | Rx | O parâmetro **RX** inclui os três seguintes elementos:<ul><li>Tipo de taxa de bits padrão ou especificada (UBR, CBR, VBR ou ABR)</li><li>Velocidade de linha padrão ou especificada</li><li>Tamanho de SDU especificado.</li></ul> |

- O comando **atmadm/a** produz uma saída semelhante à seguinte:

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```

- O comando **atmadm/s** produz uma saída semelhante à seguinte:

    ```
    Windows atM call Manager Statistics
    atM call Manager statistics for Interface : [009] Olicom atM PCI 155 Adapter
    Current active calls                        = 4
    Total successful Incoming calls             = 1332
    Total successful Outgoing calls             = 1297
    Unsuccessful Incoming calls                 = 1
    Unsuccessful Outgoing calls                 = 1
    calls Closed by remote                      = 1302
    calls Closed Locally                        = 1323
    Signaling and ILMI Packets Sent            = 33655
    Signaling and ILMI Packets Received        = 34989
    ```

    A tabela a seguir contém descrições de cada elemento na saída de exemplo de **atmadm/s** .

    | Estatística do Gerenciador de chamadas | Descrição |
    | ------------- | -------- |
    | Chamadas ativas atuais | Chamadas atualmente ativas no adaptador atM instalado neste computador. |
    | Total de chamadas de entrada com êxito | Chamadas recebidas com êxito de outros dispositivos nesta rede atM. |
    | Total de chamadas de saída com êxito | Chamadas concluídas com êxito para outros dispositivos atM nesta rede deste computador. |
    | Chamadas de entrada malsucedidas | Chamadas de entrada que falharam ao se conectar a este computador. |
    | Chamadas de saída malsucedidas | Chamadas de saída que falharam ao se conectar a outro dispositivo na rede. |
    | Chamadas encerradas por remoto | Chamadas fechadas por um dispositivo remoto na rede. |
    | Chamadas fechadas localmente | Chamadas fechadas por este computador. |
    | Pacotes de sinalização e ILMI enviados | Número de pacotes de ILMI (interface de gerenciamento local) integrados enviados ao comutador ao qual este computador está tentando se conectar. |
    | Pacotes de sinalização e ILMI recebidos | Número de pacotes ILMI recebidos do comutador atM. |

## <a name="examples"></a>Exemplos

Para exibir informações de chamada para todas as conexões atuais com o adaptador de rede atM instalado neste computador, digite:

```
atmadm /c
```

Para exibir o endereço do ponto de acesso do serviço de rede atM (NSAP) registrado para cada adaptador instalado neste computador, digite:

```
atmadm /a
```

Para exibir estatísticas para monitorar o status de conexões atM ativas, digite:

```
atmadm /s
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
