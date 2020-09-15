---
title: Ajuste de desempenho para servidores de arquivos SMB
description: Ajuste de desempenho para servidores de arquivos SMB
ms.topic: article
author: phstee
ms.author: nedpyle
ms.date: 4/14/2017
ms.openlocfilehash: 2515f400f746c5e256a168d191efa842d4ba50fd
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077203"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Ajuste de desempenho para servidores de arquivos SMB

## <a name="smb-configuration-considerations"></a>Considerações sobre configuração de SMB
Não habilite nenhum serviço ou recurso que o servidor de arquivos e os clientes não precisam. Eles podem incluir assinatura SMB, cache do lado do cliente, mini filtros do sistema de arquivos, serviço de pesquisa, tarefas agendadas, criptografia NTFS, compactação NTFS, IPSEC, filtros de firewall, Teredo e criptografia SMB.

Verifique se os modos de gerenciamento de energia do BIOS e do sistema operacional estão definidos conforme necessário, o que pode incluir o modo de alto desempenho ou o estado C alterado. Verifique se os drivers de dispositivo de rede e armazenamento mais recentes, mais resilientes e mais rápidos estão instalados.

Copiar arquivos é uma operação comum executada em um servidor de arquivos. O Windows Server tem vários utilitários de cópia de arquivos internos que você pode executar usando um prompt de comando. O Robocopy é recomendado. Introduzido no Windows Server 2008 R2, a opção **/MT** do Robocopy pode melhorar significativamente a velocidade em transferências de arquivos remotos usando vários threads ao copiar vários arquivos pequenos. Também é recomendável usar a opção **/log** para reduzir a saída do console redirecionando os logs para um dispositivo nul ou para um arquivo. Ao usar o xcopy, é recomendável adicionar as opções **/q** e **/k** aos parâmetros existentes. A opção anterior reduz a sobrecarga da CPU, reduzindo a saída do console e a última reduz o tráfego de rede.

## <a name="smb-performance-tuning"></a>Ajuste de desempenho de SMB


O desempenho do servidor de arquivos e os ajustes disponíveis dependem do protocolo SMB negociado entre cada cliente e o servidor e os recursos do servidor de arquivos implantado. A versão de protocolo mais alta disponível atualmente é SMB 3.1.1 no Windows Server 2016 e Windows 10. Você pode verificar qual versão do SMB está em uso em sua rede usando o Windows PowerShell **Get-SMBConnection** em clientes e **Get-SMBSession | FL** nos servidores.

### <a name="smb-30-protocol-family"></a>Família de protocolos SMB 3,0

