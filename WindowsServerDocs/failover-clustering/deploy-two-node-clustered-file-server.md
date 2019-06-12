---
title: Implantando um servidor de arquivos em cluster com dois nós
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/01/2019
description: Este artigo descreve como criar um cluster de servidor de arquivos com dois nós
ms.openlocfilehash: 9f50470b379bd0ab05834eb3c5a35be0f5e9e93a
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453001"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Implantando um servidor de arquivos em cluster com dois nós

> Aplica-se a: Windows Server 2019, Windows Server 2016

Um cluster de failover é um conjunto de computadores independentes que trabalham em conjunto para aumentar a disponibilidade de aplicativos e serviços. Os servidores clusterizados (chamados de nós) são conectados por cabos físicos e por software. Se um dos nós do cluster falhar, o outro nó começará a fornecer o serviço (um processo conhecido como failover). Os usuários vivenciam um mínimo de interrupções no serviço.

Este guia descreve as etapas para instalar e configurar um cluster de failover de servidor de arquivos de finalidade geral que tem dois nós. Ao criar a configuração neste guia, você pode saber mais sobre clusters de failover e familiarizar-se com a interface snap-in Gerenciamento de Cluster de Failover no Windows Server 2019 ou Windows Server 2016.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Visão geral de um cluster de servidores de arquivos com dois nós

Servidores em um cluster de failover podem funcionar em uma variedade de funções, incluindo as funções de servidor de arquivos, servidor Hyper-V ou servidor de banco de dados e podem fornecer alta disponibilidade para uma variedade de outros serviços e aplicativos. Este guia descreve como configurar um cluster de servidores de arquivos com dois nós.

Um cluster de failover normalmente inclui uma unidade de armazenamento conectada fisicamente a todos os servidores no cluster, embora qualquer volume no armazenamento somente seja acessado por um servidor de cada vez. O diagrama a seguir mostra um cluster de failover de dois nós conectado a uma unidade de armazenamento.

![Cluster de dois nós](media/Cluster-File-Server/Cluster-FS-Overview.png)

Volumes ou LUNs de armazenamento expostos aos nós em um cluster não devem ser expostos a outros servidores, inclusive a servidores em outro cluster. O diagrama a seguir ilustra isso.

![LUNs de armazenamento](media/Cluster-File-Server/Cluster-FS-LUNs.png)

Observe que para a disponibilidade máxima de qualquer servidor, é importante seguir práticas recomendadas para gerenciamento de servidores. Por exemplo, gerenciar cuidadosamente o ambiente físico dos servidores, testar alterações de software antes de implementá-las completamente e acompanhar as atualizações de software e as alterações de configuração em todos os servidores clusterizados.

O cenário a seguir descreve como pode ser configurado um cluster de failover de servidores de arquivos. Os arquivos que são compartilhados estão no armazenamento de cluster e qualquer servidor clusterizado pode agir como o servidor de arquivos que os compartilha.

## <a name="shared-folders-in-a-failover-cluster"></a>Pastas compartilhadas em um cluster de failover

A lista a seguir descreve a funcionalidade de configuração de pasta compartilhada que é integrada ao clustering de failover:

- Exibição tem como escopo nas pastas compartilhadas clusterizadas somente (não há combinação com pastas compartilhadas não-clusterizados): Quando um usuário exibe pastas compartilhadas, especificando o caminho de um servidor de arquivos em cluster, a exibição incluirá apenas as pastas compartilhadas que fazem parte da função de servidor de arquivo específico. Ela exclui pastas compartilhadas não clusterizado e compartilhamentos parte das funções de servidor de arquivos separados que podem estar em um nó do cluster.

- Enumeração baseada em acesso: você pode usar a enumeração baseada em acesso para ocultar da exibição dos usuários uma pasta especificada. Em vez de permitir que os usuários vejam a pasta, mas não acessem nada dela, é possível optar por impedi-los de ver a pasta. Você pode configurar a enumeração baseada em acesso para uma pasta compartilhada clusterizada da mesma forma que para uma pasta compartilhada não clusterizado.

- Acesso offline: você pode configurar o acesso offline (cache) para uma pasta compartilhada clusterizada da mesma forma que para uma pasta compartilhada não clusterizada.

