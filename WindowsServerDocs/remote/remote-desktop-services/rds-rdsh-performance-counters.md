---
title: Use contadores de desempenho para diagnosticar os problemas de capacidade de resposta do aplicativo nos Hosts da Sessão da Área de Trabalho Remota
description: Seu aplicativo está executando lentamente no RDS? Saiba mais sobre contadores de desempenho que você pode usar para diagnosticar problemas de desempenho do aplicativo no RDSH
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/11/2019
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7b222104abd5b0b964bac748c3be15049075191d
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950423"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Use contadores de desempenho para diagnosticar os problemas de desempenho do aplicativo nos Hosts da Sessão da Área de Trabalho Remota

> Aplica-se a: Windows Server 2019, Windows 10

Um dos problemas mais difíceis de diagnosticar é o desempenho insatisfatório do aplicativo – quando os aplicativos estão lentos ou não respondem. Tradicionalmente, você pode iniciar o seu diagnóstico pela coleta de CPU, memória, entrada/saída de disco e outras métricas e, em seguida, usar ferramentas como o Windows Performance Analyzer para tentar descobrir o que está causando o problema. Na maioria das situações, esses dados não o ajudarão a identificar a causa raiz, porque contadores de consumo de recursos têm variações grandes e frequentes. Isso torna difícil ler os dados e correlacioná-los com o problema relatado. Para ajudá-lo a resolver seus problemas de desempenho do aplicativo rapidamente, adicionamos alguns novos contadores de desempenho (disponíveis [para download](#download-windows-server-insider-software) por meio do [Programa Windows Insider](https://insider.windows.com)) que medem os fluxos de entrada do usuário.

>[!NOTE]
>O contador de Atraso de Entrada do Usuário só é compatível com:
> - Windows Server 2019 ou posterior
> - Windows 10, versão 1809 ou posterior

O contador de Atraso de Entrada do Usuário pode ajudá-lo a identificar rapidamente a causa raiz de experiências de RDP ruins do usuário final. Esse contador mede por quanto tempo qualquer entrada do usuário (por exemplo, uso de mouse ou teclado) permanece na fila antes de ser captada por um processo. O contador funciona em sessões locais e remotas.

A imagem a seguir mostra uma representação aproximada do fluxo de entrada do usuário do cliente para o aplicativo.

![Área de Trabalho Remota – fluxos de entrada do usuário do cliente de Área de Trabalho Remota para o aplicativo](./media/rds-user-input.png)

O contador de Atraso de Entrada do Usuário mede o delta máximo (dentro de um intervalo de tempo) entre a entrada que está sendo enfileirada e quando ela é coletada pelo aplicativo em um [loop de mensagem tradicional](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), conforme mostrado no fluxograma a seguir:

![Área de Trabalho Remota – fluxo de contador de desempenho de Atraso de Entrada do Usuário](./media/rds-user-input-delay.png)

Um detalhe importante desse contador é que ele relata o atraso máximo de entrada de usuário dentro de um intervalo configurável. Isso é o tempo mais longo necessário para uma entrada alcançar o aplicativo, o que pode afetar a velocidade de ações visíveis e importantes, tais como digitar.

Por exemplo, na tabela a seguir, o atraso de entrada do usuário seria informado como 1.000 ms dentro deste intervalo. O contador informa o atraso de entrada do usuário mais lento no intervalo porque a percepção do usuário de "lento" é determinada pelo tempo de entrada mais lento (o máximo) que ele experimenta, não pela velocidade média de todas as entradas no total.

|Número| 0 | 1 | 2 |
|------|---|---|---|
|Atraso |16 ms| 20 ms| 1\.000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Habilitar e usar os novos contadores de desempenho

Para usar esses novos contadores de desempenho, você precisa primeiro habilitar uma chave do Registro executando este comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Se estiver usando o Windows 10 versão 1809 ou posterior ou Windows Server 2019 ou posterior, você não precisará habilitar a chave do Registro.

Em seguida, reinicie o servidor. Depois, abra o Monitor de Desempenho e selecione o sinal de adição (+), conforme mostrado na captura de tela a seguir.

![Área de Trabalho Remota – uma captura de tela mostrando como adicionar o contador de desempenho de atraso de entrada do usuário](./media/rds-add-user-input-counter-screen.png)

Depois de fazer isso, você deverá ver a caixa de diálogo Adicionar Contadores, na qual você poderá selecionar **Atraso de Entrada do Usuário por Processo** ou **Atraso de Entrada do Usuário por Sessão**.

![Área de Trabalho Remota – uma captura de tela mostrando como adicionar atraso de entrada do usuário por sessão](./media/rds-user-delay-per-session.png)

![Área de Trabalho Remota – uma captura de tela mostrando como adicionar atraso de entrada do usuário por processo](./media/rds-user-delay-per-process.png)

Se selecionar **Atraso de Entrada do Usuário por Processo**, você verá as **instâncias do objeto selecionado** (em outras palavras, os processos) no formato ```SessionID:ProcessID <Process Image>```.

Por exemplo, se o aplicativo Calculadora estiver executando uma [ID de Sessão 1](https://msdn.microsoft.com/library/ms524326.aspx), você verá ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Nem todos os processos são incluídos. Você não verá todos os processos que estão sendo executados como SYSTEM.

O contador começará a reportar o atraso de entrada do usuário assim que você adicioná-lo. Observe que a escala máxima é definida como 100 (ms) por padrão. 

![Área de Trabalho Remota – um exemplo de atividade para o atraso de entrada do usuário por processo no Monitor de Desempenho](./media/rds-sample-user-input-delay-perfmon.png)

Em seguida, vamos examinar o **Atraso de entrada do usuário por sessão**. Há instâncias para cada ID de sessão e os respectivos contadores mostram o atraso de entrada do usuário de qualquer processo dentro da sessão especificada. Além disso, há duas instâncias chamadas "Max" (o atraso de entrada do usuário máximo entre todas as sessões) e "Média" (a média entre todas as sessões).

Esta tabela mostra um exemplo visual dessas instâncias. (Você pode obter as mesmas informações no Perfmon, alternando para o tipo de grafo de Relatório.)

|Tipo de contador|Nome da instância|Atraso relatado (ms)|
|---------------|-------------|-------------------|
|Atraso de Entrada do Usuário por processo|1:4232 <Calculator.exe>|  200|
|Atraso de Entrada do Usuário por processo|2:1000 <Calculator.exe>|  16|
|Atraso de Entrada do Usuário por processo|1:2000 <Calculator.exe>|  32|
|Atraso de entrada do usuário por sessão|1|    200|
|Atraso de entrada do usuário por sessão|2|    16|
|Atraso de entrada do usuário por sessão|Média|  108|
|Atraso de entrada do usuário por sessão|Máx.|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Contadores usados em um sistema sobrecarregado

Agora vamos examinar o que você vê no relatório se o desempenho de um aplicativo está degradado. O gráfico a seguir mostra leituras para usuários que trabalham remotamente no Microsoft Word. Nesse caso, o desempenho do servidor RDSH degrada ao longo do tempo conforme mais usuários fazem logon.

![Área de Trabalho Remota – um exemplo de grafo de desempenho para o servidor RDSH executando o Microsoft Word](./media/rds-user-input-perf-graph.png)

Eis aqui como ler as linhas do grafo:

- A linha rosa mostra o número de sessões conectadas no servidor.
- A linha vermelha é o uso da CPU.
- A linha verde é o atraso máximo de entrada de usuário entre todas as sessões.
- A linha azul (exibida como preta nesse grafo) representa o atraso médio de entrada do usuário entre todas as sessões.

Você observará que há uma correlação entre picos de CPU e o atraso da entrada do usuário – à medida que a CPU é mais usada, o atraso da entrada do usuário aumenta. Além disso, conforme mais usuários são adicionados ao sistema, o uso da CPU fica próximo a 100%, levando a picos de atraso de entrada do usuário mais frequentes. Embora esse contador seja muito útil nos casos em que o servidor fica sem recursos, você também pode usá-lo para rastrear o atraso de entrada do usuário relacionado a um aplicativo específico.

## <a name="configuration-options"></a>Opções de configuração

Uma coisa importante a se lembrar ao usar esse contador de desempenho é que ele relata o atraso de entrada do usuário em um intervalo de 1.000 ms por padrão. Se você definir a propriedade de intervalo de amostra de contador de desempenho (como mostrado na captura de tela a seguir) como algo diferente, o valor relatado será incorreto.

![Área de Trabalho Remota – as propriedades para o monitor de desempenho](./media/rds-user-input-perfmon-properties.png)

Para corrigir isso, você pode definir a chave do Registro a seguir para corresponder ao intervalo (em milissegundos) que você deseja usar. Por exemplo, se alterarmos a amostra a cada x segundos para 5 segundos, precisaremos definir essa chave para 5.000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Se estiver usando o Windows 10 versão 1809 ou posterior ou o Windows Server 2019 ou posterior, você não precisará definir LagCounterInterval para corrigir o contador de desempenho.

Também adicionamos algumas chaves que podem ser úteis sob a mesma chave do Registro:

**LagCounterImageNameFirst** – defina essa chave como `DWORD 1` (o valor padrão 0 ou a chave não existe). Isso altera os nomes do contador para "Nome da Imagem <IDdaSessão:IDdoProcesso>." Por exemplo, "explorer <1:7964>". Isso é útil se você deseja classificar por nome da imagem.

**LagCounterShowUnknown** – defina essa chave como `DWORD 1` (o valor padrão 0 ou a chave não existe). Isso mostra todos os processos que estão sendo executados como serviços ou SYSTEM. Alguns processos aparecerão com sua sessão definida como "?."

Eis aqui a aparência que isso teria se você ativasse ambas as chaves:

![Área de Trabalho Remota – o monitor de desempenho com ambas as chaves ativadas](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Usando os novos contadores com ferramentas não Microsoft

Ferramentas de monitoramento podem consumir esse contador usando a [API do Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Baixar o software do Windows Server Insider

Insiders registrados podem navegar diretamente para a [página de download do Windows Server Insider Preview](https://www.microsoft.com/software-download/windowsinsiderpreviewserver) para obter os downloads de software mais recentes do Insider.  Para saber como se registrar como um Insider, confira [Introdução ao servidor](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Compartilhe seus comentários

Você pode enviar comentários para esse recurso por meio do Hub de Comentários. Selecione **Aplicativos > Todos os outros aplicativos** e inclua "Contadores de desempenho do RDS – monitor de desempenho" no título da sua postagem.

Para obter ideias gerais sobre os recursos, visite a [página de UserVoice do RDS](https://aka.ms/uservoice-rds).
