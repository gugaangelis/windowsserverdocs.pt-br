---
title: Como otimizar o Windows 10, Build 2004, para uma função de área de trabalho virtual
description: Configurações e definições recomendadas para minimizar a sobrecarga para desktops Windows 10, versão 2004, usados como imagens da VDI.
ms.prod: windows-server
ms.reviewer: robsmi, timuessi
ms.technology: remote-desktop-services
author: Heidilohr
ms.author: helohr
ms.topic: article
manager: ''
ms.date: 09/24/2020
ms.openlocfilehash: b39207461d9ee99a2da0bdf07a9190c769b7511f
ms.sourcegitcommit: fa74f7297855ff1c9cb7df4b784b946cdce99e22
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91206391"
---
# <a name="optimizing-windows-10-version-2004-for-a-virtual-desktop-infrastructure-vdi-role"></a>Como otimizar o Windows 10, versão 2004, para uma função da VDI (Virtual Desktop Infrastructure)

O objetivo deste artigo é dar sugestões de configurações para o Windows 10, Build 2004, para um desempenho ideal em ambientes de área de trabalho virtualizados, incluindo VDI (Virtual Desktop Infrastructure) e Área de Trabalho Virtual do Windows. Todas as configurações neste guia são apenas configurações de otimização sugeridas e não são obrigatórias.

As informações neste guia são pertinentes ao Windows 10, versão 2004, Build 19041, do SO (sistema operacional).

Os princípios condutores para otimizar o desempenho do Windows 10 em um ambiente de área de trabalho virtual são minimizar redesenhos e efeitos gráficos, atividades em segundo plano que não têm grandes benefícios para o ambiente de área de trabalho virtual e reduzir de um modo geral os processos em execução ao mínimo. Uma meta secundária é reduzir o uso do espaço em disco na imagem base para o mínimo necessário. Com as implementações da área de trabalho virtual, a menor base possível (ou tamanho "ouro" de imagem) pode reduzir ligeiramente a utilização de memória no sistema host, bem como provocar uma pequena redução nas operações gerais de rede necessárias para fornecer o ambiente de área de trabalho ao consumidor.

Nenhuma otimização deve reduzir a experiência do usuário. Cada configuração de otimização foi cuidadosamente examinada para garantir que não haja degradação considerável da experiência do usuário.

> [!NOTE]
> As configurações neste artigo podem ser aplicadas a outras instalações do Windows 10, como a versão 1909, dispositivos físicos ou outras máquinas virtuais. Não há recomendações neste artigo que afetem o suporte do Windows 10 em um ambiente de área de trabalho virtual.

## <a name="vdi-optimization-principles"></a>Princípios da otimização da VDI

Um ambiente de área de trabalho virtual "completo" pode apresentar uma sessão de área de trabalho completa, incluindo aplicativos, a um usuário de computador em uma rede. O veículo de entrega de rede pode ser uma rede local, a Internet ou ambos. Algumas implementações dos ambientes de área de trabalho virtual usam uma imagem de sistema operacional de "base" que se torna o pilar para as áreas de trabalho posteriormente apresentada aos usuários para o trabalho. Há variações das implementações da área de trabalho virtual, como "persistente", "“não persistente" e "sessão da área de trabalho". O tipo persistente preserva as alterações no sistema operacional de desktop virtual de uma sessão para a outra. O tipo não persistente não preserva as alterações no sistema operacional de desktop virtual de uma sessão para a outra. Para o usuário, essa área de trabalho é pouco diferente daquela de outro dispositivo físico ou virtual, exceto pelo fato de que ela é acessada por uma rede.

As configurações de otimização podem ocorrer em um computador de referência. Uma VM (máquina virtual) seria um lugar ideal para criar a VM, pois o estado pode ser salvo, pontos de verificação podem ser feitos, os backups podem ser feitos e assim por diante. Uma instalação padrão do sistema operacional é executada na VM base. Essa VM base é então otimizada com a remoção de aplicativos desnecessários, a instalação das atualizações do Windows, a instalação de outras atualizações, a exclusão de arquivos temporários, aplicação de configurações etc.

