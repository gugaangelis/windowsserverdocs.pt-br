---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Implantar uma testemunha de nuvem para um Cluster de Failover
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: Como usar o Microsoft Azure para hospedar a testemunha para um Cluster de Failover do Windows Server na nuvem – também conhecido como como implantar uma testemunha de nuvem.
ms.openlocfilehash: f7e1c84e54f08044a772f06e591588c1add33026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857977"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Implantar uma testemunha de nuvem para um Cluster de Failover

> Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server

Testemunha de nuvem é um tipo de testemunha de quorum de Cluster de Failover que usa o Microsoft Azure para fornecer um voto de quorum do cluster. Este tópico fornece uma visão geral de instruções sobre como configurar uma testemunha de nuvem para um Cluster de Failover, os cenários que ele dá suporte e o recurso de testemunha de nuvem.

## <a name="CloudWitnessOverview"></a>Visão geral da testemunha de nuvem

Figura 1 ilustra uma configuração de quorum multissite ampliada Cluster de Failover com o Windows Server 2016. Essa configuração de exemplo (Figura 1), existem 2 nós em 2 datacenters (conhecidos como Sites). Observe que é possível para um cluster abranger mais de 2 datacenters. Além disso, cada centro de dados pode ter mais de 2 nós. Uma configuração de quorum de cluster típico nessa configuração (SLA de failover automático) fornece um voto de cada nó. Um voto extra é fornecido para a testemunha de quorum para permitir que o cluster manter em execução, mesmo se qualquer um dos datacenter passa por uma interrupção de energia. A matemática é simple – há 5 total de votos e você precisa 3 votos para o cluster para mantê-lo em execução.  

![Testemunha de compartilhamento de arquivo em um terceiro separado do site com 2 nós 2 outros sites](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "testemunha de compartilhamento de arquivo")  
**Figura 1: Usando uma testemunha de compartilhamento de arquivo como uma testemunha de quorum**  

No caso de queda de energia em um data center, para oferecer oportunidades iguais para o cluster em outro datacenter para mantê-lo funcionando, é recomendável para hospedar a testemunha de quorum em um local diferente os dois datacenters. Isso normalmente significa que exigem um terceiro separar datacenter (site) para hospedar um servidor de arquivos que está dando suporte a compartilhamento de arquivos que é usado como a testemunha de quorum (testemunha de compartilhamento de arquivos).  

A maioria das organizações não tem um terceiro separar o datacenter que irá hospedar o servidor de arquivos, fazendo a testemunha de compartilhamento de arquivo. Isso significa que as organizações principalmente hospedam o servidor de arquivos em um dos dois datacenters, o que, por extensão, torna esse datacenter o datacenter primário. Em um cenário em que há uma queda de energia no datacenter primário, o cluster pudesse cair conforme outro datacenter teria apenas 2 votos que está abaixo a maioria de quorum de 3 votos necessários. Para os clientes que têm o terceiro datacenter separado para hospedar o servidor de arquivos, é uma sobrecarga para manter o servidor de arquivos altamente disponível, fazendo a testemunha de compartilhamento de arquivo. Hospedar máquinas virtuais na nuvem pública com o servidor de arquivos para a testemunha de compartilhamento de arquivo em execução no sistema operacional convidado é uma sobrecarga significativa em termos de instalação e manutenção.  

Testemunha de nuvem é um novo tipo de testemunha de quorum de Cluster de Failover que utiliza o Microsoft Azure como o ponto de arbitragem (Figura 2). Ele usa o armazenamento de BLOBs do Azure para um arquivo de blob que é usado como um ponto de arbitragem em caso de resolução de separação de leitura/gravação.  

Há significativos benefícios que essa abordagem:
1. Utiliza o Microsoft Azure (sem necessidade de datacenter separados em terceiro lugar).  
2. Usa o padrão do Azure Blob de armazenamento disponível (sem sobrecarga adicional de manutenção de máquinas virtuais hospedadas na nuvem pública).  
3. Mesma conta de armazenamento do Azure pode ser usada para vários clusters (arquivo de um blob por cluster; usado como nome de arquivo de blob de id exclusiva do cluster).  
4. Muito baixa em andamento o $cost à conta de armazenamento (muito pequena de dados gravados por arquivo de blob, arquivo de blob atualizado apenas uma vez quando muda o estado de nós de cluster).  
5. Tipo de recurso de testemunha de nuvem interno.  

