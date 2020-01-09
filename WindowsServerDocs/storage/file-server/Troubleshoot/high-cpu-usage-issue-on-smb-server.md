---
title: Alto problema de uso da CPU no servidor SMB
description: Apresenta como solucionar o problema de alto uso da CPU no servidor SMB.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 8a14952ca90b6ee3bea901fd944d7c9843492fd4
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654337"
---
# <a name="high-cpu-usage-issue-on-the-smb-server"></a>Alto problema de uso da CPU no servidor SMB

Este artigo discute como solucionar o problema de alto uso da CPU no servidor SMB.

## <a name="high-cpu-usage-because-of-storage-performance-issues"></a>Alto uso da CPU devido a problemas de desempenho de armazenamento

Os problemas de desempenho de armazenamento podem causar alto uso da CPU em servidores SMB. Antes de solucionar problemas, verifique se o pacote cumulativo de atualizações mais recente está instalado no servidor SMB para eliminar problemas conhecidos no srv2. sys.

Na maioria dos casos, você observará o problema de alto uso da CPU no processo do sistema. Antes de prosseguir, use o Gerenciador de processos para certificar-se de que srv2. sys ou NTFS. sys está consumindo recursos de CPU excessivos.

### <a name="storage-area-network-san-scenario"></a>Cenário de SAN (rede de área de armazenamento)

Em níveis agregados, o desempenho geral da SAN pode parecer bom. No entanto, quando você trabalha com problemas de SMB, o tempo de resposta de solicitação individual é o mais importante.

Em geral, esse problema pode ser causado por alguma forma de enfileiramento de comandos na SAN. Você pode usar o **Perfmon** para capturar um rastreamento **do Microsoft-Windows-Storport** e analisá-lo para determinar com precisão a capacidade de resposta do armazenamento.

### <a name="disk-io-latency"></a>Latência de e/s de disco

A latência de e/s de disco é uma medida do atraso entre a hora em que uma solicitação de e/s de disco é criada e concluída.

A latência de e/s medida no perfmon inclui todo o tempo gasto nas camadas de hardware mais o tempo gasto na fila do driver de porta da Microsoft (Storport. sys para SCSI). Se os processos em execução gerarem uma fila StorPort grande, a latência medida aumentará. Isso ocorre porque a e/s deve aguardar antes de ser expedida para as camadas de hardware.

No PerfMon, os seguintes contadores mostram a latência do disco físico:

- "Objeto de desempenho de disco físico"-\> "média de disco s/contador de leitura" – mostra a latência média de leitura.

- "Objeto de desempenho de disco físico"-\> "contador Méd. de disco s/gravação" – mostra a latência média de gravação.

- "Objeto de desempenho de disco físico"-\> "média de disco s/transferência do contador" – mostra as médias combinadas para leituras e gravações.

A instância "\_total" é uma média das latências de todos os discos físicos no computador. Cada uma das outras instâncias representa um disco físico individual.

> [!NOTE]
> Não confunda esses contadores com média de transferências de disco/s. Esses são contadores completamente diferentes.

### <a name="windows-storage-stack-follows"></a>A pilha de armazenamento do Windows segue

Esta seção fornece uma breve explicação sobre a pilha de armazenamento do Windows a seguir.

Quando um aplicativo cria uma solicitação de e/s, ele envia a solicitação para o subsistema de e/s do Windows na parte superior da pilha. Em seguida, a e/s viaja até a pilha para o subsistema de "disco" do hardware. Em seguida, a resposta viaja todo o processo de backup. Durante esse processo, cada camada executa sua função e, em seguida, passa a e/s para a próxima camada.

![Fluxo de pilha](media/high-cpu-usage-issue-on-smb-server-1.png)

O PerfMon não cria dados de desempenho por segundo. Em vez disso, ele consome dados fornecidos por outros subsistemas no Windows.

Para o "objeto de desempenho de disco físico", os dados são capturados no nível "Gerenciador de partição" na pilha de armazenamento.