- Discos clusterizados sempre são reconhecidos como parte do cluster: Se você usar a interface de cluster de failover, Windows Explorer ou o snap-in Gerenciamento de compartilhamento e armazenamento, o Windows reconhece se um disco tiver sido designado como estando no armazenamento de cluster. Se esse disco já tiver sido configurado no gerenciamento de Cluster de Failover como parte de um servidor de arquivos em cluster, você pode usar qualquer uma das interfaces mencionadas anteriormente para criar um compartilhamento no disco. Se esse disco não tiver sido configurado como parte de um servidor de arquivos clusterizado, não será possível criar uma pasta nele por engano. Em vez disso, um erro indica que o disco deve primeiro ser configurado como parte de um servidor de arquivos clusterizado para poder ser compartilhado.

- Integração de serviços para Network File System: A função de servidor de arquivos no Windows Server inclui o serviço de função opcional chamado serviços de sistema de arquivos de rede (NFS). Ao instalar o serviço de função e configurar pastas compartilhadas com os Serviços de NFS, você pode criar um servidor de arquivos clusterizado que ofereça suporte aos clientes UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Requisitos de um cluster de failover com dois nós

Para um cluster de failover no Windows Server 2016 ou Windows Server 2019 para ser considerado uma solução oficialmente suportada pela Microsoft, a solução deve atender aos critérios a seguir.

