---
title: Desempenho de e/s de armazenamento do Hyper-V
description: Considerações de desempenho de e/s de armazenamento no ajuste de desempenho do Hyper-V
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 762ff719eb60a2fbcec61c0b9b6cb2e14f9ba677
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078214"
---
# <a name="hyper-v-storage-io-performance"></a>Desempenho de e/s de armazenamento do Hyper-V

Esta seção descreve as diferentes opções e considerações para ajustar o desempenho de e/s de armazenamento em uma máquina virtual. O caminho de e/s de armazenamento se estende da pilha de armazenamento de convidado, por meio da camada de virtualização de host, para a pilha de armazenamento do host e, em seguida, para o disco físico. A seguir estão explicações sobre como as otimizações são possíveis em cada um desses estágios.

## <a name="virtual-controllers"></a>Controladores virtuais

O Hyper-V oferece três tipos de controladores virtuais: IDE, SCSI e HBAs (adaptadores de barramento de host virtual).

## <a name="ide"></a>IDE

Os controladores IDE expõem discos IDE para a máquina virtual. O controlador IDE é emulado e é o único controlador que está disponível para VMs convidadas que executam uma versão mais antiga do Windows sem a máquina virtual Integration Services. A e/s de disco executada usando o driver de filtro IDE que é fornecida com a máquina virtual Integration Services é significativamente melhor do que o desempenho de e/s de disco fornecido com o controlador IDE emulado. Recomendamos que os discos IDE sejam usados somente para os discos do sistema operacional porque eles têm limitações de desempenho devido ao tamanho máximo de e/s que pode ser emitido para esses dispositivos.

## <a name="scsi-sas-controller"></a>SCSI (controlador SAS)

Controladores SCSI expõem discos SCSI para a máquina virtual, e cada controlador SCSI virtual pode dar suporte a até 64 dispositivos. Para obter um desempenho ideal, recomendamos que você anexe vários discos a um único controlador SCSI virtual e crie controladores adicionais somente quando eles forem necessários para dimensionar o número de discos conectados à máquina virtual. O caminho SCSI não é emulado, o que o torna o controlador preferencial para qualquer disco que não seja o disco do sistema operacional. Na verdade, com VMs de geração 2, é o único tipo de controlador possível. Introduzido no Windows Server 2012 R2, esse controlador é relatado como SAS para dar suporte a VHDX compartilhado.

## <a name="virtual-fibre-channel-hbas"></a>HBAs de Fibre Channel virtual

Os HBAs de Fibre Channel virtual podem ser configurados para permitir o acesso direto para máquinas virtuais a Fibre Channel e Fibre Channel LUNs FCoE (Ethernet). Discos de Fibre Channel virtual ignoram o sistema de arquivos NTFS na partição raiz, o que reduz o uso da CPU de e/s de armazenamento.

Unidades de dados grandes e unidades compartilhadas entre várias máquinas virtuais (para cenários de clustering de convidado) são candidatas principais para discos de Fibre Channel virtual.

Discos Fibre Channel virtuais exigem que um ou mais Fibre Channel HBAs (adaptadores de barramento de host) sejam instalados no host. Cada HBA host é necessário para usar um driver HBA que dá suporte aos recursos virtuais Fibre Channel/NPIV do Windows Server 2016. A malha de SAN deve dar suporte a NPIV e as portas de HBA usadas para o Fibre Channel virtual devem ser configuradas em uma topologia de Fibre Channel que ofereça suporte a NPIV.

Para maximizar a taxa de transferência em hosts instalados com mais de um HBA, recomendamos que você configure vários HBAs virtuais dentro da máquina virtual do Hyper-V (até quatro HBAs podem ser configurados para cada máquina virtual). O Hyper-V fará automaticamente um melhor esforço para balancear HBAs virtuais para hospedar HBAs que acessam a mesma SAN virtual.

## <a name="virtual-disks"></a>Discos virtuais

Os discos podem ser expostos às máquinas virtuais por meio dos controladores virtuais. Esses discos podem ser discos rígidos virtuais que são abstrações de arquivo de um disco ou um disco de passagem no host.