Há outros tipos de tecnologia de área de trabalho virtual, como RDS (Sessão da Área de Trabalho Remota) e a [Área de Trabalho Virtual do Windows](https://azure.microsoft.com/services/virtual-desktop/) do Microsoft Azure lançada recentemente. Uma discussão aprofundada sobre essas tecnologias não está no escopo deste artigo. Este artigo se concentra nas configurações da imagem base do Windows, sem referência a outros fatores no ambiente, como a otimização de hardware do host.

A segurança e a estabilidade estão entre as principais prioridades da Microsoft quando se trata de produtos e serviços. No âmbito da área de trabalho virtual, a segurança não é tratada de maneira muito diferente de dispositivos físicos. Os clientes corporativos podem optar por utilizar os serviços internos do Windows da segurança do Windows, que consistem em um conjunto de serviços que funcionam bem conectados ou não conectados à Internet. Para os ambientes de área de trabalho virtual não conectados à Internet, as assinaturas de segurança podem ser baixadas proativamente várias vezes por dia, porque a Microsoft pode lançar mais de uma atualização de assinatura por dia. Essas assinaturas podem então ser fornecidas aos dispositivos da área de trabalho virtual e agendadas para serem instaladas durante a produção, independentemente de serem persistentes ou não persistentes. Dessa forma, a proteção da VM é a mais atual possível.

Há algumas configurações de segurança que não são aplicáveis aos ambientes da área de trabalho virtual que não estão conectados à Internet e, portanto, não podem participar da segurança habilitada para nuvem. Há outras configurações que os dispositivos "normais" do Windows podem utilizar, como a Experiência de Nuvem, a Microsoft Store etc. A remoção do acesso aos recursos não utilizados reduz o volume, a largura de banda da rede e a superfície de ataque.

Sobre atualizações, o Windows 10 utiliza um ritmo de atualização mensal. Em alguns casos, os administradores da área de trabalho virtual controlam o processo de atualização por meio de um processo de desligamento de VMs com base em uma imagem "mestra" ou "ouro", desselam a imagem que é somente leitura, aplicam o patch a ela e, em seguida, selam a imagem novamente e a colocam de novo em produção. Portanto, não há necessidade de ter dispositivos de área de trabalho virtual verificando o Windows Update. No entanto, há casos em que ocorrem procedimentos normais de aplicação de patch, como em dispositivos de área de trabalho virtual "pessoais" persistentes. Em alguns casos, o Windows Update pode ser utilizado. Em alguns casos, o Intune pode ser utilizado. Em alguns casos, o Microsoft Endpoint Configuration Manager (anteriormente SCCM) é utilizado para lidar com a atualização e a outra entrega de pacote. Cabe a cada organização determinar a melhor abordagem para a atualização de dispositivos de área de trabalho virtual enquanto reduz os ciclos de sobrecarga.

As configurações de política local, bem como muitas outras configurações neste guia, podem ser substituídas por uma política baseada em domínio. É recomendável percorrer as configurações de política por completo e remover ou não usar nenhuma que não seja desejada ou aplicável ao seu ambiente. As configurações listadas neste documento tentam alcançar o melhor equilíbrio entre otimização de desempenho em ambientes de área de trabalho virtual mantendo uma experiência de usuário de qualidade.

> [!NOTE]
> Há um conjunto de scripts disponíveis no GitHub.com que executará todos os itens de trabalho documentados neste documento. A URL da Internet para os scripts de otimização pode ser encontrada em [https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool](https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool). Esse script foi projetado para ser facilmente personalizável para seu ambiente e requisitos. O código principal é o PowerShell, e o trabalho é feito chamando arquivos de entrada, que são texto sem formatação (agora .JSON), com o também arquivos de exportação da ferramenta LGPO (Objeto de Política de Grupo Local). Esses arquivos de texto contêm listas dos aplicativos a serem removidos, os serviços a serem desabilitados e assim por diante. Se você não quiser remover um aplicativo específico ou desabilitar um serviço específico, poderá editar o arquivo de texto correspondente e remover o item com relação ao qual você não deseja agir. Por fim, há uma exportação de configurações de política local que podem ser importadas para os computadores do seu ambiente. É melhor ter algumas configurações na imagem base do que aplicar as configurações por meio da Política de Grupo, pois algumas das configurações entram em vigor na próxima reinicialização ou quando um componente é usado pela primeira vez.

### <a name="persistent-virtual-desktop-environments"></a>Ambientes de área de trabalho virtual persistentes

A área de trabalho virtual persistente está no nível básico, um dispositivo que salva o estado do sistema operacional entre as reinicializações. Outras camadas de software da solução de área de trabalho virtual fornecem aos usuários acesso fácil e contínuo às respectivas VMs atribuídas, muitas vezes, com uma solução de logon único.

Há várias implementações diferentes da área de trabalho virtual persistente.

- As VMs tradicionais, em que a VM tem o próprio arquivo de disco virtual, é inicializada normalmente e salva as alterações de uma sessão para a outra. A única diferença está em como o usuário acessa essa VM. Pode haver um portal da Web no qual o usuário faz logon e direciona automaticamente o usuário para uma ou mais VMs (dispositivos de área de trabalho virtual) atribuídas a eles.
- VMs persistentes com base em imagem, opcionalmente com PVDs (discos virtuais pessoais). Nesse tipo de implementação, há uma imagem base/gold em um ou mais servidores host. Uma VM é criada, assim como um ou mais discos virtuais são criados e atribuídos a esse disco para armazenamento persistente.
  - Quando a VM é iniciada, uma cópia da imagem base é lida no espaço de memória da VM. Ao mesmo tempo, um disco virtual persistente é atribuído a essa VM, com todos os deltas do SO anterior mesclados por meio de um processo complexo.
  - Alterações como gravações de log de eventos, gravações de log e assim por diante. são redirecionados para o disco virtual de leitura/gravação atribuído a essa VM.
  - Nessa circunstância, o SO e a manutenção do aplicativo podem funcionar normalmente usando o software de manutenção tradicional, como o Windows Server Update Services ou outras tecnologias de gerenciamento.
- A diferença entre um dispositivo de área de trabalho virtual persistente e um dispositivo de área de trabalho virtual "normal" é a relação com a imagem mestra/ouro. Em algum ponto, as atualizações precisam ser aplicadas ao mestre. É nesse momento que as organizações decidem como as alterações persistentes do usuário são tratadas. Em alguns casos, o disco com as alterações do usuário é descartado ou redefinido. Também pode ocorrer que as alterações ao computador feitas pelo usuário são mantidas por meio de atualizações de qualidade mensal e a base é redefinida após uma atualização de recursos.

### <a name="non-persistent-virtual-desktop-environments"></a>Ambientes de área de trabalho virtual não persistentes

Quando uma implementação de área de trabalho virtual não persistente é baseada em uma imagem base ou "ouro", as otimizações são executadas principalmente na imagem base e por meio das configurações e políticas locais.

Com ambientes de área de trabalho virtual baseados em imagem NP (não persistentes), a imagem base é somente leitura. Quando um dispositivo de área de trabalho virtual NP (VM) é iniciado, uma cópia da imagem base é transmitida para a VM. A atividade que ocorre durante a inicialização e daí em diante até a próxima reinicialização é redirecionada para um local temporário. Normalmente, os usuários recebem locais de rede para armazenar seus dados. Em alguns casos, o perfil do usuário é mesclado com a VM padrão para fornecer aos usuários suas configurações.

Um aspecto importante da área de trabalho virtual NP com base em uma imagem é a manutenção. As atualizações no SO (sistema operacional) e nos componentes do SO geralmente são fornecidas uma vez por mês. Com o ambiente de área de trabalho virtual baseado em imagem, há um conjunto de processos que devem ser executados para obter atualizações à imagem:

- Em um determinado host, todas as VMs nesse host com base na imagem básica devem ser desligadas ou desativadas. Isso significa que os usuários são redirecionados para outras VMs.

- Em algumas implementações, isso é chamado de "drenagem". A máquina virtual ou o host de sessão, quando definido para o modo de descarga, deixa de aceitar novas solicitações, mas continua atendendo aos usuários atualmente conectados ao dispositivo.

- No modo de descarga, quando o último usuário faz logoff do dispositivo, esse dispositivo fica pronto para operações de manutenção.
- A imagem base é aberta e iniciada. Todas as atividades de manutenção são executadas, como atualizações do SO, atualizações do .NET, atualizações de aplicativo etc.
- Todas as configurações novas que precisam ser aplicadas são aplicadas nesse momento.
- Qualquer outra manutenção é realizada nesse momento.
- A imagem base é desligada.
- A imagem base é validada e definida para voltar à produção.
- Os usuários têm permissão para fazer logon novamente.

> [!NOTE]
> O Windows 10 executa, periodicamente, um conjunto de tarefas de manutenção de modo automático. Há uma tarefa agendada que é definida para ser executada, por padrão, às 3h todos os dias. Essa tarefas agendada executa uma lista de tarefas, incluindo a limpeza do Windows Update. Com este comando do PowerShell, você pode exibir todas as categorias de manutenção que ocorrem automaticamente:
>
> ```powershell
> Get-ScheduledTask | Where-Object {$_.Settings.MaintenanceSettings}
> ```

Um dos desafios de uma área de trabalho virtual não persistente é que quase toda a atividade do SO é descartada quando um usuário faz logoff. O estado e/ou o perfil do usuário pode ser salvo em uma localização centralizada, mas a máquina virtual em si descarta quase todas as alterações que foram feitas desde a última reinicialização. Portanto, as otimizações destinadas a um computador Windows que salva o estado de uma sessão para outra não são aplicáveis.

Dependendo da arquitetura do dispositivo da área de trabalho virtual, itens como PreFetch e SuperFetch não vão ajudar de uma sessão para outra, pois todas as otimizações são descartadas na reinicialização da VM. A indexação pode ser um desperdício parcial de recursos, pois seria qualquer otimização de disco, como uma desfragmentação tradicional.

> [!NOTE]
> Se estiver preparando uma imagem usando a virtualização e se estiver conectado à Internet durante o processo de criação da imagem, no primeiro logon, você deverá adiar as atualizações de recursos acessando **Configurações** > **Windows Update**.

### <a name="to-sysprep-or-not-sysprep"></a>Para Sysprep ou não Sysprep

O Windows 10 tem uma funcionalidade interna chamada de [Ferramenta de preparação do sistema](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview), também conhecida como Sysprep. A ferramenta Sysprep é usada para preparar uma imagem personalizada do Windows 10 para duplicação. O processo do Sysprep garante que o sistema operacional resultante seja adequadamente exclusivo para ser executado no ambiente de produção.

Há argumentos a favor e contra a execução do Sysprep. No caso da área de trabalho virtual, talvez seja conveniente a capacidade de personalizar o perfil do usuário padrão que será usado como o modelo de perfil para usuários subsequentes que entram usando essa imagem. Você pode ter os aplicativos que deseja instalados, mas também pode controlar as configurações por aplicativo.

A alternativa é usar um ISO padrão a partir do qual instalar, possivelmente usando um arquivo de resposta de instalação autônoma, bem como uma sequência de tarefas para instalar ou remover aplicativos. Também é possível usar uma sequência de tarefas para definir as configurações da política local, talvez usando a ferramenta [LGPO (Utilitário de Objeto da Política de Grupo Local)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

Para saber mais sobre a preparação de imagem para o Azure, confira [Preparar um VHD do Windows ou VHDX para upload para o Azure](/azure/virtual-machines/windows/prepare-for-upload-vhd-image)

### <a name="supportability"></a>Capacidade de suporte

Sempre que os padrões do Windows são alterados, surgem perguntas em relação à capacidade de suporte. Depois que uma imagem da área de trabalho virtual (VM ou sessão) é personalizada, todas as alterações feitas na imagem precisam ser controladas em um log de alterações. Se chegar a hora de solucionar problemas, muitas vezes uma imagem pode ser isolada em um pool e configurada para análise de problemas. Depois que um problema é acompanhado até a causa raiz, essa alteração pode então ser distribuída para o ambiente de teste primeiro e, finalmente, para a carga de trabalho de produção.

Este documento evita intencionalmente abordar serviços, políticas ou tarefas do sistema que afetam a segurança. Depois disso, vem o Serviço do Windows. A capacidade de atender imagens da área de trabalho virtual fora das janelas de manutenção é removida, pois as janelas de manutenção existem quando a maioria dos eventos de serviço ocorre em ambientes da área de trabalho virtual, com exceção das atualizações de software de segurança. A Microsoft publicou diretrizes para segurança do Windows em ambientes de área de trabalho virtual aqui:

**Microsoft**: [Guia de implantação para o Windows Defender Antivírus em um ambiente de VDI (infraestrutura de área de trabalho virtual)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus)

Considere a capacidade de suporte ao alterar as configurações padrão do Windows. Ocasionalmente, ocorrem problemas difíceis de resolver ao alterar serviços do sistema, políticas ou tarefas agendadas em nome do reforço, da "flexibilização" etc. Consulte a Base de Dados de Conhecimento Microsoft para obter os atuais problemas conhecidos sobre as configurações padrão alteradas. As diretrizes deste documento e o script associado no GitHub serão mantidos com relação a problemas conhecidos, se surgirem. Além disso, você poderá relatar problemas de várias maneiras à Microsoft.

Você pode usar seu mecanismo de pesquisa favorito com os termos `"start value" site:support.microsoft.com` para apresentar problemas conhecidos relacionados aos valores iniciais padrão dos serviços.

Você poderá observar que este documento e os scripts associados no GitHub não modificam nenhuma permissão padrão. Se você estiver interessado em aumentar suas configurações de segurança de modo compatível e robusto, comece com o projeto conhecido como "**AaronLocker**", que pode ser encontrado em [ANUNCIANDO: incluir aplicativos na lista de permissões com o "AaronLocker"](https://blogs.msdn.microsoft.com/aaron_margosis/2018/06/26/announcing-application-whitelisting-with-aaronlocker/)

### <a name="virtual-desktop-optimization-categories"></a>Categorias de otimização de área de trabalho virtual

- Limpeza de aplicativos UWP (Plataforma Universal do Windows)
- Limpeza dos recursos opcionais
- Configurações de política local
- Serviços do sistema
- Tarefas Agendadas
- Aplicar atualizações do Windows (entre outras)
- Rastreamentos automáticos do Windows
- Otimização do Windows Defender com VDI
- Ajuste de desempenho de rede do cliente nas configurações do Registro
- Configurações adicionais das diretrizes de "Linha de Base da Funcionalidade Limitada de Tráfego Restrito do Windows".
- Limpeza de disco

### <a name="universal-windows-platform-uwp-application-cleanup"></a>Limpeza de aplicativos do UWP (Plataforma Universal do Windows)

Uma das metas de uma imagem de área de trabalho virtual é ser o mais leve possível em relação ao armazenamento persistente. Uma forma de reduzir o tamanho da imagem é remover os apps (aplicativos) UWP que não serão usados no ambiente. Com aplicativos UWP, há os principais arquivos de aplicativo, também conhecidos como conteúdo. Há uma pequena quantidade de dados armazenados em cada perfil de usuário para configurações específicas do aplicativo. Também há uma pequena quantidade de dados no perfil "Todos os Usuários".

Além disso, todos os aplicativos UWP são registrados no nível do usuário ou do computador em algum momento após a inicialização do dispositivo e logon para o usuário. Os aplicativos UWP, que incluem o menu iniciar e o Shell do Windows, executam várias tarefas em ou após a instalação, e novamente quando um usuário faz logon pela primeira vez e em uma extensão menor nos logons subsequentes. Para todos os aplicativos UWP, são realizadas há avaliações ocasionais, como:

- Você precisa atualizar o aplicativo para a versão mais recente?
- O aplicativo, se fixado no menu Iniciar, pode ter dados de bloco dinâmicos para baixar
- O aplicativo tem um cache de dados que precisa ser atualizado, como mapas ou clima?
- O aplicativo tem dados persistentes do perfil do usuário que precisam ser apresentados no logon (por exemplo, Notas Autoadesivas)

Com uma instalação padrão do Windows 10, nem todos os aplicativos UWP podem ser usados por uma organização. Portanto, se esses aplicativos forem removidos, haverá necessidade de menos avaliações, menos armazenamento em cache e assim por diante. O segundo método aqui é instruir o Windows a desabilitar as "experiências do consumidor". Isso reduz a atividade de armazenamento ao precisar verificar, para cada usuário, quais aplicativos estão instalados, quais aplicativos estão disponíveis e então começar a baixar alguns aplicativos UWP. A economia de desempenho pode ser significativa quando há centenas ou milhares de usuários, todos começam a trabalhar quase ao mesmo tempo ou até mesmo iniciando o trabalho em horários dinâmicos em vários fusos horários.

Conectividade e tempo são fatores importantes quando se trata da limpeza de aplicativos UWP. Se você implantar a imagem base em um dispositivo sem conectividade de rede, o Windows 10 não poderá se conectar à Microsoft Store e baixará aplicativos e tentará instalá-los enquanto você estiver tentando desinstalá-los. Essa pode ser uma boa estratégia para personalizar a imagem e, em seguida, atualizar o que permanecerá em um estágio posterior do processo de criação da imagem.

Se você modificar seu .WIM base usado para instalar o Windows 10 e remover aplicativos UWP desnecessários do .WIM antes da instalação, os aplicativos não serão instalados do princípio e seus tempos de criação de perfil subsequentes serão mais curtos. Há um link mais adiante nesta seção com informações sobre como remover aplicativos UWP do arquivo .WIM de instalação.

Uma boa estratégia de ambiente de área de trabalho virtual é provisionar os aplicativos que deseja na imagem base e limitar ou bloquear o acesso subsequente à Microsoft Store. Os aplicativos da Store são atualizados periodicamente em segundo plano em computadores normais. Os aplicativos UWP podem ser atualizados durante a janela de manutenção quando outras atualizações são aplicadas.

#### <a name="delete-the-payload-of-uwp-apps"></a>Excluir o conteúdo de aplicativos UWP

Os aplicativos UWP que não são necessários continuam no sistema de arquivos consumindo um pequeno volume de espaço em disco. Em aplicativos que nunca serão necessários, o conteúdo de aplicativos UWP indesejados pode ser removido da imagem base usando comandos do PowerShell. Se você excluir as cargas de aplicativo UWP do arquivo .WIM de instalação usando os links fornecidos mais adiante nesta seção, poderá começar desde o início com uma lista muito enxuta de aplicativos UWP.

Execute o seguinte comando para enumerar os aplicativos UWP provisionados em um SO em execução, como nesta saída de exemplo truncada do PowerShell:

```powershell
Get-AppxProvisionedPackage -Online

DisplayName  : Microsoft.3DBuilder
Version      : 13.0.10349.0  
Architecture : neutral
ResourceId   : \~
PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
Regions      :

DisplayName  : Microsoft.Appconnector
Version      : 2015.707.550  
Architecture : neutral
ResourceId   : \~
PackageName  : Microsoft.Appconnector_2015.707.550.0_neutral_\~_8wekyb3d8bbwe
Regions      :
...
```

Os aplicativos UWP que são provisionados para um sistema podem ser removidos durante a instalação do SO como parte de uma sequência de tarefas ou depois que o sistema operacional estiver instalado. Esse pode ser o método preferido, pois torna modular todo o processo geral de criar ou manter uma imagem. Depois de desenvolver os scripts, se algo mudar em um build subsequente, você edita um script existente em vez de repetir o processo do zero.

Se você quiser saber mais, veja alguns recursos que podem ajudá-lo a:

- [Removendo aplicativos nativos do Windows 10 durante uma sequência de tarefas](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence)
- [Removendo aplicativos internos do arquivo WIM do Windows 10 com o PowerShell – versão 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)
- [Windows 10 1607: impedindo aplicativos de retornarem durante a implantação da atualização de recursos](https://blogs.technet.microsoft.com/mniehaus/2016/08/23/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update/)
- [Removendo aplicativos nativos do Windows 10 durante uma sequência de tarefas](https://blogs.technet.microsoft.com/mniehaus/2015/11/11/removing-windows-10-in-box-apps-during-a-task-sequence/)

Em seguida, execute o seguinte comando do PowerShell para remover as cargas de aplicativo UWP:

```powershell
Remove-AppxProvisionedPackage -Online - PackageName MyAppxPackage
```

Como uma observação final sobre este tópico, cada aplicativo UWP deve ser avaliado quanto à aplicabilidade em cada ambiente exclusivo. Você desejará fazer uma instalação padrão do Windows 10, versão 2004, e observar quais aplicativos estão sendo executados e consumindo memória. Por exemplo, talvez você queira considerar a remoção de aplicativos que são iniciados automaticamente ou de aplicativos que exibem informações automaticamente no menu Iniciar, como Clima e Notícias, e que podem não ser de uso no seu ambiente.

> [!NOTE]
> Se você estiver usando os scripts do GitHub, poderá controlar com facilidade quais aplicativos serão removidos antes de executar o script. Depois de baixar os arquivos de script, localize o arquivo "AppxPackage.json", edite-o e remova as entradas de aplicativos que deseja manter, como Calculadora, Notas Autoadesivas etc.

## <a name="windows-optional-features-cleanup"></a>Limpeza dos recursos opcionais do Windows

### <a name="managing-optional-features-with-powershell"></a>como gerenciar recursos opcionais com o PowerShell

Microsoft: [Windows 10: como gerenciar recursos opcionais com o PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/39386.windows-10-managing-optional-features-with-powershell.aspx)

Gerencie recursos opcionais do Windows usando o PowerShell. Para enumerar os recursos do Windows atualmente instalados, execute o seguinte comando do PowerShell:

```powershell
Get-WindowsOptionalFeature -Online
```

Usando o PowerShell, um recurso opcional enumerado do Windows pode ser configurado como habilitado ou desabilitado, como no seguinte exemplo:

```powershell
Enabled-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

Veja um exemplo de comando que desabilita o recurso do Windows Media Player na imagem da área de trabalho virtual:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName "WindowsMediaPlayer"
```

Em seguida, talvez você queira remover o pacote do Windows Media Player. Este exemplo de comando mostrará como fazer isso:

```powershell

PS C:\> Get-WindowsPackage -Online -PackageName *media*

PackageName              : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153
Applicable               : True
Copyright                : Copyright (c) Microsoft Corporation. All Rights Reserved
Company                  :
CreationTime             :
Description              : Play audio and video files on your local machine and on the Internet.
InstallClient            : DISM Package Manager Provider
InstallPackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153.mum
InstallTime              : 5/11/2020 5:43:37 AM
LastUpdateTime           :
DisplayName              : Windows Media Player
ProductName              : Microsoft-Windows-MediaPlayer-Package
ProductVersion           :
ReleaseType              : OnDemandPack
RestartRequired          : Possible
SupportInformation       : http://support.microsoft.com/?kbid=777777
PackageState             : Installed
CompletelyOfflineCapable : Undetermined
CapabilityId             : Media.WindowsMediaPlayer~~~~0.0.12.0
Custom Properties        :

Features                 : {}
```

Caso deseje remover o pacote do Windows Media Player (para liberar cerca de 60 MB de espaço em disco), execute este comando:

```powershell
PS C:\Windows\system32> Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153 -Online

Path          :
Online        : True
RestartNeeded : False
```

### <a name="enable-of-disabling-windows-features-using-dism"></a>Habilitar a desabilitação de recursos do Windows usando o DISM

Use a ferramenta Dism.exe para enumerar e controlar os recursos opcionais do Windows. Um script do Dism.exe pode ser desenvolvido e executado durante uma sequência de tarefas de instalação do sistema operacional. A tecnologia do Windows envolvida é chamada Recursos sob Demanda. Confira o seguinte artigo para obter mais informações sobre os recursos sob demanda no Windows:

Microsoft: [Recursos sob demanda](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)

### <a name="default-user-settings"></a>Configurações padrão de usuário

Você pode personalizar o arquivo de Registro do Windows em `C:\Users\Default\NTUSER.DAT`. Qualquer alteração de configuração feita nesse arquivo será aplicada a todos os perfis de usuário subsequentes criados em um computador que executa essa imagem. Controle quais configurações aplicar ao perfil do usuário padrão editando o arquivo **DefaultUserSettings.txt**.

Para reduzir a transmissão de dados gráficos na infraestrutura de área de trabalho virtual, você pode definir a tela de fundo padrão como uma cor sólida, em vez da imagem padrão do Windows 10. Você também pode definir a tela de conexão com uma cor sólida, bem como desligar o efeito de desfoque opaco na conexão.

As configurações a seguir são aplicadas ao hive do Registro do perfil do usuário padrão, principalmente para reduzir as animações. Se algumas ou todas essas configurações não forem desejadas, exclua as configurações que você não quer aplicar a novos perfis de usuário com base nesta imagem. O objetivo dessas configurações é habilitar as seguintes configurações equivalentes:

- Mostrar sombras sob o ponteiro do mouse
- Mostrar sombras no Windows
- Suavizar bordas de fontes de tela

![Uma captura de tela do menu de opções de desempenho com os itens relevantes selecionados.](media/performance-options.png)

Além disso, esta versão de configurações apresenta um novo método para desabilitar as duas seguintes configurações de privacidade para qualquer perfil do usuário criado após a execução da otimização:

- Permitir que sites forneçam conteúdo local relevante acessando minha lista de idiomas
- Mostrar conteúdo sugerido no app Configurações

![Uma captura de tela da janela de configurações de privacidade. As duas configurações desabilitadas são realçadas em vermelho.](media/privacy-settings.png)

Veja a seguir as configurações de otimização aplicadas ao hive do Registro do perfil do usuário padrão para otimizar o desempenho. Observe que essa operação é executada primeiro carregando o hive do Registro do perfil do usuário padrão **NTUser.dat**, como o nome da chave efêmera **Temp** e depois fazendo as modificações listadas abaixo:

```regedit
Load HKLM\Temp C:\Users\Default\NTUSER.DAT
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer" /v ShellState /t REG_BINARY /d 240000003C2800000000000000000000 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v IconsOnly /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ListviewAlphaSelect /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ListviewShadow /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowCompColor /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowInfoTip /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v TaskbarAnimations /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects" /v VisualFXSetting /t REG_DWORD /d 3 /f
add "HKLM\Temp\Software\Microsoft\Windows\DWM" /v EnableAeroPeek /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\DWM" /v AlwaysHiberNateThumbnails /t REG_DWORD /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v DragFullWindows /t REG_SZ /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v FontSmoothing /t REG_SZ /d 2 /f
add "HKLM\Temp\Control Panel\Desktop" /v UserPreferencesMask /t REG_BINARY /d 9032078010000000 /f
add "HKLM\Temp\Control Panel\Desktop\WindowMetrics" /v MinAnimate /t REG_SZ /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy" /v 01 /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338393Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-353694Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-353696Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338389Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Control Panel\International\User Profile" /v HttpAcceptLanguageOptOut /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitInkCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitTextCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Personalization\Settings" /v AcceptedPrivacyPolicy /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization\TrainedDataStore" /v HarvestContacts /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
Unload HKLM\Temp
```

Outra série de configurações de usuário padrão adicionadas recentemente é desabilitar a inicialização e a execução de vários aplicativos do Windows em segundo plano. Embora não seja significativo em um dispositivo individual, o Windows 10 inicia vários processos para cada sessão de usuário em um determinado dispositivo (host). O aplicativo Skype é um exemplo e o Microsoft Edge é outro. As configurações incluídas desligam a capacidade de execução em segundo plano de vários aplicativos. Se essa funcionalidade for desejada como está, exclua as linhas no arquivo "DefaultUserSettings.txt" que incluem os nomes de aplicativo "**Windows.Photos**", "**SkypeApp**", "**YourPhone**" e/ou "**MicrosoftEdge**".

### <a name="local-policy-settings"></a>Configurações de política local

Muitas otimizações para Windows 10 em um ambiente da área de trabalho virtual podem ser feitas usando a política do Windows. As configurações listadas na tabela desta seção podem ser aplicadas localmente à imagem base/ouro. Se as configurações equivalentes não forem especificadas de nenhuma outra maneira, como a política de grupo, elas ainda serão aplicadas.

Observe que algumas decisões podem se basear nas especificidades do ambiente.

- O ambiente de área de trabalho virtual tem permissão para acessar a Internet?
- A solução de área de trabalho virtual é persistente ou não persistente?

As configurações a seguir foram escolhidas para não contrariar nem entrar em conflito com qualquer configuração relacionada à segurança. Essas configurações foram escolhidas para remover as configurações ou desabilitar as funcionalidades que possam não ser aplicáveis aos ambientes da área de trabalho virtual.

| Configuração de política | Item | Subitem | Configuração possível e comentários |
|--------------|----|--------|----------------------------|
| Política do Computador Local \\ Configuração do Computador \\ Configurações do Windows \\ Configurações de Segurança | N/D | N/D | N/D |
| Políticas do Gerenciador de Listas de Redes | Todas as propriedades de redes | Local da rede | **O usuário não pode alterar a localização** (Isso é definido para evitar o pop-up do lado direito quando uma nova rede é detectada) |
| Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Painel de Controle | N/D | N/D |
| Painel de Controle | Permitir dicas online | N/D  | **Desabilitado** (configurações não contatarão os serviços de conteúdo da Microsoft para recuperar dicas e conteúdo de ajuda) |
| Painel de Controle\Personalização | (Forçar uma imagem de logon e tela de bloqueio padrão específica) | N/D | Habilitado (Essa configuração permite forçar uma tela de bloqueio padrão específica e uma imagem de logon inserindo o caminho (localização) do arquivo de imagem. A mesma imagem será usada para as telas de bloqueio e de logon. <p>O motivo para essa recomendação é reduzir os bytes transmitidos pela rede para ambientes de área de trabalho virtual. Essa configuração pode ser removida ou personalizada para cada ambiente.)|
|Painel de Controle\Opções Regionais e de Idioma\Personalização de manuscrito|Desativar aprendizado automático| N/D |**Habilitado** (com essa configuração de política habilitada, o aprendizado automático é interrompido e os dados armazenados são excluídos. Os usuários não podem definir essa configuração no Painel de Controle).|
|Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Rede|N/D|N/D|N/D|
|BITS|Permitir cache de sistemas pares do BITS| N/D |**Desabilitado** (Essa configuração de política determina se o recurso de cache de pares de Serviço de Transferência Inteligente em Segundo Plano (BITS) está habilitado em um computador específico.)|
|BITS|Não permitir que o cliente BITS use o Windows Branch Cache|N/D|**Habilitado** (Com essa configuração de política habilitada, o cliente BITS não usa o Windows Branch Cache.)<p>O motivo para essa recomendação é que os dispositivos de área de trabalho virtual não sejam usados para o cache de conteúdo, e os dispositivos não terão permissão para usar a largura de banda da rede para fazer isso.|
|BITS|Não permitir que o computador aja como um cliente de cache de sistemas pares do BITS|N/D|**Habilitado** (Com essa configuração de política habilitada, o computador não usará mais o recurso de cache de pares de BITS para baixar arquivos. Os arquivos serão baixados somente do servidor de origem.)|
BITS|Não permitir que o computador aja como um servidor de cache de sistemas pares do BITS|N/D|**Habilitado** (Com essa configuração de política habilitada, o computador não armazenará em cache os arquivos baixados e os oferecerá a seus pares.)|
|BranchCache|Habilitar o BranchCache|N/D|**Desabilitado** (Com essa seleção desabilitada, o BranchCache é desativado para todos os computadores cliente nos quais a política é aplicada.)|
|*Fontes|Provedores de fontes habilitados|N/D|**Desabilitado** (com essa configuração desabilitada, o Windows não se conecta a um provedor de fontes online e enumera apenas as fontes instaladas localmente)|
|Autenticação de hotspot|Habilitar a autenticação do hotspot| N/D |**Desabilitado** (Essa configuração de política define se os pontos de acesso à WLAN são investigados para suporte de protocolo de roaming do WISPr (provedor de serviços de Internet sem fio). Com essa configuração de política desabilitada, os hotspots WLAN não são investigados quanto ao suporte do protocolo WISPr, e os usuários só podem se autenticar com hotspots WLAN usando um navegador da Web.)|
|Serviços de rede ponto a ponto da Microsoft|Desligar os Serviços de rede ponto a ponto da Microsoft|N/D|**Habilitado** (Essa configuração desativa os Serviços de Rede Ponto a Ponto da Microsoft em sua totalidade e fará com que todos os aplicativos dependentes parem de funcionar. Se você habilitar essa configuração, os protocolos ponto a ponto serão desativados.)|
|Indicador de status da conectividade de rede<p>(Há outras configurações nesta seção que podem ser usadas em redes isoladas)|Especificar a sondagem passiva|Desabilitar a sondagem passiva (**caixa de seleção**)|**Habilitado** (Essa configuração de política permite que você especifique o comportamento de sondagem passiva. O NCSI sonda várias medições em toda a pilha de rede em um intervalo frequente para determinar se a conectividade de rede foi perdida. Use as opções para controlar o comportamento de sondagem passiva.)<P>Desabilitar a sondagem passiva do NCIS pode aprimorar a carga de trabalho da CPU em servidores ou em outros computadores cuja conectividade de rede é estática.|
|Arquivos Offline|Permitir ou não permitir o uso do recurso Arquivos Offline|N/D|**Desabilitado** (Essa configuração de política determina se o recurso Arquivos Offline está habilitado. Arquivos Offline salva uma cópia dos arquivos de rede no computador do usuário para uso quando o computador não está conectado à rede. Com essa configuração de política desabilitada, o recurso Arquivos Offline é desabilitado e os usuários não podem habilitá-lo.)|
|*Configurações TCPIP\Tecnologias de Transição IPv6| Definir o Estado Teredo|Estado Desabilitado|**Habilitado** (Com essa configuração habilitada e definida como "Estado Desabilitado", nenhuma interface Teredo está presente no host)|
*Serviço WLAN\ Configurações de WLAN|Permitir que o Windows se conecte automaticamente a hotspots abertos sugeridos, redes compartilhadas por contatos e hotspots que oferecem serviços pagos|N/D|**Desabilitado** (Essa configuração de política determina se os usuários podem habilitar as seguintes configurações de WLAN: "Conectar-se a hotspots abertos sugeridos", "Conectar-se a redes compartilhadas por meus contatos" e "Habilitar serviços pagos". Com essa configuração de política desabilitada, as opções "Conectar a hotspots abertos sugeridos", "Conectar a redes compartilhadas por meus contatos" e "Habilitar serviços pagos" serão desativadas e os usuários nesse dispositivo serão impedidos de habilitá-las.)|
|Serviço de WWAN\Acesso a Dados de Celular|Permitir que os aplicativos do Windows acessem dados da rede celular|Padrão para todos os aplicativos: **Forçar Negação**|**Habilitado** (Se você escolher a opção "Forçar Negação", os aplicativos do Windows não terão permissão para acessar dados da rede celular e os usuários não poderão alterá-la.)|
|Política do Computador Local\Configuração do Computador\Modelos Administrativos\Menu Iniciar e Barra de Tarefas|N/D|N/D|
|*Notificações|Desabilitar o uso da rede para notificações| N/D |**Habilitado** (Com essa configuração de política habilitada, os aplicativos e recursos do sistema não poderão receber notificações da rede do WNS ou por APIs de sondagem de notificação)|
|Política do Computador Local\Configuração do Computador\Modelos Administrativos\Sistema| N/D | N/D |N/D|
|Instalação de dispositivos|Não enviar um Relatório de Erros do Windows quando um driver genérico for instalado em um dispositivo| N/D |**Habilitado** (Com essa configuração de política habilitada, um relatório de erros não é enviado quando um driver genérico é instalado.)|
|Instalação de dispositivo|Impedir a criação de um ponto de restauração do sistema durante atividade do dispositivo que normalmente solicitaria essa criação| N/D |**Habilitado** (Com essa configuração de política habilitada, o Windows não cria um ponto de restauração do sistema quando um normalmente seria criado.)|
|Instalação de dispositivo|Impedir a recuperação de metadados do dispositivo da Internet| N/D |**Habilitado** (Essa configuração de política permite impedir que o Windows recupere os metadados do dispositivo da Internet. Com essa configuração de política habilitada, o Windows não recupera os metadados de dispositivo para dispositivos instalados da Internet. Essa configuração de política substitui a configuração na caixa de diálogo Configurações de Instalação do Dispositivo (Painel de Controle > Sistema e Segurança > Sistema > Configurações Avançadas do Sistema > Guia Hardware).)|
|Instalação de dispositivo|Desligar balões de "Novo Hardware Encontrado" durante instalação de dispositivos| N/D |**Habilitado** (Essa configuração de política permite que você desligue os balões de "Novo Hardware Encontrado" durante a instalação do dispositivo. Com essa configuração de política habilitada, os balões "Novo Hardware Encontrado" não aparecem enquanto um dispositivo está sendo instalado.)|
|Sistema de Arquivos\NTFS|Opções de criação de nome curto|Opções de criação de nome curto: Desabilitado em todos os volumes|**Habilitado** (Essas configurações fornecem controle sobre se os nomes curtos são ou não gerados durante a criação do arquivo. Alguns aplicativos exigem nomes curtos para compatibilidade, mas nomes curtos têm um impacto negativo sobre o desempenho do sistema. Com nomes curtos desabilitados em todos os volumes, eles nunca serão gerados.)|
|*Política de grupo|Continuar experiências neste dispositivo| N/D |**Desabilitado** (Essa configuração de política determina se o dispositivo Windows tem permissão para participar de experiências entre dispositivos (continuar experiências). Desabilitar essa política impede que este dispositivo seja detectável por outros dispositivos e, portanto, não pode participar de experiências entre dispositivos.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar links "Events.asp" do Visualizador de Eventos| N/D |**Habilitado** (Essa configuração de política especifica se os hiperlinks "Events.asp" estão disponíveis para eventos dentro do aplicativo Visualizador de Eventos.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar compartilhamento de dados de personalização de manuscrito| N/D |**Habilitado** (Desativa o compartilhamento de dados da ferramenta de personalização de reconhecimento de manuscrito.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar relatório de erros de reconhecimento de manuscrito| N/D |**Habilitado** (Desativa a ferramenta de relatório de erros de reconhecimento de manuscrito.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar pesquisa do Centro de Ajuda e Suporte na Base de Dados de Conhecimento Microsoft| N/D |**Habilitado** (Essa configuração de política especifica se os usuários podem executar uma pesquisa da Base de Dados de Conhecimento Microsoft no Centro de Ajuda e Suporte.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desligar Assistente de Conexão com a Internet se a URL de conexão fizer referência a Microsoft.com| N/D |**Habilitado** (Essa configuração de política especifica se o assistente de conexão com a Internet pode se conectar à Microsoft para baixar uma lista de ISPs (provedores de serviços de Internet).)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar o download da Internet para assistentes de publicação na Web e de encomendas online| N/D |**Habilitado** (Essa configuração de política especifica se o Windows deve baixar uma lista de provedores para os assistentes de publicação na Web e de ordenação online.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar o serviço de associação de arquivo pela Internet| N/D |**Habilitado** (Essa configuração de política especifica se o serviço Web da Microsoft deve ser usado para localizar um aplicativo para abrir um arquivo com uma associação de arquivo sem tratamento.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar Registro se a URL de conexão fizer referência a Microsoft.com| N/D |**Habilitado** (Essa configuração de política especifica se o assistente de registro do Windows se conecta ao Microsoft.com para registro online.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desligar atualizações de arquivo de conteúdo do Search Companion| N/D |**Habilitado** (Essa configuração de política especifica se o Search Companion deve baixar automaticamente as atualizações de conteúdo durante as pesquisas locais e da Internet.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar a tarefa "Encomendar Cópias" de fotos| N/D |**Habilitado** (Se você habilitar essa configuração de política, a tarefa "Solicitar Impressões Online" será removida das Tarefas de Imagem nas pastas do Explorador de Arquivos.)|
Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar a tarefa "Publicar na Web" de arquivos e pastas| N/D |**Habilitado* (Essa configuração de política especifica se as tarefas "Publicar este arquivo na Web", "Publicar esta pasta na Web" e "Publicar os itens selecionados na Web" estão disponíveis em Tarefas de Arquivo e Pasta nas pastas do Windows.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar o Programa de Aperfeiçoamento da Experiência do Usuário do Windows| N/D |**Habilitado** (O CEIP (Programa de Aperfeiçoamento da Experiência do Usuário) do Windows coleta informações sobre sua configuração de hardware e sobre como você usa nosso software e serviços para identificar tendências e padrões de uso. Se você habilitar essa configuração de política, todos os usuários serão recusados pelo Programa de Aperfeiçoamento da Experiência do Usuário do Windows.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar Relatório de Erros do Windows| N/D |**Habilitado** (Essa configuração de política controla se os erros são ou não relatados à Microsoft. Se você habilitar essa configuração de política, os usuários não receberão a opção de relatar erros.)|
|Gerenciamento de Comunicação da Internet\Configurações de Comunicação da Internet|Desativar a procura por drivers de dispositivo no Windows Update| N/D |**Habilitado** (Essa configuração de política especifica se o Windows pesquisará drivers de dispositivos no Windows Update quando não houver drivers locais para um dispositivo. Se você habilitar essa configuração de política, a Windows Update não será pesquisada quando um novo dispositivo for instalado.)|
|Logon|Não exibir a tela de boas-vindas de Introdução no logon| N/D |**Habilitado** (Com essa configuração habilitada, a tela de boas-vindas fica oculta do usuário que está fazendo logon em um dispositivo Windows.)|
|Logon|Não enumerar usuários conectados em computadores ingressados no domínio| N/D |**Habilitado** (Com essa configuração habilitada, a interface do usuário de logon não enumerará nenhum usuário conectado em computadores ingressados no domínio.)|
|Logon|Enumerar usuários locais em computadores ingressados no domínio| N/D |**Desabilitado** (Com essa configuração desabilitada, a interface do usuário de logon não enumerará usuários locais em computadores ingressados no domínio.)|
|Logon|Mostrar tela de fundo de logon| N/D |**Habilitado** (Essa configuração de política desabilita o efeito de desfoque acrílico na imagem da tela de fundo do logon. Com essa configuração habilitada, a imagem da tela de fundo de logon é mostrada sem desfoque.)|
|Logon|Mostrar animação de primeira entrada| N/D |**Desabilitado** (Essa configuração de política permite que você controle se os usuários veem a primeira animação de entrada ao entrarem no computador pela primeira vez. Isso se aplica ao primeiro usuário do computador que conclui a configuração inicial e os usuários que são adicionados ao computador mais tarde. Ele também controla se usuários da conta Microsoft receberão o prompt de aceitação para serviços durante sua primeira entrada.<p>Com essa configuração desabilitada, os usuários não verão a primeira animação de logon e conta Microsoft os usuários não verão o prompt de aceitação para serviços.)|
|Logon|Desativar notificações de aplicativos na tela de bloqueio| N/D |**Habilitado** (Essa configuração de política permite impedir que as notificações do aplicativo apareçam na tela de bloqueio. Com essa configuração habilitada, nenhuma notificação de aplicativo é exibida na tela de bloqueio.)|
|Gerenciamento de energia|Selecionar um plano de energia ativo|Plano de energia ativo: Alto Desempenho|**Habilitado** (Se você habilitar essa configuração de política, especifique um plano de energia na lista Plano de Energia Ativo.) <p>Com o serviço de "Energia" desabilitado, a interface do usuário powercfg.cpl não pode exibir essas opções de energia e, em vez disso, retorna um erro de RPC.|
|Gerenciamento de Energia\Configurações de Vídeo e Exibição|Ativar apresentação de slides em segundo plano da área de trabalho (conectado)| N/D |**Desabilitado** (Essa configuração de política permite que você especifique se o Windows deve habilitar a apresentação de slides em segundo plano da área de trabalho.) Com essa configuração desabilitada, a apresentação em segundo plano da área de trabalho está desabilitada. Essa configuração provavelmente não tem nenhum efeito em uma VM.|
|Recuperação|Permitir restauração do sistema ao estado padrão| N/D |**Desabilitado** (Com essa configuração desabilitada, os itens "Usar uma imagem do sistema criada anteriormente para recuperar o computador" e "Reinstalar o Windows" (ou "Retornar o computador para a condição de fábrica") em recuperação (no painel de controle) ficarão não disponíveis.)|
|*Integridade de Armazenamento|Permitir o download de atualizações para o Modelo de Previsão de Falha de Disco| N/D |**Desabilitado** (As atualizações não serão baixadas para o Modelo de Falha de Previsão de Falha de Disco)|
|Restauração do Sistema|Desligar Restauração do Sistema| N/D |**Habilitado** (Com essa configuração habilitada, a restauração do sistema está desativada e o assistente de restauração do sistema não pode ser acessado. A opção de configurar a restauração do sistema ou criar um ponto de restauração por meio da proteção do sistema também está desabilitada.).)|
|Solução de Problemas e Diagnóstico\Manutenção Agendada|Configurar Comportamento de Manutenção Programada| N/D |**Desabilitado** (Determina se o diagnóstico agendado será executado para detectar e resolver problemas do sistema proativamente. Com essa configuração de política desabilitada, o Windows não conseguirá detectar, solucionar problemas ou solucionar o problema de modo programado.)|
|Solução de Problemas e Diagnósticos\Diagnósticos em Script|Solução de problemas: Permitir que os usuários acessem e executem assistentes de solução de problemas| N/D |**Desabilitado** (Com essa configuração desabilitada, os usuários não podem acessar nem executar as ferramentas de solução de problemas no painel de controle.)|
|Solução de Problemas e Diagnósticos\Diagnósticos em Script|Solução de problemas: Permitir que os usuários acessem o conteúdo de solução de problemas online em servidores Microsoft no painel de controle de solução de problemas (por meio do WOTS [Serviço Online de Solução de Problemas do Windows])| N/D |**Desabilitado** (Com essa configuração desabilitada, os usuários só poderão acessar e pesquisar o conteúdo de solução de problemas disponível localmente nos computadores deles, mesmo se estiverem conectados à Internet. Eles são impedidos de se conectar aos servidores da Microsoft que hospedam o serviço de solução de problemas do Windows online.|
|Solução de Problemas e Diagnóstico\Diagnóstico de Desempenho de Inicialização do Windows|Configurar Nível de Execução do Cenário| N/D |**Desabilitado** (Determina o nível de execução do Diagnóstico de Desempenho de Inicialização do Windows. Se você desabilitar essa configuração de política, o Windows não poderá detectar, solucionar problemas nem resolver problemas de Desempenho de Inicialização do Windows tratados pelo DPS.)<p>Essa configuração pode ser muito útil durante as fases de design, teste, desenvolvimento ou manutenção. Essa configuração pode ser habilitada em um host de sessão ou VM isolada, medições tomadas e resultados observados em logs de eventos na origem "Microsoft-Windows-Diagnostics-performance/Operational": Diagnóstico-desempenho, categoria de tarefa "Monitoramento de desempenho de inicialização".<p>**TAMBÉM**: Com o serviço DPS desabilitado, essa configuração não tem efeito, pois o Windows não registrará os dados de desempenho.|
|Solução de Problemas e Diagnóstico\Diagnóstico de Perda de Memória do Windows|Configurar Nível de Execução do Cenário| N/D |**Desabilitado** (Essa configuração de política determina se o DPS (serviço de política de diagnóstico) diagnostica problemas de vazamento de memória. Com essa configuração desabilitada, o DPS não é capaz de diagnosticar problemas de vazamento de memória.) <p>Muitos modos de diagnóstico podem ser habilitados e ferramentas usadas como WPT, embora isso geralmente seja feito em cenários de desenvolvimento/teste/manutenção e não habilitado e usado em sessões ou VMs de produção|
|Solução de Problemas e Diagnósticos\PerfTrack de Desempenho do Windows|Habilitar/desabilitar PerfTrack| N/D |**Desabilitado** (Essa configuração de política especifica se o acompanhamento de eventos de capacidade de resposta deve habilitado ou desabilitado. Com essa configuração desabilitada, os eventos de capacidade de resposta não são processados.)|
|Solução de Problemas e Diagnóstico\Detecção e Resolução de Esgotamento de Recursos do Windows|Configurar Nível de Execução do Cenário| N/D |**Desabilitado** (Determina o nível de execução para Detecção e Resolução de Esgotamento de Recursos do Windows. Com essa configuração desabilitada, o Windows não será capaz de detectar, solucionar problemas ou resolver problemas de esgotamento de recursos do Windows que são tratados pelo DPS.)|
|Solução de Problemas e Diagnóstico\Diagnóstico de Desempenho de Desligamento do Windows|Configurar Nível de Execução do Cenário| N/D |**Desabilitado** (Determina o nível de execução do Diagnóstico de Desempenho de Desligamento do Windows. Com essa configuração desabilitada, o Windows não será capaz de detectar, solucionar problemas ou resolver problemas de Desempenho de Desligamento do Windows que são tratados pelo DPS.)|
|Solução de Problemas e Diagnóstico\Diagnóstico de Desempenho do Windows em Espera/Reiniciado|Configurar Nível de Execução do Cenário| N/D |**Desabilitado** (Determina o nível de execução do Diagnóstico de Desempenho do Windows em Espera/Reiniciado. Com essa configuração desabilitada, o Windows não será capaz de detectar, solucionar problemas ou resolver problemas de Desempenho em Espera/Reiniciado do Windows que são tratados pelo DPS.)|
|Solução de Problemas e Diagnóstico\Diagnóstico do Desempenho da Capacidade de Resposta do Sistema Windows|Configurar Nível de Execução do Cenário| N/D |**Desabilitado** (Determina o nível de execução do Diagnóstico de Responsividade do Sistema Windows. Com essa configuração desabilitada, o Windows não será capaz de detectar, solucionar problemas ou resolver problemas de Responsividade do Sistema do Windows que são tratados pelo DPS.)|
*Perfis do Usuário|Desativar a ID de anúncio| N/D |**Habilitado** (Com essa configuração habilitada, a ID de anúncio é desativada. Os aplicativos não podem usar a ID para experiências entre aplicativos)|
|Política do Computador Local\Configuração do Computador\Modelos Administrativos\Componentes do Windows| N/D | N/D | N/D |
|*Privacidade do aplicativo|Permitir que aplicativos do Windows acessem informações de diagnóstico sobre outros aplicativos|Padrão para todos os aplicativos: Forçar Negação|**Habilitado** (Com essa configuração habilitada e usando a opção "Forçar Negação", os aplicativos do Windows não têm permissão para obter informações de diagnóstico sobre outros aplicativos e os funcionários em sua organização não podem alterá-la.)|
|*Privacidade do aplicativo|Permitir que os aplicativos do Windows acessem a localização|Padrão para todos os aplicativos: Forçar Negação|**Habilitado** (Com essa configuração habilitada e usando a opção "Forçar Negação", os aplicativos do Windows não têm permissão para acessar a localização e os usuários não podem alterar a configuração.|
|*Privacidade do aplicativo|Permitir que aplicativos do Windows acessem movimento|Padrão para todos os aplicativos: Forçar Negação|**Habilitado** (Com essa configuração habilitada e usando a opção "Forçar Negação", os aplicativos do Windows não têm permissão para acessar dados de movimento e os usuários não podem alterar a configuração.)|
|*Privacidade do aplicativo|Permitir que aplicativos do Windows acessem notificações|Padrão para todos os aplicativos: Forçar Negação|**Habilitado** (Com essa configuração habilitada e usando a opção "Forçar Negação", os aplicativos do Windows não têm permissão para acessar notificações e os usuários não podem alterar a configuração)|
|*Privacidade do aplicativo|Permitir a ativação de aplicativos do Windows por voz|Padrão para todos os aplicativos: Forçar Negação|**Habilitado** (Essa configuração de política especifica se os aplicativos do Windows podem ser ativados por voz.)|
|*Privacidade do aplicativo|Permitir a ativação de aplicativos do Windows por voz enquanto o sistema está bloqueado|Padrão para todos os aplicativos: Forçar Negação|**Habilitado** (Esta configuração de política especifica se os aplicativos do Windows podem ser ativados por voz enquanto o sistema está bloqueado.)|
|*Privacidade do aplicativo|Let Windows apps control radios|Padrão para todos os aplicativos: Forçar Negação|**Habilitado** (Se você escolher a opção "Forçar Negação", os aplicativos do Windows não terão acesso ao controle de rádios e os funcionários da organização não poderão alterá-la)|
|Compatibilidade de aplicativos|Desligar Coletor de Inventário| N/D |**Habilitado** (Essa configuração de política controla o estado do Coletor de Inventário. O Coletor de Inventário inventaria aplicativos, arquivos, dispositivos e drivers no sistema e envia as informações à Microsoft. Com essa configuração de política habilitada, o Coletor de Inventário será desativado e nenhum dado será enviado à Microsoft. A coleta de dados de instalação por meio do Auxiliar de Compatibilidade de Programas também está desabilitada.)|
|Políticas de Reprodução Automática|Definir o comportamento padrão de Execução Automática|Não executar comandos de execução automática|**Habilitado** (Essa configuração de política define o comportamento padrão para comandos Autorun.)|
|*Políticas de Reprodução Automática|Desativar Reprodução Automática|Todas as unidades|**Habilitado** (Se você habilitar essa configuração de política, a reprodução automática será desabilitada em todas as unidades.)|
|*Conteúdo de nuvem|Não mostrar dicas do Windows| N/D |**Habilitado** (Essa configuração de política impede que dicas do Windows sejam mostradas aos usuários)|
|*Conteúdo de nuvem|Desativar as experiências do cliente da Microsoft| N/D |**Habilitado** (Com essa configuração de política habilitada, os usuários não verão mais recomendações personalizadas da Microsoft e notificações sobre a respectiva conta Microsoft)|
|*Coleta de Dados e Versões Prévias|Permitir Telemetria|0 – Segurança [Somente Empresa]|**Habilitado** (Definir um valor de 0 aplica-se somente a dispositivos que executam as edições Enterprise, Education, IoT ou Windows Server e reduz a telemetria enviada para o nível mais básico com suporte)|
|Coleta de dados e versões prévias|Configurar a coleta de dados de navegação para Análise de Área de Trabalho|Configurar a coleta de telemetria: Não permitir o envio de histórico de intranet ou Internet|**Habilitado** (Você pode configurar o Microsoft Edge para enviar somente o histórico da intranet, somente o histórico da Internet ou ambos para a Análise de Área de Trabalho para dispositivos corporativos com uma ID comercial configurada. Se estiver desabilitado ou não configurado, o Microsoft Edge não enviará dados de histórico de navegação para a Análise de Área de Trabalho.)|
|*Coleta de Dados e Versões Prévias|Não mostrar notificações de comentários| N/D |**Habilitado** (Essa configuração de política permite que uma organização impeça que seus dispositivos mostrem perguntas de comentários da Microsoft.)|
|Otimização de Entrega|Modo de download|Modo de Download: Simples (99)|**Habilitado** (99 = modo de download simples sem emparelhamento. A Otimização de Entrega baixa usando apenas HTTP e não tenta contatar os serviços de nuvem da Otimização de Entrega.)|
|Gerenciador de Janelas da Área de Trabalho|Não permitir animações de janelas| N/D |**Habilitado** (Essa configuração de política controla a aparência de animações de janelas, como aquelas encontradas durante a restauração, a minimização e a maximização do Windows. Com essa configuração de política habilitada, as animações de janela são desativadas.)|
|Gerenciador de Janelas da Área de Trabalho|Use cor sólida para o fundo da tela inicial| N/D |**Habilitado** (Essa configuração de política controla os visuais da tela de fundo de Iniciar. Com essa configuração de política habilitada, a tela de fundo de Iniciar usará uma cor sólida.)|
|UI Borda|Permitir passar o dedo na borda| N/D |**Desabilitado** (Se você desabilitar essa configuração de política, os usuários não poderão invocar nenhuma interface do usuário do sistema passando o dedo de qualquer borda da tela.)|
|UI Borda|Desabilitar dicas de ajuda| N/D |**Habilitado** (Se essa configuração estiver habilitada, o Windows não mostrará dicas de ajuda para o usuário.)|
|Explorador de Arquivos|Não mostrar a notificação de "novo aplicativo instalado"| N/D |**Habilitado** (Essa política remove a notificação do usuário final para novas associações de aplicativo. Essas associações se baseiam em tipos de arquivo (por exemplo, arquivos TXT) ou protocolos (por exemplo, HTTP). Se essa política estiver habilitada, nenhuma notificação será mostrada para o usuário final)|
|Histórico de arquivos|Desligar o histórico de arquivos| N/D |**Habilitado** (Com essa configuração de política habilitada, o Histórico de Arquivos não pode ser ativado para criar backups regulares e automáticos.)|
|*Localizar meu dispositivo|Ativar/desativar Localizar Meu Dispositivo| N/D |**Desabilitado** (Quando a opção Localizar Meu Dispositivo está desligada, o dispositivo e a localização dele não são registrados e o recurso não funciona. O usuário também não poderá exibir o local do último uso de seu digitalizador ativo no dispositivo.)|
|Homegroup|Impedir que o computador ingresse em um grupo doméstico| N/D |**Habilitado** (Se você habilitar essa configuração de política, os usuários não poderão adicionar computadores a um grupo doméstico. Essa configuração de política não afeta outros recursos de compartilhamento de rede.)|
|*Internet Explorer|Permitir que os serviços Microsoft forneçam sugestões avançadas enquanto o usuário digita algo na barra de endereços| N/D |**Desabilitado** (Os usuários não receberão sugestões avançadas ao digitar na barra de endereços. Além disso, os usuários não poderão alterar a configuração de Sugestões)|
|Internet Explorer|Desabilitar a verificação periódica de atualizações de software do Internet Explorer| N/D |**Habilitado** (Impede que o Internet Explorer verifique se uma nova versão do navegador está disponível.)|
|Internet Explorer|Desabilitar a exibição da tela inicial| N/D |**Habilitado** (Impede que a tela inicial do Internet Explorer apareça quando os usuários iniciam o navegador.)|
|Internet Explorer|Instalar novas versões do Internet Explorer automaticamente| N/D |**Desabilitado** (Esta configuração de política define que novas versões do Internet Explorer deverão ser instaladas automaticamente quando estiverem disponíveis.)|
|Internet Explorer|Impedir participação no Programa de Aperfeiçoamento da Experiência do Usuário| N/D |**Habilitado** (Essa configuração de política impede que o usuário participe do Programa de Aperfeiçoamento da Experiência do Usuário (CEIP).)|
|Internet Explorer|Impedir a execução do Assistente de Primeira Execução|Ir direto para a home page|**Habilitado** (Essa configuração de política impede que o Internet Explorer execute o assistente de primeira execução na primeira vez que um usuário iniciar o navegador depois de instalar o Internet Explorer ou o Windows.)|
|Internet Explorer|Definir crescimento de processos de guias|Baixa|**Habilitado** (Essa configuração de política permite que você defina a taxa na qual o Internet Explorer cria processos de guia.)|
|Internet Explorer|Desativar notificações de desempenho de complementos| N/D |**Habilitado** (Essa configuração de política impede que o Internet Explorer exiba uma notificação quando o tempo médio para carregar todos os complementos habilitados pelo usuário excede o limite.)|
|Internet Explorer|Desligar Recuperação Automática de Pane| N/D |**Habilitado** (Essa configuração de política desativa a Recuperação Automática de Pane. Com essa configuração de política habilitada, a Recuperação Automática de Pane não envia um prompt para que o usuário recupere seus dados depois que um programa para de responder.)|
|*Internet Explorer|Desativar a geolocalização do navegador| N/D |**Habilitado** (Com essa configuração de política habilitada, o suporte à localização geográfica do navegador será desativado)|
|*Internet Explorer|Ativar o recurso Sites Sugeridos| N/D |**Desabilitado** (Com essa configuração de política desabilitada, os pontos de entrada e a funcionalidade associada a esse recurso serão desativados.)|
|Internet Explorer\Painel de Controle da Internet\Página Avançada|Reproduzir animações em páginas da Web| N/D |**Desabilitado** (Essa configuração de política permite que você gerencie se o Internet Explorer exibirá imagens animadas encontradas no conteúdo da Web. Geralmente, somente arquivos GIF animados são afetados por essa configuração. Conteúdo da Web ativo, como miniaplicativos Java, não são.)|
|Internet Explorer\Painel de Controle da Internet\Página Avançada|Reproduzir sons em páginas da Web| N/D |**Desabilitado** (Com essa configuração de política desabilitada, o Internet Explorer não reproduzirá nem baixará sons no conteúdo da Web, ajudando na exibição mais rápida das páginas.)|
|Internet Explorer\Painel de Controle da Internet\Página Avançada|Executar vídeos em páginas da Web| N/D |**Desabilitado** (Se você desabilitar essa configuração de política, o Internet Explorer não reproduzirá nem baixará vídeos, ajudando na exibição mais rápida das páginas.)|
|Internet Explorer\Painel de Controle da Internet\Página Avançada|Desativar o carregamento de sites e conteúdo em segundo plano para otimizar o desempenho| N/D |**Habilitado** (Com essa configuração de política habilitada, o IE não carrega sites ou conteúdo em segundo plano.)|
|*Internet Explorer\Painel de Controle da Internet\Página Avançada|Desativar o recurso de virar com previsão de página| N/D |**Habilitado** (A Microsoft coleta seu histórico de navegação para aprimorar o funcionamento do recurso de virar a página com previsão de página. Se você habilitar essa configuração de política, o recurso de virar com previsão de página será desativado e a próxima página da Web não será carregada em segundo plano.)|
|Internet Explorer\Configurações da Internet\Configurações Avançadas\Navegação|Desativar a detecção de número de telefone| N/D |**Habilitado** (Essa configuração de política determina se os números de telefone são reconhecidos e transformados em hiperlinks, que podem ser usados para invocar o aplicativo de telefone padrão no sistema. Se você desabilitar essa configuração de política, a detecção do número de telefone será ligada. Os usuários não poderão modificar essa configuração.)|
|Serviços de informações da Internet|Impedir a instalação do IIS| N/D |**Habilitado** (Com essa configuração de política habilitada, o IIS não pode ser instalado e você não poderá instalar componentes ou aplicativos do Windows que exigem o IIS.)|
|*Localização e sensores|Desativar local| N/D |**Habilitado** (Com essa configuração habilitada, o recurso de localização será desativado e todos os programas nesse dispositivo serão impedidos de usar informações de localização do recurso de localização)|
|Localização e sensores|Desativar sensores| N/D |**Habilitado** (Essa configuração de política desativa o recurso de sensor para este dispositivo. Com essa configuração de política habilitada, o recurso de sensor é desligado e nenhum programa neste computador pode usar o recurso de sensor.)|
|Locais e sensores / Localizador do Windows|Desativar Localizador do Windows| N/D |**Habilitado** (Essa configuração de política desativa o recurso do provedor de localização do Windows para este dispositivo.)|
|*Mapas|Desativar Download e Atualização Automáticos de Dados do Mapa| N/D |**Habilitado** (Com essa configuração habilitada, o download e a atualização automáticos dos dados do mapa são desativados.)|
|*Mapas|Desativar tráfego de rede não solicitado na página de configurações de Mapas Offline| N/D |**Habilitado** (Com essa configuração habilitada, os recursos que geram tráfego de rede na página de configurações dos Mapas Offline serão desativados. Observação: Isso pode desligar toda a página de configurações)|
|*Mensagens|Permitir sincronização de nuvem do serviço de mensagem| N/D |**Desabilitado** (Essa configuração de política permite o backup e a restauração de mensagens de texto de celular para serviços de nuvem da Microsoft.)|
|*Microsoft Edge|Permitir atualizações de configuração para a Biblioteca de Livros| N/D |**Desabilitado** (Com essa configuração desabilitada, o Microsoft Edge não baixará automaticamente os dados de configuração atualizados para a biblioteca de livros.)|
|*Microsoft Edge|Permitir telemetria estendida para a guia Livros| N/D |**Desabilitado** (Com essa configuração desabilitada, o Microsoft Edge só envia dados de telemetria básicos, dependendo da configuração do dispositivo.)|
|Microsoft Edge|Permita que o Microsoft Edge inicie previamente na inicialização do Windows, quando o sistema estiver ocioso e cada vez que o Microsoft Edge for fechado|Configurar pré-inicialização: Impedir inicialização prévia|**Habilitado** (Com essa configuração habilitada e configurada para evitar a pré-inicialização, o Microsoft Edge não será inicializado durante a entrada do Windows, quando o sistema estiver ocioso ou a cada vez que o Microsoft Edge for fechado.)|
|Microsoft Edge|Permitir que o Microsoft Edge inicie e carregue a página de guia Iniciar e nova na inicialização do Windows e sempre que o Microsoft Edge for fechado|Configurar o pré-carregamento de guia: Impedir o pré-carregamento de guia|**Habilitado** (Essa configuração de política permite que você decida se o Microsoft Edge pode carregar a página de guia Iniciar e nova durante a entrada do Windows e cada vez que o Microsoft Edge é fechado. Por padrão, essa configuração é para permitir o pré-carregamento. Com o pré-carregamento desabilitado, o Microsoft Edge não carregará a página de início ou de nova guia durante a entrada do Windows e sempre que o Microsoft Edge for fechado.)|
|Microsoft Edge|Permitir conteúdo da Web na página Nova Guia| N/D |**Desabilitado** (Com essa configuração desabilitada, o Edge abre uma nova guia com uma página em branco. Se essa configuração estiver configurada, os usuários não poderão alterar a configuração.)|
|*Microsoft Edge|Impedir que a página da Web Introdução abra no Microsoft Edge| N/D |**Habilitado** (os usuários não verão a página Primeira Execução ao abrir o Microsoft Edge pela primeira vez)|
|OneDrive|Impedir o OneDrive de gerar tráfego de rede até o usuário entrar no OneDrive| N/D |**Habilitado** (Habilite essa configuração para impedir que o cliente de sincronização do OneDrive, o OneDrive.exe, gere tráfego de rede (verificar atualizações etc.) até que o usuário entre no OneDrive ou inicie a sincronização de arquivos com o computador local)|
|Assistência online|Desligar Ajuda Ativa| N/D |**Habilitado** (com essa configuração de política habilitada, os links de conteúdo ativo não são renderizados. O texto é exibido, mas não há links clicáveis para esses elementos.)|
|OOBE|Não iniciar a experiência de configurações de privacidade no momento do logon do usuário| N/D |**Habilitado** (ao fazer logon em uma nova conta de usuário pela primeira vez ou após uma atualização em alguns cenários, esse usuário pode ver uma tela ou uma série de telas solicitando que ele escolha as configurações de privacidade para a conta. Habilite essa política para evitar que essa experiência seja iniciada.)|
|Feeds RSS|Impedir a descoberta automática de feeds e Web Slices| N/D |**Habilitado** (Essa configuração de política impede que os usuários definam o Internet Explorer para descobrir automaticamente se um feed ou Web Slice está disponível para uma página da Web associada.)|
|*RSS Feeds|Desativar a sincronização em segundo plano de feeds e Web Slices| N/D |**Habilitado** (Com essa configuração de política habilitada, a capacidade de sincronizar feeds e Web Slices em segundo plano será desativada.)|
|*Pesquisa|Permitir a Cortana| N/D |**Desabilitado** (Essa configuração de política especifica se a Cortana é permitida no dispositivo. Quando a Cortana for desativada, os usuários ainda poderão usar a pesquisa para encontrar itens no dispositivo.)|
|Pesquisar|Permitir a Cortana acima da tela de bloqueio| N/D |**Desabilitado** (Essa configuração de política determina se o usuário pode ou não interagir com a Cortana usando a fala enquanto o sistema está bloqueado.)|
|*Pesquisa|Permitir que a pesquisa e a Cortana usem a localização| N/D |**Desabilitado** (Essa configuração de política especifica se a pesquisa e a Cortana podem fornecer pesquisa com reconhecimento de localização e resultados da Cortana.)|
|Pesquisar|Controlar visualizações avançadas para anexos|Controle visualizações avançadas para anexos: **.docx;.xlsx;.txt;.xls**|**Habilitado** (Habilitar essa política define uma lista delimitada por ponto e vírgula de extensões de arquivo que poderão ter visualizações de anexo avançadas.)<p>**OBSERVAÇÃO**: Essa configuração pode ser usada para limitar os tipos de anexos que são visualizados, o que também pode ajudar a impedir a visualização automática de alguns tipos de conteúdo potencialmente perigosos.|
|Pesquisar|Não permitir pesquisas na Web| N/D |**Habilitado** (Habilitar essa política remove a opção de pesquisar na Web do Windows Desktop Search.)|
|*Pesquisa|Não pesquisar na Web ou exibir resultados da Web na Pesquisa| N/D |**Habilitado** (Com essa configuração de política habilitada, as consultas não serão realizadas na Web e os resultados da Web não serão exibidos quando um usuário executar uma consulta em Pesquisa.)|
|Pesquisar|Habilitar a indexação de pastas do Exchange não armazenadas em cache| N/D |**Desabilitado** (Habilitar essa política permite a indexação de itens de email em um servidor do Microsoft Exchange quando o Microsoft Outlook não está em execução no modo cache. O comportamento padrão da pesquisa é não indexar pastas do Exchange não armazenadas em cache. Desabilitar essa política bloqueará qualquer indexação de pastas do Exchange não armazenadas em cache.)|
|Pesquisar|Impedir indexação de arquivos no cache de arquivos offline| N/D |**Habilitado** (Se habilitada, os arquivos em compartilhamentos de rede disponibilizados offline não serão indexados. Caso contrário, são indexados. Desabilitado por padrão.)|
|*Pesquisa|Definir quais informações são compartilhadas na Pesquisa|Informações anônimas|**Habilitado** (Informações anônimas: Compartilhe informações de uso, mas não compartilhe o histórico de pesquisa, informações da conta Microsoft ou uma localização específica)|
|Pesquisar|Parar a indexação em caso de espaço em disco rígido limitado|Limite de MB: **5.000**|**Habilitado** (Habilitar essa política impede que a indexação continue depois que uma quantidade de espaço em disco rígido inferior à especificada for deixada na mesma unidade que a localização do índice. Selecione entre 0 e 2.147.483.647 MB.)|
|Plataforma de Proteção de Software|Desligar Validação AVS Online no Cliente KMS| N/D |**Habilitado** (Com essa configuração habilitada, o dispositivo não envia dados à Microsoft em relação ao seu estado de ativação)|
|*Fala|Permitir a Atualização Automática de Dados de Fala| N/D |**Desabilitado** (Especifica se o dispositivo receberá atualizações para os modelos de reconhecimento de fala e síntese de fala.)|
|Repositório|Desativar a oferta de atualização para a última versão do Windows| N/D |**Habilitado** (Habilita ou desabilita a oferta da Store para atualizar para a versão mais recente do Windows. Se você habilitar essa configuração, o aplicativo Store não oferecerá atualizações para a versão mais recente do Windows.)|
|Entrada de Texto|Aprimorar reconhecimento de escrita à tinta e digitação| N/D |**Desabilitado** (Esta configuração de política controla a capacidade de enviar escrita à tinta e digitar dados para a Microsoft aprimorar as funcionalidades de reconhecimento de linguagem e sugestão dos aplicativos e serviços em execução no Windows.)|
|Relatório de Erros do Windows|Desabilitar o Relatório de Erros do Windows| N/D |**Habilitado** (Com essa configuração de política habilitada, o Relatório de Erros do Windows não envia informações sobre problemas à Microsoft. Além disso, as informações da solução não estão disponíveis em segurança e manutenção no painel de controle.)|
|Gravação e Transmissão de Jogos do Windows|Habilita ou desabilita a Gravação e a Transmissão de Jogos do Windows| N/D |**Desabilitado** (Com essa configuração desabilitada, a gravação de jogos do Windows não será permitida.)|
|Workspace do Windows Ink|Permitir o Espaço de Trabalho do Windows Ink|Selecione uma das seguintes ações: Desabilitado|**Habilitado** (Com essa configuração habilitada e a subconfiguração definida como desabilitada, a funcionalidade de Espaço de Trabalho do Windows Ink não está disponível.)|
|Windows Installer|Controlar tamanho máximo do cache de arquivo de linha de base|5|**Habilitado** (Essa política controla o percentual de espaço em disco disponível para o cache de arquivos de linha de base de Windows Installer. Com essa configuração de política habilitada, você pode modificar o tamanho máximo do cache de arquivos de linha de base de Windows Installer.)|
|Windows Installer|Desabilitar a criação de pontos de verificação de Restauração do Sistema| N/D |**Habilitado** (Com essa configuração de política habilitada, o Windows Installer não gera pontos de verificação de restauração do sistema ao instalar aplicativos.)|
|Centro de Mobilidade do Windows|Desativar o Centro de Mobilidade do Windows| N/D |**Habilitado** (Com essa configuração de política habilitada, o usuário não pode invocar o Centro de Mobilidade do Windows. A interface do usuário do Centro de Mobilidade do Windows é removida de todos os pontos de entrada do Shell e o arquivo .exe não a inicia.)|
|Análise de Confiabilidade do Windows|Configurar os Provedores WMI de Confiabilidade| N/D |**Desabilitado** (Com essa configuração de política desabilitada, o monitor de confiabilidade não exibirá informações de confiabilidade do sistema e os aplicativos compatíveis com o WMI não poderão acessar as informações de confiabilidade dos provedores listados.)|
|Segurança do Windows\Notificações|Ocultar notificações não críticas| N/D |**Habilitado** (Com essa configuração habilitada, os usuários locais só verão notificações críticas da segurança do Windows. Eles não verão outros tipos de notificações, como informações comuns de integridade do dispositivo ou do PC.)|
|Windows Update|Ativar notificações de software| N/D |**Desabilitado** Essa configuração de política permite que você controle se os usuários veem mensagens de notificação aprimoradas detalhadas sobre o software em destaque do serviço do Microsoft Update. As mensagens de notificação aprimoradas transmitem o valor e promovem a instalação e o uso de software opcional. Essa configuração de política destina-se a uso em ambientes de gerenciamento flexível nos quais você permite que o usuário final acesse o serviço do Microsoft Update.)|
|*Windows Update\Windows Update para Empresas|Gerenciar versões prévias|Definir o comportamento de recebimento das versões prévias: **Desabilitar builds de versão prévia**|**Habilitado** (Selecionar "Desabilitar builds de versão prévia" impedirá a instalação de builds de versão prévia no dispositivo. Isso impedirá que os usuários aceitem participar do Programa Windows Insider por meio de Configurações -> Atualização e Segurança)|
|*Windows Update\Windows Update para Empresas|Selecionar quando Versões Prévias e Atualizações de Recursos forem recebidas|Selecione o nível de preparação do Windows para as atualizações que você deseja receber:<p>**Canal Semestral**<p>Após o lançamento do Build de Versão Prévia ou da Atualização de Recursos, adie o recebimento para este número de dias: **365**<p>Pausar Builds de Versão Prévia ou Atualizações de Recursos a partir de: **aaaa-mm-dd**|**Habilitado** (Habilite essa política para especificar o nível do build da versão prévia ou as atualizações de recursos a serem recebidas e quando. Canal semestral: Receba atualizações de recursos quando elas forem lançadas para o público em geral.<p> Ao selecionar Canal Semestral:<p>– Você pode adiar o recebimento de atualizações de recursos por até 365 dias.<p>– Para impedir que as atualizações de recursos sejam recebidas no horário agendado, você pode pausá-las temporariamente. A pausa permanecerá em vigor por 35 dias a partir da hora de início informada.<p> – Para retomar o recebimento de atualizações de recursos em pausa, desmarque o campo data de início.)|
|Windows Update\Windows Update para Empresas|Selecionar quando as Atualizações de Qualidade serão recebidas|Depois que uma atualização de qualidade for liberada, adie o recebimento para este número de dias: **30**<p>Pausar início das atualizações de qualidade aaaa-mm-dd|**Habilitado** (Habilite esta política para especificar quando receber atualizações de qualidade.<p>Você pode adiar o recebimento de atualizações de qualidade por até 30 dias.<p>Para impedir que as atualizações de qualidade sejam recebidas em seu horário agendado, você pode pausá-las temporariamente. A pausa permanecerá em vigor por 35 dias ou até você limpar o campo data de início.<p>Para retomar o recebimento de Atualizações de Qualidade em pausa, desmarque o campo data de início.)<p>Essa recomendação é para ajudar a controlar quando as atualizações são aplicadas e garantir que as atualizações não sejam oferecidas e instaladas inesperadamente|
|Política do Computador Local\Configuração do Usuário\Modelos Administrativos| N/D | N/D | N/D |
|Painel de Controle\Opções Regionais e de Idioma|Desativar a oferta de previsões de texto ao digitar| N/D |**Habilitado** (Essa política desativa a opção de previsões de texto de oferta como eu digitar. No entanto, isso não impede que o usuário ou um aplicativo altere a configuração de modo programático. Com essa configuração de política habilitada, a opção será bloqueada para não oferecer previsões de texto.)|
|Desktop|Não adicionar compartilhamentos de documentos recentemente abertos a Locais de Rede| N/D |**Habilitado** (Com essa configuração habilitada, as pastas compartilhadas não são adicionadas aos locais de rede automaticamente quando você abre um documento na pasta compartilhada.)|
|Desktop|Desativar a janela Aero Shake minimizando o gesto do mouse| N/D |**Habilitado** (Impede que o Windows seja minimizado ou restaurado quando a janela ativa é sacudida com o mouse. Com essa política habilitada, as janelas de aplicativo não serão minimizadas nem restauradas quando a janela ativa for sacudida para frente e para trás com o mouse.)|
|Área de Trabalho/Active Directory|Tamanho máximo de pesquisas do Active Directory|Número de objetos retornados:**1500**|**Habilitado** (Especifica o número máximo de objetos que o sistema exibe em resposta a um comando para procurar ou pesquisar no Active Directory. Essa configuração afeta todas as exibições de procura associadas ao Active Directory, como aquelas em usuários e grupos locais, usuários do Active Directory e computadores e caixas de diálogo usadas para definir permissões para objetos de usuário ou grupo no Active Directory.)|
|Menu Iniciar e barra de tarefas|Não exibir nem controlar itens nas Listas de Atalhos a partir de locais remotos| N/D |**Habilitado** (Essa configuração de política permite controlar a exibição ou o acompanhamento de itens em listas de atalhos de locais remotos.)|
|Menu Iniciar e barra de tarefas|Não pesquisar na Internet| N/D |**Habilitado** (Com essa configuração de política habilitada, a caixa de pesquisa do menu Iniciar não pesquisará o histórico da Internet nem os favoritos.)|
|Menu Iniciar e barra de tarefas|Não usar o método baseado em pesquisa ao resolver atalhos do shell| N/D |**Habilitado** (Essa configuração de política impede que o sistema conduza uma pesquisa abrangente da unidade de destino para resolver um atalho.)|
|Menu Iniciar e barra de tarefas|Desativar todas as notificações de balão| N/D |**Habilitado** (Com essa configuração de política habilitada, nenhum balão de notificação é mostrado ao usuário.)
|Menu Iniciar e barra de tarefas|Desligar o recurso de notificações de balões de publicidade| N/D |**Habilitado** (Com essa configuração de política habilitada, determinados balões de notificação marcados como anúncios de recursos não são mostrados.)|
|Menu Iniciar e barra de tarefas|Desativar o rastreamento de usuário| N/D |**Habilitado** (Com essa configuração de política habilitada, o sistema não rastreia os programas que o usuário executa e não exibe programas usados com frequência no menu iniciar.)|
|Menu Iniciar e Barra de Tarefas/Notificações|Desativar notificações de aviso| N/D |**Habilitado** (Com essa configuração de política habilitada, os aplicativos não poderão gerar notificações do sistema.)|
|*Menu Iniciar e barra de tarefas/Notificações|Desligar notificações do sistema na tela de bloqueio| N/D |**Habilitado** (Com essa configuração de política habilitada, os aplicativos não poderão gerar notificações do sistema na tela de bloqueio.)|
|Política do computador local/configuração do usuário| N/D | N/D | N/D |
|Componentes do Windows/Conteúdo de Nuvem|Configurar o destaque do Windows na tela de bloqueio| N/D |**Desabilitado** (Com essa política desabilitada, o destaque do Windows será desativado e os usuários não poderão mais selecioná-lo como tela de bloqueio. Os usuários verão a imagem de tela de bloqueio padrão e poderão selecionar outra imagem, a menos que você tenha habilitado a política "Impedir a alteração da imagem da tela de bloqueio".)|
|*Componentes do Windows/Conteúdo de Nuvem|Não sugerir conteúdo de terceiros no destaque do Windows| N/D |**Habilitado** (Com essa política habilitada, recursos de destaque do Windows como o Spotlight da tela de bloqueio, aplicativos sugeridos no menu iniciar ou dicas do Windows não sugerirão mais aplicativos e conteúdo de editores de software de terceiros. Os usuários ainda podem ver sugestões e dicas para torná-los mais produtivos com os recursos e aplicativos da Microsoft.)|
|Componentes do Windows/Conteúdo de Nuvem|Não use dados de diagnóstico para experiências personalizadas| N/D |**Habilitado** (Com essa configuração de política habilitada, o Windows não usará dados de diagnóstico deste dispositivo (esses dados podem incluir o uso de navegador, aplicativo e recurso, dependendo do valor de configuração de "dados de diagnóstico") para personalizar o conteúdo mostrado na tela de bloqueio, dicas do Windows, recursos de consumidor da Microsoft e outros recursos relacionados.)|
|Componentes do Windows/Conteúdo de Nuvem|Desativar todos os recursos de Destaque do Windows| N/D |**Habilitado** (Destaque do Windows na tela de bloqueio, dicas do Windows, recursos de consumidor da Microsoft e outros recursos relacionados serão desativados. Você deve habilitar essa configuração de política se a sua meta é minimizar o tráfego de rede dos dispositivos de destino.)|
|UI Borda|Desativar rastreamento de uso do aplicativo| N/D |**Habilitado** (Essa configuração de política impede que o Windows mantenha o controle dos aplicativos que são usados e pesquisados com mais frequência. Se você habilitar essa configuração de política, os aplicativos serão classificados em ordem alfabética em:<p> - resultados da pesquisa<p> – os painéis Pesquisar e Compartilhar<p> – a lista suspensa de aplicativos no Seletor)|
|Explorador de Arquivos|Desligar armazenamento em cache de imagens em miniatura| N/D |**Habilitado** (Com essa configuração de política habilitada, exibições de miniatura não são armazenadas em cache.)|
|Explorador de Arquivos|Desligar animações de janela e controle comum| N/D |**Habilitado** (Desabilitar animações pode aprimorar a usabilidade para usuários com algumas deficiências visuais, bem como aprimorar o desempenho e a vida útil da bateria em alguns cenários.)|
|Explorador de Arquivos|Desligar a exibição de entradas de pesquisas recentes na caixa de pesquisa do Explorador de Arquivos| N/D |**Habilitado** (Desabilita a sugestão de consultas recentes para a caixa de pesquisa e impede que as entradas na caixa de pesquisa sejam armazenadas no Registro para referências futuras.)|
|Explorador de Arquivos|Desligar o armazenamento em cache das miniaturas em arquivos thumbs.db ocultos| N/D |**Habilitado** (Com essa configuração de política habilitada, o Explorador de Arquivos não cria, lê nem grava em arquivos thumbs.db.)|

\* Vem da [Linha de base de funcionalidade limitada de tráfego restrito do Windows](https://go.microsoft.com/fwlink/?linkid=828887).

### <a name="system-services"></a>Serviços do sistema

Se você estiver pensando em desabilitar os serviços do sistema para preservar recursos, verifique se o serviço não é um componente de algum outro serviço. Neste documento e com os scripts GitHub disponíveis, alguns serviços não estão na lista porque não podem ser desabilitados de maneira compatível.

Além disso, a maioria dessas recomendações espelha as recomendações para o Windows Server 2016, instalado com a Experiência Desktop, com base nas instruções em [Diretrizes sobre como desabilitar serviços do sistema no Windows Server 2016 com Experiência Desktop](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md).

Muitos serviços que possam parecer bons candidatos à desabilitação são definidos com o tipo de início de serviço manual. Isso significa que o serviço não será iniciado automaticamente e não será iniciado, a menos que um processo ou um evento dispare uma solicitação para o serviço que está sendo considerado para desabilitação. Os serviços que já estão definidos para tipo de início manual normalmente são listados aqui.

> [!NOTE]
> Você pode enumerar os serviços em execução com este código de exemplo do PowerShell, gerando apenas o nome curto do serviço:
>
> ```powershell
> Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object -ExpandProperty Name
> ```

A seguinte tabela contém alguns serviços que podem ser considerados para desabilitar em ambientes de área de trabalho virtual:

| Serviço do Windows | Nome do serviço | Item | Comentário|
| --------------- | ------------ | ---- | ------ |
|Tempo de celular|autotimesvc|Esse serviço define a hora com base em mensagens NITZ de uma rede móvel|Ambientes de área de trabalho virtual podem não ter esses dispositivos disponíveis. <p>Para saber mais, confira [este artigo](/windows-hardware/drivers/network/mb-nitz-support). |
|Serviço de usuário Transmissão e GameDVR|BcastDVRUserService|Esse serviço (por usuário) é usado para Gravações de Jogo e Transmissões ao Vivo|OBSERVAÇÃO: esse é um "serviço por usuário" e, como tal, o serviço de modelo deve ser desabilitado. Esse serviço de usuário é usado para Gravações de Jogo e Transmissões ao Vivo.<p>Para saber mais, confira [este artigo](/windows-hardware/drivers/network/mb-nitz-support). |
|CaptureService|CaptureService|Habilita a funcionalidade de captura de tela opcional para aplicativos que chamam a API Windows.Graphics.Capture.|Serviço de captura OneCore: habilita a funcionalidade de captura de tela opcional para aplicativos que chamam a API do Windows.Graphics.Capture<p> Para obter mais informações, consulte [este artigo](/uwp/api/windows.graphics.capture?view=winrt-19041&preserve-view=true).|
|Serviço de Plataforma de Dispositivos Conectados|CDPSvc|Esse serviço é usado para cenários de Plataforma de Dispositivos Conectados|Serviço de Plataforma de Dispositivos Conectados <p> Para saber mais, confira [este artigo](/openspecs/windows_protocols/ms-cdp/929c2238-6d49-4ba4-a36a-37e732c4f736)|
|Serviço de usuário de CDP|CDPUserSvc| N/D |Serviço do usuário da plataforma de dispositivos conectados. Para saber mais, confira [este artigo](/openspecs/windows_protocols/ms-cdp/f5a15c56-ac3a-48f9-8c51-07b2eadbe9b4).<p> Esse serviço de usuário é usado para cenários de Plataforma de Dispositivos Conectados <br><br>Esse é um serviço "por usuário" e, como tal, o serviço de modelo deve ser desabilitado (CDPUserSvc).|
|Otimizar unidades|defragsvc|Ajuda o computador a ser executado com mais eficiência pela otimização de arquivos em unidades de armazenamento.|Soluções de área de trabalho virtual normalmente não se beneficiam da otimização do disco. As "unidades" normalmente não são unidades tradicionais e, muitas vezes, são apenas uma alocação de armazenamento temporário.|
|Serviço de execução de diagnóstico|DiagSvc|Executa ações de diagnóstico para solução de problemas de suporte|Desabilitar esse serviço desabilita a capacidade de executar o Serviço de Execução de Diagnóstico referente ao diagnóstico do Windows.|
|Experiência do Usuário Conectado e Telemetria|DiagTrack|Esse serviço habilita os recursos que dão suporte a experiências de usuário no aplicativo e conectadas. Além disso, esse serviço gerencia a coleção controlada por evento e a transmissão de informações de diagnóstico e de uso (usadas para aprimorar a experiência e a qualidade da Plataforma Windows) quando as configurações de opção de privacidade de uso e diagnóstico são habilitadas em Comentários e Diagnóstico.|Considere a possibilidade de desabilitação se estiver em uma rede desconectada. Para saber mais, confira [este artigo](/windows/privacy/configure-windows-diagnostic-data-in-your-organization).|
|Serviço de Política de Diagnóstico|DPS|O Serviço de Política de Diagnóstico permite a detecção e a solução de problemas, bem como a resolução para componentes do Windows. Se esse serviço for interrompido, o diagnóstico deixará de funcionar.|Desabilitar esse serviço desabilita a capacidade de executar o diagnóstico do Windows. Para obter mais informações, consulte [este artigo](/uwp/api/Windows.System.Diagnostics?view=winrt-19041&preserve-view=true).|
|Gerenciador de Instalação de Dispositivo|DsmSvc|Habilita a detecção, o download e a instalação de software relacionado ao dispositivo. |Se esse serviço for desabilitado, os dispositivos poderão ser configurados com um software desatualizado e poderão não funcionar corretamente. <p>Os ambientes de área de trabalho virtual controlam fortemente qual software está instalado e mantêm essa consistência em todo o ambiente.|
|Serviço de uso de dados|DusmSvc|Uso de dados de rede, limite de dados, restringir dados em segundo plano, redes limitadas.| Para obter mais informações, consulte [este artigo](/uwp/schemas/mobilebroadbandschema/dusm/schema-root). |
|Serviço de Hotspot Móvel do Windows|icssvc|Fornece a capacidade de compartilhar uma conexão de dados da rede celular com outro dispositivo.|Para saber mais, confira [este artigo](/uwp/api/Windows.Networking.NetworkOperators.NetworkOperatorTetheringAccessPointConfiguration?view=winrt-19041&preserve-view=true).|
|Serviço de Instalação da Microsoft Store|InstallService|Fornece suporte de infraestrutura para a Microsoft Store. |Esse serviço é iniciado sob demanda e, se for desabilitado, as instalações não funcionarão corretamente.<p>Considere desabilitar esse serviço em uma área de trabalho virtual não persistente e, para soluções de área de trabalho virtual persistentes, deixe no estado em que se encontram.|
|Serviço de Geolocalização|Lfsvc|Monitora a localização atual do sistema e gerencia cercas geográficas (uma localização geográfica com eventos associados). |Se você desligar esse serviço, os aplicativos não poderão usar nem receber notificações para a geolocalização ou cercas geográficas. Para saber mais, confira [este artigo](/uwp/api/Windows.Devices.Geolocation?view=winrt-19041&preserve-view=true).|
|Gerenciador de Mapas Baixados|MapsBroker|O serviço Windows para acesso de aplicativo aos mapas baixados. Esse serviço é iniciado sob demanda pelo aplicativo que acessa os mapas baixados.|A desabilitação desse serviço impedirá que os aplicativos acessem mapas. Para saber mais, confira [este artigo](/uwp/api/Windows.Services.Maps?view=winrt-19041&preserve-view=true).|
|MessagingService|MessagingService|Serviço que dá suporte a mensagens de texto e funcionalidade relacionada.|Esse é um "serviço por usuário" e, como tal, o serviço de modelo deve ser desabilitado.|
|Host de Sincronização|OneSyncSvc|Esse serviço sincroniza emails, contatos, calendário e vários outros dados do usuário. |(UWP) Os aplicativos de email e outros aplicativos dependentes dessa funcionalidade não funcionarão corretamente quando esse serviço não estiver em execução. <p>Esse é um "serviço por usuário" e, como tal, o serviço de modelo deve ser desabilitado.|
|Dados de Contato|PimIndexMaintenanceSvc|Indexa dados de contato para pesquisa rápida de contatos. Se você interromper ou desabilitar esse serviço, os contatos poderão ficar ausentes dos resultados da pesquisa.|Esse é um "serviço por usuário" e, como tal, o serviço de modelo deve ser desabilitado.|
|Energia|Energia|Gerencia a política de energia e a entrega de notificação da política de energia.|As máquinas virtuais praticamente não têm influência sobre as propriedades de energia. Se esse serviço estiver desabilitado, o gerenciamento de energia e os relatórios não estarão disponíveis. Para saber mais, confira [este artigo](/windows-hardware/drivers/powermeter/user-mode-power-service).|
|Gerenciador de pagamentos e NFC/SE|SEMgrSvc|Gerencia pagamentos e elementos seguros com base em NFC (Comunicação a Curta Distância).|Pode não precisar desse serviço para pagamentos no ambiente corporativo.|
|Serviço de Roteador de SMS do Microsoft Windows|SmsRouter|Roteia mensagens com base em regras para clientes apropriados.|Poderá não precisar desse serviço se outras ferramentas forem usadas para mensagens, como Teams, Skype ou outras. Para saber mais, confira [este artigo](/dotnet/framework/wcf/feature-details/routing-service)|.
|Superfetch (SysMain)|SysMain|Mantém e melhora o desempenho do sistema ao longo do tempo.|O SuperFetch geralmente não aprimora o desempenho em ambientes de área de trabalho virtual por vários motivos. O armazenamento subjacente geralmente é virtualizado e possivelmente distribuído em várias unidades. Em algumas soluções de área de trabalho virtual, o estado de usuário acumulado é descartado quando o usuário faz logoff. O recurso SysMain deve ser avaliado em cada ambiente.|
|Serviço de Teclado Virtual e Painel de Manuscrito|TabletInputService|Habilita a funcionalidade de caneta e tinta do Teclado Virtual e do Painel de Manuscrito|Não é necessário, a menos que haja uma tela de toque ativa em uso ou um dispositivo de entrada de manuscrito.|
|Atualizar o Serviço Orchestrator|UsoSvc|Gerencia as Atualizações do Windows. Se ele for interrompido, os dispositivos não poderão baixar nem instalar as atualizações mais recentes.|Os dispositivos de área de trabalho virtual geralmente são gerenciados cuidadosamente com relação às atualizações. A manutenção normalmente é executada durante as janelas de manutenção. Em alguns casos, um cliente de atualização pode ser utilizado, como o SCCM. A exceção seriam atualizações de assinatura de segurança, aplicadas a qualquer momento, a qualquer dispositivo de área de trabalho virtual, para manter assinaturas atualizadas. Se você desabilitar esse serviço, teste para garantir que as assinaturas de segurança ainda possam ser instaladas.|
|Cópias de Sombra de Volume|VSS|Gerencia e implementa Cópias de Sombra de Volume usadas para backup e outros fins. |Se esse serviço for interrompido, as cópias de sombra não estarão disponíveis para backup e o backup poderá falhar. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados. Para saber mais, confira [este artigo](../../../WindowsServerDocs/storage/file-server/volume-shadow-copy-service.md).|
|Host do Sistema de Diagnóstico|WdiSystemHost|O Host do Sistema de Diagnóstico é usado pelo Serviço de Política de Diagnóstico para hospedar os diagnósticos que precisam ser executados em um contexto do Sistema Local. Se esse serviço for interrompido, os diagnósticos que dependem dele deixarão de funcionar.|Desabilitar esse serviço desabilita a capacidade de executar o diagnóstico do Windows
|Relatório de Erros do Windows|WerSvc|Permite que os erros sejam relatados quando os programas param de funcionar ou responder e permite que as soluções existentes sejam entregues. Também permite que os logs sejam gerados para serviços de diagnóstico e reparo. Se esse serviço for interrompido, o relatório de erros poderá não funcionar corretamente e os resultados dos serviços de diagnóstico e os reparos poderão não ser exibidos.|Com ambientes de área de trabalho virtual, os diagnósticos geralmente são executados em um cenário "offline" e não na produção principal. Além disso, alguns clientes desabilitam o WER mesmo assim. O WER incorre em uma pequena quantidade de recursos para muitas tarefas diferentes, incluindo falha ao instalar um dispositivo ou falha ao instalar uma atualização. Para saber mais, confira [este artigo](/windows/win32/wer/windows-error-reporting).|
|Windows Search|WSearch|Fornece indexação de conteúdo, cache de propriedades e resultados da pesquisa para arquivos, emails e outros tipos de conteúdo.|Desabilitar esse serviço impede a indexação de email e outras ações. Teste antes de desabilitar este serviço. Para saber mais, confira [este artigo](/windows/win32/search/-search-3x-wds-overview#windows-search-service). |
|Gerenciador de Autenticação Xbox Live|XblAuthManager|Fornece serviços de autenticação e autorização para interação com o Xbox Live. |Se esse serviço for interrompido, alguns aplicativos poderão não funcionar corretamente.
|Salvamento de Jogo do Xbox Live|XblGameSave|Esse serviço sincroniza os dados de salvamento de jogos habilitados para salvamento do Xbox Live. |Se esse serviço for interrompido, não será feito upload dos dados de salvamento de jogos no Xbox Live nem baixados dele.
|Serviço de Gerenciamento do Acessório do Xbox|XboxGipSvc|Esse serviço gerencia os acessórios do Xbox conectados.| N/D |
|Serviço de Rede Xbox Live|XboxNetApiSvc|Este serviço dá suporte à interface de programação de aplicativo Windows.Networking.XboxLive.| N/D |

#### <a name="per-user-services-in-windows"></a>Serviços por usuário no Windows

Os serviços por usuário referem-se a serviços que são criados quando um usuário entra no Windows ou no Windows Server e que são interrompidos e excluídos quando o usuário sai do serviço. Esses serviços são executados no contexto de segurança da conta de usuário – isso fornece melhor gerenciamento de recursos que a abordagem anterior de executar esses tipos de serviço no Explorador, associados a uma conta pré-configurada, ou como tarefas. Para obter mais informações, confira [Serviços por usuário no Windows](/windows/application-management/per-user-services-in-windows).

Se você pretende alterar o valor inicial de um serviço, o método preferencial é abrir um prompt .CMD com privilégios elevados e executar a ferramenta Gerenciador de Controle de Serviço `SC.EXE`. Para obter mais informações, confira [SC](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754599(v=ws.11)).

### <a name="scheduled-tasks"></a>Tarefas Agendadas

Como outros itens no Windows, verifique se um item não é necessário antes de desabilitar uma tarefa agendada. Pode não ser desejável executar em produção algumas tarefas em ambientes de área de trabalho virtual, como **StartComponentCleanup**, mas podem ser bom executá-las durante uma janela de manutenção na "imagem ouro" (imagem de referência).

A lista a seguir inclui as tarefas que realizam otimizações ou coletas de dados em computadores que mantêm seu estado entre as reinicializações. Quando um dispositivo de área de trabalho virtual reinicia e descarta todas as alterações desde a última inicialização, as otimizações destinadas a computadores físicos não são úteis.

Você pode obter todas as tarefas agendadas atuais, incluindo as descrições, com o seguinte código do PowerShell:

```powershell
  Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

> [!NOTE]
> Há várias tarefas que não podem ser desabilitadas com um script, mesmo quando executadas em um prompt de comandos com privilégios elevados. As recomendações aqui e nos scripts do GitHub não tentam desabilitar tarefas que não podem ser desabilitadas com um script.

|Nome da tarefa agendada|Descrição|
|-------------------|-----------|
|MNO|Analisador de metadados da experiência de conta de banda larga móvel|
|AnalyzeSystem|Esta tarefa analisa o sistema que procura condições que podem causar alto uso de energia|
|Celular|Relacionado a dispositivos celulares|
|Compatibilidade|Coletará informações de telemetria do programa se você optar pelo Programa de Aperfeiçoamento da Experiência do Usuário da Microsoft.|
|Consolidador|Se o usuário tiver concordado em participar do Programa de Aperfeiçoamento da Experiência do Usuário do Windows, esse trabalho coletará e enviará dados de uso para a Microsoft|
|Diagnósticos|(DiskFootprint no caminho da tarefa) "DiskFootprint" é a contribuição combinada de todos os processos que emitem E/S de armazenamento na forma de leituras, gravações e liberações de armazenamento.|
|FamilySafetyMonitor|Inicializa o monitoramento e a imposição da Proteção para a Família.|
|FamilySafetyRefreshTask|Sincroniza as configurações mais recentes com o serviço de recursos da família Microsoft.|
|MapsToastTask|Esta tarefa mostra várias notificações do sistema relacionadas ao mapa|
|Microsoft-Windows-DiskDiagnosticDataCollector|O Diagnóstico de Disco do Windows relata informações gerais do disco e do sistema à Microsoft para usuários que participam do Programa de Experiência do Cliente.|
|NotificationTask|Tarefa em segundo plano para executar interações por usuário e Web|
|ProcessMemoryDiagnosticEvents|Agenda um diagnóstico de memória em resposta a eventos do sistema|
|Proxy|Essa tarefa coleta e carrega dados de SQM do Autochk se você opta pelo Programa de Aperfeiçoamento da Experiência do Usuário da Microsoft.|
|QueueReporting|Tarefa de Relatório de Erros do Windows para processar relatórios em fila.|
|RecommendedTroubleshootingScanner|Verificar a solução de problemas recomendada da Microsoft|
|RegIdleBackup|Tarefa de backup ocioso do Registro|
|RunFullMemoryDiagnostic|Detecta e atenua problemas na memória física (RAM).|
|Agendado|A Tarefa de Manutenção Agendada do Windows executa a manutenção periódica do sistema de computador corrigindo problemas automaticamente ou relatando-os por meio de Segurança e Manutenção.|
|ScheduledDefrag|Essa tarefa otimiza as unidades de armazenamento local.|
|SilentCleanup|Tarefa de manutenção usada pelo sistema para iniciar uma limpeza de disco automática silenciosa ao ficar com pouco espaço livre em disco.|
|SpeechModelDownloadTask|
|Tarefas do SQM|Essa tarefa coleta informações sobre o TPM (Trusted Platform Module), a inicialização segura e a inicialização medida.|
|SR|Esta tarefa cria pontos de proteção do sistema regulares.|
|StartComponentCleanup|Tarefa de manutenção com melhor execução durante as janelas de manutenção|
|StartupAppTask|Verifica entradas de inicialização e gera notificação para o usuário se há muitas entradas de inicialização.|
|SyspartRepair|
|WindowsActionDialog|Notificação de localização|
|WinSAT|Mede o desempenho e as funcionalidades do sistema|
|XblGameSaveTask|Tarefa em espera do Xbox Live GameSave|

### <a name="apply-windows-and-other-updates"></a>Aplicar atualizações do Windows (entre outras)

Seja do Microsoft Update, seja de recursos internos, aplique as atualizações disponíveis, incluindo assinaturas do Windows Defender. Esse é um bom momento para aplicar outras atualizações disponíveis, incluindo o Microsoft Office, se instalado, bem como outras atualizações. Se o PowerShell permanecer na imagem, baixe a ajuda mais recente disponível para o PowerShell executando o comando `Update-Help`.

#### <a name="servicing-os-and-apps"></a>Sistema operacional de manutenção e aplicativos

Em algum momento durante o processo de otimização de imagem, as atualizações disponíveis do Windows devem ser aplicadas. Há uma definição nas Configurações de Atualização do Windows 10 que pode fornecer atualizações adicionais. Você pode encontrá-lo em **Configurações** > **Opções avançadas**. Uma vez lá, defina **Fornecer atualizações para outros produtos uMirosoft ao atualizar o Windows** como **Ativado**.

![Uma captura de tela do menu de opções avançadas. A configuração "Fornecer atualizações para outros produtos da Microsoft" está ativada.](media/rds-vdi-recommendations-1909/servicing.png)

Essa seria uma boa configuração no caso de você instalar aplicativos Microsoft, como o Microsoft Office na imagem base. Dessa forma, o Office é atualizado quando a imagem é colocada em serviço. Também há atualizações do .NET e alguns componentes de terceiros, como Adobe, cujas atualizações são disponibilizadas pelo Windows Update.

Uma consideração muito importante para VMs de dispositivos de área de trabalho virtual não persistente são as atualizações de segurança, incluindo arquivos de definição de software de segurança. Essas atualizações podem ser lançadas uma ou mais vezes por dia.

Para o Windows Defender, pode ser melhor permitir que as atualizações ocorram, mesmo em ambientes de área de trabalho virtual não persistentes. As atualizações serão aplicadas praticamente a cada vez que você entrar, mas são pequenas e não devem ser um problema. Além disso, o dispositivo não ficará atrasado nas atualizações, pois apenas a última disponível será aplicada. O mesmo pode ser verdadeiro para arquivos de definição de terceiros.

>[!NOTE]
> Os aplicativos da loja (aplicativos UWP) são atualizados pela Windows Store. As versões modernas do Office, como o Office 365, são atualizadas por meio dos próprios mecanismos quando conectadas diretamente à Internet ou por meio de tecnologias de gerenciamento quando não conectadas à Internet.

#### <a name="windows-system-startup-event-traces-autologgers"></a>Rastreamentos de eventos de inicialização do sistema Windows (AutoLoggers)

O Windows está configurado, por padrão, para coletar e salvar dados de diagnóstico. O objetivo é habilitar o diagnóstico ou gravar dados, caso seja necessária uma solução de problemas adicional. Os rastreamentos automáticos do sistema podem ser encontrados na localização mostrada na seguinte ilustração:

![Uma captura de tela da pasta de gerenciamento do computador. A pasta rastreamentos do sistema está aberta e o arquivo de sessão WiFi está selecionado.](media/rds-vdi-recommendations-1909/system-traces.png)

Alguns dos rastreamentos exibidos em **Sessões de Rastreamento de Eventos** e **Sessões de Rastreamento de Eventos de Inicialização** não podem e não devem ser interrompidos. Outros, como o rastreamento WiFiSession, podem ser interrompidos. Para interromper um rastreamento em execução em **Sessões de Rastreamento de Eventos**, clique com o botão direito do mouse no rastreamento e selecione **Interromper**. Use o seguinte procedimento para impedir que os rastreamentos sejam iniciados automaticamente na inicialização:

1. Selecione a pasta **Sessões de Rastreamento de Eventos de Inicialização**.

2. Localize e selecione o arquivo de rastreamento que você deseja examinar para abri-lo.

3. Selecione a guia **Sessão de Rastreamento**.

4. Desmarque a caixa rotulada como **Habilitado**.

5. Selecione **OK**.

A seguinte tabela lista alguns rastreamentos do sistema que você deve considerar desabilitar em seus ambientes de área de trabalho virtual:

|Nome|Comentário|
|---|--------|
|Cellcore|[https://docs.microsoft.com/windows-hardware/drivers/network/cellular-architecture-and-driver-model](/windows-hardware/drivers/network/cellular-architecture-and-driver-model)|
|CloudExperienceHostOOBE|Documentado [aqui](/windows/security/identity-protection/hello-for-business/hello-how-it-works-technology#cloud-experience-host).|
|DiagLog|Um log gerado pelo serviço de política de diagnóstico, que está documentado [aqui](/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server)|
|RadioMgr|Documentado [aqui](/windows-hardware/drivers/nfc/what-s-new-in-nfc-device-drivers)|
|ReadyBoot|Documentação [aqui](/previous-versions/windows/desktop/xperf/readyboot-analysis).|
|WDIContextLog|Interface do Driver de Dispositivo da WLAN (rede local sem fio) e documentado aqui. |
|WiFiDriverIHVSession|Documentado [aqui](/windows-hardware/drivers/network/user-initiated-feedback-normal-mode).|
|WiFiSession|Log de diagnóstico para a tecnologia WLAN. Se o Wi-Fi não estiver implementado, não haverá necessidade desse agente|
|WinPhoneCritical|Log de diagnóstico para o telefone (Windows?). Se não estiver usando telefones, esse agente não é necessário|

#### <a name="windows-defender-optimization-in-the-virtual-desktop-environment"></a>Otimização do Windows Defender no ambiente de área de trabalho virtual

Para obter mais detalhes sobre como otimizar o Windows Defender em um ambiente de área de trabalho virtual, confira o [Guia de implantação do Windows Defender Antivírus em um ambiente de VDI (Virtual Desktop Infrastructure)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

O artigo acima contém procedimentos para apresentar a imagem de área de trabalho virtual "ouro" e como manter os clientes de área de trabalho virtual enquanto eles estão em execução. Para reduzir a largura de banda da rede quando dispositivos de área de trabalho virtual precisarem atualizar suas assinaturas do Windows Defender, agende as reinicializações fora do horário comercial quando possível. As atualizações de assinatura do Windows Defender podem estar mantidas internamente em compartilhamentos de arquivos e, quando for conveniente, esses arquivos podem compartilhados nos mesmos segmentos de rede (ou próximos) que os dispositivos da área de trabalho virtual.

#### <a name="client-network-performance-tuning-by-registry-settings"></a>Ajuste de desempenho de rede do cliente nas configurações do Registro

Há algumas configurações do Registro que podem aumentar o desempenho da rede. Isso é especialmente importante em ambientes em que o dispositivo da área de trabalho virtual ou computador físico tem uma carga de trabalho que se baseia quase toda na rede. As configurações nesta seção são recomendadas para ajustar o desempenho do perfil de carga de trabalho de rede, configurando o buffer adicional e o cache de itens como entradas de diretório e assim por diante.

> [!NOTE]
> Algumas configurações desta seção são somente baseadas no Registro e devem ser incorporadas na imagem base antes que ela seja implantada para uso em produção.

As configurações a seguir estão documentadas nas [Diretrizes de ajuste de desempenho para o Windows Server 2016](../../administration/performance-tuning/index.md).

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

Aplica-se ao Windows 10. O padrão é **0**. Por padrão, o redirecionador SMB limita a taxa de transferência em conexões de rede de alta latência, em alguns casos, para evitar tempos limite relacionados à rede. Definir esse valor de Registro como **1** desabilita essa limitação, permitindo uma taxa de transferência de arquivo mais alta por conexões de rede de alta latência. Considere a possibilidade de definir esse valor como **1**.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax`

Aplica-se ao Windows 10. O padrão é **64**, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de metadados de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando muitos arquivos são acessados. Tente aumentar esse valor para **1024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

Aplica-se ao Windows 10. O padrão é **16**, com um intervalo válido de 1 a 4096. Esse valor é usado para determinar a quantidade de informações de diretório que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando diretórios grandes são acessados. Considere aumentar esse valor para **1024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

Aplica-se ao Windows 10. O padrão é **128**, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de informações de nome de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando muitos nomes de arquivo são acessados. Considere aumentar esse valor para **2048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

Aplica-se ao Windows 10. O padrão é **1023**. Esse parâmetro especifica o número máximo de arquivos que deve ser aberto em um recurso compartilhado após o aplicativo fechar o arquivo. Onde muitos milhares de clientes estiverem se conectando a servidores SMB, considere a redução desse valor para **256**.

É possível definir muitas configurações SMB usando os cmdlets [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps&preserve-view=true) e [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps&preserve-view=true) do Windows PowerShell. As configurações somente de Registro também podem ser definidas com o Windows PowerShell, como no seguinte exemplo:

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

#### <a name="additional-settings-from-the-windows-restricted-traffic-limited-functionality-baseline-guidance"></a>Configurações adicionais das diretrizes de Linha de Base da Funcionalidade Limitada de Tráfego Restrita pelo Windows

A Microsoft lançou uma linha de base criada usando os mesmos procedimentos das [Linhas de Base de Segurança do Windows](/windows/device-security/windows-security-baselines) para ambientes que não são conectados diretamente à Internet ou que querem reduzir dados enviados à Microsoft e outros serviços.

As configurações da [Linha de Base de Funcionalidade Limitada de Tráfego Restrita pelo Windows](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services) são indicadas na tabela da Política de Grupo com um asterisco.

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Limpeza de disco (incluindo o uso do assistente para Limpeza de Disco)

A limpeza de disco pode ser especialmente útil com implementações da área de trabalho virtual da imagem mestra/ouro. Depois que a imagem ouro/mestra é preparada, atualizada e configurada, uma das últimas tarefas a ser executada é a limpeza do disco. Os scripts de otimização no Github.com têm o código do PowerShell para executar tarefas comuns de limpeza de disco

> [!NOTE]
> Configurações de limpeza de disco e estão na categoria de Configurações do "Sistema" chamada "Armazenamento". Por padrão, o sensor de armazenamento é executado quando um limite de espaço livre em disco baixo é atingido.
>
> Para saber mais sobre como usar o sensor de armazenamento com imagens VHD personalizadas do Azure, confira [Preparar e personalizar uma imagem VHD mestra](/azure/virtual-desktop/set-up-customize-master-image).
>
> Para o host de sessão de Área de Trabalho Virtual do Windows que usa multisseção do Windows 10 Enterprise ou Windows 10 Enterprise, é recomendável desabilitar o sensor de armazenamento. Você pode desabilitar o sensor de armazenamento no menu Configurações em **Armazenamento**.

Veja a seguir algumas sugestões para diversas tarefas de limpeza de disco. Todas elas devem ser testadas antes da implementação:

1. O sensor de armazenamento pode ser utilizado manual ou automaticamente. Para obter mais informações sobre o sensor de armazenamento, confira este artigo: Use o OneDrive e o sensor de armazenamento no Windows 10 para gerenciar o espaço em disco
2. Limpe manualmente arquivos e logs temporários. Em um prompt de comandos com privilégios elevados, execute estes comandos:

   1. `Del C:\*.tmp /s`
   2. `C:\*.etl /s`
   3. `C:\*.evtx /s`

   ```powershell
   Get-ChildItem -Path c:\ -Include *.tmp, *.dmp, *.etl, *.evtx, thumbcache*.db, *.log -File -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\Temp\* -Recurse -Force -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\ReportArchive\* -Recurse -Force -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\ReportQueue\* -Recurse -Force -ErrorAction SilentlyContinue

   Clear-RecycleBin -Force -ErrorAction SilentlyContinue

   Clear-BCCache -Force -ErrorAction SilentlyContinue
   ```

3. Exclua os perfis não utilizados no sistema executando o seguinte comando:

    `wmic path win32_UserProfile where LocalPath="C:\\users\\<users>" Delete`

Para perguntas ou questões sobre as informações neste artigo, entre em contato com a equipe de contas Microsoft, pesquise o [blog do profissional de TI sobre área de trabalho virtual da Microsoft](https://community.windows.com/stories/virtual-desktop-windows-10), poste uma mensagem nos [fóruns da área de trabalho virtual da Microsoft](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/bd-p/WindowsVirtualDesktop) ou entre em contato com a [Microsoft](https://support.microsoft.com/contactus/).

### <a name="re-enable-windows-update"></a>Reabilitar o Windows Update

Se você quiser habilitar o uso de Windows Update depois de desabilitá-lo, como no caso de área de trabalho virtual persistente, siga estas etapas:

1. Reabilitar as configurações da política de grupo:

   - Vá para **Política de Computador Local** > **Configuração do Computador** > **Modelos Administrativos** > **Sistema** > **Gerenciamento de Comunicação da Internet** > **Configurações de Comunicação da Internet**.
     - Desligue o acesso a todos os recursos de Windows Update alterando a configuração de **habilitado** para **não configurado**.
   - Vá para **Política do Computador Local** > **Configuração do Computador** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update**.
     - Remova o acesso a todos os recursos de Windows Update alterando a configuração de **habilitado** para **não configurado**.
     - Não se conecte a nenhum local Windows Update Internet alterando a configuração de **habilitada** para **não configurada**.
   - Vá para **Política do Computador Local** > **Configuração do Computador** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update** > **Windows Update para Empresas**.
     - Selecione quando as atualizações de qualidade são recebidas (altere **habilitado** para **não configurado**)
   - Vá para **Política do Computador Local** > **Configuração do Computador** > **Modelos Administrativos** > **Componentes do Windows** > **Windows Update** > **Windows Update para Empresas**.
     - Selecione quando as versões prévias e as atualizações de recursos são recebidas (altere de **habilitado** para **não configurado**)

2. Reabilitar serviço(s):

   - Altere **Atualizar serviço do Orchestrator** de **desabilitado** para **Automático (Início Adiado)** .

3. Edite o Registro do Windows (aviso: tenha cuidado ao editar o Registro).

   - Acesse `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState`.
     - Altere **DeferQualityUpdates** de "1" para "0".
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings`
     - Exclua qualquer valor para **PausedQualityDate**.
   - Acesse `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU`
     - Definido como **Desabilitado**.

4. Reabilitar tarefas agendadas:

   - Vá para **Biblioteca do Agendador de Tarefas** > **Microsoft** > **Windows** > **InstallService** > **ScanForUpdates**.
   - Vá para **Biblioteca do Agendador de Tarefas** > **Microsoft** > **Windows** > **InstallService** > **ScanForUpdatesAsUser**.

5. Reinicie o dispositivo para fazer todas essas configurações entrarem em vigor.

6. Caso não deseje que este dispositivo receba as atualizações de recursos, acesse **Configurações** > **Windows Update** > **Opções avançadas** > **Escolha quando as atualizações são instaladas** e defina manualmente a opção **Uma atualização de recursos inclui novas funcionalidades e aprimoramentos. Ela pode ser adiada por esta quantidade de dias** para qualquer valor diferente de zero, como 180, 365 etc.

## <a name="additional-information"></a>Informações adicionais

Saiba mais sobre a arquitetura de VDI da Microsoft na [documentação da Área de Trabalho Virtual do Windows](https://azure.microsoft.com/services/virtual-desktop/).

Se precisar de mais ajuda com a solução de problemas do Sysprep, confira [O Sysprep falha após remover ou atualizar aplicativos da Microsoft Store que incluem imagens internas do Windows](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu).
