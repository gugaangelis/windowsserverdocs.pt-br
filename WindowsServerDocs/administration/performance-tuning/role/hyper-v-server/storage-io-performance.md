---
title: Desempenho do armazenamento do Hyper-V e/s
description: Considerações de desempenho de e/s de armazenamento no ajuste de desempenho do Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fedc23083914bcf97a8cde12b78c0b174143de25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831307"
---
# <a name="hyper-v-storage-io-performance"></a>Desempenho do armazenamento do Hyper-V e/s

Esta seção descreve as diferentes opções e considerações para o ajuste de desempenho de e/s em uma máquina virtual de armazenamento. O caminho de e/s de armazenamento estende da pilha de armazenamento de convidado, por meio da camada de virtualização do host, para a pilha de armazenamento do host e, em seguida, para o disco físico. A seguir está as explicações sobre como as otimizações são possíveis em cada um desses estágios.

## <a name="virtual-controllers"></a>Controladores virtuais

Hyper-V oferece três tipos de controladores virtuais: IDE, SCSI e Virtual adaptadores de barramento host (HBAs).

## <a name="ide"></a>IDE

Controladores IDE devem expõem discos IDE para a máquina virtual. O controlador IDE é emulado, e é o único controlador que está disponível para VMs convidadas executando a versão mais antiga do Windows sem os serviços de integração de máquina Virtual. E/s que é executada usando o driver de filtro do IDE que é fornecido com os serviços de integração de máquina Virtual do disco é significativamente melhor do que o disco de desempenho de e/s que é fornecido com o controlador IDE emulado. É recomendável que os discos IDE ser usado somente para discos do sistema operacional porque eles têm limitações de desempenho devido ao tamanho máximo de e/s que podem ser emitidos para esses dispositivos.

## <a name="scsi-sas-controller"></a>SCSI (controlador SAS)

Controladores SCSI expõem discos SCSI na máquina virtual, e cada controlador virtual SCSI pode dar suporte a até 64 dispositivos. Para otimizar o desempenho, é recomendável que você anexe vários discos a um único controlador de SCSI virtual e cria controladores adicionais somente quando forem necessários para dimensionar o número de discos conectados à máquina virtual. Caminho de SCSI não é emulado que torna o controlador preferencial para qualquer disco que não seja o disco do sistema operacional. Na verdade, com VMs da geração 2, ele é o único tipo de controlador possíveis. Introduzido no Windows Server 2012 R2, esse controlador será relatado como SAS para dar suporte a VHDX compartilhado.

## <a name="virtual-fibre-channel-hbas"></a>HBAs de Fibre Channel virtual

HBAs de Fibre Channel virtual podem ser configurados para permitir acesso direto para máquinas virtuais para Fibre Channel e Fibre Channel por Ethernet (FCoE) LUNs. Discos virtuais Fibre Channel de ignoram o sistema de arquivos NTFS na partição raiz, que reduz o uso da CPU de e/s de armazenamento.

Unidades de dados grandes e unidades que são compartilhadas entre várias máquinas virtuais (para cenários de clustering de convidado) são candidatos ideais para discos virtuais Fibre Channel.

Os discos de Fibre Channel virtual requerem um ou mais adaptadores de barramento de host (HBAs) a serem instalados no host Fibre Channel. Cada host HBA é necessário usar um driver HBA que dá suporte a recursos do Windows Server 2016 Virtual Fibre Channel/NPIV. A malha de SAN deve dar suporte a NPIV, e as portas HBA que são usadas para o Fibre Channel virtual devem ser configuradas em uma topologia de Fibre Channel com suporte para NPIV.

Para maximizar a taxa de transferência em hosts que são instalados com mais de um HBA, recomendamos que você configure vários HBAs virtuais dentro da máquina virtual de Hyper-V (até quatro HBAs pode ser configurados para cada máquina virtual). Hyper-V fará automaticamente o melhor esforço para equilibrar os HBAs virtuais ao host HBAs que acessam a mesma SAN virtual.

## <a name="virtual-disks"></a>Discos virtuais

Discos podem ser expostos para as máquinas virtuais por meio de controladores de virtual. Esses discos poderiam ser discos rígidos virtuais que são abstrações de arquivo de um disco ou um disco de passagem no host.

