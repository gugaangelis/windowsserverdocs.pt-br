---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Implantar uma testemunha de nuvem para um Cluster de Failover
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 10/2/2017
description: "Como usar o Microsoft Azure para hospedar a testemunha para um Cluster de Failover do Windows Server na nuvem – também conhecido como como implantar uma testemunha na nuvem."
ms.openlocfilehash: 564c6668fcc80a8bd1531c05c142996689de8154
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Implantar uma testemunha de nuvem para um Cluster de Failover

> Aplica-se a: aplica-se a: Windows Server (anual por canal), o Windows Server 2016

Testemunha na nuvem é um novo tipo de testemunha quorum de Cluster de Failover sendo introduzido no Windows Server 2016. Este tópico fornece uma visão geral de instruções sobre como configurar uma testemunha de nuvem para um Cluster de Failover que está executando o Windows Server 2016, os cenários que dá suporte a ele e o recurso de nuvem testemunha.

## <a name="CloudWitnessOverview"></a>Visão geral de testemunha de nuvem

Uma configuração de quorum vários local esticada Cluster de Failover com o Windows Server 2016 ilustrado na Figura 1. Nesta configuração de exemplo (Figura 1), há 2 nós em 2 datacenters (chamados de Sites). Observe que é possível que um cluster distribuí-lo datacenters mais de 2. Além disso, cada datacenter pode ter mais de 2 nós. Uma configuração de quorum de cluster típico nessa configuração (SLA de failover automático) oferece uma votação de cada nó. Votos extra é fornecido para a testemunha quorum para permitir que o cluster manter em execução mesmo se qualquer uma das datacenter experiências uma queda de energia. A matemática é simple – existem 5 votos totais e você precisa 3 votos para o cluster para mantê-lo funcionando.  

![Testemunha de compartilhamento de arquivos em um terceiro separado do site com 2 nós em 2 outros sites](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "testemunha de compartilhamento de arquivos")  
**Figura 1: Usando um compartilhamento de arquivos testemunha como testemunha quorum**  

Em caso de queda de energia em um datacenter, para dar a oportunidade de igual para o cluster em outro datacenter para mantê-lo funcionando, é recomendável para hospedar a testemunha quorum em um local diferente dois centros de dados. Isso normalmente significa que exigem uma terceira separar datacenter (site) para hospedar um servidor de arquivos que está fazendo o compartilhamento de arquivos que é usado como testemunha quorum (testemunha de compartilhamento de arquivos).  

A maioria das organizações não têm uma terceira separar datacenter que irá hospedar o servidor de arquivos, fazendo a testemunha de compartilhamento de arquivos. Isso significa que as organizações principalmente hospedam o servidor de arquivos em um dos dois centros de dados, que, por extensão, torna essa datacenter do data center principal. Em um cenário onde há queda de energia no datacenter principal, o cluster deve ir para baixo conforme outro datacenter teria apenas 2 votos que está abaixo da maioria quorum de 3 votos necessários. Para os clientes que têm o terceiro datacenter separada para hospedar o servidor de arquivos, é uma sobrecarga para manter o servidor de arquivos altamente disponíveis fazendo a testemunha de compartilhamento de arquivos. Hospedando máquinas virtuais na nuvem pública que tem o servidor de arquivos para o compartilhamento de arquivos testemunha em execução no sistema operacional convidado é uma sobrecarga significativa em termos de instalação e manutenção.  

Testemunha na nuvem é um novo tipo de testemunha quorum de Cluster de Failover que aproveita o Microsoft Azure como o ponto de arbitragem (Figura 2). Ele usa o armazenamento de BLOBs do Azure para um arquivo de blob que é usada como um ponto de arbitragem em caso de uma resolução de leitura/gravação.  

Há significativa essa abordagem se beneficia que:
1. Aproveita o Microsoft Azure (sem necessidade de datacenter terceiro separado).  
2. Usa o padrão Azure Blob de armazenamento disponível (sem sobrecarga de manutenção adicionais de máquinas virtuais hospedadas em nuvem pública).  
3. Mesma conta de armazenamento do Azure pode ser usada para vários clusters (arquivo de um blob por cluster; usado como nome de arquivo do blob de id exclusivo do cluster).  
4. $Cost contínuo muito baixo para a conta de armazenamento (muito pequenos dados gravados por arquivo de blob, arquivo de blob atualizado somente uma vez quando muda o estado de nós de cluster).  
5. Tipo de recurso de nuvem testemunha interno.  

