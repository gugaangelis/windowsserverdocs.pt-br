---
title: Solucionar problemas do Log de Inventário de Software
description: Descreve como resolver problemas comuns de implantação de log de inventário de Software.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 8a1fe22a1f721c0ac94096fe22c559c9bb092293
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435309"
---
# <a name="troubleshoot-software-inventory-logging"></a>Solucionar problemas do Log de Inventário de Software 

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

## <a name="understanding-sil"></a>Noções básicas sobre o SIL

Antes de iniciar o SIL de solução de problemas, você deve ter uma boa compreensão de seus componentes e como ele funciona. Os vídeos a seguir fornecem uma visão geral do SIL e o agregador do SIL e como usá-los para encaminhar e dados de inventário do relatório:

1. [Uma introdução ao inventário de Software SIL (log) (57:10)](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Software de log de inventário: Como configurar o agregador do SIL (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Software de log de inventário: Habilitar o encaminhamento do SIL (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>Como que o fluxo de dados SIL funciona

A estrutura do SIL tem dois componentes principais e dois canais de comunicação. Fluxo de dados em ambos os canais e entre os dois componentes, é necessário para uma implantação bem-sucedida do SIL (Isso pressupõe um ambiente virtualizado ou nuvem,--ambientes físicos puramente precisam apenas um dos canais de comunicação). Você precisará entender os componentes e fluxo de dados do SIL para implantá-lo corretamente. Depois de assistir os vídeos de visão geral acima, você já viu o diagrama de arquitetura que ilustra os componentes e o fluxo de dados em ambos os canais. Setas laranjas indicam consultas remotas no WinRM, setas tracejadas verdes indicam que HTTPS envia para o agregador do SIL do SIL em cada nó de término do WS:

![](../media/software-inventory-logging/image1.png)

Se você encontrar um problema com o SIL, ele provavelmente está relacionado a uma interrupção no fluxo de dados sobre os canais e entre os componentes. A seguir estão os problemas mais comuns relacionados ao fluxo de dados, seguido as etapas de solução de problemas para resolver cada um dos três problemas na próxima seção:

-   **Problema 1 de fluxo de dados** – **nenhum dado no relatório ao usar o cmdlet Publish-SilReport** (ou dados ausentes em geral).

-   **Problema 2 de fluxo de dados** – **excesso muitos servidores em um Host desconhecido** no relatório.

-   **Problema 3 de fluxo de dados** – **excesso várias VMs em hosts físicos listados como o SO desconhecido** no relatório, e/ou um erro produzidas quando usando **Publish-SilData** em servidores Windows que esteja executando o SIL.

## <a name="troubleshooting-data-flow-issues"></a>Solucionando problemas de fluxo de dados

Antes de começar, você precisará saber quanto tempo atrás agregador do SIL iniciado com o **Start-SilAggregator** cmdlet.

>[!IMPORTANT]
>Não haverá nenhum dado no relatório até que o cubo de dados SQL é processado por hora do sistema local 3h. Não continue com as etapas de solução de problemas até que o cubo processado dados.

Se você estiver solucionando problemas de dados no relatório (ou ausentes do relatório) que é mais recentes que a última vez que o cubo processado ou antes que nunca processe o cubo (para uma nova instalação), siga estas etapas para processar o cubo de dados SQL em tempo real :

1. Faça logon como um administrador do SQL Server e execute **SSMS** em um prompt de comando.
2. Não é possível conectar-se ao Mecanismo do Banco de Dados.
3. Expandir **SQL Server Agent** e, em seguida, expanda **trabalhos**.
4. Clique com botão direito **SILStagingRefresh** e, em seguida, selecione **Iniciar trabalho na etapa**.
5. Clique em **iniciar** e aguarde até que a barra de progresso de atualização ser concluída.
6. Abra o PowerShell como administrador e execute o **silreport publicar - AbrirRelatório** cmdlet.

Se ainda não houver nenhum dado no relatório, continue com a solução de problemas de fluxo de dados de três.

### <a name="data-flow-issue-1"></a>Problema de fluxo de dados 1 

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>Nenhum dado no relatório ao usar o cmdlet Publish-SilReport (ou os dados estão ausentes em geral)

Se dados estão ausentes, provavelmente devido ao cubo de dados SQL não ter processado ainda. Se ele tiver processado recentemente e você acredita que devem ter chegado a dados ausentes no agregador antes do processamento de cubo, siga o caminho dos dados na ordem inversa. Escolha um host exclusivo e uma única VM para solucionar problemas. O caminho de dados em ordem inversa seria **relatório SILA** &lt; **banco de dados SILA** &lt; **diretório local de SILA** &lt;  **host físico remoto** ou **executando o SIL/tarefa do agente de VM de WS**.

#### <a name="check-to-see-if-data-is-in-the-database"></a>Verifique se os dados estão no banco de dados

Há duas maneiras de verificar se há dados: **PowerShell** ou **SSMS**.

>[!Important]
>Se o cubo foi processado pelo menos uma vez, pois o SILA inseriu dados no banco de dados, esses dados devem ser refletidos no relatório. Se não houver nenhum dado no banco de dados, em seguida, qualquer um dos hosts físicos de sondagem está falhando ou nada está sendo recebido via HTTPS, ou ambos.

 #### <a name="powershell"></a>PowerShell

1. Abra o PowerShell como administrador e execute o **get-silvmhost** cmdlet e, em seguida, execute **get-silaggregator**.

    >[!NOTE]
    >A saída de **get-silaggregator** sempre imita a **guia de detalhes do Windows Server** de relatório do Excel. Não espere um resultado diferente.

2. Executar **get-silvmhost**
   - Se não há hosts estiverem listados, em seguida, adicione hosts usando o **silvmhost adicionar** cmdlet.
   - Se os hosts são listados como **desconhecido**, em seguida, vá para o problema 2. 
   d - se hosts são listados, mas não há nenhuma **datetime** sob o **sondagem recente** coluna e, em seguida, vá para **problema 2** abaixo.

**Outros comandos relacionados**

**Get-SilAggregator - Computername &lt;fqdn de um servidor conhecido enviando dados por push&gt;** : Isso produzirá informações do banco de dados sobre um computador (VM), mesmo antes do cubo foi processado. Assim, esse cmdlet pode ser usado para verificar em dados no banco de dados para um envio por push dados de SIL via HTTPS, antes ou sem o processo de cubo às 3:00 (ou se você ainda não tiver atualizado o cubo em tempo real, conforme descrito no início desta seção) do Windows Server.

**Get-SilAggregator - VmHostName &lt;fqdn de um host físico poll onde há um valor na coluna de pesquisa recentes ao usar o cmdlet Get-SilVmHost&gt;** : Isso produzirá informações do banco de dados sobre um host físico, mesmo antes do cubo foi processado.

#### <a name="ssms"></a>SSMS

n**verificar se há dados a partir de hosts sendo pesquisados:**
 
1. Abra **SSMS** e conecte-se para o **mecanismo de banco de dados**.
2. Expandir **bancos de dados**, expanda o **SoftwareInventoryLogging** banco de dados, expanda **tabelas**, clique com botão direito do **HostInfo** tabela e, em seguida, Selecione as primeiras 1000 linhas. 

    Se houver dados para um ou mais hosts na tabela, sondagem, em seguida, para hosts que/those foi bem-sucedida pelo menos uma vez.

   **Verificação de dados de VMs ou servidores autônomos, o que tem enviado por push dados via HTTPS:** 

3. Abra **SSMS** e conecte-se para o **mecanismo de banco de dados**.
   a2. Expandir **bancos de dados**, expanda o **SoftwareInventoryLogging** banco de dados, expanda **tabelas**, clique com botão direito do **VMInfo** tabela e, em seguida, Selecione as 1000 linhas superiores. 

    >[!NOTE] 
    >Cada linha de uma VM exclusiva representará um processado **bmil** arquivo enviado por push via HTTPS e processados pelo agregador do SIL com êxito. Bmil arquivos são arquivos de propriedade usados pelo SIL, uma será criada cada nosso por cada instância do SIL Observe que isso só é necessário quando o SIL e o SILA são usados em ambientes virtuais. Caso contrário, somente o tráfego HTTPS é necessário/esperado).

   Todos os dados no banco de dados devem ser refletidos nos relatórios do SIL após o cubo foi processado.

### <a name="data-flow-issue-2"></a>Problema de fluxo de dados 2
#### <a name="too-many-servers-under-unknown-host"></a>Muitos servidores em um Host desconhecido

Isso é muito provável que ocorram em ambientes virtuais quando o agregador do SIL não está sondando com êxito a hosts físicos que hospedam as máquinas virtuais.

1. Abra **PowerShell** como administrador e execute o **get-silvmhost** cmdlet.

    -   Se os hosts são listados como **desconhecido**, o **silvmhost adicionar** cmdlet não funciona corretamente – geralmente devido a credenciais incorretas adicionadas para o acesso a esses hosts (assim, desconhecido). Mas se credenciais forem verificadas, isso poderia significar a detecção automática de **hosttype** e **hypervisortype** no **adicionar-silvmhost** cmdlet não foi capaz de reconhecer o plataforma. Existem avançados disponíveis para essas situações de etapas de solução de problemas, mas não são abordados aqui (verificação de canais do Visualizador de eventos o agregador do SIL).

    -   Se os hosts estiverem listados, e **hosttype** e **hypervisortype** são listados com valores que não são **desconhecido**, ou seja, Windows e Hyper-v, ou Ubuntu e Xen, etc., mas não há nenhuma **datetime** sob **sondagem recente** coluna, em seguida, a sondagem ainda não com êxito ocorreu.

        -   Você precisará aguardar uma hora depois de adicionar o host de sondagem para ocorrer (supondo que esse intervalo é definido como padrão – pode ser verificada usando o **get-silaggregator** cmdlet).

        -   Se ele tiver sido uma hora, pois o host foi adicionado, verifique se a tarefa de sondagem está em execução: Na **Agendador de tarefas**, selecione **Software Inventory Logging Aggregator** sob **Microsoft** &gt; **Windows** e verificar o histórico da tarefa.

    -   Se um host está listado, mas não há nenhum valor para **RecentPoll**, **HostType**, ou **HypervisorType**, isso pode ser amplamente ignorado. Isso somente ocorrerá em ambientes Hyper-v. Esses dados realmente vem de VM do Windows Server, identificando o host físico que ele está em execução via HTTPS. Isso pode ser útil para identificar uma VM específica que está se comunicando, mas requer a mineração do banco de dados usando o **Get-SilAggregatorData** cmdlet.

Depois que os hosts estão sondando corretamente, você poderá ver os dados para esses hosts físicos no banco de dados SILA onde há uma **datetime** sob **sondagem recente**. Seção 1 problema acima fornece etapas para recuperar os dados.

### <a name="data-flow-issue-3"></a>Problema de fluxo de dados 3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Muitos hosts físicos com VMs listados como o sistema operacional desconhecido

1. Escolha um nó do Windows Server final (VM) que você sabe que está em um desses hosts, faça logon como administrador.
2. Abra o PowerShell como administrador.
3. Verifique se **SilLogging** está em execução usando o **Get-SilLogging** cmdlet.
   - Se em execução, tente enviar manualmente os dados SIL usando **Publish-SilData**.

   - Se houver um erro:
     - Verifique se o **targeturi** tem **https://** na entrada.
     - Verifique se que todos os pré-requisitos foram atendidos 
     - Verifique se todas as atualizações necessárias para o Windows Server estão instaladas (consulte os pré-requisitos para o SIL). Uma maneira rápida de verificar (no WS 2012 R2 apenas) é procurar por elas usando o seguinte cmdlet: **Get-SilWindowsUpdate \*3060, \*3000**
     - Verifique se o certificado que está sendo usado para autenticar no agregador está instalado no repositório correto no servidor local a ser inventariado com **SilLogging**.
     - No agregador do SIL, certifique-se que a impressão digital do certificado que está sendo usado para autenticar no agregador é adicionada à lista usando o **Set-SilAggregator** **– AddCertificateThumbprint** cmdlet.
     - Se estiver usando certificados corporativos, verifique se o servidor com o SIL habilitado está ingressado no domínio para o qual o certificado foi criado ou se ele é, de outro modo, verificável com uma autoridade raiz. Se um certificado não for confiável no computador local que tenta encaminhar/enviar por push os dados para um Agregador, essa ação falhará com um erro.

     Se todos os itens acima foram verificados e verificados, mas o problema persistir:

     - Verifique se o certificado usado para instalar o agregador do SIL está íntegro e corresponde ao nome do servidor agregador do SIL em si. Além disso, se certificados corporativos forem usados para instalar o Agregador do SIL, o Agregador talvez precise ser ingressado no domínio em que o certificado foi criado (essas etapas são desnecessárias se outras máquinas estão encaminhando com êxito para o mesmo Agregador do SIL).

     -  Por fim, você pode verificar o seguinte local para arquivos do SIL armazenados em cache no servidor que está tentando encaminhar/enviar por push **\Windows\System32\Logfiles\SIL**. Se **SilLogging** foi iniciado e está em execução por mais de uma hora, ou **Publish-SilData** foi executado recentemente, e não existem arquivos nesse diretório, que foi o registro em log para o agregador concluída com êxito.

Se não houver nenhum erro e nenhuma saída no console, em seguida, os dados por push/publicar a partir do nó de final do Windows Server para o agregador do SIL via HTTPS foi bem-sucedida. Siga o caminho de logon de encaminhamento, os dados para o agregador do SIL como um administrador e examinar os arquivos de dados que foram recebidos. Vá para **arquivos de programas (x86)** &gt; **agregador do SIL Microsoft** &gt; diretório SILA. Você pode inspecionar os arquivos de dados que chegam em tempo real.

>[!NOTE] 
>Mais de um arquivo de dados ter sido transferido com o **Publish-SilData** cmdlet. SIL no nó final armazenará em cache com falha de envios por push para até 30 dias. Sobre o próximo envio por push com êxito todos os arquivos de dados serão vá para o agregador para processamento. Dessa forma, um agregador do SIL recentemente definir poderia mostrar dados de um nó final bem antes da sua própria instalação.

>[!NOTE] 
>Há regras que SILA segue durante o processamento de arquivos de dados no diretório SILA que só são relevantes em situações de baixo tráfego. Alto tráfego sempre irá disparar o processamento em tempo real. O comportamento padrão é que processamento começará a depois que chegam de 100 arquivos no diretório, ou após 15 minutos. Ao solucionar problemas de ponta a ponta em um ambiente pequeno, geralmente é necessário esperar 15 minutos.

Depois que esses arquivos são processados, você verá os dados no banco de dados.