## <a name="virtual-hard-disks"></a>Discos rígidos virtuais

Há dois formatos de disco rígido virtual, o VHD e VHDX. Cada um desses formatos dá suporte a três tipos de arquivos de disco rígido virtual.

## <a name="vhd-format"></a>Formato VHD

O formato VHD era o formato de disco rígido virtual somente que era suportado pelo Hyper-V em versões anteriores. Introduzido no Windows Server 2012, no formato VHD foi modificado para permitir um melhor alinhamento, o que resulta em um desempenho significativamente melhor em novos discos de setor grande.

Qualquer novo VHD é criado em um Windows Server 2012 ou mais recente tem o alinhamento de 4 KB ideal. Esse formato alinhado é completamente compatível com sistemas de operacionais anteriores do Windows Server. No entanto, a propriedade do alinhamento será quebrada para novas alocações de analisadores de reconhecimento de alinhamento como (um analisador VHD de uma versão anterior do Windows Server) ou um analisador não são da Microsoft não 4 KB.

Qualquer VHD que é movido de uma versão anterior não é convertida automaticamente para esse novo formato aprimorado de VHD.

Para converter para novo formato VHD, execute o seguinte comando do Windows PowerShell:

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

Você pode verificar a propriedade de alinhamento para todos os VHDs no sistema, e ele deverá ser convertido para o alinhamento de 4 KB ideal. Criar um novo VHD com os dados a partir do VHD original usando o **criar a partir da origem** opção.

Para verificar alinhamento usando o Windows Powershell, examine a linha de alinhamento, conforme mostrado abaixo:

``` syntax
Get-VHD –Path E:\vms\testvhd\test.vhd

Path                    : E:\vms\testvhd\test.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69245440
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 10
Alignment               : 0
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

Para verificar o alinhamento usando o Windows PowerShell, examine a linha de alinhamento, conforme mostrado abaixo:

``` syntax
Get-VHD –Path E:\vms\testvhd\test-converted.vhd

Path                    : E:\vms\testvhd\test-converted.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69369856
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 0
Alignment               : 1
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

## <a name="vhdx-format"></a>Formato VHDX

VHDX é um novo formato de disco rígido virtual introduzido no Windows Server 2012, que permite que você crie resilientes discos virtuais de alto desempenho até 64 terabytes. Benefícios neste formato:

-   Suporte para a capacidade de armazenamento de disco rígido virtual de até 64 terabytes.

-   Proteção contra dados corrompidos durante falhas de energia, registrando alterações de estruturas de metadados VHDX.

-   Capacidade de armazenar metadados personalizados sobre um arquivo, que um usuário pode querer gravar, como a versão do sistema operacional ou patches aplicados.

O formato VHDX também fornece os seguintes benefícios de desempenho:

-   Melhor alinhamento do formato de disco rígido virtual para funcionar bem em discos de setor grande.

-   Tamanhos de bloco maiores para discos dinâmicos e diferenciais, que permite que esses discos se sintonizem com as necessidades da carga de trabalho.

-   4 KB setor lógico disco virtual que permite maior desempenho quando usado por aplicativos e cargas de trabalho que são projetadas para setores de 4 KB.

-   Eficiência na representação de dados, o que resulta em menor tamanho de arquivo e permite que o dispositivo de armazenamento físico subjacente recuperar espaço não utilizado. (Trim requer passagem ou discos SCSI e hardware compatível com trim.)

Quando você atualiza para o Windows Server 2016, é recomendável que você converta todos os arquivos VHD para o formato VHDX devido a esses benefícios. O único cenário em que faria sentido manter os arquivos no formato VHD é quando uma máquina virtual tem o potencial de ser movido para uma versão anterior do Hyper-V que não suporta o formato VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Tipos de arquivos de disco rígido virtual

Há três tipos de arquivos VHD. As seções a seguir estão as características de desempenho e as compensações entre os tipos.

As recomendações a seguir devem ser levadas em consideração com relação a seleção de um tipo de arquivo VHD:

-   Ao usar o formato VHD, recomendamos que você use o tipo fixo, porque ele tem características de desempenho em comparação com outros tipos de arquivo VHD e melhor resiliência.

