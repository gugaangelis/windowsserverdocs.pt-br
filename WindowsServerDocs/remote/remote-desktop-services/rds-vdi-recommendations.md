---
title: Configuração recomendada para desktops VDI
description: Configurações e definições recomendadas para minimizar a sobrecarga para desktops com Windows 10 1607 (10.0.1393) usados como imagens de VDI
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/18/2018
ms.topic: article
ms.assetid: 2a44dc9f-c221-4bf7-89c3-fb4c86a90f8c
author: jaimeo
manager: dougkim
ms.openlocfilehash: 2ab78ccbc4e49bd95a74fe1e17d5ea14891eb1b8
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857269"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Configurações recomendadas para desktops de VDI

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows 10

A Virtualização de Área de Trabalho da Microsoft detecta automaticamente as configurações do dispositivo e as condições da rede para colocar os usuários em funcionamento mais cedo, permitindo a instalação instantânea de áreas de trabalho e aplicativos corporativos, além de capacitar a TI a dar acesso a aplicativos herdados durante a migração para o Windows 10.

Embora o sistema operacional Windows 10 seja muito bem ajustado para usar, há oportunidades para você aperfeiçoar ainda mais especificamente para o ambiente corporativo da VDI (Virtual Desktop Infrastructure) da Microsoft. No ambiente da VDI, muitos serviços e tarefas em segundo plano são desabilitados desde o início.

Este tópico não é um blueprint, mas um guia ou um ponto de partida. Algumas recomendações podem desabilitar funcionalidades que você prefere usar, portanto você deve considerar o custo em relação ao benefício de ajustar qualquer configuração específica em seu cenário.

Estas instruções e as configurações recomendadas são pertinentes ao Windows 10 1607 (versão 10.0.1393).

> [!NOTE]
> As configurações não mencionadas especificamente neste tópico podem ser deixadas em seus valores padrão (ou definidas de acordo com seus requisitos e políticas) sem impacto considerável na funcionalidade da VDI.

