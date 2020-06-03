---
title: Solucionando problemas de cluster com a ID de evento 1135
description: Descreve como solucionar o problema de inicialização do serviço de cluster com a ID de evento 1135.
ms.date: 05/28/2020
ms.openlocfilehash: d59f8b89e89ea7ff42aecd79670465aee8d63524
ms.sourcegitcommit: 5fac756c2c9920757e33ef0a68528cda0c85dd04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84306525"
---
# <a name="troubleshooting-cluster-issue-with-event-id-1135"></a>Solucionando problemas de cluster com a ID de evento 1135

Este artigo ajuda você a diagnosticar e resolver a ID de evento 1135, que pode ser registrada durante a inicialização do Serviço de cluster no ambiente de clustering de failover.

## <a name="start-page"></a>Start Page

A ID de evento 1135 indica que um ou mais nós de cluster foram removidos da Associação de cluster de failover ativa. Ele pode ser acompanhado pelos seguintes sintomas:

- Failover\nodes de cluster sendo removido da Associação de cluster de failover ativo: [tendo um problema com os nós sendo removidos da Associação de cluster de failover ativo](/problem-nodes-failover-cluster.md)
- ID de evento 1069 ID [de evento 1069 – disponibilidade de aplicativo ou serviço em cluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc756225(v=ws.10))
- ID do evento 1177 para a ID de evento de perda de quorum [1177 — quorum e conectividade necessários para quorum](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773498(v=ws.10))
- ID do evento 1006 para Serviço de cluster interrompida: [ID do evento 1006 — inicialização do serviço de cluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773418(v=ws.10))

Uma validação e os testes de rede seriam recomendados como uma das etapas de solução de problemas iniciais para garantir que não haja nenhum problema de configuração que possa ser uma causa de problemas.

### <a name="check-if-installed-the-recommended-hot-fixes"></a>Verificar se o hotfix recomendado foi instalado

O Serviço de cluster é o componente de software essencial que controla todos os aspectos da operação de cluster de failover e gerencia o banco de dados de configuração de cluster. Se você vir a ID de evento 1135, a Microsoft recomendará que você instale as correções mencionadas nos artigos da base de conhecimento abaixo e reinicialize todos os nós do cluster e, em seguida, observe se o problema ocorre novamente.

