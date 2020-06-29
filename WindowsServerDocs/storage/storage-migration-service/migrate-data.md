---
title: Migrar um servidor de arquivos usando o serviço de migração de armazenamento
description: Breve descrição do tópico para resultados do mecanismo de pesquisa
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 03/25/2020
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: e012706eeb9d483f19eff6f4ba2e1f57e0c0852d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475323"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Usar o serviço de migração de armazenamento para migrar um servidor

Este tópico discute como migrar um servidor, incluindo seus arquivos e a configuração, para outro servidor usando o [serviço de migração de armazenamento](overview.md) e o centro de administração do Windows. A migração leva três etapas depois que você instala o serviço e abriu as portas de firewall necessárias: inventariar seus servidores, transferir dados e passar para os novos servidores.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Etapa 0: instalar o serviço de migração de armazenamento e verificar as portas de firewall

Antes de começar, instale o serviço de migração de armazenamento e verifique se as portas de firewall necessárias estão abertas.

1. Verifique os [requisitos do serviço de migração de armazenamento](overview.md#requirements) e instale o [centro de administração do Windows](../../manage/windows-admin-center/understand/windows-admin-center.md) no computador ou em um servidor de gerenciamento, se ainda não tiver feito isso. Se estiver migrando computadores de origem ingressados no domínio, você deverá instalar e executar o serviço de migração de armazenamento em um servidor ingressado no mesmo domínio ou floresta que os computadores de origem.
2. No centro de administração do Windows, conecte-se ao servidor do Orchestrator que executa o Windows Server 2019. <br>Este é o servidor no qual você instalará o serviço de migração de armazenamento e usará o para gerenciar a migração. Se você estiver migrando apenas um servidor, poderá usar o servidor de destino contanto que ele esteja executando o Windows Server 2019. Recomendamos que você use um servidor de orquestração separado para qualquer migração de vários servidores.
3. Vá para **Gerenciador do servidor** (no centro de administração do Windows) > **serviço de migração de armazenamento** e selecione **instalar** para instalar o serviço de migração de armazenamento e seus componentes necessários (mostrados na Figura 1).
    ![Captura de tela da página do serviço de migração de armazenamento mostrando o botão instalar ](media/migrate/install.png) **Figura 1: Instalando o serviço de migração de armazenamento**
4. Instale o proxy do serviço de migração de armazenamento em todos os servidores de destino que executam o Windows Server 2019. Isso duplica a velocidade de transferência quando instalado nos servidores de destino. <br>Para fazer isso, conecte-se ao servidor de destino no centro de administração do Windows e vá para **Gerenciador do servidor** (no centro de administração do windows) > **funções e recursos**, > **recursos**, selecione proxy de serviço de **migração de armazenamento**e, em seguida, selecione **instalar**.
5. Se você pretende migrar de ou para clusters do Windows failover, instale as ferramentas de clustering de failover no servidor do Orchestrator. <br>Para fazer isso, conecte-se ao servidor do Orchestrator no centro de administração do Windows e vá para **Gerenciador do servidor** (no centro de administração do windows) > **funções e recursos**, > **recursos**, > **ferramentas de administração de servidor remoto**, > ferramentas de **Administração de recursos**, selecione **ferramentas de clustering de failover**e, em seguida, selecione **instalar**.
6. Em todos os servidores de origem e em todos os servidores de destino que executam o Windows Server 2012 R2 ou o Windows Server 2016, no centro de administração do Windows, conecte-se a cada servidor, vá para **Gerenciador do servidor** (no centro de administração do Windows) > regras de **Firewall**  >  **entrada**do firewall e verifique se as regras a seguir estão habilitadas:
    - Compartilhamento de arquivos e de impressora (SMB-Entrada)
    - Serviço Netlogon (NP-in)
    - DCOM-In (Instrumentação de Gerenciamento do Windows)
    - WMI-In (Instrumentação de Gerenciamento do Windows)

   Se você estiver usando firewalls de terceiros, os intervalos de portas de entrada a serem abertos serão TCP/445 (SMB), TCP/135 (mapeador de ponto de extremidade RPC/DCOM) e TCP 1025-65535 (portas efêmeras RPC/DCOM). As portas do serviço de migração de armazenamento são TCP/28940 (Orchestrator) e TCP/28941 (proxy).

7. Se você estiver usando um servidor Orchestrator para gerenciar a migração e desejar baixar eventos ou um log de quais dados você transfere, verifique se a regra de firewall de compartilhamento de arquivos e impressoras (SMB-in) está habilitada nesse servidor também.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Etapa 1: criar um trabalho e inventariar seus servidores para descobrir o que migrar

Nesta etapa, você especifica quais servidores migrar e, em seguida, os verifica para coletar informações sobre seus arquivos e configurações.

1. Selecione **novo trabalho**, nomeie o trabalho e, em seguida, selecione se deseja migrar servidores Windows e clusters ou servidores Linux que usam o samba. Depois, selecione **OK**.
2. Na página **Inserir credenciais** , digite as credenciais de administrador que funcionam nos servidores dos quais você deseja migrar e, em seguida, selecione **Avançar**. <br>Se você estiver migrando de servidores Linux, insira as credenciais nas páginas **credenciais do samba** e credenciais do **Linux** , incluindo uma senha ssh ou uma chave privada.

3. Selecione **Adicionar um dispositivo**, digite um nome do servidor de origem ou o nome de um servidor de arquivos clusterizado e, em seguida, selecione **OK**. <br>Repita esse procedimento para todos os outros servidores que você deseja inventariar.

4. Selecione **Iniciar verificação**.<br>A página é atualizada para mostrar quando a verificação foi concluída.
    ![Captura de tela mostrando um servidor pronto para ser examinado ](media/migrate/inventory.png) **Figura 2: servidores de inventário**
5. Selecione cada servidor para examinar os compartilhamentos, a configuração, os adaptadores de rede e os volumes que foram inventariados. <br><br>O serviço de migração de armazenamento não transferirá arquivos ou pastas que sabemos que podem interferir na operação do Windows. portanto, nesta versão, você verá avisos para todos os compartilhamentos localizados na pasta sistema do Windows. Você precisará ignorar esses compartilhamentos durante a fase de transferência. Para obter mais informações, consulte [quais arquivos e pastas são excluídos das transferências](faq.md#what-files-and-folders-are-excluded-from-transfers).
6. Selecione **Avançar** para passar para a transferência de dados.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Etapa 2: transferir dados dos servidores antigos para os servidores de destino

Nesta etapa, você transfere dados depois de especificar onde colocá-los nos servidores de destino.

1. Na página **transferir dados**  >  **Insira as credenciais** , digite as credenciais de administrador que funcionam nos servidores de destino para os quais você deseja migrar e, em seguida, selecione **Avançar**.
2. Na página **Adicionar um dispositivo de destino e mapeamentos** , o primeiro servidor de origem é listado. Digite o nome do servidor ou servidor de arquivos clusterizado para o qual você deseja migrar e, em seguida, selecione **verificar dispositivo**. Se estiver migrando de um computador de origem ingressado no domínio, o servidor de destino deverá ingressar no mesmo domínio. Você também pode clicar em "criar uma nova VM do Azure" e usar o assistente para implantar um novo servidor de destino no Azure. Isso dimensionará automaticamente a VM, provisionará o armazenamento, formatará os discos, ingressará no domínio e adicionará o proxy do serviço de migração de armazenamento a um destino do Windows Server 2019. Você pode escolher entre as VMs do Windows Server 2019 (recomendado), do Windows Server 2016 e do Windows Server 2012 R2 de qualquer tamanho e usar discos gerenciados.

    > [!NOTE]
    > Usar "criar uma nova VM do Azure" requer que você tenha:
    > - Uma assinatura válida do Azure.
    > - Um grupo de recursos de computação do Azure existente no qual você tem direitos de criação.
    > - Uma rede virtual do Azure e uma sub-rede existentes.
    > - Uma solução de VPN ou de rota expressa do Azure vinculada à rede virtual e à sub-rede que permite a conectividade dessa VM IaaS do Azure para seus clientes locais, controladores de domínio, o computador Orchestrator do serviço de migração de armazenamento, o computador do centro de administração do Windows e o computador de origem a ser migrado.

    Aqui está um vídeo mostrando como usar o serviço de migração de armazenamento para migrar para VMs do Azure.
    > [!VIDEO https://www.youtube-nocookie.com/embed/k8Z9LuVL0xQ]

3. Mapeie os volumes de origem para volumes de destino, desmarque a caixa de seleção **incluir** para todos os compartilhamentos que você não deseja transferir (incluindo quaisquer compartilhamentos administrativos localizados na pasta de sistema do Windows) e, em seguida, selecione **Avançar**.
   ![Captura de tela mostrando um servidor de origem e seus volumes e compartilhamentos e para onde eles serão transferidos no destino ](media/migrate/transfer.png) **Figura 3: um servidor de origem e onde seu armazenamento será transferido para**
4. Adicione um servidor de destino e mapeamentos para mais servidores de origem e, em seguida, selecione **Avançar**.
5. Na página **ajustar configurações de transferência** , especifique se deseja migrar usuários e grupos locais nos servidores de origem e, em seguida, selecione **Avançar**. Isso permite recriar quaisquer usuários e grupos locais nos servidores de destino para que as permissões de arquivo ou compartilhamento definidas para usuários e grupos locais não sejam perdidas. Aqui estão as opções ao migrar usuários e grupos locais:

    - **Renomear contas com o mesmo nome** é selecionado por padrão e migra todos os usuários e grupos locais no servidor de origem. Se ele encontrar usuários ou grupos locais com o mesmo nome na origem e no destino, ele os renomeará no destino, a menos que eles sejam internos (por exemplo, o usuário administrador e o grupo Administradores). Não use essa configuração se o servidor de origem ou de destino for um controlador de domínio.
    - **Reutilizar contas com o mesmo nome** mapeia usuários e grupos com nomes idênticos na origem e no destino. Não use essa configuração se o servidor de origem ou de destino for um controlador de domínio.
    - **Não transfira usuários e grupos** ignora a migração de usuários e grupos locais, o que é necessário quando sua origem ou destino é um controlador de domínio ou ao propagar dados para replicação do DFS (o replicação do DFS não dá suporte a grupos e usuários locais).

   > [!NOTE]
   > As contas de usuário migradas são desabilitadas no destino e recebem uma senha de 127 caracteres que são complexas e aleatórias, portanto, você precisará habilitá-las e atribuir uma nova senha quando terminar de usá-las. Isso ajuda a garantir que contas antigas com senhas esquecidas e fracas na fonte não continuem a ser um problema de segurança no destino. Você também pode querer fazer check-out da [solução de senha de administrador local (LAPs)](https://www.microsoft.com/download/details.aspx?id=46899) como uma maneira de gerenciar senhas de administrador local.

6. Selecione **validar** e, em seguida, **Avançar**.
7. Selecione **Iniciar transferência** para iniciar a transferência de dados.<br>Na primeira vez que você transferir, moveremos todos os arquivos existentes em um destino para uma pasta de backup. Em transferências subsequentes, por padrão, atualizaremos o destino sem fazer o backup primeiro. <br>Além disso, o serviço de migração de armazenamento é inteligente o suficiente para lidar com compartilhamentos sobrepostos — não copiaremos as mesmas pastas duas vezes no mesmo trabalho.
8. Depois que a transferência for concluída, verifique o servidor de destino para certificar-se de que tudo foi transferido corretamente. Selecione **log de erros somente** se desejar baixar um log de todos os arquivos que não foram transferidos.

   > [!NOTE]
   > Se você quiser manter uma trilha de auditoria de transferências ou se estiver planejando executar mais de uma transferência em um trabalho, clique em **transferir log** ou em outras opções de salvar log para salvar uma cópia CSV. Cada transferência subsequente substitui as informações do banco de dados de uma execução anterior.

Neste ponto, você tem três opções:

- **Vá para a próxima etapa**, recortando para que os servidores de destino adotem as identidades dos servidores de origem.
- **Considere a migração completa** sem assumir as identidades dos servidores de origem.
- **Transfira novamente**, copiando somente os arquivos que foram atualizados desde a última transferência.

Se seu objetivo for sincronizar os arquivos com o Azure, você poderá configurar os servidores de destino com Sincronização de Arquivos do Azure depois de transferir arquivos ou depois de passar para os servidores de destino (consulte [planejando uma implantação de sincronização de arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-cut-over-to-the-new-servers"></a>Etapa 3: recortar para os novos servidores

Nesta etapa, você reduz os servidores de origem para os servidores de destino, movendo os endereços IP e os nomes dos computadores para os servidores de destino. Depois que essa etapa for concluída, os aplicativos e usuários acessarão os novos servidores por meio dos nomes e endereços dos servidores dos quais você migrou.

1. Se você navegou para fora do trabalho de migração, no centro de administração do Windows, acesse **Gerenciador do servidor**  >  **serviço de migração de armazenamento** e selecione o trabalho que deseja concluir.
2. Na página **recortar para os novos servidores**  >  **Insira as credenciais** , selecione **Avançar** para usar as credenciais digitadas anteriormente.

   Se o destino for um servidor de arquivos clusterizado, talvez seja necessário fornecer credenciais com permissões para remover o cluster do domínio e, em seguida, adicioná-lo novamente com o novo nome.

3. Na página **Configurar a transferência** , especifique qual adaptador de rede no destino deve assumir as configurações de cada adaptador na origem. Isso move o endereço IP da origem para o destino como parte da transferência, fornecendo ao servidor de origem um novo endereço IP estático ou DHCP. Você tem a opção de ignorar todas as migrações de rede ou determinadas interfaces.
4. Especifique o endereço IP a ser usado para o servidor de origem após a transferência mover seu endereço para o destino. Você pode usar o DHCP ou um endereço estático. Se você estiver usando um endereço estático, a nova sub-rede deverá ser a mesma que a sub-rede antiga ou a transferência falhará.
    ![Captura de tela mostrando um servidor de origem e seus endereços IP e nomes de computador e o que eles serão substituídos após a transferência da ](media/migrate/cutover.png) **Figura 4: um servidor de origem e como sua configuração de rede será movida para o destino**
5. Especifique como renomear o servidor de origem após o servidor de destino assumir seu nome. Você pode usar um nome gerado aleatoriamente ou digitar um por conta própria. Em seguida, selecione **Avançar**.
6. Selecione **Avançar** na página **ajustar configurações de transferência** .
7. Selecione **validar** na página **validar dispositivo de origem e de destino** e, em seguida, selecione **Avançar**.
8. Quando estiver pronto para realizar a transferência, selecione **Iniciar transferência**. <br>Os usuários e aplicativos podem experimentar uma interrupção enquanto o endereço e os nomes são movidos e os servidores são reiniciados várias vezes cada, mas não serão afetados pela migração. Por quanto tempo a transferência demora depende da rapidez com que os servidores são reiniciados, bem como Active Directory e tempos de replicação de DNS.

## <a name="additional-references"></a>Referências adicionais

- [Visão geral do serviço de migração de armazenamento](overview.md)
- [Perguntas frequentes sobre serviços de migração de armazenamento](faq.md)
- [Planejando uma implantação da Sincronização de Arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)
