---
title: Configuração recomendada para desktops da VDI
description: Recomendado as configurações e definições para minimizar a sobrecarga para desktops com Windows 10 1607 (10.0.1393) usadas como imagens VDI
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/18/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a44dc9f-c221-4bf7-89c3-fb4c86a90f8c
author: jaimeo
manager: dougkim
ms.openlocfilehash: 24704373dedf6a44809b83f3df17bd073cee2bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818377"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Configurações recomendadas para desktops da VDI

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Virtualização de área de trabalho da Microsoft detecta automaticamente as configurações do dispositivo e as condições da rede para que os usuários de funcionamento mais cedo, permitindo a instalação instantânea de áreas de trabalho e aplicativos corporativos, e ele equipa TI forneça acesso para herdado aplicativos durante a migração para o Windows 10.

Embora o sistema operacional Windows 10 é muito bem ajustado prontos, há oportunidades para você aperfeiçoar ainda mais especificamente para o ambiente corporativo da Microsoft Virtual Desktop Infrastructure (VDI). No ambiente de VDI, muitos serviços em segundo plano e tarefas estão desabilitadas desde o início.

Este tópico não é um plano gráfico, mas em vez disso, uma guia ou o ponto de partida. Algumas recomendações podem desabilitar a funcionalidade que você prefere usar, portanto você deve considerar o custo em comparação com o benefício de ajustar qualquer configuração específica em seu cenário.

Estas instruções e as configurações recomendadas são relevantes para o Windows 10 1607 (versão 10.0.1393).

> [!NOTE]  
> Todas as configurações não especificamente mencionadas neste tópico podem ser deixadas em seus valores padrão (ou definir por seus requisitos e políticas) sem impacto considerável sobre a funcionalidade VDI.