- [Hotfix para Windows Server 2012 R2](https://support.microsoft.com/help/2920151)
- [Hotfix para Windows Server 2012](https://support.microsoft.com/help/2784261)
- [Hotfix para Windows Server 2008 R2](https://support.microsoft.com/help/2545685)

### <a name="check-if-the-cluster-service-running-on-all-the-nodes"></a>Verificar se o serviço de cluster está em execução em todos os nós

Siga o comando a seguir de acordo com o sistema operacional Windows para validar se o serviço de cluster está em execução e disponível continuamente.

#### <a name="for-windows-server-2008-r2-cluster"></a>Para o cluster do Windows Server 2008 R2

na execução do prompt cmd elevado: **nó do cluster. exe/stat**  

#### <a name="for-windows-server-2012-and-windows-server-2012-r2-cluster"></a>Para o Windows Server 2012 \ e o cluster do Windows Server 2012 R2

executar comando PS: **nó de cluster/status**  

O serviço de cluster está em execução continuamente e disponível em todos os nós?

## <a name="solution-for-cluster-service-is-failing"></a>A solução para o serviço de cluster está falhando

Se o serviço de cluster estiver falhando, solucione o problema usando este artigo: [Opções de inicialização do cluster de failover do Windows Server 2008 e do 2008R2](/archive/blogs/askcore/windows-server-2008-and-2008r2-failover-cluster-startup-switches).


## <a name="several-scenarios-of-event-id-1135"></a>Vários cenários de ID de evento 1135

Queremos que você dê uma olhada mais detalhada nos logs de eventos do sistema em todos os nós do cluster. Examine a ID de evento 1135 que você está vendo nos nós e copie todas as instâncias desse evento. Isso o tornará conveniente para você examiná-los e analisá-los.

```console
Event ID 1135
Cluster node ' **NODE A** ' was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Há três cenários típicos:

### <a name="scenario-a"></a>Cenário A

Você está examinando todos os eventos e todos os nós no cluster estão indicando que o nó A perdeu A comunicação.

![Cenário um ](media/troubleshooting-cluster-event-id-1135/18647.png)
 ![ cenário a](media/troubleshooting-cluster-event-id-1135/18648.png)

Pode ser possível que, quando você estiver vendo os logs do sistema no nó A, ele tenha eventos para todos os nós restantes no cluster.

#### <a name="solution"></a>Solução

Isso sugere que, no momento do problema, seja devido ao congestionamento da rede ou, de outra forma, a comunicação com o nó A foi perdida.

Você deve examinar e validar a configuração de rede e os problemas de comunicação. Lembre-se de procurar problemas referentes ao nó A.

### <a name="scenario-b"></a>Cenário B

Você está observando os eventos nos nós e nos permite dizer que o cluster está disperso em dois sites. NÓ A, nó B e nó C no site 1 e nó D & nó E no site 2.

![Cenário B](media/troubleshooting-cluster-event-id-1135/18649.png)

Nos nós A, B e C, você verá que os eventos registrados são para conectividade com os nós D & E. Da mesma forma, quando você vê os eventos nos nós D & E, os eventos sugerem que perdemos a comunicação com A, B e C.

![Cenário B](media/troubleshooting-cluster-event-id-1135/18650.png)

#### <a name="solution"></a>Solução

Se você vir uma atividade semelhante, indicará que houve uma falha de comunicação, sobre o link que conecta esses sites. Recomendamos que você examine a conexão entre os sites, se isso estiver em uma conexão WAN, sugerimos que você verifique com seu ISP sobre a conectividade.

### <a name="scenario-c"></a>Cenário C

Você está observando os eventos nos nós e vê que os nomes dos nós não são altos com qualquer padrão específico. Digamos que o cluster esteja disperso em dois sites. NÓ A, nó B e nó C no site 1 e nó D & nó E no site 2.

- No nó A: você vê eventos para os nós B, D, E.
- No nó B: você vê eventos para os nós C, D, E.
- No nó C: você vê eventos para os nós A, B, E.
- No nó D: você vê eventos para os nós A, C, E.
- No nó E: você vê eventos para os nós B, C, D.
- Ou quaisquer outras combinações.

![Cenário C](media/troubleshooting-cluster-event-id-1135/18651.png)

#### <a name="solution"></a>Solução

Esses eventos são possíveis quando os canais de rede entre os nós são obstrudos e as mensagens de comunicação do cluster não estão chegando em tempo hábil, fazendo com que o cluster sinta que a comunicação entre os nós é perdida, resultando na remoção de nós da associação do cluster.

## <a name="review-cluster-networks"></a>Examinar redes de cluster

Recomendamos que você examine as redes de cluster verificando as três opções a seguir, uma a uma, para continuar este guia de solução de problemas.

### <a name="check-for-antivirus-exclusion"></a>Verificar a exclusão de antivírus

Exclua os seguintes locais do sistema de arquivos da verificação de vírus em um servidor que esteja executando serviços de cluster:

- O caminho da testemunha de FileShare

- A *pasta%SystemRoot%\Cluster*

Configure o componente de verificação em tempo real em seu software antivírus para excluir os seguintes diretórios e arquivos:

- Diretório de configuração de máquina virtual padrão (C:\ProgramData\Microsoft\Windows\Hyper-V)

- Diretórios de configuração de máquina virtual personalizada

- Diretório de unidade de disco rígido virtual padrão (discos rígidos C:\Users\Public\Documents\Hyper-V\Virtual)

- Diretórios de unidade de disco rígido virtual personalizados

- Diretórios de dados de replicação personalizados, se você estiver usando a réplica do Hyper-V

- Diretórios de instantâneos

- MMS. exe

    > [!NOTE]
    > Esse arquivo pode ter que ser configurado como uma exclusão de processo dentro do software antivírus.)

- VMWP. exe

    > [!NOTE]
    > Esse arquivo pode ter que ser configurado como uma exclusão de processo dentro do software antivírus.

Além disso, ao usar Migração ao Vivo junto com volumes compartilhados do cluster, exclua o caminho CSV *C:\Clusterstorage* e todos os seus subdiretórios. Se você estiver solucionando problemas de failover ou problemas gerais com os serviços de cluster e o software antivírus estiver instalado, desinstale temporariamente o software antivírus ou verifique com o fabricante do software para determinar se o software antivírus funciona com os serviços de cluster. Apenas desabilitar o software antivírus é insuficiente na maioria dos casos. Mesmo que você desabilite o software antivírus, o driver de filtro ainda será carregado quando você reiniciar o computador.

### <a name="check-for-network-port-configuration-in-firewall"></a>Verificar a configuração de porta de rede no firewall

O Serviço de cluster controla as operações de cluster de servidor e gerencia o banco de dados do cluster. Um cluster é uma coleção de computadores independentes que atuam como um único computador. Gerentes, programadores e usuários veem o cluster como um único sistema. O software distribui dados entre os nós do cluster. Se um nó falhar, outros nós fornecerão os serviços e os dados que foram fornecidos anteriormente pelo nó ausente. Quando um nó é adicionado ou reparado, o software de cluster migra alguns dados para esse nó.

Nome do serviço de sistema: **ClusSvc**  

|Aplicativo|Protocolo|Portas|
|---|---|---|
|Serviço de cluster|UDP|3343|
|Serviço de cluster|TCP|3343 (essa porta é necessária durante uma operação de ingresso no nó.)|
|RPC|TCP|135|
|Administrador do cluster|UDP|137|
|Kerberos|UDP\TCP|464 *|
|SMB|TCP|445|
|Portas UDP altas alocadas aleatoriamente * *|UDP|Número da porta aleatória entre 1024 e 65535<br/>Número da porta aleatória entre 49152 e 65535 * * *|
||||

> [!NOTE]
> Além disso, para uma validação bem-sucedida nos clusters de failover do Windows no Windows Server 2008 e superior, permita o tráfego de entrada e de saída para ICMP4, ICMP6.

- Para obter mais informações, consulte [criando um cluster de failover do Windows Server 2012 falha com o erro 0xc000005e](https://support.microsoft.com/help/2830510).

- Para obter mais informações sobre como personalizar essas portas, consulte [visão geral do serviço e requisitos de porta de rede para Windows](https://support.microsoft.com/help/832017/) na seção "referências".

Esse é o intervalo no Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista.

Além disso, execute o seguinte comando para verificar a configuração de porta de rede no firewall. Por exemplo: esse comando ajuda a determinar a porta 3343 available\open usada para o cluster de failover:

```console
netsh advfirewall firewall show rule name="Failover Clusters (UDP-In)" verbose
```

### <a name="run-the-cluster-validation-report-for-any-errors-or-warnings"></a>Executar o relatório de validação de cluster para erros ou avisos

A ferramenta de validação de cluster executa um conjunto de testes para verificar se o seu hardware e as configurações são compatíveis com o clustering de failover.

Siga estas instruções:

1. Execute o relatório de validação de cluster para erros ou avisos. Para obter mais informações, consulte [noções básicas sobre testes de validação de cluster: rede](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN)

    ![subhatt1](media/troubleshooting-cluster-event-id-1135/18653.png)

2. Verifique se há avisos e erros para redes. Para obter mais informações, consulte [noções básicas sobre testes de validação de cluster: rede](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN).

    ![Resultados por rede de categoria ](media/troubleshooting-cluster-event-id-1135/18654.png) ![](media/troubleshooting-cluster-event-id-1135/18655.png)

#### <a name="check-the-list-network-binding-order"></a>Verificar a ordem de associação de rede

Esse teste lista a ordem na qual as redes são ligadas aos adaptadores em cada nó.

A guia adaptadores e associações lista as conexões na ordem em que as conexões são acessadas pelos serviços de rede. A ordem dessas conexões reflete a ordem em que chamadas/pacotes de TCP/IP genéricos são enviadas para a conexão.

Siga as etapas abaixo para alterar a ordem de ligação dos adaptadores de rede:

1. Clique em **Iniciar**, **executar**, digite ncpa. cpl e clique em **OK**. Você pode ver as conexões disponíveis na seção **LAN e Internet de alta velocidade** da janela **conexões de rede** .

2. No menu **avançado** , clique em **Configurações avançadas**e, em seguida, clique na guia **adaptadores e associações** .

3. Na área **conexões** , selecione a conexão que você deseja mover para cima na lista. Use os botões de seta para mover a conexão. Como regra geral, o cartão que se comunica com a rede (conectividade de domínio, roteamento para outras redes etc. deve ser o primeiro cartão associado (parte superior da lista).

Os nós de cluster são sistemas de hospedagem múltipla. A prioridade de rede afeta o cliente DNS para a conectividade de rede de saída. Os adaptadores de rede usados para comunicação do cliente devem estar na parte superior da ordem de associação. Redes não roteadas podem ser colocadas em prioridade mais baixa. No Windows Server 2012 e no Windows Server2012 R2, o driver de rede de cluster (NETFT. SYS) é colocado automaticamente na parte inferior da lista de ordem de associação.

#### <a name="check-the-validate-network-communication"></a>Verificar a comunicação de rede de validação

A latência na sua rede também poderia fazer com que isso aconteça. Os pacotes não podem ser perdidos entre os nós, mas eles podem não chegar aos nós rápido o suficiente antes que o período de tempo limite expire.

Esse teste valida que os servidores testados podem se comunicar com latência aceitável em todas as redes.

Por exemplo: em validar comunicação de rede, você poderá ver as mensagens a seguir para problemas de latência de rede.

```console
Succeeded in pinging network interface node003.contoso.com IP Address 192.168.0.2 from network interface node004.contoso.com IP Address 192.168.0.3 with maximum delay 500 after 1 attempt(s).
Either address 10.0.0.96 is not reachable from 192.168.0.2 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node003.contoso.com - Heartbeat Network and node004.contoso.com - Production Network are on different cluster networks
Either address 192.168.0.2 is not reachable from 10.0.0.96 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node004.contoso.com - Production Network and node003.contoso.com - Heartbeat Network for MSCS are on different cluster networks
```

Para o cluster multissite, você pode aumentar os valores de tempo limite. Para obter mais informações, consulte [definir configurações de pulsação e DNS em um cluster de failover de vários sites](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197562(v=ws.10)?redirectedfrom=MSDN).

Verifique com o ISP se há problemas de conectividade de WAN.

Verifique se você encontrou qualquer um dos problemas a seguir.

##### <a name="network-packets-lost-between-nodes"></a>Pacotes de rede perdidos entre nós

1. Verificar a perda de pacotes usando o desempenho

    Se o pacote for perdido na conexão em algum lugar entre os nós, as pulsações falharão. Podemos descobrir facilmente se esse é um problema usando o monitor de desempenho para examinar o contador "rede Interface\Packets recebida descartada". Depois de adicionar esse contador, examine os números médio, mínimo e máximo e, se eles forem qualquer valor maior que zero, o buffer de recebimento precisará ser ajustado para o adaptador.

    ![Adicionar Contadores](media/troubleshooting-cluster-event-id-1135/18652.png)

    Se você estiver tendo um pacote de rede perdido na plataforma de virtualização VmWare, consulte a seção "cluster instalado na plataforma de virtualização VmWare".

2. Atualizar os drivers da NIC

    Esse problema pode ocorrer devido a componentes de NIC drivers\Integration (IC) desatualizados \VmTools ou adaptadores NIC com falha.
    Se houver pacotes de rede perdidos entre nós em computadores físicos, tenha as atualizações de driver do adaptador de rede. Drivers de placa de rede antigos ou desatualizados e/ou firmware.
    Às vezes, uma simples configuração incorreta da placa de rede ou do comutador também pode causar perda de pulsações.

##### <a name="cluster-installed-in-the-vmware-virtualization-platform"></a>Cluster instalado na plataforma de virtualização VmWare

Verifique os problemas do adaptador VMware no caso do ambiente VMware. 

Esse problema pode ocorrer se os pacotes forem removidos durante altas intermitências de tráfego. Verifique se não há nenhuma filtragem de tráfego ocorrendo (por exemplo, com um filtro de email). Depois de eliminar essa possibilidade, Aumente gradualmente o número de buffers no sistema operacional convidado e verifique.

Para reduzir os descartes de tráfego de intermitência, siga estas etapas:

1. Abra a caixa de execução usando a tecla Windows + R.
2. Digite devmgmt. msc e pressione **Enter**.
3. Expandir **adaptadores de rede**  
4. Clique com o botão direito do mouse em **vmxnet3 e clique em Propriedades.**  
5. Clique na guia **Avançado**.
6. Clique em **buffers RX pequenos** e aumente o valor. O valor padrão é 512 e o máximo é 8192.
7. Clique em **RX Ring #1** tamanho e aumente o valor. O valor padrão é 1024 e o máximo é 4096.

Verifique as URLs a seguir para verificar os problemas do adaptador do VMware no caso do ambiente VMware:

- [Nós sendo removidos da Associação de cluster de failover no VMware ESX?](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx).

- [Grande perda de pacotes no nível do sistema operacional convidado no VMXNET3 vNIC no ESXi](https://kb.vmware.com/s/article/2039495)

##### <a name="noticed-any-network-congestion"></a>Percebido qualquer congestionamento de rede

O congestionamento de rede também pode causar problemas de conectividade de rede.

Verifique se sua rede está configurada de acordo com as recomendações da MS e do fornecedor, consulte [Configurando redes de cluster de failover do Windows](/archive/blogs/askcore/configuring-windows-failover-cluster-networks)

##### <a name="check-the-network-configuration"></a>Verificar a configuração de rede

Se ainda não funcionar, verifique se você viu a rede particionada na GUI do cluster ou se você tem o agrupamento NIC habilitado na NIC de pulsação.

Se você vir a rede particionada na GUI do cluster, consulte [redes de cluster "particionadas"](/archive/blogs/askcore/partitioned-cluster-networks) para solucionar o problema.

Se você tiver o agrupamento NIC habilitado na NIC de pulsação, verifique a funcionalidade do software de agrupamento de acordo com a recomendação do fornecedor de agrupamento.

##### <a name="upgrade-the-nic-drivers"></a>Atualizar os drivers da NIC

Esse problema pode ocorrer devido a drivers de NIC desatualizados ou adaptadores de NIC com falha.

Se houver pacotes de rede perdidos entre nós em máquinas físicas, tenha as atualizações do driver do adaptador de rede. Drivers de placa de rede antigos ou desatualizados e/ou firmware.

Às vezes, uma simples configuração incorreta da placa de rede ou do comutador também pode causar perda de pulsações.

##### <a name="check-the-network-configuration"></a>Verificar a configuração de rede

Se ainda não funcionar, verifique se você viu rede particionada na GUI do cluster ou se você tem o agrupamento NIC habilitado na NIC de pulsação.
