---
title: Solucionar problemas do Log de Inventário de Software
description: Descreve como resolver problemas comuns de implantação de log de inventário de software.
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: f6ad4686ce5afb86ce60ec0313bd675f8a9a1f4a
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408795"
---
# <a name="troubleshoot-software-inventory-logging"></a>Solucionar problemas do Log de Inventário de Software

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

## <a name="understanding-sil"></a>Entendendo o SIL

Antes de iniciar a solução de problemas do SIL, você deve ter uma boa compreensão de seus componentes e de como ele funciona. Os vídeos a seguir fornecem uma visão geral do agregador SIL e SIL e como usá-los para encaminhar e relatar dados de inventário:

1. [Uma introdução ao log de inventário de software (SIL) (10:57)](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Log de inventário de software: Configurando o agregador SIL (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Log de inventário de software: Habilitando o encaminhamento de SIL (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>Como funciona o fluxo de dados do SIL

A estrutura SIL tem dois componentes principais e dois canais de comunicação. O fluxo de dados em ambos os canais, e entre ambos os componentes, é necessário para uma implantação bem-sucedida do SIL (isso pressupõe um ambiente virtualizado, ou de nuvem, apenas ambientes físicos que precisam de um dos canais de comunicação). Você precisará entender os componentes e o fluxo de dados do SIL para implantá-lo corretamente. Depois de assistir aos vídeos de visão geral acima, você terá visto o diagrama arquitetônico que ilustra os componentes e o fluxo de dados em ambos os canais. Setas em laranja indicam consultas remotas sobre WinRM, setas tracejadas verdes indicam postagens HTTPS para o agregador SIL de SIL em cada nó WS-end:

![Diagrama do SIL Framework](../media/software-inventory-logging/image1.png)

Se você encontrar um problema com o SIL, é provável que ele esteja relacionado a uma interrupção no fluxo de dados nos canais e entre os componentes. A seguir estão os problemas mais comuns relacionados ao fluxo de dados, seguidos na próxima seção pelas etapas de solução de problemas para resolver cada um dos três problemas:

-   **Problema de fluxo de dados 1** – **não há dados no relatório ao usar o cmdlet Publish-SilReport** (ou geralmente dados ausentes).

-   **Problema de fluxo de dados 2** – **muitos servidores em host desconhecido** no relatório.

-   **Problema de fluxo de dados 3** – **muitas VMs em hosts físicos listadas como so desconhecido** no relatório e/ou um erro produzido ao usar **Publish-SilData** em servidores Windows que executam o Sil.

## <a name="troubleshooting-data-flow-issues"></a>Solucionando problemas de fluxo de dados

Antes de começar, você precisará saber quanto tempo atrás o agregador SIL começou com o cmdlet **Start-SilAggregator** .

>[!IMPORTANT]
>Não haverá dados no relatório até que o cubo de dados SQL seja processado em 3AM hora do sistema local. Não continue com as etapas de solução de problemas até que o cubo processe dados.

Se você estiver solucionando problemas de dados no relatório (ou ausente do relatório) que seja mais recente do que a última vez que o cubo foi processado ou antes que o cubo tenha sido processado (para uma nova instalação), siga estas etapas para processar o cubo de dados SQL em tempo real:

1. Faça logon como administrador de SQL Server e execute o **SSMS** em um prompt de comando.
2. Não é possível conectar-se ao Mecanismo do Banco de Dados.
3. Expanda **SQL Server Agent** e, em seguida, expanda **trabalhos**.
4. Clique com o botão direito do mouse em **SILStagingRefresh** e selecione **Iniciar trabalho na etapa**.
5. Clique em **Iniciar** e aguarde a conclusão da barra de progresso de atualização.
6. Abra o PowerShell como administrador e execute o cmdlet **Publish-silreport-OpenReport** .

Se ainda não houver dados no relatório, continue com a solução de problemas dos três problemas de fluxo de dados.

### <a name="data-flow-issue-1"></a>Problema de fluxo de dados 1

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>Não há dados no relatório ao usar o cmdlet Publish-SilReport (ou os dados geralmente estão ausentes)

Se os dados estiverem ausentes, provavelmente porque o cubo de dados SQL ainda não foi processado. Se ele tiver processado recentemente e você acreditar que os dados ausentes devem ter chegado ao agregador antes do processamento do cubo, siga o caminho dos dados na ordem inversa. Escolha um host exclusivo e uma VM exclusiva para solucionar problemas. O caminho de dados em ordem inversa seria o **Sila Report** &lt; **Sila Database** &lt; **Sila o diretório local** &lt; **Remote host físico** ou **WS VM running Sil agente/tarefa**.

#### <a name="check-to-see-if-data-is-in-the-database"></a>Verifique se os dados estão no banco de dado

Há duas maneiras de verificar os dados: **PowerShell** ou **SSMS**.

>[!Important]
>Se o cubo tiver processado pelo menos uma vez desde que o SILA inseriu dados no banco de dado, esses dados deverão ser refletidos no relatório. Se não houver nenhum dado no banco de dados, a sondagem dos hosts físicos está falhando ou nada está sendo recebido por HTTPS ou ambos.

 #### <a name="powershell"></a>PowerShell

1. Abra o PowerShell como administrador e execute o cmdlet **Get-silvmhost** e execute **Get-silaggregator**.

    >[!NOTE]
    >A saída de **Get-silaggregator** sempre irá imitar a **guia detalhes do Windows Server** do relatório do Excel. Não espere um resultado diferente.

2. Executar **Get-silvmhost**
   - Se não houver nenhum host listado, adicione hosts usando o cmdlet **Add-silvmhost** .
   - Se os hosts estiverem listados como **desconhecidos**, vá para o problema 2.
   d-se os hosts estiverem listados, mas não houver nenhum **DateTime** na coluna **sondagem recente** , vá para o **problema 2** abaixo.

**Outros comandos relacionados**

**Get-SilAggregator-ComputerName &lt; FQDN de um servidor conhecido enviando dados &gt; por push**: isso produzirá informações do Database sobre um computador (VM) mesmo antes do processamento do cubo. Portanto, esse cmdlet pode ser usado para verificar os dados no banco de dado para um Windows Server enviando dados SIL por HTTPS, antes ou sem, o processo de cubo em 3AM (ou se você não tiver atualizado o cubo em tempo real, conforme descrito no início desta seção).

**Get-SilAggregator-VmHostName &lt; FQDN de um host físico sondado em que há um valor na coluna de sondagem recente ao usar o cmdlet &gt; Get-SilVmHost**: isso produzirá informações do banco de dados sobre um host físico, mesmo antes de o cubo ser processado.

#### <a name="ssms"></a>SSMS

n**verificar se há dados de hosts sendo sondados:**

1. Abra o **SSMS** e conecte-se ao **mecanismo de banco de dados**.
2. Expanda **bancos**de dados, expanda o **SoftwareInventoryLogging** Database, expanda **tabelas**, clique com o botão direito do mouse na tabela **HostInfo** e selecione as 1000 principais linhas.

    Se houver dados para um ou mais hosts na tabela, a sondagem desse (s) host (es) foi bem-sucedida pelo menos uma vez.

   **Verifique se há dados de VMs ou servidores autônomos que tenham enviado dados por HTTPS:**

3. Abra o **SSMS** e conecte-se ao **mecanismo de banco de dados**.
   a2. Expanda **bancos**de dados, expanda o banco de **SoftwareInventoryLogging** , expanda **tabelas**, clique com o botão direito do mouse na tabela **VMInfo** e selecione as 1000 linhas superiores.

    >[!NOTE]
    >Cada linha para uma VM exclusiva representará um arquivo **bmil** processado enviado com êxito por HTTPS e processado pelo agregador Sil. Os arquivos Bmil são arquivos proprietários usados pelo SIL, um é criado cada uma das instâncias de cada SIL Observe que isso só é necessário quando SIL e SILA são usados em ambientes virtuais. Caso contrário, somente o tráfego HTTPS será necessário/esperado).

   Todos os dados no banco de dado devem ser refletidos nos relatórios do SIL depois que o cubo for processado.

### <a name="data-flow-issue-2"></a>Problema de fluxo de dados 2
#### <a name="too-many-servers-under-unknown-host"></a>Muitos servidores em um host desconhecido

Isso provavelmente ocorrerá em ambientes virtuais quando o agregador SIL não sondar com êxito os hosts físicos que estão hospedando as máquinas virtuais.

1. Abra o **PowerShell** como administrador e execute o cmdlet **Get-silvmhost** .

    -   Se os hosts estiverem listados como **desconhecidos**, o cmdlet **Add-silvmhost** não funcionou corretamente – geralmente devido a credenciais incorretas adicionadas para acesso a esses hosts (portanto, desconhecido). Mas se as credenciais forem verificadas, isso pode significar que a detecção automática de **HostType** e de **hipervisortype** no cmdlet **Add-silvmhost** não pôde reconhecer a plataforma. Há etapas de solução de problemas avançadas disponíveis para essas situações, mas não são abordadas aqui (verifique os canais do agregador visualizador SIL).

    -   Se os hosts estiverem listados e o **HostType** e o **hipervisortype** estiverem listados com valores que não são **desconhecidos**, ou seja, Windows e HyperV, ou Ubuntu e Xen, etc., mas não há nenhum **DateTime** na coluna de **pesquisa recente** , então a sondagem ainda não ocorreu com êxito.

        -   Você precisará aguardar uma hora depois de adicionar o host para que a sondagem ocorra (supondo que esse intervalo esteja definido como padrão – pode ser verificado usando o cmdlet **Get-silaggregator** ).

        -   Se tiver sido uma hora desde que o host foi adicionado, verifique se a tarefa de sondagem está em execução: em **Agendador de tarefas**, selecione **agregador de log de inventário de software** no **Microsoft** &gt; **Windows** e verifique o histórico da tarefa.

    -   Se um host estiver listado, mas não houver nenhum valor para **RecentPoll**, **HostType**ou **hipervisortype**, isso poderá ser amplamente ignorado. Isso só ocorrerá em ambientes do HyperV. Esses dados são realmente provenientes da VM do Windows Server, identificando o host físico que está sendo executado via HTTPS. Isso pode ser útil para identificar uma VM específica que está sendo relatada, mas requer a mineração do banco de dados usando o cmdlet **Get-SilAggregatorData** .

Depois que os hosts forem sondados corretamente, você poderá ver os dados para esses hosts físicos no banco de dados SILA em que há um **DateTime** na **pesquisa recente**. A seção Issue 1 acima fornece etapas para recuperar esses dados.

### <a name="data-flow-issue-3"></a>Problema de fluxo de dados 3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Muitos hosts físicos com VMs listadas como so desconhecido

1. Escolha um nó de extremidade do Windows Server (VM) que você sabe que está em um desses hosts, faça logon como administrador.
2. Abra o PowerShell como administrador.
3. Verifique se **SilLogging** está em execução usando o cmdlet **Get-SilLogging** .
   - Se estiver em execução, tente enviar manualmente os dados de SIL usando **Publish-SilData**.

   - Se houver um erro:
     - Verifique se **targetUri** tem **https://** na entrada.
     - Garantir que todos os pré-requisitos sejam atendidos
     - Verifique se todas as atualizações necessárias para o Windows Server estão instaladas (consulte pré-requisitos para SIL). Uma maneira rápida de verificar (somente no WS 2012 R2) é procurar por eles usando o seguinte cmdlet: **Get-SilWindowsUpdate \* 3060, \* 3000**
     - Verifique se o certificado que está sendo usado para autenticar com o agregador está instalado no armazenamento correto no servidor local para ser inventariado com **SilLogging**.
     - No agregador SIL, certifique-se de que a impressão digital do certificado que está sendo usada para autenticar com o agregador seja adicionada à lista usando o cmdlet **set-SilAggregator** **– AddCertificateThumbprint** .
     - Se estiver usando certificados corporativos, verifique se o servidor com o SIL habilitado está ingressado no domínio para o qual o certificado foi criado ou se ele é, de outro modo, verificável com uma autoridade raiz. Se um certificado não for confiável no computador local que tenta encaminhar/enviar por push os dados para um Agregador, essa ação falhará com um erro.

     Se todas as versões acima tiverem sido verificadas e verificadas, mas o problema persistir:

     - Verifique se o certificado usado para instalar o agregador SIL está íntegro e corresponde ao nome do próprio servidor do agregador SIL. Além disso, se certificados corporativos forem usados para instalar o Agregador do SIL, o Agregador talvez precise ser ingressado no domínio em que o certificado foi criado (essas etapas são desnecessárias se outras máquinas estão encaminhando com êxito para o mesmo Agregador do SIL).

     -  Por fim, você pode verificar o seguinte local para arquivos SIL armazenados em cache no servidor que está tentando encaminhar/enviar por push, **\Windows\System32\Logfiles\SIL**. Se **SilLogging** tiver começado e estiver em execução por mais de uma hora, ou o **Publish-SilData** tiver sido executado recentemente, e não houver nenhum arquivo nesse diretório, o logon no agregador terá sido bem-sucedido.

Se não houver nenhum erro e nenhuma saída no console, os dados enviados/publicados do nó final do Windows Server para o agregador SIL por HTTPS foram bem-sucedidos. Para seguir o caminho dos dados em direção, faça logon no agregador SIL como administrador e examine os arquivos de dados que chegaram. Acesse o diretório **arquivos de programas (x86)** &gt; **Microsoft Sil Aggregation** &gt; Sila. Você pode ver os arquivos de dados chegando em tempo real.

>[!NOTE]
>Mais de um arquivo de dados pode ter sido transferido com o cmdlet **Publish-SilData** . SIL no nó final ocorrerá um erro de envio por push de cache por até 30 dias. No próximo Push bem-sucedido, todos os arquivos de dados vão para o agregador para processamento. Dessa forma, um novo agregador SIL poderia mostrar dados de um nó final bem antes de sua própria configuração.

>[!NOTE]
>Há regras que o SILA segue ao processar arquivos de dados no diretório SILA que são relevantes apenas em situações de tráfego baixo. O tráfego alto sempre irá disparar o processamento em tempo real. O comportamento padrão é que o processamento começará após a chegada de 100 arquivos no diretório ou após 15 minutos. Ao solucionar problemas de ponta a ponta em um ambiente pequeno, geralmente é necessário aguardar 15 minutos.

Depois que esses arquivos forem processados, você verá os dados no banco de dado.