## <a name="virtual-hard-disks"></a>Discos rígidos virtuais

Há dois formatos de disco rígido virtual, VHD e VHDX. Cada um desses formatos dá suporte a três tipos de arquivos de disco rígido virtual.

## <a name="vhd-format"></a>Formato VHD

O formato VHD era o único formato de disco rígido virtual com suporte do Hyper-V em versões anteriores. Introduzido no Windows Server 2012, o formato VHD foi modificado para permitir um melhor alinhamento, o que resulta em um desempenho significativamente melhor em novos discos de setor grande.

Qualquer novo VHD criado em um Windows Server 2012 ou mais recente tem o alinhamento ideal de 4 KB. Esse formato alinhado é completamente compatível com os sistemas operacionais anteriores do Windows Server. No entanto, a propriedade Alignment será quebrada para novas alocações de analisadores que não tenham reconhecimento de alinhamento de 4 KB (como um analisador VHD de uma versão anterior do Windows Server ou um analisador que não seja da Microsoft).

Qualquer VHD movido de uma versão anterior não é convertido automaticamente para esse novo formato de VHD aprimorado.

Para converter em um novo formato VHD, execute o seguinte comando do Windows PowerShell:

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

Você pode verificar a propriedade Alignment de todos os VHDs no sistema e ele deve ser convertido para o alinhamento ideal de 4 KB. Você cria um novo VHD com os dados do VHD original usando a opção **criar-de-fonte** .

Para verificar o alinhamento usando o Windows PowerShell, examine a linha de alinhamento, conforme mostrado abaixo:

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

O VHDX é um novo formato de disco rígido virtual introduzido no Windows Server 2012, que permite criar discos virtuais de alto desempenho resilientes de até 64 terabytes. Os benefícios desse formato incluem:

-   Suporte para capacidade de armazenamento de disco rígido virtual de até 64 terabytes.

-   Proteção contra dados corrompidos durante falhas de energia, registrando alterações de estruturas de metadados VHDX.

-   Capacidade de armazenar metadados personalizados sobre um arquivo, que um usuário pode querer registrar, como versão do sistema operacional ou patches aplicados.

O formato VHDX também fornece os seguintes benefícios de desempenho:

-   Melhor alinhamento do formato de disco rígido virtual para funcionar bem em discos de setor grande.

-   Tamanhos de bloco maiores para discos dinâmicos e diferenciais, o que permite que esses discos Attune as necessidades da carga de trabalho.

-   disco virtual do setor lógico de 4 KB que permite maior desempenho quando usado por aplicativos e cargas de trabalho que são projetadas para setores de 4 KB.

-   Eficiência na representação de dados, o que resulta em um tamanho de arquivo menor e permite que o dispositivo de armazenamento físico subjacente recupere espaço não utilizado. (Trim requer discos de passagem ou SCSI e hardware compatível com o Trim.)

Ao atualizar para o Windows Server 2016, recomendamos que você converta todos os arquivos VHD para o formato VHDX devido a esses benefícios. O único cenário no qual faria sentido manter os arquivos no formato VHD é quando uma máquina virtual tem o potencial de ser movido para uma versão anterior do Hyper-V que não dá suporte ao formato VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Tipos de arquivos de disco rígido virtual

Há três tipos de arquivos VHD. As seções a seguir são as características de desempenho e as compensações entre os tipos.

As seguintes recomendações devem ser levadas em consideração com relação à seleção de um tipo de arquivo VHD:

-   Ao usar o formato VHD, recomendamos que você use o tipo fixo porque ele tem características de resiliência e desempenho melhores em comparação com os outros tipos de arquivo VHD.

-   Ao usar o formato VHDX, recomendamos que você use o tipo dinâmico, pois ele oferece garantias de resiliência, além da economia de espaço associada à alocação de espaço somente quando houver a necessidade de fazer isso.

-   O tipo fixo também é recomendado, independentemente do formato, quando o armazenamento no volume de hospedagem não é monitorado ativamente para garantir que haja espaço em disco suficiente ao expandir o arquivo VHD em tempo de execução.

