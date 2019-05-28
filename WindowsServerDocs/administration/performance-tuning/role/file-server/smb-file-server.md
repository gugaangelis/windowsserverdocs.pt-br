---
title: Ajuste de desempenho para servidores de arquivos SMB
description: Ajuste de desempenho para servidores de arquivos SMB
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 337716792a4bb3cf730b723df3abe1029631426b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222499"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Ajuste de desempenho para servidores de arquivos SMB

## <a name="smb-configuration-considerations"></a>Considerações de configuração do SMB
Não habilite todos os serviços ou recursos que seu servidor de arquivos e os clientes não exigem. Eles podem incluir assinatura SMB, armazenamento em cache do lado do cliente, minifiltros de sistema de arquivos, serviço de pesquisa, tarefas agendadas, criptografia de NTFS, a compactação NTFS, IPSEC, filtros de firewall, Teredo e SMB criptografia.

Certifique-se de que o BIOS e modos de gerenciamento de energia do sistema operacional são definidos conforme o necessário, que pode incluir o modo de alto desempenho ou alterado o estado-C. Certifique-se de que o armazenamento mais resiliente, mais rápido e mais recente e drivers de dispositivo de rede estão instalados.

Copiando arquivos é uma operação comum executada em um servidor de arquivos. Windows Server possui vários utilitários de cópia de arquivo internos que podem ser executados usando um prompt de comando. Robocopy é recomendado. Introduzido no Windows Server 2008 R2, o **/mt** opção do Robocopy pode melhorar significativamente a velocidade em transferências de arquivos remoto usando vários threads ao copiar vários arquivos pequenos. Também é recomendável usar o **/log** opção para reduzir a saída do console, redirecionando os logs para um dispositivo NUL ou para um arquivo. Quando você usa o Xcopy, é recomendável adicionar o **/q** e **/k** opções para seus parâmetros existentes. A opção anterior reduz a CPU sobrecarga, reduzindo a saída do console e a última opção reduz o tráfego de rede.

## <a name="smb-performance-tuning"></a>Ajuste de desempenho do SMB


Desempenho do servidor de arquivos e tunings disponíveis dependem o protocolo SMB será negociado entre cada cliente e o servidor e os recursos de servidor de arquivo implantado. A versão mais recente do protocolo disponível no momento é SMB 3.1.1 no Windows Server 2016 e Windows 10. Você pode verificar qual versão do SMB está em uso na sua rede usando o Windows PowerShell **Get-SMBConnection** nos clientes e **Get-SMBSession | FL** nos servidores.

### <a name="smb-30-protocol-family"></a>Família de protocolo SMB 3.0

