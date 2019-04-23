---
title: Centros de dados com redundância geográfica do RDS no Azure
description: Saiba como criar uma implantação do RDS que usa vários data centers para fornecer alta disponibilidade em locais geográficos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 7d895b1098c4d8cdf162c77f35209b7308872d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849957"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>Criar um centro com redundância geográfica, vários data de implantação do RDS para recuperação de desastres

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode habilitar a recuperação de desastre para sua implantação de serviços de área de trabalho remota, aproveitando a vários data centers no Azure. Ao contrário de uma implantação de RDS altamente disponível padrão (conforme descrito na [arquitetura dos serviços de área de trabalho remota](desktop-hosting-logical-architecture.md)), que usa os centros de dados em uma única região do Azure (por exemplo, na Europa Ocidental), uma implantação de vários data centers usa dados centros em várias localizações geográficas, aumentando a disponibilidade da sua implantação - um data center do Azure podem não estar disponíveis, mas é improvável que várias regiões deve ir para baixo ao mesmo tempo. Ao implantar uma arquitetura RDS com redundância geográfica, você pode habilitar o failover no caso de falha catastrófica de uma região inteira.

Você pode usar as instruções abaixo para aproveitar os serviços de infraestrutura do Microsoft Azure e RDS para entregar licenças de acesso de assinante (SALs) e serviços de hospedagem de desktop com redundância geográfica para vários locatários por meio de [provedor de serviços da Microsoft Programa de licenciamento por SPLA (contrato)](https://www.microsoft.com/hosting/licensing/splabenefits.aspx). Você também pode usar as etapas abaixo para criar um serviço de hospedagem com redundância geográfica para seus próprios funcionários usando [RDS CALs de usuário estendido direitos por meio do Software Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>Arquitetura lógica para alta disponibilidade - única e várias regiões
A imagem a seguir mostra a arquitetura para uma implantação altamente disponível em uma única região do Azure:

![Uma implantação altamente disponível em uma única região do Azure](media/rds-ha-single-region.png)

A implantação consiste em três camadas:

- Serviços do Azure – as interfaces de gerenciamento do Azure, incluindo o portal do Azure e APIs e serviços de rede públicos, como DNS e endereçamento de IP público.
- Serviço - máquinas virtuais, redes, armazenamento, serviços do Azure e serviços de função do Windows Server de hospedagem de área de trabalho
- O Azure Fabric - sistemas de operacionais do Windows Server que executa a função Hyper-V, usada para a virtualização de servidores físicos, unidades de armazenamento, comutadores de rede e roteadores. Usando o Azure Fabric lhe permite criar máquinas virtuais, redes, armazenamento e aplicativos independentes do hardware subjacente.


Em comparação, aqui está a arquitetura para uma implantação que usa vários data centers do Azure:

![Uma implantação do RDS que usa várias regiões do Azure](media/rds-ha-multi-region.png)

Toda a implantação do RDS é replicada em uma segunda região do Azure para criar uma implantação com redundância geográfica. Essa arquitetura usa um modelo ativo-passivo, onde apenas uma implantação de RDS está sendo executado por vez. Uma conexão de rede virtual para rede virtual permite que os dois ambientes se comunicam entre si. As implantações de RDS são com base em um domínio/floresta de Active Directory único e os servidores do AD replicam entre as duas implantações, os usuários de significado podem entrar em qualquer implantação usando as mesmas credenciais. As configurações de usuário e dados armazenados em discos de perfil do usuário (UPD) são armazenados em um servidor de arquivos de escalabilidade horizontal de espaços de armazenamento diretos (S2D) de cluster de dois nós (SOFS). Um segundo cluster S2D idêntico é implantado na segunda região (passivo) e a réplica de armazenamento é usada para replicar os perfis de usuário do ativo para passiva implantação. O Azure Traffic Manager é usado para direcionar automaticamente os usuários finais a qualquer implantação está ativa no momento – da perspectiva do usuário final, a implantação usando uma única URL de acesso e não estão cientes de qual região eles acabam usando.


Você *poderia* criar uma implantação do RDS não altamente disponível em cada região, mas se até mesmo uma única VM for reiniciada em uma região, ocorrerá um failover, aumentando a probabilidade de failovers ocorrendo com associados impactos no desempenho.

## <a name="deployment-steps"></a>Etapas de implantação
Crie os seguintes recursos no Azure para criar uma implantação do RDS vários data centers com redundância geográfica:

1. Separam os dois grupos de recursos em duas regiões do Azure. Por exemplo A RG (a implantação ativa, o RG significa "grupo de recursos") e B de RG (implantação passiva).
2. Uma implantação altamente disponível do Active Directory no RG A. Você pode usar o [novo domínio do AD com o modelo de controladores de domínio 2](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) para criar a implantação.
3. Uma implantação de RDS altamente disponível no RG A. Use o [RDS usando o active directory existente de implantação de farm](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) modelo para criar a implantação de RDS básica e, em seguida, siga as informações em [Remote Desktop Services - alta disponibilidade](rds-plan-high-availability.md) para configurar os outros componentes RDS para alta disponibilidade.
4. Uma rede virtual no RG B - Certifique-se de usar um espaço de endereço que não se sobreponha a implantação no RG A.
5. Um [conexão de VNet para VNet](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) entre os grupos de dois recursos.
6. Duas máquinas de virtuais do AD em uma conjunto de disponibilidade no RG B - Verifique se os nomes de VM são diferentes das VMs no RG A. Deploy duas VMs de 2016 de Windows Server em um único de disponibilidade definido, instalar a função Serviços de domínio do Active Directory e, em seguida, promovê-los para a continuação do domínio do AD rolo de tinta no domínio que você criou na etapa 1.
7. Uma segunda implantação RDS altamente disponível no RG B. 
   1. Use o [RDS usando o active directory existente de implantação de farm](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) modelo novamente, mas desta vez faça as seguintes alterações. (Para personalizar o modelo, selecione-o na galeria, clique em **implantar no Azure** e, em seguida **Editar modelo**.)
      1. Ajustar o espaço de endereço de IP privado de servidor DNS para corresponder à VNet no RG B. 
      
         Pesquise por "dnsServerPrivateIp" nas variáveis. Editar o padrão IP (10.0.0.4) para corresponder ao espaço de endereço definido na rede virtual no RG B.
   
      2. Edite os nomes de computador para que eles não entrem em conflito com aquelas em que a implantação no RG A.
      
         Localize as VMs na **recursos** seção do modelo. Alterar o **NomeDoComputador** campo sob **osProfile**. Por exemplo, "gateway" pode se tornar "gateway **-b**"; "[concat ('rdsh-', copyIndex())]" pode se tornar "[concat ('rdsh - b-', copyIndex())]" e "agente" pode se tornar "broker **-b**".
      
         (Você também pode alterar os nomes das VMs manualmente depois de executar o modelo.)
   2. Como na etapa 3 acima, use as informações em [serviços de área de trabalho remota - alta disponibilidade](rds-plan-high-availability.md) para configurar os outros componentes RDS para alta disponibilidade.
8. Espaços de armazenamento diretos expansão servidor de arquivos com a réplica de armazenamento entre as duas implantações. Use o [script do PowerShell](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts) para implantar o [modelo](https://github.com/robotechredmond/301-s2d-sr-dr-md) entre os grupos de recursos.

   > [!NOTE]
   > Você pode provisionar armazenamento manualmente (em vez de usar o script do PowerShell e o modelo): 
   >1. Implantar um [SOFS de S2D de 2 nós](rds-storage-spaces-direct-deployment.md) no RG A armazenar seus discos de perfil do usuário (UPDs).
   >2. Implantar um segundo, idêntico SOFS de S2D no RG B - Certifique-se de usar a mesma quantidade de armazenamento em cada cluster.
   >3. Configure [a réplica de armazenamento com a replicação assíncrona](../../storage/storage-replica/cluster-to-cluster-storage-replication.md) entre os dois.

### <a name="enable-upds"></a>Habilitar UPDs
A réplica de armazenamento replica dados de um volume de origem (associado com a implantação primária/ativa) para um volume de destino (associado com a implantação secundária/passivo). Por design, o cluster de destino é exibida como **Online (sem acesso)** -a réplica de armazenamento desmonta os volumes de destino e seus pontos de montagem ou letras de unidade. Isso significa que a habilitação UPDs para a implantação secundária, fornecendo o caminho do compartilhamento de arquivo falhará, pois o volume não está montado. 

Quer saber mais sobre como gerenciar a replicação? Fazer check-out [replicação de armazenamento de cluster para Cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Para habilitar os UPDs em ambas as implantações, faça o seguinte:

1. Execute o [cmdlet Set-RDSessionCollectionConfiguration](https://technet.microsoft.com/itpro/powershell/windows/remote-desktop/set-rdsessioncollectionconfiguration) para habilitar os discos de perfil do usuário para a implantação primária (ativa) – forneça um caminho para o compartilhamento de arquivos no volume de origem (o que você criou na etapa 7 nas etapas de implantação).
2. Inverter a direção da réplica de armazenamento para que o volume de destino se torne o volume de origem (Isso monta o volume e o torna acessível pela implantação secundária). Você pode executar **Set-SRPartnership** para fazer isso. Por exemplo: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. Habilite os discos de perfil do usuário na implantação secundário (passivo). Use as mesmas etapas, assim como para a implantação primária, na etapa 1.
4. Inverter a direção da réplica de armazenamento novamente, para que o volume de origem original é novamente o volume de origem na parceria SR, e a implantação primária pode acessar o compartilhamento de arquivos. Por exemplo: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Gerenciador de Tráfego do Azure 

Criar uma [Gerenciador de tráfego do Azure](/azure/traffic-manager/traffic-manager-overview) de perfil e certifique-se de selecionar o **prioridade** método de roteamento. Defina dois pontos de extremidade para os endereços IP públicos de cada implantação. Sob **configuração**, alterar o protocolo HTTPS (em vez de HTTP) e a porta 443 (em vez de 80). Anote o **vida útil do DNS**e defini-lo adequadamente para as necessidades de seu failover. 

Observe que o Gerenciador de tráfego requer que os pontos de extremidade para retornar 200 Okey em resposta a uma solicitação GET para ser marcado como "Íntegro". O objeto de publicIP criado a partir de modelos RDS funcionará, mas não adicione um adendo do caminho. Em vez disso, você pode dar aos usuários finais a URL do Gerenciador de tráfego com "/ RDWeb" acrescentado, por exemplo: ```http://deployment.trafficmanager.net/RDWeb```

Implantando o Gerenciador de tráfego do Azure com o método de roteamento de prioridade, impedir que os usuários finais acessem a implantação passiva durante a implantação do Active Directory é funcional. Se os usuários finais acessar a implantação passiva e a direção de réplica de armazenamento ainda não foi alternada para o failover, a entrada do usuário para de responder conforme a implantação tenta e não pode acessar o compartilhamento de arquivos no cluster do S2D passivo - eventualmente a implantação será desista e dê o usuário a um perfil temporário.  

### <a name="deallocate-vms-to-save-resources"></a>Desalocar as VMs para salvar os recursos 
Depois de configurar ambas as implantações, você pode, opcionalmente, desligar e desalocar as VMs de RDSH economize nessas máquinas virtuais e infraestrutura de RDS de secundário. O servidor de SOFS de S2D e o AD VMs sempre deve permanecer em execução na implantação secundária/passivo para habilitar a sincronização de conta e perfil do usuário.  

Quando ocorre um failover, você precisará iniciar as VMs desalocadas. Essa configuração de implantação tem a vantagem de ser um custo menor, mas às custas do tempo de failover. Se ocorrer uma falha catastrófica na implantação do Active Directory, você precisará iniciar manualmente a implantação passiva, ou você precisará de um script de automação para detectar a falha e iniciar a implantação passiva automaticamente. Em ambos os casos, pode levar vários minutos para obter a implantação passiva em execução e disponíveis para os usuários a entrar, resultando em algum tempo de inatividade para o serviço. Esse tempo de inatividade depende da quantidade de tempo ele leva para iniciar a infraestrutura RDS e VMs de RDSH (normalmente 2 a 4 minutos, se as VMs são iniciadas em paralelo em vez de em série) e o tempo para colocar o cluster passivo online (que depende do tamanho do cluster normalmente 2 a 4 minutos para um cluster de 2 nós com 2 discos por nó). 

### <a name="active-directory"></a>Active Directory 
Os servidores do Active Directory em cada implantação são as réplicas dentro do mesmo domínio/floresta. O Active Directory tem um protocolo de sincronização interna para manter os controladores de domínio de quatro em sincronia. No entanto, pode haver algum atraso para que se um novo usuário é adicionado a um servidor do AD, pode levar algum tempo para serem replicadas em todos os servidores do AD em duas implantações. Consequentemente, certifique-se de avisar os usuários para não tentar entrar imediatamente depois de serem adicionados ao domínio. 

### <a name="rd-license-server"></a>Servidor de licenças de área de trabalho remota 
Fornecer um [CAL de área de trabalho remota por usuário](rds-client-access-license.md) para cada usuário nomeado que está autorizado a acessar a implantação com redundância geográfica. Distribuir o por usuário CALs uniformemente entre os dois servidores de licenças de área de trabalho remota na implantação do Active Directory. Em seguida, duplicar essas CALs para dois servidores de licenças de área de trabalho remota na implantação passiva. Porque as CALs são duplicadas entre a implantação de ativa e passiva em um determinado momento, apenas uma implantação pode estar ativa a usuários que se conectam; Caso contrário, você pode violar o contrato de licença.  

### <a name="image-management"></a>Gerenciamento de imagens 
Conforme você atualiza as imagens RDSH para fornecer atualizações de software ou novos aplicativos, você precisará atualizar separadamente as coleções de RDSH em cada implantação para manter uma experiência de usuário comum entre ambas as implantações. Você pode usar o [modelo de coleção de RDSH atualização](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/), mas observe que a implantação passiva infraestrutura de RDS e RDSH VMs devem estar executando para executar o modelo. 

## <a name="failover"></a>Failover

No caso de implantação ativa-passiva, o failover exige que você iniciar as VMs da implantação secundária. Você pode fazer isso manualmente ou com um script de automação. No caso de um failover catastrófico do SOFS de S2D, altere a direção da parceria de réplica de armazenamento, para que o volume de destino se torna o volume de origem. Por exemplo: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

Você pode aprender mais em [replicação de armazenamento de cluster para Cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

O Gerenciador de tráfego do Azure reconhece automaticamente que a implantação primária falha e que a implantação secundária esteja íntegra (em que as VMs de Gateway de área de trabalho remota foram iniciados no RG B) e direciona o tráfego de usuário para a implantação secundária. Os usuários podem usar a mesma URL do Gerenciador de tráfego para continuar a trabalhar em seus recursos remotos, aproveitando uma experiência consistente. Observe que o cliente de cache DNS não atualizará o registro durante o TTL definido na configuração do Gerenciador de tráfego do Azure.

### <a name="test-failover"></a>Failover de teste
Em uma parceria de réplica de armazenamento, apenas um volume (a origem) pode estar ativo por vez. Isso significa que quando você alternar a direção de parceria SR, o volume na implantação principal (RG A) se torna o destino de replicação e, portanto, está oculto. Portanto, quaisquer usuários que se conectam a RG A não terá mais acesso aos seus UPDs armazenados no SOFS no RG A. 

Para testar o failover, permitindo que os usuários continuem a fazer logon:
1. Iniciar as VMs de RDSH e uma infraestrutura de VMs no RG B.
2. Alternar a direção de parceria SR (cluster-b-s2d-c se torna o volume de origem).
3. [Desabilitar ponto de extremidade](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint) de RG A no perfil do Gerenciador de tráfego do Azure para forçar o ATM para direcionar o tráfego para RG B. como alternativa, use um script do PowerShell:

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B é agora a implantação primária ativa. Para alternar novamente para A RG como a implantação primária:

1. Alternar a direção de parceria SR (cluster a s2d c se torna o volume de origem):

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. Habilite novamente o ponto de extremidade de RG A no perfil do Gerenciador de tráfego do Azure:

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>Considerações para implantações locais

Embora uma implantação de local não foi possível usar os modelos de início rápido do Azure mencionado neste artigo, você pode implementar todas as funções de infraestrutura manualmente. Em uma implantação de local em que o custo não é orientado pelo consumo do Azure, considere o uso de um modelo ativo-ativo para o failover mais rápido.

Você pode usar o Gerenciador de tráfego do Azure com pontos de extremidade local, mas ela requer uma assinatura do Azure. Como alternativa, para o DNS fornecido aos usuários finais, dê a eles um registro CNAME que simplesmente direciona os usuários para a implantação primária. No caso de failover, modifique o registro DNS CNAME para redirecionar para a implantação secundária. Dessa forma, o usuário final usa uma única URL, assim como com o Azure Traffic Manager, que direciona o usuário para a implantação apropriada. 

Se você estiver interessado na criação de um modelo no local-Azure-site, considere o uso [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).
