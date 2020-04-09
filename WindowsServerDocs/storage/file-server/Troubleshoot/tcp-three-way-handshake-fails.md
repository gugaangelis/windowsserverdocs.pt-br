---
title: Falha de handshake de três vias TCP durante conexão SMB
description: Apresenta a falha de handshake de três vias TCP durante a conexão SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: cb88fa89344172cfc1ed036865a4496ed73e9a22
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815319"
---
# <a name="tcp-three-way-handshake-failure-during-smb-connection"></a>Falha de handshake de três vias TCP durante conexão SMB

Ao analisar um rastreamento de rede, você observa que há uma falha de handshake de três vias do protocolo TCP (Transmission Control Protocol) que causa a ocorrência do problema SMB. Este artigo descreve como solucionar essa situação.

## <a name="troubleshooting"></a>Solução de problemas

Em geral, a causa é um firewall local ou de infraestrutura que bloqueia o tráfego. Esse problema pode ocorrer em qualquer um dos cenários a seguir.

## <a name="scenario-1"></a>Cenário 1

O pacote TCP SYN chega no servidor SMB, mas o servidor SMB não retorna um pacote TCP SYN-ACK.

Para solucionar esse problema, siga estas etapas.

### <a name="step-1"></a>Etapa 1

Execute **netstat** ou **Get-NetTcpConnection** para verificar se há um ouvinte na porta TCP 445 que deve pertencer ao processo do sistema.

```cmd
netstat -ano | findstr :445
```

```PowerShell
Get-NetTcpConnection -LocalPort 445
```

### <a name="step-2"></a>Etapa 2

Verifique se o serviço do servidor foi iniciado e está em execução.

### <a name="step-3"></a>Etapa 3

Faça uma captura da WFP (plataforma de filtragem do Windows) para determinar qual regra ou programa está descartando o tráfego. Para fazer isso, execute o seguinte comando em uma janela de prompt de comando:

```cmd
netsh wfp capture start
```

Reproduza o problema e, em seguida, execute o seguinte comando:

```cmd
netsh wfp capture stop
```

Execute um rastreamento de cenário e procure descartes de WFP no tráfego SMB (na porta TCP 445).

Opcionalmente, você pode remover os programas de antivírus porque eles nem sempre são baseados em WFP.

### <a name="step-4"></a>Etapa 4

Se o Firewall do Windows estiver habilitado, habilite o log de firewall para determinar se ele registra um queda no tráfego.

Verifique se as regras apropriadas "compartilhamento de arquivos e impressoras (SMB-in)" estão habilitadas no **Firewall do Windows com segurança avançada** \> **regras de entrada**.

> [!NOTE]
> Dependendo de como o computador está configurado, "Firewall do Windows" pode ser chamado de "Windows Defender firewall".

## <a name="scenario-2"></a>Cenário 2

O pacote TCP SYN nunca chega ao servidor SMB.

Nesse cenário, você precisa investigar os dispositivos ao longo do caminho de rede. Você pode analisar os rastreamentos de rede que são capturados em cada dispositivo para determinar qual dispositivo está bloqueando o tráfego.
