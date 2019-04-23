---
title: atmadm
description: Tópico de comandos do Windows para **atmadm** -monitora conexões e os endereços que são registrados pelo atM chamam Manager em uma rede de atM (modo) de transferência assíncrona.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a8a8883bad9aa2abdc90d5db5702ef42f46c8a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831767"
---
# <a name="atmadm"></a>atmadm

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Monitora conexões e os endereços que são registrados pelo atM chamam Manager em uma rede de atM (modo) de transferência assíncrona. Você pode usar **atmadm** para exibir estatísticas para chamadas de entrada e saídas em adaptadores atM. Usado sem parâmetros, **atmadm** exibe as estatísticas para monitorar o status das conexões atM Active Directory. 

## <a name="syntax"></a>Sintaxe

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|/c|Exibe informações de chamadas para todas as conexões atuais para o adaptador de rede atM instalado neste computador.|
|/a|Exibe o acesso ao serviço de rede atM registrado ponto endereço (NSAP) para cada adaptador instalado neste computador.|
|/s|Exibe as estatísticas para monitorar o status das conexões atM Active Directory.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- O **atmadm /c** comando produz uma saída semelhante à seguinte:

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
    
    A tabela a seguir contém descrições de cada elemento de **atmadm /c** uma saída de exemplo.
    
    |tipo de dados|Exibição de tela|Descrição|
    |--------|---------|--------|
    |Informações de Conexão|Entrada/saída|direção da chamada.  **No** é para o adaptador de rede atM de outro dispositivo.  **Out** é do adaptador de rede atM para outro dispositivo.|
    ||PMP|Chamada de ponto para vários pontos.|
    ||P-P|Chamada de ponto a ponto.|
    ||SVC|Conexão está em um circuito virtual comutado.|
    ||PVC|Conexão está em um circuito virtual permanente.|
    |Informações de VPI/VCI|VPI/VCI|Caminho virtual e canal virtual da chamada de entrada ou saída.|
    |endereço remota/mídia parâmetros|47000580FFE1000000F21A2E180000C110081500|Endereço NSAP do chama **(em)** ou chamado **(saída)** dispositivo atM.|
    ||**Tx**|O **Tx** parâmetro inclui três elementos:<br /><br />-Padrão ou especificado o tipo de taxa de bits (UBR, CBR, VBR ou ABR)<br />-Padrão ou especificado a velocidade da linha<br />-Tamanho de unidade (SDU) de dados de serviço especificado|
    ||**Rx**|O **Rx** parâmetro inclui três elementos:<br /><br />-Padrão ou especificado o tipo de taxa de bits (UBR, CBR, VBR ou ABR)<br />-Padrão ou especificado a velocidade da linha<br />-Tamanho SDU especificado|
    
- O **atmadm /c** comando produz uma saída semelhante à seguinte:

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- O **atmadm /s** comando produz uma saída semelhante à seguinte:

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
    
    A tabela a seguir contém descrições de cada elemento de **atmadm /s** uma saída de exemplo.
    
    |Estatística do Gerenciador de chamadas|Descrição|
    |-------------|--------|
    |Chamadas ativas atuais|chamadas ativas no momento o adaptador de atM instalado neste computador.|
    |Total de chamadas recebidas com êxito|chamadas recebidas com êxito de outros dispositivos na rede atM.|
    |Total de chamadas enviadas com êxito|chamadas concluídas com êxito para outros dispositivos atM nesta rede deste computador.|
    |Chamadas recebidas sem êxito|Chamadas de entrada que não conseguiu se conectar a este computador.|
    |Chamadas enviadas sem êxito|Chamadas de saída que não conseguiu se conectar a outro dispositivo na rede.|
    |chamadas fechado remotamente|chamadas encerradas por um dispositivo remoto na rede.|
    |chama fechado localmente|chamadas encerradas por este computador.|
    |Pacotes de sinal e ILMI enviados|Número de pacotes de interface (ILMI) de gerenciamento local integrado enviados ao comutador ao qual este computador está tentando se conectar.|
    |Pacotes de sinal e ILMI recebidos|Número de pacotes ILMI recebidos do comutador atM.|
    
## <a name="BKMK_Examples"></a>Exemplos

Para exibir informações de chamada para todas as conexões atuais para o adaptador de rede atM instalado neste computador, digite:

```
atmadm /c
```

Para exibir o endereço de ponto (NSAP) de acesso de serviço da rede do atM registrado para cada adaptador instalado neste computador, digite:

```
atmadm /a
```

Para exibir estatísticas para monitorar o status das conexões atM Active Directory, digite:

```
atmadm /s
```

## <a name="additional-references"></a>Referências adicionais

- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