-   Instantâneos de uma máquina virtual criam um VHD diferencial para armazenar gravações nos discos. Ter apenas alguns instantâneos pode elevar o uso da CPU de e/SS de armazenamento, mas talvez não afete visivelmente o desempenho, exceto em cargas de trabalho de servidor altamente intensivas de e/s. No entanto, ter uma grande cadeia de instantâneos pode afetar o desempenho de forma perceptível, pois a leitura do VHD pode exigir a verificação dos blocos solicitados em muitos VHDs diferenciais. Manter a cadeia de instantâneos curta é importante para manter O bom desempenho de e/s de disco.

## <a name="fixed-virtual-hard-disk-type"></a>Tipo de disco rígido virtual fixo

O espaço para o VHD é alocado primeiro quando o arquivo VHD é criado. Esse tipo de arquivo VHD é menos provável de fragmentar, o que reduz a taxa de transferência de e/s quando uma e/s única é dividida em várias e/SS. Ele tem a menor sobrecarga de CPU dos três tipos de arquivo VHD porque leituras e gravações não precisam Pesquisar o mapeamento do bloco.

## <a name="dynamic-virtual-hard-disk-type"></a>Tipo de disco rígido virtual dinâmico

O espaço para o VHD é alocado sob demanda. Os blocos no disco começam como blocos não alocados e não são apoiados por nenhum espaço real no arquivo. Quando um bloco é gravado pela primeira vez no, a pilha de virtualização deve alocar espaço dentro do arquivo VHD para o bloco e, em seguida, atualizar os metadados. Isso aumenta o número de e/SS de disco necessárias para a gravação e aumenta o uso da CPU. Leituras e gravações em blocos existentes incorrem no acesso ao disco e na sobrecarga da CPU ao pesquisar o mapeamento dos blocos nos metadados.

## <a name="differencing-virtual-hard-disk-type"></a>Tipo de disco rígido virtual diferencial

O VHD aponta para um arquivo VHD pai. Qualquer gravação em blocos não gravados resulta na alocação de espaço no arquivo VHD, assim como com um VHD de expansão dinâmica. As leituras são atendidas a partir do arquivo VHD se o bloco tiver sido gravado no. Caso contrário, eles serão atendidos a partir do arquivo VHD pai. Em ambos os casos, os metadados são lidos para determinar o mapeamento do bloco. Leituras e gravações nesse VHD podem consumir mais CPU e resultar em mais e/s do que um arquivo VHD fixo.

## <a name="block-size-considerations"></a>Considerações de tamanho de bloco

O tamanho do bloco pode afetar significativamente o desempenho. É ideal fazer a correspondência entre o tamanho do bloco e os padrões de alocação da carga de trabalho que está usando o disco. Por exemplo, se um aplicativo estiver alocando em partes de 16 MB, seria ideal ter um tamanho de bloco de disco rígido virtual de 16 MB. Um tamanho de bloco de &gt; 2 MB só é possível em discos rígidos virtuais com o formato VHDX. Ter um tamanho de bloco maior que o padrão de alocação para uma carga de trabalho de e/s aleatória aumentará significativamente o uso do espaço no host.

## <a name="sector-size-implications"></a>Implicações de tamanho de setor

A maior parte do setor de software dependia de setores de disco de 512 bytes, mas o padrão é migrar para os setores de disco de 4 KB. Para reduzir os problemas de compatibilidade que podem surgir de uma alteração no tamanho do setor, os fornecedores de disco rígido estão introduzindo um tamanho de transição referido como 512e (unidades de emulação) de 512.

Essas unidades de emulação oferecem algumas das vantagens oferecidas por 4 KB de unidades nativas de setor de disco, como eficiência de formato aprimorada e um esquema aprimorado para ECC (códigos de correção de erros). Eles são fornecidos com menos problemas de compatibilidade que ocorreriam expondo um tamanho de setor de 4 KB na interface do disco.

## <a name="support-for-512e-disks"></a>Suporte para discos 512e