Quando você cria uma imagem para basear a implantação de VDI, certifique-se de usar o **Branch atual**. Para obter mais informações sobre o Branch atual, consulte [informações de versão do Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Criando a imagem do Windows 10
A primeira etapa é instalar uma imagem de referência do Windows 10 1607 (versão 10.0.1393) em uma máquina virtual ou física. Instalando a uma máquina virtual é fácil e permite que você salve as versões do arquivo virtual (VHD) no disco rígido, caso você deseje reverter para uma versão anterior.

Durante a instalação, você pode escolher **das configurações expressas** ou **personalizar**. As configurações oferecido durante o **personalizar** opção são ajustáveis por meio da política de grupo, então o método de instalação do sistema operacional de base não é tão importante.


Se você escolheu **personalizar**, você pode ajustar essas configurações durante a instalação:

## <a name="in-customize-settings"></a>Em "Personalizar as configurações"

Você também pode ajustá-las após a instalação com o Editor de diretiva de grupo; Consulte a seção "Configurações de diretiva de grupo" deste tópico.

|Configuração|Valor padrão|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|**Personalização**| | |
|Personalize sua fala, digitando e escrita à tinta de entrada, enviando seus dados de entrada para a Microsoft.|    Ativado| Desativado|
|Envie digitando e dados à Microsoft para aprimorar a plataforma de reconhecimento e sugestão de escrita.|  Ativado| Desativado|
|Permitir que aplicativos usem sua ID de anúncio para experiência entre aplicativos.|  Ativado| Desativado|
|Deixe o Skype (se instalado) o ajudam a conectar-se com seus amigos no catálogo de endereços e verificar seu número de celular. Tarifas SMS e os dados podem ser aplicadas.|    Ativado| Desativado|
|**Local**| | |
|Ativar o localizar meu dispositivo e let Windows e aplicativos de seu local, incluindo o histórico de localização de solicitação| Ativado| Desativado|
|Conectividade e o relatório de erros| | |
|Conectar automaticamente a hotspots abertos sugeridos. Nem todas as redes são seguras.|    Ativado| Desativado|
|Conecte-se automaticamente para abrir os pontos de acesso temporariamente para ver se paga pelos serviços de rede estão disponíveis.| Ativado| Desativado|
|Envie dados de uso e diagnóstico completo à Microsoft. A desativação dessa opção envia somente dados básicos.| Ativado| Desativado|
|**Navegador, proteção e atualização**| | |
|Use SmartScreen online services para ajudar a proteger contra conteúdo mal-intencionado e baixa em sites carregados por navegadores de Windows e aplicativos da Store|    Ativado| No (se não houver nenhum acesso à Internet, em seguida, definida como Off.)
|Usar previsão de página para melhorar a leitura, acelerar a navegação e tornar sua experiência geral melhor em navegadores Windows. Seus dados de navegação serão enviados à Microsoft.| Ativado| Desativado|
|Obtenha atualizações do e enviar atualizações para outros computadores na Internet para acelerar o aplicativo e downloads de atualização do Windows|   Ativado| Desativado|

Depois que a instalação for concluída, você pode continuar ajustando as configurações a partir **configurações do Windows**.

## <a name="in-windows-settings"></a>Nas configurações do Windows
Para acessar as configurações do Windows, clique em **inicie** (o ícone de Windows na barra de tarefas) e, em seguida, clique no **configurações** ícone (em forma de uma engrenagem).

### <a name="in-the-system-area-of-windows-settings"></a>Na área de "Sistema" das configurações do Windows
Na área de configurações do Windows, clicando na **sistema** ícone fornece acesso a uma série de configurações relacionadas ao sistema. Nem todos eles precisam de ajustes uso ideal de VDI – essas configurações são os mais importantes:

#### <a name="apps-and-features"></a>Aplicativos e recursos

Para remover um aplicativo, assim, a exclusão do sua imagem VDI, clique no aplicativo e, em seguida, clique em **desinstalação**. Se **desinstalação** estiver acinzentada, você não pode removê-lo por esse método; você poderá removê-lo com o Windows PowerShell, ou tente estas etapas:
1. Clique em **gerenciar recursos opcionais** (imediatamente abaixo de **aplicativos e recursos** título na mesma página).
2. Clique no recurso opcional e, em seguida, clique em **desinstalação**.

Recursos a serem considerados removendo (se houver) incluem o seguinte:
- **Contate o suporte**
- **Conteúdo de demonstração de varejo do inglês (Estados Unidos)**
- **Conteúdo de demonstração de varejo neutro**
- **Assistência rápida**

#### <a name="default-apps"></a>Aplicativos padrão

Essa área define o aplicativo a ser usado por padrão para determinadas funções genéricas, como email, navegação na web e mapas. Se você quiser que um aplicativo diferente a ser usado para uma função específica, clique na entrada atual e, em seguida, clique no aplicativo que você preferir a ser usado na imagem de VDI. Para um aplicativo que não são da Microsoft ser uma opção disponível, você deve instalar o aplicativo antes do ajuste dessa configuração.

#### <a name="notifications-and-actions"></a>Notificações e ações

Esses valores recomendados reduzirá as notificações e a atividade de rede em segundo plano em um ambiente de VDI:

|Configuração|Valor padrão|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|Obter notificações de aplicativos e outros remetentes| Ativado| Desativado|
|Mostra notificações na tela de bloqueio.|    Ativado| Desativado|
|Mostra chamadas VoIP, lembretes e alarmes na tela de bloqueio.|   Ativado| Desativado|
|Mostre dicas, truques e sugestões de como usar o Windows.|    Ativado| Desativado|


#### <a name="offline-maps"></a>Mapas offline

Essa configuração é aplicável somente se o aplicativo de mapas está instalado. Seu valor padrão é **na**; para o valor recomendado é de uso VDI **Off**. 

#### <a name="tablet-mode"></a>Modo tablet

|Configuração|Valor padrão|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|Quando eu entrar|    Usar o modo apropriado para o meu hardware|   Usar o modo de área de trabalho|
|Quando este dispositivo automaticamente alterna modo ativada ou desativada|    Sempre perguntar antes de alternar| Não me pergunte e não mudar|
|Ocultar ícones de aplicativos na barra de tarefas no modo tablet|  Ativado| Desativado|


### <a name="in-the-devices-area-of-windows-settings"></a>Na área de "Dispositivos" das configurações do Windows
Na área de configurações do Windows, clicando na **dispositivos** ícone fornece acesso a uma série de configurações relacionadas ao sistema. Nem todos eles precisam de ajustes uso ideal de VDI – essas configurações são os mais importantes:

#### <a name="autoplay"></a>Autoplay

|Configuração|Valor padrão|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|Usar a reprodução automática para todos os dispositivos e mídia|    Ativado| Desativado|
|Unidade removível:|Escolha um padrão|Nenhuma ação|
|Cartão de memória|Escolha um padrão|Nenhuma ação|

### <a name="in-the-personalization-area-of-windows-settings"></a>Na área "Personalização" das configurações do Windows
Na área de configurações do Windows, clicando na **personalização** ícone fornece acesso a uma série de configurações relacionadas ao sistema. Nem todos eles precisam de ajustes uso ideal de VDI – essas configurações são os mais importantes:

#### <a name="background"></a>Histórico
Às vezes, o plano de fundo preta padrão pode fazer com que usuários acha que o computador não está respondendo. Alterar a cor do plano de fundo pode ajudar a torná-lo mais clara. Para fazer isso, execute estas etapas:
1. No **plano de fundo** área, clique no menu suspenso.
2. Para alterar a cor do plano de fundo, clique em **cor sólida**e, em seguida, clique em qualquer uma das cores que não seja preto. Como alternativa, você poderia clicar **imagem** e, em seguida, selecione uma imagem para usar como plano de fundo.

#### <a name="start"></a>Início

|Configuração|Valor padrão|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|Ocasionalmente, Mostrar sugestões de início|    Ativado| Desativado|
|Mostrar mais usado aplicativos|Ativado|Desativado|
|Mostrar aplicativos adicionados recentemente|Ativado|Desativado|
|Mostrar itens abertos recentemente nas listas de atalhos no início ou a barra de tarefas|Ativado|Desativado|

#### <a name="taskbar"></a>Barra de tarefas
A configuração padrão é usar os botões da barra de tarefas grandes (ou seja, um valor de "Desativado" para **usar botões na barra de tarefas pequeno**). Essa configuração faz com que o item Cortana usar muitos da área da barra de tarefas. Para evitar isso, defina **usar botões na barra de tarefas pequeno** como "Ativado". Se você preferir que os itens da barra de tarefas fique maiores, mas preferem não ter Cortana, ocupando muito espaço, a barra de tarefas com o botão direito, aponte para **Cortana**e, no menu que sai, selecione **Hidden**.

### <a name="in-the-privacy-area-of-windows-settings"></a>Na área "Privacidade" das configurações do Windows
Na área de configurações do Windows, clicando na **privacidade** ícone fornece acesso a uma série de configurações relacionadas ao sistema. Nem todos eles precisam de ajustes uso ideal de VDI – essas configurações são os mais importantes:

#### <a name="general"></a>Geral
Algumas dessas configurações também são definidas na janela "Personalizar as configurações", discutida no início deste tópico.

|Configuração|Valor padrão|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|Permitir que o aplicativos usem minha ID de anúncio para experiências entre aplicativos (desativar essa configuração será redefinir a sua ID)|  Ativado| Desativado|
|Permitir que sites forneçam conteúdo local relevante acessando minha lista de idiomas|Ativado|Desativado|
|Permitir que aplicativos em Meus outros aplicativos aberto de dispositivos e continuar experiências neste dispositivo|Ativado|Desativado|

#### <a name="camera"></a>Câmera

O valor padrão para "Permitir que aplicativos usem minha câmera" está **na**; para o valor recomendado é de uso VDI **Off**.


#### <a name="microphone"></a>Microfone

O valor padrão para "Permitir que aplicativos usem o microfone" está **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="notifications"></a>Notificações

O valor padrão para "Permitir que aplicativos acessem meus notificações" está **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="contacts"></a>Contatos

O valor padrão para "Permitir que aplicativos acessem meus contatos" está **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="calendar"></a>Calendário

O valor padrão para "Permitir que aplicativos acessem o meu calendário" está **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="call-history"></a>Histórico de chamadas

O valor padrão para "Permitir que aplicativos acessem o histórico de chamadas" está **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="email"></a>Email

É o valor padrão para "Permitir que aplicativos acessem e enviar email" **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="messaging"></a>Sistema de mensagens

O valor padrão para "Permitir que aplicativos ler ou enviar mensagens (texto ou MMS)" está **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="radios"></a>Rádios

O valor padrão para "Permitir que aplicativos controle rádios" está **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="other-devices"></a>Outros dispositivos

É o valor padrão de "Permitem que seus aplicativos automaticamente compartilham e sincronizar informações com dispositivos sem fio que não emparelhem explicitamente com o seu PC, tablet ou telefone" **na**; para o valor recomendado é de uso VDI **Off**.

#### <a name="feedback-and-diagnostics"></a>Comentários e diagnóstico

O valor padrão para "Windows deverá solicitar meus comentários" está **automaticamente**; para o uso VDI, o valor recomendado é **nunca**.

#### <a name="background-apps"></a>Aplicativos de segundo plano
Os aplicativos listados têm um valor padrão de **em**, que permite que eles recebem informações, enviar notificações e atualizar-se se eles estão sendo usados ou não. Você deve desabilitar (definido como **desativar**) qualquer aplicativos que você não desejar em execução em segundo plano na imagem de VDI.

### <a name="update-and-security"></a>Atualização e segurança
#### <a name="windows-update"></a>Windows Update
No **atualizar as configurações** área, clique em **opções avançadas** ajustar essas configurações:

|Configuração|Valor padrão|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|Dê-me atualizações para outros produtos da Microsoft quando eu atualizar o Windows|    Desmarcada|    Selecionado|
|Adiar atualizações de recursos|Desmarcada|Selecionado|
|Usar minhas informações de entrada concluir automaticamente a configuração de meu dispositivo após uma atualização |Desmarcada|Depende da configuração específica do VDI|

Sobre o **opções avançadas** , clique em **escolha como as atualizações são entregues** para acessar a configuração de "Atualizações de mais de um lugar". O valor padrão é **na**; para o valor recomendado é de uso VDI **Off**.

## <a name="in-control-panel-and-other-system-utilities"></a>No painel de controle e outros utilitários do sistema

As configurações nesta seção são ajustáveis por navegando por meio do painel de controle ou abrindo o utilitário diretamente.

> [!NOTE]  
> Todas as configurações não especificamente mencionadas neste tópico podem ser deixadas em seus valores padrão (ou definir por seus requisitos e políticas) sem impacto considerável sobre a funcionalidade VDI.


### <a name="task-scheduler"></a>Agendador de Tarefas
A maneira mais rápida para abrir o Agendador de tarefas é pressionar o botão do Windows e o tipo *Agendador de tarefas* ou *taskschd*. Nos resultados que retornam, clique em **Agendador de tarefas** para abrir o utilitário. No Agendador de tarefas, expanda **biblioteca do Agendador de tarefas**, expanda **Microsoft**e, em seguida, expanda **Windows**. Agora você tem acesso à lista de coleções de tarefas. Para alterar o estado de cada tarefa agendada, clique duas vezes e, em seguida, clique em estado desejado (normalmente, **desabilitado** para uso VDI).

|Coleção de tarefas|Nome da tarefa|Estado padrão|Estado recomendado para uso VDI|  
|-------------------|-------------|----------|--------------|
|Programa de Aperfeiçoamento da Experiência do Usuário||||
||Consolidador|Enabled|Desabilitada|
||KernelCeipTask|Enabled|Desabilitada|
||UsbCeip|Enabled|Desabilitada|
|executa a desfragmentação||||
||ScheduledDefrag|Enabled|Desabilitada|
|Local padrão||||
||Notificações|Enabled|Desabilitada|
||WindowsActionDialog|Enabled|Desabilitada|
|Manutenção||||
||WinSAT|Enabled|Desabilitada|
|Mapas||||
||MapsToastTask|Enabled|Desabilitada|
||MapsUpdateTask|Enabled|Desabilitada|
|Contas de banda larga móveis||||
||Analisador de metadados MNO|Enabled|Desabilitada|
|Diagnóstico de eficiência de energia||||
||Analisar o sistema|Enabled|Desabilitada|
|Ambiente de recuperação||||
||VerifyWinRE|Enabled|Desabilitada|
|Demonstração de varejo||||
||CleanupOfflineContent|Enabled|Desabilitada|
|Shell||||
||FamilySafetyMonitor|Enabled|Desabilitada|
||FamilySafetyRefreshTask|Enabled|Desabilitada|
|Relatório de erros do Windows||||
||QueueReporting|Enabled|Desabilitada|
|Compartilhamento de mídia do Windows||||
||UpdateLibrary|Enabled|Desabilitada|

Clique em **Windows** novamente para recolhê-los, em seguida, clique em **XblGameSave**. Isso fornece acesso às tarefas **XBLGameSaveTask** e **XBLGameSaveTaskLogon**; ambos podem ser definidos como **desabilitado**.

### <a name="performance-monitor"></a>Monitor de Desempenho
A maneira mais rápida para abrir o Monitor de desempenho é pressionar o botão do Windows e o tipo *monitor de desempenho* ou *PerfMon*. Nos resultados que retornam, clique em **Monitor de desempenho**. No Monitor de desempenho, clique em **conjuntos de Coletores de dados** e, em seguida, clique duas vezes em **sessões de rastreamento de evento**. Clique com botão direito **WiFiSession**; se ele estiver no estado padrão do **executando**, em seguida, clique em **parar**.

Clique em **StartupEventTraceSessions**, em seguida, clique com botão direito **ReadyBoot**; se ele estiver em execução, clique em **parar**. Clique em **sessões de rastreamento de eventos**, clique com botão direito **ReadyBoot**e, em seguida, clique em **propriedades**. Na caixa de diálogo que é aberta, clique o **sessão de rastreamento** guia. Desmarque a **Enabled** caixa de seleção.

### <a name="services"></a>Serviços
A maneira mais rápida para gerenciar os serviços é pressionar o botão do Windows e o tipo *services*. Nos resultados que retornam, clique em **Services**. Os serviços a seguir são bons candidatos para desabilitar para uso em cenários VDI; No entanto, você talvez precise fazer alguns testes para verificar que eles não são necessários para suas finalidades. Para desabilitar um serviço, na **Services** snap-in, clique no nome do serviço e, em seguida, clique em **propriedades**. Sobre o **gerais** , clique no **tipo de inicialização** menu suspenso e, em seguida, clique em **desabilitado**. Clique em **OK**.

- BranchCache
- Otimização de Entrega
- Host de serviço de diagnóstico
- Serviço de ponto de acesso móvel do Windows
- Gerenciador de autenticação ao vivo do Xbox
- Salvar jogo do Xbox Live
- Serviço de rede ao vivo do Xbox

### <a name="file-explorer-options"></a>Opções do Gerenciador de arquivo
Enviar por push com o botão do Windows e o tipo *painel de controle*. Nos resultados que retornam, clique em **painel de controle**. No painel de controle, clique em **opções do Gerenciador de arquivo**. Na caixa de diálogo que é aberta, clique o **pesquisa** guia e, em seguida, no **ao pesquisar locais não indexados** área, desmarque a caixa de seleção **incluir diretórios do sistema**. Clique em **Okey** para salvar.

### <a name="flash-settings"></a>Configurações de Flash
Enviar por push com o botão do Windows e o tipo *painel de controle*. Nos resultados que retornam, clique em **painel de controle**. No painel de controle, clique em **Flash Player** para abrir o Gerenciador de configurações do Flash Player. Sobre o **armazenamento** , selecione o botão de opção **bloquear todos os sites armazenem informações neste computador**. Na caixa de diálogo que é aberta, clique em **Okey**.

Sobre o **câmera e microfone** guia o **câmera e microfone configurações** área, selecione o botão de opção para **bloquear todos os sites que usa a câmera e microfone**.

Sobre o **reprodução** guia da **assistido por par de rede** área, selecione o botão de opção para **bloquear todos os sites de usar assistido por par de rede**. Feche o Gerenciador de configurações do Flash Player.

### <a name="internet-options"></a>Opções da Internet
Enviar por push com o botão do Windows e o tipo *painel de controle*. Nos resultados que retornam, clique em **painel de controle**. No painel de controle, clique em **opções da Internet** para abrir as propriedades da Internet. No **Home page** área, insira a URL para o site da web que você deseja que os usuários para ver como a home page em navegadores. Isso pode ser um site da web para a sua empresa ou você pode defini-lo para uma página inicial em branco, inserindo *sobre: em branco*.

No **histórico de navegação** área, selecione a caixa de seleção **Excluir histórico de navegação ao sair**.

### <a name="power-options"></a>Opções de Energia
Enviar por push com o botão do Windows e o tipo *painel de controle*. Nos resultados que retornam, clique em **painel de controle**. No painel de controle, clique em **opções de energia** para abrir o painel de controle Opções de energia. No **escolher ou personalizar um plano de energia** área, clique na seta para baixo **Mostrar planos adicionais**e, em seguida, selecione o botão de opção para **alto desempenho**. Essa configuração terá um impacto mínimo no host de VDI.

### <a name="system"></a>Sistema
Enviar por push com o botão do Windows e o tipo *painel de controle*. Nos resultados que retornam, clique em **painel de controle**. No painel de controle, clique em **sistema** para abrir o painel de controle do sistema. No painel esquerdo, clique em **configurações avançadas do sistema**. Na caixa de diálogo que é aberta, clique o **avançado** guia. No **desempenho** área, clique no **configurações** botão, em seguida, na **efeitos visuais** guia na caixa de diálogo é aberta, selecione o **ajustar para melhor desempenho**  botão de opção. Clique em **Okey** para salvar e sair.

## <a name="group-policy-settings"></a>Configurações da Política de Grupo

Para editar as configurações de diretiva de grupo, pressione o botão do Windows e o tipo *política de grupo* ou *gpedit. msc*. Nos resultados que retornam, clique em **editar a política de grupo** para abrir o Editor de diretiva de Grupo Local.

> [!NOTE]  
> Todas as configurações não especificamente mencionadas neste tópico podem ser deixadas em seus valores padrão (ou definir por seus requisitos e políticas) sem impacto considerável sobre a funcionalidade VDI.

Sob **configuração do computador**, expanda **configurações do Windows**e, em seguida, expanda **configurações de segurança**. Clique em **políticas do Gerenciador de listas de rede**e, em seguida, clique duas vezes em **todas as redes**. Na caixa de diálogo que abre, o **local de rede** área, selecione o botão de opção para **usuário não pode alterar o local**. Clique o **Okey** botão para salvar.

Recolher **configurações do Windows**e, em seguida, expanda **modelos administrativos**. Clique ou expanda **rede**e, em seguida, ajustar a cada configuração da seguinte maneira clicando duas vezes, em seguida, selecionando o botão de opção para o valor indicado e o **Okey** botão:

|Área de configuração|Configuração|Valor recomendado para uso VDI|  
|-------------------|-------|----------|
|Serviço de Transferência Inteligente em Segundo Plano (BITS)|||
||Não permitir que o cliente de BITS usar o Cache de ramificação do Windows|Enabled|
||Não permitir que o computador atuar como um cliente de cache par do BITS|Enabled|
||Não permitir que o computador atuar como um servidor de cache par do BITS|Enabled|
||Permitir que o cache par do BITS|Desabilitada|
|BranchCache||
||Ativar o BranchCache|Desabilitada|
|Autenticação de ponto de acesso||
||Habilitar a autenticação de ponto de acesso|Desabilitada|
|Serviços de rede ponto a ponto da Microsoft||
||Desativar os serviços de rede ponto a ponto da Microsoft|Enabled|
|Arquivos Offline||
||Permitir ou não permitir usar o recurso Arquivos Offline|Desabilitada|

Recolher **rede**e, em seguida, expanda **sistema**. Ajustar cada configuração da seguinte maneira clicando duas vezes nele, e em seguida, selecionando o botão de opção para o valor indicado e clicando na **Okey** botão:

|Área de configuração|Configuração|Valor recomendado para uso VDI|  
|-------------------|----------|--------------|
|Instalação de dispositivo||
||Não enviar um relatório de erros do Windows quando um driver genérico é instalado em um dispositivo|Enabled|
||Impedir a criação de um ponto de restauração do sistema durante a atividade de dispositivo que normalmente seria solicitam a criação de um ponto de restauração|Enabled|
||Impedir a recuperação de metadados de dispositivo da Internet|Enabled|
||Impedir a enviar um relatório de erro quando um solicitações adicionais de software durante a instalação do Windows|Enabled|
||Desativar balões de "Novo Hardware encontrado" durante a instalação do dispositivo|Enabled|

Expandir **Filesystem**, clique duas vezes em **NTFS**, clique duas vezes em **as opções de criação de nome abreviadas**, selecione o botão de rádio para **habilitado**, e, em seguida, use o **opções** menu suspenso para selecionar **habilitar em todos os volumes**. Clique o **Okey** botão para salvar.

Recolher **Filesystem**e, em seguida, expanda **Internet Communication Management**. Clique em **configurações de comunicação da Internet**. Ajustar cada configuração da seguinte maneira por duas vezes nele e, em seguida, selecionando o botão de opção para **Enabled**e, em seguida, clicando no **Okey** botão:

- Desativar links "Events. asp" do Visualizador de eventos
- Desativar compartilhamento de dados de personalização de manuscrito
- Desativar relatório de erros de reconhecimento de manuscrito
- Desativar Centro de Ajuda e suporte "Você sabia?" content
- Desabilitar pesquisa na Ajuda e suporte Center Microsoft Knowledge Base
- Desativar Assistente de Conexão de Internet se a conexão de URL fizer referência a Microsoft.com
- Desativar download na Internet para publicação na Web e assistentes de pedidos online
- Desativar o serviço de associação de arquivo da Internet
- Desativar registro se a conexão de URL fizer referência a Microsoft.com
- Desativar a tarefa de imagem "Encomendar cópias"
- Desativar a tarefa "Publicar na Web" para arquivos e pastas
- Desativar o programa de Aperfeiçoamento da experiência do Windows Messenger cliente
- Desativar o programa de Aperfeiçoamento da experiência do usuário do Windows
- Desativar relatório de erros do Windows
- Desativar a pesquisa de driver de dispositivo do Windows Update

Clique em **gerenciamento de energia** e, em seguida, clique duas vezes em **selecionar um plano de energia ativo**. Selecione o botão de opção **Enabled**e, em seguida, usar o **opções** menu suspenso para selecionar **alto desempenho**. Clique o **Okey** botão para salvar.

Clique em **recuperação**e, em seguida, clique duas vezes em **permitir a restauração do sistema para o estado padrão**. Selecione o botão de opção **Enabled**e, em seguida, clique no **Okey** botão para salvar.

Expandir **solução de problemas e diagnóstico**. Clique em **manutenção agendada**, clique duas vezes em **configurar comportamento de manutenção agendada**e, em seguida, selecione o botão de opção para **desabilitado**. Clique o **Okey** botão para salvar.

Para cada uma das seguintes áreas de configurações, clique nela, clique duas vezes **configurar o nível de execução de cenário**, selecione o botão de opção **desabilitado**e, em seguida, clique no **Okey**botão para salvar:

- Diagnóstico de desempenho de inicialização do Windows
- Diagnóstico de vazamento de memória do Windows
- Resolução e detecção de esgotamento de recursos do Windows
- Diagnóstico de desempenho de desligamento do Windows
- Diagnóstico de desempenho do Windows em espera/retomar
- Diagnóstico de desempenho de capacidade de resposta de sistema do Windows

Recolher **System**e, em seguida, expanda **componentes do Windows**. Ajustar cada configuração da seguinte maneira clicando duas vezes, em seguida, selecionando o botão de opção para o valor indicado e o **Okey** botão:

|Área de configuração|Configuração|Valor recomendado para uso VDI|  
|-------------------|-------|----------|
|Adicionar recursos ao Windows 10|||
||Impedir que o assistente seja executado|Enabled|
|Políticas de reprodução automática|||
||Definir o comportamento padrão para a execução automática|Habilitado, em seguida, use o **opções** menu suspenso para selecionar **não executa quaisquer comandos de execução automática**|
|Conteúdo de nuvem|||
||Não Mostrar dicas do Windows|Enabled|
||Desativar as experiências do cliente da Microsoft|Enabled|
|Coleta de dados e compilações de visualização|||
||Permitir telemetria|Habilitado, em seguida, use o **opções** menu suspenso para selecionar **1-básico**|
||Desabilitar recursos ou configurações pré-lançamento|     Desabilitada|
||Não mostrar notificações de comentários|       Enabled|
||Ativar/desativar o controle do usuário sobre compilações do Insider|      Desabilitada|
|Gerenciador de janelas da área de trabalho|||
||Não permitir a invocação de Flip3D|       Enabled|
||Não permitir animações de janelas|       Enabled|
||Usar cor sólida para o plano de fundo de início|     Enabled|
|Borda da interface do usuário|||
||Permitir que o dedo da borda|     Desabilitada|
||Desabilitar as dicas de ajuda|        Enabled|
|Explorador de Arquivos|||
||Não mostrar a notificação de 'novo aplicativo instalado'|     Enabled|
|Gerenciador de jogos|||
||Desativar o download de informações sobre o jogo|     Enabled|
||Desabilitar atualizações de jogos|        Enabled|
||Desativar o controle de última hora de jogos na pasta jogos|     Enabled|
|Grupo Doméstico|||
||Impedir que o computador ingresse em um grupo doméstico|        Enabled|
|Internet Explorer|||
||Permitir que os serviços Microsoft deem sugestões avançadas enquanto o usuário digita na barra de endereços|        Desabilitada|
||Desabilitar a verificação periódica de atualizações de software do Internet Explorer|        Enabled|
||Desabilitar a exibição da tela inicial|        Enabled|
||Instalar novas versões do Internet Explorer automaticamente|      Desabilitada|
||Impedir a participação no programa de Aperfeiçoamento da experiência do cliente|     Enabled|
||Impedir que o Assistente de primeira execução em execução ir diretamente para a home page|   Habilitado, em seguida, use o **opções** menu suspenso para selecionar **ir diretamente para a home page**|
||Definir o crescimento do processo de guia|Habilitado, digite o seguinte na **guia processo crescimento** caixa: *Baixa*.|
||Especificar o comportamento padrão para uma nova guia|Habilitado, em seguida, use o **opções** menu suspenso para selecionar **nova página da guia**|
||Desativar notificações de desempenho de complementos|        Enabled|
||Desativar a localização geográfica do navegador|     Enabled|
||Desativar reabrir última sessão de navegação|        Enabled|
||Desativar sugestões para todos os provedores instalados pelo usuário|        Enabled|
||Ativar sites sugeridos|       Desabilitada|

No mesmo nível como o **Internet Explorer** configurações ajustadas na tabela anterior, observe a outro nível de pastas que variam de **aceleradores** para **barras de ferramentas**. Em outras palavras, agora você está no Local da política do computador > Configuração do computador > modelos administrativos > componentes do Windows > Internet Explorer. 

Abra o **Excluir histórico de navegação** pasta, clique duas vezes em **permite a exclusão de histórico de navegação ao sair**, selecione **habilitar**e, em seguida, clique em **Okey**para salvar e sair.

Use a seta para voltar no canto superior esquerda da política de Editor de Grupo Local para voltar para o **Internet Explorer** nível. Clique duas vezes em **as configurações da Internet**, clique duas vezes em **configurações avançadas**e, em seguida, ajuste as configurações nas subpastas da seguinte maneira:

|Pasta de configuração em **configurações avançadas**|Configuração|Valor recomendado para uso VDI|  
|-------------------|-------|----------|
|**Navegação**|||
||Desativar a detecção do número de telefone|Enabled|
|**Multimídia**|||
||Permitir que o Internet Explorer para reproduzir arquivos de mídia que usam os codecs alternativos|Desabilitada|

Vá novamente até o nível de **Internet Explorer**, em seguida, clique duas vezes em **configurações de Internet**. Nessa pasta, defina essas duas configurações sob **AutoCompletar** à **habilitado**:

- Desativar Sugestões de URLs
- Desativar o preenchimento automático do Windows Search

Vá fazer backup de quatro níveis para **componentes do Windows**, clique duas vezes em **local e sensores**e, em seguida, defina dessas três configurações como **habilitado** (para cada um, clique em  **Okey** para salvar e sair):

- Desativar local
- Desativar o local de script
- Desativar sensores

Enquanto no nível de **local e sensores**, clique duas vezes em **localizador do Windows** e defina **desativar o provedor do Windows local** para **habilitado**. Clique em **Okey** para salvar e sair.

No painel esquerdo, clique em **mapas**, defina essas configurações no **Enabled**; para cada um, em seguida, clique em **Okey** para salvar e sair:

- Turn off Automatic Download and Update of Map Data
- Desativar tráfego de rede não solicitado na página de configurações de Mapas Offline

Usando o painel esquerdo, insira cada uma das seguintes configurações subpastas e ajustar as configurações individuais da seguinte maneira:

|A pasta configurações em **componentes do Windows**|Configuração|Valor recomendado para uso VDI|  
|-------------------|-------|----------|
|**OneDrive**|||
||Impedir o uso do OneDrive para armazenamento de arquivos|Enabled|
||Salve documentos no OneDrive por padrão|Desabilitada|
|**RSS Feeds**|||
||Impedir que a descoberta automática de feeds e Web Slices|Enabled|
|**Pesquisa**|||
||Permitir Cortana|        Desabilitada|
||Permitir Cortana acima da tela de bloqueio|      Desabilitada|
||Permitir que a Cortana e a pesquisa usem a localização|     Desabilitada|
||Não permitir pesquisas na Web|      Enabled|
||Não pesquisar na web ou exibir os resultados da web na pesquisa|        Enabled|
||Evitar a adição de UNC locais para indexar a partir do painel de controle|     Enabled|
||Impedir a indexação de arquivos no cache de arquivos offline|        Enabled|
|**Store**|||
||Desativar a oferta para atualizar para a versão mais recente do Windows|Enabled|
|**Relatório de erros do Windows**|||
||Enviar automaticamente os despejos de memória para relatórios de erros gerados pelo sistema operacional|       Desabilitada|
||Desabilitar o Relatório de Erros do Windows|      Enabled|
|**Windows Installer**|||
||Controle o tamanho máximo do cache de arquivos de linha de base|  Habilitado, em seguida, use da caixa de rotação na **opções** área **tamanho máximo do cache de arquivo de linha de base** para *5*.|
||Desativar a criação de pontos de verificação de restauração do sistema|      Enabled|
|**Windows Mail**|||
||Desativar o recurso de comunidades|Enabled|
|**Windows Media Player**|||
||Não mostrar caixas de diálogo do primeiro uso|       Enabled|
||Impedir o compartilhamento de mídia|        Enabled|
|**Windows Mobility Center**|||
||Desativar o Windows Mobility Center|Enabled|
|**Análise de confiabilidade do Windows**|||
||Configurar provedores WMI de confiabilidade|Desabilitada|
|**Windows Update**|||
||Permitir instalação imediata de atualizações automáticas|       Enabled|
||Remover o acesso a todos os recursos de atualização do Windows|     Enabled|
|No **Windows Update** pasta, abra **adiar atualização do Windows**|||
||Selecione quando as atualizações de recurso são recebidas|Habilitado, em seguida, nos **opções** área, use o **selecione o nível de prontidão de ramificação para as atualizações de recurso que você deseja receber** menu suspenso para selecionar **Branch atual para negócios**. Defina as **após o lançamento de uma atualização do recurso, adiar a recebê-lo para este número de dias** spinbox para *180 dias*.
||Selecione quando as atualizações de qualidade são recebidas|Habilitado, em seguida, no **opções** área, defina as **após o lançamento de uma atualização de qualidade, adiar a recebê-lo para este número de dias** spinbox para *30 dias* e marque a caixa de seleção **Pausar atualizações de qualidade**.

No painel esquerdo do Editor de diretiva de Grupo Local, clique em **configuração do usuário**. Usando o painel esquerdo, clique em **modelos administrativos** e, em seguida, insira cada uma das seguintes subpastas de configurações e ajustar as configurações individuais da seguinte maneira:

|A pasta configurações em **modelos administrativos**|Configuração|Valor recomendado para uso VDI|  
|-------------------|-------|----------|
|**Área de Trabalho**|||
||Não adicionar compartilhamentos de documentos abertos recentemente para locais de rede|Enabled|
|No **área de trabalho** pasta, abra **do Active Directory**|||
||Tamanho máximo de pesquisas do Active Directory|Habilitado, em seguida, nos **opções** área, use da caixa de rotação para definir **número de objetos retornados** para *5000*.|
|**Manu iniciar e barra de tarefas**|||
||Limpar a lista de programas recentes para novos usuários|     Enabled|
||Não exibir nem controlar itens nas Listas de Atalhos a partir de locais remotos|        Enabled|
||Desativar as notificações de balão de anúncio do recurso|     Enabled|
||Desativar o controle do usuário|       Enabled|
|No **Menu Iniciar e barra de tarefas** pasta, abra **notificações**|||
||Desativar as notificações do sistema|Enabled|
|No **componentes do Windows** pastas:|||
|**Conteúdo de nuvem**|||
||Desativar todos os recursos de Destaque do Windows|Enabled|
|**Explorador de arquivos**|||
||Desativar o cache de imagens em miniatura|       Enabled|
||Desativar exibição de entradas recentes de pesquisa na caixa de pesquisa do Explorador de arquivos|        Enabled|
||Desativar o cache de miniaturas no arquivo Thumbs oculto.|      Enabled|

## <a name="microsoft-store-apps"></a>Aplicativos da Microsoft Store
Há um número de aplicativos da Microsoft Store que deseja remover da imagem VDI; Removendo-os, diminuir o uso de CPU e conservar espaço em disco. Bons candidatos para remoção incluem:

- Baixe o Office
- Skype (visualização)
- Introdução ao (especialmente se não houver nenhuma conexão de Internet)
- Hub de Feedback
- Coleção de Paciência da Microsoft
- Pago Wi-Fi e celular

Para personalizar o perfil de usuário padrão usado para a criação de imagens de VDI, use a conta de administrador interna. Se ele já não estiver habilitado, você deve fazer isso usando usuários e grupos locais no gerenciamento do computador. Faça logon na conta de administrador para concluir as etapas a seguir.

> [!NOTE]  
> Não remova os aplicativos do sistema, como a app Store. Eles são difíceis de reinstalar. Outros aplicativos são facilmente reinstallable da Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Excluir aplicativos indesejados do perfil do usuário administrador
1. No Windows PowerShell, execute `Get-AppxPackage | ft PackageFamilyName` para ver a lista de aplicativos instalados.
2. Para cada empacotador de aplicativo que você deseja desinstalar executar cmdlets desse formato de exemplo:

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Excluir o conteúdo dos aplicativos da Store indesejados
Isso impedirá que os aplicativos seja reinstalado.
1. Aplicativos da Store lista e outros itens que provisionou os dados no armazenamento com este cmdlet: `Get-AppxProvisionedPackage -Online`.
2. Remover um pacote fornecido com `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, usando o MyAppPackage apropriado retornados da etapa 1. Por exemplo, para remover o pacote de Zune, você executaria `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Remoção de outros itens
Você pode remover o ícone do OneDrive e o aplicativo, desative os ícones do sistema e excluir as atualizações baixadas.

### <a name="remove-onedrive-icon-and-app"></a>Remover o aplicativo e o ícone do OneDrive
1. Clique em **inicie** e role até a **OneDrive** ícone.
2. Clique com botão direito do **OneDrive** ícone, aponte para **mais**e, em seguida, clique em **abrir local do arquivo**.
3. Clique com botão direito do **OneDrive** ícone no seu local de arquivo e clique em **excluir**.

Para remover o aplicativo OneDrive:
1. Clique em **inicie** e role até a **OneDrive** ícone.
2. Clique com botão direito do **OneDrive** ícone e clique **desinstalar**. Programas e recursos se abre.
3. Em programas e recursos, clique com botão direito **Microsoft OneDrive** e clique em **desinstalar**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programas e recursos (de versões anteriores do painel de controle)
1. Enviar por push o **inicie** botão, digite *controle*, e pressione ENTER.
2. Toque ou clique duas vezes **programas e recursos**.
3. Na extrema esquerda, sob **página inicial do painel de controle**, toque ou clique em **ativar ou desativar recursos do Windows ativar**. Uma nova interface do usuário será aberta.
4. Desmarque as caixas de seleção para todos os itens que você não deseja ou precisa na imagem de base, por exemplo: **Suporte para compartilhamento de arquivos SMB 1.0/CIFS**.

### <a name="turn-system-icons-off"></a>Desativar os ícones do sistema
1. Enviar por push ou clique em **inicie**e, em seguida, clique em **configurações** (o ícone de engrenagem).
2. No **encontrar uma configuração** área de texto, digite *na barra de tarefas*e, em seguida, clique em **configurações de barra de tarefas**.
3. Sob o **na barra de tarefas** seção, role ou passe o dedo para baixo até a **área de notificação** seção.
4. Clique ou toque em **ativar ou desativar a ícones de sistema**e, em seguida, ativar cada ícone do sistema ou desativar como preferir para a imagem.

### <a name="delete-downloaded-updates"></a>Excluir atualizações baixadas
1. Usando o Explorador de arquivos, navegue até **C:\Windows\Software Distribution\Download**.
2. Exclua todos os arquivos e pastas nesse diretório.













 