- Todos os componentes de hardware e software devem cumprir as qualificações do logotipo adequado. Para Windows Server 2016, esse é o logotipo "Certificado para Windows Server 2016". Para Windows Server de 2019, este é o logotipo "Certified for Windows Server 2019". Para obter mais informações sobre quais sistemas de hardware e software tenham sido certificados, a Microsoft, visite [catálogo do Windows Server](https://www.windowsservercatalog.com/default.aspx) site.

- A solução totalmente configurada (servidores, rede e armazenamento) deve passar por todos os testes no Assistente de validação, que faz parte do que o snap-in de cluster de failover.

O exemplo a seguir serão necessárias para um cluster de failover com dois nós.

- **servidores:** É recomendável usar computadores correspondentes com os mesmos ou componentes similares.  Os servidores para um cluster de failover com dois nós devem executar a mesma versão do Windows Server. Eles também devem ter as mesmas atualizações de software (patches).

- **Adaptadores e cabo de rede:** O hardware de rede, como outros componentes na solução de cluster de failover, deve ser compatível com o Windows Server 2016 ou Windows Server 2019. Se você usar iSCSI, os adaptadores de rede devem ser dedicados à comunicação de rede ou iSCSI, não ambos. Na infraestrutura de rede que conecta os nós do cluster, evite ter pontos de falha únicos. Há várias maneiras de realizar isso. É possível conectar os nós do cluster por várias redes distintas. Como alternativa, é possível conectar os nós do cluster com uma rede construída com adaptadores de rede emparelhados, comutadores redundantes, roteadores redundantes ou hardware semelhante que remova pontos de falha únicos.

   > [!NOTE]
   > Se os nós de cluster estiver conectados com uma única rede, a rede passará no requisito de redundância validar um Assistente de configuração.  No entanto, o relatório incluirá um aviso de que a rede não deve ter um ponto único de falha.

- **Controladores de dispositivo ou adaptadores adequados para armazenamento:**
    - **Serial Attached SCSI ou Fibre Channel:** Se estiver usando SAS ou Fibre Channel, em todos os servidores clusterizados, todos os componentes da pilha de armazenamento deverão ser idênticos. É necessário que o software do Multipath i / o (MPIO) e os componentes de software do módulo específico de dispositivo (DSM) sejam idênticos.  É recomendável que os controladores de dispositivo de armazenamento em massa — ou seja, a controladora, os drivers da controladora e o firmware da controladora — que estão conectados ao armazenamento clusterizado sejam idênticos. Se usar HBAs diferentes, você deverá verificar, com o fornecedor de armazenamento, se está cumprindo as configurações com suporte ou recomendadas.
    - **iSCSI:** Se você estiver usando iSCSI, cada servidor clusterizado deverá ter um ou mais adaptadores de rede ou adaptadores de barramento de host dedicados ao armazenamento ISCSI. A rede usada para iSCSI não deve ser usada para comunicação de rede. Em todos os servidores clusterizados, os adaptadores de rede usados para conexão com o destino de armazenamento iSCSI devem ser idênticos. É recomendável também usar Gigabit Ethernet ou superior.  

- **Armazenamento:** Você deve usar armazenamento compartilhado que é certificado para o Windows Server 2016 ou Windows Server 2019.
  
    Para um cluster de failover com dois nós, o armazenamento deve conter pelo menos dois volumes (LUNs) separados se usando um disco de testemunha de quorum. O disco testemunha é um disco no armazenamento de cluster designado para manter uma cópia do banco de dados de configuração do cluster. Para este exemplo de cluster de dois nós, a configuração de quorum será maioria de nós e disco. Maioria de nós e disco significa que os nós e o disco testemunha contêm cópias da configuração do cluster e o cluster tem quorum desde que a maioria (duas entre três) dessas cópias esteja disponível. O outro volume (LUN) conterá os arquivos que estão sendo compartilhados para os usuários.

    Os requisitos de armazenamento incluem o seguinte:

    - Para usar o suporte de disco nativo incluído no cluster de failover, use discos básicos, não discos dinâmicos.
    - É recomendável formatar as partições com NTFS (para o disco testemunha, a partição deve ser NTFS).
    - Para o estilo de partição do disco, é possível usar MBR (registro mestre de inicialização) ou GPT (tabela de partição GUID).
    - O armazenamento deve responder corretamente a comandos SCSI específicos, o armazenamento deve seguir o padrão chamado 3 SCSI Primary Commands-(SPC-3). Em particular, o armazenamento deve oferecer suporte a Reservas Persistentes, como especificado no padrão SPC-3. 
    -  O driver de miniporta usado para o armazenamento deve funcionar com o driver de armazenamento do Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implantando redes de área de armazenamento com clusters de failover

Ao implantar uma rede de área de armazenamento (SAN) com um cluster de failover, as diretrizes a seguir devem ser observadas.

- **Confirme a certificação do armazenamento:** Usando o [catálogo do Windows Server](https://www.windowsservercatalog.com/default.aspx) do site, confirme se o armazenamento de fornecedor, incluindo drivers, firmware e software, é certificado para o Windows Server 2016 ou Windows Server 2019.

- **Isole dispositivos de armazenamento, um cluster por dispositivo:** Servidores de diferentes clusters não devem poder acessar os mesmos dispositivos de armazenamento. Na maioria dos casos, um LUN usado para um conjunto de servidores de cluster deve ser isolado de todos os outros servidores por meio de mascaramento ou zoneamento de LUN.

- **Considere o uso de software MPIO:** Em uma malha de armazenamento altamente disponível, é possível implantar clusters de failover com várias controladoras usando o software MPIO. Isso fornece o mais alto nível de redundância e disponibilidade. A solução multipath deve ser baseada no Microsoft Multipath i / (MPIO). O fornecedor de hardware de armazenamento pode fornecer um módulo MPIO específicos do dispositivo (DSM) para o hardware, embora o Windows Server 2016 e Windows Server 2019 incluem um ou mais DSMs como parte do sistema operacional.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Requisitos de infraestrutura de rede e conta de domínio

Será necessária a infraestrutura de rede a seguir para um cluster de failover com dois nós e uma conta administrativa com as seguintes permissões de domínio:

- **As configurações de rede e endereços IP:** Ao usar adaptadores de rede idênticos para uma rede, use também configurações de comunicação idênticas nesses adaptadores (por exemplo, Velocidade, Modo Duplex, Controle de Fluxo e Tipo de Mídia). Além disso, compare as configurações entre o adaptador de rede e o comutador com o qual ele se conecta e verifique se não há nenhuma configuração em conflito.

    Se você tiver redes privadas que não sejam roteadas para o restante de sua infraestrutura de rede, verifique se cada uma dessas redes privadas usa uma sub-rede exclusiva. Isso será necessário mesmo que você dê a cada adaptador de rede um endereço IP exclusivo. Por exemplo, se você tiver um nó de cluster em uma matriz que use uma rede física e outro nó em uma filial que use uma rede física separada, não especifique 10.0.0.0/24 para as duas redes, mesmo que você atribua a cada adaptador um endereço IP exclusivo.

    Para obter mais informações sobre os adaptadores de rede, consulte os requisitos de Hardware para um cluster de failover com dois nós, neste guia.

- **DNS:** Os servidores no cluster devem usar DNS (Sistema de Nomes de Domínio) para resolução de nomes. O protocolo de atualização dinâmica de DNS pode ser usado.

- **Função de domínio:** Todos os servidores do cluster devem estar no mesmo domínio do Active Directory. Como prática recomendada, todos os servidores clusterizados devem ter a mesma função de domínio (servidor membro ou controlador de domínio). A função recomendada é servidor membro.

- **Controlador de domínio:** É recomendável que os servidores clusterizados sejam servidores membros. Se forem, será necessário um servidor adicional que atue como controlador de domínio no domínio que contém o cluster de failover.

- **Clientes:** Quando necessário para fins de teste, você poderá conectar um ou mais clientes da rede ao cluster de failover que você criou e observar o efeito em um cliente quando você move ou executa o failover do servidor de arquivo clusterizado de um nó do cluster para o outro.

- **Conta para administrar o cluster:** Ao criar um cluster ou ao adicionar servidores ao cluster pela primeira vez, você deve fazer logon no domínio com uma conta que tenha direitos e permissões de administrador em todos os servidores daquele cluster. A conta não precisa ser uma conta Admins. do Domínio, mas poderá ser uma conta Usuários do Domínio que estiver no grupo Administradores em cada servidor com cluster. Além disso, se a conta não é uma conta de Admins. do domínio, a conta (ou o grupo que a conta é membro) deve receber a **criar objetos de computador** e **ler todas as propriedades** permissões no domínio unidade organizacional (UO) que é residirá no.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Etapas para instalar um cluster de servidores de arquivos com dois nós

Você precisa concluir as etapas a seguir para instalar um cluster de failover de servidores de arquivos com dois nós.

Etapa 1: Conectar os servidores de cluster às redes e ao armazenamento

Etapa 2: Instalar o recurso de cluster de failover

Etapa 3: Validar a configuração do cluster

Etapa 4: Criar o cluster

Se você já tiver instalado os nós do cluster e deseja configurar um cluster de failover do servidor de arquivos, consulte as etapas para configurar um cluster de servidor de arquivos com dois nós, neste documento.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Etapa 1: Conectar os servidores de cluster às redes e ao armazenamento

Para uma rede de cluster de failover, evite ter pontos de falha únicos. Há várias maneiras de realizar isso. É possível conectar os nós do cluster por várias redes distintas. Como alternativa, você pode conectar seus nós do cluster a uma rede construída com adaptadores de rede emparelhados, comutadores redundantes, roteadores redundantes ou hardware semelhante, capazes de remover pontos de falha únicos (se você usar uma rede para iSCSI, deverá criar essa rede em acréscimo às outras redes).

Em um cluster de servidores de arquivos com dois nós, quando você conectar os servidores ao armazenamento do cluster, deverá expor pelo menos dois volumes (LUNs). Você pode expor volumes adicionais quando necessário para testes completos da sua configuração. Não exponha os volumes clusterizados a servidores que não fazem parte do cluster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Para conectar os servidores de cluster às redes e ao armazenamento

1. Examine os detalhes sobre redes em requisitos de Hardware para um cluster de failover com dois nós e requisitos de conta domínio e de infraestrutura de rede para um cluster de failover com dois nós, neste guia.

2. Conecte e configure as redes que serão usadas pelos servidores no cluster.

3. Se sua configuração de teste incluir clientes ou um controlador de domínio não clusterizado, verifique se esses computadores podem conectar-se aos servidores clusterizados por meio de pelo menos uma rede.

4. Siga as instruções do fabricante para conectar fisicamente os servidores ao armazenamento.

5. Verifique se os discos (LUNs) que deseja usar no cluster são expostos aos servidores que você irá clusterizar (e apenas a esses servidores). É possível usar qualquer uma das interfaces a seguir para expor discos ou LUNs:

    - A interface fornecida pelo fabricante do armazenamento.

    - Se estiver usando iSCSI, uma interface iSCSI apropriada.

6. Se você tiver comprado software que controla o formato ou a função do disco, siga as instruções do fornecedor sobre como usar esse software com o Windows Server.

7. Em um dos servidores que você deseja cluster, clique em Iniciar, clique em Ferramentas administrativas, clique em gerenciamento do computador e, em seguida, clique em gerenciamento de disco. (Se a caixa de diálogo controle de conta de usuário for exibida, confirme se a ação exibida é o que você deseja e clique em continuar.) No gerenciamento de disco, confirme se os discos de cluster estão visíveis.

8. Se desejar ter um novo volume de armazenamento com mais de 2 terabytes e estiver usando a interface do Windows para controlar o formato do disco, converta esse disco no estilo de partição chamado de GPT (tabela de partição GUID). Para fazer isso, fazer backup de todos os dados no disco, exclua todos os volumes no disco e, em gerenciamento de disco, clique com botão direito no disco (não uma partição) e clique em Converter em disco GPT.  Para volumes menores que 2 terabytes, em vez de usar a GGT, você pode usar o estilo de partição chamado MBR (registro mestre de inicialização).

9. Verifique o formato de qualquer volume ou LUN exposto. É recomendável usar NTFS como formato (para o disco testemunha, é preciso usar NTFS).

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Etapa 2: Instalar o arquivo servidor failover e a função de recurso de cluster

Nesta etapa, o arquivo failover e a função de cluster recurso de servidor será instalado. Ambos os servidores devem estar executando o Windows Server 2016 ou Windows Server 2019.

#### <a name="using-server-manager"></a>Usando o Gerenciador do servidor

1. Abra **Gerenciador de servidores** e, nas **gerenciar** lista suspensa, selecione **adicionar funções e recursos**.

   ![Adicionar recurso](media/Cluster-File-Server/Cluster-FS-Add-Feature.png)

2. Se o **antes de começar** janela é aberta, escolha **próxima**.

3. Para o **tipo de instalação**, selecione **instalação baseada em função ou recurso** e **próxima**.

4. Certifique-se **selecione um servidor no pool de servidores** é selecionada, o nome da máquina é realçado, e **próxima**.

5. Para a função de servidor, na lista de funções, abra **serviços de arquivo**, selecione **servidor de arquivos**, e **próxima**.

   ![Adicionar função](media/Cluster-File-Server/Cluster-FS-Add-FS-Role-1.png)

6. Para os recursos, na lista de recursos, selecione **Clustering de Failover**.  Uma caixa de diálogo pop-up mostrará que lista as ferramentas de administração também está sendo instaladas.  Manter todos os selecionado, escolha **adicionar recursos** e **próxima**.

   ![Adicionar recurso](media/Cluster-File-Server/Cluster-FS-Add-WSFC-1.png)

7. Na página de confirmação, selecione instalar.

8. Depois que a instalação for concluída, reinicie o computador.

9. Repita as etapas no segundo computador.

#### <a name="using-powershell"></a>Usando o PowerShell

1. Abra uma sessão do PowerShell administrativa clicando duas vezes no botão Iniciar e, em seguida, selecionando **Windows PowerShell (Admin)** .
2. Para instalar a função de servidor de arquivos, execute o comando:

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Para instalar o recurso de cluster de Failover e suas ferramentas de gerenciamento, execute o comando:

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Depois que tiver concluído, verifique se que eles são instalados com os comandos:

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Depois de verificar se que eles estão instalados, reinicie o computador com o comando:

    ```PowerShell
    Restart-Computer
    ```

6. Repita as etapas no segundo servidor.

### <a name="step-3-validate-the-cluster-configuration"></a>Etapa 3: Validar a configuração do cluster

Antes de criar um cluster, é recomendável validar a configuração. A validação ajuda a confirmar se a configuração dos servidores, da rede e do armazenamento atendem a um conjunto específico de requisitos para clusters de failover.

#### <a name="using-failover-cluster-manager"></a>Usando o Gerenciador de Cluster de Failover

1. Partir **Gerenciador do servidor**, escolha o **ferramentas** lista suspensa e selecione **Gerenciador de Cluster de Failover**.

2. Na **Gerenciador de Cluster de Failover**, vá para a coluna do meio sob **Management** e escolha **validar configuração**.

3. Se o **antes de começar** janela é aberta, escolha **próxima**.

4. No **selecionar servidores ou um Cluster** janela, adicionar os nomes dos dois computadores que serão os nós do cluster.  Por exemplo, se os nomes forem NODE1 e NODE2, insira o nome e selecione **adicionar**.  Você também pode escolher o **procurar** botão para pesquisar os nomes do Active Directory.  Depois que ambos são listadas na **servidores selecionados**, escolha **próxima**.

5. No **opções de teste** janela, selecione **executar todos os testes (recomendados)** , e **próxima**.

6. Sobre o **confirmação** página, ele lhe dará a listagem de todos os testes que ele verificará.  Escolher **próxima** e os testes serão iniciado.

7. Uma vez concluído, o **resumo** página será exibida após a execução dos testes. Para exibir tópicos da Ajuda para ajudar a interpretar os resultados, clique em **Mais informações sobre os testes de validação de cluster**.

8. Ainda na página Resumo, clique em Exibir relatório e ler os resultados do teste. Faça as alterações necessárias na configuração e execute os testes novamente. <br>Para exibir os resultados dos testes depois de fechar o assistente, consulte *data do relatório SystemRoot\Cluster\Reports\Validation e time*.

9. Para exibir tópicos da Ajuda sobre a validação de cluster depois de fechar o assistente, no gerenciamento de Cluster de Failover, clique em Ajuda, clique em tópicos da Ajuda, clique na guia conteúdo, expanda o conteúdo de Ajuda do cluster de failover e clique em validar uma configuração de Cluster de Failover .

#### <a name="using-powershell"></a>Usando o PowerShell

1. Abra uma sessão do PowerShell administrativa clicando duas vezes no botão Iniciar e, em seguida, selecionando **Windows PowerShell (Admin)** .

2. Para validar as máquinas (por exemplo, os nomes de máquina que está sendo NODE1 e NODE2) para Clustering de Failover, execute o comando:

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Para exibir os resultados dos testes depois de fechar o assistente, consulte o arquivo especificado (em SystemRoot\Cluster\Reports\), em seguida, faça as alterações necessárias na configuração e executar novamente os testes.

Para obter mais informações, consulte [Validando uma configuração de Cluster de Failover](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Etapa 4: Criar o Cluster

A seguir criará um cluster sem as máquinas e a configuração que você tem.

#### <a name="using-failover-cluster-manager"></a>Usando o Gerenciador de Cluster de Failover

1. Partir **Gerenciador do servidor**, escolha o **ferramentas** lista suspensa e selecione **Gerenciador de Cluster de Failover**.

2. Na **Gerenciador de Cluster de Failover**, vá para a coluna do meio sob **Management** e escolha **criar Cluster**.

3. Se o **antes de começar** janela é aberta, escolha **próxima**.

4. No **selecionar servidores** janela, adicionar os nomes dos dois computadores que serão os nós do cluster.  Por exemplo, se os nomes forem NODE1 e NODE2, insira o nome e selecione **adicionar**.  Você também pode escolher o **procurar** botão para pesquisar os nomes do Active Directory.  Depois que ambos são listadas na **servidores selecionados**, escolha **próxima**.

5. No **ponto de acesso para administrar o Cluster** janela, insira o nome do cluster que você usará.  Observe que isso não é o nome que você usará para se conectar a seus compartilhamentos de arquivos com.  Isso é para simplesmente administrar o cluster.

   > [!NOTE]
   > Se você estiver usando endereços IP estáticos, você precisará selecionar a rede para usar e insira o endereço de IP, ele usará o nome do cluster.  Se você estiver usando DHCP para seus endereços IP, o endereço IP será configurado automaticamente para você.

6. Escolher **próxima**.

7. Sobre o **confirmação** , verifique se o que você configurou e selecione **próxima** para criar o Cluster.

8. Sobre o **resumo** página, ele lhe dará a configuração que ele foi criado.  Você pode selecionar Exibir relatório para ver o relatório da criação.

#### <a name="using-powershell"></a>Usando o PowerShell

1. Abra uma sessão do PowerShell administrativa clicando duas vezes no botão Iniciar e, em seguida, selecionando **Windows PowerShell (Admin)** .

2. Execute o seguinte comando para criar o cluster, se você estiver usando endereços IP estáticos.  Por exemplo, os nomes de computador são NODE1 e NODE2, o nome do cluster será o CLUSTER e o endereço IP será 1.1.1.1.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Execute o seguinte comando para criar o cluster, se você estiver usando DHCP para endereços IP.  Por exemplo, os nomes de computador são NODE1 e NODE2, e o nome do cluster será o CLUSTER.

   ```PowerShell    
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Etapas para configurar um cluster de failover do servidor de arquivos

Para configurar um cluster de failover do servidor de arquivos, siga as etapas a seguir.

1. Partir **Gerenciador do servidor**, escolha o **ferramentas** lista suspensa e selecione **Gerenciador de Cluster de Failover**.

2. Quando o Gerenciador de Cluster de Failover é aberto, ele deve abrir automaticamente no nome do cluster que você criou.  Se não existir, vá para a coluna do meio, sob **Management** e escolha **conectar-se ao Cluster**.  Insira o nome do cluster que você criou e **Okey**.

3. Na árvore de console, clique o ">" próximo ao cluster que você criou para expandir os itens abaixo dele.

4. Clique do botão direito do mouse em **funções** e selecione **configurar função**.

5. Se o **antes de começar** janela é aberta, escolha **próxima**.

6. Na lista de funções, escolha **servidor de arquivos** e **próxima**.

7. O tipo de servidor de arquivo, selecione **servidor de arquivos para uso geral** e **próxima**.<br>Para obter informações sobre o servidor de arquivos de escalabilidade horizontal, consulte [visão geral do servidor de arquivos de escalabilidade horizontal](sofs-overview.md).

   ![Tipo de servidor de arquivo](media/Cluster-File-Server/Cluster-FS-File-Server-Type.png)

8. No **ponto de acesso para cliente** janela, insira o nome do servidor de arquivos que você usará.  Observe que isso não é o nome do cluster.  Isso é para a conectividade de compartilhamento de arquivo.  Por exemplo, se eu quiser se conectar ao \\SERVER, o nome inserido seria servidor.

   > [!NOTE]
   > Se você estiver usando endereços IP estáticos, você precisará selecionar a rede para usar e insira o endereço de IP, ele usará o nome do cluster.  Se você estiver usando DHCP para seus endereços IP, o endereço IP será configurado automaticamente para você.

6. Escolher **próxima**.

7. No **selecionar armazenamento** janela, selecione a unidade adicional (não a testemunha) que conterá seus compartilhamentos e **próxima**.

8. Sobre o **confirmação** página, verifique se sua configuração e selecione **próxima**.

9. Sobre o **resumo** página, ele lhe dará a configuração que ele foi criado.  Você pode selecionar o modo de exibição de relatório para ver o relatório de criação de função de servidor de arquivos.

10. Sob **funções** na árvore de console, você verá a nova função criada listado como o nome que você criou.  Com ele realçado, sob o **ações** painel à direita, escolha **adicionar um compartilhamento de**.

11. Execute o Assistente de compartilhamento inserindo o seguinte:

    - Tipo de compartilhamento será
    - Caminho do local que a pasta compartilhada será
    - O nome dos usuários de compartilhamento se conectará ao
    - Configurações adicionais, como enumeração baseada em acesso, cache, criptografia, etc
    - Permissões de nível de arquivo, se eles serão diferentes dos padrões

12. Sobre o **confirmação** , verifique se o que você configurou e selecione **criar** para criar o compartilhamento de servidor de arquivos.

13. Sobre o **resultados** , selecione Fechar se ele criou o compartilhamento.  Se ele não foi possível criar o compartilhamento, ele lhe dará os erros incorridos.

14. Escolher **fechar**.

15. Repita esse processo para todos os compartilhamentos adicionais.