Quando medimos os contadores que são mencionados na seção anterior, estamos medindo o tempo gasto pela solicitação abaixo do nível "Gerenciador de partições". Quando a solicitação de e/s é enviada pelo Gerenciador de partições para baixo na pilha, estamos carimbando o carimbo de data/hora. Quando ele retorna, podemos carimbar o carimbo de data e hora novamente e calcular a diferença de tempo. A diferença de tempo é a latência.

Ao fazer isso, estamos contando com o tempo gasto nos seguintes componentes:

- Driver de classe – gerencia o tipo de dispositivo, como discos, fitas e assim por diante.

- Driver de porta – gerencia o protocolo de transporte, como SCSI, FC, SATA e assim por diante.

- Driver de miniporta de dispositivo – este é o driver de dispositivo para o adaptador de armazenamento. Ele é fornecido pelo fabricante dos dispositivos, como controlador RAID e HBA FC.

- Subsistema de disco – inclui tudo o que está abaixo do driver de miniporta de dispositivo. Isso pode ser tão simples quanto um cabo conectado a um único disco rígido físico, ou tão complexo quanto uma rede de área de armazenamento. Se o problema for determinado devido a esse componente, você poderá entrar em contato com o fornecedor do hardware para obter mais informações sobre solução de problemas.

### <a name="disk-queuing"></a>Fila de disco

Há uma quantidade limitada de e/s que um subsistema de disco pode aceitar em um determinado momento. A e/s em excesso será colocada na fila até que o disco possa aceitar e/s novamente. O tempo que a e/s gasta nas filas abaixo do nível "Gerenciador de partições" é contabilizado nas medidas de latência de disco físico do Perfmon. À medida que as filas crescem maiores e a e/s devem aguardar mais, a latência medida também aumenta.

Há várias filas abaixo do nível "Gerenciador de partições", da seguinte maneira:

- Fila do Microsoft Port Driver Queue-SCSIport ou Storport Queue

- Fila de driver de dispositivo fornecida pelo fabricante-driver de dispositivo OEM

- Filas de hardware – como fila do controlador de disco, fila de comutadores de SAN, fila do controlador de matriz e fila de disco rígido

Também levamos em conta o tempo que o disco rígido gasta atendendo ativamente à e/s e o tempo de viagem que é levado para a solicitação retornar ao nível de "Gerenciador de partições" a ser marcado como concluído.

Por fim, temos de prestar atenção especial à fila do driver de porta (para SCSI Storport. sys). O driver de porta é o último componente da Microsoft para tocar uma e/s antes de entregá-la ao driver de miniporta de dispositivo fornecido pelo fabricante.

Se o driver de miniporta de dispositivo não puder aceitar mais e/s porque sua fila ou as filas de hardware abaixo estão saturadas, começaremos a acumular e/s na fila do driver de porta. O tamanho da fila do driver de porta da Microsoft é limitado apenas pela RAM (memória do sistema) disponível e pode aumentar muito. Isso causa uma grande latência medida.

## <a name="high-cpu-caused-by-enumerating-folders"></a>Alta CPU causada pela enumeração de pastas 

Para solucionar esse problema, desabilite o recurso ABE (enumeração baseada em acesso).

Para determinar quais compartilhamentos SMB têm o ABE habilitado, execute o seguinte comando do PowerShell,

```PowerShell
Get-SmbShare | Select Name, FolderEnumerationMode
```

Irrestrito = ABE desabilitado. <br />
AccessBase = ABE habilitado.


Você pode habilitar o ABE no **Gerenciador do servidor**. Navigatie para **serviços de arquivo e armazenamento** > **compartilhamentos**, clique com o botão direito do mouse no compartilhamento, selecione **Propriedades**, vá para **configurações** e, em seguida, selecione **habilitar enumeração baseada em acesso**.

![Opções da interface do usuário](media/high-cpu-usage-issue-on-smb-server-2.png)

Além disso, você pode reduzir o **ABELevel** para um nível mais baixo (1 ou 2) para melhorar o desempenho.

Você pode verificar o desempenho do disco quando a enumeração é lenta abrindo a pasta localmente por meio de um console ou de uma sessão RDP.