SMB 3.0 foi introduzido no Windows Server 2012 e ainda mais aprimorada no Windows Server 2012 R2 (SMB 3.02) e Windows Server 2016 (SMB 3.1.1). Esta versão introduziu as tecnologias que podem melhorar significativamente o desempenho e a disponibilidade do servidor de arquivos. Para obter mais informações, consulte [SMB no Windows Server 2012 e 2012 R2 2012](https://aka.ms/smb3plus) e [Novidades no SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB Direct

O SMB Direct introduziu a capacidade de usar interfaces de rede RDMA para alta taxa de transferência com baixa latência e baixa utilização da CPU.

Sempre que o SMB detecta uma rede compatíveis com RDMA, ele tenta automaticamente usar a capacidade RDMA. No entanto, se por algum motivo, o cliente SMB falhar para se conectar usando o caminho RDMA, ele será simplesmente continuar a usar conexões TCP/IP em vez disso. Todas as interfaces RDMA são compatíveis com o SMB Direct são necessárias para implementar uma pilha de TCP/IP e o SMB Multichannel está ciente disso.

O SMB Direct não é necessária em todas as configurações de SMB, mas ' s sempre recomendado para quem deseja latência mais baixa e menor utilização da CPU.

Para obter mais informações sobre o SMB Direct, consulte [melhorar o desempenho de um servidor de arquivos com o SMB Direct](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB Multichannel

O SMB Multichannel permite que os servidores de arquivo usar várias conexões de rede simultaneamente e fornece maior taxa de transferência.

Para obter mais informações sobre o SMB Multichannel, consulte [implantar SMB Multichannel](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>Expansão SMB

Expansão SMB permite que o SMB 3.0 em uma configuração de cluster para mostrar um compartilhamento em todos os nós de um cluster. Essa configuração ativo/ativo possibilita a clusters de servidor de arquivos de dimensionamento ainda mais, sem uma configuração complexa com vários volumes, compartilhamentos e recursos de cluster. A largura de banda máxima de compartilhamento é a largura de banda total de todos os nós de cluster de servidor de arquivos. A largura de banda total não é mais limitada pela largura de banda de um único nó de cluster, mas em vez disso depende da capacidade do sistema de armazenamento de backup. Você pode aumentar a largura de banda total adicionando nós.

Para obter mais informações sobre o SMB Scale-Out, consulte [servidor de arquivos de escalabilidade horizontal para visão geral de dados de aplicativo](https://technet.microsoft.com/library/hh831349.aspx) e a postagem de blog [escalar horizontalmente ou não a expansão, eis a questão](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Contadores de desempenho do SMB 3.0

Os seguintes contadores de desempenho do SMB foram introduzidos no Windows Server 2012, e eles são considerados um conjunto básico de contadores ao monitorar o uso de recursos do SMB 2 e versões superiores. Os contadores de desempenho de log até um local e o log de contador de desempenho bruto (. blg). Ela é menos dispendiosa coletar todas as instâncias usando o caractere curinga (\*) e, em seguida, extraia instâncias específicas durante o pós-processamento usando Relog.exe.

-   **Compartilhamentos de cliente SMB**

    Esses contadores exibem informações sobre compartilhamentos de arquivos no servidor que estão sendo acessados por um cliente que está usando o SMB 2.0 ou versões superiores.

    Se você ' re familiarizados com os contadores de disco regulares no Windows, você observará uma semelhança determinada. Que ' s não por acidente. Os contadores de desempenho de compartilhamentos de cliente SMB foram projetados para coincidir exatamente com os contadores de disco. Dessa forma, você pode reutilizar facilmente todas as orientações sobre ajuste de desempenho de disco de aplicativo você tem atualmente. Para obter mais informações sobre o mapeamento de contador, consulte [por compartilhamento de blog de contadores de desempenho de cliente](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).

-   **Compartilhamentos do servidor SMB**

    Esses contadores exibem informações sobre o SMB 2.0 ou superiores compartilhamentos de arquivos no servidor.

-   **Sessões do servidor SMB**

    Esses contadores exibem informações sobre sessões do servidor SMB que estão usando o SMB 2.0 ou superior.

    Ativar os contadores no lado do servidor (compartilhamentos de servidor ou sessões do servidor) pode ter impacto significativo no desempenho para cargas de trabalho de e/s alta.

-   **Filtro de chave de retomada**

    Esses contadores exibem informações sobre o filtro de chave de retomada.

-   **Conexão direta do SMB**

    Esses contadores medem diferentes aspectos da atividade de conexão. Um computador pode ter várias conexões SMB Direct. Os contadores de Conexão direta do SMB representam cada conexão como um par de endereços IP e portas, onde o primeiro endereço IP e porta representam o ponto de extremidade local da conexão, e o segundo endereço IP e porta representam o ponto de extremidade remoto da conexão.

-   **Disco físico, SMB, as relações de contadores de desempenho de FS de CSV**

    Para obter mais informações sobre como os contadores de (sistema de arquivos) do disco físico, SMB e CSV FS estão relacionados, consulte a postagem do blog a seguir: [Contadores de desempenho do Volume compartilhado do cluster](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Parâmetros de ajuste para servidores de arquivos SMB


O seguinte REG\_configurações do Registro DWORD podem afetar o desempenho dos servidores de arquivos SMB:

-   **Smb2CreditsMin** e **Smb2CreditsMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
    ```

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
    ```

    Os padrões são 512 e 8192, respectivamente. Esses parâmetros permitem que o servidor aumentar a simultaneidade da operação de cliente dinamicamente dentro dos limites especificados. Alguns clientes podem alcançar maior taxa de transferência mais alta limites de simultaneidade, por exemplo, copiando os arquivos em links de alta largura de banda, alta latência.
    
    >[!TIP]
    > Antes do Windows 10 e Windows Server 2016, o número de créditos concedidas ao cliente variam dinamicamente Smb2CreditsMin e Smb2CreditsMax com base em um algoritmo que tentou a determinar o número ideal de créditos para conceder com base na latência de rede e o uso de crédito. No Windows 10 e Windows Server 2016, o servidor SMB foi alterado para conceder incondicionalmente créditos mediante solicitação até o número máximo configurado de créditos. Como parte dessa alteração, o crédito de mecanismo, o que reduz o tamanho da janela de crédito do cada conexão quando o servidor estiver sob pressão de memória, a limitação foi removido. Evento de pouca memória do kernel que disparou a limitação é sinalizado somente quando o servidor está muito baixo de memória (< alguns MB) virou inútil. Uma vez que o servidor não diminui windows crédito a configuração de Smb2CreditsMin não é mais necessária e agora é ignorada.

    > Você pode monitorar compartilhamentos de cliente SMB\\crédito fica parada/seg. para ver se há problemas com créditos.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    O padrão é 0, o que significa que nenhum thread de trabalho críticas de kernel adicionais é adicionado. Esse valor afeta o número de threads que usa o cache do sistema de arquivo para solicitações de leitura antecipada e write-behind. Aumentar esse valor pode permitir que mais e/s na fila no subsistema de armazenamento, e ele pode melhorar o desempenho de e/s, especialmente em sistemas com vários processadores lógicos e hardware de armazenamento avançados.

    >[!TIP]
    > O valor precisa ser aumentado se a quantidade de Gerenciador de cache sujos dados (contador de desempenho de Cache\\páginas sujas) está crescendo consumir uma grande parte (mais de % ~ 25) de memória e/SS de leitura ou se o sistema está fazendo muitas síncrona.

-   **MaxThreadsPerQueue**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
    ```

    O padrão é 20. Aumentar esse valor aumenta o número de threads que o servidor de arquivos pode usar para atender a solicitações simultâneas. Quando um grande número de conexões ativas precisa ser atendidos e recursos de hardware, como largura de banda de armazenamento, são suficientes, aumentando o valor pode melhorar os tempos de resposta, desempenho e escalabilidade do servidor.

    >[!TIP]
    > Uma indicação de que o valor precisa ser aumentado é se as filas de trabalho SMB2 estão crescendo muito grandes (contador de desempenho ' filas de trabalho do servidor\\comprimento da fila\\SMB2 sem bloqueio \*' estiver consistentemente acima de ~ 100).

    >[!Note]
    >No Windows 10 e Windows Server 2016, MaxThreadsPerQueue não está disponível. O número de threads para um pool de threads será "20 * o número de processadores em um nó NUMA".  

-   **AsynchronousCredits**

    ``` 
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
    ```

    O padrão é 512. Este parâmetro limita o número de simultâneos comandos assíncronos de SMB que são permitidos em uma única conexão. Alguns casos (por exemplo, quando há um servidor front-end com um servidor do IIS de back-end) exigem uma grande quantidade de simultaneidade (para solicitações de notificação de alteração de arquivo em particular). O valor desta entrada pode ser aumentado para dar suporte a esses casos.

### <a name="smb-server-tuning-example"></a>Exemplo de ajuste de servidor SMB

As configurações a seguir podem otimizar um computador para o desempenho do servidor de arquivos em muitos casos. As configurações não são ideais nem adequadas em todos os computadores. Você deve avaliar o impacto de configurações individuais antes de aplicá-las.

| Parâmetro                       | Valor | Padrão |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Contadores de monitor de desempenho de cliente SMB

Para obter mais informações sobre contadores de cliente SMB, consulte [dica do servidor de arquivo do Windows Server 2012: Novos contadores de desempenho por compartilhamento SMB cliente fornecem uma ótima visão](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).
