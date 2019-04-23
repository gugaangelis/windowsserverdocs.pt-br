---
title: Migrar um servidor de arquivos usando o serviço de migração de armazenamento
description: Uma breve descrição do tópico para resultados da pesquisa
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 966f25eb0bd43513b3c544fb3dc97115ed668b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872747"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Use o serviço de migração de armazenamento para migrar um servidor

Este tópico aborda como migrar um servidor, incluindo seus arquivos e a configuração para outro servidor, usando [serviço de migração de armazenamento](overview.md) e Windows Admin Center. Migrando necessárias três etapas depois de ter instalado o serviço e abrir portas de firewall necessárias: inventário de seus servidores, transferir dados e transferência para os novos servidores.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Etapa 0: Instalar o serviço de migração de armazenamento e verifique as portas do firewall

Antes de começar, instale o serviço de migração de armazenamento e certifique-se de que as portas de firewall necessárias estão abertas.

1. Verifique as [requisitos de serviço de migração de armazenamento](overview.md#requirements) e instale [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) em seu PC ou um servidor de gerenciamento se você ainda não fez isso.
2. No Windows Admin Center do, conecte-se ao servidor orchestrator executando o Windows Server 2019. <br>Este é o servidor que você instalar o serviço de migração de armazenamento e usar para gerenciar a migração. Se você estiver migrando apenas um servidor, você pode usar o servidor de destino enquanto ele está em execução em Windows Server 2019. É recomendável que você use um servidor separado de orquestração para as migrações de vários servidores.
1. Vá para **Gerenciador de servidores** (no Windows Admin Center do) > **serviço de migração de armazenamento** e selecione **instalar** para instalar o serviço de migração de armazenamento e seus componentes necessários (mostrado na Figura 1).
    ![Captura de tela da página do serviço de migração de armazenamento, mostrando o botão instalar](media/migrate/install.png) **Figura 1: Instalando o serviço de migração de armazenamento**
1. Instale o proxy de serviço de migração de armazenamento em todos os servidores de destino executando o Windows Server 2019. Isso dobra a velocidade de transferência quando instalado em servidores de destino. <br>Para fazer isso, conecte-se ao servidor de destino no Windows Admin Center e, em seguida, vá para **Gerenciador de servidores** (no Windows Admin Center do) > **funções e recursos**, selecione **serviço de migração de armazenamento Proxy**e, em seguida, selecione **instalar**.
1. Em todos os servidores de origem e em quaisquer servidores de destino executando o Windows Server 2012 R2 ou Windows Server 2016, do Windows Admin Center, conecte-se a cada servidor, acesse **Gerenciador de servidores** (no Windows Admin Center do) > **Firewall**   >  **Regras de entrada**e, em seguida, verifique se as seguintes regras são habilitadas:
    - Compartilhamento de arquivos e de impressora (SMB-Entrada)
    - Serviço Netlogon (NP-In)
    - Instrumentação de gerenciamento do Windows (DCOM-In)
    - Instrumentação de Gerenciamento do Windows (WMI-In)

   > [!NOTE]
   > Se você estiver usando firewalls de terceiros, os intervalos de porta de entrada para abrir são TCP/445 (SMB), TCP/135 (Mapeador de ponto de extremidade RPC/DCOM) e TCP 1025 e 65535 (portas efêmeras do RPC/DCOM).

1. Se você estiver usando um servidor do orchestrator para gerenciar a migração e você deseja baixar um log de quais dados você transfere ou eventos, verifique que a regra de firewall (SMB-entrada) de compartilhamento de arquivo e impressora está habilitada no servidor também.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Etapa 1: Criar um trabalho e o inventário de seus servidores para descobrir o que migrar

Nesta etapa, você deve especificar quais servidores para migrar e, em seguida, verificá-los para coletar informações em seus arquivos e configurações.

1. Selecione **novo trabalho**, nomeie o trabalho e, em seguida, selecione **Okey**.
1. Sobre o **inserir credenciais** página, digite as credenciais de administrador que funcionam nos servidores que você deseja migrar e, em seguida, selecione **próxima**.
1. Selecione **adicionar um dispositivo**, digite um nome de servidor de origem e, em seguida, selecione **Okey**. <br>Repita essa etapa para todos os outros servidores que deseja inventariar.
1. Selecione **Start scan**.<br>As atualizações de página para mostra quando a verificação for concluída.
    ![Captura de tela mostrando um servidor pronto para ser examinado](media/migrate/inventory.png) **Figura 2: Inventário de servidores**
1. Selecione cada servidor para examinar os compartilhamentos, configuração, adaptadores de rede e os volumes que foram inventariados. <br><br>Serviço de migração de armazenamento não transferir arquivos ou pastas que sabemos que poderia interferir na operação do Windows, nesta versão, você verá avisos para qualquer compartilhamento localizado na pasta de sistema do Windows. Você terá a ignorar esses compartilhamentos durante a fase de transferência. Para obter mais informações, consulte [quais arquivos e pastas são excluídas de transferências de](faq.md#excluded-files).
1. Selecione **próxima** para passar para a transferência de dados.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Etapa 2: Transferir dados de seus servidores antigos para os servidores de destino

Nesta etapa você transferir dados depois de especificar onde colocá-lo nos servidores de destino.

 1. Sobre o **transferir dados** > **inserir credenciais** página, digite as credenciais de administrador que funcionam nos servidores de destino que você deseja migrar para o e, em seguida, selecione **próximo**.
 1. Sobre o **adicionar um dispositivo de destino e mapeamentos** é listada a página, o primeiro servidor de origem. Digite o nome do servidor ao qual você deseja migrar e, em seguida, selecione **verificar dispositivo**.
 1. Mapeie os volumes de origem para volumes de destino, desmarque a **Include** caixa de seleção para quaisquer outros compartilhamentos você não quiser transferir (incluindo qualquer compartilhamentos administrativos localizados na pasta de sistema do Windows) e, em seguida, selecione **Avançar** .
    ![Captura de tela mostrando um servidor de origem com volumes e compartilhamentos e onde eles serão transferidos no destino](media/migrate/transfer.png) **Figura 3: Um servidor de origem e de onde seu armazenamento será transferido para**
 1. Adicionar um servidor de destino e os mapeamentos para qualquer mais servidores de origem e, em seguida, selecione **próxima**.
 1. Opcionalmente, ajuste as configurações de transferência e, em seguida, selecione **próxima**.
 1. Selecione **Validate** e, em seguida, selecione **próxima**.
 1. Selecione **inicie a transferência** para iniciar a transferência de dados.<br>Na primeira vez que você transfere, podemos moverá todos os arquivos existentes em um destino para uma pasta de backup. Em transferências subsequentes, por padrão vamos atualizá o destino sem fazer o backup primeiro. <br>Além disso, o serviço de migração de armazenamento é inteligente o suficiente para lidar com a sobreposição de compartilhamentos — não copiamos as mesmas pastas duas vezes no mesmo trabalho.
 1. Após a transferência, confira o servidor de destino para garantir que tudo transferido corretamente. Selecione **log de erros apenas** se você quiser baixar um log de todos os arquivos que não foi transferida.

  > [!NOTE]
  > Se você quiser manter uma trilha de auditoria de transferências ou estiver planejando executar mais de uma transferência em um trabalho, clique em **log de transferência** para salvar uma cópia CSV. Todas as transferências subsequentes substitui as informações de banco de dados de uma execução anterior. 

Neste ponto, você tem três opções:

- **Vá para a próxima etapa**, cortando pela, de modo que os servidores de destino adotam as identidades dos servidores de origem.
- **Considere a migração completa** sem colocar sobre as identidades dos servidores de origem.
- **Transferir novamente**, copiando apenas os arquivos que foram atualizados desde a última transferência.

Se seu objetivo é sincronizar os arquivos com o Azure, você pode configurar os servidores de destino com a sincronização de arquivos do Azure após a transferência de arquivos, ou após a redução de failover para os servidores de destino (consulte [Planejando uma implantação de sincronização de arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>Etapa 3: Opcionalmente, transferir para os novos servidores

Nesta etapa você transferir nos servidores de origem para os servidores de destino, movendo os endereços IP e nomes de computador para os servidores de destino. Depois que essa etapa for concluída, aplicativos e usuários acessar os novos servidores por meio de nomes e endereços dos servidores que você migrou do.

 1. Se você depois de navegar para fora o trabalho de migração, do Windows Admin Center, vá para **Gerenciador de servidores** > **serviço de migração de armazenamento** e, em seguida, selecione o trabalho que você deseja concluir. 
 1. No **transferir para os novos servidores** > **inserir credenciais** página, selecione **próximo** para usar as credenciais que você digitou anteriormente.
 1. Sobre o **Configurar substituição** página, especifique quais adaptadores de rede para assumir o controle de configurações do dispositivo de cada origem. Isso move o endereço IP da origem para o destino como parte da substituição.
 1. Especifique qual endereço IP para usar para o servidor de origem após a substituição move seu endereço para o destino. Você pode usar o DHCP ou um endereço estático. Se usar um endereço estático, a nova sub-rede deve ser que o mesmo que a sub-rede antiga ou a substituição falhará.
    ![Captura de tela mostrando um servidor de origem e seus endereços IP e nomes de computador e o que elas serão substituídas por depois da substituição](media/migrate/cutover.png)
    **Figura 4: Um servidor de origem e como sua configuração de rede será movido para o destino**
 1. Especifique como renomear o servidor de origem depois que o servidor de destino tem sobre seu nome. Você pode usar um nome gerado aleatoriamente ou digitar um por conta própria. Em seguida, selecione **próxima**.
 1. Selecione **próxima** sobre o **ajustar as configurações de substituição** página.
 1. Selecione **Validate** no **dispositivo de origem e destino validar** página e, em seguida, selecione **próxima**.
 1. Quando estiver pronto para executar a substituição, selecione **iniciar substituição**. <br>Usuários e aplicativos podem ocorrer uma interrupção enquanto os nomes e endereços são movidos e os servidores reiniciado várias vezes cada um, mas caso contrário, não serão afetados pela migração. Quanto tempo leva substituição depende de quão rapidamente reiniciar os servidores, bem como tempos de replicação do Active Directory e DNS.

## <a name="see-also"></a>Consulte também

- [Visão geral do serviço de migração de armazenamento](overview.md)
- [Serviços de migração de armazenamento, perguntas frequentes (FAQ)](faq.md)
- [Planejando uma implantação de sincronização de arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)