Quando você cria uma imagem para basear a implantação da VDI, certifique-se de usar o **Branch Atual**. Para obter mais informações sobre o Branch Atual, veja [informações de versão do Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Criando a imagem do Windows 10
A primeira etapa é instalar uma imagem de referência do Windows 10 1607 (versão 10.0.1393) em uma máquina virtual ou física. A instalação em uma máquina virtual é fácil e permite que você salve as versões do arquivo do VHD (disco rígido virtual), caso queira reverter para uma versão anterior.

Durante a instalação, você pode escolher **Configurações Expressas** ou **Personalizar**. As configurações oferecidas durante a opção **Personalizar** são ajustáveis por meio da Política de Grupo, portanto o método de instalação do sistema operacional de base não é tão importante.


Se você escolher **Personalizar**, poderá ajustar essas configurações durante a instalação:

## <a name="in-customize-settings"></a>Em "Personalizar configurações"

Você também pode ajustá-las após a instalação com o Editor de Política de Grupo. Consulte a seção "Configurações de Política de Grupo" deste tópico.

|Configuração|Valor padrão|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|**Personalização**| | |
|Personalizar sua entrada de fala, digitação e escrita à tinta, enviando seus dados de entrada para a Microsoft.|    Ativado| Off|
|Enviar dados de digitação e escrita à tinta para a Microsoft para aprimorar a plataforma de reconhecimento e sugestão.|  Ativado| Off|
|Permitir que aplicativos usem sua ID de anúncio para experiência entre aplicativos.|  Ativado| Off|
|Deixar o Skype (se instalado) o ajudar a conectar-se com seus amigos no catálogo de endereços e verificar seu número de celular. Tarifas de SMS e dados podem ser aplicadas.|    Ativado| Off|
|**Local**| | |
|Ativar o Localizar Meu Dispositivo e permitir que o Windows e aplicativos solicitem sua localização, incluindo o histórico de localização| Ativado| Off|
|Conectividade e relatórios de erros| | |
|Conectar automaticamente a hotspots abertos sugeridos. Nem todas as redes são seguras.|    Ativado| Off|
|Conectar-se automaticamente a hotspots abertos temporariamente para ver se há serviços de rede pagos disponíveis.| Ativado| Off|
|Enviar dados de uso e diagnóstico completos à Microsoft. A desativação dessa opção envia somente dados básicos.| Ativado| Off|
|**Navegador, proteção e atualização**| | |
|Use serviços online do SmartScreen para ajudar a proteger contra conteúdo mal-intencionado e downloads em sites carregados por navegadores do Windows e aplicativos da Microsoft Store|    Ativado| Ativado (se não houver acesso à Internet, defina como Desativado).
|Usar previsão de página para melhorar a leitura, acelerar a navegação e tornar sua experiência geral melhor em navegadores do Windows. Seus dados de navegação serão enviados à Microsoft.| Ativado| Off|
|Obter atualizações e enviar atualizações para outros computadores na Internet para acelerar os downloads de aplicativos e do Windows Update|   Ativado| Off|

Depois que a instalação for concluída, você poderá continuar ajustando as configurações, começando por **Configurações do Windows**.

## <a name="in-windows-settings"></a>Em Configurações do Windows
Para acessar as Configurações do Windows, clique em **Iniciar** (o ícone do Windows na barra de tarefas) e, em seguida, clique no ícone **Configurações** (em forma de uma engrenagem).

### <a name="in-the-system-area-of-windows-settings"></a>Na área "Sistema" das Configurações do Windows
Na área de Configurações do Windows, clicando no ícone **Sistema**, você tem acesso a uma série de configurações relacionadas ao sistema. Nem todos elas precisam de ajustes para um uso ideal da VDI. Essas configurações são as mais importantes:

#### <a name="apps-and-features"></a>Aplicativos e recursos

Para remover um aplicativo, excluindo-o da sua imagem da VDI, clique no aplicativo e, em seguida, clique em **Desinstalar**. Se a opção **Desinstalar** estiver esmaecida, você não poderá removê-lo por esse método e poderá removê-lo com o Windows PowerShell ou tentar estas etapas:
1. Clique em **Gerenciar recursos opcionais** (imediatamente abaixo do título **Aplicativos e recursos** na mesma página).
2. Clique no recurso opcional e, em seguida, clique em **Desinstalar**.

Recursos para considerar remoção (se estiverem presentes) incluem o seguinte:
- **Entre em contato com o suporte**
- **Conteúdo de demonstração de varejo em inglês (Estados Unidos)**
- **Conteúdo de demonstração de varejo neutro**
- **Assistência Rápida**

#### <a name="default-apps"></a>Aplicativos padrão

Essa área define o aplicativo a ser usado por padrão para determinadas funções genéricas, como email, navegação na Web e mapas. Se você quiser que um aplicativo diferente seja usado para uma função específica, clique na entrada atual e, em seguida, clique no aplicativo que você prefere que seja usado na imagem da VDI. Para que um aplicativo que não seja da Microsoft apareça como uma opção disponível, você deve instalar o aplicativo antes de ajustar essa configuração.

#### <a name="notifications-and-actions"></a>Notificações e ações

Esses valores recomendados reduzirão as notificações e a atividade de rede em segundo plano em um ambiente de VDI:

|Configuração|Valor padrão|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|Obter notificações de aplicativos e outros remetentes| Ativado| Off|
|Mostrar notificações na tela de bloqueio.|    Ativado| Off|
|Mostrar alarmes, lembretes e chamadas VoIP na tela de bloqueio.|   Ativado| Off|
|Mostrar dicas, truques e sugestões ao usar o Windows.|    Ativado| Off|


#### <a name="offline-maps"></a>Mapas offline

Essa configuração será aplicável somente se o aplicativo Mapas estiver instalado. Seu valor padrão é **Ativado**; para a VDI o valor recomendado é **Desativado**.

#### <a name="tablet-mode"></a>Modo tablet

|Configuração|Valor padrão|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|Quando eu entrar|    Usar o modo apropriado para o meu hardware|   Usar modo desktop|
|Quando este dispositivo alternar automaticamente entre ligar ou desligar o modo tablet|    Sempre perguntar antes de alternar| Não me perguntar e não alternar|
|Ocultar ícones de aplicativos na barra de tarefas no modo tablet|  Ativado| Off|


### <a name="in-the-devices-area-of-windows-settings"></a>Na área "Dispositivos" das Configurações do Windows
Na área de Configurações do Windows, clicando no ícone **Dispositivos**, você tem acesso a uma série de configurações relacionadas ao sistema. Nem todos elas precisam de ajustes para um uso ideal da VDI. Essas configurações são as mais importantes:

#### <a name="autoplay"></a>Reprodução Automática

|Configuração|Valor padrão|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|Usar a Reprodução Automática para todas as mídias e dispositivos|    Ativado| Off|
|Unidade de disco removível:|Escolher um padrão|Nenhuma ação|
|Cartão de memória|Escolher um padrão|Nenhuma ação|

### <a name="in-the-personalization-area-of-windows-settings"></a>Na área "Personalização" das Configurações do Windows
Na área de Configurações do Windows, clicando no ícone **Personalização**, você tem acesso a uma série de configurações relacionadas ao sistema. Nem todos elas precisam de ajustes para um uso ideal da VDI. Essas configurações são as mais importantes:

#### <a name="background"></a>Tela de fundo
Às vezes, a tela de fundo preta padrão pode fazer os usuários pensarem que o computador não está respondendo. Alterar a cor da tela de fundo pode ajudar a deixar isso mais claro. Para fazer isso, execute estas etapas:
1. Na área **Tela de fundo**, clique no menu suspenso.
2. Para alterar a cor da tela de fundo, clique em **Cor sólida** e, em seguida, clique em qualquer uma das cores diferente de preto. Como alternativa, você poderia clicar em **Imagem** e, em seguida, selecionar uma imagem para usar como tela de fundo.

#### <a name="start"></a>Inicie o

|Configuração|Valor padrão|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|Ocasionalmente mostrar sugestões em Iniciar|    Ativado| Off|
|Mostrar aplicativos mais usados|Ativado|Off|
|Mostrar aplicativos adicionados recentemente|Ativado|Off|
|Mostrar itens abertos recentemente nas Listas de Atalhos em Iniciar ou na Barra de tarefas|Ativado|Off|

#### <a name="taskbar"></a>Barra de tarefas
A configuração padrão é usar botões grandes da barra de tarefas (ou seja, um valor de "Desativado" para **Usar botões pequenos da barra de tarefas**). Essa configuração faz com que o item Cortana use muito espaço da área da barra de tarefas. Para evitar isso, defina **Usar botões pequenos da barra de tarefas** como "Ativado". Se você preferir que os itens da barra de tarefas fiquem maiores, mas desejar que a Cortana não ocupe muito espaço, clique com o botão direito do mouse na barra de tarefas, aponte para **Cortana** e, no menu que aparecer, selecione **Oculto**.

### <a name="in-the-privacy-area-of-windows-settings"></a>Na área "Privacidade" das Configurações do Windows
Na área de Configurações do Windows, clicando no ícone **Privacidade**, você tem acesso a uma série de configurações relacionadas ao sistema. Nem todos elas precisam de ajustes para um uso ideal da VDI. Essas configurações são as mais importantes:

#### <a name="general"></a>Geral
Algumas dessas configurações também são definidas na janela "Personalizar configurações", abordada no início deste tópico.

|Configuração|Valor padrão|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|Permitir que os aplicativos usem minha ID de anúncio para experiências entre aplicativos (desligar essa opção redefinirá sua ID)|  Ativado| Off|
|Permitir que sites forneçam conteúdo local relevante acessando minha lista de idiomas|Ativado|Off|
|Permitir que os aplicativos em meus outros dispositivos abram aplicativos e continuem experiências neste dispositivo|Ativado|Off|

#### <a name="camera"></a>Câmera

O valor padrão para "Permitir que aplicativos usem minha câmera" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.


#### <a name="microphone"></a>Microfone

O valor padrão para "Permitir que aplicativos usem meu microfone" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="notifications"></a>Notificações

O valor padrão para "Permitir que aplicativos acessem minhas notificações" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="contacts"></a>Contacts

O valor padrão para "Permitir que aplicativos acessem meus contatos" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="calendar"></a>Calendar

O valor padrão para "Permitir que aplicativos acessem meu calendário" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="call-history"></a>Histórico de chamadas

O valor padrão para "Permitir que aplicativos acessem meu histórico de chamadas" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="email"></a>Email

O valor padrão para "Permitir que os aplicativos acessem e enviem email" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="messaging"></a>Sistema de mensagens

O valor padrão para "Permitir que os aplicativos leiam ou enviem mensagens (texto ou MMS)" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="radios"></a>Rádios

O valor padrão para "Permitir que os aplicativos controlem rádios" é **Ativado**; para o uso da VDI, o valor recomendado é **Desativado**.

#### <a name="other-devices"></a>Outros dispositivos

O valor padrão para "Permitir que os aplicativos compartilhem e sincronizem informações com dispositivos sem fio que não emparelham explicitamente com seu computador, tablet ou telefone automaticamente" é **Ativado**; para uso da VDI, o valor recomendado é **Desativado**.

#### <a name="feedback-and-diagnostics"></a>Comentários e diagnóstico

O valor padrão para "O Windows deve pedir meu feedback" é **Automaticamente**; para o uso da VDI, o valor recomendado é **Nunca**.

#### <a name="background-apps"></a>Aplicativos de segundo plano
Os aplicativos listados têm um valor padrão de **Ativado**, o que permite que eles recebam informações, enviem notificações e sejam atualizados, estando ou não em uso. Você deve desabilitar (definir como **Desativado**) os aplicativos que você não quer a execução em segundo plano na imagem da VDI.

### <a name="update-and-security"></a>Atualização e segurança
#### <a name="windows-update"></a>Windows Update
Na área **Atualizar configurações**, clique em **Opções avançadas** para ajustar essas configurações:

|Configuração|Valor padrão|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|Fornecer atualizações para outros produtos Microsoft quando eu atualizar o Windows|    desmarcado|    selecionado|
|Adiar atualizações de recursos|desmarcado|selecionado|
|Usar minhas informações de entrada para concluir automaticamente a configuração do meu dispositivo após uma atualização |desmarcado|Depende da configuração específica da VDI|

Na página **Opções avançadas**, clique em **Escolher como as atualizações serão obtidas** para acessar a configuração de "Atualizações de mais de um local". O valor padrão é **Ativado**; para a VDI o valor recomendado é **Desativado**.

## <a name="in-control-panel-and-other-system-utilities"></a>Em Painel de Controle e outros utilitários do sistema

As configurações desta seção são ajustáveis ao navegar no Painel de Controle ou abrindo diretamente o utilitário.

> [!NOTE]
> As configurações não mencionadas especificamente neste tópico podem ser deixadas em seus valores padrão (ou definidas de acordo com seus requisitos e políticas) sem impacto considerável na funcionalidade da VDI.


### <a name="task-scheduler"></a>Agendador de Tarefas
A maneira mais rápida de abrir o Agendador de Tarefas é pressionar o botão Windows e digitar *Agendador de Tarefas* ou *taskschd.msc*. Nos resultados retornados, clique em **Agendador de Tarefas** para abrir o utilitário. No Agendador de Tarefas, expanda **Biblioteca do Agendador de Tarefas**, expanda **Microsoft** e, em seguida, expanda **Windows**. Agora você tem acesso à lista de coleções de tarefas. Para alterar o estado de cada tarefa agendada, clique com o botão direito do mouse e, em seguida, clique no estado desejado (normalmente **Desabilitado** para uso da VDI).

|Coleção de tarefas|Nome da tarefa|Estado padrão|Estado recomendado para uso da VDI|
|-------------------|-------------|----------|--------------|
|Programa de Aperfeiçoamento da Experiência do Usuário||||
||Consolidador|Habilitada|Desabilitado|
||KernelCeipTask|Habilitada|Desabilitado|
||UsbCeip|Habilitada|Desabilitado|
|Desfragmentação||||
||ScheduledDefrag|Habilitada|Desabilitado|
|Local||||
||Notificações|Habilitada|Desabilitado|
||WindowsActionDialog|Habilitada|Desabilitado|
|Manutenção||||
||WinSAT|Habilitada|Desabilitado|
|Mapas||||
||MapsToastTask|Habilitada|Desabilitado|
||MapsUpdateTask|Habilitada|Desabilitado|
|Contas de banda larga móvel||||
||Analisador de metadados MNO|Habilitada|Desabilitado|
|Diagnóstico de Eficiência de Consumo de Energia||||
||Analisar Sistema|Habilitada|Desabilitado|
|Ambiente de Recuperação||||
||VerifyWinRE|Habilitada|Desabilitado|
|Demonstração de Revenda||||
||CleanupOfflineContent|Habilitada|Desabilitado|
|Shell||||
||FamilySafetyMonitor|Habilitada|Desabilitado|
||FamilySafetyRefreshTask|Habilitada|Desabilitado|
|Relatório de Erros do Windows||||
||QueueReporting|Habilitada|Desabilitado|
|Compartilhamento de Mídia do Windows||||
||UpdateLibrary|Habilitada|Desabilitado|

Clique em **Windows** novamente para recolher e, em seguida, clique em **XblGameSave**. Isso fornece acesso às tarefas **XBLGameSaveTask** e **XBLGameSaveTaskLogon**; ambas podem ser definidas como **Desabilitado**.

### <a name="performance-monitor"></a>Desempenho do sistema
A maneira mais rápida para abrir o Monitor de Desempenho é pressionar o botão Windows e digitar *monitor de desempenho* ou *perfmon.msc*. Nos resultados retornados, clique em **Monitor de Desempenho**. No Monitor de Desempenho, clique em **Conjuntos de Coletores de Dados** e, em seguida, clique duas vezes em **Sessões de Rastreamento de Eventos**. Clique com o botão direito do mouse em **WiFiSession**; se estiver no estado padrão **Em execução**, clique em **Parar**.

Clique em **StartupEventTraceSessions** e, em seguida, clique com o botão direito do mouse em **ReadyBoot**; se estiver em execução, clique em **Parar**. Clique em **Sessões de Rastreamento de Eventos**, clique com o botão direito do mouse em **ReadyBoot** e, em seguida, clique em **Propriedades**. Na caixa de diálogo que é aberta, clique na guia **Sessão de Rastreamento**. Desmarque a caixa de seleção **Habilitado**.

### <a name="services"></a>Serviços
A maneira mais rápida para gerenciar os Serviços é pressionar o botão Windows e digitar *serviços*. Nos resultados retornados, clique em **Serviços**. Os serviços a seguir são bons candidatos para desabilitar para uso em cenários de VDI, no entanto, talvez seja interessante fazer alguns testes para verificar se eles não são necessários para suas finalidades. Para desabilitar um serviço, no snap-in **Serviços**, clique com o botão direito do mouse no nome do serviço e, em seguida, clique em **Propriedades**. Na guia **Geral**, clique no menu suspenso **Tipo de inicialização** e, em seguida, clique em **Desabilitado**. Clique em **OK**.

- BranchCache
- Otimização de Entrega
- Host de Serviço de Diagnóstico
- Serviço de Hotspot Móvel do Windows
- Gerenciador de Autenticação Xbox Live
- Salvamento de Jogo do Xbox Live
- Serviço de Rede Xbox Live

### <a name="file-explorer-options"></a>Opções do Explorador de Arquivos
Pressione o botão Windows e digite *painel de controle*. Nos resultados retornados, clique em **Painel de Controle**. No Painel de Controle, clique em **Opções do Explorador de Arquivos**. Na caixa de diálogo que é aberta, clique na guia **Pesquisa** e, em seguida, na área **Ao pesquisar locais não indexados**, desmarque a caixa de seleção **Incluir diretórios do sistema**. Clique em **OK** para salvar.

### <a name="flash-settings"></a>Configurações de Flash
Pressione o botão Windows e digite *painel de controle*. Nos resultados retornados, clique em **Painel de Controle**. No Painel de Controle, clique em **Flash Player** para abrir o Gerenciador de Parâmetros do Flash Player. Na guia **Armazenamento**, selecione o botão de opção **Bloquear todos os sites contra o armazenamento de informações neste computador**. Na caixa de diálogo que é aberta, clique em **OK**.

Na guia **Câmera e Microfone** na área **Configurações de Câmera e Microfone**, selecione o botão de opção para **Bloquear o uso da câmera e do microfone por todos os sites**.

Na guia **Reprodução** da área **Rede de pares**, selecione o botão de opção para **Bloquear o uso da rede de pares por todos os sites**. Feche o Gerenciador de Parâmetros do Flash Player.

### <a name="internet-options"></a>Opções da Internet
Pressione o botão Windows e digite *painel de controle*. Nos resultados retornados, clique em **Painel de Controle**. No Painel de Controle, clique em **Opções da Internet** para abrir as Propriedades da Internet. Na área **Home page**, insira a URL do site da Web que você deseja que os usuários vejam como a página inicial em navegadores. Pode ser um site da Web da sua empresa ou você pode definir uma página inicial em branco, inserindo *about:blank*.

Na área **Histórico de navegação**, selecione a caixa de seleção **Excluir histórico de navegação ao sair**.

### <a name="power-options"></a>Opções de Energia
Pressione o botão Windows e digite *painel de controle*. Nos resultados retornados, clique em **Painel de Controle**. No Painel de Controle, clique em **Opções de Energia** para abrir o painel de controle correspondente. Na área **Escolher ou personalizar um plano de energia**, clique na seta para baixo **Mostrar planos adicionais** e, em seguida, selecione o botão de opção para **Alto desempenho**. Essa configuração terá um impacto mínimo no host de VDI.

### <a name="system"></a>Sistema
Pressione o botão Windows e digite *painel de controle*. Nos resultados retornados, clique em **Painel de Controle**. No Painel de Controle, clique em **Sistema** para abrir o painel de controle Sistema. No painel esquerdo, clique em **Configurações avançadas do sistema**. Na caixa de diálogo que é aberta, clique na guia **Avançado**. Na área **Desempenho**, clique no botão **Configurações** e, em seguida, na guia **Efeitos Visuais** na caixa de diálogo que é aberta, selecione o botão de opção **Ajustar para melhor desempenho**. Clique em **OK** para salvar e sair.

## <a name="group-policy-settings"></a>Configurações de Política de Grupo

Para editar as configurações da Política de Grupo, pressione o botão Windows e digite *política de grupo* ou *gpedit.msc*. Nos resultados retornados, clique em **Editar a política de grupo** para abrir o Editor de Política de Grupo Local.

> [!NOTE]
> As configurações não mencionadas especificamente neste tópico podem ser deixadas em seus valores padrão (ou definidas de acordo com seus requisitos e políticas) sem impacto considerável na funcionalidade da VDI.

Em **Configuração do Computador**, expanda **Configurações do Windows** e, em seguida, expanda **Configurações de Segurança**. Clique em **Políticas do Gerenciador de Listas de Redes** e, em seguida, clique duas vezes em **Todas as Redes**. Na caixa de diálogo que é aberta, na área **Localização de rede**, selecione o botão de opção de **Usuário não pode alterar a localização**. Clique no botão **OK** para salvar.

Recolha **Configurações do Windows** e, em seguida, expanda **Modelos Administrativos**. Clique ou expanda **Rede** e, em seguida, ajuste cada configuração conforme explicado a seguir clicando duas vezes e, em seguida, selecionando o botão de opção de acordo com o valor indicado. Depois, clique no botão **OK**:

|Área de configuração|Configuração|Valor recomendado para uso da VDI|
|-------------------|-------|----------|
|BITS|||
||Não permitir que o cliente BITS use o Windows Branch Cache|Habilitada|
||Não permitir que o computador aja como um cliente de cache de sistemas pares do BITS|Habilitada|
||Não permitir que o computador aja como um servidor de cache de sistemas pares do BITS|Habilitada|
||Permitir cache de sistemas pares do BITS|Desabilitado|
|BranchCache||
||Habilitar o BranchCache|Desabilitado|
|Autenticação de hotspot||
||Habilitar a autenticação de hotspot|Desabilitado|
|Serviços de rede ponto a ponto da Microsoft||
||Desligar os Serviços de rede ponto a ponto da Microsoft|Habilitada|
|Arquivos Offline||
||Permitir ou não permitir o uso do recurso Arquivos Offline|Desabilitado|

Recolha **Rede** e, em seguida, expanda **Sistema**. Ajuste cada configuração conforme explicado a seguir clicando duas vezes e, em seguida, selecionando o botão de opção de acordo com o valor indicado. Depois, clique no botão **OK**:

|Área de configuração|Configuração|Valor recomendado para uso da VDI|
|-------------------|----------|--------------|
|Instalação de dispositivos||
||Não enviar um Relatório de Erros do Windows quando um driver genérico for instalado em um dispositivo|Habilitada|
||Impedir a criação de um ponto de restauração do sistema durante uma atividade do dispositivo que normalmente solicitaria a criação de um ponto de restauração|Habilitada|
||Impedir a recuperação de metadados de dispositivo da Internet|Habilitada|
||Impedir o Windows de enviar um relatório de erros quando um driver de dispositivo solicitar software adicional durante a instalação|Habilitada|
||Desligar balões de "Novo Hardware Encontrado" durante instalação de dispositivos|Habilitada|

Expanda **Filesystem**, clique duas vezes em **NTFS**, clique duas vezes em **Opções de criação de nome curto**, selecione o botão de opção para **Habilitado** e, em seguida, use o menu suspenso **Opções** para selecionar **Habilitar em todos os volumes**. Clique no botão **OK** para salvar.

Recolha **Filesystem** e, em seguida, expanda **Gerenciamento de Comunicação da Internet**. Clique em **Configurações de Comunicação da Internet**. Ajuste cada configuração conforme explicado a seguir clicando duas vezes e, em seguida, selecionando o botão de opção para **Habilitado** e, em seguida, clique no botão **OK**:

- Desligar links "Events. asp" do Visualizador de Eventos
- Desligar compartilhamento de dados de personalização de manuscrito
- Desligar relatório de erros de reconhecimento de manuscrito
- Desligar o conteúdo "Você sabia?" do Centro de Ajuda e Suporte conteúdo
- Desligar pesquisa do Centro de Ajuda e Suporte na Base de Dados de Conhecimento Microsoft
- Desligar Assistente de Conexão com a Internet se a URL de conexão fizer referência a Microsoft.com
- Desligar download na Internet para publicação na Web e assistentes de pedidos online
- Desligar o serviço de Associação de Arquivo na Internet
- Desligar registro se a conexão de URL fizer referência a Microsoft.com
- Desligar a tarefa da imagem "Encomendar cópias"
- Desligar a tarefa "Publicar na Web" para arquivos e pastas
- Desligar o Programa de Aperfeiçoamento da Experiência do Usuário no Windows Messenger
- Desligar o Programa de Aperfeiçoamento da Experiência do Usuário do Windows
- Desligar Relatório de Erros do Windows
- Desligar a pesquisa de driver de dispositivo do Windows Update

Clique em **Gerenciamento de Energia** e, em seguida, clique duas vezes em **Selecionar um plano de energia ativo**. Selecione o botão de opção **Habilitado** e, em seguida, use o menu suspenso **Opções** para selecionar **Alto Desempenho**. Clique no botão **OK** para salvar.

Clique em **Recuperação** e, em seguida, clique duas vezes em **Permitir restauração do sistema ao estado padrão**. Selecione o botão de opção **Habilitado** e, em seguida, clique no botão **OK** para salvar.

Expanda **Solução de problemas e Diagnóstico**. Clique em **Manutenção Agendada**, clique duas vezes em **Configurar Comportamento de Manutenção Programada** e, em seguida, selecione o botão de opção para **Desabilitado**. Clique no botão **OK** para salvar.

Clique em cada uma das seguintes áreas de configurações, em seguida, clique duas vezes em **Configurar Nível de Execução do Cenário**, selecione o botão de opção **Desabilitado** e, em seguida, clique no botão **OK** para salvar:

- Diagnóstico de Desempenho de Inicialização do Windows
- Diagnóstico de Perda de Memória do Windows
- Detecção e Resolução de Exaustão de Recursos do Windows
- Diagnóstico de Desempenho de Desligamento do Windows
- Diagnóstico de Desempenho do Windows em Espera/Reiniciado
- Diagnóstico do Desempenho da Capacidade de Resposta do Sistema Windows

Recolha **Sistema** e, em seguida, expanda **Componentes do Windows**. Ajuste cada configuração conforme explicado a seguir ao clicar duas vezes e, em seguida, selecione o botão de opção de acordo com o valor indicado. Depois, clique no botão **OK**:

|Área de configuração|Configuração|Valor recomendado para uso da VDI|
|-------------------|-------|----------|
|Adicionar recursos ao Windows 10|||
||Impedir que o assistente seja executado|Habilitada|
|Políticas de reprodução automática|||
||Definir o comportamento padrão de Execução Automática|Habilitado, em seguida, use o menu suspenso **Opções** para selecionar **Não executar comandos de autorun**|
|Conteúdo de nuvem|||
||Não mostrar dicas do Windows|Habilitada|
||Desativar as experiências do cliente da Microsoft|Habilitada|
|Coleta de dados e versões prévias|||
||Permitir Telemetria|Habilitado, em seguida, use o menu suspenso **Opções** para selecionar **1 – Básico**|
||Desabilitar recursos ou configurações pré-lançamento|     Desabilitado|
||Não mostrar notificações de comentários|       Habilitada|
||Ativar/desativar o controle do usuário sobre builds do Insider|      Desabilitado|
|Gerenciador de Janelas da Área de Trabalho|||
||Não permitir invocação de Flip3D|       Habilitada|
||Não permitir animações de janelas|       Habilitada|
||Use cor sólida para a tela de fundo da tela inicial|     Habilitada|
|UI Borda|||
||Permitir passar o dedo na borda|     Desabilitado|
||Desabilitar dicas de ajuda|        Habilitada|
|Explorador de Arquivos|||
||Não mostrar a notificação de ‘novo aplicativo instalado’|     Habilitada|
|Navegador de Jogos|||
||Desligar download de informações de jogos|     Habilitada|
||Desligar atualizações de jogos|        Habilitada|
||Desligar acompanhamento da última hora de jogos na pasta Jogos|     Habilitada|
|Homegroup|||
||Impedir que o computador ingresse em um grupo doméstico|        Habilitada|
|Internet Explorer|||
||Permitir que os serviços Microsoft forneçam sugestões avançadas enquanto o usuário digita algo na barra de endereços|        Desabilitado|
||Desabilitar a verificação periódica de atualizações de software do Internet Explorer|        Habilitada|
||Desabilitar a exibição da tela inicial|        Habilitada|
||Instalar novas versões do Internet Explorer automaticamente|      Desabilitado|
||Impedir participação no Programa de Aperfeiçoamento da Experiência do Usuário|     Habilitada|
||Impedir a execução do Assistente de Primeira Execução. Ir diretamente para a home page|   Habilitado, em seguida, use o menu suspenso **Opções** para selecionar **Ir diretamente para a home page**|
||Definir crescimento de processos de guias|Habilitado, em seguida, digite o seguinte na caixa **Crescimento de Processos de Guias**: *Baixa*.|
||Especificar categoria padrão para uma nova guia|Habilitado, em seguida, use o menu suspenso **Opções** para selecionar **Nova página da guia**|
||Desativar notificações de desempenho de complementos|        Habilitada|
||Desativar a geolocalização do navegador|     Habilitada|
||Desligar Reabrir Última Sessão de Navegação|        Habilitada|
||Desligar sugestões para os provedores instalados pelo usuário|        Habilitada|
||Ativar Sites Sugeridos|       Desabilitado|

No mesmo nível que as configurações do **Internet Explorer** ajustadas na tabela anterior, observe outro nível de pastas que variam de **Aceleradores** a **Barras de ferramentas**. Em outras palavras, agora você está em Política do Computador Local> Configuração do Computador > Modelos Administrativos > Componentes do Windows > Internet Explorer.

Abra a pasta **Excluir Histórico de Navegação**, clique duas vezes em **Permitir exclusão de histórico de navegação ao sair**, selecione **Habilitar** e, em seguida, clique em **OK** para salvar e sair.

Use a seta para voltar no canto superior esquerdo do Editor de Política de Grupo Local para voltar para o nível **Internet Explorer**. Clique duas vezes em **Configurações da Internet**, clique duas vezes em **Configurações Avançadas** e, em seguida, ajuste as configurações nas subpastas da seguinte maneira:

|Pasta de configuração em **Configurações Avançadas**|Configuração|Valor recomendado para uso da VDI|
|-------------------|-------|----------|
|**Navegação**|||
||Desativar a detecção de número de telefone|Habilitada|
|**Multimídia**|||
||Permitir que o Internet Explorer reproduza arquivos de mídia que usem codecs alternativos|Desabilitado|

Retorne ao nível do **Internet Explorer** e, em seguida, clique duas vezes em **Configurações de Internet**. Nessa pasta, defina essas duas configurações em **Preenchimento Automático** como **Habilitado**:

- Desativar Sugestões de URLs
- Desligar o preenchimento automático do Windows Search

Retorne quatro níveis até **Componentes do Windows**, clique duas vezes em **Localização e sensores** e, em seguida, defina dessas três configurações como **Habilitado** (para cada uma, clique em **OK** para salvar e sair):

- Desligar localização
- Desligar script de localização
- Desligar sensores

No nível de **Localização e sensores**, clique duas vezes em **Localizador do Windows** e defina **Desligar Localizador do Windows** como **Habilitado**. Clique em **OK** para salvar e sair.

No painel esquerdo, clique em **Mapas**, defina essas configurações como **Habilitado**; para cada uma delas, clique em **OK** para salvar e sair:

- Desativar Download e Atualização Automáticos de Dados do Mapa
- Desativar tráfego de rede não solicitado na página de configurações de Mapas Offline

Usando o painel esquerdo, insira cada uma das seguintes subpastas de configurações e ajuste as configurações individuais da seguinte maneira:

|Pasta configurações em **Componentes do Windows**|Configuração|Valor recomendado para uso da VDI|
|-------------------|-------|----------|
|**OneDrive**|||
||Impedir o uso do OneDrive para armazenamento de arquivos|Habilitada|
||Salvar documentos no OneDrive por padrão|Desabilitado|
|**RSS Feeds**|||
||Impedir a descoberta automática de feeds e Web Slices|Habilitada|
|**Pesquisa**|||
||Permitir a Cortana|        Desabilitado|
||Permitir a Cortana acima da tela de bloqueio|      Desabilitado|
||Permitir que a pesquisa e a Cortana usem a localização|     Desabilitado|
||Não permitir pesquisas na Web|      Habilitada|
||Não pesquisar na Web nem exibir resultados da Web na Pesquisa|        Habilitada|
||Impedir a adição de locais UNC ao índice do Painel de Controle|     Habilitada|
||Impedir indexação de arquivos no cache de arquivos offline|        Habilitada|
|**Store**|||
||Desligar a oferta de atualização para a última versão do Windows|Habilitada|
|**Relatório de Erros do Windows**|||
||Enviar automaticamente despejos de memória para relatórios de erro gerados pelo SO|       Desabilitado|
||Desabilitar o Relatório de Erros do Windows|      Habilitada|
|**Windows Installer**|||
||Controlar tamanho máximo do cache de arquivo de linha de base|  Habilitado, em seguida, use da caixa de rotação na área **Opções** para definir **Tamanho máximo do cache de arquivo de linha de base** como *5*.|
||Desabilitar a criação de pontos de verificação de Restauração do Sistema|      Habilitada|
|**Windows Mail**|||
||Desligar o recurso de comunidades|Habilitada|
|**Windows Media Player**|||
||Não Mostrar Caixas de Diálogo de Primeiro Uso|       Habilitada|
||Impedir Compartilhamento de Mídia|        Habilitada|
|**Centro de Mobilidade do Windows**|||
||Desligar o Centro de Mobilidade do Windows|Habilitada|
|**Análise de Confiabilidade do Windows**|||
||Configurar os Provedores WMI de Confiabilidade|Desabilitado|
|**Windows Update**|||
||Permitir instalação imediata de Atualizações Automáticas|       Habilitada|
||Remover acesso ao uso de todos os recursos do Windows Update|     Habilitada|
|Na pasta **Windows Update**, abra **Adiar Atualização do Windows**|||
||Selecione quando as atualizações de recursos são recebidas|Habilitado, em seguida, na área **Opções**, use o menu suspenso **Selecione o nível de preparação de branch das atualizações de recursos que você deseja receber** para selecionar **Branch Atual para Negócios**. Defina a caixa de rotação **Após o lançamento de uma atualização de recursos, adiar o recebimento durante este número de dias** como *180 dias*.
||Selecionar quando as Atualizações de Qualidade serão recebidas|Habilitado, em seguida, na área **Opções**, defina a caixa de rotação **Após o lançamento de uma atualização de qualidade, adiar o recebimento durante este número de dias** como *30 dias* e marque a caixa de seleção **Pausar atualizações de qualidade**.

No painel esquerdo do Editor de Política de Grupo Local, clique em **Configuração do Usuário**. Usando o painel esquerdo, clique em **Modelos Administrativos** e, em seguida, insira cada uma das seguintes subpastas de configurações e ajuste as configurações individuais da seguinte maneira:

|Pasta configurações em **Modelos Administrativos**|Configuração|Valor recomendado para uso da VDI|
|-------------------|-------|----------|
|**Área de trabalho**|||
||Não adicionar compartilhamentos de documentos recentemente abertos a Locais de Rede|Habilitada|
|Na pasta **Área de Trabalho**, abra **Active Directory**|||
||Tamanho máximo de pesquisas do Active Directory|Habilitado, em seguida, na área **Opções**, use a caixa de rotação para definir **Número de objetos retornados** como *5000*.|
|**Menu Iniciar e Barra de Tarefas**|||
||Limpar a lista de programas recentes para novos usuários|     Habilitada|
||Não exibir nem controlar itens nas Listas de Atalhos a partir de locais remotos|        Habilitada|
||Desligar o recurso de notificações de balões de publicidade|     Habilitada|
||Desligar o acompanhamento de usuário|       Habilitada|
|Na pasta **Menu Iniciar e Barra de tarefas**, abra **Notificações**|||
||Desligar notificações de aviso|Habilitada|
|Na pasta **Componentes do Windows**, abra:|||
|**Conteúdo de nuvem**|||
||Desativar todos os recursos de Destaque do Windows|Habilitada|
|**Explorador de Arquivos**|||
||Desligar armazenamento em cache de imagens em miniatura|       Habilitada|
||Desligar a exibição de entradas de pesquisas recentes na caixa de pesquisa do Explorador de Arquivos|        Habilitada|
||Desligar o cache de miniaturas no arquivo oculto thumbs.db|      Habilitada|

## <a name="microsoft-store-apps"></a>Aplicativos da Microsoft Store
Há diversos aplicativos da Microsoft Store que podem ser removidos da imagem da VDI. Se optar por removê-los, poderá diminuir o uso de CPU e conservar espaço em disco. Bons candidatos para remoção incluem:

- Adquira o Office
- Skype (versão prévia)
- Introdução (especialmente se não houver conexão com a Internet)
- Hub de Feedback
- Microsoft Solitaire Collection
- Wi-Fi pago e Celular

Para personalizar o perfil do usuário padrão usado para a criação de imagens de VDI, use a conta de administrador interno. Se ela ainda não estiver habilitada, faça isso usando Usuários e Grupos Locais no Gerenciamento do Computador. Depois, faça logon na Conta de Administrador para concluir as etapas a seguir.

> [!NOTE]
> Não remova aplicativos do sistema como o aplicativo da Store. Eles são difíceis de reinstalar. Outros aplicativos são facilmente reinstalados por meio da Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Excluir aplicativos indesejados usando o perfil do usuário Administrador
1. No Windows PowerShell, execute `Get-AppxPackage | ft PackageFamilyName` para ver a lista de aplicativos instalados.
2. Para cada empacotador de aplicativo que você deseja desinstalar, execute os cmdlets desse formato de exemplo:

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Excluir o conteúdo dos aplicativos da Store indesejados
Isso impedirá que os aplicativos sejam reinstalados.
1. Liste os aplicativos e outros itens da Store que provisionaram dados no armazenamento com este cmdlet: `Get-AppxProvisionedPackage -Online`.
2. Remova um pacote determinado com `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, usando o MyAppPackage apropriado retornado da etapa 1. Por exemplo, para remover o pacote relacionado ao Zune, você executaria `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Remoção de outros itens
Você pode remover o ícone do OneDrive e o aplicativo, desligar ícones do sistema e excluir as atualizações baixadas.

### <a name="remove-onedrive-icon-and-app"></a>Remover o aplicativo e o ícone do OneDrive
1. Clique em **Iniciar** e role até o ícone do **OneDrive**.
2. Clique com o botão direito do mouse no ícone do **OneDrive**, aponte para **Mais** e, em seguida, clique em **Abrir local do arquivo**.
3. Clique com o botão direito do mouse no ícone do **OneDrive** na sua localização de arquivo e clique em **Excluir**.

Para remover o aplicativo OneDrive:
1. Clique em **Iniciar** e role até o ícone do **OneDrive**.
2. Clique com o botão direito do mouse no ícone do **OneDrive** e, em seguida, clique em **Desinstalar**. A janela Programas e Recursos é aberta.
3. Em Programas e Recursos, clique com o botão direito do mouse em **Microsoft OneDrive** e clique em **Desinstalar**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programas e Recursos (de versões anteriores do Painel de Controle)
1. Pressione o botão **Iniciar**, digite *Controle* e, em seguida, pressione ENTER.
2. Toque ou clique duas vezes em **Programas e Recursos**.
3. Mais à esquerda, em **Página Inicial do Painel de Controle**, toque ou clique em **Ligar ou desligar recursos do Windows**. Uma nova interface do usuário será aberta.
4. Desmarque as caixas de seleção de todos os itens que você não deseja ou não precisa na imagem de base, por exemplo: **Suporte para Compartilhamento de Arquivos SMB 1.0/ CIFS**.

### <a name="turn-system-icons-off"></a>Desligar ícones do sistema
1. Pressione ou clique em **Iniciar** e, em seguida, clique em **Configurações** (o ícone de engrenagem).
2. Na área de texto **Encontrar uma Configuração**, digite *Barra de tarefas* e, em seguida, clique em **Configurações de Barra de tarefas**.
3. Na seção **Barra de tarefas**, role ou passe o dedo para baixo até a seção **Área de notificação**.
4. Clique ou toque em **Ligar ou desligar ícones do sistema** e, em seguida, ative ou desative os ícone do sistema como preferir para a imagem.

### <a name="delete-downloaded-updates"></a>Excluir atualizações baixadas
1. Usando o Explorador de Arquivos, navegue até **C:\Windows\Software Distribution\Download**.
2. Exclua todos os arquivos e pastas desse diretório.