-   Ao usar o formato VHDX, é recomendável que você use o tipo dinâmico porque ele oferece garantias de resiliência, além de economia de espaço que estão associada ao alocar espaço somente quando é necessário para fazer isso.

-   O tipo fixo também é recomendado, independentemente do formato, quando o armazenamento no volume do host não está ativamente monitorado para garantir que o espaço em disco suficiente esteja presente ao expandir o arquivo VHD em tempo de execução.

-   Instantâneos de uma máquina virtual criam um diferencial VHD para armazenar gravações nos discos. Ter apenas alguns instantâneos podem elevar o uso da CPU de armazenamento e/SS, mas talvez não visivelmente afetar o desempenho, exceto em cargas de trabalho de servidor altamente/S intensas. No entanto, ter uma grande cadeia de instantâneos pode afetar notoriamente o desempenho porque ler a partir do VHD pode exigir a verificação para os blocos solicitados de muitos VHDs diferenciais. Manter cadeias de instantâneo curtas é importante para manter o desempenho de e/s de disco em boas condições.

## <a name="fixed-virtual-hard-disk-type"></a>Tipo de disco rígido virtual fixo

Espaço para o VHD é atribuído primeiro quando o arquivo VHD é criado. Esse tipo de arquivo VHD é menos provável de fragmento, que reduz a taxa de transferência de e/s quando uma única e/s é dividido em várias e/s. Ele tem a menor sobrecarga da CPU dos três tipos de arquivo do VHD porque as leituras e gravações não precisa pesquisar o mapeamento do bloco.

## <a name="dynamic-virtual-hard-disk-type"></a>Tipo de disco rígido virtual dinâmico

Espaço para o VHD é alocado sob demanda. Os blocos no disco iniciar como blocos não alocados e não são apoiados por qualquer espaço real no arquivo. Quando um bloco é o primeiro gravado, a pilha de virtualização deve alocar espaço no arquivo de VHD para o bloco e, em seguida, atualizar os metadados. Isso aumenta o número de e/SS de disco necessários para a gravação e aumenta o uso da CPU. Leituras e gravações em blocos existentes incorrer em sobrecarga de CPU e de acesso ao disco ao procurar o mapeamento dos blocos nos metadados.

## <a name="differencing-virtual-hard-disk-type"></a>Tipo de disco rígido virtual diferencial

O VHD aponta para um arquivo VHD pai. Gravações não gravadas para resultar no espaço que está sendo alocado no arquivo VHD, assim como acontece com um VHD de expansão dinâmica de blocos. Leituras são atendidas a partir do arquivo VHD se o bloco foi gravado. Caso contrário, eles são atendidos a partir do arquivo VHD pai. Em ambos os casos, os metadados são lidos para determinar o mapeamento do bloco. Leituras e gravações nesse VHD podem consumir mais CPU e resultar em mais e/SS que um arquivo VHD fixo.

## <a name="block-size-considerations"></a>Considerações de tamanho de bloco

Tamanho do bloco pode afetar significativamente o desempenho. Ele é ideal para coincidir com o tamanho do bloco para os padrões de alocação da carga de trabalho que está usando o disco. Por exemplo, se um aplicativo está alocando em blocos de 16 MB, seria ideal ter um tamanho de bloco de disco rígido virtual de 16 MB. Um tamanho de bloco de &gt;2 MB é possível somente em discos rígidos virtuais com o formato VHDX. Ter um tamanho de bloco maior do que o padrão de alocação para uma carga de trabalho de e/s aleatória aumentará significativamente o uso do espaço no host.

## <a name="sector-size-implications"></a>Implicações de tamanho de setor

A maioria da indústria de software depende de setores de disco de 512 bytes, mas o padrão está se movendo em setores do disco de 4 KB. Para reduzir problemas de compatibilidade que podem surgir de uma alteração no tamanho do setor, fornecedores de unidade de disco rígido introduzindo um tamanho de transitional chamado de unidades de emulação de 512 (512e).

