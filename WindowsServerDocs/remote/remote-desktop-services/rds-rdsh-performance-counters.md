---
title: Use contadores de desempenho para diagnosticar problemas de capacidade de resposta do aplicativo em Hosts de sessão de área de trabalho remota
description: Seu aplicativo é executado lento no RDS? Saiba mais sobre contadores de desempenho, que você pode usar para diagnosticar problemas de desempenho do aplicativo em RDSH
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 241b2b776a68cf5aec68a4d331201a07f0e5ea53
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133712"
---
# Use contadores de desempenho para diagnosticar problemas de desempenho do aplicativo em Hosts de sessão de área de trabalho remota

Um dos problemas mais difíceis de diagnosticar é desempenho ruim do aplicativo — os aplicativos executam lenta ou não responder. Tradicionalmente, inicie o diagnóstico pela coleta de CPU, memória, disco de entrada/saída e outras métricas e, em seguida, usar ferramentas como o Windows Performance Analyzer para tentar descobrir o que está causando o problema. Infelizmente na maioria das situações esses dados não ajudam a identificar a causa raiz porque contadores de consumo de recursos têm grandes e frequentes variações. Isso torna difíceis de ler os dados e correlacioná-lo com o problema reportado. Para ajudar a você mais rapidamente resolver os problemas de desempenho do aplicativo, adicionamos alguns novos contadores de desempenho (disponível [para download](#download-windows-server-insider-software) por meio do [Programa Windows Insider](https://insider.windows.com)) que medem fluxos de entrada do usuário.

O contador de atraso de entrada do usuário pode ajudá-lo a identificar rapidamente a causa raiz para ruins usuário final que experiências de RDP. Esse contador mede quanto qualquer entrada (por exemplo, o uso do mouse ou teclado) do usuário permanece na fila antes que ele é identificado por um processo, e o contador funciona em sessões locais e remotas.

A imagem a seguir mostra uma representação aproximada do fluxo de entrada do usuário do cliente ao aplicativo.

![Área de trabalho remota - fluxos entradas de usuário do cliente de área de trabalho remota do usuários para o aplicativo](.\media\rds-user-input.png)

O contador de atraso de entrada do usuário mede o delta máximo (dentro de um intervalo de tempo) entre a entrada na fila e quando ele é identificado pelo aplicativo em um [loop de mensagem tradicional](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), conforme mostrado no gráfico de fluxo a seguir:

![Área de trabalho remota - fluxo de contador de desempenho de atraso de entrada do usuário](.\media\rds-user-input-delay.png)

Um detalhe importante desse contador é que ele relata o atraso de entrada do usuário máximo em um intervalo configurável. Isso é o mais longo tempo que leva para uma entrada para acessar o aplicativo, que pode afetar a velocidade de ações importantes e visíveis como digitar.

Por exemplo, na tabela a seguir, o atraso de entrada do usuário seria relatado como 1.000 ms dentro desse intervalo. O contador informa o usuário mais lento entrada atraso no intervalo porque a percepção do usuário de "lento" é determinada pelo tempo slowest entrado (o máximo) eles experiência, não a velocidade média de todas as entradas total.

|Número| 0 | 1 | 2 |
|------|---|---|---|
|Atraso |16 ms| 20 ms| 1.000 ms|

## Ativar e usar os novos contadores de desempenho

Para usar esses novos contadores de desempenho, primeiro você deve ativar uma chave do Registro executando este comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Se você estiver usando o Windows 10, versão 1809 ou posterior ou Windows Server 2019 ou posterior, você não precisa habilitar a chave do registro.

Em seguida, reinicie o servidor. Em seguida, abra o Monitor de desempenho e selecione o sinal de mais (+), conforme mostrado na captura de tela a seguir.

![Contador de desempenho de atraso de entrada de área de trabalho remota - uma captura de tela mostrando como adicionar o usuário](.\media\rds-add-user-input-counter-screen.png)

Depois de fazer isso, você deve ver a caixa de diálogo Adicionar contadores, onde você pode selecionar o **Atraso de entrada do usuário por processo** ou **Atraso de entrada do usuário por sessão**.

![Área de trabalho remota - uma captura de tela mostrando como adicionar o atraso de entrada do usuário por sessão](.\media\rds-user-delay-per-session.png)

![Área de trabalho remota - uma captura de tela mostrando como adicionar o atraso de entrada do usuário por processo](.\media\rds-user-delay-per-process.png)

Se você selecionar o **Atraso de entrada do usuário por processo**, você verá as **instâncias do objeto selecionado** (em outras palavras, os processos) em ```SessionID:ProcessID <Process Image>``` formato.

Por exemplo, se o aplicativo Calculadora é executado em uma [sessão ID 1](https://msdn.microsoft.com/library/ms524326.aspx), você verá ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Nem todos os processos são incluídos. Você não verá quaisquer processos que estão em execução como sistema.

O contador inicia relatório atraso de entrada do usuário assim que você adicioná-lo. Observe que a escala máxima é definida como 100 (ms) por padrão. 

![Área de trabalho remota - um exemplo de atividade para o atraso de entrada do usuário por processo no Monitor de desempenho](.\media\rds-sample-user-input-delay-perfmon.png)

Em seguida, vamos examinar o **Atraso de entrada do usuário por sessão**. Há instâncias para cada ID de sessão e seus contadores de mostram o atraso de entrada do usuário de qualquer processo dentro da sessão especificada. Além disso, há duas instâncias chamadas "Max" (o atraso entrada de usuário máximo entre todas as sessões) e "Médio" (o acorss de médio todas as sessões).

Esta tabela mostra um exemplo visual de nesses casos. (Você pode obter as mesmas informações em Perfmon alternando para o tipo de gráfico de relatório.)

|Tipo de contador|Nome da instância|Atraso relatado (ms)|
|---------------|-------------|-------------------|
|Atraso de entrada do usuário por processo|1:4232 < Calculator.exe >|  200|
|Atraso de entrada do usuário por processo|2:1000 < Calculator.exe >|  16|
|Atraso de entrada do usuário por processo|1:2000 < Calculator.exe >|  32|
|Atraso de entrada do usuário por sessão|1|    200|
|Atraso de entrada do usuário por sessão|2|    16|
|Atraso de entrada do usuário por sessão|Médio|  108|
|Atraso de entrada do usuário por sessão|Máx.|  200|

## Contadores usados em um sistema sobrecarregado

Agora vamos examinar o que você verá no relatório se o desempenho de um aplicativo é reduzido. O gráfico a seguir mostra as leituras para usuários trabalhando remotamente no Microsoft Word. Nesse caso, o desempenho do servidor RDSH degrada ao longo do tempo conforme mais usuários fazer logon.

![Área de trabalho remota - um gráfico de desempenho de exemplo para o servidor RDSH executando o Microsoft Word](.\media\rds-user-input-perf-graph.png)

Aqui está como ler linhas do gráfico:

- A linha rosa mostra o número de sessões de entrar no servidor.
- A linha vermelha é o uso da CPU.
- A linha verde é o atraso de entrada do usuário máximo entre todas as sessões.
- A linha azul (exibida como preto neste gráfico) representa o atraso de entrada de usuário médio entre todas as sessões.

Você notará que há uma correlação entre picos de CPU e atraso de entrada do usuário — conforme a CPU fica mais de uso, o usuário digitar aumenta de atraso. Além disso, mais usuários são adicionados ao sistema, o uso de CPU se aproximar 100%, levando a mais frequentes picos de atraso de entrada do usuário. Embora esse contador é muito útil em casos onde o servidor é executado fora de recursos, você também pode usá-lo para controlar o atraso de entrada de usuário relacionado a um aplicativo específico.

## Opções de configuração

Um aspecto importante lembrar ao usar esse contador de desempenho é que ele relata o atraso de entrada do usuário em um intervalo de 1.000 ms por padrão. Se você definir a propriedade de intervalo de exemplo de contador de desempenho (como mostrado na seguinte captura de tela) como algo diferente, o valor relatado será incorreto.

![Área de trabalho remota - as propriedades para o monitor de desempenho](.\media\rds-user-input-perfmon-properties.png)

Para corrigir isso, você pode definir a seguinte chave do registro para corresponder o intervalo (em milissegundos) que você deseja usar. Por exemplo, se podemos alterar cada x segundos de amostra para 5 segundos, precisamos definir essa chave 5000 MS.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Se você estiver usando o Windows 10, versão 1809 ou posterior ou Windows Server 2019 ou posterior, você não precisa definir LagCounterInterval para corrigir o contador de desempenho.

Também adicionamos algumas chaves que podem ser úteis sob a mesma chave do registro:

**LagCounterImageNameFirst** — definir essa chave `DWORD 1` (valor padrão 0 ou a chave não existir). Isso altera os nomes de contador "Imagem Name < SessionID:ProcessId >". Por exemplo, "explorer < 1:7964 >". Isso é útil se você quiser classificar por nome da imagem.

**LagCounterShowUnknown** — definir essa chave `DWORD 1` (valor padrão 0 ou a chave não existir). Isso mostra todos os processos que estão em execução como sistema ou serviços. Alguns processos aparecerão com sua sessão definida como "?."

Isso é sua aparência se você ativar as duas chaves:

![Área de trabalho remota - o monitor de desempenho com ambas as chaves em](.\media\rds-user-input-delay-with-two-counters.png)

## Usando os contadores de novos com ferramentas de terceiros

Ferramentas de monitoramento podem consumir esse contador usando a [API de Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## Baixar o software do Windows Server Insider

Os participantes registrados podem navegar diretamente para a [página de download do Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) para obter os downloads de software mais recentes do Insider.  Para saber como registrar-se como um Insider, consulte a [Introdução ao servidor](https://insider.windows.com/en-us/for-business-getting-started-server/).

## Compartilhe seu feedback

Você pode enviar comentários para esse recurso por meio do Hub de Feedback. Selecione **aplicativos > todos os outros aplicativos** e inclua "contadores de desempenho de RDS — monitor de desempenho" no título do seu post.

Para ter ideias gerais de recurso, visite a [página de RDS UserVoice](https://aka.ms/uservoice-rds).
