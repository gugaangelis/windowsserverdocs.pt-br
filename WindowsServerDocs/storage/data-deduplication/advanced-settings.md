---
ms.assetid: 01c8cece-66ce-4a83-a81e-aa6cc98e51fc
title: Configurações avançadas de Eliminação de Duplicação de Dados
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 73f9ce6e88fa56a645f0ffedba4f38dec87e973b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936373"
---
# <a name="advanced-data-deduplication-settings"></a>Configurações avançadas de Eliminação de Duplicação de Dados

> Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este documento descreve como alterar configurações avançadas de [Eliminação de Duplicação de Dados](overview.md). Para [cargas de trabalho recomendadas](install-enable.md#enable-dedup-candidate-workloads), as configurações padrão devem ser suficientes. O principal motivo para alterar essas configurações é melhorar o desempenho da Eliminação de Duplicação de Dados com outros tipos de cargas de trabalho.

## <a name="modifying-data-deduplication-job-schedules"></a><a id="modifying-job-schedules"></a>Alterar o plano de trabalho da Eliminação de Duplicação de Dados
Os [planos de trabalho de Eliminação de Duplicação de Dados padrão](understand.md#job-info) são projetados para funcionarem bem com cargas de trabalho recomendadas e serem o menos intrusivos possível (exceto o trabalho *Otimização de Prioridade*, que está habilitado para o tipo de uso [**Backup**](understand.md#usage-type-backup)). Quando as cargas de trabalho têm grandes requisitos de recursos, é possível fazer com que os trabalhos sejam executados somente durante horas ociosas, ou reduzir ou aumentar a quantidade de recursos do sistema que um trabalho de Eliminação de Duplicação de Dados pode consumir.

### <a name="changing-a-data-deduplication-schedule"></a><a id="modifying-job-schedules-change-schedule"></a>Alterar um plano de Eliminação de Duplicação de Dados
Os trabalhos de Eliminação de Duplicação de Dados são programados pelo Agendador de Tarefas do Windows e podem ser exibidos e editados lá no caminho Microsoft\Windows\Deduplication. A Eliminação de Duplicação de Dados inclui vários cmdlets que facilitam o agendamento.
* [`Get-DedupSchedule`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))mostra os trabalhos agendados atuais.
* [`New-DedupSchedule`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))Cria um novo trabalho agendado.
* [`Set-DedupSchedule`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))modifica um trabalho agendado existente.
* [`Remove-DedupSchedule`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))Remove um trabalho agendado.

O motivo mais comum para alterar quando executar trabalhos de Eliminação de Duplicação de Dados é garantir que os trabalhos sejam executados durante fora do horário comercial. O exemplo de passo a passo a seguir mostra como modificar o plano de Eliminação de Duplicação de Dados para um cenário em que *tudo corre bem*: um host hiperconvergido do Hyper-V que fica ocioso nos fins de semana e depois das 19h durante a semana. Para alterar a agenda, execute os cmdlets do PowerShell a seguir em um contexto de Administrador.

