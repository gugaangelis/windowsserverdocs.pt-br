---
title: Etapa 4 – definir configurações de Política de Grupo para Atualizações Automáticas
description: Tópico Windows Server Update Service (WSUS) – definir configurações de Política de Grupo para Atualizações Automáticas é a etapa quatro em um processo de quatro etapas para implantar o WSUS
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a01d8881e8f0f7ca6feff691938f926a12460db0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361656"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Etapa 4: Definir as configurações da política de grupo para atualizações automáticas

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em um ambiente do Active Directory, você pode usar Política de Grupo para definir como computadores e usuários (mencionados neste documento como clientes do WSUS) podem interagir com as atualizações do Windows para obter atualizações automáticas do Windows Server Update Services (WSUS).

Este tópico contém duas seções principais:

[Política de grupo configurações para atualizações do cliente do WSUS](#group-policy-settings-for-wsus-client-updates), que fornece orientações prescritivas e detalhes comportamentais sobre as configurações de Windows Update e Agendador de manutenção de política de grupo que controlam como os clientes WSUS podem interagir com Windows Update para obter atualizações automáticas.

As [informações complementares](#supplemental-information) têm as seguintes seções:

-   [Acessar as configurações de Windows Update no política de grupo](#accessing-the-windows-update-settings-in-group-policy), que fornece orientação geral sobre como usar o editor de gerenciamento política de grupo e informações sobre como acessar as configurações de extensões de política de serviços de atualização e o Agendador de manutenção no política de grupo.

-   [Alterações no WSUS relevantes para este guia](#changes-to-wsus-relevant-to-this-guide): para administradores familiarizados com o WSUS 3,2 e versões anteriores, esta seção fornece um breve resumo das principais diferenças entre a versão atual e a anterior do WSUS relevantes para este guia.

-   [Termos e definições](#terms-and-definitions): definições para vários termos pertencentes ao WSUS e aos serviços de atualização que são usados neste guia.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Configurações de Política de Grupo para atualizações do cliente do WSUS
Esta seção fornece informações sobre três extensões de Política de Grupo. Nessas extensões, você encontrará as configurações que você pode usar para configurar como os clientes WSUS podem interagir com Windows Update para receber atualizações automáticas.

-   [Configuração do computador &gt; configurações de política de Windows Update](#computer-configuration--windows-update-policy-settings)

-   [Configuração do computador &gt; configurações de política do Agendador de manutenção](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuração do usuário &gt; configurações de política de Windows Update](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> Este tópico pressupõe que você já usa o e está familiarizado com o Política de Grupo. Se você não estiver familiarizado com Política de Grupo, é recomendável revisar as informações na seção [informações complementares](#supplemental-information) deste documento antes de tentar definir as configurações de política para o WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuração do computador > configurações de política de Windows Update
Esta seção fornece detalhes sobre as seguintes configurações de política baseadas em computador:

-   [Permitir Atualizações Automáticas instalação imediata](#allow-automatic-updates-immediate-installation)

-   [Permitir que não administradores recebam notificações de atualização](#allow-non-administrators-to-receive-update-notifications)

-   [Permitir atualizações assinadas de um local do serviço de atualização da intranet da Microsoft](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Frequência de detecção de Atualizações Automáticas](#automatic-updates-detection-frequency)

-   [Configurar Atualizações Automáticas](#configure-automatic-updates)

-   [Atrasar reinicialização para instalações agendadas](#delay-restart-for-scheduled-installations)

-   [Não ajuste a opção padrão para "instalar atualizações e desligar" na caixa de diálogo desligar o Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Não exibir a opção "instalar atualizações e desligar" na caixa de diálogo desligar o Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Habilitar direcionamento do lado do cliente](#enable-client-side-targeting)

-   [Habilitando o gerenciamento de energia Windows Update para ativar o computador automaticamente para instalar atualizações agendadas](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [Nenhuma reinicialização automática com usuários conectados para instalações de atualizações automáticas agendadas](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Solicitar novamente a reinicialização com instalações agendadas](#re-prompt-for-restart-with-scheduled-installations)

-   [Reagendar Atualizações Automáticas instalações agendadas](#reschedule-automatic-updates-scheduled-installations)

-   [Especificar o local do serviço de atualização na intranet da Microsoft](#specify-intranet-microsoft-update-service-location)

-   [Ativar as atualizações recomendadas via Atualizações Automáticas](#turn-on-recommended-updates-via-automatic-updates)

-   [Ativar notificações de software](#turn-on-software-notifications)

No GPME, as políticas de Windows Update para a configuração baseada em computador estão localizadas em caminho: *política* > **configuração do computador** > **políticas** > **modelos administrativos** > **componentes do Windows** ** > Windows Update.**

> [!NOTE]
> Por padrão, essas configurações não são definidas.

#### <a name="allow-automatic-updates-immediate-installation"></a>Permitir instalação imediata de Atualizações Automáticas
Especifica se Atualizações Automáticas instalará automaticamente as atualizações que não interrompem os serviços do Windows ou reiniciam o Windows.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> se a configuração de política "Configurar Atualizações Automáticas" estiver definida como **desabilitada**, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que as atualizações não são instaladas imediatamente. Os administradores locais podem alterar essa configuração usando o editor de Política de Grupo local.|
|**Habilitada**|Especifica que Atualizações Automáticas instala imediatamente as atualizações depois que elas são baixadas e prontas para serem instaladas.|
|**Desabilitada**|Especifica que as atualizações não são instaladas imediatamente.|

**Opções:** Não há opções para essa configuração.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Permitir que não administradores recebam notificações de atualização
Especifica se usuários não administrativos receberão notificações de atualização com base na configuração de política de Atualizações Automáticas de definição.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Consulte os detalhes na tabela abaixo.|

> [!NOTE]
> se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada ou não estiver configurada, essa configuração de política não terá nenhum efeito.

> [!IMPORTANT]
> a partir do Windows 8 e do Windows RT, essa configuração de política é habilitada por padrão. Em todas as versões anteriores do Windows, ele é desabilitado por padrão.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que os usuários sempre verão uma janela de controle de conta e exigem permissões elevadas para realizar essas tarefas. Um administrador local pode alterar essa configuração usando o editor de Política de Grupo local.|
|**Habilitada**|Especifica que a atualização automática do Windows e o Microsoft Update incluirão não administradores ao determinar qual usuário conectado receberá notificações de atualização. Usuários não administrativos poderão instalar todo o conteúdo de atualização opcional, recomendado e importante para o qual receberam uma notificação. Os usuários não verão uma janela de controle de conta de usuário e não precisarão de permissões elevadas para instalar essas atualizações, exceto no caso de atualizações que contêm interface do usuário, contrato de licença de usuário final ou alterações de configuração de Windows Update.<br /><br />Há duas situações em que o efeito dessa configuração depende do computador operacional:<br /><br />1. **ocultar** ou **restaurar** atualizações<br />2. **Cancelar** uma instalação de atualização<br /><br />No Windows Vista ou no Windows XP, se essa configuração de política estiver habilitada, os usuários não verão uma janela de controle de conta de usuário e não precisarão de permissões elevadas para ocultar, restaurar ou cancelar atualizações.<br /><br />No Windows Vista, se essa configuração de política estiver habilitada, os usuários não verão uma janela de controle de conta de usuário e não precisarão de permissões elevadas para ocultar, restaurar ou cancelar atualizações. Se essa configuração de política não estiver habilitada, os usuários sempre verão uma janela de controle de conta e precisarão de permissões elevadas para ocultar, restaurar ou cancelar atualizações.<br /><br />No Windows 7, essa configuração de política não tem nenhum efeito. Os usuários sempre verão uma janela de controle de conta e precisarão de permissões elevadas para realizar essas tarefas.<br /><br />No Windows 8 e no Windows RT, essa configuração de política não tem nenhum efeito.|
|**Desabilitada**|Especifica que somente administradores conectados recebem notificações de atualização. **Observação:** No Windows 8 e no Windows RT, essa configuração de política é habilitada por padrão. Em todas as versões anteriores do Windows, ele é desabilitado por padrão.|

**Opções:** Não há opções para essa configuração.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Permitir atualizações assinadas de um local do serviço de atualização na intranet da Microsoft
Especifica se Atualizações Automáticas aceita atualizações que são assinadas por entidades diferentes da Microsoft quando a atualização é encontrada em um local de serviço de atualização da intranet da Microsoft.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Atualizações de um serviço que não seja um serviço de atualização da Microsoft na intranet devem ser sempre assinadas pela Microsoft e não são afetadas por essa configuração de política.

> [!NOTE]
> Não há suporte para essa política no Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

**Opções:** Não há opções para essa configuração.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que as atualizações de um local de serviço de atualização da intranet da Microsoft devem ser assinadas pela Microsoft.|
|**Habilitada**|Especifica que Atualizações Automáticas aceita atualizações recebidas por meio de um local de serviço de atualização da intranet da Microsoft se elas forem assinadas por um certificado encontrado no repositório de certificados "editores confiáveis" do computador local.|
|**Desabilitada**|Especifica que as atualizações de um local de serviço de atualização da intranet da Microsoft devem ser assinadas pela Microsoft.|

**Opções:** Não há opções para essa configuração.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Sempre reiniciar automaticamente no horário agendado
Especifica se um temporizador de reinicialização sempre será iniciado imediatamente depois que Windows Update instalar atualizações importantes, em vez de notificar primeiro os usuários na tela de entrada por pelo menos dois dias.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> se a configuração de política "nenhuma reinicialização automática com usuários conectados para instalações de atualizações automáticas agendadas" estiver habilitada, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que Windows Update não alterará o comportamento de reinicialização do computador.|
|**Habilitada**|Especifica que um temporizador de reinicialização sempre será iniciado imediatamente depois que Windows Update instalar atualizações importantes, em vez de notificar primeiro os usuários na tela de entrada por pelo menos dois dias.<br /><br />O temporizador de reinicialização pode ser configurado para iniciar com qualquer valor de 15 a 180 minutos. Quando o temporizador for esgotado, a reinicialização continuará mesmo que o computador tenha usuários conectados.|
|**Desabilitada**|Especifica que Windows Update não alterará o comportamento de reinicialização do computador.|

**Opções:** se essa configuração estiver habilitada, você poderá especificar a quantidade de tempo que o decorrerá depois que as atualizações forem instaladas antes que ocorra uma reinicialização forçada do computador.

#### <a name="automatic-updates-detection-frequency"></a>Frequência de detecção de Atualizações Automáticas
Especifica a quantidade de horas que o Windows usará para determinar por quanto tempo deve esperar para verificar se há atualizações disponíveis. O tempo de espera exato é determinado pelas horas especificadas aqui menos zero a 20% das horas especificadas. Por exemplo, se essa política for usada para especificar uma frequência de detecção de 20 horas, todos os clientes aos quais essa política for aplicada verificarão se há atualizações em qualquer lugar entre 16 e 20 horas.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> A configuração "especificar local do serviço de atualização da intranet da Microsoft" deve estar habilitada para que essa política tenha efeito.
>
> se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

> [!NOTE]
> Não há suporte para essa política no Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que o Windows verificará se há atualizações disponíveis no intervalo padrão de 22 horas.|
|**Habilitada**|Especifica que o Windows verificará se há atualizações disponíveis no intervalo especificado.|
|**Desabilitada**|Especifica que o Windows verificará se há atualizações disponíveis no intervalo padrão de 22 horas.|

**Opções:** se essa configuração estiver habilitada, você poderá especificar o intervalo de tempo (em horas) que o Windows Update aguarda antes de verificar se há atualizações.

#### <a name="configure-automatic-updates"></a>Configurar Atualizações Automáticas
Especifica se as atualizações automáticas estão habilitadas neste computador.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Se habilitada, você deve selecionar uma das quatro opções fornecidas nesta configuração de Política de Grupo.

Para usar essa configuração, selecione **habilitado**e, em **Opções** , em **Configurar atualização automática**, selecione uma das opções (2, 3, 4 ou 5).

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que o uso de atualizações automáticas não está especificado no nível de Política de Grupo. No entanto, um administrador de computador ainda pode configurar as atualizações automáticas no painel de controle.|
|**Habilitada**|Especifica que o Windows reconhece quando o computador está online e usa sua conexão com a Internet para pesquisar Windows Update de atualizações disponíveis.<br /><br />Quando habilitado, os administradores locais terão permissão para usar o painel de controle Windows Update para selecionar uma opção de configuração de sua escolha. No entanto, os administradores locais não terão permissão para desabilitar a configuração para Atualizações Automáticas.<br /><br />-   **2-notificar para baixar e notificar para instalar**<br />    Quando Windows Update localizar atualizações que se aplicam ao computador, os usuários serão notificados de que as atualizações estão prontas para download. Os usuários podem executar Windows Update para baixar e instalar as atualizações disponíveis.<br />-   **3-baixar automaticamente e notificar para instalar** (configuração padrão)<br />    Windows Update localiza as atualizações aplicáveis e as baixa em segundo plano; o usuário não é notificado ou interrompido durante o processo. Quando os downloads forem concluídos, os usuários serão notificados de que há atualizações prontas para serem instaladas. Os usuários podem executar Windows Update para instalar as atualizações baixadas.<br />-   **4-baixar automaticamente e agendar a instalação**<br />    Você pode especificar o agendamento usando as opções nessa configuração de Política de Grupo. Se nenhuma agenda for especificada, o agendamento padrão para todas as instalações será todos os dias às 3:00. Se alguma atualização exigir uma reinicialização para concluir a instalação, o Windows reiniciará o computador automaticamente. (se um usuário estiver conectado ao computador quando o Windows estiver pronto para ser reiniciado, o usuário será notificado e terá a opção de atrasar a reinicialização.) **Observação:** iniciando o Windows 8, você pode definir as atualizações a serem instaladas durante a manutenção automática em vez de usar uma agenda específica vinculada a Windows Update. A manutenção automática instalará atualizações quando o computador não estiver em uso e evitará a instalação de atualizações quando o computador estiver sendo executado com baterias. Se a manutenção automática não puder instalar atualizações em dias, Windows Update instalará atualizações imediatamente. Os usuários serão notificados sobre uma reinicialização pendente. Uma reinicialização pendente só ocorrerá se não houver nenhum potencial para perda acidental de dados.    Você pode especificar opções de agendamento nas configurações do Agendador de manutenção do GPME, que estão localizadas no caminho, *policyname* > **configuração do computador** > **políticas** > **modelos administrativos** > **componentes do Windows** > o **Agendador de manutenção** > limite de ativação de **manutenção automática**. Consulte a seção desta referência intitulada: [configurações do Agendador de manutenção](#computer-configuration--maintenance-scheduler-policy-settings), para obter detalhes de configuração.    **5-permitir que o administrador local escolha a configuração**<br />-Especifica se os administradores locais têm permissão para usar o painel de controle de Atualizações Automáticas para selecionar uma opção de configuração de sua escolha, por exemplo, se os administradores locais podem escolher um horário de instalação agendado.<br />    Os administradores locais não poderão desabilitar a configuração para as Atualizações Automáticas.|
|**Desabilitada**|Especifica que todas as atualizações do cliente que estão disponíveis no serviço de Windows Update público devem ser baixadas manualmente da Internet e instaladas.|

#### <a name="delay-restart-for-scheduled-installations"></a>Atrasar reinicialização para instalações agendadas
Especifica a quantidade de tempo Atualizações Automáticas aguardará antes de prosseguir com uma reinicialização agendada.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa política se aplica somente quando Atualizações Automáticas está configurado para executar instalações agendadas de atualizações. se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que, após a instalação das atualizações, o tempo de espera padrão de quinze minutos decorrerá antes que ocorra qualquer reinicialização agendada.|
|**Habilitada**|Especifica que quando a instalação for concluída, uma reinicialização agendada ocorrerá depois que o número especificado de minutos tiver expirado.|
|**Desabilitada**|Especifica que, após a instalação das atualizações, o tempo de espera padrão de quinze minutos decorrerá antes que ocorra qualquer reinicialização agendada.|

**Opções:** se essa configuração estiver habilitada, você poderá usar essa opção para especificar a quantidade de tempo (em minutos) atualizações automáticas aguardar antes de prosseguir com uma reinicialização agendada.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>Não ajustar a opção padrão para instalar atualizações e desligar na caixa de diálogo desligar o Windows
Essa configuração de política permite que você especifique se a opção **instalar atualizações e desligar** é permitida como opção padrão na caixa de diálogo **desligar o Windows** .

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração de política não tem impacto se a *política* > **configuração do computador** > **políticas** > modelos administrativos **componentes** do Windows ** >  > Windows Update** não **exibe a opção "instalar atualizações e desligar" em desligar a configuração da política de diálogo do Windows** está habilitada. > 

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que **instalar atualizações e desligar** será a opção padrão na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|
|**Habilitada**|Se você habilitar essa configuração de política, a última opção de desligamento do usuário (por exemplo, hibernar ou reiniciar) será a opção padrão na caixa de diálogo **desligar o Windows** , independentemente de a opção **instalar atualizações e desligar** estar disponível no menu **o que você deseja que o computador faça?** .|
|**Desabilitada**|Especifica que **instalar atualizações e desligar** será a opção padrão na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>Não conectar a localizações do Windows Update na Internet
Mesmo quando Windows Update estiver configurado para receber atualizações de um serviço de atualização da intranet, ele recuperará periodicamente as informações do serviço de Windows Update público para habilitar conexões futuras com Windows Update e outros serviços, como Microsoft Update ou Microsoft Store.

Habilitar essa política desabilitará a funcionalidade para recuperar periodicamente as informações do serviço de atualização do Windows Server público e poderá fazer com que a conexão com os serviços públicos, como Microsoft Store, pare de funcionar.

|Com suporte em:|Excluir|
|---------|-------|
|a partir do Windows Server 2012 R2, Windows 8.1 ou Windows RT 8,1, sistemas operacionais Windows que ainda estão em seus [produtos Microsoft ciclo de vida de suporte](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa política se aplica somente quando o computador está configurado para se conectar a um serviço de atualização de intranet usando a configuração de política "especificar o local do serviço de atualização da Microsoft na intranet".

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|O comportamento padrão para recuperar informações do serviço de atualização do Windows Server público persiste.|
|**Habilitada**|Especifica que os computadores não recuperarão informações do serviço de atualização do Windows Server público.|
|**Desabilitada**|O comportamento padrão para recuperar informações do serviço de atualização do Windows Server público persiste.|

**Opções:** Não há opções para essa configuração.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>Não exibir a opção instalar atualizações e desligar na caixa de diálogo desligar o Windows
Especifica se a opção **instalar atualizações e desligar** é exibida na caixa de diálogo **desligar o Windows** .

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que a opção **instalar atualizações e desligar** está disponível na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis quando o usuário selecionar a opção desligar para desligar o computador. Um administrador local pode alterar essa configuração usando a política local.|
|**Habilitada**|Especifica que a **instalação de atualizações e desligamentos** não aparecerá como uma opção na caixa de diálogo **desligar o Windows** , mesmo que as atualizações estejam disponíveis para instalação quando o usuário selecionar a opção desligar para desligar o computador.|
|**Desabilitada**|Especifica que a opção **instalar atualizações e desligar** será a opção padrão na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="enable-client-side-targeting"></a>Habilitar o direcionamento do lado cliente
Especifica o nome do grupo de destino ou os nomes que são configurados no console do WSUS que devem receber atualizações do WSUS.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Essa política se aplica somente quando este computador está configurado para dar suporte aos nomes do grupo de destino especificado no WSUS. Se o nome do grupo de destino não existir no WSUS, ele será ignorado até ser criado. Se a configuração de política "especificar local do serviço de atualização da intranet da Microsoft" estiver desabilitada ou não estiver configurada, essa política não terá efeito.

> [!NOTE]
> Não há suporte para essa política no Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que nenhuma informação de grupo de destino é enviada ao WSUS. Um administrador local pode alterar essa configuração usando uma política local.|
|**Habilitada**|Especifica que as informações do grupo de destino especificadas são enviadas ao WSUS, que o utiliza para determinar quais atualizações devem ser implantadas neste computador. Se o WSUS oferecer suporte a vários grupos de destino, você poderá usar essa política para especificar vários nomes de grupos, separados por ponto-e-vírgula, se tiver adicionado os nomes de grupos de destino na lista de grupos de computadores no WSUS. Caso contrário, um único grupo deverá ser especificado.|
|**Desabilitada**|Especifica que nenhuma informação de grupo de destino é enviada ao WSUS.|

**Opções:** Use este espaço para especificar um ou mais nomes de grupos de destino.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>Habilitando o gerenciamento de energia Windows Update para ativar o computador automaticamente para instalar atualizações agendadas
Especifica se Windows Update usará os recursos de gerenciamento de energia do Windows ou opções de energia para ativar automaticamente o computador da hibernação, se houver atualizações agendadas para instalação.

O computador será ativado automaticamente somente se Windows Update estiver configurado para instalar atualizações automaticamente. Se o computador estiver em hibernação quando ocorrer o horário de instalação agendado e houver atualizações a serem aplicadas, Windows Update usará os recursos de gerenciamento de energia do Windows ou opções de energia para ativar automaticamente o computador para instalar as atualizações. Windows Update também ativará o computador e instalará uma atualização se ocorrer um prazo de instalação.

O computador não será ativado a menos que haja atualizações a serem instaladas. Se o computador estiver com energia da bateria, quando Windows Update ativá-lo, ele não instalará atualizações e o computador retornará automaticamente à hibernação em dois minutos.

|Com suporte em:|Excluir|
|---------|-------|
|a partir do Windows Vista e do Windows Server 2008 (Windows 7), os sistemas operacionais Windows que ainda estão no [ciclo de vida de suporte dos produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Windows Update não ativa o computador da hibernação para instalar atualizações. Um administrador local pode alterar essa configuração usando uma política local.|
|**Habilitada**|Windows Update ativa o computador da hibernação para instalar atualizações nas condições listadas anteriormente.|
|**Desabilitada**|Windows Update não ativa o computador da hibernação para instalar atualizações.|

**Opções:** Não há opções para essa configuração.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>Não há reinicializações automáticas para usuários conectados, referentes às instalações de atualizações automáticas agendadas
Especifica que para concluir uma instalação agendada, Atualizações Automáticas aguardará o computador ser reiniciado por qualquer usuário que esteja conectado, em vez de fazer com que o computador seja reiniciado automaticamente.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa política se aplica somente quando Atualizações Automáticas está configurado para executar instalações agendadas de atualizações. se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que Atualizações Automáticas notificará o usuário de que o computador será reiniciado automaticamente em cinco minutos para concluir a instalação.|
|**Habilitada**|Algumas atualizações exigem que o computador seja reiniciado para que as atualizações entrem em vigor. Se o status for definido como habilitado, Atualizações Automáticas não reiniciará um computador automaticamente durante uma instalação agendada se um usuário estiver conectado ao computador. Em vez disso, Atualizações Automáticas notificará o usuário para reiniciar o computador.|
|**Desabilitada**|Especifica que Atualizações Automáticas notificará o usuário de que o computador será reiniciado automaticamente em cinco minutos para concluir a instalação.|

**Opções:** Não há opções para essa configuração.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Solicitar reinicialização novamente com instalações agendadas
Especifica a quantidade de tempo para Atualizações Automáticas aguardar antes de solicitar novamente uma reinicialização agendada.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Essa política se aplica somente quando Atualizações Automáticas está configurado para executar instalações agendadas de atualizações. se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

> [!NOTE]
> Essa política não tem nenhum efeito em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Uma reinicialização agendada ocorre dez minutos depois que o prompt de reinicialização da mensagem é Descartado. Um administrador local pode alterar essa configuração usando uma política local.|
|**Habilitada**|Especifica que, depois que o prompt anterior para reinicialização for adiado, uma reinicialização agendada ocorrerá após o número especificado de minutos decorrido.|
|**Desabilitada**|Uma reinicialização agendada ocorre dez minutos depois que o prompt de reinicialização da mensagem é Descartado.|

**Opções:** Quando habilitado, você pode usar essa opção de configuração para especificar (em minutos) a duração do tempo decorrido antes que os usuários sejam solicitados novamente sobre uma reinicialização agendada.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Reagendar instalações agendadas de Atualizações Automáticas
Especifica a quantidade de tempo para Atualizações Automáticas aguardar após uma inicialização do computador, antes de prosseguir com uma instalação agendada que foi perdida anteriormente.

Se o status for definido como **não configurado**, uma instalação agendada perdida ocorrerá um minuto após a próxima inicialização do computador.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa política se aplica somente quando Atualizações Automáticas está configurado para executar instalações agendadas de atualizações. se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que uma instalação agendada perdida ocorrerá um minuto após a próxima inicialização do computador.|
|**Habilitada**|Especifica que uma instalação agendada que não tenha sido realizada anteriormente ocorrerá no número especificado de minutos após o computador ser iniciado pela próxima vez.|
|**Desabilitada**|Especifica que uma instalação agendada perdida ocorrerá com a próxima instalação agendada.|

**Opções:** Quando essa configuração de política está habilitada, você pode usá-la para especificar um número de minutos após a próxima inicialização do computador, que uma instalação agendada que não tenha ocorrido anteriormente ocorrerá.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Especificar o local do serviço de atualização na intranet da Microsoft
Especifica um servidor de intranet para hospedar atualizações do Microsoft Update. Em seguida, você pode usar o WSUS para atualizar automaticamente os computadores em sua rede.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Essa configuração permite que você especifique um servidor WSUS em sua rede que funcionará como um serviço de atualização interno. Em vez de usar as atualizações da Microsoft na Internet, os clientes do WSUS pesquisarão esse serviço em busca de atualizações que se apliquem.

Para usar essa configuração, você deve definir dois valores de nome de servidor: o servidor do qual o cliente detecta e baixa atualizações e o servidor no qual as estações de trabalho atualizadas carregam estatísticas. Os valores não precisam ser diferentes se ambos os serviços estiverem configurados no mesmo servidor.

> [!NOTE]
> se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

> [!NOTE]
> Não há suporte para essa política no Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|se Atualizações Automáticas não for desabilitada pela política ou preferência do usuário, essa política especificará que os clientes se conectam diretamente ao site do Windows Update na Internet.|
|**Habilitada**|Especifica que o cliente se conecta ao servidor WSUS especificado, em vez de Windows Update, para procurar e baixar atualizações. A habilitação dessa configuração significa que os usuários finais em sua organização não precisam passar por um firewall para obter atualizações e oferece a oportunidade de testar atualizações antes de implantá-las.|
|**Desabilitada**|se Atualizações Automáticas não for desabilitada pela política ou preferência do usuário, essa política especificará que os clientes se conectam diretamente ao site do Windows Update na Internet.|

**Opções:** Quando essa configuração de política está habilitada, você deve especificar o serviço de atualização da intranet que os clientes do WSUS usarão ao detectar atualizações e o servidor de estatísticas da Internet para o qual os clientes WSUS atualizados carregarão estatísticas. Valores de exemplo:


|                    Opção de configuração:                    |    Valor de exemplo:    |
|-------------------------------------------------------|----------------------|
| Definir o serviço de atualização da intranet para detectar atualizações |  http://wsus01:8530  |
|          Definir o servidor de estatísticas da intranet           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Ativar as atualizações recomendadas via Atualizações Automáticas
Especifica se Atualizações Automáticas fornecerá atualizações importantes e recomendadas do WSUS.

|Com suporte em:|Excluir|
|---------|-------|
|a partir do Windows Vista, os sistemas operacionais Windows que ainda estão no [ciclo de vida de suporte](https://support.microsoft.com/gp/lifeselect)de seus produtos da Microsoft.|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que Atualizações Automáticas continuará a fornecer atualizações importantes se já estiver configurada para fazer isso.|
|**Habilitada**|Especifica que Atualizações Automáticas instalará atualizações recomendadas e atualizações importantes do WSUS.|
|**Desabilitada**|Especifica que Atualizações Automáticas continuará a fornecer atualizações importantes se já estiver configurada para fazer isso.|

**Opções:** Não há opções para essa configuração.

#### <a name="turn-on-software-notifications"></a>Ativar notificações de software
Essa configuração de política permite que você controle se os usuários veem mensagens de notificação aprimoradas detalhadas sobre o software de destaque do serviço de Microsoft Update. As mensagens de notificação aprimoradas transmitem o valor e promovem a instalação e o uso de software opcional. Essa configuração de política destina-se a uso em ambientes de gerenciamento flexível nos quais você permite que o usuário final acesse o serviço de Microsoft Update.

Se você não estiver usando o serviço de Microsoft Update, a configuração de política "notificações de software" não terá nenhum efeito.

se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada ou não estiver configurada, a configuração de política "notificações de software" não terá nenhum efeito.

|Com suporte em:|Excluir|
|---------|-------|
|a partir do Windows Server 2008 (Windows Vista) e do Windows 7, os sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Por padrão, essa configuração de política está desabilitada.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Os usuários em computadores que executam o Windows 7 não recebem mensagens para aplicativos opcionais. Os usuários em computadores que executam o Windows Vista não recebem mensagens para aplicativos opcionais ou atualizações. Um administrador local pode alterar essa configuração usando o painel de controle ou uma política local.|
|**Habilitada**|Se você habilitar essa configuração de política, uma mensagem de notificação será exibida no computador do usuário quando o software em destaque estiver disponível. O usuário pode clicar na notificação para abrir Windows Update e obter mais informações sobre o software ou instalá-lo. O usuário também pode clicar em **fechar esta mensagem** ou **mostrá-lo mais tarde** para adiar a notificação conforme apropriado.<br /><br />No Windows 7, essa configuração de política só controlará as notificações detalhadas para aplicativos opcionais. No Windows Vista, essa configuração de política controla as notificações detalhadas para aplicativos opcionais e atualizações.|
|**Desabilitada**|Especifica que os usuários que executam o Windows 7 não receberão mensagens de notificação detalhadas para aplicativos opcionais, e os usuários que executam o Windows Vista não receberão mensagens de notificação detalhadas para aplicativos opcionais ou atualizações opcionais.|

**Opções:** Não há opções para essa configuração.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuração do computador > configurações de política do Agendador de manutenção
Na configuração definir Atualizações Automáticas, você selecionou a opção **4 – baixar automaticamente e agendar a instalação**. você pode especificar as configurações do Agendador de manutenção de agendamento no GPMC para computadores que executam o Windows 8 e o Windows RT. Se você não selecionou a opção 4 na configuração "definir Atualizações Automáticas", não será necessário definir essas configurações para fins de atualizações automáticas. As configurações do Agendador de manutenção estão localizadas em caminho: *política* > **configuração do computador** > **políticas** > **modelos administrativos** **componentes do Windows** > o **Agendador de manutenção**. >  A extensão do Agendador de manutenção do Política de Grupo contém as seguintes configurações:

-   [Limite de ativação de manutenção automática](#automatic-maintenance-activation-boundary)

-   [Atraso aleatório na manutenção automática](#automatic-maintenance-random-delay)

-   [Política de ativação automática](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Limite de Ativação de Manutenção Automática
Essa política permite que você configure a configuração "limite de ativação de manutenção automática".

O limite de ativação de manutenção é o horário agendado diário no qual a manutenção automática é iniciada.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração está relacionada à opção 4 em **configurar atualizações automáticas**. Se você não selecionou a opção 4 em **configurar atualizações automáticas**, não é necessário definir essa configuração.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Se essa configuração de política não estiver configurada, o horário agendado diariamente, conforme especificado em computadores cliente na **central de ações** > painel de controle de **manutenção automática** será aplicado.|
|**Habilitada**|A habilitação dessa configuração de política substitui qualquer configuração padrão ou modificada configurada em computadores cliente no **painel de controle** > a **central de ações** > **manutenção automática** (ou em algumas versões de cliente, **manutenção**).|
|**Desabilitada**|Se você definir essa configuração de política como **desabilitada**, o horário agendado diariamente, conforme especificado na **central de ações** > **manutenção automática**, no painel de controle será aplicado.|

#### <a name="automatic-maintenance-random-delay"></a>Atraso aleatório na manutenção automática
Essa configuração de política permite que você configure o atraso aleatório de ativação de manutenção automática.

O atraso aleatório de manutenção é a quantidade de tempo até a qual a manutenção automática atrasará a partir de seu limite de ativação. Essa configuração é útil para máquinas virtuais em que a manutenção aleatória pode ser um requisito de desempenho.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração está relacionada à opção 4 em **configurar atualizações automáticas**. Se você não selecionou a opção 4 em **configurar atualizações automáticas**, não é necessário definir essa configuração.

Por padrão, quando habilitado, o atraso aleatório de manutenção regular é definido como **PT4H**.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Um atraso aleatório de quatro horas é aplicado a **automático**.|
|**Habilitada**|A manutenção automática atrasará a partir de seu limite de ativação até o período de tempo especificado.|
|**Desabilitada**|Nenhum atraso aleatório é aplicado à manutenção automática.|

#### <a name="automatic-wakeup-policy"></a>Política de ativação automática
Essa configuração de política permite que você configure a política de ativação de manutenção automática.

A política de ativação de manutenção especifica se a manutenção automática deve fazer uma solicitação de ativação para o computador operacional para a manutenção agendada diária.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> se a política de ativação de energia do computador operacional estiver explicitamente desabilitada, essa configuração não terá efeito.

> [!NOTE]
> Essa configuração está relacionada à opção 4 em **configurar atualizações automáticas**. Se você não selecionou a opção 4 em **configurar atualizações automáticas**, não é necessário definir essa configuração.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Se você não definir essa configuração de política, a configuração de ativação, conforme especificado na **central de ações** > painel de controle de **manutenção automática** será aplicada.|
|**Habilitada**|Se você habilitar essa configuração de política, a manutenção automática tentará definir uma política de ativação do sistema operacional e fará uma solicitação de ativação para a hora agendada diária, se necessário.|
|**Desabilitada**|Se você desabilitar essa configuração de política, a configuração de ativação, conforme especificado na **central de ações** > painel de controle de **manutenção automática** será aplicada.|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuração do usuário > configurações de política de Windows Update
Esta seção fornece detalhes sobre as seguintes configurações de política baseadas no usuário:

-   [Não exibir a opção "instalar atualizações e desligar" na caixa de diálogo desligar o Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Não ajustar a opção padrão para "instalar atualizações e desligar" na caixa de diálogo desligar o Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [remover o acesso para usar todos os recursos de Windows Update](#remove-access-to-use-all-windows-update-features)

No GPMC, as configurações de usuário para atualizações automáticas de computador estão localizadas em caminho: *policyname* > **políticas** de > de **configuração de usuário** > **modelos administrativos** > **componentes do Windows** ** > Windows Update.** As configurações são listadas na mesma ordem em que aparecem na configuração do computador e nas extensões de configuração do usuário em Política de Grupo, quando a guia **configurações** da política de Windows Update é selecionada para classificar as configurações em ordem alfabética.

> [!NOTE]
> Por padrão, a menos que indicado de outra forma, essas configurações não são configuradas.

> [!TIP]
> para cada uma dessas configurações, você pode usar as seguintes etapas para habilitar, desabilitar ou navegar entre as configurações:

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>Não exibir a opção ' instalar atualizações e desligar ' na caixa de diálogo desligar o Windows
Especifica se a opção **instalar atualizações e desligar** é exibida na caixa de diálogo **desligar o Windows** .

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica que a opção **instalar atualizações e desligar** será exibida na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis quando o usuário selecionar a opção desligar para desligar o computador.|
|**Habilitada**|Se você habilitar essa configuração de política, **as atualizações de instalação e desligamento** não aparecerão como uma opção na caixa de diálogo **desligar o Windows** , mesmo que as atualizações estejam disponíveis para instalação quando o usuário selecionar a opção desligar para desligar o computador.|
|**Desabilitada**|Especifica que a opção **instalar atualizações e desligar** será exibida na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis quando o usuário selecionar a opção desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>Não ajustar a opção padrão para "instalar atualizações e desligar" na caixa de diálogo desligar o Windows
Especifica se a opção **instalar atualizações e desligar** é permitida como opção padrão na caixa de diálogo **desligar o Windows** .

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração de política não tem impacto se o *policyname > política* de ** > de** configuração de **usuário** > **modelos administrativos** **componentes do Windows** >  > Windows Update não exibir a **opção "instalar atualizações e desligar" na caixa de diálogo desligar o Windows** está habilitada. > 

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Especifica se a opção **instalar atualizações e desligar** será a opção padrão na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|
|**Habilitada**|Especifica se a última opção de desligamento do usuário (por exemplo, hibernação ou reinicialização) é a opção padrão na caixa de diálogo **desligar o Windows** , independentemente de a **opção instalar atualizações e desligar** estar disponível no menu **o que você deseja que o computador faça?** .|
|**Desabilitada**|Especifica se a opção **instalar atualizações e desligar** será a opção padrão na caixa de diálogo **desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Remover o acesso para usar todos os recursos do Windows Update
Essa configuração permite remover o acesso de cliente do WSUS ao Windows Update.

|Com suporte em:|Excluir|
|---------|-------|
|Sistemas operacionais Windows que ainda estão em seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não configurado**|Os usuários podem se conectar ao site Windows Update.|
|**Habilitada**|**Importante:** se habilitar, todos os recursos de Windows Update serão removidos. Isso inclui bloquear o acesso ao site Windows Update em https://windowsupdate.microsoft.com, no hiperlink Windows Update no menu iniciar ou na tela iniciar, e também no menu **ferramentas** do Internet Explorer. A atualização automática do Windows também está desabilitada; o usuário não será notificado nem receberá atualizações críticas do Windows Update. Essa configuração também impede que Device Manager instalem automaticamente atualizações de driver do site Windows Update.<br /><br />Quando habilitado, você pode configurar uma das seguintes opções de notificação:<br /><br />-   **0-não mostrar notificações**<br />    Essa configuração removerá todo o acesso a Windows Update recursos e nenhuma notificação será mostrada.<br />-   **1-Mostrar notificações necessárias de reinicialização**<br />    Essa configuração mostrará notificações sobre reinicializações necessárias para concluir uma instalação. **Observação:** Em computadores que executam o Windows 8 e o Windows RT, se essa política estiver habilitada, somente as notificações relacionadas a reinicializações e a incapacidade de detectar atualizações serão mostradas. Não há suporte para as opções de notificação. As notificações na tela de entrada são sempre exibidas.|
|**Desabilitada**|Os usuários podem se conectar ao site Windows Update.|

**Opções:** Consulte **habilitado** na tabela para essa configuração.

## <a name="supplemental-information"></a>Informações complementares
Esta seção fornece informações de adição sobre como usar abrir e salvar configurações do WSUS em políticas de grupo, bem como definições para termos usados neste guia. Para administradores familiarizados com versões anteriores do WSUS (WSUS 3,2 e versões anteriores), há uma tabela que resume rapidamente as diferenças entre as versões do WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Acessando as configurações de Windows Update no Política de Grupo
O procedimento a seguir descreve como abrir o GPMC em seu controlador de domínio. Em seguida, o procedimento descreve como abrir um GPO (objeto de Política de Grupo) de nível de domínio existente para edição ou criar um novo GPO de nível de domínio e abri-lo para edição.

> [!NOTE]
> Você deve ser um membro do grupo **Admins** . do domínio ou equivalente para executar esse procedimento.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir ou adicionar e abrir um objeto Política de Grupo

1.  No controlador de domínio, vá para **Gerenciador do servidor**, > **ferramentas**> **Gerenciamento de política de grupo**. O Console de Gerenciamento de Política de Grupo é aberto.

2.  No painel esquerdo, expanda sua floresta. Por exemplo, clique duas vezes em **floresta: example.com**.

3.  No painel esquerdo, clique duas vezes em **domínios**e, em seguida, clique duas vezes no domínio para o qual você deseja gerenciar um objeto de política de grupo. Por exemplo, clique duas vezes em **example.com**.

4.  Siga um destes procedimentos:

    -  **Para abrir um GPO de nível de domínio existente para edição**, clique duas vezes no domínio que contém o objeto de política de grupo que você deseja gerenciar, clique com o botão direito do mouse na política de domínio que você deseja gerenciar e clique em **Editar**. O GPME (editor de gerenciamento de Política de Grupo) é aberto.

    -  **Para criar um novo objeto de política de grupo e abrir para edição**:
        1.  Clique com o botão direito do mouse no domínio para o qual você deseja criar um novo objeto de Política de Grupo e, em seguida, clique em **criar um GPO nesse domínio e vincule-o aqui**.

        2.  Em **novo GPO**, em **nome**, digite um nome para o novo objeto de política de grupo e clique em **OK**.

        3.  Clique com o botão direito do mouse no novo objeto Política de Grupo e clique em **Editar**. GPME é aberto.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Para abrir as extensões de Windows Update ou Agendador de manutenção do Política de Grupo

1.  No Política de Grupo Editor de gerenciamento, siga um destes procedimentos:

    -   **Abra a Windows Update de configuração do computador > a extensão de política de grupo**. Navegue até: *policyname* > **configuração do computador** > **políticas** / **modelos administrativos** > **componentes do Windows** > **Windows Update**.

    -   **Abra a extensão de Windows Update de configuração de usuário > do política de grupo**. Navegue até: *policyname* > **políticas** de > de **configuração de usuário** > **modelos administrativos** > **componentes do Windows** > Windows Update.

    -   **Abra a configuração do computador > extensão do Agendador de manutenção do política de grupo**. Em GPOE, navegue até *policyname* > **configuração do computador** > **políticas** > **modelos administrativos** > **componentes do Windows** > o **Agendador de manutenção**.

Para obter mais informações sobre o Política de Grupo, consulte [política de grupo visão geral](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Depois de abrir a extensão do Política de Grupo desejado, você pode usar as seguintes etapas para habilitar, desabilitar ou navegar entre as configurações:

##### <a name="to-configure-group-policy-settings"></a>Para definir configurações da Política de grupo

1.  No *ExtensionOfGroupPolicy*, clique duas vezes na configuração que você deseja exibir ou modificar.

2.  Para definir a configuração, siga um destes procedimentos:

    -   Para manter o estado padrão não especificado da configuração, selecione **não configurado**.

    -   Para habilitar a configuração, selecione **habilitado**.

    -   Para desabilitar a configuração, selecione **desabilitado**.

3.  Em **Opções**, se qualquer opção estiver listada, mantenha os valores padrão ou modifique-os conforme necessário.

4.  Siga um destes procedimentos:

    -   Para salvar as alterações e prosseguir para a próxima configuração, clique em **aplicar**e, em seguida, clique em **próxima configuração**.

    -   Para salvar as alterações e fechar a caixa de diálogo, clique em **OK**.

    -   Para descartar todas as alterações não salvas e fechar a caixa de diálogo, clique em **Cancelar**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Alterações no WSUS relevantes para este guia
A tabela a seguir resume as principais diferenças entre as versões atuais e anteriores do WSUS que são relevantes para este guia.

|Versões do Windows Server e do WSUS|Descrição|
|------------------|--------|
| Windows Server 2012 R2 com WSUS 6,0 e versões subsequentes|a partir do Windows Server 2012, a função de servidor do WSUS é integrada ao sistema operacional e as configurações de Política de Grupo associadas para clientes do WSUS são, por padrão, incluídas no Política de Grupo.|
| Windows Server 2008 (e versões anteriores do Windows Server) com o WSUS 3,2 e anterior|No Windows Server 2008 (e versões anteriores do Windows Server) usando as versões 3,2 do WSUS (e anteriores), as configurações de Política de Grupo que regem os clientes WSUS não estão incluídas nesses sistemas operacionais Windows Server. As configurações de política estão no modelo administrativo do WSUS, **wuau. adm**. Nessas versões de servidor, o modelo administrativo do WSUS deve primeiro ser adicionado ao Console de Gerenciamento de Política de Grupo (GPMC) antes que as configurações do cliente do WSUS possam ser configuradas.|

### <a name="terms-and-definitions"></a>Termos e definições
Veja a seguir uma lista de termos usados neste guia.

|Termo|Definição|
|----|-------|
|Atualizações Automáticas|**Um serviço que é executado em computadores Windows** (atualizações automáticas): refere-se ao componente de computador cliente interno dos sistemas operacionais Microsoft Windows Vista, windows Server 2003, Windows XP e Windows 2000 com SP3 para obter atualizações de Microsoft Update ou Windows Update.<br /><br />**Referência casual** (atualizações automáticas): o termo usado para descrever quando o agente de Windows Update agenda e baixa automaticamente as atualizações.|
|servidor autônomo|Use para fazer referência a um servidor de Windows Server Update Services downstream (WSUS) no qual os administradores podem gerenciar os componentes do WSUS.|
|servidor downstream|Use para fazer referência a um servidor de Windows Server Update Services (WSUS) que obtém atualizações de outro servidor do WSUS em vez de Microsoft Update ou Windows Update.|
|Extensão de Política de Grupo (e: extensão de Política de Grupo|Uma coleção de configurações em Política de Grupo que são usadas para controlar como os usuários e computadores (aos quais as políticas se aplicam) podem configurar e usar vários serviços e recursos do Windows. Os administradores podem usar o WSUS com Política de Grupo para a configuração do lado do cliente do cliente Atualizações Automáticas, para ajudar a garantir que os usuários finais não possam desabilitar ou burlar políticas de atualização corporativa.<br /><br />O WSUS não requer o uso do Active Directory ou do Política de Grupo. A configuração do cliente também pode ser aplicada usando a política de grupo local ou modificando o registro do Windows.|
|serviço de atualização interno|Uma referência casual a uma infraestrutura de rede que usa um ou mais servidores WSUS para distribuir atualizações.|
|servidor de réplica|Use para fazer referência a um servidor de Windows Server Update Services downstream (WSUS) que espelha as aprovações e configurações no servidor upstream ao qual ele está conectado. Você não pode gerenciar o WSUS em um servidor de réplica.|
|Microsoft Update|**Um site de download da Microsoft baseado na Internet:** Um site da Internet da Microsoft que armazena e distribui atualizações para computadores Windows (drivers de dispositivo), sistemas operacionais Windows e outros produtos de software da Microsoft.|
|SUS (Software Update Services)|O SUS foi o produto predecessor da Windows Server Update Services (WSUS).|
|atualizações|Qualquer uma coleção de revisões de software, hotfixes, Service Packs, pacotes de recursos e drivers de dispositivo que podem ser instalados em um computador para estender a funcionalidade ou melhorar o desempenho e a segurança.|
|Arquivos de atualização|Os arquivos necessários para instalar uma atualização em um computador.|
|atualizar informações (também conhecidas como metadados de atualização)|As informações sobre uma atualização, em oposição aos arquivos binários de atualização em um pacote de atualização. Por exemplo, os metadados fornecem informações para as propriedades de uma atualização, permitindo que você descubra o que a atualização é útil. Os metadados também incluem termos de licença para software Microsoft. O pacote de metadados baixado para uma atualização normalmente é muito menor do que o pacote de arquivo de atualização real.|
|atualizar origem|O local para o qual um servidor de Windows Server Update Services (WSUS) sincroniza para obter arquivos de atualização. Esse local pode ser Microsoft Update ou um servidor WSUS upstream.|
|servidor upstream|Um servidor Windows Server Update Services (WSUS) que fornece arquivos de atualização para outro servidor WSUS, que, por sua vez, é chamado de servidor downstream.|
|Windows Server Update Services (WSUS)|Um programa de função de servidor executado em um ou mais computadores Windows Server em uma rede corporativa. Uma infraestrutura do WSUS permite que você gerencie atualizações de computadores na rede para instalar o.<br /><br />Você pode usar o WSUS para aprovar ou recusar atualizações antes do lançamento, para forçar as atualizações a serem instaladas por uma determinada data e obter relatórios extensos sobre quais atualizações cada computador em sua rede exige. Você pode configurar o WSUS para aprovar determinadas classes de atualizações automaticamente (atualizações críticas, atualizações de segurança, Service Packs, Drivers, etc.). O WSUS também permite que você aprove atualizações apenas para "detecção", para que você possa ver quais computadores precisarão de uma determinada atualização sem precisar instalar as atualizações.<br /><br />Em uma implementação do WSUS, pelo menos um servidor WSUS na rede deve ser capaz de se conectar ao Microsoft Update para obter as atualizações disponíveis. Com base na segurança e na configuração de rede, o administrador pode determinar quantos outros servidores se conectam diretamente a Microsoft Update.<br /><br />Você pode configurar um servidor do WSUS para obter atualizações pela Internet de tais locais como:<br /><br />-o Microsoft Update público<br />-o Windows Update público<br />-Microsoft Store|
|Windows Update|**Um site de download da Microsoft baseado na Internet:** Um site da Internet da Microsoft que armazena e distribui atualizações para computadores Windows (drivers de dispositivo) e sistemas operacionais Windows.<br /><br />**serviço de computador:** O nome do serviço de Windows Update que é executado em computadores. Windows Update detecta, baixa e instala atualizações em computadores Windows.<br /><br />Dependendo das configurações do computador e da política, o agente de Windows Update pode baixar atualizações de:<br /><br />-Microsoft Update<br />-Windows Update<br />-Microsoft Store<br />-Um serviço de atualização de Internet (rede) (WSUS)<br /><br />os computadores que não são gerenciados em um ambiente baseado no WSUS normalmente usarão Windows Update para se conectar diretamente pela Internet para Windows Update, Microsoft Update ou Microsoft Store para obter atualizações.|
|Cliente do WSUS|Um computador que recebe atualizações de um serviço de atualização da intranet do WSUS.<br /><br />No caso de Política de Grupo configurações que controlam a interação do usuário final com Atualizações Automáticas: um usuário de um computador em um ambiente do WSUS.|
