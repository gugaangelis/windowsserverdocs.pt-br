---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Implantar uma testemunha de nuvem para um cluster de failover
ms.prod: windows-server
manager: lizross
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: Como usar Microsoft Azure para hospedar a testemunha para um cluster de failover do Windows Server na nuvem – também conhecido como implantar uma testemunha em nuvem.
ms.openlocfilehash: 0b4ba643dca81d2d19b94b1d27485149f938e1c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827909"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Implantar uma testemunha de nuvem para um Cluster de Failover

> Aplica-se a: Windows Server 2019, Windows Server 2016

A testemunha em nuvem é um tipo de testemunha de quorum de cluster de failover que usa Microsoft Azure para fornecer um voto no quorum de cluster. Este tópico fornece uma visão geral do recurso de testemunha em nuvem, os cenários aos quais ele dá suporte e instruções sobre como configurar uma testemunha de nuvem para um cluster de failover.

## <a name="cloud-witness-overview"></a><a name="CloudWitnessOverview"></a>Visão geral da testemunha em nuvem

A Figura 1 ilustra uma configuração de quorum de cluster de failover ampliado de vários sites com o Windows Server 2016. Neste exemplo de configuração (Figura 1), há dois nós em 2 datacenters (chamados de sites). Observe que é possível que um cluster abranja mais de 2 data centers. Além disso, cada datacenter pode ter mais de dois nós. Uma configuração de quorum de cluster típica nesta configuração (SLA de failover automático) fornece a cada nó um voto. Um voto extra é dado à testemunha de quorum para permitir que o cluster continue em execução mesmo se um dos datacenters sofrer uma queda de energia. A matemática é simples-há 5 votos totais e você precisa de três votos para o cluster mantê-lo em execução.  

![Testemunha de compartilhamento de arquivos em um terceiro site separado com 2 nós em dois outros sites](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "Testemunha de Compartilhamento de Arquivos")  
**Figura 1: usando uma testemunha de compartilhamento de arquivos como uma testemunha de quorum**  

No caso de queda de energia em um datacenter, para dar uma oportunidade igual ao cluster em outro datacenter para mantê-lo em execução, é recomendável hospedar a testemunha de quorum em um local diferente dos dois data centers. Isso normalmente significa exigir que um terceiro datacenter (site) separado hospede um servidor de arquivos que está fazendo backup do compartilhamento de arquivos que é usado como a testemunha de quorum (testemunha de compartilhamento de arquivos).  

A maioria das organizações não tem um terceiro datacenter separado que hospedará o servidor de arquivos que fará o backup da testemunha de compartilhamento de arquivos. Isso significa que as organizações hospedam principalmente o servidor de arquivos em um dos dois data centers, que, por extensão, torna esse datacenter o datacenter primário. Em um cenário em que há queda de energia no datacenter primário, o cluster fica inativo, pois o outro datacenter teria apenas dois votos, que está abaixo da maioria de quorum de 3 votos necessários. Para os clientes que têm o terceiro datacenter separado para hospedar o servidor de arquivos, é uma sobrecarga para manter o servidor de arquivos altamente disponível fazendo backup da testemunha de compartilhamento de arquivos. Hospedar máquinas virtuais na nuvem pública que têm o servidor de arquivos para a testemunha de compartilhamento de arquivos em execução no sistema operacional convidado é uma sobrecarga significativa em termos de instalação & manutenção.  

A testemunha em nuvem é um novo tipo de testemunha de quorum de cluster de failover que aproveita Microsoft Azure como ponto de arbitragem (Figura 2). Ele usa o armazenamento de BLOBs do Azure para ler/gravar um arquivo de BLOB que é usado como um ponto de arbitragem em caso de resolução de divisão.  

Há benefícios significativos que essa abordagem:
1. Aproveita Microsoft Azure (sem necessidade de um datacenter separado).  
2. Usa o armazenamento de BLOBs padrão do Azure (sem sobrecarga de manutenção extra de máquinas virtuais hospedadas na nuvem pública).  
3. A mesma conta de armazenamento do Azure pode ser usada para vários clusters (um arquivo de blob por cluster; ID exclusiva de cluster usada como nome de arquivo de BLOB).  
4. $Cost muito baixa na conta de armazenamento (dados muito pequenos gravados por arquivo de BLOB, arquivo de blob atualizado apenas uma vez quando o estado dos nós de cluster é alterado).  
5. Tipo de recurso de testemunha em nuvem interno.  