Essas unidades de emulação oferecem algumas vantagens oferecidas por 4 KB setor nativo unidades de disco, como a eficiência de formato aprimorado e um esquema melhorado para códigos de correção de erros (ECC). Eles são fornecidos com menos problemas de compatibilidade que podem ocorrer ao expor um tamanho de setor de 4 KB na interface de disco.

## <a name="support-for-512e-disks"></a>Suporte para discos 512e

Um disco 512e pode executar uma gravação apenas em termos de um sector físico — ou seja, ele não é possível gravar diretamente um setor de 512 bytes que é emitido para ele. O processo interno no disco que possibilita a essas gravações segue estas etapas:

-   O disco lê o setor físico de 4 KB para seu cache interno, que contém o setor lógico de 512 bytes referido na gravação.

-   Os dados no armazenamento em buffer de 4 KB são modificados para incluir o setor de 512 bytes atualizado.

-   O disco executa uma gravação de buffer de 4 KB atualizado novamente ao seu setor físico no disco.

Esse processo é chamado de read-modificar-write (RMW). O impacto no desempenho geral do processo RMW depende das cargas de trabalho. O processo RMW causa degradação do desempenho em discos rígidos virtuais pelos seguintes motivos:

-   Discos rígidos virtuais dinâmicos e diferenciais têm um bitmap de setor de 512 bytes na frente de sua carga de dados. Além disso, rodapé, cabeçalho e localizadores pai se alinham a um setor de 512 bytes. É comum para o driver de disco rígido virtual para emitir comandos de gravação de 512 bytes para atualizar estas estruturas, resultando no processo RMW descrito anteriormente.

-   Aplicativos normalmente problema leituras e gravações em múltiplos de tamanhos de 4 KB (o tamanho de cluster padrão do NTFS). Porque há um bitmap de setor de 512 bytes na frente do bloco de dados de conteúdo dinâmico e discos rígidos virtuais diferenciais, os blocos de 4 KB não são alinhados com o limite físico de 4 KB. A figura a seguir mostra um VHD de 4 KB bloco (realçado) que é não alinhado com o limite físico de 4 KB.

![bloco de 4 kb de VHD](../../media/perftune-guide-vhd-4kb-block.png)

Cada comando de gravação de 4 KB que é emitido pelo analisador atual para atualizar os dados de carga resulta em duas leituras para dois blocos no disco, que são então atualizados e, subsequentemente, Write-back para dois blocos de disco. Hyper-V no Windows Server 2016 minimiza alguns dos efeitos de desempenho em discos 512e na pilha VHD, preparando-se das estruturas mencionadas anteriormente para o alinhamento para limites de 4 KB no formato VHD. Isso evita o efeito RMW ao acessar os dados dentro do arquivo de disco rígido virtual e ao atualizar as estruturas de metadados de disco rígido virtual.

Como mencionado anteriormente, os VHDs são copiados de versões anteriores do Windows Server serão não automaticamente alinhados em 4 KB. Você pode convertê-las para alinhar ideal usando manualmente o **cópia de origem** opção está disponível nas interfaces do VHD de disco.

Por padrão, os VHDs são expostos com um tamanho de setor físico de 512 bytes. Isso é feito para garantir que os aplicativos dependentes de tamanho de setor físico não são afetados quando o aplicativo e os VHDs são movidos de uma versão anterior do Windows Server.

Por padrão, os discos com o formato VHDX são criados com o tamanho de setor físico de 4 KB para otimizar seus discos regulares de perfil de desempenho e discos de setor grande. Para fazer uso total de setores de 4 KB é recomendável usar o formato VHDX.

## <a name="support-for-native-4kb-disks"></a>Suporte para discos nativos de 4KB

Hyper-V no Windows Server 2012 R2 e posteriores discos nativos de 4KB dá suporte. Mas, ainda é possível armazenar o disco VHD no disco nativo de 4 KB. Isso é feito com a implementação de um software RMW algoritmo na camada de pilha de armazenamento virtual que converte as solicitações de acesso e atualização de 512 bytes em 4 KB correspondente aos acessos e atualizações.

Porque o arquivo VHD pode apenas expostas como discos de tamanho de setor lógico de 512 bytes, é muito provável que haja aplicativos que emitem solicitações de e/s de 512 bytes. Nesses casos, a camada RMW será satisfazer esses pedidos e causar degradação do desempenho. Isso também é verdadeiro para um disco que é formatado com VHDX que tem um tamanho de setor lógico de 512 bytes.

