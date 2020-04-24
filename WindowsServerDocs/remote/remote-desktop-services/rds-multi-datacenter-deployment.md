---
title: Data centers do RDS com redundância geográfica no Azure
description: Saiba como criar uma implantação do RDS que usa vários data centers para fornecer alta disponibilidade em locais geográficos.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 5c0f5d6937a79f36df264597400fe71af3f3779b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855589"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>Criar uma implantação do RDS em vários data centers com redundância geográfica para recuperação de desastres

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

É possível habilitar a recuperação de desastres na implantação dos Serviços de Área de Trabalho Remota ao utilizar vários data centers no Azure. Ao contrário da implantação do RDS altamente disponível padrão (como descrito na [Arquitetura dos Serviços de Área de Trabalho Remota](desktop-hosting-logical-architecture.md)), que usa os data centers em uma única região do Azure (por exemplo, na Europa Ocidental), uma implantação desse tipo usa data centers em vários locais geográficos, aumentando a disponibilidade da sua implantação: um data center do Azure pode não estar disponível, mas é improvável que várias regiões fiquem indisponíveis ao mesmo tempo. Ao implantar uma arquitetura do RDS com redundância geográfica, habilite o failover no caso de falha catastrófica de uma região inteira.

Use as instruções abaixo para aproveitar os serviços de infraestrutura do Microsoft Azure e do RDS para fornecer serviços de hospedagem de área de trabalho redundantes e SALs (licenças de acesso ao assinante) para vários locatários por meio do [programa SPLA (contrato de licença do provedor de serviços) da Microsoft](https://www.microsoft.com/hosting/licensing/splabenefits.aspx). Também é possível usar as etapas abaixo para criar um serviço de hospedagem com redundância geográfica para seus próprios funcionários usando [Direitos estendidos de CALs do usuário do RDS por meio do Software Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>Arquitetura lógica para alta disponibilidade: região única e várias regiões
A imagem a seguir mostra a arquitetura para uma implantação altamente disponível em uma única região do Azure:

![Uma implantação altamente disponível em uma única região do Azure](media/rds-ha-single-region.png)

A implantação consiste em três camadas:

- Serviços do Azure: as interfaces de gerenciamento do Azure, incluindo as APIs e o portal do Azure, e os serviços de rede públicos, como DNS e endereçamento de IP público.
- Serviço de hospedagem de área de trabalho: máquinas virtuais, redes, armazenamento, serviços do Azure e serviços de função do Windows Server
- Azure Fabric: sistemas operacionais Windows Server que executam a função Hyper-V, usada para virtualizar servidores físicos, unidades de armazenamento, comutadores de rede e roteadores. Usar o Azure Fabric possibilita criar máquinas virtuais, redes, armazenamento e aplicativos independentes do hardware subjacente.


Como comparação, esta é a arquitetura de uma implantação que usa vários data centers do Azure:

![Uma implantação do RDS que usa várias regiões do Azure](media/rds-ha-multi-region.png)

Toda a implantação do RDS é replicada em uma segunda região do Azure para criar uma implantação com redundância geográfica. Essa arquitetura usa um modelo ativo-passivo, em que apenas uma implantação de RDS é executada por vez. Uma conexão de rede virtual para rede virtual permite que os dois ambientes se comuniquem entre si. As implantações do RDS são baseadas em um domínio/floresta do Active Directory único e os servidores do AD replicam entre as duas implantações, isso significa que os usuários podem entrar em qualquer implantação usando as mesmas credenciais. As configurações de usuários e os dados em UDP (discos de perfil de usuário) são armazenados em um SOFS (servidor de arquivos de escalabilidade horizontal) de Espaços de Armazenamento Diretos de cluster de dois nós. Um segundo cluster de Espaços de Armazenamento Diretos idêntico é implantado na segunda região (passiva) e a Réplica de Armazenamento é usada para replicar os perfis de usuários da implantação ativa para passiva. O Gerenciador de Tráfego do Microsoft Azure é usado para direcionar automaticamente os usuários finais à implantação que está ativa no momento, da perspectiva do usuário final. A implantação é acessada usando uma única URL e os usuários não ficam cientes de qual região estão usando.


Você *poderia* criar uma implantação do RDS não altamente disponível em cada região, mas se até mesmo uma única VM for reiniciada em uma região, haverá um failover, aumentando a probabilidade de failovers ocorrendo com impactos associados no desempenho.

## <a name="deployment-steps"></a>Etapas de implantação
Crie os seguintes recursos no Azure para criar uma implantação do RDS de vários data centers com redundância geográfica:

1. Dois grupos de recursos em duas regiões separadas do Azure. Por exemplo o GR A (a implantação ativa, RG significa "grupo de recursos") e o GR B (a implantação passiva).
2. Uma implantação altamente disponível do Active Directory no GR A. Use o [Novo domínio do AD com dois modelos de controladores de domínio](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) para criar a implantação.
3. Uma implantação do RDS altamente disponível no GR A. Use o modelo de [implantação de farm do RDS usando o Active Directory existente](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) para criar a implantação do RDS básica e, em seguida, siga as informações em [Serviços de Área de Trabalho Remota: alta disponibilidade](rds-plan-high-availability.md) para configurar os outros componentes do RDS para alta disponibilidade.
4. Uma rede virtual no GR B, certifique-se de usar um espaço de endereço que não se sobreponha à implantação no GR A.
5. Uma [conexão de rede virtual para rede virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) entre os dois grupos de recursos.
6. Duas máquinas virtuais do AD em um conjunto de disponibilidade no GR B, verifique se os nomes das VMs são diferentes das VMs do AD no GR A. Implante duas VMs com Windows Server 2016 em um único conjunto de disponibilidade, instale a função Active Directory Domain Services e, em seguida, promova-os para o controlador de domínio no domínio que você criou na etapa 1.
7. Uma segunda implantação do RDS altamente disponível no GR B. 
   1. Use o modelo de [implantação de farm do RDS usando o Active Directory existente](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) novamente, mas desta vez faça as seguintes alterações. (Para personalizar o modelo, selecione-o na galeria, clique em **Implantar no Azure** e, em seguida **Editar modelo**.)
      1. Ajuste o espaço de endereço do IP privado de servidor DNS para corresponder à rede virtual no GR B. 
      
         Pesquise nas variáveis por "dnsServerPrivateIp". Edite o IP padrão (10.0.0.4) para corresponder ao espaço de endereço definido na rede virtual no GR B.
   
      2. Edite os nomes dos computadores para que não entrem em conflito com os nomes da implantação no GR A.
      
         Localize as VMs na seção **Resources** do modelo. Altere o campo **computerName** em **osProfile**. Por exemplo, "gateway" pode se tornar "gateway **-b**"; "[concat ('rdsh-', copyIndex())]" pode se tornar "[concat ('rdsh-b-', copyIndex())]" e "broker" pode se tornar "broker **-b**".
      
         (Também é possível alterar os nomes das VMs manualmente após executar o modelo.)
   2. Como na etapa 3 acima, use as informações em [Serviços de Área de Trabalho Remota: alta disponibilidade](rds-plan-high-availability.md) para configurar os outros componentes do RDS para alta disponibilidade.
8. Um servidor de arquivos de escalabilidade horizontal de Espaços de Armazenamento Diretos com Réplica de Armazenamento entre as duas implantações. Use o [script do PowerShell](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts) para implantar o [modelo](https://github.com/robotechredmond/301-s2d-sr-dr-md) entre os grupos de recursos.

   > [!NOTE]
   > Você pode provisionar o armazenamento manualmente (em vez de usar o modelo e o script do PowerShell): 
   >1. Implante um [SOFS de Espaços de Armazenamento Diretos de dois nós](rds-storage-spaces-direct-deployment.md) no GR A para armazenar seus UPDs (discos de perfil do usuário).
   >2. Implante um segundo SOFS de Espaços de Armazenamento Diretos idêntico no GR B, certifique-se de usar a mesma quantidade de armazenamento em cada cluster.
   >3. Configure a [Réplica de Armazenamento com replicação assíncrona](../../storage/storage-replica/cluster-to-cluster-storage-replication.md) entre os dois.

### <a name="enable-upds"></a>Habilitar UPDs
A Réplica de Armazenamento replica os dados de um volume de origem (associado com a implantação primária/ativa) para um volume de destino (associado com a implantação secundária/passiva). Por design, o cluster de destino é exibido como **Online (sem acesso)** , a Réplica de Armazenamento desmonta os volumes de destino e seus pontos de montagem ou letras de unidade. Isso significa que habilitar UPDs para a implantação secundária fornecendo o caminho do compartilhamento de arquivos falhará, pois o volume não está montado. 

Quer saber mais sobre como gerenciar a replicação? Confira [Replicação de armazenamento de cluster para cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Para habilitar os UPDs em ambas as implantações, faça o seguinte:

1. Execute o [cmdlet Set-RDSessionCollectionConfiguration](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) para habilitar os discos de perfil do usuário na implantação primária (ativa), forneça um caminho para o compartilhamento de arquivos no volume de origem (criado na etapa 7 das etapas de implantação).
2. Inverta a direção da Réplica de Armazenamento para que o volume de destino se torne o volume de origem (isso monta o volume e o torna acessível pela implantação secundária). Para tanto, execute o cmdlet **Set-SRPartnership**. Por exemplo:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. Habilite os discos de perfil do usuário na implantação secundária (passiva). Use as mesmas etapas realizadas na implantação primária, na etapa 1.
4. Inverta novamente a direção da Réplica de Armazenamento, assim o volume de origem original é novamente o volume de origem na parceria SR, e a implantação primária pode acessar o compartilhamento de arquivos. Por exemplo:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Gerenciador de Tráfego do Azure 

Crie um perfil no [Gerenciador de Tráfego do Azure](/azure/traffic-manager/traffic-manager-overview) e certifique-se de selecionar o método de roteamento **Prioridade**. Defina os dois pontos de extremidade para os endereços IP públicos de cada implantação. Em **Configuração**, altere o protocolo para HTTPS (em vez de HTTP) e a porta para 443 (em vez de 80). Observe a **Vida útil do DNS** e defina-a de acordo com suas necessidades de failover. 

Note que o Gerenciador de Tráfego exige que os pontos de extremidade retornem 200 OK em resposta a uma solicitação GET para ser marcado como "íntegro". O objeto publicIP criado a partir de modelos do RDS funcionará, mas não adicione um adendo de caminho. Em vez disso, forneça aos usuários finais a URL do Gerenciador de Tráfego acrescida de "/RDWeb", por exemplo: ```http://deployment.trafficmanager.net/RDWeb```

Ao implantar o Gerenciador de Tráfego do Azure com o método de roteamento Prioridade, você impede que os usuários finais acessem a implantação passiva enquanto a implantação ativa estiver funcionando. Se os usuários finais acessarem a implantação passiva e a direção da Réplica de Armazenamento ainda não tiver sido alternada para o failover, a entrada do usuário para de responder conforme a implantação tenta e não consegue acessar o compartilhamento de arquivos no cluster dos Espaços de Armazenamento Diretos passivo e, eventualmente, a implantação desiste e fornece ao usuário um perfil temporário.  

### <a name="deallocate-vms-to-save-resources"></a>Desalocar as VMs para economizar recursos 
Após configurar ambas as implantações, você pode, opcionalmente, desligar e desalocar a infraestrutura secundária de RDS e as VMs de RDSH para economizar com estas VMs. O SOFS dos Espaços de Armazenamento Diretos e as VMs do servidor AD sempre devem permanecer em execução na implantação secundária/passiva para habilitar a sincronização de conta e perfil do usuário.  

Quando ocorrer um failover, você precisará iniciar as VMs desalocadas. Essa configuração de implantação tem a vantagem de um custo menor, mas um tempo de failover maior. Se ocorrer uma falha catastrófica na implantação ativa, você precisará iniciar manualmente a implantação passiva, ou precisará de um script de automação para detectar a falha e iniciar a implantação passiva automaticamente. Em ambos os casos, pode levar vários minutos para colocar a implantação passiva em execução e deixá-la disponível para os usuários, resultando em algum tempo de inatividade para o serviço. Esse tempo de inatividade depende da quantidade de tempo que leva para iniciar a infraestrutura do RDS e as VMs de RDSH (normalmente dois a quatro minutos, se as VMs forem iniciadas em paralelo em vez de em série) e o tempo para colocar o cluster passivo online (que depende do tamanho do cluster, normalmente dois a quatro minutos para um cluster de dois nós com dois discos por nó). 

### <a name="active-directory"></a>Active Directory 
Os servidores do Active Directory em cada implantação são réplicas dentro do mesmo domínio/floresta. O Active Directory tem um protocolo de sincronização interno para manter os quatro controladores de domínio em sincronia. No entanto, pode haver algum atraso, assim, se um novo usuário for adicionado a um servidor do AD, pode levar algum tempo para ser replicado em todos os servidores do AD nas duas implantações. Consequentemente, certifique-se de avisar os usuários para não tentarem entrar imediatamente depois de serem adicionados ao domínio. 

### <a name="rd-license-server"></a>Servidor de licença RD 
Forneça uma [CAL do RD por usuário](rds-client-access-license.md) para cada usuário nomeado que está autorizado a acessar a implantação com redundância geográfica. Distribua as CALs por usuários de forma uniforme entre os dois servidores de licenças RD na implantação do ativa. Em seguida, duplique essas CALs nos dois servidores de licenças RD na implantação passiva. Como as CALs são duplicadas entre a implantação ativa e passiva, somente uma implantação pode estar ativa com a conexão dos usuários em qualquer momento; caso contrário, você violará o contrato de licença.  

### <a name="image-management"></a>Gerenciamento de imagens 
Ao atualizar as imagens de RDSH para fornecer atualizações de software ou novos aplicativos, você precisa atualizar separadamente as coleções de RDSH em cada implantação para manter uma experiência de usuário comum entre ambas as implantações. É possível usar a opção [Atualizar modelo de coleção de RDSH](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/), mas observe que a infraestrutura de RDS e as VMs de RDSH da implantação passiva devem estar em execução para executar o modelo. 

## <a name="failover"></a>Failover

No caso de implantação ativa-passiva, o failover exige que você inicie as VMs da implantação secundária. Você pode fazer isso manualmente ou com um script de automação. No caso de um failover catastrófico do SOFS dos Espaços de Armazenamento Diretos, altere a direção da parceria de Réplica de Armazenamento,de modo que o volume de destino se torne o volume de origem. Por exemplo:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

Você pode aprender mais em [Replicação de armazenamento de cluster para cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

O Gerenciador de Tráfego do Azure reconhece automaticamente que a implantação primária falhou e que a implantação secundária está íntegra (nas VMs de Gateway de área de trabalho remota que foram iniciadas no GR B) e direciona o tráfego de usuários para a implantação secundária. Os usuários podem usar a mesma URL do Gerenciador de Tráfego para continuar a trabalhar em seus recursos remotos, desfrutando de uma experiência consistente. Observe que o cache DNS do cliente não atualizará o registro durante o TTL definido na configuração do Gerenciador de Tráfego do Azure.

### <a name="test-failover"></a>Failover de teste
Em uma parceria de Réplica de Armazenamento, apenas um volume (a origem) pode estar ativo por vez. Isso significa que ao alternar a direção de parceria SR, o volume na implantação principal (GR A) torna-se o destino de replicação, logo, fica oculto. Portanto, qualquer usuário que se conectar ao GR A não terá mais acesso aos seus UPDs armazenados no SOFS no GR A. 

Para testar o failover permitindo que os usuários continuem a fazer logon:
1. Inicie as VMs de RDSH e da infraestrutura no GR B.
2. Mude a direção da Parceria SR (o cluster-b-s2d-c se torna o volume de origem).
3. [Desabilite o ponto de extremidade](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint) de GR A no perfil do Gerenciador de Tráfego do Azure para forçar o ATM a direcionar o tráfego para GR B. Como alternativa, use um script do PowerShell:

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

Agora o GR B é a implantação primária ativa. Para mudar novamente para o GR A como a implantação primária:

1. Mude a direção da Parceria SR (o cluster-a-s2d-c torna-se o volume de origem):

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. Habilite novamente o ponto de extremidade de GR A no perfil do Gerenciador de Tráfego do Azure:

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>Considerações para implantações locais

Embora uma implantação local não possa usar os Modelos de Início Rápido do Azure mencionados neste artigo, é possível implementar todas as funções de infraestrutura manualmente. Em uma implantação local em que o custo não é orientado pelo consumo do Azure, considere o uso de um modelo ativo-ativo para o failover mais rápido.

Você pode usar o Gerenciador de Tráfego do Azure com pontos de extremidade locais, mas isso requer uma assinatura do Azure. Como alternativa, ao DNS fornecido aos usuários finais, forneça a eles um registro CNAME que simplesmente direciona os usuários para a implantação primária. No caso de failover, modifique o registro DNS CNAME para redirecionar para a implantação secundária. Dessa forma, o usuário final usa uma única URL, assim como no Gerenciador de Tráfego do Azure, que direciona o usuário para a implantação apropriada. 

Se você estiver interessado na criação de um modelo local-para site do Azure, considere o uso do [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).
