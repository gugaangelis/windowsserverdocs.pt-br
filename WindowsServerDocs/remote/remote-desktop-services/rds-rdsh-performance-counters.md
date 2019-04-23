---
title: Use contadores de desempenho para diagnosticar os problemas de capacidade de resposta do aplicativo nos Hosts de sessão de área de trabalho remota
description: Seu aplicativo está executando lentamente no RDS? Saiba mais sobre contadores de desempenho, que você pode usar para diagnosticar problemas de desempenho do aplicativo em RDSH
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844647"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Usar contadores de desempenho para diagnosticar problemas de desempenho do aplicativo nos Hosts de sessão de área de trabalho remota

Um dos problemas mais difíceis de diagnosticar é o desempenho insatisfatório do aplicativo — os aplicativos estão muito lentos ou não responder. Tradicionalmente, você pode iniciar o seu diagnóstico pela coleta de CPU, memória, entrada/saída de disco e outras métricas e, em seguida, usa ferramentas como o analisador de desempenho do Windows para tentar descobrir o que está causando o problema. Infelizmente na maioria das situações esses dados não ajudarão-lo a identificar a causa raiz, como contadores de consumo de recursos têm grandes e frequentes variações. Isso torna difícil de ler os dados e correlacioná-los com o problema relatado. Para ajudá-lo mais rapidamente resolver seus problemas de desempenho do aplicativo, adicionamos alguns novos contadores de desempenho (disponível [para baixar](#download-windows-server-insider-software) por meio de [Windows Insider Program](https://insider.windows.com)) fluxos de entrada que o usuário medida.

O contador de atraso de entrada do usuário pode ajudá-lo a identificar rapidamente a causa raiz de incorreto RDP experiências de usuário final. Esse contador mede o quanto qualquer usuário de entrada (por exemplo, o uso de mouse ou teclado) permanece na fila antes de que é captado por um processo e o contador funciona em sessões locais e remotas.

A imagem a seguir mostra uma representação aproximada do fluxo de entrada do usuário do cliente ao aplicativo.

![Área de trabalho remota - fluxos de entrada usuário do cliente de área de trabalho remota de usuários para o aplicativo](.\media\rds-user-input.png)

O contador de atraso de entrada do usuário mede o delta máximo (dentro de um intervalo de tempo) entre a entrada que está sendo enfileirado e quando é captado pelo aplicativo em um [loop de mensagem tradicional](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), conforme mostrado no gráfico de fluxo a seguir:

![Área de trabalho remota - fluxo de contador de desempenho de atraso de entrada do usuário](.\media\rds-user-input-delay.png)

Um detalhe importante desse contador é que ele relata o atraso de entrada de usuário máxima dentro de um intervalo configurável. Isso é o tempo mais longo necessário para alcançar o aplicativo, que pode afetar a velocidade das ações visíveis e importantes, como digitar uma entrada.

Por exemplo, na tabela a seguir, o atraso de entrada do usuário seria informado como 1.000 ms dentro deste intervalo. O contador informa o usuário mais lento de entrada atraso no intervalo porque a percepção do usuário de "lento" é determinada pela hora de entrada mais lenta (o máximo) eles experimentam, não a velocidade média de todas as entradas de total.

|Número| 0 | 1 | 2 |
|------|---|---|---|
|Atraso |16 ms| 20 ms| 1.000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Habilitar e usar contadores de desempenho

Para usar esses novos contadores de desempenho, você primeiro deve ativar uma chave do Registro executando este comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Se você estiver usando o Windows 10, versão 1809 ou posterior ou Windows Server 2019 ou posterior, você não precisará habilitar a chave do registro.

Em seguida, reinicie o servidor. Em seguida, abra o Monitor de desempenho e selecione o sinal de adição (+), conforme mostrado na captura de tela a seguir.

![Contador de desempenho de atraso de entrada de área de trabalho remota - uma captura de tela mostrando como adicionar o usuário](.\media\rds-add-user-input-counter-screen.png)

Depois de fazer isso, você deverá ver a caixa de diálogo Adicionar contadores, onde você pode selecionar **atraso de entrada do usuário por processo** ou **atraso de entrada do usuário por sessão**.

![Área de trabalho remota - uma captura de tela mostrando como adicionar o atraso de entrada do usuário por sessão](.\media\rds-user-delay-per-session.png)

![Área de trabalho remota - uma captura de tela mostrando como adicionar o atraso de entrada do usuário por processo](.\media\rds-user-delay-per-process.png)

Se você selecionar **atraso de entrada do usuário por processo**, você verá o **instâncias do objeto selecionado** (em outras palavras, os processos) no ```SessionID:ProcessID <Process Image>``` formato.

Por exemplo, se o aplicativo Calculadora está em execução um [ID de sessão 1](https://msdn.microsoft.com/library/ms524326.aspx), você verá ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Nem todos os processos são incluídos. Você não verá todos os processos que estão sendo executados como sistema.

O contador começará a reportar o atraso de entrada do usuário, assim você adicioná-lo. Observe que a escala máxima é definida como 100 (ms) por padrão. 

![Área de trabalho remota - um exemplo de atividade para o atraso de entrada do usuário por processo no Monitor de desempenho](.\media\rds-sample-user-input-delay-perfmon.png)

Em seguida, vamos examinar a **atraso de entrada do usuário por sessão**. Há instâncias para cada ID de sessão e seus contadores mostram o atraso de entrada do usuário de qualquer processo dentro da sessão especificada. Além disso, há duas instâncias, chamadas "Max" (o atraso entrada máxima de usuários em todas as sessões) e "Média" (o acorss de média todas as sessões).

Esta tabela mostra um exemplo visual dessas instâncias. (Você pode obter as mesmas informações no Perfmon, alternando para o tipo de gráfico do relatório.)

|Tipo de contador|Nome da instância|Atraso relatado (ms)|
|---------------|-------------|-------------------|
|Atraso de entrada do usuário por processo|1:4232 < Calculator.exe >|  200|
|Atraso de entrada do usuário por processo|2:1000 < Calculator.exe >|  16|
|Atraso de entrada do usuário por processo|1:2000 < Calculator.exe >|  32|
|Atraso de entrada do usuário por sessão|1|    200|
|Atraso de entrada do usuário por sessão|2|    16|
|Atraso de entrada do usuário por sessão|Média|  108|
|Atraso de entrada do usuário por sessão|Máx|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Contadores usados em um sistema sobrecarregado

Agora vamos examinar o que você verá no relatório se o desempenho de um aplicativo está degradado. O gráfico a seguir mostra as leituras para os usuários que trabalham remotamente no Microsoft Word. Nesse caso, o desempenho do servidor RDSH degrada ao longo do tempo conforme os usuários mais fazer logon.

![Área de trabalho remota - um exemplo de grafo de desempenho para o servidor RDSH executando o Microsoft Word](.\media\rds-user-input-perf-graph.png)

Aqui está como ler linhas do gráfico:

- A linha rosa mostra o número de sessões no servidor conectado.
- A linha vermelha é o uso da CPU.
- A linha verde é o atraso de entrada de usuário máxima em todas as sessões.
- A linha azul (exibida como preto nesse gráfico) representa o atraso de entrada do usuário médio em todas as sessões.

Você observará que há uma correlação entre picos de CPU e o atraso de entrada do usuário — à medida que a CPU obtiver mais uso, a entrada do usuário aumenta de atraso. Além disso, conforme mais usuários são adicionados ao sistema, o uso da CPU fique próximo a 100%, levando a picos de entrada de atraso de usuário mais frequentes. Enquanto este contador é muito útil em casos em que o servidor fica sem recursos, você pode usá-lo para rastrear o atraso de entrada do usuário relacionado a um aplicativo específico.

## <a name="configuration-options"></a>Opções de configuração

Uma coisa importante a lembrar ao usar esse contador de desempenho é que ele relata o atraso de entrada do usuário em um intervalo de 1.000 ms por padrão. Se você definir a propriedade de intervalo de amostra de contador de desempenho (como mostrado na seguinte captura de tela) como algo diferente, o valor relatado será incorreto.

![Área de trabalho remota - as propriedades para o monitor de desempenho](.\media\rds-user-input-perfmon-properties.png)

Para corrigir isso, você pode definir a seguinte chave do registro para coincidir com o intervalo (em milissegundos) que você deseja usar. Por exemplo, se alterarmos exemplo cada x segundos para 5 segundos, precisamos definir essa chave para 5.000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Se você estiver usando o Windows 10, versão 1809 ou posterior ou Windows Server 2019 ou posterior, você não precisa definir LagCounterInterval para corrigir o contador de desempenho.

Também adicionamos algumas chaves que podem ser úteis na mesma chave do registro:

**LagCounterImageNameFirst** — defina essa chave como `DWORD 1` (valor padrão 0 ou a chave não existe). Isso altera os nomes do contador para o "Nome da imagem < SessionID:ProcessId >". Por exemplo, "Gerenciador" < 1:7964 >. Isso é útil se você deseja classificar pelo nome da imagem.

**LagCounterShowUnknown** — defina essa chave como `DWORD 1` (valor padrão 0 ou a chave não existe). Isso mostra todos os processos que estão sendo executados como serviços ou do sistema. Alguns processos aparecerão com sua sessão definida como "?."

Este é o que se parece se você ativar as duas chaves:

![Área de trabalho remota – o monitor de desempenho com ambas as chaves no](.\media\rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Usando os novos contadores com ferramentas de não-Microsoft

Ferramentas de monitoramento podem consumir esse contador usando o [Perfmon API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Baixar o software do Windows Server Insider

Participantes registrados podem navegar diretamente para o [página de download do Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) para obter o software mais recente do Insider downloads.  Para saber como se registrar como um Insider, consulte [Introdução ao servidor](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Compartilhe seus comentários

Você pode enviar comentários para esse recurso por meio do Hub de comentários. Selecione **aplicativos > todos os outros aplicativos** e incluem "contadores de desempenho de RDS — o monitor de desempenho" no título da sua postagem.

Para obter ideias de recurso geral, visite o [página de UserVoice do RDS](https://aka.ms/uservoice-rds).