Um disco 512e pode executar uma gravação somente em termos de um setor físico, ou seja, não pode gravar diretamente um setor 512byte que é emitido para ele. O processo interno no disco que torna essas gravações possíveis segue estas etapas:

-   O disco lê o setor físico de 4 KB para seu cache interno, que contém o setor lógico de 512 bytes referido na gravação.

-   Os dados no armazenamento em buffer de 4 KB são modificados para incluir o setor de 512 bytes atualizado.

-   O disco executa uma gravação do buffer de 4 KB atualizado de volta ao seu setor físico no disco.

Esse processo é chamado de Read-Modify-Write (RMW). O impacto geral no desempenho do processo RMW depende das cargas de trabalho. O processo RMW causa degradação de desempenho em discos rígidos virtuais pelos seguintes motivos:

-   Discos rígidos virtuais dinâmicos e diferenciais têm um bitmap de setor de 512 bytes na frente de sua carga de dados. Além disso, o rodapé, o cabeçalho e os localizadores pai se alinham a um setor de 512 bytes. É comum que o driver de disco rígido virtual emita comandos de gravação de 512 bytes para atualizar essas estruturas, resultando no processo RMW descrito anteriormente.

-   Aplicativos normalmente emitem leituras e gravações em múltiplos de 4 KB de tamanhos (o tamanho de cluster padrão do NTFS). Como há um bitmap de setor de 512 bytes na frente do bloco de carga de dados de discos rígidos virtuais dinâmicos e diferenciais, os blocos de 4 KB não estão alinhados ao limite físico de 4 KB. A figura a seguir mostra um bloco de 4 KB do VHD (realçado) que não está alinhado com o limite físico de 4 KB.

![bloco do VHD 4 KB](../../media/perftune-guide-vhd-4kb-block.png)

Cada comando de gravação de 4 KB emitido pelo analisador atual para atualizar os dados de carga resulta em duas leituras para dois blocos no disco, que são atualizados e, subsequentemente, gravados nos dois blocos de disco. O Hyper-V no Windows Server 2016 atenua alguns dos efeitos de desempenho nos discos 512e na pilha VHD, preparando as estruturas mencionadas anteriormente para alinhamento a quatro limites de KB no formato VHD. Isso evita o efeito de RMW ao acessar os dados no arquivo de disco rígido virtual e ao atualizar as estruturas de metadados do disco rígido virtual.

Como mencionado anteriormente, os VHDs copiados de versões anteriores do Windows Server não serão automaticamente alinhados com 4 KB. Você pode convertê-los manualmente para um alinhamento ideal usando a opção **Copiar do disco de origem** que está disponível nas interfaces VHD.

Por padrão, os VHDs são expostos com um tamanho de setor físico de 512 bytes. Isso é feito para garantir que os aplicativos dependentes do tamanho do setor físico não sejam afetados quando o aplicativo e os VHDs forem movidos de uma versão anterior do Windows Server.

Por padrão, os discos com o formato VHDX são criados com o tamanho de setor físico de 4 KB para otimizar seus discos regulares de perfil de desempenho e discos de setor grande. Para fazer uso completo de 4 KB de setores, é recomendável usar o formato VHDX.

## <a name="support-for-native-4kb-disks"></a>Suporte para discos nativos de 4 KB

O Hyper-V no Windows Server 2012 R2 e posterior dá suporte a 4 KB de discos nativos. Mas ainda é possível armazenar o disco VHD em um disco nativo de 4 KB. Isso é feito com a implementação de um algoritmo RMW de software na camada de pilha de armazenamento virtual que converte o acesso de 512 bytes e as solicitações de atualização para acessos e atualizações correspondentes de 4 KB.

Como o arquivo VHD só pode se expor como discos de tamanho de setor lógico de 512 bytes, é muito provável que haja aplicativos que emitam solicitações de e/s de 512 bytes. Nesses casos, a camada RMW atenderá a essas solicitações e causará a degradação do desempenho. Isso também é verdadeiro para um disco que é formatado com VHDX que tem um tamanho de setor lógico de 512 bytes.