O SMB 3,0 foi introduzido no Windows Server 2012 e aprimorado ainda mais no Windows Server 2012 R2 (SMB 3, 2) e no Windows Server 2016 (SMB 3.1.1). Essa versão introduziu tecnologias que podem melhorar significativamente o desempenho e a disponibilidade do servidor de arquivos. Para obter mais informações, consulte [SMB no Windows Server 2012 e 2012 R2 2012](https://aka.ms/smb3plus) e [o que há de novo no SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB Direct

O SMB Direct introduziu a capacidade de usar interfaces de rede RDMA para alta taxa de transferência com baixa latência e baixa utilização da CPU.

Sempre que o SMB detecta uma rede compatível com RDMA, ele tenta automaticamente usar o recurso RDMA. No entanto, se por algum motivo o cliente SMB não conseguir se conectar usando o caminho RDMA, ele simplesmente continuará a usar conexões TCP/IP. Todas as interfaces RDMA compatíveis com SMB Direct também são necessárias para implementar uma pilha TCP/IP e o SMB multicanal está ciente disso.

O SMB Direct não é necessário em nenhuma configuração de SMB, mas é sempre recomendado para aqueles que desejam menor latência e menor utilização da CPU.

Para obter mais informações sobre o SMB Direct, consulte [melhorar o desempenho de um servidor de arquivos com o SMB Direct](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB Multichannel

O SMB Multichannel permite que os servidores de arquivos usem várias conexões de rede simultaneamente e fornece maior taxa de transferência.

Para obter mais informações sobre o SMB multicanal, consulte [implantar o SMB multicanal](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>Expansão do SMB

A expansão do SMB permite que o SMB 3,0 em uma configuração de cluster mostre um compartilhamento em todos os nós de um cluster. Essa configuração ativa/ativa torna possível dimensionar os clusters de servidor de arquivos ainda mais, sem uma configuração complexa com vários volumes, compartilhamentos e recursos de cluster. A largura de banda máxima de compartilhamento é a largura de banda total de todos os nós de cluster de servidor de arquivos. A largura de banda total não é mais limitada pela largura de banda de um único nó de cluster, mas depende da capacidade do sistema de armazenamento de backup. Você pode aumentar a largura de banda total adicionando nós.

Para obter mais informações sobre a expansão do SMB, consulte a [visão geral de servidor de arquivos de escalabilidade horizontal para dados de aplicativos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831349(v=ws.11)) e a postagem do blog [para escalar horizontalmente ou não para escalar horizontalmente, essa é a questão](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Contadores de desempenho para SMB 3,0

Os contadores de desempenho SMB a seguir foram introduzidos no Windows Server 2012 e são considerados um conjunto base de contadores quando você monitora o uso de recursos do SMB 2 e versões posteriores. Registre os contadores de desempenho em um log de contador de desempenho local e bruto (. blg). É mais barato coletar todas as instâncias usando o caractere curinga ( \* ) e, em seguida, extrair instâncias específicas durante o pós-processamento usando Relog.exe.

-   **Compartilhamentos de cliente SMB**

    Esses contadores exibem informações sobre compartilhamentos de arquivos no servidor que estão sendo acessados por um cliente que está usando o SMB 2,0 ou versões posteriores.

    Se você estiver familiarizado com os contadores de disco comuns no Windows, poderá observar uma certa semelhança. Isso não é por acidente. Os contadores de desempenho do SMB Client compartilhamentos foram projetados para corresponder exatamente aos contadores de disco. Dessa forma, você pode facilmente reutilizar qualquer orientação sobre o ajuste de desempenho de disco de aplicativo que você tem atualmente. Para obter mais informações sobre mapeamento de contador, consulte [blog de contadores de desempenho de cliente por compartilhamento](/archive/blogs/josebda/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight).

-   **Compartilhamentos de servidor SMB**

    Esses contadores exibem informações sobre os compartilhamentos de arquivos SMB 2,0 ou superior no servidor.

-   **Sessões do servidor SMB**

    Esses contadores exibem informações sobre sessões de servidor SMB que estão usando SMB 2,0 ou superior.

    Ativar os contadores no lado do servidor (compartilhamentos de servidor ou sessões de servidor) pode ter um impacto significativo no desempenho para cargas de trabalho de alta e/s.

-   **Retomar filtro de chave**

    Esses contadores exibem informações sobre o filtro de chave de retomada.

-   **Conexão direta do SMB**

    Esses contadores medem diferentes aspectos da atividade de conexão. Um computador pode ter várias conexões SMB diretas. Os contadores de conexão SMB Direct representam cada conexão como um par de endereços IP e portas, em que o primeiro endereço IP e a porta representam o ponto de extremidade local da conexão, e o segundo endereço IP e a porta representam o ponto de extremidade remoto da conexão.

-   **Relações de contadores de desempenho de disco físico, SMB e CSV FS**

    Para obter mais informações sobre como os contadores disco físico, SMB e CSV FS (sistema de arquivos) estão relacionados, consulte a seguinte postagem no blog: [volume compartilhado clusterizado contadores de desempenho](https://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Parâmetros de ajuste para servidores de arquivos SMB


As seguintes \_ configurações de registro do reg DWORD podem afetar o desempenho dos servidores de arquivos SMB:

- **Smb2CreditsMin** e **Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  Os padrões são 512 e 8192, respectivamente. Esses parâmetros permitem que o servidor controle a simultaneidade da operação do cliente dinamicamente dentro dos limites especificados. Alguns clientes podem alcançar uma taxa de transferência maior com limites de simultaneidade mais altos, por exemplo, copiar arquivos por links de alta largura de banda e alta latência.

  > [!TIP]
  > Antes do Windows 10 e do Windows Server 2016, o número de créditos concedidos ao cliente variavam dinamicamente entre Smb2CreditsMin e Smb2CreditsMax com base em um algoritmo que tentava determinar o número ideal de créditos a serem concedidos com base na latência de rede e no uso de crédito. No Windows 10 e no Windows Server 2016, o servidor SMB foi alterado para conceder créditos incondicionalmente mediante solicitação até o número máximo configurado de créditos. Como parte dessa alteração, o mecanismo de limitação de crédito, que reduz o tamanho da janela de crédito de cada conexão quando o servidor está sob pressão de memória, foi removido. O evento de memória insuficiente do kernel que disparou a limitação só é sinalizado quando o servidor está tão baixo na memória (< alguns MB) para ser inútil. Como o servidor não reduz mais as janelas de crédito, a configuração Smb2CreditsMin não é mais necessária e agora é ignorada.
  >
  > Você pode monitorar \\ as interrupções de crédito de compartilhamentos de cliente SMB/s para ver se há problemas com créditos.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    O padrão é 0, o que significa que nenhum thread de trabalho de kernel crítico adicional é adicionado. Esse valor afeta o número de threads que o cache do sistema de arquivos usa para solicitações Read-Ahead e write-behind. Aumentar esse valor pode permitir uma e/s mais enfileirada no subsistema de armazenamento, e pode melhorar O desempenho de e/s, especialmente em sistemas com muitos processadores lógicos e hardware de armazenamento avançado.

    >[!TIP]
    > Talvez seja necessário aumentar o valor se a quantidade de dados sujos do Gerenciador de cache (páginas sujas no cache do contador de desempenho \\ ) estiver crescendo para consumir uma parte grande (mais de aproximadamente 25%) de memória ou se o sistema estiver fazendo muitas e/SS de leitura síncronas.

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  O padrão é 20. Aumentar esse valor gera o número de threads que o servidor de arquivos pode usar para atender a solicitações simultâneas. Quando um grande número de conexões ativas precisa ser atendido, e recursos de hardware, como largura de banda de armazenamento, são suficientes, aumentar o valor pode melhorar a escalabilidade, o desempenho e os tempos de resposta do servidor.

  >[!TIP]
  > Uma indicação de que o valor pode precisar ser aumentado é se as filas de trabalho do SMB2 estiverem crescendo muito grandes (o contador de desempenho ' servidor de filas de trabalho de filas \\ SMB2 o comprimento de fila \\ \* é consistentemente acima de 100).

  >[!Note]
  >No Windows 10 e no Windows Server 2016, o MaxThreadsPerQueue está indisponível. O número de threads para um pool de threads será "20 * o número de processadores em um nó NUMA".


- **AsynchronousCredits**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  O padrão é 512. Esse parâmetro limita o número de comandos SMB assíncronos simultâneos que são permitidos em uma única conexão. Alguns casos (como quando há um servidor front-end com um servidor IIS de back-end) exigem uma grande quantidade de simultaneidade (para solicitações de notificação de alteração de arquivo, em particular). O valor dessa entrada pode ser aumentado para dar suporte a esses casos.

### <a name="smb-server-tuning-example"></a>Exemplo de ajuste de servidor SMB

As configurações a seguir podem otimizar um computador para o desempenho do servidor de arquivos em muitos casos. As configurações não são ideais nem adequadas em todos os computadores. Você deve avaliar o impacto de configurações individuais antes de aplicá-las.

| Parâmetro                       | Valor | Padrão |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Contadores do monitor de desempenho de cliente SMB

Para obter mais informações sobre contadores de cliente SMB, consulte [dica do servidor de arquivos do Windows Server 2012: novos contadores de desempenho de cliente SMB por compartilhamento fornecem ótima percepção](/archive/blogs/josebda/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight).