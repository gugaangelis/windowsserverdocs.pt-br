---
title: Implantando um servidor de arquivos clusterizado de dois nós
description: Este artigo descreve como criar um cluster de servidor de arquivos de dois nós
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 02/01/2019
ms.openlocfilehash: b91aeadcce645797f42a029f7a8c82371b42d618
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87968003"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Implantando um servidor de arquivos clusterizado de dois nós

> Aplica-se a: Windows Server 2019, Windows Server 2016

Um cluster de failover é um conjunto de computadores independentes que trabalham em conjunto para aumentar a disponibilidade de aplicativos e serviços. Os servidores clusterizados (chamados de nós) são conectados por cabos físicos e por software. Se um dos nós do cluster falhar, o outro nó começará a fornecer o serviço (um processo conhecido como failover). Os usuários vivenciam um mínimo de interrupções no serviço.

Este guia descreve as etapas para instalar e configurar um cluster de failover do servidor de arquivos de uso geral que tem dois nós. Ao criar a configuração neste guia, você pode aprender sobre clusters de failover e se familiarizar com a interface de snap-in de gerenciamento de cluster de failover no Windows Server 2019 ou no Windows Server 2016.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Visão geral de um cluster de servidores de arquivos com dois nós

Os servidores em um cluster de failover podem funcionar em uma variedade de funções, incluindo as funções de servidor de arquivos, servidor Hyper-V ou servidor de banco de dados, e podem fornecer alta disponibilidade para vários outros serviços e aplicativos. Este guia descreve como configurar um cluster de servidores de arquivos com dois nós.

Um cluster de failover normalmente inclui uma unidade de armazenamento conectada fisicamente a todos os servidores no cluster, embora qualquer volume no armazenamento somente seja acessado por um servidor de cada vez. O diagrama a seguir mostra um cluster de failover de dois nós conectado a uma unidade de armazenamento.

![Cluster de dois nós](media/Cluster-File-Server/Cluster-FS-Overview.png)

Volumes ou LUNs de armazenamento expostos aos nós em um cluster não devem ser expostos a outros servidores, inclusive a servidores em outro cluster. O diagrama a seguir ilustra isso.

![LUNs no armazenamento](media/Cluster-File-Server/Cluster-FS-LUNs.png)

Observe que para a disponibilidade máxima de qualquer servidor, é importante seguir práticas recomendadas para gerenciamento de servidores. Por exemplo, gerenciar cuidadosamente o ambiente físico dos servidores, testar alterações de software antes de implementá-las completamente e acompanhar as atualizações de software e as alterações de configuração em todos os servidores clusterizados.

O cenário a seguir descreve como pode ser configurado um cluster de failover de servidores de arquivos. Os arquivos que são compartilhados estão no armazenamento de cluster e qualquer servidor clusterizado pode agir como o servidor de arquivos que os compartilha.

## <a name="shared-folders-in-a-failover-cluster"></a>Pastas compartilhadas em um cluster de failover

A lista a seguir descreve a funcionalidade de configuração de pasta compartilhada que é integrada ao clustering de failover:

- A exibição tem como escopo somente pastas compartilhadas clusterizadas (sem misturar com pastas compartilhadas não clusterizadas): quando um usuário exibe pastas compartilhadas especificando o caminho de um servidor de arquivos clusterizado, a exibição incluirá apenas as pastas compartilhadas que fazem parte da função de servidor de arquivos específica. Ele excluirá pastas compartilhadas não clusterizadas e compartilhará parte das funções de servidor de arquivos separadas que estão em um nó do cluster.

- Enumeração baseada em acesso: você pode usar a enumeração baseada em acesso para ocultar uma pasta especificada da exibição dos usuários. Em vez de permitir que os usuários vejam a pasta, mas não acessem nada dela, é possível optar por impedi-los de ver a pasta. Você pode configurar a enumeração baseada em acesso para uma pasta compartilhada clusterizada da mesma maneira que para uma pasta compartilhada não clusterizada.

- Acesso offline: você pode configurar o acesso offline (Caching) para uma pasta compartilhada clusterizada da mesma maneira que para uma pasta compartilhada não clusterizada.

- Os discos clusterizados são sempre reconhecidos como parte do cluster: se você usa a interface de cluster de failover, o Windows Explorer ou o snap-in de gerenciamento de compartilhamento e armazenamento, o Windows reconhece se um disco foi designado como estando no armazenamento de cluster. Se esse disco já tiver sido configurado no gerenciamento de cluster de failover como parte de um servidor de arquivos clusterizado, você poderá usar qualquer uma das interfaces mencionadas anteriormente para criar um compartilhamento no disco. Se esse disco não tiver sido configurado como parte de um servidor de arquivos clusterizado, não será possível criar uma pasta nele por engano. Em vez disso, um erro indica que o disco deve primeiro ser configurado como parte de um servidor de arquivos clusterizado para poder ser compartilhado.

- Integração de serviços para o sistema de arquivos de rede: a função de servidor de arquivos no Windows Server inclui o serviço de função opcional chamado serviços para NFS (sistema de arquivos de rede). Ao instalar o serviço de função e configurar pastas compartilhadas com os Serviços de NFS, você pode criar um servidor de arquivos clusterizado que ofereça suporte aos clientes UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Requisitos de um cluster de failover com dois nós

Para que um cluster de failover no Windows Server 2016 ou no Windows Server 2019 seja considerado uma solução oficialmente suportada pela Microsoft, a solução deve atender aos critérios a seguir.

- Todos os componentes de hardware e software devem cumprir as qualificações do logotipo adequado. Para o Windows Server 2016, esse é o logotipo "certificado para o Windows Server 2016". Para o Windows Server 2019, esse é o logotipo "certificado para o Windows Server 2019". Para obter mais informações sobre quais sistemas de hardware e software foram certificados, visite o site do catálogo do Microsoft [Windows Server](https://www.windowsservercatalog.com/default.aspx) .

- A solução totalmente configurada (servidores, rede e armazenamento) deve passar em todos os testes no assistente de validação, que faz parte do snap-in de cluster de failover.

Os itens a seguir serão necessários para um cluster de failover de dois nós.

- **Servidores:** É recomendável usar computadores correspondentes com os mesmos ou componentes semelhantes.  Os servidores para um cluster de failover de dois nós devem executar a mesma versão do Windows Server. Eles também devem ter as mesmas atualizações de software (patches).

- **Adaptadores de rede e cabo:** O hardware de rede, como outros componentes na solução de cluster de failover, deve ser compatível com o Windows Server 2016 ou o Windows Server 2019. Se você usar o iSCSI, os adaptadores de rede deverão ser dedicados à comunicação de rede ou iSCSI, não ambos. Na infraestrutura de rede que conecta os nós do cluster, evite ter pontos de falha únicos. Há várias maneiras de realizar isso. É possível conectar os nós do cluster por várias redes distintas. Como alternativa, é possível conectar os nós do cluster com uma rede construída com adaptadores de rede emparelhados, comutadores redundantes, roteadores redundantes ou hardware semelhante que remova pontos de falha únicos.

   > [!NOTE]
   > Se os nós de cluster estiverem conectados a uma única rede, a rede passará o requisito de redundância no Assistente para validar uma configuração.  No entanto, o relatório incluirá um aviso informando que a rede não deve ter um ponto único de falha.

- **Controladores de dispositivo ou adaptadores apropriados para armazenamento:**
    - **SCSI ou Fibre Channel serial anexados:** Se você estiver usando SCSI anexado serial ou Fibre Channel, em todos os servidores clusterizados, todos os componentes da pilha de armazenamento deverão ser idênticos. É necessário que os componentes de software e DSM (módulo específico de dispositivo) e de software de e/s (MPIO) de vários caminhos sejam idênticos.  É recomendável que os controladores de dispositivo de armazenamento em massa — ou seja, a controladora, os drivers da controladora e o firmware da controladora — que estão conectados ao armazenamento clusterizado sejam idênticos. Se usar HBAs diferentes, você deverá verificar, com o fornecedor de armazenamento, se está cumprindo as configurações com suporte ou recomendadas.
    - **iSCSI:** Se você estiver usando iSCSI, cada servidor clusterizado deverá ter um ou mais adaptadores de rede ou adaptadores de barramento de host dedicados ao armazenamento ISCSI. A rede usada para iSCSI não deve ser usada para comunicação de rede. Em todos os servidores clusterizados, os adaptadores de rede usados para conexão com o destino de armazenamento iSCSI devem ser idênticos. É recomendável também usar Gigabit Ethernet ou superior.

- **Armazenamento:** Você deve usar o armazenamento compartilhado que é certificado para o Windows Server 2016 ou o Windows Server 2019.

    Para um cluster de failover de dois nós, o armazenamento deve conter pelo menos dois volumes separados (LUNs) se estiver usando um disco de testemunha para quorum. O disco testemunha é um disco no armazenamento de cluster designado para manter uma cópia do banco de dados de configuração do cluster. Para este exemplo de cluster de dois nós, a configuração de quorum será a maioria dos nós e discos. A configuração Maioria dos Nós e Discos significa que os nós e o disco testemunha contêm, cada um, cópias da configuração do cluster, e que o cluster tem quorum desde que a maioria (duas entre três) dessas cópias esteja disponível. O outro volume (LUN) conterá os arquivos que estão sendo compartilhados para os usuários.

    Os requisitos de armazenamento incluem o seguinte:

    - Para usar o suporte de disco nativo incluído no cluster de failover, use discos básicos, não discos dinâmicos.
    - É recomendável formatar as partições com NTFS (para o disco testemunha, a partição deve ser NTFS).
    - Para o estilo de partição do disco, é possível usar MBR (registro mestre de inicialização) ou GPT (tabela de partição GUID).
    - O armazenamento deve responder corretamente a comandos SCSI específicos, o armazenamento deve seguir o padrão chamado de comandos primários SCSI-3 (SPC-3). Em particular, o armazenamento deve oferecer suporte a Reservas Persistentes, como especificado no padrão SPC-3.
    -  O driver de miniporta usado para o armazenamento deve funcionar com o driver de armazenamento do Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implantando redes de área de armazenamento com clusters de failover

Ao implantar uma SAN (rede de área de armazenamento) com um cluster de failover, as diretrizes a seguir devem ser observadas.

- **Confirme a certificação do armazenamento:** Usando o site do [catálogo do Windows Server](https://www.windowsservercatalog.com/default.aspx) , confirme se o armazenamento do fornecedor, incluindo drivers, firmware e software, é certificado para o windows Server 2016 ou o windows Server 2019.

- **Isolar dispositivos de armazenamento, um cluster por dispositivo:** Os servidores de clusters diferentes não devem ser capazes de acessar os mesmos dispositivos de armazenamento. Na maioria dos casos, um LUN usado para um conjunto de servidores de cluster deve ser isolado de todos os outros servidores por meio de mascaramento ou zoneamento de LUN.

- **Considere O uso do software de e/s de vários caminhos:** Em uma malha de armazenamento altamente disponível, você pode implantar clusters de failover com vários adaptadores de barramento de host usando O software de e/s multipath. Isso fornece o mais alto nível de redundância e disponibilidade. A solução de vários caminhos deve ser baseada no Microsoft Multipath I/O (MPIO). O fornecedor de hardware de armazenamento pode fornecer um DSM (módulo específico de dispositivo) do MPIO para seu hardware, embora o Windows Server 2016 e o Windows Server 2019 incluam um ou mais DSMs como parte do sistema operacional.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Requisitos de infraestrutura de rede e conta de domínio

Será necessária a infraestrutura de rede a seguir para um cluster de failover com dois nós e uma conta administrativa com as seguintes permissões de domínio:

- **Configurações de rede e endereços IP:** Quando você usa adaptadores de rede idênticos para uma rede, também usa configurações de comunicação idênticas nesses adaptadores (por exemplo, velocidade, modo duplex, controle de fluxo e tipo de mídia). Além disso, compare as configurações entre o adaptador de rede e o comutador com o qual ele se conecta e verifique se não há nenhuma configuração em conflito.

    Se você tiver redes privadas que não sejam roteadas para o restante de sua infraestrutura de rede, verifique se cada uma dessas redes privadas usa uma sub-rede exclusiva. Isso será necessário mesmo que você dê a cada adaptador de rede um endereço IP exclusivo. Por exemplo, se você tiver um nó de cluster em uma matriz que use uma rede física e outro nó em uma filial que use uma rede física separada, não especifique 10.0.0.0/24 para as duas redes, mesmo que você atribua a cada adaptador um endereço IP exclusivo.

    Para obter mais informações sobre os adaptadores de rede, consulte Requisitos de hardware para um cluster de failover com dois nós, anteriormente neste guia.

- **DNS:** Os servidores no cluster devem usar o DNS (sistema de nomes de domínio) para a resolução de nomes. O protocolo de atualização dinâmica de DNS pode ser usado.

- **Função de domínio:** Todos os servidores no cluster devem estar no mesmo domínio Active Directory. Como prática recomendada, todos os servidores clusterizados devem ter a mesma função de domínio (servidor membro ou controlador de domínio). A função recomendada é servidor membro.

- **Controlador de domínio:** Recomendamos que os servidores clusterizados sejam servidores membro. Se forem, será necessário um servidor adicional que atue como controlador de domínio no domínio que contém o cluster de failover.

- **Clientes:** Conforme necessário para o teste, você pode conectar um ou mais clientes em rede ao cluster de failover que você cria e observar o efeito em um cliente quando você move ou faz o failover do servidor de arquivos clusterizado de um nó de cluster para o outro.

- **Conta para administrar o cluster:** Ao criar um cluster ou adicionar servidores a ele pela primeira vez, você deve estar conectado ao domínio com uma conta que tenha direitos de administrador e permissões em todos os servidores desse cluster. A conta não precisa ser uma conta Admins. do Domínio, mas poderá ser uma conta Usuários do Domínio que estiver no grupo Administradores em cada servidor com cluster. Além disso, se a conta não for uma conta admins. do domínio, a conta (ou o grupo do qual a conta é membro) deverá receber as permissões **criar objetos de computador** e **ler todas as propriedades** na unidade organizacional do domínio (UO) que residirá no.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Etapas para instalar um cluster de servidores de arquivos com dois nós

Você precisa concluir as etapas a seguir para instalar um cluster de failover de servidores de arquivos com dois nós.

Etapa 1: conectar os servidores de cluster às redes e ao armazenamento

Etapa 2: instalar o recurso de cluster de failover

Etapa 3: validar a configuração do cluster

Etapa 4: criar o cluster

Se você já instalou os nós de cluster e deseja configurar um cluster de failover de servidor de arquivos, consulte Etapas para configurar um cluster de servidores de arquivos com dois nós, mais adiante neste guia.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Etapa 1: conectar os servidores de cluster às redes e ao armazenamento

Para uma rede de cluster de failover, evite ter pontos de falha únicos. Há várias maneiras de realizar isso. É possível conectar os nós do cluster por várias redes distintas. Como alternativa, você pode conectar seus nós do cluster a uma rede construída com adaptadores de rede emparelhados, comutadores redundantes, roteadores redundantes ou hardware semelhante, capazes de remover pontos de falha únicos (se você usar uma rede para iSCSI, deverá criar essa rede em acréscimo às outras redes).

Em um cluster de servidores de arquivos com dois nós, quando você conectar os servidores ao armazenamento do cluster, deverá expor pelo menos dois volumes (LUNs). Você pode expor volumes adicionais quando necessário para testes completos da sua configuração. Não exponha os volumes clusterizados a servidores que não fazem parte do cluster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Para conectar os servidores de cluster às redes e ao armazenamento

1. Examine os detalhes sobre redes em requisitos de hardware para um cluster de failover de dois nós e requisitos de conta de domínio e de infraestrutura de rede para um cluster de failover de dois nós, anteriormente neste guia.

2. Conecte e configure as redes que serão usadas pelos servidores no cluster.

3. Se sua configuração de teste incluir clientes ou um controlador de domínio não clusterizado, verifique se esses computadores podem conectar-se aos servidores clusterizados por meio de pelo menos uma rede.

4. Siga as instruções do fabricante para conectar fisicamente os servidores ao armazenamento.

5. Verifique se os discos (LUNs) que deseja usar no cluster são expostos aos servidores que você irá clusterizar (e apenas a esses servidores). É possível usar qualquer uma das interfaces a seguir para expor discos ou LUNs:

    - A interface fornecida pelo fabricante do armazenamento.

    - Se estiver usando iSCSI, uma interface iSCSI apropriada.

6. Se você comprou um software que controla o formato ou a função do disco, siga as instruções do fornecedor sobre como usar esse software com o Windows Server.

7. Em um dos servidores que você deseja incluir no cluster, clique em Iniciar, Ferramentas Administrativas, Gerenciamento do Computador e Gerenciamento de Disco. (Se a caixa de diálogo controle de conta de usuário for exibida, confirme se a ação exibida é o que você deseja e clique em continuar.) Em gerenciamento de disco, confirme se os discos de cluster estão visíveis.

8. Se desejar ter um novo volume de armazenamento com mais de 2 terabytes e estiver usando a interface do Windows para controlar o formato do disco, converta esse disco no estilo de partição chamado de GPT (tabela de partição GUID). Para fazer isso, faça o backup de todos os dados do disco, exclua todos os volumes do disco e, em Gerenciamento de Disco, clique com o botão direito do mouse no disco (não em uma partição) e clique em Converter em Disco GPT.  Para volumes menores que 2 terabytes, em vez de usar a GGT, você pode usar o estilo de partição chamado MBR (registro mestre de inicialização).

9. Verifique o formato de qualquer volume ou LUN exposto. É recomendável usar NTFS como formato (para o disco testemunha, é preciso usar NTFS).

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Etapa 2: instalar a função de servidor de arquivos e o recurso de cluster de failover

Nesta etapa, a função de servidor de arquivos e o recurso de cluster de failover serão instalados. Ambos os servidores devem estar executando o Windows Server 2016 ou o Windows Server 2019.

#### <a name="using-server-manager"></a>Usando Gerenciador do Servidor

1. Abra **Gerenciador do servidor** e, na lista suspensa **gerenciar** , selecione **adicionar funções e recursos**.

   ![Adicionar recurso](media/Cluster-File-Server/Cluster-FS-Add-Feature.png)

2. Se a janela **antes de começar** for aberta, escolha **Avançar**.

3. Para o **tipo de instalação**, selecione instalação baseada em **função ou recurso** e **Avançar**.

4. Verifique se **selecionar um servidor do pool de servidores** está selecionado, se o nome do computador está realçado e **próximo**.

5. Para a função de servidor, na lista de funções, abra **serviços de arquivos**, selecione **servidor de arquivos**e **Avançar**.

   ![Adicionar função](media/Cluster-File-Server/Cluster-FS-Add-FS-Role-1.png)

6. Para os recursos, na lista de recursos, selecione **clustering de failover**.  Uma caixa de diálogo pop-up mostrará que lista as ferramentas de administração também estão sendo instaladas.  Mantenha todas as selecionadas, escolha **Adicionar recursos** e **Avançar**.

   ![Adicionar recurso](media/Cluster-File-Server/Cluster-FS-Add-WSFC-1.png)

7. Na página Confirmação, selecione Instalar.

8. Quando a instalação for concluída, reinicie o computador.

9. Repita as etapas no segundo computador.

#### <a name="using-powershell"></a>Usando o PowerShell

1. Abra uma sessão administrativa do PowerShell clicando com o botão direito do mouse em Iniciar e selecionando **Windows PowerShell (administrador)**.
2. Para instalar a função de servidor de arquivos, execute o comando:

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Para instalar o recurso de clustering de failover e suas ferramentas de gerenciamento, execute o comando:

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Depois que eles forem concluídos, você poderá verificar se eles estão instalados com os comandos:

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Depois de verificar se eles estão instalados, reinicie o computador com o comando:

    ```PowerShell
    Restart-Computer
    ```

6. Repita as etapas no segundo servidor.

### <a name="step-3-validate-the-cluster-configuration"></a>Etapa 3: validar a configuração do cluster

Antes de criar um cluster, é recomendável validar a configuração. A validação ajuda a confirmar se a configuração dos servidores, da rede e do armazenamento atendem a um conjunto específico de requisitos para clusters de failover.

#### <a name="using-failover-cluster-manager"></a>Usando o Gerenciador de Cluster de Failover

1. Em **Gerenciador do servidor**, escolha a lista suspensa **ferramentas** e selecione **Gerenciador de cluster de failover**.

2. Em **Gerenciador de cluster de failover**, vá para a coluna do meio em **Gerenciamento** e escolha **validar configuração**.

3. Se a janela **antes de começar** for aberta, escolha **Avançar**.

4. Na janela **selecionar servidores ou um cluster** , adicione os nomes dos dois computadores que serão os nós do cluster.  Por exemplo, se os nomes forem NODE1 e NODE2, insira o nome e selecione **Adicionar**.  Você também pode escolher o botão **procurar** para pesquisar Active Directory nomes.  Depois que ambos estiverem listados em **servidores selecionados**, escolha **Avançar**.

5. Na janela **Opções de teste** , selecione **executar todos os testes (recomendado)** e **Avançar**.

6. Na página de **confirmação** , ele fornecerá a listagem de todos os testes que serão verificados.  Escolha **Avançar** e os testes serão iniciados.

7. Depois de concluído, a página **Resumo** é exibida após a execução dos testes. Para exibir tópicos da Ajuda para ajudar a interpretar os resultados, clique em **Mais informações sobre os testes de validação de cluster**.

8. Ainda na página Resumo, clique em Exibir relatório e leia os resultados do teste. Faça as alterações necessárias na configuração e execute novamente os testes. <br>Para exibir os resultados dos testes depois de fechar o assistente, consulte *data do relatório SystemRoot\Cluster\Reports\Validation e time.html*.

9. Para exibir os tópicos da Ajuda sobre a validação de cluster depois de fechar o assistente, em Gerenciamento de Cluster de Failover, clique em Ajuda, Tópicos da Ajuda, guia Conteúdo, expanda o conteúdo da Ajuda do cluster de failover e clique em Validando a Configuração de um Cluster de Failover.

#### <a name="using-powershell"></a>Usando o PowerShell

1. Abra uma sessão administrativa do PowerShell clicando com o botão direito do mouse em Iniciar e selecionando **Windows PowerShell (administrador)**.

2. Para validar os computadores (por exemplo, os nomes dos computadores sendo NODE1 e NODE2) para clustering de failover, execute o comando:

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Para exibir os resultados dos testes depois de fechar o assistente, consulte o arquivo especificado (em SystemRoot\Cluster\Reports \) , faça as alterações necessárias na configuração e execute novamente os testes.

Para obter mais informações, consulte [Validando uma configuração de cluster de failover](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Etapa 4: criar o cluster

O seguinte procedimento criará um cluster fora dos computadores e da configuração que você tem.

#### <a name="using-failover-cluster-manager"></a>Usando o Gerenciador de Cluster de Failover

1. Em **Gerenciador do servidor**, escolha a lista suspensa **ferramentas** e selecione **Gerenciador de cluster de failover**.

2. Em **Gerenciador de cluster de failover**, vá para a coluna do meio em **Gerenciamento** e escolha **criar cluster**.

3. Se a janela **antes de começar** for aberta, escolha **Avançar**.

4. Na janela **selecionar servidores** , adicione os nomes dos dois computadores que serão os nós do cluster.  Por exemplo, se os nomes forem NODE1 e NODE2, insira o nome e selecione **Adicionar**.  Você também pode escolher o botão **procurar** para pesquisar Active Directory nomes.  Depois que ambos estiverem listados em **servidores selecionados**, escolha **Avançar**.

5. Na janela **ponto de acesso para administrar o cluster** , insira o nome do cluster que você usará.  Observe que esse não é o nome que você usará para se conectar aos seus compartilhamentos de arquivos com o.  Isso é para simplesmente administrar o cluster.

   > [!NOTE]
   > Se você estiver usando endereços IP estáticos, será necessário selecionar a rede a ser usada e inserir o endereço IP que será usado para o nome do cluster.  Se você estiver usando o DHCP para seus endereços IP, o endereço IP será configurado automaticamente para você.

6. Escolha **Próxima**.

7. Na página **confirmação** , verifique o que você configurou e selecione **Avançar** para criar o cluster.

8. Na página **Resumo** , ele fornecerá a configuração que ele criou.  Você pode selecionar Exibir relatório para ver o relatório da criação.

#### <a name="using-powershell"></a>Usando o PowerShell

1. Abra uma sessão administrativa do PowerShell clicando com o botão direito do mouse em Iniciar e selecionando **Windows PowerShell (administrador)**.

2. Execute o comando a seguir para criar o cluster se você estiver usando endereços IP estáticos.  Por exemplo, os nomes de computador são NODE1 e NODE2, o nome do cluster será CLUSTER e o endereço IP será 1.1.1.1.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Execute o comando a seguir para criar o cluster se você estiver usando o DHCP para endereços IP.  Por exemplo, os nomes dos computadores são NODE1 e NODE2, e o nome do cluster será CLUSTER.

   ```PowerShell
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Etapas para configurar um cluster de failover do servidor de arquivos

Para configurar um cluster de failover do servidor de arquivos, siga as etapas abaixo.

1. Em **Gerenciador do servidor**, escolha a lista suspensa **ferramentas** e selecione **Gerenciador de cluster de failover**.

2. Quando Gerenciador de Cluster de Failover é aberto, ele deve trazer automaticamente o nome do cluster que você criou.  Caso contrário, vá para a coluna do meio em **Gerenciamento** e escolha **conectar ao cluster**.  Insira o nome do cluster que você criou e **OK**.

3. Na árvore de console, clique no sinal ">" ao lado do cluster que você criou para expandir os itens abaixo dele.

4. Clique com o botão direito do mouse em **funções** e selecione **Configurar função**.

5. Se a janela **antes de começar** for aberta, escolha **Avançar**.

6. Na lista de funções, escolha **servidor de arquivos** e **Avançar**.

7. Para o tipo de servidor de arquivos, selecione **servidor de arquivos para uso geral** e **Avançar**.<br>Para obter informações sobre Servidor de Arquivos de Escalabilidade Horizontal, consulte [servidor de arquivos de escalabilidade horizontal visão geral](sofs-overview.md).

   ![Tipo de servidor de arquivos](media/Cluster-File-Server/Cluster-FS-File-Server-Type.png)

8. Na janela **ponto de acesso para cliente** , insira o nome do servidor de arquivos que você usará.  Observe que esse não é o nome do cluster.  Isso é para a conectividade de compartilhamento de arquivos.  Por exemplo, se eu quiser me conectar ao \\ servidor, o nome reemitido seria servidor.

   > [!NOTE]
   > Se você estiver usando endereços IP estáticos, será necessário selecionar a rede a ser usada e inserir o endereço IP que será usado para o nome do cluster.  Se você estiver usando o DHCP para seus endereços IP, o endereço IP será configurado automaticamente para você.

6. Escolha **Próxima**.

7. Na janela **selecionar armazenamento** , selecione a unidade adicional (não a testemunha) que manterá seus compartilhamentos e **em seguida**.

8. Na página **confirmação** , verifique sua configuração e selecione **Avançar**.

9. Na página **Resumo** , ele fornecerá a configuração que ele criou.  Você pode selecionar Exibir relatório para ver o relatório da criação da função de servidor de arquivos.

10. Em **funções** na árvore de console, você verá a nova função criada listada como o nome que você criou.  Com ele realçado, no painel **ações** à direita, escolha **Adicionar um compartilhamento**.

11. Execute o assistente de compartilhamento, inserindo o seguinte:

    - Tipo de compartilhamento será
    - Local/caminho em que a pasta compartilhada será
    - O nome dos usuários do compartilhamento se conectarão ao
    - Configurações adicionais, como enumeração baseada em acesso, Caching, criptografia, etc
    - Permissões de nível de arquivo se elas forem diferentes dos padrões

12. Na página **confirmação** , verifique o que você configurou e selecione **criar** para criar o compartilhamento do servidor de arquivos.

13. Na página **resultados** , selecione fechar se ele criou o compartilhamento.  Se não foi possível criar o compartilhamento, ele apresentará os erros incorridos.

14. Escolha **Fechar**.

15. Repita esse processo para quaisquer compartilhamentos adicionais.