É possível configurar um arquivo VHDX seja exposto como um disco de tamanho de setor lógico de 4 KB, e isso seria uma configuração ideal para o desempenho quando o disco está hospedado em um dispositivo físico nativo de 4 KB. Tome cuidado para garantir que o convidado e o aplicativo que está usando o disco virtual contam com o tamanho de setor lógico de 4 KB. O VHDX formatação irá funcionar corretamente em um dispositivo de tamanho de setor lógico de 4 KB.

## <a name="pass-through-disks"></a>Discos de passagem

O VHD em uma máquina virtual pode ser mapeado diretamente para um disco físico ou o número de unidade lógica (LUN), em vez de em um arquivo VHD. A vantagem é que essa configuração ignora o sistema de arquivos NTFS na partição raiz, que reduz o uso da CPU de e/s de armazenamento. O risco é que os discos físicos ou LUNs podem ser mais difíceis de se mover entre computadores que os arquivos VHD.

Os discos de passagem devem ser evitados devido a limitações introduzida com cenários de migração de máquina virtual.

## <a name="advanced-storage-features"></a>Recursos avançados de armazenamento

### <a name="storage-quality-of-service-qos"></a>QoS (Qualidade de armazenamento do serviço)

A partir do Windows Server 2012 R2, Hyper-V inclui a capacidade de definir certos parâmetros (QoS) de qualidade de serviço para armazenamento nas máquinas virtuais. QoS de armazenamento fornece isolamento de desempenho de armazenamento em um ambiente multilocatário e mecanismos para notificá-lo quando o desempenho de e/s de armazenamento não atende o limite definido para executar com eficiência as cargas de trabalho de máquina virtual.

O QoS de armazenamento fornece a capacidade de especificar um valor máximo de IOPS (operações de entrada/saída por segundo) para seu disco rígido virtual. Um administrador pode restringir a E/S de armazenamento para impedir que um locatário consuma recursos de armazenamento excessivos que possam afetar outro locatário.

Você também pode definir um valor mínimo de IOPS. Eles serão notificados quando o IOPS para um determinado disco rígido virtual estiver abaixo do limite necessário para desempenho ideal.

A infraestrutura de métrica da máquina virtual também é atualizada, com parâmetros relacionados ao armazenamento para permitir que o administrador monitore os parâmetros relacionados a desempenho e estorno.

Valores mínimo e máximo são especificados em termos de IOPS normalizado, em que cada 8 KB de dados é contado como uma e/s.

Algumas das limitações são da seguinte maneira:

-   Somente para discos virtuais

-   Disco diferencial não pode ter o disco virtual pai em um volume diferente

-   Réplica – QoS para o site de réplica configurado separadamente do site primário

-   Não há suporte para VHDX compartilhado