É possível configurar um arquivo VHDX para ser exposto como um disco de tamanho de setor lógico de 4 KB, e isso seria uma configuração ideal para o desempenho quando o disco está hospedado em um dispositivo físico nativo de 4 KB. Deve-se ter cuidado para garantir que o convidado e o aplicativo que está usando o disco virtual sejam apoiados pelo tamanho de setor lógico de 4 KB. A formatação VHDX funcionará corretamente em um dispositivo de tamanho de setor lógico de 4 KB.

## <a name="pass-through-disks"></a>Discos de passagem

O VHD em uma máquina virtual pode ser mapeado diretamente para um disco físico ou LUN (número de unidade lógica), em vez de para um arquivo VHD. O benefício é que essa configuração ignora o sistema de arquivos NTFS na partição raiz, o que reduz o uso da CPU de e/s de armazenamento. O risco é que os discos físicos ou LUNs podem ser mais difíceis de se mover entre máquinas que arquivos VHD.

Discos de passagem devem ser evitados devido às limitações introduzidas com cenários de migração de máquina virtual.

## <a name="advanced-storage-features"></a>Recursos de armazenamento avançados

### <a name="storage-quality-of-service-qos"></a>QoS (Qualidade de armazenamento do serviço)

A partir do Windows Server 2012 R2, o Hyper-V inclui a capacidade de definir determinados parâmetros de QoS (qualidade de serviço) para armazenamento nas máquinas virtuais. O QoS de armazenamento fornece isolamento de desempenho de armazenamento em um ambiente multilocatário e mecanismos para notificá-lo quando o desempenho de E/S do armazenamento não atender ao limites definido para executar, de forma eficiente, cargas de trabalho de sua máquina virtual.

O QoS de armazenamento fornece a capacidade de especificar um valor máximo de IOPS (operações de entrada/saída por segundo) para seu disco rígido virtual. Um administrador pode restringir a E/S de armazenamento para impedir que um locatário consuma recursos de armazenamento excessivos que possam afetar outro locatário.

Você também pode definir um valor de IOPS mínimo. Eles serão notificados quando o IOPS para um determinado disco rígido virtual estiver abaixo do limite necessário para desempenho ideal.

A infraestrutura de métrica da máquina virtual também é atualizada, com parâmetros relacionados ao armazenamento para permitir que o administrador monitore os parâmetros relacionados a desempenho e estorno.

Os valores máximo e mínimo são especificados em termos de IOPS normalizados, em que cada 8 KB de dados é contado como uma e/s.

Algumas das limitações são as seguintes:

-   Somente para discos virtuais

-   O disco diferencial não pode ter um disco virtual pai em um volume diferente

-   Réplica-QoS para o site de réplica configurado separadamente do site primário

-   VHDX compartilhado não é suportado

Para obter mais informações sobre a qualidade de serviço de armazenamento, consulte [qualidade de serviço de armazenamento para Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282281(v=ws.11)).

### <a name="numa-io"></a>E/S NUMA

O Windows Server 2012 e posterior dá suporte a máquinas virtuais grandes, e qualquer configuração de máquina virtual grande (por exemplo, uma configuração com Microsoft SQL Server em execução com processadores virtuais 64) também precisará de escalabilidade em termos de taxa de transferência de e/s.

Os principais aprimoramentos a seguir introduzidos pela primeira vez na pilha de armazenamento do Windows Server 2012 e no Hyper-V fornecem as necessidades de escalabilidade de e/s de grandes máquinas virtuais:

-   Um aumento no número de canais de comunicação criados entre os dispositivos convidados e a pilha de armazenamento do host.

-   Um mecanismo de conclusão de e/s mais eficiente envolvendo a distribuição de interrupção entre os processadores virtuais para evitar interrupções caras de interprocessador.

Introduzido no Windows Server 2012, há algumas entradas do registro, localizadas em HKLM \\ System \\ CurrentControlSet \\ enum \\ VMBUS \\ {ID do dispositivo} \\ {Instance ID} \\ StorChannel, que permitem que o número de canais seja ajustado. Eles também alinham os processadores virtuais que lidam com as conclusões de e/s para as CPUs virtuais que são atribuídas pelo aplicativo para serem os processadores de e/s. As configurações do registro são definidas em uma base por adaptador na chave de hardware do dispositivo.

