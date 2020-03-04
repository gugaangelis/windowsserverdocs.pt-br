---
title: Etapa 4 – Definir as configurações da política de grupo para atualizações automáticas
description: Tópico sobre o WSUS (Windows Server Update Service) – definir configurações de Política de Grupo para Atualizações Automáticas é a etapa quatro em um processo de quatro etapas para implantar o WSUS
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
ms.openlocfilehash: f8ebe1f82cd6f616d42521729c5efc14821c20fa
ms.sourcegitcommit: 9687d3eb221b89061a48bf1e73fb3b25bee69f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78169576"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Etapa 4: Definir as configurações da Política de Grupo para atualizações automáticas

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em um ambiente do Active Directory, você pode usar Política de Grupo para definir como computadores e usuários (mencionados neste documento como clientes do WSUS) podem interagir com o Windows Update para obter atualizações automáticas do WSUS (Windows Server Update Services).

Este tópico contém duas seções principais:

[Configurações de Política de Grupo para atualizações do cliente do WSUS](#group-policy-settings-for-wsus-client-updates), que dão orientações prescritivas e detalhes comportamentais sobre as configurações do Windows Update e do Agendador de Manutenção da Política de Grupo que controlam como os clientes WSUS podem interagir com Windows Update para obterem atualizações automáticas.

As [Informações complementares](#supplemental-information) têm as seguintes seções:

-   [Como acessar as configurações do Windows Update na Política de Grupo](#accessing-the-windows-update-settings-in-group-policy), que fornece orientação geral sobre como usar o Editor de Gerenciamento de Política de Grupo e informações sobre como acessar as extensões de política dos Serviços de Atualização e as configurações do Agendador de Manutenção na Política de Grupo.

-   [Alterações ao WSUS relevantes para este guia](#changes-to-wsus-relevant-to-this-guide): para administradores familiarizados com o WSUS 3.2 e versões anteriores, esta seção apresenta um breve resumo das principais diferenças entre a versão atual e a anterior do WSUS relevantes para este guia.

-   [Termos e definições](#terms-and-definitions): definições para vários termos referentes ao WSUS e aos serviços de atualização que são usados neste guia.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Configurações de Política de Grupo para atualizações do cliente do WSUS
Esta seção fornece informações sobre três extensões da Política de Grupo. Nessas extensões, você encontrará as configurações que você pode usar para configurar como os clientes do WSUS podem interagir com o Windows Update para receber atualizações automáticas.

-   [Configuração do computador &gt; Configurações de política do Windows Update](#computer-configuration--windows-update-policy-settings)

-   [Configuração do computador &gt; Configurações de política do Agendador de Manutenção](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuração do usuário &gt; Configurações de política do Windows Update](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> Este tópico pressupõe que você já usa e está familiarizado com a Política de Grupo. Se você não estiver familiarizado com a Política de Grupo, é recomendável examinar as informações na seção [Informações complementares](#supplemental-information) deste documento antes de tentar definir as configurações de política para o WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuração do computador > Configurações de política do Windows Update
Esta seção fornece detalhes sobre as seguintes configurações de política baseadas em computador:

-   [Permitir instalação imediata de Atualizações Automáticas](#allow-automatic-updates-immediate-installation)

-   [Permitir que não administradores recebam notificações de atualização](#allow-non-administrators-to-receive-update-notifications)

-   [Permitir atualizações assinadas de um local do serviço Microsoft Update na intranet](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Frequência de detecção de Atualizações Automáticas](#automatic-updates-detection-frequency)

-   [Configurar atualizações automáticas](#configure-automatic-updates)

-   [Adiar a reinicialização para instalações agendadas](#delay-restart-for-scheduled-installations)

-   [Não ajustar a opção padrão para "Instalar Atualizações e Desligar" na caixa de diálogo Desligar o Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Não exibir a opção "Instalar atualizações e Desligar" na caixa de diálogo Desligar o Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Habilitar o direcionamento do lado cliente](#enable-client-side-targeting)

-   [Como habilitar o Gerenciamento de Energia do Windows Update para ativar o computador automaticamente para instalar atualizações agendadas](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [Não há reinicializações automáticas para usuários conectados, referentes às instalações de atualizações automáticas agendadas](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Solicitar reinicialização novamente com instalações agendadas](#re-prompt-for-restart-with-scheduled-installations)

-   [Reagendar instalações agendadas de Atualizações Automáticas](#reschedule-automatic-updates-scheduled-installations)

-   [Especificar o local do serviço Microsoft Update na intranet](#specify-intranet-microsoft-update-service-location)

-   [Ativar as atualizações recomendadas por meio de Atualizações Automáticas](#turn-on-recommended-updates-via-automatic-updates)

-   [Ativar notificações de software](#turn-on-software-notifications)

No GPME, as políticas do Windows Update para a configuração baseada em computador estão localizadas no caminho: *PolicyName* > **Configuração do Computador** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update**.

> [!NOTE]
> Por padrão, essas configurações não são configuradas.

#### <a name="allow-automatic-updates-immediate-installation"></a>Permitir instalação imediata de Atualizações Automáticas
Especifica se o recurso Atualizações Automáticas instalará automaticamente as atualizações que não interrompem os serviços do Windows nem reiniciam o Windows.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Se a configuração de política "Configurar Atualizações Automáticas" estiver **Desabilitada**, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que as atualizações não são instaladas imediatamente. Os administradores locais podem alterar essa configuração usando o editor de Política de Grupo Local.|
|**Habilitada**|Especifica que o recurso Atualizações Automáticas instala imediatamente as atualizações depois que elas são baixadas e estão prontas para serem instaladas.|
|**Desabilitada**|Especifica que as atualizações não são instaladas imediatamente.|

**Opções:** Não há opções para essa configuração.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Permitir que não administradores recebam notificações de atualização
Especifica se usuários não administrativos receberão notificações de atualização com base na configuração de política de Configurar Atualizações Automáticas.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|Consulte os detalhes na tabela abaixo.|

> [!NOTE]
> Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada ou não estiver configurada, essa configuração de política não terá efeito.

> [!IMPORTANT]
> Do Windows 8 e do Windows RT em diante, essa configuração de política é habilitada por padrão. Em todas as versões anteriores do Windows, está desabilitado por padrão.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que os usuários sempre verão uma janela de Controle de Conta e exigem permissões elevadas para realizar essas tarefas. Um administrador local pode alterar essa configuração usando o editor de Política de Grupo Local.|
|**Habilitada**|Especifica que a Atualização Automática do Windows e o Microsoft Update incluirão não administradores ao determinar qual usuário conectado receberá notificações de atualização. Usuários não administrativos poderão instalar todo o conteúdo de atualização opcional, recomendado e IMPORTANTE para o qual receberam uma notificação. Os usuários não verão uma janela de Controle de Conta de Usuário e não precisarão de permissões elevadas para instalar essas atualizações, exceto no caso de atualizações que contêm alterações de configuração de Interface do Usuário, Contrato de Licença de Usuário Final ou do Windows Update.<br /><br />Há duas situações em que o efeito dessa configuração depende do computador operacional:<br /><br />1.  **Ocultar** ou **Restaurar** atualizações<br />2.  **Cancelar** uma instalação de atualização<br /><br />No Windows Vista ou no Windows XP, se essa configuração de política estiver habilitada, os usuários não verão uma janela de controle de conta de usuário e não precisarão de permissões elevadas para ocultar, restaurar ou cancelar atualizações.<br /><br />No Windows Vista, se essa configuração de política estiver habilitada, os usuários não verão uma janela de controle de conta de usuário e não precisarão de permissões elevadas para ocultar, restaurar ou cancelar atualizações. Se essa configuração de política não estiver habilitada, os usuários sempre verão uma janela de controle de conta e precisarão de permissões elevadas para ocultar, restaurar ou cancelar atualizações.<br /><br />No Windows 7, essa configuração de política não tem nenhum efeito. Os usuários sempre verão uma janela de Controle de Conta e precisarão de permissões elevadas para realizar essas tarefas.<br /><br />No Windows 8 e no Windows RT, essa configuração de política não tem nenhum efeito.|
|**Desabilitada**|Especifica que somente administradores conectados recebem notificações de atualização. **Observação**: No Windows 8 e no Windows RT, essa configuração de política é habilitada por padrão. Em todas as versões anteriores do Windows, está desabilitado por padrão.|

**Opções:** Não há opções para essa configuração.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Permitir atualizações assinadas de um local do serviço Microsoft Update na intranet
Especifica se as Atualizações Automáticas aceitarão atualizações assinadas por entidades diferentes da Microsoft quando a atualização for encontrada em um local do serviço Microsoft Update na intranet.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> As atualizações de um serviço que não seja um serviço Microsoft Update na intranet sempre deverão ser assinadas pela Microsoft e não são afetadas por essa configuração de política.

> [!NOTE]
> Esta política não é compatível com o Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

**Opções:** Não há opções para essa configuração.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que as atualizações de um local de serviço Microsoft Update na intranet devem ser assinadas pela Microsoft.|
|**Habilitada**|Especifica que o recurso Atualizações Automáticas aceita atualizações recebidas por meio de um local de serviço Microsoft Update da intranet se forem assinadas por um certificado encontrado no repositório de certificados "Editores Confiáveis" do computador local.|
|**Desabilitada**|Especifica que as atualizações de um local de serviço Microsoft Update na intranet devem ser assinadas pela Microsoft.|

**Opções:** Não há opções para essa configuração.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Sempre reiniciar automaticamente no horário agendado
Especifica se um temporizador de reinicialização sempre será iniciado imediatamente depois que o Windows Update instalar atualizações importantes, em vez de notificar primeiro os usuários na tela de entrada por pelo menos dois dias.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Se a configuração de política "Não há reinicializações automáticas para usuários conectados, referentes às instalações de atualizações automáticas agendadas" estiver habilitada, esta política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que o Windows Update não alterará o comportamento de reinicialização do computador.|
|**Habilitada**|Especifica que um temporizador de reinicialização sempre será iniciado imediatamente depois que o Windows Update instalar atualizações importantes, em vez de notificar primeiro os usuários na tela de entrada por pelo menos dois dias.<br /><br />O temporizador de reinicialização pode ser configurado para iniciar com qualquer valor de 15 a 180 minutos. Quando o temporizador chegar ao fim, a reinicialização continuará mesmo que o computador tenha usuários conectados.|
|**Desabilitada**|Especifica que o Windows Update não alterará o comportamento de reinicialização do computador.|

**Opções:** se essa configuração estiver habilitada, você poderá especificar a quantidade de tempo que se passará depois que as atualizações forem instaladas antes que ocorra uma reinicialização forçada do computador.

#### <a name="automatic-updates-detection-frequency"></a>Frequência de detecção de Atualizações Automáticas
Especifica a quantidade de horas que o Windows usará para determinar por quanto tempo deve esperar para verificar se há atualizações disponíveis. O tempo de espera exato é determinado pelas horas especificadas aqui menos zero a 20% das horas especificadas. Por exemplo, se essa política for usada para especificar uma frequência de detecção de 20 horas, todos os clientes aos quais essa política é aplicada verificarão atualizações em qualquer lugar por 16 e 20 horas.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> A configuração "Especificar o local do serviço Microsoft Update na intranet" deve estar habilitada para que esta política tenha efeito.
>
> Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

> [!NOTE]
> Esta política não é compatível com o Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que o Windows verificará se há atualizações disponíveis ao intervalo padrão de 22 horas.|
|**Habilitada**|Especifica que o Windows verificará se há atualizações disponíveis ao intervalo especificado.|
|**Desabilitada**|Especifica que o Windows verificará se há atualizações disponíveis ao intervalo padrão de 22 horas.|

**Opções:** se essa configuração estiver habilitada, você poderá especificar o intervalo de tempo (em horas) que o Windows Update aguardará antes de verificar se há atualizações.

#### <a name="configure-automatic-updates"></a>Configurar Atualizações Automáticas
Especifica se as atualizações automáticas estão habilitadas neste computador.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Se estiverem habilitadas, você deverá selecionar uma das quatro opções fornecidas nesta configuração de Política de Grupo.

Para usar essa configuração, selecione **Habilitado** e, em **Opções**, em **Configurar atualização automática**, selecione uma das opções (2, 3, 4 ou 5).

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que o uso de atualizações automáticas não está especificado no nível de Política de Grupo. No entanto, um administrador de computador ainda pode configurar as atualizações automáticas no Painel de Controle.|
|**Habilitada**|Especifica que o Windows reconhece quando o computador está online e usa sua conexão com a Internet para pesquisar atualizações disponíveis no Windows Update.<br /><br />Quando habilitado, os administradores locais terão permissão para usar o painel de controle Windows Update para selecionar uma opção de configuração de sua escolha. Porém, os administradores locais não poderão desabilitar a configuração para as Atualizações Automáticas.<br /><br />-   **2 – Notificar para download e notificar para instalação**<br />    Quando o Windows Update localizar atualizações que se aplicam ao computador, os usuários serão notificados de que as atualizações estão prontas para download. Os usuários podem executar o Windows Update para baixar e instalar as atualizações disponíveis.<br />-   **3 – Baixar automaticamente e notificar para instalação** (configuração padrão)<br />    O Windows Update localiza as atualizações aplicáveis e as baixa em segundo plano; o usuário não é notificado nem interrompido durante o processo. Quando os downloads forem concluídos, os usuários serão notificados de que há atualizações prontas para serem instaladas. Os usuários então podem executar o Windows Update para instalar as atualizações baixadas.<br />-   **4 – Baixar automaticamente e agendar a instalação**<br />    Você pode especificar o agendamento usando as opções nesta configuração de Política de Grupo. Se nenhum agendamento for especificado, o agendamento padrão para todas as instalações será todos os dias às 3h. Se alguma atualização exigir uma reinicialização para concluir a instalação, o Windows reiniciará o computador automaticamente. (Se um usuário estiver conectado ao computador quando o Windows estiver pronto para ser reiniciado, o usuário será notificado e terá a opção de adiar a reinicialização.) **Observação:** ao iniciar o Windows 8, você pode definir as atualizações a serem instaladas durante a manutenção automática, em vez de usar um agendamento específico vinculada ao Windows Update. A manutenção automática instalará atualizações quando o computador não estiver em uso e evitará a instalação de atualizações quando o computador estiver sendo executado com baterias. Se a manutenção automática não puder instalar atualizações dentro de dias, o Windows Update instalará atualizações imediatamente. Os usuários serão notificados sobre uma reinicialização pendente. Uma reinicialização pendente só ocorrerá se não houver nenhum potencial para perda acidental de dados.    Você pode especificar as opções de agendamento nas configurações do Agendador de Manutenção do GPME, que estão localizadas no caminho *PolicyName* > **Configuração do Computador** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Agendador de Manutenção** > **Limite de Ativação de Manutenção Automática**. Confira a seção desta referência intitulada: [Configurações do Agendador de Manutenção](#computer-configuration--maintenance-scheduler-policy-settings) para obter detalhes de configuração.    **5 – Permitir que o administrador local escolha a configuração**<br />- Especifica se os administradores locais têm permissão para usar o painel de controle de Atualizações Automáticas para selecionar uma opção de configuração de escolha deles, por exemplo, se os administradores locais podem escolher um horário de instalação agendado.<br />    Os administradores locais não poderão desabilitar a configuração para as Atualizações Automáticas.|
|**Desabilitada**|Especifica que todas as atualizações do cliente disponíveis no serviço do Windows Update público devem ser baixadas manualmente da Internet e instaladas.|

#### <a name="delay-restart-for-scheduled-installations"></a>Adiar a reinicialização para instalações agendadas
Especifica a quantidade de tempo que as Atualizações Automáticas esperarão antes de prosseguirem com uma reinicialização agendada.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta política se aplica somente quando a opção Atualizações Automáticas está configurada para executar instalações agendadas de atualizações. Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que, após a instalação das atualizações, o tempo de espera padrão de quinze minutos decorrerá antes que haja qualquer reinicialização agendada.|
|**Habilitada**|Especifica que, quando a instalação for concluída, uma reinicialização agendada ocorrerá depois que o número especificado de minutos tiver expirado.|
|**Desabilitada**|Especifica que, após a instalação das atualizações, o tempo de espera padrão de quinze minutos decorrerá antes que haja qualquer reinicialização agendada.|

**Opções:** se essa configuração estiver habilitada, você poderá usar essa opção para especificar a quantidade de tempo (em minutos) que as Atualizações Automáticas esperarão antes de prosseguirem com uma reinicialização agendada.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>Não ajustar a opção padrão para Instalar Atualizações e Desligar na caixa de diálogo Desligar o Windows
Essa configuração de política permite que você especifique se a opção **Instalar Atualizações e Desligar** é permitida como a opção padrão na caixa de diálogo **Desligar o Windows**.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração de política não será afetará se a configuração de política *PolicyName* > **Configuração do Computador** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update** > **Não exibir a opção "Instalar Atualizações e Desligar" na caixa de diálogo Desligar o Windows** estiver habilitada.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que **Instalar Atualizações e Desligar** será a opção padrão na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|
|**Habilitada**|Se você habilitar essa configuração de política, a última opção de desligar do usuário (por exemplo, Hibernar ou Reiniciar) será a opção padrão na caixa de diálogo **Desligar o Windows**, independentemente de a opção **Instalar Atualizações e Desligar** estar disponível no menu **O que você deseja que o computador faça?** .|
|**Desabilitada**|Especifica que **Instalar Atualizações e Desligar** será a opção padrão na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>Não conectar a localizações do Windows Update na Internet
Mesmo quando o Windows Update estiver configurado para receber atualizações de um serviço de atualização da intranet, ele recuperará informações periodicamente do serviço Windows Update público para habilitar conexões futuras ao Windows Update e outros serviços, como o Microsoft Update ou a Microsoft Store.

Habilitar essa política desabilitará a funcionalidade para recuperar periodicamente as informações do serviço de atualização do Windows Server público e poderá fazer com que a conexão com os serviços públicos, como o Microsoft Store, pare de funcionar.

|Compatível com:|Excluindo:|
|---------|-------|
|Começando com o Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1, os sistemas operacionais Windows que ainda estiverem no [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa política só se aplica quando o computador está configurado para se conectar a um serviço de atualização da intranet usando a configuração de política "Especificar o local do serviço Microsoft Update na intranet".

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|O comportamento padrão para recuperar informações do Serviço de Atualização do Windows Server público persiste.|
|**Habilitada**|Especifica que os computadores não recuperarão informações do Windows Server Update Service público.|
|**Desabilitada**|O comportamento padrão para recuperar informações do Serviço de Atualização do Windows Server público persiste.|

**Opções:** Não há opções para essa configuração.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>Não exibir a opção Instalar atualizações e Desligar na caixa de diálogo Desligar o Windows
Especifica se a opção **Instalar Atualizações e Desligar** é exibida na caixa de diálogo **Desligar o Windows**.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que a opção **Instalar Atualizações e Desligar** estará disponível na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis quando o usuário selecionar a opção Desligar para desligar o computador. Um administrador local pode alterar essa configuração usando uma política local.|
|**Habilitada**|Especifica que **Instalar Atualizações e Desligar** não aparecerá como uma opção na caixa de diálogo **Desligar o Windows**, mesmo se houver atualizações disponíveis para instalação quando o usuário selecionar a opção desligar para desligar o computador.|
|**Desabilitada**|Especifica que **Instalar Atualizações e Desligar** será a opção padrão na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="enable-client-side-targeting"></a>Habilitar o direcionamento do lado cliente
Especifica os nomes do grupo de destino configurados no console do WSUS que devem receber atualizações do WSUS.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Essa política se aplica somente quando este computador está configurado para dar suporte aos nomes do grupo de destino especificado no WSUS. Se o nome do grupo de destino não existir no WSUS, ele será ignorado até ser criado. Se a configuração de política Especificar o local do serviço Microsoft Update na intranet estiver desabilitada ou não configurada, essa política não terá efeito.

> [!NOTE]
> Esta política não é compatível com o Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que nenhuma informação de grupo de destino é enviada ao WSUS. Um administrador local pode alterar essa configuração usando uma política local.|
|**Habilitada**|Especifica que as informações do grupo de destino determinadas são enviadas ao WSUS, que as utiliza para determinar quais atualizações devem ser implantadas neste computador. Se o WSUS oferecer suporte a vários grupos de destino, você poderá usar essa política para especificar vários nomes de grupos, separados por ponto-e-vírgula, se tiver adicionado os nomes de grupos de destino à lista de grupos de computadores no WSUS. Caso contrário, um único grupo deverá ser especificado.|
|**Desabilitada**|Especifica que nenhuma informação de grupo de destino é enviada ao WSUS.|

**Opções:** Use este espaço para especificar um ou mais nomes de grupos de destino.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>Como habilitar o Gerenciamento de Energia do Windows Update para ativar o computador automaticamente para instalar atualizações agendadas
Especifica se o Windows Update usará os recursos de gerenciamento de energia do Windows ou opções de energia para ativar automaticamente o computador da hibernação se houver atualizações agendadas para instalação.

O computador será ativado automaticamente somente se o Windows Update estiver configurado para instalar atualizações automaticamente. Se o computador estiver em hibernação quando chegar o horário de instalação agendado e houver atualizações a serem aplicadas, o Windows Update usará os recursos de gerenciamento de energia do Windows ou as opções de energia para ativar automaticamente o computador para instalar as atualizações. O Windows Update também ativará o computador e instalará uma atualização se chegar o prazo de instalação.

O computador não será ativado, a menos que haja atualizações a serem instaladas. Se o computador estiver sendo alimentado por bateria, quando o Windows Update o ativar, ele não instalará atualizações e o computador retornará automaticamente para a hibernação em dois minutos.

|Compatível com:|Excluindo:|
|---------|-------|
|Começando com o Windows Vista e o Windows Server 2008 (Windows 7), os sistemas operacionais Windows que ainda estiverem no [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|O Windows Update não ativa o computador da hibernação para instalar atualizações. Um administrador local pode alterar essa configuração usando uma política local.|
|**Habilitada**|O Windows Update ativa o computador da hibernação para instalar atualizações nas condições listadas anteriormente.|
|**Desabilitada**|O Windows Update não ativa o computador da hibernação para instalar atualizações.|

**Opções:** Não há opções para essa configuração.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>Não há reinicializações automáticas para usuários conectados, referentes às instalações de atualizações automáticas agendadas
Especifica que, para concluir uma instalação agendada, as Atualizações Automáticas aguardarão o computador ser reiniciado por qualquer usuário que esteja conectado, em vez de fazer com que o computador seja reiniciado automaticamente.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta política se aplica somente quando a opção Atualizações Automáticas está configurada para executar instalações agendadas de atualizações. Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que as Atualizações Automáticas notificarão o usuário de que o computador será automaticamente reinicializado em cinco minutos para concluir a instalação.|
|**Habilitada**|Algumas atualizações exigem que o computador seja reiniciado para que as atualizações entrem em vigor. Se o status for definido como habilitado, as Atualizações Automáticas não reiniciarão um computador automaticamente durante uma instalação agendada se um usuário estiver conectado ao computador. Em vez disso, as Atualizações Automáticas notificarão o usuário para reiniciar o computador.|
|**Desabilitada**|Especifica que as Atualizações Automáticas notificarão o usuário de que o computador será automaticamente reinicializado em cinco minutos para concluir a instalação.|

**Opções:** Não há opções para essa configuração.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Solicitar reinicialização novamente com instalações agendadas
Especifica quanto tempo as Atualizações Automáticas aguardam antes de solicitarem novamente uma reinicialização agendada.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Esta política se aplica somente quando a opção Atualizações Automáticas está configurada para executar instalações agendadas de atualizações. Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

> [!NOTE]
> Essa política não é válida em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Uma reinicialização agendada ocorre dez minutos depois de ignorar a mensagem de aviso de reinicialização. Um administrador local pode alterar essa configuração usando uma política local.|
|**Habilitada**|Especifica que, depois que o prompt anterior para reinicialização ter sido adiado, uma reinicialização agendada ocorrerá após o número especificado de minutos ter se passado.|
|**Desabilitada**|Uma reinicialização agendada ocorre dez minutos depois de ignorar a mensagem de aviso de reinicialização.|

**Opções:** Quando habilitada, você pode usar essa opção de configuração para especificar a duração (em minutos) do tempo decorrido antes que os usuários sejam solicitados novamente sobre uma reinicialização agendada.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Reagendar instalações agendadas de Atualizações Automáticas
Especifica quanto tempo as Atualizações Automáticas aguardam após uma inicialização do computador antes de prosseguirem com uma instalação agendada ignorada anteriormente.

Se o status for definido como **Não Configurado**, uma instalação agendada ignorada ocorrerá um minuto após a próxima inicialização do computador.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta política se aplica somente quando a opção Atualizações Automáticas está configurada para executar instalações agendadas de atualizações. Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que uma instalação agendada ignorada ocorrerá um minuto após a próxima inicialização do computador.|
|**Habilitada**|Especifica que uma instalação agendada que não tenha sido realizada anteriormente ocorrerá o número especificado de minutos após a próxima inicialização do computador.|
|**Desabilitada**|Especifica que uma instalação agendada ignorada ocorrerá com a próxima instalação agendada.|

**Opções:** Quando essa configuração de política está habilitada, você pode usá-la para especificar um número de minutos após a próxima inicialização do computador em que ocorrerá uma instalação agendada que não tenha sido realizada anteriormente.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Especificar o local do serviço Microsoft Update na intranet
Especifica um servidor de intranet para hospedar atualizações do Microsoft Update. Você então pode usar o WSUS para atualizar automaticamente os computadores em sua rede.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Essa configuração permite que você especifique um servidor WSUS em sua rede que funcione como um serviço de atualização interno. Em vez de usar o Microsoft Updates na Internet, os clientes do WSUS pesquisarão esse serviço em busca de atualizações que se apliquem.

Para usar essa configuração, defina dois valores de nome do servidor: o servidor em que o cliente detecta e baixa atualizações, e o servidor no qual as estações de trabalho atualizadas carregam estatísticas. Os valores não precisarão ser diferentes se ambos os serviços estiverem configurados no mesmo servidor.

> [!NOTE]
> Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada, essa política não terá efeito.

> [!NOTE]
> Esta política não é compatível com o Windows RT. Habilitar essa política não terá nenhum efeito em computadores que executam o Windows RT.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|se a opção Atualizações Automáticas não estiver desabilitada pela política ou preferência do usuário, essa política especificará que os clientes se conectam diretamente ao site do Windows Update na Internet.|
|**Habilitada**|Especifica que o cliente se conecta ao servidor WSUS especificado, em vez de ao Windows Update, para pesquisar e baixar atualizações. A habilitação dessa configuração significa que os usuários finais em sua organização não precisam passar por um firewall para obter atualizações e dá a você a oportunidade de testar as atualizações antes de implantá-las.|
|**Desabilitada**|se a opção Atualizações Automáticas não estiver desabilitada pela política ou preferência do usuário, essa política especificará que os clientes se conectam diretamente ao site do Windows Update na Internet.|

**Opções:** Quando essa configuração de política está habilitada, você deve especificar o serviço de atualização da intranet que os clientes do WSUS usarão na detecção atualizações e o servidor de estatísticas da Internet para o qual os clientes WSUS atualizados carregarão as estatísticas. Valores de exemplo:


|                    Opção de configuração:                    |    Valor de exemplo:    |
|-------------------------------------------------------|----------------------|
| Definir o serviço de atualização da intranet para detectar atualizações |  http://wsus01:8530  |
|          Definir o servidor de estatísticas da intranet           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Ativar as atualizações recomendadas por meio de Atualizações Automáticas
Especifica se Atualizações Automáticas fornecerão atualizações IMPORTANTES e recomendadas do WSUS.

|Compatível com:|Excluindo:|
|---------|-------|
|Começando com o Windows Vista, os sistemas operacionais Windows que ainda estão no [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que Atualizações Automáticas continuarão a fornecer atualizações importantes se já estiverem configurada para fazer isso.|
|**Habilitada**|Especifica que o recurso Atualizações Automáticas instalará atualizações recomendadas e atualizações importantes do WSUS.|
|**Desabilitada**|Especifica que Atualizações Automáticas continuarão a fornecer atualizações importantes se já estiverem configurada para fazer isso.|

**Opções:** Não há opções para essa configuração.

#### <a name="turn-on-software-notifications"></a>Ativar notificações de software
Essa configuração de política permite que você controle se os usuários veem mensagens de notificação aprimoradas detalhadas sobre o software em destaque do serviço do Microsoft Update. As mensagens de notificação aprimoradas transmitem o valor e promovem a instalação e o uso de software opcional. Essa configuração de política destina-se a uso em ambientes de gerenciamento flexível nos quais você permite que o usuário final acesse o serviço do Microsoft Update.

Se você não estiver usando o serviço do Microsoft Update, a configuração de política "Notificações de Software" não terá efeito.

Se a configuração de política "Configurar Atualizações Automáticas" estiver desabilitada ou não estiver configurada, a configuração de política "Notificações de Software" não terá efeito.

|Compatível com:|Excluindo:|
|---------|-------|
|Do Windows Server 2008 (Windows Vista) e Windows 7, os sistemas operacionais Windows que ainda estiverem no [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Por padrão, essa configuração de política está desabilitada.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Os usuários em computadores que executam o Windows 7 não recebem mensagens para aplicativos opcionais. Os usuários em computadores que executam o Windows Vista não recebem mensagens para aplicativos opcionais ou atualizações. Um administrador local pode alterar essa configuração usando um Painel de Controle ou uma política local.|
|**Habilitada**|Se você habilitar essa configuração de política, uma mensagem de notificação será exibida no computador do usuário quando o software em destaque estiver disponível. O usuário pode clicar na notificação para abrir o Windows Update e obter mais informações sobre o software ou instalá-lo. O usuário também pode clicar em **Fechar essa mensagem** ou **Mostrar mais tarde** para adiar a notificação conforme apropriado.<br /><br />No Windows 7, essa configuração de política só controlará as notificações detalhadas para aplicativos opcionais. No Windows Vista, essa configuração de política controla as notificações detalhadas para aplicativos e atualizações opcionais.|
|**Desabilitada**|Especifica que os usuários que executam o Windows 7 não receberão mensagens de notificação detalhadas para aplicativos opcionais e os usuários que executam o Windows Vista não receberão mensagens de notificação detalhadas para aplicativos opcionais ou atualizações opcionais.|

**Opções:** Não há opções para essa configuração.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuração do computador > Configurações de política do Agendador de Manutenção
Em Configurar Atualizações Automáticas, você selecionou a opção **4 – Baixar automaticamente e agendar a instalação**; você pode especificar as configurações do Agendador de Manutenção de agendamento no GPMC para computadores que executam o Windows 8 e o Windows RT. Se você não selecionou a opção 4 na configuração "Configurar Atualizações Automáticas", não será necessário definir essas configurações para fins de atualizações automáticas. As configurações do Agendador de Manutenção estão localizadas no caminho: *PolicyName* > **Configuração do Computador** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Agendador de Manutenção**. A extensão do Agendador de Manutenção da Política de Grupo contém as seguintes configurações:

-   [Limite de Ativação de Manutenção Automática](#automatic-maintenance-activation-boundary)

-   [Atraso Aleatório de Manutenção Automática](#automatic-maintenance-random-delay)

-   [Política de Ativação Automática](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Limite de Ativação de Manutenção Automática
Essa política permite que você configure a configuração "Limite de ativação de Manutenção Automática".

O limite de ativação de manutenção é o horário agendado diário no qual a manutenção automática é iniciada.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração está relacionada à opção 4 em **Configurar Atualizações Automáticas**. Se você não selecionou a opção 4 em **Configurar Atualizações Automáticas**, não será necessário definir essa configuração.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Se essa configuração de política não estiver configurada, o horário agendado diariamente, conforme especificado nos computadores cliente no Painel de Controle da **Central de Ações** > **Manutenção Automática**, será aplicado.|
|**Habilitada**|A habilitação dessa configuração de política substitui qualquer configuração padrão ou modificada definida em computadores cliente no **Painel de Controle** > **Central de Ações** > **Manutenção Automática** (ou, em algumas versões de cliente, **Manutenção**).|
|**Desabilitada**|Se você definir essa configuração de política como **Desabilitada**, a hora agendada diária, conforme especificada no Painel de Controle da **Central de Ações** > **Manutenção Automática**, será aplicada.|

#### <a name="automatic-maintenance-random-delay"></a>Atraso Aleatório de Manutenção Automática
Essa configuração de política permite que você configure o atraso aleatório de ativação de Manutenção Automática.

O atraso aleatório de manutenção é o tempo pelo qual a Manutenção Automática adiará o início de seu limite de ativação. Essa configuração é útil para máquinas virtuais em que a manutenção aleatória pode ser um requisito de desempenho.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração está relacionada à opção 4 em **Configurar Atualizações Automáticas**. Se você não selecionou a opção 4 em **Configurar Atualizações Automáticas**, não será necessário definir essa configuração.

Por padrão, quando habilitado, o atraso aleatório de manutenção regular é definido como **PT4H**.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Um atraso aleatório de quatro horas é aplicado a **Automático**.|
|**Habilitada**|A Manutenção Automática adiará o início de seu limite de ativação até a quantidade de tempo especificada.|
|**Desabilitada**|Nenhum atraso aleatório é aplicado à Manutenção Automática.|

#### <a name="automatic-wakeup-policy"></a>Política de Ativação Automática
Essa configuração de política permite que você configure a política de ativação da Manutenção Automática.

A política de ativação de manutenção especifica se a Manutenção Automática deve fazer uma solicitação de ativação ao computador operacional para a manutenção agendada diária.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Se a política de ativação de energia do computador operacional estiver explicitamente desabilitada, essa configuração não terá efeito.

> [!NOTE]
> Essa configuração está relacionada à opção 4 em **Configurar Atualizações Automáticas**. Se você não selecionou a opção 4 em **Configurar Atualizações Automáticas**, não será necessário definir essa configuração.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Se você não definir essa configuração de política, a configuração de ativação, conforme especificado no Painel de Controle da **Central de Ações** > **Manutenção Automática** controle será aplicada.|
|**Habilitada**|Se você habilitar essa configuração de política, a manutenção automática tentará definir uma política de ativação do sistema operacional e fará uma solicitação de ativação para a hora agendada diária, se necessário.|
|**Desabilitada**|Se você desabilitar essa configuração de política, a configuração de ativação, conforme especificado no Painel de Controle da **Central de Ações** > **Manutenção Automática** controle será aplicada.|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuração do usuário > Configurações de política do Windows Update
Esta seção fornece detalhes sobre as seguintes configurações de política baseadas em usuário:

-   [Não exibir a opção "Instalar atualizações e Desligar" na caixa de diálogo Desligar o Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Não ajustar a opção padrão para "Instalar Atualizações e Desligar" na caixa de diálogo Desligar o Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Remover o acesso para usar todos os recursos do Windows Update](#remove-access-to-use-all-windows-update-features)

No GPMC, as configurações de usuário para atualizações automáticas do computador estão localizadas no caminho: *PolicyName* > **Configuração do Usuário** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update**. As configurações são listadas na mesma ordem em que aparecem na configuração do computador e nas extensões de configuração do usuário em Política de Grupo, quando a guia **Configurações** da política do Windows Update é selecionada para classificar as configurações em ordem alfabética.

> [!NOTE]
> Por padrão, a menos que indicado de outra forma, essas configurações não são definidas.

> [!TIP]
> Para cada uma dessas configurações, você pode usar as seguintes etapas para habilitar, desabilitar ou navegar entre as configurações:

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>Não exibir a opção "Instalar atualizações e Desligar" na caixa de diálogo Desligar o Windows
Especifica se a opção **Instalar Atualizações e Desligar** é exibida na caixa de diálogo **Desligar o Windows**.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica que a opção **Instalar Atualizações e Desligar** aparecerá na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis quando o usuário selecionar a opção Desligar para desligar o computador.|
|**Habilitada**|Se você habilitar esta configuração de política, **Instalar Atualizações e Desligar** não aparecerá como uma opção na caixa de diálogo **Desligar o Windows**, mesmo se houver atualizações disponíveis para instalação quando o usuário selecionar a opção desligar para desligar o computador.|
|**Desabilitada**|Especifica que a opção **Instalar Atualizações e Desligar** aparecerá na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis quando o usuário selecionar a opção Desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>Não ajustar a opção padrão para "Instalar Atualizações e Desligar" na caixa de diálogo Desligar o Windows
Especifica se a opção **Instalar Atualizações e Desligar** é permitida como a opção padrão na caixa de diálogo **Desligar o Windows**.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Essa configuração de política não será afetará se *PolicyName* > **Configuração do Usuário** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update** > **Não exibir a opção "Instalar Atualizações e Desligar" na caixa de diálogo Desligar o Windows** estiver habilitada.

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Especifica se **Instalar Atualizações e Desligar** será a opção padrão na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|
|**Habilitada**|Especifica se a última opção de desligar do usuário (por exemplo, Hibernar ou Reiniciar) será a opção padrão na caixa de diálogo **Desligar o Windows**, independentemente de a opção **Instalar Atualizações e Desligar** estar disponível no menu **O que você deseja que o computador faça?** .|
|**Desabilitada**|Especifica se **Instalar Atualizações e Desligar** será a opção padrão na caixa de diálogo **Desligar o Windows** se houver atualizações disponíveis para instalação no momento em que o usuário selecionar a opção desligar para desligar o computador.|

**Opções:** Não há opções para essa configuração.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Remover o acesso para usar todos os recursos do Windows Update
Essa configuração permite remover o acesso de cliente do WSUS ao Windows Update.

|Compatível com:|Excluindo:|
|---------|-------|
|Os sistemas operacionais Windows que ainda estão dentro de seu [Ciclo de Vida de Suporte de Produtos da Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuração de política**|**Comportamento**|
|**Não Configurado**|Os usuários podem se conectar ao site Windows Update.|
|**Habilitada**|**IMPORTANTE:** se essa opção estiver habilitada, todos os recursos de Windows Update serão removidos. Isso inclui bloquear o acesso ao site Windows Update em https://windowsupdate.microsoft.com, do hiperlink do Windows Update no menu Iniciar ou tela inicial e também no menu **Ferramentas** no Internet Explorer. A atualização automática do Windows também está desabilitada; o usuário não será notificado nem receberá atualizações críticas do Windows Update. Essa configuração também impede que o Gerenciador de Dispositivos instale automaticamente atualizações de driver do site Windows Update.<br /><br />Quando habilitada, você pode configurar uma das seguintes opções de notificação:<br /><br />-   **0 – Não mostrar notificações**<br />    Essa configuração removerá todo o acesso a recursos do Windows Update e nenhuma notificação será mostrada.<br />-   **1 – Mostrar notificações necessárias de reinicialização**<br />    Essa configuração mostrará notificações sobre reinicializações necessárias para concluir uma instalação. **Observação**: Em computadores que executam o Windows 8 e o Windows RT, se essa política estiver habilitada, somente as notificações relacionadas à reinicializações e à incapacidade de detectar atualizações serão mostradas. Não há suporte para as opções de notificação. As notificações na tela de entrada sempre são exibidas.|
|**Desabilitada**|Os usuários podem se conectar ao site Windows Update.|

**Opções:** Confira **Habilitado** na tabela para essa configuração.

## <a name="supplemental-information"></a>Informações complementares
Esta seção fornece informações de adição sobre como usar, abrir e salvar configurações do WSUS em políticas de grupo, bem como definições para termos usados neste guia. Para administradores familiarizados com versões anteriores do WSUS (WSUS 3.2 e versões anteriores), há uma tabela que resume rapidamente as diferenças entre as versões do WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Como acessar as configurações do Windows Update na Política de Grupo
O procedimento a seguir descreve como abrir o GPMC em seu controlador de domínio. Em seguida, o procedimento descreve como abrir um GPO (Objeto de Política de Grupo) no nível de domínio existente para edição ou criar um novo GPO em nível de domínio e abri-lo para edição.

> [!NOTE]
> É preciso ser membro do grupo **Administradores de Domínio** ou equivalente para realizar esse procedimento.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir ou adicionar e abrir um Objeto de Política de Grupo

1.  No controlador de domínio, acesse **Gerenciador do Servidor** > **Ferramentas** > **Gerenciamento de Política de Grupo**. O Console de Gerenciamento de Política de Grupo é aberto.

2.  No painel esquerdo, expanda sua floresta. Por exemplo, clique duas vezes em **floresta: example.com**.

3.  No painel esquerdo, clique duas vezes em **Domínios** e, em seguida, clique duas vezes no domínio para o qual você deseja gerenciar um objeto de Política de Grupo. Por exemplo, clique duas vezes em **example.com**.

4.  Realize um dos seguintes procedimentos:

    -  **Para abrir um GPO em nível de domínio existente para edição**, clique duas vezes no domínio que contém o objeto de Política de Grupo que você deseja gerenciar, clique com o botão direito do mouse na política de domínio que você deseja gerenciar e clique em **Editar**. O GPME (Editor de Gerenciamento de Política de Grupo) é aberto.

    -  **Para criar um novo objeto de Política de Grupo e abrir para edição**:
        1.  Clique com o botão direito do mouse no domínio para o qual você deseja criar um objeto de Política de Grupo e então clique em **Criar um GPO neste domínio e vinculá-lo aqui**.

        2.  Em **Novo GPO**, em **Nome**, digite um nome para o novo objeto de Política de Grupo e clique em **OK**.

        3.  Clique com o botão direito do mouse em seu novo objeto de Política de Grupo e clique em **Editar**. O GPME é aberto.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Para abrir as extensões do Windows Update ou as extensões do Agendador de Manutenção da Política de Grupo

1.  No editor de Gerenciamento de Política de Grupo, siga um destes procedimentos:

    -   **Abra Configuração do computador > extensão do Windows Update de Política de Grupo**. Vá até: *PolicyName* > **Configuração do Computador** > **Políticas** / **Modelos Administrativos** > **Componentes do Windows** > **Windows Update**.

    -   **Abra Configuração do Usuário > extensão do Windows Update de Política de Grupo**. Vá até: *PolicyName* > **Configuração do Usuário** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update**.

    -   **Abra a Configuração do Computador > Extensão do Agendador de Manutenção do Política de Grupo**. No GPOE, navegue para *PolicyName* > **Configuração do Computador** > **Políticas** > **Modelos Administrativos** > **Componentes do Windows** > **Agendador de Manutenção**.

Para obter mais informações sobre a Política de Grupo, confira [Visão geral da Política de Grupo](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Depois de abrir a extensão de Política de Grupo desejada, use as seguintes etapas para habilitar, desabilitar ou navegar entre as configurações:

##### <a name="to-configure-group-policy-settings"></a>Para definir configurações da Política de grupo

1.  Em *ExtensionOfGroupPolicy*, clique duas vezes na configuração que você deseja exibir ou modificar.

2.  Para definir a configuração, siga um destes procedimentos:

    -   Para manter o estado padrão não especificado da configuração, selecione **Não Configurado**.

    -   Para habilitar a configuração, selecione **Habilitado**.

    -   Para desabilitar a configuração, selecione **Desabilitado**.

3.  Em **Opções**, se houver opções listadas, mantenha os valores padrão ou modifique-os conforme necessário.

4.  Realize um dos seguintes procedimentos:

    -   Para salvar as alterações e prosseguir para a próxima configuração, clique em **Aplicar** e, em seguida, clique em **Próxima configuração**.

    -   Para salvar as alterações e fechar a caixa de diálogo, clique em **OK**.

    -   Para descartar todas as alterações não salvas e fechar a caixa de diálogo, clique em **Cancelar**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Alterações ao WSUS relevantes para este guia
A tabela a seguir resume as principais diferenças entre as versões atuais e anteriores do WSUS que são relevantes para este guia.

|Versões do Windows Server e do WSUS|Descrição|
|------------------|--------|
| Windows Server 2012 R2 com WSUS 6.0 e versões subsequentes|Do Windows Server 2012 em diante, a função de servidor do WSUS é integrada ao sistema operacional e as configurações de Política de Grupo associadas para clientes do WSUS são, por padrão, incluídas na Política de Grupo.|
| Windows Server 2008 (e versões anteriores do Windows Server) com o WSUS 3.2 e anterior|No Windows Server 2008 (e versões anteriores do Windows Server) usando as versões 3.2 do WSUS (e anteriores), as configurações de Política de Grupo que regem os clientes WSUS não estão incluídas nesses sistemas operacionais do Windows Server. As configurações de política estão no modelo administrativo do WSUS, **wuau.adm**. Nessas versões de servidor, o modelo administrativo do WSUS deve primeiro ser adicionado ao GPMC (Console de Gerenciamento de Política de Grupo) antes que as configurações do cliente do WSUS possam ser configuradas.|

### <a name="terms-and-definitions"></a>Termos e definições
Veja a seguir uma lista de termos usados neste guia.

|Termo|Definição|
|----|-------|
|Atualizações Automáticas|**Um serviço executado em computadores Windows** (Atualizações Automáticas): refere-se ao componente de computador cliente interno dos sistemas operacionais Microsoft Windows Vista, Windows Server 2003, Windows XP e Windows 2000 com SP3 para obter atualizações de Microsoft Update ou Windows Update.<br /><br />**Referência Casual** (atualizações automáticas): o termo usado para descrever quando o agente de Windows Update agenda e baixa automaticamente as atualizações.|
|servidor autônomo|Use para fazer referência a um servidor do WSUS (Windows Server Update Services) downstream no qual os administradores podem gerenciar os componentes do WSUS.|
|servidor downstream|Use para fazer referência a um servidor do WSUS (Windows Server Update Services) que obtém atualizações de outro servidor do WSUS, em vez do Microsoft Update ou do Windows Update.|
|Extensão de Política de Grupo (e: extensão de Política de Grupo)|Uma coleção de configurações em Política de Grupo usadas para controlar como os usuários e os computadores (aos quais as políticas se aplicam) podem configurar e usar vários serviços e recursos do Windows. Os administradores podem usar o WSUS com Política de Grupo para a configuração do lado do cliente para o cliente Atualizações Automáticas para ajudar a garantir que os usuários finais não possam desabilitar nem burlar políticas de atualização corporativa.<br /><br />O WSUS não requer o uso do Active Directory nem da Política de Grupo. A configuração do cliente também pode ser aplicada usando a política de grupo local ou modificando o Registro do Windows.|
|serviço de atualização interno|Uma referência casual a uma infraestrutura de rede que usa um ou mais servidores WSUS para distribuir atualizações.|
|servidor de réplica|Use para fazer referência a um servidor do WSUS (Windows Server Update Services) downstream que espelha as aprovações e configurações no servidor upstream ao qual ele está conectado. Você não pode gerenciar o WSUS em um servidor de réplica.|
|Microsoft Update|**Um site de download da Microsoft baseado na Internet:** Um site da Internet da Microsoft que armazena e distribui atualizações para computadores Windows (drivers de dispositivo), sistemas operacionais Windows e outros produtos de software da Microsoft.|
|SUS (Serviços de Atualização de Software)|O SUS foi o produto predecessor do WSUS (Windows Server Update Services).|
|atualizações|Qualquer coleção de revisões de software, hotfixes, Service Packs, pacotes de recursos e drivers de dispositivo que podem ser instalados em um computador para estender a funcionalidade ou melhorar o desempenho e a segurança.|
|arquivos de atualização|Os arquivos necessários para instalar uma atualização em um computador.|
|Informações sobre a atualização (também conhecidas como metadados de atualização)|As informações sobre uma atualização, em comparação aos arquivos binários de atualização em um pacote de atualização. Por exemplo, os metadados fornecem informações sobre as propriedades de uma atualização, permitindo que você determine para que a atualização é útil. Os metadados incluem também Termos de Licença para Software Microsoft. Normalmente, o pacote de metadados baixado para uma atualização é muito menor do que o pacote de arquivos de atualização real.|
|atualizar origem|O local com o qual um servidor do WSUS (Windows Server Update Services) sincroniza-se para obter arquivos de atualização. Esse local pode ser o Microsoft Update ou um servidor WSUS upstream.|
|servidor upstream|Um servidor do WSUS (Windows Server Update Services) que fornece arquivos de atualização para outro servidor WSUS, que, por sua vez, é chamado de servidor downstream.|
|Windows Server Update Services (WSUS)|Um programa de função de servidor executado em um ou mais computadores Windows Server em uma rede corporativa. Uma infraestrutura do WSUS permite que você gerencie as atualizações de computadores na rede a serem instaladas.<br /><br />Você pode usar o WSUS para aprovar ou recusar atualizações antes do lançamento, forçar as atualizações a serem instaladas até uma determinada data e obter relatórios extensos sobre as atualizações que cada computador em sua rede exige. Você pode configurar o WSUS para aprovar determinadas classes de atualizações automaticamente (atualizações críticas, atualizações de segurança, Service Packs, Drivers etc.). O WSUS também permite que você aprove atualizações apenas para "detecção", para que você possa ver quais computadores precisarão de uma determinada atualização sem precisar instalar as atualizações.<br /><br />Em uma implementação do WSUS, pelo menos um servidor do WSUS na rede deve conseguir conectar-se ao Microsoft Update para obter as atualizações disponíveis. Com base na segurança e configuração da rede, o administrador pode determinar quantos servidores estão diretamente conectados ao Microsoft Update.<br /><br />Você pode configurar um servidor do WSUS para obter atualizações pela Internet de locais como:<br /><br />- o Microsoft Update público<br />- o Windows Update público<br />-   Microsoft Store|
|Windows Update|**Um site de download da Microsoft baseado na Internet:** Um site da Internet da Microsoft que armazena e distribui atualizações para computadores Windows (drivers de dispositivo) e sistemas operacionais Windows.<br /><br />**Serviço de computador:** o nome do serviço de Windows Update executado em computadores. O Windows Update detecta, baixa e instala atualizações em computadores Windows.<br /><br />Dependendo das configurações do computador e da política, o agente do Windows Update pode baixar atualizações de:<br /><br />-   Microsoft Update<br />-   Windows Update<br />-   Microsoft Store<br />-   Um serviço de atualização de Internet (rede) (WSUS)<br /><br />Os computadores que não são gerenciados em um ambiente baseado no WSUS normalmente usarão o Windows Update para conectarem-se diretamente pela Internet ao Windows Update, ao Microsoft Update ou à Microsoft Store para obterem atualizações.|
|Cliente do WSUS|Um computador que recebe atualizações de um serviço de atualização da intranet do WSUS.<br /><br />No caso de configurações de Política de Grupo que controlam a interação do usuário final com as Atualizações Automáticas: um usuário de computador em um ambiente do WSUS.|