1. Desabilite os trabalhos de [Otimização](understand.md#job-info-optimization)agendados por hora.
    ```PowerShell
    Set-DedupSchedule -Name BackgroundOptimization -Enabled $false
    Set-DedupSchedule -Name PriorityOptimization -Enabled $false
    ```

2. Remova os trabalhos de [Coleta de Lixo](understand.md#job-info-gc) e [Anulação de Integridade](understand.md#job-info-scrubbing) agendados atualmente.
    ```PowerShell
    Get-DedupSchedule -Type GarbageCollection | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    Get-DedupSchedule -Type Scrubbing | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    ```

3. Crie um trabalho noturno de Otimização a ser executado às 19h com prioridade alta e todas as CPUs e memória disponíveis no sistema.
    ```PowerShell
    New-DedupSchedule -Name "NightlyOptimization" -Type Optimization -DurationHours 11 -Memory 100 -Cores 100 -Priority High -Days @(1,2,3,4,5) -Start (Get-Date "2016-08-08 19:00:00")
    ```

    >[!NOTE]
    > A parte *date* do parâmetro `System.Datetime` fornecido a `-Start` é irrelevante (desde que esteja no passado), mas a parte *time* especifica quando o trabalho deve começar.
4. Crie um trabalho de Coleta de Lixo semanal a ser executado no sábado, começando às 19h, com prioridade alta e todas as CPUs e memória disponíveis no sistema.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyGarbageCollection" -Type GarbageCollection -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(6) -Start (Get-Date "2016-08-13 07:00:00")
    ```

5. Crie um trabalho de Anulação de Integridade semanal a ser executado no domingo, começando às 7h, com prioridade alta e todas as CPUs e memória disponíveis no sistema.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyIntegrityScrubbing" -Type Scrubbing -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(0) -Start (Get-Date "2016-08-14 07:00:00")
    ```

### <a name="available-job-wide-settings"></a><a id="modifying-job-schedules-available-settings"></a>Configurações disponíveis para todos os trabalhos
Você pode alternar as seguintes configurações para trabalhos de Eliminação de Duplicação de Dados novos ou agendados:

<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nome do parâmetro</th>
            <th>Definição</th>
            <th>Valores aceitos</th>
            <th>Por que você deseja definir esse valor?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Type</td>
            <td>O tipo do trabalho que deve ser agendado</td>
            <td>
                <ul>
                    <li>Otimização</li>
                    <li>Coleta de Lixo</li>
                    <li>Anulação</li>
                </ul>
            </td>
            <td>Esse valor é necessário porque é o tipo de trabalho que você deseja agendar. Esse valor não pode ser alterado depois que a tarefa é agendada.</td>
        </tr>
        <tr>
            <td>Prioridade</td>
            <td>A prioridade do sistema do trabalho agendado</td>
            <td>
                <ul>
                    <li>Alto</li>
                    <li>Médio</li>
                    <li>Baixo</li>
                </ul>
            </td>
            <td>Esse valor ajuda o sistema a determinar como alocar tempo da CPU. <em>Alta</em> usará mais tempo de CPU; <em>Baixa</em> usará menos.</td>
        </tr>
        <tr>
            <td>Dias</td>
            <td>Os dias em que o trabalho foi agendado</td>
            <td>Uma matriz de números inteiros de 0 a 6 que representa os dias da semana:<ul>
                <li>0 = Domingo</li>
                <li>1 = Segunda-feira</li>
                <li>2 = Terça-feira</li>
                <li>3 = Quarta-feira</li>
                <li>4 = Quinta-feira</li>
                <li>5 = Sexta-feira</li>
                <li>6 = Sábado</li>
            </ul></td>
            <td>Tarefas agendadas devem ser executadas em pelo menos um dia.</td>
        </tr>
        <tr>
            <td>Núcleos</td>
            <td>O percentual de núcleos no sistema que um trabalho deve usar</td>
            <td>Números inteiros de 0 a 100 (indica uma porcentagem)</td>
            <td>Para controlar o nível de impacto que um trabalho terá nos recursos de computação no sistema</td>
        </tr>
        <tr>
            <td>DurationHours</td>
            <td>O número máximo de horas que um trabalho deve ter permissão de execução</td>
            <td>Números inteiros positivos</td>
            <td>Para impedir que um trabalho seja executado em uma carga&#39;s horas não ociosas</td>
        </tr>
        <tr>
            <td>Habilitada</td>
            <td>Se o trabalho será executado ou não</td>
            <td>Verdadeiro/Falso</td>
            <td>Para desabilitar um trabalho sem removê-lo</td>
        </tr>
        <tr>
            <td>Completo</td>
            <td>Para agendar um trabalho de Coleta de Lixo completo</td>
            <td>Alternar (verdadeiro/falso)</td>
            <td>Por padrão, o quarto trabalho sempre é um trabalho de Coleta de Lixo. Com essa opção, você pode agendar a coleta de lixo completa para ser executada com maior frequência.</td>
        </tr>
        <tr>
            <td>InputOutputThrottle</td>
            <td>Especifica a quantidade de limitação de entrada/saída aplicada ao trabalho</td>
            <td>Números inteiros de 0 a 100 (indica uma porcentagem)</td>
            <td>A limitação garante que os trabalhos Don&#39;t interfiram com outros processos com uso intensivo de e/s.</td>
        </tr>
        <tr>
            <td>Memória</td>
            <td>A porcentagem de memória do sistema que um trabalho deve usar</td>
            <td>Números inteiros de 0 a 100 (indica uma porcentagem)</td>
            <td>Para controlar o nível de impacto que o trabalho terá sobre os recursos de memória do sistema</td>
        </tr>
        <tr>
            <td>Nome</td>
            <td>O nome do trabalho agendado</td>
            <td>String</td>
            <td>Um trabalho deve ter um nome de identificação exclusivo.</td>
        </tr>
        <tr>
            <td>ReadOnly</td>
            <td>Indica que o trabalho de anulação processa e relata as corrupções encontradas, mas não executa ações de reparo</td>
            <td>Alternar (verdadeiro/falso)</td>
            <td>Você deseja restaurar manualmente os arquivos que ficam em seções inválidas do disco.</td>
        </tr>
        <tr>
            <td>Iniciar</td>
            <td>Especifica a hora em que um trabalho deve ser iniciado</td>
            <td><code>System.DateTime</code></td>
            <td>A parte de <em>Data</em> do <code>System.Datetime</code> desde o <em>início</em> é irrelevante (desde que&#39;s no passado), mas a parte de <em>hora</em> especifica quando o trabalho deve ser iniciado.</td>
        </tr>
        <tr>
            <td>StopWhenSystemBusy</td>
            <td>Especifica se a Eliminação de Duplicação de Dados deve parar quando o sistema está ocupado</td>
            <td>Alternar (Verdadeiro/Falso)</td>
            <td>Essa opção oferece a capacidade de controlar o comportamento da Eliminação de Duplicação de Dados. Isso é particularmente importante se você deseja executar a Eliminação de Duplicação de Dados enquanto sua carga de trabalho não está ociosa.</td>
        </tr>
    </tbody>
</table>

## <a name="modifying-data-deduplication-volume-wide-settings"></a><a id="modifying-volume-settings"></a>Alterar configurações de Eliminação de Duplicação de Dados em todo o volume
### <a name="toggling-volume-settings"></a><a id="modifying-volume-settings-how-to-toggle"></a>Alternar configurações de volume
Você pode definir as configurações padrão para Eliminação de Duplicação de Dados de todo o volume por meio do [tipo de uso](understand.md#usage-type) que você seleciona quando habilita a eliminação de duplicação de um volume. A Eliminação de Duplicação de Dados inclui cmdlets que facilitam a edição das configurações de todo o volume:

* [`Get-DedupVolume`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))
* [`Set-DedupVolume`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))

As principais razões para modificar as configurações de volume do tipo de uso selecionado são para melhorar o desempenho de leitura de arquivos específicos (como multimídia ou outros tipos de arquivos já compactados) ou para ajustar a Eliminação de Duplicação de Dados e ter uma otimização melhor da sua carga de trabalho específica. O exemplo a seguir mostra como alterar as configurações de Eliminação de Duplicação de Dados do volume para uma carga de trabalho que se parece mais com uma carga de trabalho do servidor de arquivos para uso geral, mas utiliza arquivos grandes que mudam com frequência.

1. confira as configurações de volume atuais para o Volume Compartilhado Clusterizado 1.
    ```PowerShell
    Get-DedupVolume -Volume C:\ClusterStorage\Volume1 | Select *
    ```

2. Habilite OptimizePartialFiles no Volume Compartilhado Clusterizado 1 para que a política MinimumFileAge se aplique a seções do arquivo em vez de se aplicar a todo o arquivo. Isso faz com que a maior parte do arquivo seja otimizada, mesmo que as seções do arquivo sejam alteradas regularmente.
    ```PowerShell
    Set-DedupVolume -Volume C:\ClusterStorage\Volume1 -OptimizePartialFiles
    ```

### <a name="available-volume-wide-settings"></a><a id="modifying-volume-settings-available-settings"></a>Configurações de todo o volume disponíveis
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nome da configuração</th>
            <th>Definição</th>
            <th>Valores aceitos</th>
            <th>Por que você deseja alterar esse valor?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ChunkRedundancyThreshold</td>
            <td>O número de vezes que uma parte é referenciada antes de ser duplicada na seção do ponto de acesso do Repositório de partes. O valor da seção HotSpot é que, portanto, chamadas &quot; de &quot; partes ativas que são referenciadas com frequência têm vários caminhos de acesso para melhorar o tempo de acesso.</td>
            <td>Números inteiros positivos</td>
            <td>O principal motivo para alterar esse número é o aumento da taxa de economia de volumes com eliminação de duplicação alta. Em geral, o valor padrão (100) é a configuração recomendada, e você deve ter&#39;t precisa modificar isso.</td>
        </tr>
        <tr>
            <td>ExcludeFileType</td>
            <td>Tipos de arquivos que são excluídos da otimização</td>
            <td>Matriz de extensões de arquivo</td>
            <td>Alguns tipos de arquivo, especialmente multimídia ou arquivos já compactados, não se beneficiam muito da otimização. Essa configuração permite que você configure quais tipos são excluídos.</td>
        </tr>
        <tr>
            <td>ExcludeFolder</td>
            <td>Especifica caminhos de pasta que não devem ser considerados para otimização</td>
            <td>Matriz de caminhos de pasta</td>
            <td>Se você quer melhorar o desempenho ou evitar que o conteúdo de determinados caminhos sejam otimizados, pode excluir determinados caminhos do volume na consideração para otimização.</td>
        </tr>
        <tr>
            <td>InputOutputScale</td>
            <td>Especifica o nível da paralelização de E/S (filas E/S) que a Eliminação de Duplicação de Dados usa em um volume durante um trabalho de pós-processamento</td>
            <td>Números inteiros positivos no intervalo de 1 a 36</td>
            <td>O principal motivo para modificar esse valor é a diminuição do impacto no desempenho de uma carga de trabalho alta de E/S restringindo o número de filas de E/S que a Eliminação de Duplicação de Dados pode usar em um volume. Observe que a modificação dessa configuração a partir do padrão pode causar a eliminação de duplicação de dados&#39;os trabalhos de pós-processamento sejam executados lentamente.</td>
        </tr>
        <tr>
            <td>MinimumFileAgeDays</td>
            <td>Número de dias após a criação do arquivo antes que o arquivo seja considerado na política para otimização.</td>
            <td>Números inteiros positivos (incluindo zero)</td>
            <td>Os tipos de uso <strong>Padrão</strong> e <strong>HyperV</strong> definem esse valor como 3 para maximizar o desempenho em arquivos populares ou recém-criados. Convém modificar isso se você quer que a Eliminação de Duplicação de Dados seja mais agressiva ou se não se importa com a latência extra associada à eliminação de duplicação.</td>
        </tr>
        <tr>
            <td>MinimumFileSize</td>
            <td>Tamanho mínimo que um arquivo precisa ter para ser considerado na política de otimização</td>
            <td>Números inteiros positivos (bytes) maiores que 32 KB</td>
            <td>O principal motivo para alterar esse valor é a exclusão de arquivos pequenos que podem ter limitado o valor de otimização para economizar tempo de computação.</td>
        </tr>
        <tr>
            <td>NoCompress</td>
            <td>Se os fragmentos devem ser compactados antes de serem colocados no Repositório de Partes ou não</td>
            <td>Verdadeiro/Falso</td>
            <td>Alguns tipos de arquivos, especialmente arquivos multimídia e tipos de arquivos já compactados, não podem ser compactados direito. Essa configuração permite que você desative a compactação para todos os arquivos no volume. Isso é ideal se você está otimizando um conjunto de dados que tem muitos arquivos já compactados.</td>
        </tr>
        <tr>
            <td>NoCompressionFileType</td>
            <td>Tipos de arquivo cujas partes não devem ser comprimidas antes de irem para o Repositório de partes</td>
            <td>Matriz de extensões de arquivo</td>
            <td>Alguns tipos de arquivos, especialmente arquivos multimídia e tipos de arquivos já compactados, não podem ser compactados direito. Essa configuração permite que a compactação seja desligada para esses arquivos, poupando recursos da CPU.</td>
        </tr>
        <tr>
            <td>OptimizeInUseFiles</td>
            <td>Quando habilitado, os arquivos que têm identificadores ativos serão considerados na política de otimização.</td>
            <td>Verdadeiro/Falso</td>
            <td>Habilite essa configuração se sua carga de trabalho mantém os arquivos abertos por longos períodos de tempo. Se essa configuração não estiver habilitada, um arquivo nunca será otimizado se a carga de trabalho tiver um identificador aberto para ele, mesmo se&#39;s apenas anexando dados no final.</td>
        </tr>
        <tr>
            <td>OptimizePartialFiles</td>
            <td>Quando habilitado, o valor de MinimumFileAge se aplica a segmentos de um arquivo em vez se aplicar a todo ele.</td>
            <td>Verdadeiro/Falso</td>
            <td>Habilite essa configuração se sua carga de trabalho funciona com arquivos grandes e editados frequentemente, quando a maior parte do conteúdo do arquivo permanece inalterada. Se essa configuração não está habilitada, esses arquivos nunca são otimizados porque são sempre alterados, mesmo quando a maior parte do conteúdo do arquivo está pronta para ser otimizada.</td>
        </tr>
        <tr>
            <td>Verificar</td>
            <td>Quando habilitada, se o hash de uma parte corresponde a uma parte que já temos em nosso Repositório de Partes, os fragmentos são comparados byte a byte para garantir que são idênticos.</td>
            <td>Verdadeiro/Falso</td>
            <td>Esse é um recurso de integridade que não permite que o algoritmo de hash que compara partes cometa um erro comparando dois blocos de dados que são diferentes, mas têm o mesmo hash. Na prática, é extremamente improvável que isso aconteça. A habilitação do recurso de verificação adiciona sobrecarga significativa ao trabalho de otimização.</td>
        </tr>
    </tbody>
</table>

## <a name="modifying-data-deduplication-system-wide-settings"></a><a id="modifying-dedup-system-settings"></a>Alterar configurações de Eliminação de Duplicação de Dados em todo o sistema
A Eliminação de Duplicação de Dados tem configurações adicionais em todo o sistema que podem ser definidas no [registro](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755256(v=ws.11)). Essas configurações se aplicam a todos os trabalhos e aos volumes que são executados no sistema. Redobre os cuidados ao editar o registro.

Por exemplo, convém desabilitar a coleta de lixo completa. Para saber mais sobre a utilidade disso em seu cenário, confira as [Perguntas frequentes](#faq-why-disable-full-gc). Para editar o registro com o PowerShell:

* Se a Eliminação de Duplicação de Dados está em execução em um cluster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    Set-ItemProperty -Path HKLM:\CLUSTER\Dedup -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

* Se a Eliminação de Duplicação de Dados não está em execução em um cluster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

### <a name="available-system-wide-settings"></a><a id="modifying-dedup-system-settings-available-settings"></a>Configurações de todo o sistema disponíveis
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nome da configuração</th>
            <th>Definição</th>
            <th>Valores aceitos</th>
            <th>Por que você deseja alterar isso?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WlmMemoryOverPercentThreshold</td>
            <td>Essa configuração permite que os trabalhos usem mais memória do que os juízes da Eliminação de Duplicação de Dados para realmente estarem disponíveis. Por exemplo, uma configuração de 300 significaria que o trabalho precisa usar três vezes mais a memória atribuída para ser cancelada.</td>
            <td>Números inteiros positivos (um valor 300 significa 300% ou 3 vezes)</td>
            <td>Se você tem outra tarefa, ela será interrompida se a Eliminação de Duplicação de Dados usar mais memória</td>
        </tr>
        <tr>
            <td>DeepGCInterval</td>
            <td>Essa configuração define o intervalo no qual os trabalhos de Coleta de Lixo regulares se tornam <a href="advanced-settings.md#faq-full-v-regular-gc" data-raw-source="[full Garbage Collection jobs](advanced-settings.md#faq-full-v-regular-gc)">Coleta de Lixo completa</a>. Uma configuração de X significa que a cada <sup>X</sup> trabalhos, um era trabalho de Coleta de Lixo completa. Observe que a Coleta de Lixo completa sempre está desabilitada (independentemente do valor do registro) para volumes com o <a href="understand.md#usage-type-backup" data-raw-source="[Backup Usage Type](understand.md#usage-type-backup)">tipo de uso Backup</a>. <code>Start-DedupJob -Type GarbageCollection -Full</code>poderá ser usado se a coleta de lixo completa for desejada em um volume de backup.</td>
            <td>Números inteiros (-1 indica desabilitada)</td>
            <td>Confira <a href="advanced-settings.md#faq-why-disable-full-gc" data-raw-source="[this frequently asked question](advanced-settings.md#faq-why-disable-full-gc)">esta pergunta frequente</a></td>
        </tr>
    </tbody>
</table>

## <a name="frequently-asked-questions"></a><a id="faq"></a>Perguntas frequentes
<a id="faq-use-responsibly"></a>**Mudei uma configuração da Eliminação de Duplicação de Dados e agora os trabalhos estão lentos ou não são concluídos, ou o desempenho da carga de trabalho diminuiu. Por que?**
Essas configurações dão bastante poder de controle sobre a execução da Eliminação de Duplicação de Dados. Use-as com responsabilidade e [monitore o desempenho](run.md#monitoring-dedup).

<a id="faq-running-dedup-jobs-manually"></a>**Quero executar um trabalho de eliminação de duplicação de dados no momento, mas não quero criar uma nova agenda – posso fazer isso?**
Sim, [todos os trabalhos podem ser executados manualmente](run.md#running-dedup-jobs-manually).

<a id="faq-full-v-regular-gc"></a>**Qual é a diferença entre a coleta de lixo completa e regular?**
Há dois tipos de [coleta de lixo](understand.md#job-info-gc):

- A *Coleta de Lixo regular* usa um algoritmo estatístico para encontrar as partes grandes não referenciadas que atendem a certos critérios (memória e IOPs baixos). A Coleta de Lixo regular compacta um contêiner de repositório de partes somente se uma porcentagem mínima das partes não for referenciada. Esse tipo de Coleta de Lixo é executado muito mais rapidamente e usa menos recursos do que a Coleta de Lixo completa. O agendamento padrão de trabalho de Coleta de Lixo regular é a execução uma vez por semana.
- A *Coleta de Lixo completa* faz um trabalho muito mais completo de localizar partes não referenciadas e liberar mais espaço em disco. A Coleta de Lixo completa compacta cada contêiner, mesmo que só haja uma única parte no contêiner sem referência. A Coleta de Lixo completa também libera espaço que pode estar em uso quando há uma falha ou queda de energia durante um trabalho de Otimização. Os trabalhos de Coleta de Lixo completa recuperarão 100% do espaço disponível que pode ser recuperado em um volume com eliminação de duplicação com o custo de exigir mais tempo e recursos do sistema em comparação com um trabalho de Coleta de Lixo regular. O trabalho de Coleta de Lixo completa normalmente encontra e libera até 5% mais dados não referenciados do que um trabalho de Coleta de Lixo regular. O agendamento padrão do trabalho de Coleta de Lixo completa é a execução na quarta vez em que uma Coleta de Lixo está agendada.

<a id="faq-why-disable-full-gc"></a>**Por que quero desabilitar a coleta de lixo completa?**
- A Coleta de Lixo pode prejudicar o tempo de vida da cópia de sombra de vida útil do volume e o tamanho do backup incremental. Variação alta ou cargas de trabalho com E/S intensivas podem ver uma degradação no desempenho por causa dos trabalhos de Coleta de Lixo completa.
- Você pode executar um trabalho de Coleta de Lixo completa no PowerShell manualmente para limpar vazamentos se você sabe que o seu sistema falhou.