![diagrama ilustrando um cluster ampliado multissite com testemunha de nuvem como uma testemunha de quorum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: clusters ampliados multissite com testemunha de nuvem como uma testemunha de quorum**  

Como mostra a Figura 2, não há um terceiro site separado necessário. A testemunha em nuvem, como qualquer outra testemunha de quorum, obtém um voto e pode participar de cálculos de quorum.  

## <a name="cloud-witness-supported-scenarios-for-single-witness-type"></a><a name="CloudWitnessSupportedScenarios"></a>Testemunha em nuvem: cenários com suporte para o tipo de testemunha única
Se você tiver uma implantação de cluster de failover, onde todos os nós podem acessar a Internet (por extensão do Azure), é recomendável que você configure uma testemunha de nuvem como seu recurso de testemunha de quorum.  

Alguns dos cenários com suporte para o uso da testemunha de nuvem como uma testemunha de quorum são os seguintes:  
- Os clusters de vários sites ampliados para a recuperação de desastres (consulte a Figura 2).  
- Clusters de failover sem armazenamento compartilhado (SQL Always On etc.).  
- Clusters de failover em execução dentro do sistema operacional convidado hospedado em Microsoft Azure função de máquina virtual (ou qualquer outra nuvem pública).  
- Clusters de failover em execução dentro do sistema operacional convidado de máquinas virtuais hospedadas em nuvens privadas.
- Clusters de armazenamento com ou sem armazenamento compartilhado, como clusters de servidor de arquivos de escalabilidade horizontal.  
- Clusters de filiais pequenas (até mesmo clusters de 2 nós)  

A partir do Windows Server 2012 R2, é recomendável sempre configurar uma testemunha, pois o cluster gerencia automaticamente o voto da testemunha e os nós votam com o quorum dinâmico.  

## <a name="set-up-a-cloud-witness-for-a-cluster"></a><a name="CloudWitnessSetUp"></a>Configurar uma testemunha de nuvem para um cluster
Para configurar uma testemunha de nuvem como uma testemunha de quorum para o cluster, conclua as seguintes etapas:
1. Criar uma conta de armazenamento do Azure para usar como uma testemunha de nuvem
2. Configure a testemunha de nuvem como uma testemunha de quorum para o cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Criar uma conta de armazenamento do Azure para usar como uma testemunha de nuvem

Esta seção descreve como criar uma conta de armazenamento e exibir e copiar URLs de ponto de extremidade e chaves de acesso para essa conta.

Para configurar a testemunha de nuvem, você deve ter uma conta de armazenamento do Azure válida que pode ser usada para armazenar o arquivo de BLOB (usado para arbitragem). A testemunha em nuvem cria um contêiner **MSFT-Cloud-testemunha** bem conhecido na conta de armazenamento da Microsoft. A testemunha em nuvem grava um único arquivo de blob com a ID exclusiva do cluster correspondente usada como o nome de arquivo do arquivo de blob nesse contêiner **MSFT-Cloud-testemunha** . Isso significa que você pode usar a mesma conta de Armazenamento do Microsoft Azure para configurar uma testemunha de nuvem para vários clusters diferentes.

Quando você usa a mesma conta de armazenamento do Azure para configurar a testemunha de nuvem para vários clusters diferentes, um único contêiner **MSFT-Cloud-testemunha** é criado automaticamente. Esse contêiner conterá um arquivo-blob por cluster.

### <a name="to-create-an-azure-storage-account"></a>Para criar uma conta de armazenamento do Azure

1. Entre no [portal do Azure](https://portal.azure.com).
2. No menu Hub, selecione Novo -> Dados + Armazenamento -> Conta de armazenamento.
3. Na página criar uma conta de armazenamento, faça o seguinte:
    1. Insira um nome para sua conta de armazenamento.
    <br>Os nomes da conta de armazenamento devem ter entre 3 e 24 caracteres e podem conter apenas números e letras minúsculas. O nome da conta de armazenamento também deve ser exclusivo no Azure.
        
    2. Para **tipo de conta**, selecione **uso geral**.
    <br>Você não pode usar uma conta de armazenamento de BLOBs para uma testemunha em nuvem.
    3. Para **desempenho**, selecione **padrão**.
    <br>Você não pode usar o armazenamento Premium do Azure para uma testemunha em nuvem.
    2. Para **replicação**, selecione **armazenamento com REDUNDÂNCIA local (LRS)** .
    <br>O clustering de failover usa o arquivo de blob como o ponto de arbitragem, o que requer algumas garantias de consistência ao ler os dados. Portanto, você deve selecionar **armazenamento com redundância local** para o tipo de **replicação** .

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Exibir e copiar chaves de acesso de armazenamento para sua conta de armazenamento do Azure

Quando você cria uma conta de Armazenamento do Microsoft Azure, ela é associada a duas chaves de acesso geradas automaticamente-chave de acesso primária e chave de acesso secundária. Para uma primeira criação de testemunha de nuvem, use a **chave de acesso primária**. Não há nenhuma restrição sobre qual chave usar para a testemunha de nuvem.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Para exibir e copiar chaves de acesso de armazenamento

No portal do Azure, navegue até sua conta de armazenamento, clique em **todas as configurações** e, em seguida, clique em **chaves de acesso** para exibir, copiar e regenerar as chaves de acesso da conta. A folha chaves de acesso também inclui cadeias de conexão pré-configuradas usando as chaves primárias e secundárias que você pode copiar para usar em seus aplicativos (consulte a Figura 4).

![instantâneo da caixa de diálogo gerenciar chaves de acesso no Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figura 4: chaves de acesso de armazenamento**

### <a name="view-and-copy-endpoint-url-links"></a>Exibir e copiar links de URL de ponto de extremidade  
Quando você cria uma conta de armazenamento, as URLs a seguir são geradas usando o formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

A testemunha em nuvem sempre usa o **blob** como o tipo de armazenamento. O Azure usa **. Core.Windows.net** como o ponto de extremidade. Ao configurar a testemunha de nuvem, é possível configurá-la com um ponto de extremidade diferente de acordo com seu cenário (por exemplo, o Microsoft Azure datacenter na China tem um ponto de extremidade diferente).  

> [!NOTE]  
> A URL do ponto de extremidade é gerada automaticamente pelo recurso de testemunha de nuvem e não há nenhuma etapa extra de configuração necessária para a URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Para exibir e copiar links de URL de ponto de extremidade
No portal do Azure, navegue até sua conta de armazenamento, clique em **todas as configurações** e, em seguida, clique em **Propriedades** para exibir e copiar suas URLs de ponto de extremidade (consulte a Figura 5).  

![instantâneo dos links de ponto de extremidade de testemunha de nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: links de URL de ponto de extremidade de testemunha de nuvem**

Para obter mais informações sobre como criar e gerenciar contas de armazenamento do Azure, consulte [sobre contas de armazenamento do Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurar a testemunha de nuvem como uma testemunha de quorum para o cluster
A configuração da testemunha em nuvem está bem integrada no assistente de configuração de quorum existente interno do Gerenciador de Cluster de Failover.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Para configurar a testemunha de nuvem como uma testemunha de quorum
1. Iniciar Gerenciador de Cluster de Failover.
2. Clique com o botão direito do mouse no cluster-> **mais ações** -> **definir as configurações de quorum do cluster** (veja a Figura 6). Isso inicia o assistente para configurar quorum do cluster.  
    ![instantâneo do caminho do menu para configurar as configurações de quorum do cluster na interface do usuário do Gerenciador de Cluster de Failover](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **Figura 6. Configurações de quorum do cluster**

3. Na página **selecionar configurações de quorum** , selecione **selecionar a testemunha de quorum** (veja a Figura 7).  

    ![instantâneo do botão de opção ' selecionar testemunha quotrum ' no assistente de quorum de cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Selecionar a configuração de quorum**

4. Na página **selecionar testemunha de quorum** , selecione **Configurar uma testemunha de nuvem** (veja a Figura 8).  

    ![instantâneo do botão de opção apropriado para selecionar uma testemunha de nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Selecionar a testemunha de quorum**  

5. Na página **Configurar testemunha de nuvem** , insira as seguintes informações:  
   1. (Parâmetro obrigatório) Nome da conta de armazenamento do Azure.  
   2. (Parâmetro obrigatório) Chave de acesso correspondente à conta de armazenamento.  
       1. Ao criar pela primeira vez, use a chave de acesso primária (veja a Figura 5)  
       2. Ao girar a chave de acesso primária, use a tecla de acesso secundária (veja a Figura 5)  
   3. (Parâmetro opcional) Se você pretende usar um ponto de extremidade de serviço do Azure diferente (por exemplo, o serviço de Microsoft Azure na China), atualize o nome do servidor de ponto de extremidade.  

      ![instantâneo do painel de configuração de testemunha de nuvem no assistente de quorum de cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
      **Figura 9: configurar sua testemunha de nuvem**

6. Após a configuração bem-sucedida da testemunha de nuvem, você pode exibir o recurso de testemunha recém-criado no snap-in Gerenciador de Cluster de Failover (consulte a Figura 10).

    ![configuração bem-sucedida da testemunha de nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: configuração bem-sucedida da testemunha de nuvem**

### <a name="configuring-cloud-witness-using-powershell"></a>Configurando a testemunha de nuvem usando o PowerShell  
O comando do PowerShell Set-ClusterQuorum existente tem novos parâmetros adicionais correspondentes à testemunha de nuvem.  

Você pode configurar a testemunha de nuvem usando o [`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx) seguinte comando do PowerShell:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

Caso você precise usar um ponto de extremidade diferente (raro):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Considerações de conta de armazenamento do Azure com testemunha de nuvem  
Ao configurar uma testemunha de nuvem como uma testemunha de quorum para o cluster de failover, considere o seguinte:
* Em vez de armazenar a chave de acesso, o cluster de failover irá gerar e armazenar com segurança um token de SAS (segurança de acesso compartilhado).  
* O token SAS gerado é válido, desde que a chave de acesso permaneça válida. Ao girar a chave de acesso primária, é importante primeiro atualizar a testemunha de nuvem (em todos os clusters que estão usando essa conta de armazenamento) com a chave de acesso secundária antes de regenerar a chave de acesso primária.  
* A testemunha de nuvem usa a interface REST HTTPS do serviço de conta de armazenamento do Azure. Isso significa que é necessário que a porta HTTPS seja aberta em todos os nós de cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considerações sobre proxy com testemunha de nuvem  
A testemunha de nuvem usa HTTPS (porta padrão 443) para estabelecer a comunicação com o serviço blob do Azure. Verifique se a porta HTTPS está acessível por meio do proxy de rede.

## <a name="see-also"></a>Consulte também
- [O que há de novo no clustering de failover no Windows Server](whats-new-in-failover-clustering.md)