-   **ChannelCount (DWORD)** O número total de canais a serem usados, com um máximo de 16. O padrão é um teto, que é o número de processadores virtuais/16.

-   **ChannelMask (QWORD)** A afinidade do processador para os canais. Se não estiver definido ou estiver definido como 0, o padrão será o algoritmo de distribuição de canal existente que você usa para o armazenamento normal ou para canais de rede. Isso garante que os canais de armazenamento não entrarão em conflito com os canais de rede.

### <a name="offloaded-data-transfer-integration"></a>Integração de Transferência de Dados descarregada

Tarefas de manutenção cruciais para VHDs, como mesclar, mover e compactar, dependem de copiar grandes quantidades de dados. O método atual de cópia de dados requer que os dados sejam lidos e escritos em diferentes locais, o que pode ser um processo demorado. Ele também usa recursos de CPU e memória no host, que poderiam ter sido usados para atender a máquinas virtuais.

Os fornecedores de SAN (Rede de Área de Armazenamento) estão trabalhando para fornecer operações de cópia quase instantânea de grandes quantidades de dados. Esse armazenamento é projetado para permitir que o sistema acima dos discos especifique a movimentação de um conjunto de dados específico de um local para outro. Esse recurso de hardware é conhecido como um Transferência de Dados descarregado.

O Hyper-V no Windows Server 2012 e posterior dá suporte a operações de descarregamento Transferência de Dados (ODX) para que essas operações possam ser passadas do sistema operacional convidado para o hardware do host. Isso garante que a carga de trabalho possa usar o armazenamento habilitado para ODX como faria se estivesse em execução em um ambiente não virtualizado. A pilha de armazenamento do Hyper-V também emite operações de ODX durante operações de manutenção para VHDs, como mesclar discos e meta operações de migração de armazenamento onde grandes quantidades de dados são movidas.

### <a name="unmap-integration"></a>Integração de mapeamento

Os arquivos de disco rígido virtual existem como arquivos em um volume de armazenamento e compartilham espaço disponível com outros arquivos. Como o tamanho desses arquivos tende a ser grande, o espaço que eles consomem pode crescer rapidamente. A demanda por mais armazenamento físico afeta o orçamento de hardware de ti. É importante otimizar o uso do armazenamento físico o máximo possível.

Antes do Windows Server 2012, quando os aplicativos excluem o conteúdo dentro de um disco rígido virtual, o que efetivamente abandonou o espaço de armazenamento do conteúdo, a pilha de armazenamento do Windows no sistema operacional convidado e o host do Hyper-V tinham limitações que impediam que essas informações fossem comunicadas ao disco rígido virtual e ao dispositivo de armazenamento físico. Isso impediu que a pilha de armazenamento do Hyper-V otimizasse o uso de espaço pelos arquivos de disco virtual baseados em VHD. Ele também impediu que o dispositivo de armazenamento subjacente recuperasse o espaço ocupado anteriormente pelos dados excluídos.

A partir do Windows Server 2012, o Hyper-V dá suporte a notificações de desmapeamento, que permitem que os arquivos VHDX sejam mais eficientes para representar esses dados dentro dele. Isso resulta em tamanho de arquivos menores e permite que o dispositivo de armazenamento físico subjacente recupere espaço não utilizado.

Somente o SCSI específico do Hyper-V, o IDE habilitado e os controladores de Fibre Channel virtual permitem que o comando de desmapeamento do convidado alcance a pilha de armazenamento virtual do host. Nos discos rígidos virtuais, somente os discos virtuais formatados como VHDX dão suporte a comandos de mapeamento do convidado.

Por esses motivos, recomendamos que você use arquivos VHDX anexados a um controlador SCSI quando não estiver usando discos Fibre Channel virtuais.

## <a name="additional-references"></a>Referências adicionais

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)