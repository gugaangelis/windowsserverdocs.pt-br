---
title: 'Etapa 4: definir configurações de política de grupo para atualizações automáticas'
description: Tópico do Windows Server Update Service (WSUS) - configurar configurações de diretiva de grupo para atualizações automáticas é a etapa quatro em um processo de quatro etapas para implantar o WSUS
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 08cc0b31aa123aadd57a0ea5ddbbeb96bffc3d6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880097"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Etapa 4: Definir configurações de política de grupo para atualizações automáticas

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em um ambiente do Active Directory, você pode usar a diretiva de grupo para definir como os computadores e usuários (referida neste documento como os clientes do WSUS) podem interagir com as atualizações do Windows para obter as atualizações automáticas do Windows Server Update Services (WSUS).

Este tópico contém duas seções principais:

[Grupo de configurações de política para atualizações de cliente do WSUS](4-configure-group-policy-settings-for-automatic-updates.md#BKMK_PolSettings), que fornece uma orientação prescritiva e comportamentais fornece detalhes sobre as configurações de atualização do Windows e o Agendador de manutenção da diretiva de grupo que controlam como os clientes do WSUS podem interagir com o Windows Update Para obter as atualizações automáticas.

[Informações complementares](4-configure-group-policy-settings-for-automatic-updates.md#BKMK_Supplemental) tem as seguintes seções:

-   [Acessando as configurações de atualização do Windows na política de grupo](4-configure-group-policy-settings-for-automatic-updates.md#BKMK_OpenGPO), que fornece diretrizes gerais sobre como usar o editor de gerenciamento de diretiva de grupo e informações sobre como acessar as extensões de serviços de atualização de política e configurações do Agendador de manutenção em Diretiva de grupo.

-   [Altera para o WSUS relevante para este guia](4-configure-group-policy-settings-for-automatic-updates.md#BKMK_changes): para os administradores familiarizados com o WSUS 3.2 e nas versões anteriores, esta seção fornece um breve resumo das principais diferenças entre a versão atual e anterior do WSUS relevante para este guia.

-   [Termos e definições](4-configure-group-policy-settings-for-automatic-updates.md#BKMK_Terms): definições para vários termos referentes aos serviços WSUS e atualização que são usados neste guia.

## <a name="BKMK_PolSettings"></a>Configurações de diretiva de grupo para atualizações de cliente do WSUS
Esta seção fornece informações sobre três extensões da diretiva de grupo. Essas extensões, você encontrará as configurações que você pode usar para configurar como os clientes do WSUS podem interagir com o Windows Update para receber atualizações automáticas.

-   [Configuração do computador &gt; configurações de política de atualização do Windows](#BKMK_computerPol)

-   [Configuração do computador &gt; configurações de política de Agendador de manutenção](#BKMK_MtncScheduler)

-   [Configuração do usuário &gt; configurações de política de atualização do Windows](#BKMK_UserPol)

> [!NOTE]
> Este tópico pressupõe que você já usa e estiver familiarizado com a diretiva de grupo. Se você não estiver familiarizado com a diretiva de grupo, é recomendável que você examine as informações de [informações complementares](#BKMK_Supplemental) seção deste documento antes de tentar definir as configurações de política para o WSUS.

### <a name="BKMK_computerPol"></a>Configuração do computador > configurações de política de atualização do Windows
Esta seção fornece detalhes sobre as configurações de política com base no computador a seguir:

-   [Permitir instalação imediata de atualizações automáticas](#BKMK_comp1)

-   [Permitir que não administradores receber notificações de atualização](#BKMK_comp2)

-   [Permitir atualizações assinadas de uma intranet local do serviço do Microsoft update](#BKMK_comp3)

-   [Frequência de detecção de atualizações automática](#BKMK_comp4)

-   [Configurar as atualizações automáticas](#BKMK_comp5)

-   [Reinicialização de atraso para instalações agendadas](#BKMK_comp6)

-   [Ajustar a opção de padrão para "Instalar atualizações e desligar" na caixa de diálogo de desligar o Windows](#BKMK_comp7)

-   [Não exibir a opção "Instalar atualizações e desligar" na caixa de diálogo de desligar o Windows](#BKMK_comp8)

-   [Habilitar destino do lado do cliente](#BKMK_comp9)

-   [Habilitar o gerenciamento de energia do Windows Update para automaticamente ativar o computador para instalar atualizações agendadas](#BKMK_comp10)

-   [Não reinicializar automaticamente com usuários conectados para automática agendada de atualizações de instalações](#BKMK_comp11)

-   [Solicitar reinicialização com instalações agendadas novamente](#BKMK_comp12)

-   [Reagendar instalação agendada de atualizações automáticas](#BKMK_comp13)

-   [Especifique o local de serviço de atualização na intranet da Microsoft](#BKMK_comp14)

-   [Ative as atualizações recomendadas via atualizações automáticas](#BKMK_comp15)

-   [Ativar notificações de Software](#BKMK_comp16)

No GPME, diretivas de atualização do Windows para configuração baseada em computador estão localizadas no caminho: *PolicyName* > **configuração do computador** > **políticas** > **modelos administrativos**  >  **Componentes do Windows** > **Windows Update**.

> [!NOTE]
> Por padrão, essas configurações não são configuradas.

#### <a name="BKMK_comp1"></a>Permitir instalação imediata de atualizações automáticas
Especifica se as atualizações automáticas instalará automaticamente as atualizações que não interrompem os serviços do Windows ou reinicie o Windows.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Se a configuração de política "Configurar atualizações automáticas" será definida como **desabilitado**, essa política não tem nenhum efeito.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que as atualizações não são instaladas imediatamente. Os administradores locais podem alterar essa configuração usando o editor de diretiva de Grupo Local.|
|**Enabled**|Especifica que as atualizações automáticas imediatamente instala atualizações depois que eles são baixados e pronto para instalar.|
|**Desabilitado**|Especifica que as atualizações não são instaladas imediatamente.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_comp2"></a>Permitir que não administradores receber notificações de atualização
Especifica se os usuários não administrativos receberá notificações de atualização com base na configuração de política de configurar atualizações automáticas.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Consulte os detalhes na tabela a seguir.|

> [!NOTE]
> Se a configuração de política "Configurar atualizações automáticas" está desabilitada ou não está configurada, essa configuração de política não tem efeito.

> [!IMPORTANT]
> começando no Windows 8 e Windows RT, essa configuração de política é habilitada por padrão. Em todas as versões anteriores do Windows, ele é desabilitado por padrão.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que os usuários sempre vê uma janela de controle de conta e exigem permissões elevadas para executar essas tarefas. Um administrador local pode alterar essa configuração usando o editor de diretiva de Grupo Local.|
|**Enabled**|Especifica se a atualização automática do Windows e o Microsoft Update incluirá não-administradores ao determinar qual usuário conectado receberá notificações de atualização. Os usuários não administrativos poderão instalar todo o conteúdo atualização opcional, recomendado e importante para os quais eles receberam uma notificação. Os usuários não verão uma janela de controle de conta de usuário, e eles não precisam de permissões com privilégios elevados para instalar essas atualizações, exceto no caso de atualizações que contêm alterações de configuração de Interface do usuário, o contrato de licença de usuário final ou o Windows Update.<br /><br />Há duas situações em que o efeito dessa configuração depende do computador operacional:<br /><br />1.  **Ocultar** ou **restaurar** atualizações<br />2.  **Cancelar** uma instalação de atualização<br /><br />No Windows Vista ou Windows XP, se essa configuração de política estiver habilitada, os usuários não verão uma janela de controle de conta de usuário, e eles não precisam de permissões elevadas para ocultar, restaurar ou cancelar atualizações.<br /><br />No Windows Vista, se essa configuração de política estiver habilitada, os usuários não verão uma janela de controle de conta de usuário, e eles não precisam de permissões elevadas para ocultar, restaurar ou cancelar atualizações. Se essa configuração de política não estiver habilitada, os usuários sempre verão uma janela de controle de conta e exigem permissões elevadas para ocultar, restaurar ou cancelar atualizações.<br /><br />No Windows 7, essa configuração de política não tem efeito. Os usuários sempre verão uma janela de controle de conta e exigem permissões elevadas para executar essas tarefas.<br /><br />No Windows 8 e Windows RT, essa configuração de política não tem efeito.|
|**Desabilitado**|Especifica que o logon somente os administradores recebam notificações de atualização. **Observação:** No Windows 8 e Windows RT, essa configuração de política está habilitada por padrão. Em todas as versões anteriores do Windows, ele é desabilitado por padrão.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_comp3"></a>Permitir atualizações assinadas de uma intranet local do serviço do Microsoft update
Especifica se as atualizações automáticas aceita as atualizações que são assinadas por entidades que não seja da Microsoft quando a atualização é encontrada em um local do serviço Microsoft update da intranet.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Atualizações de um serviço que não seja um serviço de atualização da intranet Microsoft sempre devem ser assinadas pela Microsoft, e eles não são afetados por essa configuração de política.

> [!NOTE]
> Não há suporte para essa política RT Windows. A habilitação dessa política não terá nenhum efeito em computadores que executam RT Windows.

**Opções:** Não existem opções para essa configuração.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que as atualizações de uma intranet local do serviço Microsoft update devem ser assinadas pela Microsoft.|
|**Enabled**|Especifica que as atualizações automáticas aceita as atualizações recebidas através de uma intranet local do serviço Microsoft update se forem assinados por um certificado localizado no repositório de certificados do computador local "Trusted Publishers".|
|**Desabilitado**|Especifica que as atualizações de uma intranet local do serviço Microsoft update devem ser assinadas pela Microsoft.|

**Opções:** Não existem opções para essa configuração.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Sempre reiniciar automaticamente no horário agendado
Especifica se um temporizador de reinício sempre será iniciada imediatamente após a atualização do Windows instala atualizações importantes, em vez de primeiros usuários de notificação na tela de entrada pelo menos dois dias.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Se a configuração de política "instalações de atualizações não reinicializar automaticamente com usuários conectados para automático agendado" estiver habilitada, essa política não terá efeito.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que o Windows Update não alterará o comportamento de reinicialização do computador.|
|**Enabled**|Especifica que um temporizador de reinício sempre será iniciada imediatamente após a atualização do Windows instala atualizações importantes, em vez de primeiros usuários de notificação na tela de entrada pelo menos dois dias.<br /><br />O temporizador de reinício pode ser configurado para iniciar com qualquer valor de 15 a 180 minutos. Quando o timer de execução se esgotar, a reinicialização continuará mesmo se o computador tem usuários conectados.|
|**Desabilitado**|Especifica que o Windows Update não alterará o comportamento de reinicialização do computador.|

**Opções:** se essa configuração estiver habilitada, você pode especificar a quantidade de tempo que devem decorrer depois que as atualizações são instaladas antes que ocorra a reinicialização do computador forçado.

#### <a name="BKMK_comp4"></a>Frequência de detecção de atualizações automática
Especifica a quantidade de horas que o Windows usará para determinar por quanto tempo deve esperar para verificar se há atualizações disponíveis. O tempo de espera exato é determinado pelas horas especificadas aqui menos zero a 20% das horas especificadas. Por exemplo, se essa política é usada para especificar uma frequência de detecção de 20 horas, todos os clientes aos quais essa política é aplicada verificará atualizações em qualquer lugar entre 16 e 20 horas.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> A configuração "Especificar intranet local do serviço do Microsoft update" deve ser habilitada para esta política tenha efeito.
>
> Se a configuração de política "Configurar atualizações automáticas" estiver desativada, essa política não tem nenhum efeito.

> [!NOTE]
> Não há suporte para essa política RT Windows. A habilitação dessa política não terá nenhum efeito em computadores que executam RT Windows.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que o Windows verificará se há atualizações disponíveis no intervalo padrão de 22 horas.|
|**Enabled**|Especifica que o Windows verificará se há atualizações disponíveis no intervalo especificado.|
|**Desabilitado**|Especifica que o Windows verificará se há atualizações disponíveis no intervalo padrão de 22 horas.|

**Opções:** se essa configuração estiver habilitada, você pode especificar o intervalo de tempo (em horas) que a atualização do Windows aguarda antes de verificar se há atualizações.

#### <a name="BKMK_comp5"></a>Configurar as atualizações automáticas
Especifica especificar se as atualizações automáticas estão habilitadas neste computador.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Se habilitada, você deve selecionar uma das quatro opções fornecidas nessa configuração de diretiva de grupo.

Para usar essa configuração, selecione **Enabled**e, em seguida, na **opções** sob **configurar a atualização automática**, selecione uma das opções (2, 3, 4 ou 5).

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que o uso das atualizações automáticas não for especificado no nível de política de grupo. No entanto, um administrador do computador ainda pode configurar as atualizações automáticas no painel de controle.|
|**Enabled**|Especifica que o Windows reconhece quando o computador está online e usa sua conexão de Internet para pesquisar o Windows Update para as atualizações disponíveis.<br /><br />Quando habilitada, os administradores locais poderão usar o painel de controle do Windows Update para selecionar uma opção de configuração de sua preferência. No entanto, os administradores locais não poderá desabilitar a configuração para atualizações automáticas.<br /><br />-   **2 - Avisar antes de baixar e notificar antes de instalar**<br />    Quando o Windows Update encontra as atualizações que se aplicam ao computador, os usuários serão notificados que as atualizações estão prontas para download. Os usuários, em seguida, podem executar o Windows Update para baixar e instalar todas as atualizações disponíveis.<br />-   **3 - Baixar automaticamente e notificar antes de instalar** (configuração padrão)<br />    Atualização do Windows encontra as atualizações aplicáveis e baixá-los em segundo plano; o usuário é notificado ou interrompido durante o processo. Quando os downloads forem concluídos, os usuários são notificados de que há atualizações prontas para serem instaladas. Os usuários, em seguida, podem executar o Windows Update para instalar as atualizações baixadas.<br />-   **4 - Baixar automaticamente e agendar a instalação**<br />    Você pode especificar o agendamento, usando as opções nessa configuração de diretiva de grupo. Se nenhum agendamento for especificado, o agendamento padrão para todas as instalações serão todos os dias às 03h00. Se alguma atualização exigir uma reinicialização para concluir a instalação, o Windows serão reiniciado automaticamente o computador. (se um usuário está conectado ao computador quando o Windows está pronto para reiniciar, o usuário será ser notificado e terá a opção de adiar a reinicialização.) **Observação:** a partir do Windows 8, você pode definir as atualizações para instalar durante a manutenção automática em vez de usar uma agenda específica ligada a atualização do Windows. Manutenção automática instalará as atualizações quando o computador não está em uso e evitar a instalação de atualizações quando o computador está funcionando com bateria. Se a manutenção automática não conseguir instalar atualizações em dias, o Windows Update instalará atualizações imediatamente. Os usuários, em seguida, serão notificados sobre uma reinicialização pendente. Uma reinicialização pendente ocorrerá somente se não houver nenhuma possibilidade de perda acidental de dados.    Você pode especificar opções de agendamento nas configurações do Agendador de manutenção do GPME, que estão localizadas no caminho, *PolicyName* > **computer Configuration**  >  **Diretivas** > **modelos administrativos** > **componentes do Windows** > **manutenção O Agendador** > **limite de ativação de manutenção automática**. Consulte a seção dessa referência intitulada: [Configurações do Agendador de manutenção](#BKMK_MtncScheduler), para obter detalhes de configuração.    **5 - permitir que o administrador local escolha a configuração**<br />-Especifica se os administradores locais têm permissão para usar o painel de controle de atualizações automáticas para selecionar uma opção de configuração de sua escolha, por exemplo, se o local de administradores pode escolher um horário agendado.<br />    Os administradores locais não poderão desabilitar a configuração para as Atualizações Automáticas.|
|**Desabilitado**|Especifica que todas as atualizações de cliente que estão disponíveis no serviço Windows Update público devem ser manualmente baixadas da Internet e instaladas.|

#### <a name="BKMK_comp6"></a>Reinicialização de atraso para instalações agendadas
Especifica a quantidade de tempo que as atualizações automáticas aguardará antes de prosseguir com uma reinicialização agendada.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa política se aplica somente quando as atualizações automáticas está configurada para executar a instalação agendada de atualizações. Se a configuração de política "Configurar atualizações automáticas" está desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que depois que as atualizações são instaladas, o tempo de espera padrão de quinze minutos devem decorrer antes que qualquer reinicialização agendada ocorra.|
|**Enabled**|Especifica que quando a instalação for concluída, uma reinicialização agendada ocorrerá depois que o número de minutos especificado tiver expirado.|
|**Desabilitado**|Especifica que depois que as atualizações são instaladas, o tempo de espera padrão de quinze minutos devem decorrer antes que qualquer reinicialização agendada ocorra.|

**Opções:** se essa configuração estiver habilitada, você pode usar essa opção para especificar a quantidade de tempo (em minutos) de atualizações automáticas espera antes de prosseguir com uma reinicialização agendada.

#### <a name="BKMK_comp7"></a>Ajustar a opção de padrão para "Instalar atualizações e desligar" na caixa de diálogo de desligar o Windows
Essa configuração de política permite que você especifique se o **instalar atualizações e desligar** opção é permitida como a opção padrão no **desligar o Windows** caixa de diálogo.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa configuração de política não tem impacto se o *PolicyName* > **computer Configuration** > **políticas**  >  **Modelos administrativos** > **componentes do Windows** > **Windows Update** > **não exibir " Opção de instalar atualizações e desligar"na caixa de diálogo de desligar o Windows** está habilitada.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que **instalar atualizações e desligar** será a opção padrão na **desligar o Windows** caixa de diálogo se houver atualizações disponíveis para instalação no momento em que o usuário seleciona a opção de desligar a desligar o computador.|
|**Enabled**|Se você habilitar essa configuração de política, última opção de desligamento do usuário (por exemplo, ou hibernação reinicialização), é a opção padrão na **desligar o Windows** caixa de diálogo, independentemente se o **instalar atualizações e desligar**  opção está disponível na **o que você deseja com o computador faça?** menu.|
|**Desabilitado**|Especifica que **instalar atualizações e desligar** será a opção padrão na **desligar o Windows** caixa de diálogo se houver atualizações disponíveis para instalação no momento em que o usuário seleciona a opção de desligar a desligar o computador.|

**Opções:** Não existem opções para essa configuração.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>Não se conectar a locais de Internet do Windows Update
Até mesmo quando atualizar do Windows está configurado para receber atualizações de um serviço de atualização da intranet, ele será periodicamente recuperar informações do serviço público atualizar do Windows para permitir conexões futuras com o Windows Update e outros serviços, como o Microsoft Update ou Microsoft Store.

A habilitação dessa política desabilitará a funcionalidade para recuperar periodicamente as informações do banco de dados público do Windows Server Update Services, e pode fazer com que a conexão para serviços públicos, como a Microsoft Store para parar de funcionar.

|Suporte para:|Excluindo:|
|---------|-------|
|começando com o Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1, sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa política se aplica somente quando o computador está configurado para se conectar a um serviço de atualização da intranet usando a configuração de política "Especificar intranet atualização local do serviço Microsoft".

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|O comportamento padrão para recuperar informações do Windows Server Update Services público persiste.|
|**Enabled**|Especifica que computadores não recuperar informações do Windows Server Update Services público.|
|**Desabilitado**|O comportamento padrão para recuperar informações do Windows Server Update Services público persiste.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_comp8"></a>Não exibir a opção "Instalar atualizações e desligar" na caixa de diálogo de desligar o Windows
Especifica se o **instalar atualizações e desligar** opção é exibida na **desligar o Windows** caixa de diálogo.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que o **instalar atualizações e desligar** opção está disponível na **desligar o Windows** caixa de diálogo se houver atualizações disponíveis quando o usuário seleciona a opção de desligar para desligar o computador. Um administrador local pode alterar essa configuração usando a diretiva local.|
|**Enabled**|Especifica que **instalar atualizações e desligar** não será exibido como uma opção na **desligar o Windows** caixa de diálogo, mesmo se as atualizações estão disponíveis para instalação quando o usuário seleciona a opção de desligar a desligar o computador.|
|**Desabilitado**|Especifica que o **instalar atualizações e desligar** será a opção padrão no **desligar o Windows** caixa de diálogo se houver atualizações disponíveis para instalação no momento em que o usuário seleciona a desligar opção de desligar o computador.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_comp9"></a>Habilitar destino do lado do cliente
Especifica o nome do grupo de destino ou nomes que são configurados no console do WSUS que devem receber atualizações do WSUS.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Essa política se aplica somente quando este computador está configurado para dar suporte os nomes de grupo de destino especificado no WSUS. Se o nome do grupo de destino não existir no WSUS, ele será ignorado até que ele seja criado. Se a configuração de política "Especificar intranet local do serviço do Microsoft update" está desabilitada ou não configurada, essa política não terá efeito.

> [!NOTE]
> Não há suporte para essa política RT Windows. A habilitação dessa política não terá nenhum efeito em computadores que executam RT Windows.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que nenhuma informação de grupo de destino é enviada para o WSUS. Um administrador local pode alterar essa configuração por meio de uma diretiva local.|
|**Enabled**|Especifica que as informações do grupo de destino especificado são enviadas para o WSUS, que utiliza para determinar quais atualizações devem ser implantadas no computador. Se o WSUS dá suporte a vários grupos de destino, você pode usar essa política para especificar vários nomes de grupo, separados por ponto e vírgula, se você tiver adicionado os nomes de grupo de destino na lista do grupo de computadores no WSUS. Caso contrário, um único grupo deverá ser especificado.|
|**Desabilitado**|Especifica que nenhuma informação de grupo de destino é enviada para o WSUS.|

**Opções:** Use este espaço para especificar um ou mais nomes de grupo de destino.

#### <a name="BKMK_comp10"></a>Habilitar o gerenciamento de energia do Windows Update para automaticamente ativar o computador para instalar atualizações agendadas
Especifica se o Windows Update usará os recursos de gerenciamento de energia do Windows ou as opções de energia para ativar o computador da hibernação automaticamente se há atualizações agendadas para instalação.

O computador será ativado automaticamente somente se o Windows Update está configurado para instalar atualizações automaticamente. Se o computador está em modo de hibernação quando ocorre o tempo de instalação agendada e há as atualizações sejam aplicadas, o Windows Update usará os recursos de gerenciamento de energia do Windows ou as opções de energia para ativar automaticamente o computador para instalar as atualizações. Atualizar do Windows também despertar o computador e instalar uma atualização se ocorrer um prazo de instalação.

O computador não será ativado se há atualizações a serem instalados. Se o computador estiver com bateria, quando o Windows Update o ativar, ele não instalará as atualizações e o computador retornará automaticamente no modo de hibernação em dois minutos.

|Suporte para:|Excluindo:|
|---------|-------|
|começando com o Windows Vista e Windows Server 2008 (Windows 7), sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Atualização do Windows não ativar o computador da hibernação para instalar atualizações. Um administrador local pode alterar essa configuração por meio de uma diretiva local.|
|**Enabled**|Atualização do Windows desperta o computador da hibernação para instalar as atualizações sob as condições listadas anteriormente.|
|**Desabilitado**|Atualização do Windows não ativar o computador da hibernação para instalar atualizações.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_comp11"></a>Não reinicializar automaticamente com usuários conectados para automática agendada de atualizações de instalações
Especifica que, para concluir uma instalação agendada, as atualizações automáticas irá esperar para o computador ser reiniciado por qualquer usuário que está conectado, em vez de causar o computador seja reiniciado automaticamente.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa política se aplica somente quando as atualizações automáticas está configurada para executar a instalação agendada de atualizações. Se a configuração de política "Configurar atualizações automáticas" está desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que as atualizações automáticas notificará o usuário que o computador será reiniciado automaticamente em cinco minutos para concluir a instalação.|
|**Enabled**|Algumas atualizações requerem que o computador ser reiniciado antes que as atualizações tenham efeito. Se o status é definido como habilitado, as atualizações automáticas não reiniciarão um computador automaticamente durante uma instalação agendada se um usuário estiver conectado ao computador. Em vez disso, as atualizações automáticas notificará o usuário para reiniciar o computador.|
|**Desabilitado**|Especifica que as atualizações automáticas notificará o usuário que o computador será reiniciado automaticamente em cinco minutos para concluir a instalação.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_comp12"></a>Solicitar reinicialização com instalações agendadas novamente
Especifica a quantidade de tempo para atualizações automáticas de espera antes de solicitar novamente com uma reinicialização agendada.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Essa política se aplica somente quando as atualizações automáticas está configurada para executar a instalação agendada de atualizações. Se a configuração de política "Configurar atualizações automáticas" está desabilitada, essa política não terá efeito.

> [!NOTE]
> Essa política não tem nenhum efeito em computadores que executam RT Windows.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Uma reinicialização agendada ocorre dez minutos depois que o prompt para reiniciar a mensagem é descartada. Um administrador local pode alterar essa configuração por meio de uma diretiva local.|
|**Enabled**|Especifica que, depois que a solicitação anterior para reinicialização foi adiada, uma reinicialização agendada ocorrerá após o número especificado de ter decorrido de minutos.|
|**Desabilitado**|Uma reinicialização agendada ocorre dez minutos depois que o prompt para reiniciar a mensagem é descartada.|

**Opções:** Quando habilitado, você pode usar essa opção de configuração para especificar (em minutos) a duração de tempo que decorrerá antes que os usuários serão solicitados novamente sobre uma reinicialização agendada.

#### <a name="BKMK_comp13"></a>Reagendar instalação agendada de atualizações automáticas
Especifica a quantidade de tempo para atualizações automáticas devem esperar após uma inicialização do computador, antes de prosseguir com uma instalação agendada que anteriormente foi perdida.

Se o status é definido como **não configurado**, uma instalação agendada perdida ocorrerá iniciada de um minuto após o computador está próximo.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa política se aplica somente quando as atualizações automáticas está configurada para executar a instalação agendada de atualizações. Se a configuração de política "Configurar atualizações automáticas" está desabilitada, essa política não terá efeito.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que uma instalação agendada perdida ocorrerá iniciada de um minuto após o computador está próximo.|
|**Enabled**|Especifica que uma instalação agendada que não tenha ocorrido anteriormente ocorrerá o número especificado de minutos depois que o computador é iniciado.|
|**Desabilitado**|Especifica que uma instalação agendada perdida ocorrerá na próxima instalação agendada.|

**Opções:** Quando essa configuração de política está habilitada, você pode usá-lo para especificar um número de minutos depois que o computador é iniciado, o que uma instalação agendada que não coloque anteriormente ocorrerá.

#### <a name="BKMK_comp14"></a>Especifique o local de serviço de atualização na intranet da Microsoft
Especifica um servidor de intranet para hospedar atualizações do Microsoft Update. Em seguida, você pode usar o WSUS para atualizar automaticamente os computadores em sua rede.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Essa configuração permite que você especificar um servidor do WSUS na sua rede que funcionará como um serviço de atualização interna. Em vez de usar o Microsoft Updates na Internet, os clientes do WSUS irá procurar esse serviço para atualizações que se aplicam.

Para usar essa configuração, você deve definir dois valores de nome de servidor: o servidor do qual o cliente detecta e baixa as atualizações e o servidor ao qual as estações de trabalho atualizadas carregam estatísticas. Os valores não precisam ser diferentes se ambos os serviços são configurados no mesmo servidor.

> [!NOTE]
> Se a configuração de política "Configurar atualizações automáticas" está desabilitada, essa política não terá efeito.

> [!NOTE]
> Não há suporte para essa política RT Windows. A habilitação dessa política não terá nenhum efeito em computadores que executam RT Windows.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Se as atualizações automáticas não está desabilitada por política ou preferência do usuário, essa diretiva especifica que os clientes se conectam diretamente ao site Windows Update na Internet.|
|**Enabled**|Especifica que o cliente se conecta ao servidor do WSUS especificado, em vez do Windows Update, para procurar e baixar atualizações. Habilitar essa configuração significa que usuários finais na sua organização não precisa passar por um firewall para obter atualizações e oferece a oportunidade de testar as atualizações antes de implantá-los.|
|**Desabilitado**|Se as atualizações automáticas não está desabilitada por política ou preferência do usuário, essa diretiva especifica que os clientes se conectam diretamente ao site Windows Update na Internet.|

**Opções:** Quando essa configuração de política está habilitada, você deve especificar o serviço de atualização da intranet que os clientes do WSUS usará ao detectar atualizações e o servidor de estatísticas de Internet para o qual atualizado os clientes do WSUS carregará as estatísticas. Valores de exemplo:

|Opção de configuração:|Valor de exemplo:|
|----------|---------|
|Definir o serviço de atualização da intranet para detectar atualizações|http://wsus01:8530|
|Definir o servidor de estatísticas da intranet|http://IntranetUpd01|

#### <a name="BKMK_comp15"></a>Ative as atualizações recomendadas via atualizações automáticas
Especifica se as atualizações automáticas entregará importantes e atualizações recomendadas do WSUS.

|Suporte para:|Excluindo:|
|---------|-------|
|começando com o Windows Vista, sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que as atualizações automáticas continuarão a fornecer atualizações importantes se ele já estiver configurado para fazer isso.|
|**Enabled**|Especifica que as atualizações automáticas instalam as atualizações recomendadas e as atualizações importantes do WSUS.|
|**Desabilitado**|Especifica que as atualizações automáticas continuarão a fornecer atualizações importantes se ele já estiver configurado para fazer isso.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_comp16"></a>Ativar notificações de Software
Essa configuração de diretiva permite controlar se os usuários veem mensagens de notificação avançada detalhada sobre o software em destaque do serviço Microsoft Update. Mensagens de notificação avançada de transmitem o valor e promovem a instalação e o uso de software opcional. Essa configuração de política se destina para uso em ambientes gerenciados de forma flexível no qual você permite o acesso de usuário final para o serviço Microsoft Update.

Se você não estiver usando o serviço Microsoft Update, a configuração de política de "Notificações de Software" não tem nenhum efeito.

Se a configuração de política "Configurar atualizações automáticas" está desabilitada ou não está configurada, a configuração de política de "Notificações de Software" não tem nenhum efeito.

|Suporte para:|Excluindo:|
|---------|-------|
|começando com o Windows Server 2008 (Windows Vista) e o Windows 7, sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Por padrão, essa configuração de política está desabilitada.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Os usuários em computadores que executam o Windows 7 não são oferecidos as mensagens para aplicativos opcionais. Os usuários em computadores que executam o Windows Vista não são oferecidos as mensagens para aplicativos opcionais ou atualizações. Um administrador local pode alterar essa configuração usando o painel de controle ou uma diretiva local.|
|**Enabled**|Se você habilitar essa configuração de política, uma mensagem de notificação será exibida no computador do usuário quando em destaque software está disponível. O usuário pode clicar na notificação para abrir o Windows Update e obter mais informações sobre o software ou instalá-lo. O usuário também pode clicar **feche esta mensagem** ou **mostre-me mais tarde** para adiar a notificação, conforme apropriado.<br /><br />No Windows 7, essa configuração de política controlará apenas notificações detalhadas para aplicativos opcionais. No Windows Vista, essa configuração de política controla notificações detalhadas para aplicativos opcionais e atualizações.|
|**Desabilitado**|Especifica que os usuários que executam o Windows 7 não receberão mensagens de notificação detalhada para aplicativos opcionais, e os usuários que executam o Windows Vista não receberão mensagens de notificação detalhada para aplicativos opcionais ou atualizações opcionais.|

**Opções:** Não existem opções para essa configuração.

### <a name="BKMK_MtncScheduler"></a>Configuração do computador > configurações de política de Agendador de manutenção
Na configuração de configurar atualizações automáticas, você selecionou a opção **4 – Baixar automaticamente e agendar a instalação**, você pode especificar agendar configurações do Agendador de manutenção no GPMC para computadores que executam o Windows 8 e RT Windows. Se você não selecionou a opção 4 na configuração "Configurar atualizações automáticas", você precisa definir essas configurações para fins de atualizações automáticas. Configurações do Agendador de manutenção estão localizadas no caminho: *PolicyName* > **computer Configuration** > **políticas** > **modelos administrativos**  >  **Componentes do Windows** > **Agendador de manutenção**. A extensão do Agendador de manutenção da diretiva de grupo contém as seguintes configurações:

-   [Limite de ativação de manutenção automática](#BKMK_comp5a)

-   [Atraso aleatório de manutenção automática](#BKMK_comp5b)

-   [Política de ativação automática](#BKMK_comp5c)

#### <a name="BKMK_comp5a"></a>Limite de ativação de manutenção automática
Essa política permite que você definir a configuração de "Limite de ativação de manutenção automática".

O limite de ativação de manutenção é horário agendado diariamente no qual inicia a manutenção automática.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa configuração é relacionada à opção 4 na **configurar atualizações automáticas**. Se você não selecionou a opção 4 na **configurar atualizações automáticas**, não é necessário definir essa configuração.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Se essa configuração de política não estiver configurada, o diário agendado conforme especificado nos computadores cliente na **Central de ações** > **manutenção automática** painel de controle serão aplicadas.|
|**Enabled**|Habilitar essa configuração de política substitui qualquer padrão ou modificado as configurações definidas em computadores cliente na **painel de controle** > **Central de ações**  >   **Manutenção automática** (ou em algumas versões do cliente, **manutenção**).|
|**Desabilitado**|Se você definir essa configuração de política como **desabilitados**, a hora agendada diária conforme especificado na **Central de ações** > **manutenção automática**, no controle Painel será aplicada.|

#### <a name="BKMK_comp5b"></a>Atraso aleatório de manutenção automática
Essa configuração de política permite que você configure o atraso aleatório na manutenção automática ativação.

O atraso aleatório na manutenção é a quantidade de tempo até que a manutenção automática atrasará a partir de seu limite de ativação. Essa configuração é útil para as máquinas virtuais em que a manutenção aleatória pode ser um requisito de desempenho.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa configuração é relacionada à opção 4 na **configurar atualizações automáticas**. Se você não selecionou a opção 4 na **configurar atualizações automáticas**, não é necessário definir essa configuração.

Por padrão, quando habilitado, o atraso aleatório na manutenção regular é definido como **PT4H**.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Um atraso aleatório de quatro horas é aplicado a **automática**.|
|**Enabled**|Manutenção automática atrasará a partir de seu limite de ativação por até o período de tempo especificado.|
|**Desabilitado**|Nenhum atraso aleatório é aplicado a manutenção automática.|

#### <a name="BKMK_comp5c"></a>Política de ativação automática
Essa configuração de política permite que você configure a política de ativação de manutenção automática.

A política de ativação de manutenção Especifica se a manutenção automática faça uma solicitação de ativação para o computador operacional para a manutenção agendada diariamente.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Se o operando do computador power wake política está desabilitada explicitamente, essa configuração não tem nenhum efeito.

> [!NOTE]
> Essa configuração é relacionada à opção 4 na **configurar atualizações automáticas**. Se você não selecionou a opção 4 na **configurar atualizações automáticas**, não é necessário definir essa configuração.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Se você não configurar essa configuração de política, o wake-up definindo como especificado na **Central de ações** > **manutenção automática** painel de controle serão aplicadas.|
|**Enabled**|Se você habilitar essa configuração de política, a manutenção automática tenta definir uma política de ativação do sistema operacional e fazer uma solicitação de ativação para o horário agendado diariamente, se necessário.|
|**Desabilitado**|Se você desabilitar essa configuração de política, o wake-up definindo como especificado na **Central de ações** > **manutenção automática** painel de controle serão aplicadas.|

### <a name="BKMK_UserPol"></a>Configuração do usuário > configurações de política de atualização do Windows
Esta seção fornece detalhes sobre as configurações de política com base no usuário a seguir:

-   [Não exibir a opção "Instalar atualizações e desligar" na caixa de diálogo de desligar o Windows](#BKMK_Client1)

-   [Ajustar a opção padrão para "Instalar atualizações e desligar" na caixa de diálogo de desligar o Windows](#BKMK_Client2)

-   [remover o acesso para usar todos os recursos de atualização do Windows](#BKMK_Client3)

No GPMC, as configurações do usuário para atualizações automáticas do computador estão localizadas no caminho: *PolicyName* > **configuração do usuário** > **políticas** > **modelos administrativos**  >  **Componentes do Windows** > **Windows Update**. As configurações são listadas na mesma ordem que aparecem na configuração do computador e extensões de configuração do usuário na diretiva de grupo, quando o **configurações** guia da política de atualização do Windows é selecionada para classificar as configurações em ordem alfabética.

> [!NOTE]
> Por padrão, a menos que indicado o contrário, essas configurações não são configuradas.

> [!TIP]
> para cada uma dessas configurações, você pode usar as etapas a seguir para habilitar, desabilitar ou navegar entre as configurações:

#### <a name="BKMK_Client1"></a>Não exibir a opção de instalar atualizações e desligar na caixa de diálogo de desligar o Windows
Especifica se o **instalar atualizações e desligar** opção é exibida na **desligar o Windows** caixa de diálogo.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica que o **instalar atualizações e desligar** opção aparecerá na **desligar o Windows** caixa de diálogo se houver atualizações disponíveis quando o usuário seleciona a opção de desligar para desligar o computador.|
|**Enabled**|Se você habilitar essa configuração de política **instalar atualizações e desligar** não será exibido como uma opção na **desligar o Windows** caixa de diálogo, mesmo se as atualizações estão disponíveis para instalação quando o usuário seleciona o Desligar a opção para desligar o computador.|
|**Desabilitado**|Especifica que o **instalar atualizações e desligar** opção aparecerá na **desligar o Windows** caixa de diálogo se houver atualizações disponíveis quando o usuário seleciona a opção de desligar para desligar o computador.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_Client2"></a>Ajustar a opção padrão para "Instalar atualizações e desligar" na caixa de diálogo de desligar o Windows
Especifica se o **instalar atualizações e desligar** opção é permitida como a opção padrão no **desligar o Windows** caixa de diálogo.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Essa configuração de política não tem impacto se o *PolicyName* > **configuração do usuário** > **políticas**  >   **Modelos administrativos** > **componentes do Windows** > **Windows Update** > **não exibir " Opção de instalar atualizações e desligar"na caixa de diálogo de desligar o Windows** está habilitado.

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Especifica se o **instalar atualizações e desligar** será a opção padrão no **desligar o Windows** caixa de diálogo se houver atualizações disponíveis para instalação no momento em que o usuário seleciona o desligamento Para baixo de opção para desligar o computador.|
|**Enabled**|Especifica se última opção de desligamento do usuário (por exemplo, ou hibernação reinicialização) é a opção padrão na **desligar o Windows** caixa de diálogo, independentemente se o **opçãodeinstalaratualizaçõesedesligar** está disponível na **o que você deseja com o computador faça?** menu.|
|**Desabilitado**|Especifica se o **instalar atualizações e desligar** será a opção padrão no **desligar o Windows** caixa de diálogo se houver atualizações disponíveis para instalação no momento em que o usuário seleciona o desligamento Para baixo de opção para desligar o computador.|

**Opções:** Não existem opções para essa configuração.

#### <a name="BKMK_Client3"></a>Remover o acesso para usar todos os recursos de atualização do Windows
Essa configuração permite que você remover o acesso de cliente do WSUS ao Windows Update.

|Suporte para:|Excluindo:|
|---------|-------|
|Sistemas operacionais Windows que são ainda dentro seu [ciclo de vida de suporte de produtos Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**Estado da política de configuração**|**Comportamento**|
|**Não configurado**|Os usuários são capazes de se conectar ao site Windows Update.|
|**Enabled**|**IMPORTANTE:** se permitem, todos os recursos de atualização do Windows são removidos. Isso inclui o bloqueio de acesso para o site Windows Update em https://windowsupdate.microsoft.com, do hiperlink de atualização do Windows na tela inicial do menu ou de início e também do **ferramentas** menu no Internet Explorer. A atualização automática do Windows também é desabilitada; o usuário não será notificado sobre nem receber atualizações importantes do Windows Update. Essa configuração também impede que o Gerenciador de dispositivos instale automaticamente atualizações de driver do site Windows Update.<br /><br />Quando habilitado, você pode configurar uma das seguintes opções de notificação:<br /><br />-   **0 – não mostrar todas as notificações**<br />    Essa configuração irá remover todo o acesso aos recursos do Windows Update, e nenhuma notificação será exibida.<br />-   **1 - Mostrar reinicialização necessária de notificações**<br />    Essa configuração mostrará as notificações sobre reinicializações são necessárias para concluir uma instalação. **Observação:** Em computadores executando o Windows 8 e Windows RT, se essa política está habilitada, apenas notificações relacionadas à reinicialização e a incapacidade de detectar atualizações serão mostrados. Não há suporte para as opções de notificação. As notificações na tela de entrada são sempre exibidas.|
|**Desabilitado**|Os usuários são capazes de se conectar ao site Windows Update.|

**Opções:** Ver **Enabled** na tabela para essa configuração.

## <a name="BKMK_Supplemental"></a>Informações complementares
Esta seção fornece informações de adição sobre o uso de abrindo e salvando as configurações do WSUS em diretivas de grupo e as definições para termos usados neste guia. Para administradores familiarizados com as versões anteriores do WSUS (WSUS 3.2 e versões anteriores), há uma tabela que resume as diferenças entre as versões do WSUS.

### <a name="BKMK_OpenGPO"></a>Acessando as configurações de atualização do Windows na política de grupo
O procedimento a seguir descreve como abrir o GPMC no controlador de domínio. O procedimento, em seguida, descreve como abrir um existente do nível do domínio grupo de política de GPO (objeto) para editar, ou criar um novo GPO de nível de domínio e abri-lo para edição.

> [!NOTE]
> Você deve ser um membro do **Admins. do domínio** agrupar ou equivalente, para executar este procedimento.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir ou adicionar e abrir um objeto de diretiva de grupo

1.  No controlador de domínio, vá para **Gerenciador de servidores**, > **ferramentas**, > **Group Policy Management**. Abre o Console de gerenciamento de diretiva de grupo.

2.  No painel esquerdo, expanda sua floresta. Por exemplo, clique duas vezes **floresta: exemplo.com**.

3.  No painel esquerdo, clique duas vezes **domínios**e, em seguida, clique duas vezes no domínio para o qual você deseja gerenciar um objeto de diretiva de grupo. Por exemplo, clique duas vezes **exemplo.com**.

4.  Siga um destes procedimentos:

    -  **Para abrir um GPO de nível de domínio existente para edição**, clique duas vezes em um domínio que contém o objeto de diretiva de grupo que você deseja gerenciar, a política de domínio que você deseja gerenciar e, em seguida, clique com o botão direito **editar**. Abre o editor de gerenciamento de diretiva de grupo (GPME).

    -  **Para criar um novo objeto de diretiva de grupo e abrir para edição**:
        1.  O domínio para o qual você deseja criar um novo objeto de diretiva de grupo e, em seguida, clique com o botão direito **criar um GPO neste domínio e vinculá-lo aqui**.

        2.  Na **novo GPO**, na **nome**, digite um nome para o novo objeto de diretiva de grupo e, em seguida, clique em **Okey**.

        3.  Seu novo objeto de diretiva de grupo com o botão direito e, em seguida, clique em **editar**. GPME é aberto.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Para abrir as extensões do Windows Update ou de um agendador de manutenção da diretiva de grupo

1.  No editor de gerenciamento de diretiva de grupo, siga um destes procedimentos:

    -   **Abra a configuração do computador > extensão do Windows Update da diretiva de grupo**. Navegue até: *PolicyName* > **computer Configuration** > **políticas** / **modelos administrativos**  >  **Componentes do Windows** > **Windows Update**.

    -   **Abra a configuração de usuário > extensão do Windows Update da diretiva de grupo**. Navegue até: *PolicyName* > **configuração do usuário** > **políticas** > **modelos administrativos**  >  **Componentes do Windows** > **Windows Update**.

    -   **Abra a configuração do computador > extensão do Agendador de manutenção da diretiva de grupo**. No GPOE, navegue até *PolicyName* > **computer Configuration** > **políticas** > **administrativo Modelos** > **componentes do Windows** > **Agendador de manutenção**.

Para obter mais informações sobre a política de grupo, consulte [visão geral da política de grupo](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Depois de abrir a extensão da diretiva de grupo desejado, você pode usar as etapas a seguir para habilitar, desabilitar ou navegar entre as configurações:

##### <a name="to-configure-group-policy-settings"></a>Para definir configurações da Política de grupo

1.  Na *ExtensionOfGroupPolicy*, clique duas vezes em configuração você deseja exibir ou modificar.

2.  Para definir a configuração, siga um destes procedimentos:

    -   Para manter o padrão não for especificado o estado de configuração, selecione **não configurado**.

    -   Para habilitar a configuração, selecione **Enabled**.

    -   Para desabilitar a configuração, selecione **desabilitado**.

3.  Na **opções**, se nenhuma opção estiver listada, mantenha os valores padrão ou modificá-los conforme necessário.

4.  Siga um destes procedimentos:

    -   Para salvar suas alterações e prosseguir para a próxima configuração, clique em **Apply**e, em seguida, clique em **próxima configuração**.

    -   Para salvar suas alterações e fechar a caixa de diálogo, clique em **Okey**.

    -   Para descartar todas as alterações não salvas e fechar a caixa de diálogo, clique em **Cancelar**.

### <a name="BKMK_changes"></a>Alterações para o WSUS relevante para este guia
A tabela a seguir resume as principais diferenças entre as versões atuais e anteriores do WSUS que são relevante para este guia.

|Versões do Windows Server e o WSUS|Descrição|
|------------------|--------|
| Windows Server 2012 R2 com o WSUS 6.0 e versões posteriores|a partir do Windows Server 2012, a função de servidor do WSUS é integrada com o sistema operacional e as configurações associadas de diretiva de grupo para clientes do WSUS são, por padrão, incluído na diretiva de grupo.|
| Windows Server 2008 (e versões anteriores do Windows Server) com o WSUS 3.2 e anteriores|No Windows Server 2008 (e versões anteriores do Windows Server) usando o WSUS versões 3.2 (e anteriores), as configurações de diretiva de grupo que controlam os clientes do WSUS não são incluídas nesses sistemas operacionais de servidor do Windows. As configurações de política estão no modelo administrativo do WSUS, **wuau**. Nessas versões de servidor, o modelo administrativo do WSUS deve primeiro ser adicionado para o grupo de política de gerenciamento GPMC (Console) antes que as configurações de cliente do WSUS podem ser configuradas.|

### <a name="BKMK_Terms"></a>Termos e definições
A seguir está uma lista de termos usados neste guia.

|Termo|Definição|
|----|-------|
|Atualizações Automáticas|**Um serviço executado em computadores Windows** (atualizações automáticas): Refere-se para o componente de computador cliente incorporado do Microsoft Windows Vista, Windows Server 2003, Windows XP e Windows 2000 com SP3 sistemas de operacionais para obter atualizações do Microsoft Update ou Windows Update.<br /><br />**Referência casual** (atualizações automáticas): O termo usado para descrever quando o Windows Update Agent agenda e automaticamente baixa as atualizações.|
|servidor autônomo|Use para se referir a um servidor downstream do Windows Server Update Services (WSUS) no qual os administradores podem gerenciar os componentes do WSUS.|
|servidor downstream|Use para se referir a um servidor do Windows Server Update Services (WSUS) que obtém as atualizações de outro servidor do WSUS, em vez do Microsoft Update ou Windows Update.|
|Extensão da diretiva de grupo (e: extensão da diretiva de grupo|Uma coleção de configurações na diretiva de grupo que são usados para controlar como os usuários e computadores (aos quais as políticas se aplicam) podem configurar e usar vários recursos e serviços do Windows. Os administradores podem usar o WSUS com diretiva de grupo para configuração do lado do cliente do cliente de atualizações automáticas, para ajudar a garantir que os usuários finais não é possível desabilitar ou contornar políticas de atualização corporativa.<br /><br />O WSUS não requer o uso da diretiva de grupo ou do Active Directory. Configuração do cliente também pode ser aplicada usando a diretiva de grupo local ou modificando o registro do Windows.|
|serviço de atualização interna|Uma referência casual em uma infraestrutura de rede que usa um ou mais servidores do WSUS para distribuir atualizações.|
|servidor de réplica|Use para se referir a um servidor Windows Server Update Services (WSUS) downstream que espelha as aprovações e configurações no servidor upstream para o qual ele está conectado. Você não pode gerenciar o WSUS em um servidor de réplica.|
|Microsoft Update|**Um site de download da Microsoft baseado na Internet:** Um site da Internet da Microsoft que armazena e distribui as atualizações para computadores Windows (drivers de dispositivo), os sistemas operacionais Windows e outros produtos de software da Microsoft.|
|Software Update Services (SUS)|SUS era o produto do predecessor para o Windows Server Update Services (WSUS).|
|atualizações|Qualquer um de uma coleção de revisões de software, hotfixes, service packs, pacotes de recursos e drivers de dispositivo que podem ser instalados em um computador para estender a funcionalidade ou melhorar o desempenho e segurança.|
|arquivos de atualização|Os arquivos necessários para instalar uma atualização em um computador.|
|informações de atualização (também conhecido como metadados de atualização)|As informações sobre uma atualização, em vez dos arquivos binários de atualização em um pacote de atualização. Por exemplo, os metadados fornecem informações para as propriedades de uma atualização, permitindo que você descubra do que a atualização é útil. Os metadados também incluem o Microsoft Software License Terms. O pacote de metadados baixado para uma atualização é normalmente muito menor do que o pacote de arquivos de atualização real.|
|origem de atualização|O local para o qual um servidor Windows Server Update Services (WSUS) sincroniza para obter arquivos de atualização. Esse local pode ser o Microsoft Update ou um servidor WSUS upstream.|
|servidor upstream|Um servidor Windows Server Update Services (WSUS) que fornece arquivos de atualização para outro servidor do WSUS, que por sua vez é conhecido como um servidor downstream.|
|Windows Server Update Services (WSUS)|Um programa de função de servidor que é executado em um ou mais computadores Windows Server em uma rede corporativa. Uma infra-estrutura do WSUS permite que você gerencie atualizações para os computadores na sua rede para instalar.<br /><br />Você pode usar o WSUS para aprovar ou recusar atualizações antes do lançamento, para forçar atualizações a serem instaladas em uma determinada data, e obter relatórios abrangentes sobre o que atualiza cada computador na sua rede requer. Você pode configurar o WSUS para aprovar determinadas classes de atualizações automaticamente (atualizações críticas, atualizações de segurança, os service packs, drivers, etc.). WSUS também permite que você aprovar as atualizações para "detecção" apenas, para que você possa ver quais computadores exigirá uma determinada atualização sem a necessidade de instalar as atualizações.<br /><br />Em uma implementação do WSUS, pelo menos um servidor do WSUS na rede deve ser capaz de se conectar ao Microsoft Update para obter atualizações disponíveis. Com base na configuração e segurança de rede, o administrador pode determinar quantos outros servidores conectam diretamente ao Microsoft Update.<br /><br />Você pode configurar um servidor WSUS para obter atualizações pela Internet de tais locais como:<br /><br />-o público do Microsoft Update<br />-a atualização pública do Windows<br />-Microsoft Store|
|Windows Update|**Um site de download da Microsoft baseado na Internet:** Um site da Internet da Microsoft que armazena e distribui as atualizações para computadores Windows (drivers de dispositivo) e os sistemas operacionais Windows.<br /><br />**serviço do computador:** O nome do serviço Windows Update que é executado nos computadores. Atualização do Windows detecta, baixa e instala as atualizações nos computadores Windows.<br /><br />Dependendo do computador e as configurações de política, o Windows Update Agent pode baixar as atualizações de:<br /><br />-Microsoft Update<br />-   Windows Update<br />-Microsoft Store<br />-Um serviço de atualização da Internet (rede) (WSUS)<br /><br />computadores que não são gerenciados em um ambiente com base no WSUS normalmente usará o Windows Update para se conectar diretamente - pela Internet - ao Windows Update, Microsoft Update ou Microsoft Store para obter atualizações.|
|Cliente WSUS|Um computador que recebe atualizações de um serviço de atualização do WSUS da intranet.<br /><br />No caso de configurações de diretiva de grupo que controlam a interação do usuário final com as atualizações automáticas: um usuário de um computador em um ambiente do WSUS.|