Para obter mais informações sobre a qualidade do serviço de armazenamento, consulte [qualidade de serviço de armazenamento para Hyper-V](https://technet.microsoft.com/library/dn282281.aspx).

### <a name="numa-io"></a>E/S DE NUMA

Windows Server 2012 e, além de máquinas de virtuais dá suporte a grandes e qualquer configuração de máquina virtual grande (por exemplo, uma configuração com o Microsoft SQL Server em execução com 64 processadores virtuais) também será necessário a escalabilidade em termos de taxa de transferência de e/s.

Os seguintes aprimoramentos principais introduzido pela primeira vez na pilha de armazenamento do Windows Server 2012 e do Hyper-V fornecem as necessidades de escalabilidade de e/s de máquinas virtuais grandes:

-   Um aumento no número de canais de comunicação criado entre os dispositivos de convidado e a pilha de armazenamento do host.

-   Um mecanismo de conclusão e/s mais eficiente que envolvem a interrupção de distribuição entre os processadores virtuais para evitar interrupções de interprocessor caras.

Introduzido no Windows Server 2012, há algumas entradas do registro, localizadas em HKLM\\System\\CurrentControlSet\\Enum\\VMBUS\\{id do dispositivo}\\{id da instância}\\StorChannel, que permitem que o número de canais para ser ajustado. Eles também alinharem os processadores virtuais que manipulam as conclusões de e/s para as CPUs virtuais que são atribuídas pelo aplicativo para ser os processadores de e/s. As configurações do registro são configuradas em uma base por adaptador na chave de hardware do dispositivo.

-   **ChannelCount (DWORD)** o número total de canais para usar, com um máximo de 16. O padrão é um teto, que é o número de processadores virtuais/16.

-   **ChannelMask (QWORD)** a afinidade do processador para os canais. Se ele não está definido ou é definido como 0, o padrão será o algoritmo de distribuição de canal existente que você usa para armazenamento normal ou para canais de rede. Isso garante que seus canais de armazenamento não entrem em conflito com seus canais de rede.

### <a name="offloaded-data-transfer-integration"></a>Integração de transferência de dados descarregada

Tarefas de manutenção cruciais para VHDs, como mesclagem, mover e compactar, dependem de copiar grandes quantidades de dados. O método atual de cópia de dados requer que os dados sejam lidos e escritos em diferentes locais, o que pode ser um processo demorado. Ele também usa os recursos de CPU e memória no host, que poderia ter sido usado para máquinas virtuais do serviço.

Os fornecedores de SAN (Rede de Área de Armazenamento) estão trabalhando para fornecer operações de cópia quase instantânea de grandes quantidades de dados. Esse armazenamento é projetado para permitir que o sistema acima dos discos especifique o movimento de um conjunto específico de dados de um local para outro. Esse recurso de hardware é conhecido como uma transferência de dados descarregados.

Hyper-V no Windows Server 2012 e posteriores oferece suporte a operações de descarregamento Data Transfer (ODX), para que essas operações podem ser passadas de sistema operacional convidado para o hardware do host. Isso garante que a carga de trabalho pode usar o armazenamento de ODX habilitada como se ele estivesse em execução em um ambiente de não-virtualizados. Como a fusão de discos e armazenamento meta-operações de migração em que grandes quantidades de dados são movidas, a pilha de armazenamento do Hyper-V também emite operações de ODX durante as operações de manutenção para VHDs.

### <a name="unmap-integration"></a>Cancelar o mapeamento de integração

Arquivos de disco rígido virtual existem como arquivos em um volume de armazenamento, e eles compartilham o espaço disponível em outros arquivos. Porque o tamanho desses arquivos tende a ser grandes, o que eles consomem espaço pode crescer rapidamente. Demanda de mais armazenamento físico afeta o orçamento de hardware de TI. É importante otimizar o uso do armazenamento físico tanto quanto possível.

Antes do Windows Server 2012, quando aplicativos excluem o conteúdo dentro de um disco rígido virtual, o que efetivamente abandonado espaço de armazenamento do conteúdo, a pilha de armazenamento do Windows no sistema operacional convidado e host do Hyper-V tinha limitações que impedia a isso informações de que está sendo comunicado para o disco rígido virtual e o dispositivo de armazenamento físico. Isso impediu a pilha de armazenamento do Hyper-V de otimizar o uso do espaço pelos arquivos de disco virtual com base no VHD. Ele também impedidos no dispositivo de armazenamento subjacente de recuperar o espaço que anteriormente era ocupado pelos dados excluídos.

A partir do Windows Server 2012, o Hyper-V dá suporte a desmapear as notificações, que permitem que os arquivos VHDX ser mais eficiente que representa os dados dentro dele. Isso resulta em menor tamanho de arquivos e permite que o dispositivo de armazenamento físico subjacente recuperar espaço não utilizado.

Somente o SCSI específico do Hyper-V, habilitados para o IDE e Fibre Channel Virtual controladores permitem que o comando de desmapeamento do convidado para alcançar a pilha de armazenamento virtual do host. Sobre os discos rígidos virtuais, apenas a discos virtuais formatados como VHDX suporte desmapear comandos do convidado.

Por esses motivos, recomendamos que você use arquivos VHDX anexados a um controlador SCSI quando não estiver usando discos virtuais Fibre Channel.

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura do Hyper-V](architecture.md)

-   [Configuração de servidor Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de e/s de rede de Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