![Diagrama ilustrando um cluster estendido de vários site com testemunha de nuvem como uma testemunha de quorum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: Clusters estendidos de vários sites com testemunha de nuvem como uma testemunha de quorum**  

Conforme mostrado na Figura 2, não há nenhum terceiro site separado que é necessário. Testemunha de nuvem, como qualquer outra testemunha de quorum, obtém um voto e pode participar em cálculos de quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Testemunha de nuvem: Cenários com suporte para o tipo de testemunha único
Se você tiver uma implantação de Cluster de Failover, onde todos os nós podem acessar a internet (pela extensão do Azure), é recomendável que você configure uma testemunha de nuvem como o recurso de testemunha de quorum.  

Alguns dos cenários em que há suporte para usam de testemunha de nuvem como uma testemunha de quorum são os seguintes:  
- Recuperação de desastres alongado clusters multissite (veja a Figura 2).  
- Clusters de failover sem armazenamento compartilhado (SQL sempre no etc.).  
- Clusters de failover em execução dentro do sistema operacional convidado hospedado na função de máquina Virtual do Microsoft Azure (ou qualquer outra nuvem pública).  
- Clusters de failover em execução dentro do sistema operacional de máquinas virtuais convidadas hospedados em nuvens privadas.
- Armazenamento de clusters com ou sem armazenamento compartilhado, como clusters de servidor de arquivos de escalabilidade horizontal.  
- Clusters de filiais pequenas (clusters de até 2 nós)  

Começando com o Windows Server 2012 R2, recomenda-se de sempre configurar uma testemunha de como o cluster gerencia automaticamente o voto de testemunha e os nós de votação com Quorum dinâmico.  

## <a name="CloudWitnessSetUp"></a> Configurar uma testemunha de nuvem para um cluster
Para configurar uma testemunha de nuvem como uma testemunha de quorum para seu cluster, conclua as seguintes etapas:
1. Criar uma conta de armazenamento do Azure para usar como uma testemunha de nuvem
2. Configure a testemunha de nuvem como uma testemunha de quorum para seu cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Criar uma conta de armazenamento do Azure para usar como uma testemunha de nuvem

Esta seção descreve como criar um ponto de extremidade do armazenamento conta e exibir e copiar as URLs e chaves de acesso para essa conta.

Para configurar a testemunha de nuvem, você deve ter uma conta de armazenamento do Azure válida que pode ser usado para armazenar o arquivo de blob (usado para arbitragem). Testemunha de nuvem cria um contêiner conhecido **msft testemunha de nuvem** sob a conta de armazenamento do Microsoft. Gravações de testemunha de nuvem um arquivo de blob único com correspondente de cluster da ID exclusiva usada como o nome do arquivo do blob de arquivo sob esse **msft testemunha de nuvem** contêiner. Isso significa que você pode usar a mesma conta de armazenamento do Microsoft Azure para configurar uma testemunha de nuvem para vários clusters diferentes.

Quando você usa a mesma conta de armazenamento do Azure para configurar a testemunha de nuvem para vários clusters, um único **msft testemunha de nuvem** contêiner é criado automaticamente. Esse contêiner conterá o arquivo de um blob por cluster.

### <a name="to-create-an-azure-storage-account"></a>Para criar uma conta de armazenamento do Azure

1. Entrar para o [Portal do Azure](http://portal.azure.com).
2. No menu Hub, selecione Novo -> dados + armazenamento -> a conta de armazenamento.
3. Em criar uma página da conta de armazenamento, faça o seguinte:
    1. Insira um nome para sua conta de armazenamento.
    <br>Os nomes da conta de armazenamento devem ter entre 3 e 24 caracteres e podem conter apenas números e letras minúsculas. O nome da conta de armazenamento também deve ser exclusivo dentro do Azure.
        
    2. Para **tipo de conta**, selecione **finalidade geral**.
    <br>Você não pode usar uma conta de armazenamento de Blob para uma testemunha de nuvem.
    3. Para **desempenho**, selecione **padrão**.
    <br>Você não pode usar o armazenamento Premium do Azure para uma testemunha de nuvem.
    2. Para **replicação**, selecione **armazenamento localmente redundante (LRS)** .
    <br>Clustering de failover usa o arquivo de blob como o ponto de arbitragem, que requer algumas garantias de consistência ao ler os dados. Portanto, você deve selecionar **armazenamento localmente redundante** para **replicação** tipo.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Exibir e copiar chaves de acesso de armazenamento para sua conta de armazenamento do Azure

Quando você cria uma conta de armazenamento do Microsoft Azure, ele está associado com duas chaves de acesso que são gerados automaticamente - chave de acesso primária e chave de acesso secundária. Para a criação de pela primeira vez de testemunha de nuvem, use o **chave de acesso primária**. Não há nenhuma restrição sobre qual chave usar para a testemunha de nuvem.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Para exibir e copiar as chaves de acesso de armazenamento

No Portal do Azure, navegue até sua conta de armazenamento, clique em **todas as configurações** e, em seguida, clique em **chaves de acesso** para exibir, copiar e regenerar as chaves de acesso da conta. A folha de chaves de acesso também inclui cadeias de caracteres de conexão pré-configuradas usando suas chaves primárias e secundárias que você pode copiar para usar em seus aplicativos (veja a Figura 4).

![Instantâneo da caixa de diálogo Gerenciar chaves de acesso no Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figura 4: Chaves de acesso de armazenamento**

### <a name="view-and-copy-endpoint-url-links"></a>Exibir e copiar os Links de URL de ponto de extremidade  
Quando você cria uma conta de armazenamento, as URLs a seguir são geradas usando o formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Testemunha de nuvem sempre usa **Blob** como o tipo de armazenamento. O Azure usa **. core.windows.net** como o ponto de extremidade. Ao configurar a testemunha de nuvem, é possível que você configurá-lo com um ponto de extremidade diferente, de acordo com seu cenário (por exemplo, o datacenter do Microsoft Azure na China tem um ponto de extremidade diferente).  

> [!NOTE]  
> A URL de ponto de extremidade é gerada automaticamente pelo recurso de testemunha de nuvem e não há nenhuma etapa adicional de configuração necessárias para a URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Para exibir e copiar os links de URL do ponto de extremidade
No Portal do Azure, navegue até sua conta de armazenamento, clique em **todas as configurações** e, em seguida, clique em **propriedades** para exibir e copiar suas URLs de ponto de extremidade (veja a Figura 5).  

![Instantâneo dos links de ponto de extremidade a testemunha de nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: Links de URL do ponto de extremidade de testemunha de nuvem**

Para obter mais informações sobre como criar e gerenciar contas de armazenamento do Azure, consulte [sobre contas de armazenamento do Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurar testemunha de nuvem como uma testemunha de quorum do cluster
Configuração de testemunha de nuvem é bem integrada dentro o Assistente de configuração de Quorum existente criado no Gerenciador de Cluster de Failover.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Para configurar a testemunha de nuvem como uma testemunha de Quorum
1. Inicie o Gerenciador de Cluster de Failover.
2. Clique com botão direito no cluster -> **mais ações** -> **configurar quorum do Cluster** (veja a Figura 6). Isso inicia o Assistente para Configurar Quorum do Cluster.  
    ![Instantâneo do caminho do menu Configurar quorum do Cluster na UI do Gerenciador de Cluster de Failover](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **Figura 6. Configurações de Quorum do cluster**

3. Sobre o **selecionar configurações de Quorum** , selecione **selecionar a testemunha de quorum** (veja a Figura 7).  

    ![Botão no Assistente para Quorum do Cluster de opção de instantâneo de 'Selecionar a testemunha quotrum'](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Selecione a configuração de Quorum**

4. Sobre o **selecionar testemunha de Quorum** , selecione **configurar uma testemunha de nuvem** (veja a Figura 8).  

    ![Instantâneo do botão de opção apropriado para selecionar uma testemunha de nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Selecionar a testemunha de Quorum**  

5. Sobre o **configurar testemunha de nuvem** página, insira as seguintes informações:  
    1. (Parâmetro necessário) Nome da conta de armazenamento do Azure.  
    2. (Parâmetro necessário) Chave de acesso correspondente à conta de armazenamento.  
        1. Ao criar pela primeira vez, use a chave de acesso primária (veja a Figura 5)  
        2. Para girar a chave de acesso primária, use a chave de acesso secundária (veja a Figura 5)  
    3. (Parâmetro opcional) Se você pretende usar um ponto de extremidade de serviço do Azure diferente (por exemplo o serviço Microsoft Azure na China), em seguida, atualize o nome do servidor de ponto de extremidade.  

    ![Instantâneo do painel de configuração de testemunha de nuvem no Assistente para Quorum do Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **Figura 9: Configure a testemunha de nuvem**

6. Após a configuração bem-sucedida da testemunha de nuvem, você poderá exibir o recurso de testemunha recém-criado no Gerenciador de Cluster de Failover snap-in (veja a Figura 10).

    ![Configuração bem-sucedida da testemunha de nuvem](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: Configuração bem-sucedida da testemunha de nuvem**

### <a name="configuring-cloud-witness-using-powershell"></a>Configurar testemunha de nuvem usando o PowerShell  
O comando Set-ClusterQuorum PowerShell existente tem novos parâmetros adicionais correspondentes a testemunha de nuvem.  

Você pode configurar a testemunha de nuvem usando o [ `Set-ClusterQuorum` ](https://technet.microsoft.com/library/ee461013.aspx) o comando PowerShell a seguir:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

Caso você precise usar um ponto de extremidade diferente (raro):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Considerações sobre a conta de armazenamento do Azure com a testemunha de nuvem  
Ao configurar uma testemunha de nuvem como uma testemunha de quorum de Cluster de Failover, considere o seguinte:
* Em vez de armazenar a chave de acesso, o Cluster de Failover gerará e armazene com segurança um token de segurança de acesso compartilhado (SAS).  
* O token SAS gerado é válido desde que a chave de acesso permanece válida. Para girar a chave de acesso primária, é importante primeiro atualize a testemunha de nuvem (em todos os seus clusters que estão usando essa conta de armazenamento) com a chave de acesso secundária antes de regenerar a chave de acesso primária.  
* Testemunha de nuvem usa a interface de HTTPS REST do serviço de conta de armazenamento do Azure. Isso significa que ele requer a porta HTTPS ser abertas em todos os nós de cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considerações sobre o proxy com testemunha de nuvem  
Testemunha de nuvem usa HTTPS (porta padrão 443) para estabelecer a comunicação com o serviço blob do Azure. Certifique-se de que a porta HTTPS é acessível por meio do Proxy de rede.

## <a name="see-also"></a>Consulte também
- [O que há de novo no Clustering de Failover no Windows Server](whats-new-in-failover-clustering.md)