![Diagrama ilustrando um cluster esticado de vários local com testemunha de nuvem como testemunha quorum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: Vários locais esticado clusters com testemunha de nuvem como testemunha quorum**  

Conforme mostrado na Figura 2, não há nenhum terceiro local separado que é necessário. Testemunha na nuvem, como qualquer outra testemunha quorum, obtém uma votação e pode participar de cálculos de quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Nuvem testemunha: Cenários com suporte para o tipo de testemunha único
Se você tiver uma implantação de Cluster de Failover, onde todos os nós podem acessar a internet (por extensão do Azure), é recomendável que você configure uma testemunha de nuvem como o recurso de testemunha quorum.  

Alguns dos cenários compatíveis usam da nuvem testemunha como uma testemunha quorum são as seguintes:  
- Recuperação de desastres esticado clusters de vários locais (ver Figura 2).  
- Clusters de failover sem armazenamento compartilhado (SQL sempre em etc.).  
- Clusters de failover em execução dentro do SO convidado hospedado em função da máquina Virtual do Microsoft Azure (ou qualquer outro nuvem pública).  
- Clusters de failover em execução dentro de convidado SO de máquinas virtuais hospedadas em nuvens privadas.
- Armazenamento clusters com ou sem armazenamento compartilhado, como clusters de servidor de arquivos fora de escala.  
- Clusters de filial pequeno (clusters de até 2 nós)  

A partir do Windows Server 2012 R2, é recomendável sempre configurar uma testemunha conforme o cluster gerencia automaticamente o votos testemunha e votam os nós com Quorum dinâmico.  

## <a name="CloudWitnessSetUp"></a>Configurar uma testemunha de nuvem para um cluster
Para configurar uma testemunha de nuvem como testemunha quorum para o cluster, conclua as seguintes etapas:
1. Criar uma conta de armazenamento do Azure para usar como uma testemunha na nuvem
2. Configure a testemunha de nuvem como testemunha quorum para o cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Criar uma conta de armazenamento do Azure para usar como uma testemunha na nuvem

Esta seção descreve como criar um ponto de extremidade de conta e o modo de exibição e a cópia de armazenamento URLs e acessar chaves para a conta.

Para configurar testemunha na nuvem, você deve ter uma conta de armazenamento do Azure válidos que podem ser usados para armazenar o arquivo de blob (usado para arbitragem). Testemunha nuvem cria um contêiner conhecido **msft-nuvem-testemunha** sob a conta de armazenamento da Microsoft. Nuvem testemunha grava um arquivo único blob com correspondente do cluster do ID exclusivo usado como o nome do arquivo do arquivo com esse blob **msft-nuvem-testemunha** contêiner. Isso significa que você pode usar a mesma conta de armazenamento do Microsoft Azure para configurar uma testemunha de nuvem para vários clusters diferentes.

Quando você usa a mesma conta de armazenamento do Azure para configurar testemunha de nuvem para vários diferentes clusters, um único **msft-nuvem-testemunha** contêiner é criado automaticamente. Este contêiner conterá o blob de um arquivo por cluster.

### <a name="to-create-an-azure-storage-account"></a>Para criar uma conta de armazenamento do Azure

1. Faça logon no [Portal do Azure](http://portal.azure.com).
2. No menu do Hub, selecione Novo -> Data + armazenamento -> conta de armazenamento.
3. Em criar uma página de conta de armazenamento, faça o seguinte:
    1. Insira um nome para sua conta de armazenamento.
    <br>Nomes de contas de armazenamento deverá ter entre 3 e 24 caracteres de comprimento e podem conter números e letras minúsculas apenas. O nome da conta de armazenamento também deve ser exclusivo no Azure.
        
    2. Para **conta kind**, selecione **finalidade geral**.
    <br>Você não pode usar uma conta de armazenamento de Blob para uma testemunha na nuvem.
    3. Para **desempenho**, selecione **padrão**.
    <br>Você não pode usar o armazenamento de Premium do Azure para um testemunha na nuvem.
    2. Para **replicação**, selecione **armazenamento redundante localmente (LRS)**.
    <br>Clustering de failover usa o arquivo de blob como o ponto de arbitragem, que requer algumas garantias de consistência, quando os dados de leitura. Portanto, você deve selecionar **armazenamento localmente redundante** para **replicação** tipo.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Exibir e copiar as teclas de acesso de armazenamento para sua conta de armazenamento do Azure

Quando você cria uma conta de armazenamento do Microsoft Azure, ele está associado com duas teclas de acesso que são gerados automaticamente - chave primária de acesso e a tecla de acesso secundário. Para a criação de pela primeira vez de nuvem testemunha, use o **principal tecla de acesso**. Não há nenhuma restrição em relação ao qual tecla usar para testemunha de nuvem.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Para exibir e copiar as teclas de acesso de armazenamento

No Portal do Azure, navegue até sua conta de armazenamento, clique em **todas as configurações** e, em seguida, clique em **as teclas de acesso** para exibir, copiar e gerar chaves de acesso à sua conta. A lâmina as teclas de acesso também inclui cadeias de caracteres de conexão pré-configuradas usando suas chaves principais e secundários que você pode copiar para usar em seus aplicativos (veja a Figura 4).

![Captura de tela da caixa de diálogo Gerenciar chaves de acesso no Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figura 4: As teclas de acesso de armazenamento**

### <a name="view-and-copy-endpoint-url-links"></a>Exibir e copiar Links de URL de ponto de extremidade  
Quando você cria uma conta de armazenamento, as URLs a seguir são geradas usando o formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Testemunha nuvem sempre usa **Blob** como o tipo de armazenamento. Usa Azure **. core.windows.net** como o ponto de extremidade. Ao configurar testemunha na nuvem, é possível que você configurá-lo com um ponto de extremidade diferente em conformidade com seu cenário (por exemplo o datacenter do Microsoft Azure na China tem um ponto de extremidade diferente).  

> [!NOTE]  
> A URL de ponto de extremidade é gerada automaticamente pelo recurso de nuvem testemunha e nenhuma etapa de configuração extra é necessário para a URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Para exibir e copiar links de URL de ponto de extremidade
No Portal do Azure, navegue até sua conta de armazenamento, clique em **todas as configurações** e, em seguida, clique em **propriedades** para exibir e copie suas URLs de ponto de extremidade (veja a Figura 5).  

![Captura de tela dos links de ponto de extremidade de nuvem testemunha](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: Links de URL de ponto de extremidade de nuvem testemunha**

Para obter mais informações sobre a criação e gerenciamento de contas de armazenamento do Azure, consulte [sobre contas de armazenamento do Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurar testemunha de nuvem como testemunha quorum para o cluster
Configuração de testemunha na nuvem é bem integrada dentro o Assistente de configuração de Quorum existentes criados para o Gerenciador de Cluster de Failover.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Para configurar testemunha de nuvem como testemunha Quorum
1. Inicie o Gerenciador de Cluster de Failover.
2. Botão direito do mouse no cluster -> **mais ações** -> **definir configurações do Cluster Quorum** (veja a Figura 6). Isso inicia o Assistente para Configurar Quorum do Cluster.  
    ![Instantâneo do caminho menu para configurar o Cluster Quorum configurações na UI do Gerenciador de Cluster de Failover](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png)**Figura 6. Configurações de Quorum de cluster**

3. Sobre o **selecionar configurações de Quorum**, selecione **selecione testemunha quorum** (ver Figura 7).  

    ![Instantâneo de 'select testemunha quotrum' radio button no Assistente de Cluster Quorum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Selecione a configuração de Quorum**

4. No **selecione Quorum testemunha**, selecione **configurar uma testemunha nuvem** (veja a Figura 8).  

    ![Captura de tela do botão de opção adequados para selecionar uma testemunha na nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Selecione a testemunha Quorum**  

5. Sobre o **configurar nuvem testemunha** página, insira as seguintes informações:  
    1. (Parâmetro obrigatório) Nome da conta de armazenamento do Azure.  
    2. (Parâmetro obrigatório) Tecla de acesso correspondente à conta de armazenamento.  
        1. Ao criar pela primeira vez, use a tecla de acesso principal (veja a Figura 5)  
        2. Ao girar a chave primária de acesso, use secundário tecla de acesso (veja a Figura 5)  
    3. (Parâmetro opcional) Se você pretende usar um ponto de extremidade do serviço Azure diferentes (por exemplo o serviço Microsoft Azure na China), em seguida, atualize o nome do servidor de ponto de extremidade.  

    ![Captura de tela do painel de configuração testemunha de nuvem no Assistente de Cluster Quorum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **Figura 9: Configurar seu testemunha na nuvem**

6. Após a configuração bem-sucedida do testemunha na nuvem, você pode exibir o recurso de testemunha recém criada no Gerenciador de Cluster de Failover snap-in (veja a Figura 10).

    ![Configuração bem-sucedida do testemunha de nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: Configuração bem-sucedida da nuvem testemunha**

### <a name="configuring-cloud-witness-using-powershell"></a>Configurando testemunha de nuvem usando o PowerShell  
O comando do PowerShell Set-ClusterQuorum existente tem novos parâmetros adicionais correspondentes testemunha na nuvem.  

Você pode configurar testemunha de nuvem usando o [`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx)comandos do PowerShell a seguir:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

No caso de você precisa usar um ponto de extremidade diferente (raro):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Azure considerações conta de armazenamento em nuvem testemunha  
Quando estiver configurando uma testemunha de nuvem como testemunha quorum para o Cluster de Failover, considere o seguinte:
* Em vez de armazenar a chave de acesso, o Cluster de Failover irá gerar e armazenar com segurança um token de segurança de acesso compartilhado (SAS).  
* O token SAS gerado é válido desde que a tecla de acesso permanece válida. Ao girar a tecla de acesso principal, é importante atualizar primeiro testemunha na nuvem (em todos os seus clusters que estão usando essa conta de armazenamento) com a tecla de acesso secundário antes de gerar a chave primária de acesso.  
* Nuvem testemunha usa interface HTTPS restante do serviço de conta de armazenamento do Azure. Isso significa que ele exige a porta HTTPS para ser aberto em todos os nós de cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considerações de proxy com a nuvem testemunha  
Nuvem testemunha usa HTTPS (padrão porta 443) para estabelecer comunicação com o serviço de BLOBs do Azure. Certifique-se de que a porta HTTPS é acessível por meio de Proxy de rede.

## <a name="see-also"></a>Consulte também
- [O que há de novo no Failover Clustering no Windows Server](whats-new-in-failover-clustering.md)
