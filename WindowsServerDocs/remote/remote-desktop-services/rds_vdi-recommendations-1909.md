---
title: Como otimizar o Windows 10, versão 1909, para uma função da VDI (Virtual Desktop Infrastructure)
description: Configurações e definições recomendadas para minimizar a sobrecarga para desktops Windows 10, versão 1909, usados como imagens da VDI.
ms.reviewer: robsmi
ms.author: helohr
ms.topic: article
author: heidilohr
manager: lizross
ms.date: 02/19/2020
ms.openlocfilehash: b0ff8f353d4536f89d698f362e2998d9682665f2
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389010"
---
# <a name="optimizing-windows-10-version-1909-for-a-virtual-desktop-infrastructure-vdi-role"></a>Como otimizar o Windows 10, versão 1909, para uma função da VDI (Virtual Desktop Infrastructure)

Este artigo ajudará você a escolher as configurações para o Windows 10, versão 1909 (build 18363) que deverão resultar no melhor desempenho em um ambiente VDI (Virtual Desktop Infrastructure). Todas as configurações deste guia são recomendações a serem consideradas, não sendo, de modo algum, requisitos.

As principais maneiras de otimizar o desempenho do Windows 10 em um ambiente VDI são minimizar os redesenhos de elementos gráficos do aplicativo e as atividades em segundo plano sem grande vantagem para o ambiente VDI e, normalmente, reduzir os processos em execução ao mínimo necessário. Uma meta secundária é reduzir o uso do espaço em disco na imagem base para o mínimo necessário. Com as implementações da VDI, a menor base possível (ou tamanho "ouro" de imagem) pode reduzir ligeiramente o uso de memória no hipervisor, bem como provocar uma pequena redução nas operações gerais de rede necessárias para fornecer a imagem de área de trabalho ao consumidor.

> [!NOTE]
> Essas configurações recomendadas podem ser aplicadas a outras instalações do Windows 10 1909, incluindo aquelas em computadores físicos ou outras máquinas virtuais. Nenhuma recomendação deste artigo deverá afetar a capacidade de suporte do Windows 10 1909.

## <a name="vdi-optimization-principles"></a>Princípios da otimização da VDI

Um ambiente VDI apresenta uma sessão de área de trabalho completa, incluindo aplicativos, a um usuário de computador em uma rede. O veículo de entrega de rede pode ser uma rede local ou a Internet. Os ambientes VDI são uma imagem "base" do sistema operacional, que então se torna a base para as áreas de trabalho apresentadas em seguida aos usuários. Há variações das implementações da VDI, como "persistente", "não persistente" e "sessão da área de trabalho". O tipo persistente preserva as alterações no sistema operacional da área de trabalho da VDI de uma sessão para a outra. O tipo não persistente não preserva as alterações no sistema operacional da área de trabalho da VDI de uma sessão para a outra. Para o usuário, essa área de trabalho não é muito diferente de nenhum outro dispositivo físico ou virtual, exceto pelo fato de ela ser acessada em uma rede.

As configurações de otimização podem ocorrer em um dispositivo de referência. Uma VM é um lugar ideal para criar a imagem, pois o estado pode ser salvo, os pontos de verificação podem ser criados e os backups podem ser feitos. Uma instalação padrão do sistema operacional é executada na VM base. Essa VM base é então otimizada com a remoção de aplicativos desnecessários, a instalação das atualizações do Windows, a instalação de outras atualizações, a exclusão de arquivos temporários e a aplicação de configurações.

Há outros tipos de VDI, como RDS (Sessão da Área de Trabalho Remota) e a recentemente lançada [Área de Trabalho Virtual do Windows](https://azure.microsoft.com/services/virtual-desktop/). Uma discussão aprofundada sobre essas tecnologias não está no escopo deste artigo. Este artigo se concentra nas configurações da imagem base do Windows, sem referência a outros fatores no ambiente, como a otimização do host.

Segurança e estabilidade são as principais prioridades da Microsoft quando se trata de produtos e serviços. Os clientes empresariais podem optar por utilizar a Segurança interna do Windows, um conjunto de serviços que funciona bem com ou sem a Internet. Para os ambientes VDI não conectados à Internet, as assinaturas de segurança podem ser baixadas várias vezes por dia, pois a Microsoft pode lançar mais de uma atualização de assinatura por dia. Essas assinaturas podem então ser fornecidas às VMs da VDI e agendadas para serem instaladas durante a produção, independentemente de serem persistentes ou não persistentes. Dessa forma, a proteção da VM é a mais atual possível.

Há algumas configurações de segurança que não são aplicáveis aos ambientes VDI que não estão conectados à Internet e, portanto, não podem participar da segurança habilitada para nuvem. Há outras configurações que os dispositivos "normais" do Windows podem utilizar, como a Experiência de Nuvem, a Microsoft Store etc. A remoção do acesso aos recursos não utilizados reduz o volume, a largura de banda da rede e a superfície de ataque.

Em relação às atualizações, o Windows 10 utiliza um algoritmo de atualização mensal, não havendo, portanto, a necessidade de uma tentativa de atualização por parte dos clientes. Na maioria dos casos, os administradores da VDI controlam o processo de atualização por meio de um processo de desligamento de VMs com base em uma imagem "mestra" ou "ouro", desselam a imagem que é somente leitura, aplicam o patch a ela e, em seguida, selam a imagem novamente e a colocam de novo em produção. Portanto, não é necessário ter VMs da VDI verificando o Windows Update. Em alguns casos, por exemplo, VMs da VDI persistentes, são realizados os procedimentos normais de aplicação de patch. O Windows Update ou o Microsoft Intune também podem ser usados. O System Center Configuration Manager pode ser usado para lidar com a atualização e outros tipos de entrega de pacote. Cabe a cada organização determinar a melhor abordagem para atualizar a VDI.

> [!TIP]
> Um script que implementa as otimizações abordadas neste tópico – bem como um arquivo de exportação de GPO que você pode importar com **LGPO.exe** – está disponível em [TheVDIGuys](https://github.com/TheVDIGuys) no GitHub.

Esse script foi projetado de acordo com o ambiente e os requisitos. O código principal é o PowerShell, e o trabalho é feito usando arquivos de entrada (texto sem formatação), com os arquivos de exportação da ferramenta LGPO (Objeto de Política de Grupo Local). Esses arquivos contêm listas de aplicativos a serem removidos e serviços a serem desabilitados. Caso não deseje remover um aplicativo específico nem desabilitar um serviço específico, edite o arquivo de texto correspondente e remova o item. Por fim, há configurações de política local que podem ser importadas para o dispositivo. É melhor ter algumas configurações na imagem base do que aplicar as configurações por meio da Política de Grupo, pois algumas das configurações entram em vigor na próxima reinicialização ou quando um componente é usado pela primeira vez.

### <a name="persistent-vdi"></a>VDI persistente

A VDI persistente é, no nível básico, uma VM que salva os estados do sistema operacional entre as reinicializações. Outras camadas de software da solução VDI fornecem aos usuários acesso fácil e contínuo às respectivas VMs atribuídas, muitas vezes, com uma solução de logon único.

Há várias implementações diferentes da VDI persistente:

- A máquina virtual tradicional, em que a VM tem o próprio arquivo de disco virtual, é inicializada normalmente e salva as alterações de uma sessão para a outra. A única diferença está em como o usuário acessa essa VM. Pode haver um portal da Web ao qual o usuário se conecta que o direciona automaticamente para uma ou mais de suas VMs de VDI atribuídas.

- A máquina virtual persistente baseada em imagem, opcionalmente, com discos virtuais pessoais. Nesse tipo de implementação, há uma imagem base/gold em um ou mais servidores host. Uma VM é criada, assim como um ou mais discos virtuais são criados e atribuídos a esse disco para armazenamento persistente.

    - Quando a VM é iniciada, uma cópia da imagem base é lida na memória da VM. Ao mesmo tempo, um disco virtual persistente é atribuído a essa VM, com todas as alterações do sistema operacional anterior mescladas por meio de um processo complexo.

    - As alterações, como gravações do log de eventos, gravações de log etc., são redirecionadas para o disco virtual de leitura/gravação atribuído a essa VM.

    - Nessa circunstância, o sistema operacional e a manutenção do aplicativo podem funcionar normalmente usando o software de manutenção tradicional, como o Windows Server Update Services ou outras tecnologias de gerenciamento.

    - A diferença entre um computador de VDI persistente e uma máquina virtual "normal" é a relação com a imagem mestra/ouro. Em algum ponto, as atualizações precisam ser aplicadas ao mestre. É nesse momento que as implementações decidem como as alterações persistentes do usuário são tratadas. Em alguns casos, o disco com as alterações é descartado e/ou redefinido, definindo, assim, um novo ponto de verificação. Também pode ocorrer que as alterações feitas pelo usuário são mantidas por meio de atualizações de qualidade mensal e a base é redefinida após uma atualização de recursos.

### <a name="non-persistent-vdi"></a>VDI não persistente

Quando uma implementação de VDI não persistente é baseada em uma imagem base ou "ouro", as otimizações são executadas principalmente na imagem base e por meio das configurações e políticas locais.

Com a VDI não persistente baseada em imagem, a imagem base é somente leitura. Quando uma VM não persistente é iniciada, uma cópia da imagem base é transmitida para a VM. A atividade que ocorre durante a inicialização e daí em diante até a próxima reinicialização é redirecionada para um local temporário. Normalmente, os usuários recebem locais de rede para armazenar os dados. Em alguns casos, o perfil do usuário é mesclado com a VM Standard para fornecer as configurações ao usuário.

Um importante aspecto da VDI não persistente que se baseia em uma só imagem é a manutenção. As atualizações no sistema operacional e nos componentes geralmente são fornecidas uma vez por mês. Com a VDI baseada em imagem, há um conjunto de processos que precisa ser executado para obter atualizações para a imagem:

- Em um host especificado, todas as VMs nesse host que são derivadas da imagem base precisam ser desligadas/desativadas. Isso significa que os usuários são redirecionados para outras VMs.

- A imagem base é aberta e iniciada. Todas as atividades de manutenção são executadas, como atualizações do sistema operacional, atualizações do .NET, atualizações de aplicativo etc.

- Todas as configurações novas que precisam ser aplicadas são aplicadas nesse momento.

- Qualquer outra manutenção é realizada nesse momento.

- A imagem base é desligada.

- A imagem base é validada e definida para voltar à produção.

- Os usuários podem fazer logon novamente.

> [!NOTE]
> O Windows 10 executa, periodicamente, um conjunto de tarefas de manutenção de modo automático. Há uma tarefa agendada que é definida para ser executada, por padrão, às 3h todos os dias. Essa tarefas agendada executa uma lista de tarefas, incluindo a limpeza do Windows Update. Com este comando do PowerShell, você pode exibir todas as categorias de manutenção que ocorrem automaticamente:
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

Um dos desafios de uma VDI não persistente é que quase toda a atividade do sistema operacional é descartada quando um usuário faz logoff. O estado e/ou o perfil do usuário pode ser salvo em uma localização centralizada, mas a máquina virtual em si descarta quase todas as alterações que foram feitas desde a última reinicialização. Portanto, as otimizações destinadas a um computador Windows que salva o estado de uma sessão para outra não são aplicáveis.

Dependendo da arquitetura da VM de VDI, itens como PreFetch e SuperFetch não vão ajudar de uma sessão para outra, pois todas as otimizações são descartadas na reinicialização da VM. A indexação pode ser um desperdício parcial de recursos, pois seria qualquer otimização de disco, como uma desfragmentação tradicional.

> [!NOTE]
> Se estiver preparando uma imagem usando a virtualização e se estiver conectado à Internet durante o processo de criação da imagem, no primeiro logon, você deverá adiar as atualizações de recursos acessando **Configurações**, **Windows Update**.

### <a name="to-sysprep-or-not-sysprep"></a>Para sysprep ou não sysprep

O Windows 10 tem uma funcionalidade interna chamada [Ferramenta de Preparação do Sistema](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) (geralmente abreviada para "Sysprep"). A ferramenta Sysprep é usada para preparar uma imagem personalizada do Windows 10 para duplicação. O processo Sysprep garante que o sistema operacional resultante seja adequadamente exclusivo para ser executado no ambiente de produção.

Há argumentos a favor e contra a execução de Sysprep. No caso da VDI, talvez seja conveniente a capacidade de personalizar o perfil do usuário padrão que será usado como o modelo de perfil para usuários subsequentes que fazem logon usando essa imagem. Você pode ter os aplicativos que deseja instalados, mas também pode controlar as configurações por aplicativo.

A alternativa é usar um ISO padrão a partir do qual instalar, possivelmente usando um arquivo de resposta de instalação autônoma, bem como uma sequência de tarefas para instalar ou remover aplicativos. Use também uma sequência de tarefas para definir as configurações da política local na imagem, talvez usando a ferramenta [LGPO (Utilitário de Objeto de Política de Grupo Local)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

### <a name="supportability"></a>Capacidade de suporte

Sempre que os padrões do Windows são alterados, surgem perguntas em relação à capacidade de suporte. Depois que uma imagem da VDI (VM ou sessão) é personalizada, todas as alterações feitas na imagem precisam ser controladas em um log de alterações. Na solução de problemas, com frequência, uma imagem pode ser isolada em um pool e configurada para análise de problemas. Depois que um problema é acompanhado até a causa raiz, essa alteração pode então ser distribuída para o ambiente de teste primeiro e, finalmente, para a carga de trabalho de produção.

Este documento evita intencionalmente abordar serviços, políticas ou tarefas do sistema que afetam a segurança. Depois disso, vem o Serviço do Windows. A capacidade de atender imagens da VDI fora das janelas de manutenção é removida, pois as janelas de manutenção existem quando a maioria dos eventos de serviço ocorre em ambientes VDI, *com exceção das atualizações de software de segurança*. A Microsoft publicou diretrizes para a Segurança do Windows em ambientes VDI. Para obter mais informações, confira [Guia de implantação do Windows Defender Antivírus em um ambiente VDI (Virtual Desktop Infrastructure)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

Considere a capacidade de suporte ao alterar as configurações padrão do Windows. Problemas difíceis podem surgir ao alterar serviços, políticas ou tarefas agendadas do sistema, em nome de proteção, "torná-lo mais leve" etc. Consulte a Base de Dados de Conhecimento Microsoft para obter os atuais problemas conhecidos sobre as configurações padrão alteradas. As diretrizes deste documento e o script associado no GitHub serão mantidos com relação a problemas conhecidos, se surgirem. Além disso, você poderá relatar problemas de várias maneiras à Microsoft.

Use seu mecanismo de pesquisa favorito com os termos "valor inicial" site:support.microsoft.com” para mostrar os problemas conhecidos relacionados aos valores iniciais padrão dos serviços.

Você poderá observar que este documento e os scripts associados no GitHub não modificam nenhuma permissão padrão. Se estiver interessado em aumentar as configurações de segurança, comece com o projeto conhecido como **AaronLocker**. Para obter mais informações, confira [Visão geral de "AaronLocker"](https://github.com/microsoft/AaronLocker).

#### <a name="vdi-optimization-categories"></a>Categorias de otimização da VDI

- Categorias de configurações globais do sistema operacional:

    - Limpeza do aplicativo UWP

    - Limpeza dos recursos opcionais

    - Configurações de política local

    - Serviços do sistema

    - Tarefas Agendadas

    - Aplicar atualizações do Windows (entre outras)

    - Rastreamentos automáticos do Windows

    - Limpeza de disco antes da finalização da imagem (validação)

    - Configurações do usuário

    - Configurações do host/do hipervisor

### <a name="universal-windows-platform-uwp-application-cleanup"></a>Limpeza de aplicativos do UWP (Plataforma Universal do Windows)

Um dos objetivos de uma imagem da VDI é ser tão leve quanto possível. Uma forma de reduzir o tamanho da imagem é remover os aplicativos UWP que não serão usados no ambiente. Com aplicativos UWP, há os principais arquivos de aplicativo, também conhecidos como conteúdo. Há uma pequena quantidade de dados armazenados em cada perfil de usuário para configurações específicas do aplicativo. Também há uma pequena quantidade de dados no perfil Todos os Usuários.

Conectividade e tempo são fatores importantes quando se trata da limpeza de aplicativos UWP. Se você implantar a imagem base em um dispositivo sem conectividade de rede, o Windows 10 não poderá se conectar à Microsoft Store e baixará aplicativos e tentará instalá-los enquanto você estiver tentando desinstalá-los. Essa pode ser uma boa estratégia para personalizar a imagem e, em seguida, atualizar o que permanecerá em um estágio posterior do processo de criação da imagem.

Se você modificar o .WIM base usado para instalar o Windows 10 e remover os aplicativos UWP desnecessários do .WIM antes da instalação, os aplicativos não serão instalados logo no início e o tempo de criação do seu perfil será mais curto. Mais adiante nesta seção, você encontrará informações sobre como remover aplicativos UWP do arquivo .WIM de instalação.

Uma boa estratégia de VDI é provisionar os aplicativos que deseja na imagem base e limitar ou bloquear o acesso subsequente à Microsoft Store. Os aplicativos da Store são atualizados periodicamente em segundo plano em computadores normais. Os aplicativos UWP podem ser atualizados durante a janela de manutenção quando outras atualizações são aplicadas. Para obter mais informações, confira [Aplicativos da Plataforma Universal do Windows](https://docs.citrix.com/citrix-virtual-apps-desktops/manage-deployment/applications-manage/universal-apps.html)

#### <a name="delete-the-payload-of-uwp-apps"></a>Excluir o conteúdo de aplicativos UWP

Os aplicativos UWP que não são necessários continuam no sistema de arquivos consumindo um pequeno volume de espaço em disco. Em aplicativos que nunca serão necessários, o conteúdo de aplicativos UWP indesejados pode ser removido da imagem base usando comandos do PowerShell.

Na verdade, se forem removidos do arquivo .WIM de instalação usando os links fornecidos mais adiante nesta seção, deverá ser possível começar com uma lista bem enxuta de aplicativo UWP.

Execute o seguinte comando para enumerar os aplicativos UWP provisionados em um sistema operacional em execução, como nesta saída de exemplo truncada do PowerShell:

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0
    Architecture : neutral
    ResourceId   : \~
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
    ...
```

Os aplicativos UWP que são provisionados para um sistema podem ser removidos durante a instalação do sistema operacional como parte de uma sequência de tarefas, ou posteriormente, depois que o sistema operacional estiver instalado. Esse pode ser o método preferido, pois torna modular todo o processo geral de criar ou manter uma imagem. Depois que você desenvolver os scripts, se algo mudar em um build seguinte, você editará um script existente em vez de repetir o processo do zero. Veja a seguir alguns links para informações sobre este tópico:

[Removendo aplicativos nativos do Windows 10 durante uma sequência de tarefas](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence)

[Removendo aplicativos internos do arquivo WIM do Windows 10 com o Powershell – versão 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)

[Windows 10 1607: impedindo aplicativos de retornarem durante a implantação da atualização de recursos](/archive/blogs/mniehaus/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update)

Execute o comando do PowerShell [Remove-AppxProvisionedPackage](/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) para remover o conteúdo do aplicativo UWP:

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

Cada aplicativo UWP deve ser avaliado para aplicabilidade em cada ambiente exclusivo. O ideal será fazer uma instalação padrão do Windows 10 1909 e, em seguida, observar quais aplicativos estão sendo executados e consumindo memória. Por exemplo, considere a possibilidade de remover os aplicativos que são iniciados automaticamente ou os aplicativos que exibem informações automaticamente no menu Iniciar, como Clima e Notícias, e que podem não ser úteis no ambiente.

> [!NOTE]
> Se você estiver utilizando os scripts do GitHub, poderá controlar com facilidade quais aplicativos serão removidos antes de executar o script. Depois de baixar os arquivos de script, localize o arquivo "AppxPackages.json", edite-o e remova as entradas de aplicativos que deseja manter, como Calculadora, Notas Autoadesivas etc. Confira [a Personalização da seção](https://github.com/TheVDIGuys/Windows_10_VDI_Optimize#customization) para obter detalhes.

### <a name="manage-windows-optional-features-using-powershell"></a>Gerenciar recursos opcionais do Windows usando o PowerShell

Gerencie recursos opcionais do Windows usando o PowerShell. Para obter mais informações, confira o [fórum do PowerShell no Windows Server](/answers/topics/windows-server-powershell.html). Para enumerar os recursos do Windows atualmente instalados, execute o seguinte comando do PowerShell:

```powershell
Get-WindowsOptionalFeature -Online
```

Habilite ou desabilite um recurso opcional específico do Windows, como mostrado neste exemplo:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

Desabilite recursos na imagem da VDI, conforme mostrado neste exemplo:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName "WindowsMediaPlayer"
```

Em seguida, o ideal será remover o pacote do Windows Media Player. Há dois pacotes do Windows Media Player no Windows 10 1909:

```powershell
Get-WindowsPackage -Online -PackageName *media*

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : DISM Package Manager Provider
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1.mum
InstallTime       : 3/19/2019 6:20:22 AM
...

Features         : {}

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : UpdateAgentLCU
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449.mum
InstallTime       : 10/29/2019 5:15:17 AM
...
```

Caso deseje remover o pacote do Windows Media Player (para liberar cerca de 60 MB de espaço em disco):

```powershell
 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online

 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449 -Online
```

#### <a name="enable-or-disable-windows-features-using-dism"></a>Habilitar ou desabilitar recursos do Windows usando o DISM

Use a ferramenta Dism.exe para enumerar e controlar os recursos opcionais do Windows. Um script do Dism.exe pode ser desenvolvido e executado durante uma sequência de tarefas de instalação do sistema operacional. A tecnologia do Windows envolvida é chamada [Recursos sob Demanda](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

#### <a name="default-user-settings"></a>Configurações padrão de usuário

Há personalizações que podem ser feitas em um arquivo do Registro do Windows chamado "C:\Users\Default\NTUSER.DAT". Todas as configurações feitas nesse arquivo serão aplicadas a todos os perfis de usuário seguintes criados com base em um dispositivo que executa essa imagem. Controle quais configurações serão aplicadas ao perfil do usuário padrão editando o arquivo "DefaultUserSettings.txt". Uma configuração que talvez você queira considerar com atenção e que é nova nesta iteração de recomendações de configurações é uma configuração chamada **TaskbarSmallIcons**. O ideal é verificar com a base de usuários antes de implementar essa configuração. **TaskbarSmallIcons** torna a barra de tarefas do Windows menor e consome menos espaço da tela, torna os ícones mais compactos, minimiza a interface de Pesquisa e é representado antes e depois nas seguintes ilustrações:

Figura 1: barra de tarefas normal do Windows 10, versão 1909

![Versão padrão da barra de tarefas do Windows 10, versão 1909](media/rds-vdi-recommendations-1909/standard-taskbar.png)

Figura 2: barra de tarefas usando a configuração de ícones pequenos

![Barra de tarefas usando a configuração de ícones pequenos](media/rds-vdi-recommendations-1909/taskbar-sm-icons.png)

Além disso, para reduzir a transmissão de imagens na infraestrutura da VDI, você pode definir a tela de fundo padrão com uma cor sólida em vez da imagem padrão do Windows 10. Você também pode definir a tela de logon com uma cor sólida, bem como desligar o efeito de desfoque opaco no logon.

As configurações a seguir são aplicadas ao hive do Registro do perfil do usuário padrão, principalmente para reduzir as animações. Se algumas ou todas essas configurações não forem desejadas, exclua as configurações que não serão aplicadas aos novos perfis de usuário com base nessa imagem. O objetivo dessas configurações é habilitar as seguintes configurações equivalentes:

Figura 3: Propriedades do Sistema otimizadas, Opções de Desempenho

![Propriedades do Sistema otimizadas, Opções de Desempenho](media/rds-vdi-recommendations-1909/performance-options.png)

Para o Windows 10, versão 1909, veja abaixo as configurações de otimização aplicadas ao hive do Registro do perfil do usuário padrão para otimizar o desempenho:

```dos
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
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.PPIProjection_cw5n1h2txyewy" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.PPIProjection_cw5n1h2txyewy" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitInkCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitTextCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Personalization\Settings" /v AcceptedPrivacyPolicy /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization\TrainedDataStore" /v HarvestContacts /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKCU\Software\Microsoft\InputPersonalization" /v RestrictImplicitInkCollection /t REG_DWORD /d 1 /f
add "HKCU\Software\Microsoft\InputPersonalization" /v RestrictImplicitTextCollection /t REG_DWORD /d 1 /f
```

Nas configurações de política local, o ideal é desabilitar imagens para telas de fundo na VDI. Caso você deseje exibir imagens, o ideal é criar imagens da tela de fundo personalizadas com uma profundidade de cor reduzida para limitar a largura de banda de rede usada para transmitir informações da imagem. Se você optar por não especificar nenhuma imagem da tela de fundo na política local, o ideal será definir a cor da tela de fundo antes de definir a política local, porque, depois que a política for definida, o usuário não terá como alterar a cor da tela de fundo. Talvez seja melhor especificar "(nulo)" como a imagem da tela de fundo. Há outra configuração de política na próxima seção sobre o não uso da tela de fundo em sessões do protocolo RDP.

### <a name="local-policy-settings"></a>Configurações de política local

Muitas otimizações para Windows 10 em um ambiente VDI podem ser feitas usando a política do Windows. As configurações listadas na tabela desta seção podem ser aplicadas localmente à imagem base/ouro. Se as configurações equivalentes não forem especificadas de nenhuma outra maneira, como a política de grupo, elas ainda serão aplicadas.

Algumas decisões podem ser baseadas nas especificações do ambiente, por exemplo:

- O ambiente VDI tem permissão para acessar a Internet?

- A solução VDI é persistente ou não persistente?

As configurações a seguir foram escolhidas para não contrariar nem entrar em conflito com qualquer configuração relacionada à segurança. Essas configurações foram escolhidas para remover as configurações ou desabilitar as funcionalidades que possam não ser aplicáveis aos ambientes VDI.

| Configuração de política | Item | Subitem | Configuração possível e comentários|
| -------------- | ---- | -------- | ---------------------------- |
| Política do Computador Local \\ Configuração do Computador \\ Configurações do Windows \\ Configurações de Segurança | | | |
| Políticas do Gerenciador de Listas de Redes | Todas as propriedades de redes | Local da rede | O usuário não pode alterar a localização |
| Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Painel de Controle | | | |
| *Painel de Controle | Permitir dicas online | | Desabilitado. As configurações não contatarão os serviços de conteúdo da Microsoft para recuperar dicas e conteúdo da Ajuda. |
| *Painel de Controle\Personalização | Não mostrará a tela de bloqueio | Habilitada. Esta configuração controla se a tela de bloqueio é exibida para os usuários. Se você habilitar esta configuração de política, os usuários que não precisarem clicar em CTRL + ALT + DEL antes de se conectarem verão o respectivo bloco selecionado após o bloqueio do computador. |
| *Painel de Controle\Personalização | Forçar uma imagem de logon e tela de bloqueio padrão específica | [![Interface do usuário usada para definir o caminho para a tela de bloqueio](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | Habilitada. Esta configuração permite que você especifique a tela de bloqueio padrão e a imagem de logon mostradas quando nenhum usuário está conectado, além de definir a imagem especificada como o padrão para todos os usuários – ela substitui a imagem padrão.<p>Recomendamos usar uma imagem não complexa de baixa resolução, de modo que menos dados sejam transmitidos na rede toda vez que a imagem é renderizada. |
| *Painel de Controle\Opções Regionais e de Idioma\Personalização de manuscrito | Desativar aprendizado automático | | Habilitada. Se você habilitar esta configuração de política, o aprendizado automático será interrompido e os dados armazenados serão excluídos. Os usuários não podem definir essa configuração no Painel de Controle. |
| Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Rede | | | |
| BITS | Não permitir que o cliente BITS use o Windows Branch Cache |  | Habilitada |
| BITS | Não permitir que o computador aja como um cliente de cache de sistemas pares do BITS |  | Habilitada |
| BITS | Não permitir que o computador aja como um servidor de cache de sistemas pares do BITS |  | Habilitada |
| BITS | Permitir cache de sistemas pares do BITS |  | Desabilitado |
| BranchCache | Habilitar o BranchCache |  | Desabilitado |
| *Fontes | Habilitar provedores de fontes |  | Desabilitado. O Windows não se conecta a um provedor de fontes online e enumera apenas as fontes instaladas localmente. |
| Autenticação do hotspot | Habilitar a autenticação do hotspot |  | Desabilitado |
| Serviços de rede ponto a ponto da Microsoft | Desligar os Serviços de rede ponto a ponto da Microsoft |  | Habilitada |
| Indicador de status da conectividade de rede | Especificar a sondagem passiva. | Desabilitar a sondagem passiva (caixa de seleção) | Habilitada. Use esta configuração se estiver em uma rede isolada ou usando um endereço IP estático. |
| Arquivos Offline | Permitir ou impedir o uso de Arquivos Offline. |  | Desabilitado |
| Configurações TCPIP \\ Tecnologias de Transição IPv6 | Definir o estado Teredo | Estado Desabilitado | Habilitada. No estado desabilitado, não há a presença de interfaces Teredo no host. |
| Serviço WLAN \\ Configurações de WLAN | Permitir que o Windows se conecte automaticamente a hotspots abertos sugeridos, redes compartilhadas por contatos e hotspots que oferecem serviços pagos. |  | Desabilitado. As configurações **Conectar a hotspots abertos sugeridos**, **Conectar a redes compartilhadas por meus contatos** e **Habilitar serviços pagos** estão desativadas, mas os usuários nesse dispositivo podem habilitá-las. |
| Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ menu Iniciar e barra de tarefas |  |  |  |
| *Notificações | Desabilitar o uso da rede para notificações |  | Habilitada. Se você habilitar esta configuração de política, os aplicativos e os recursos do sistema não poderão receber notificações do WNS pela rede nem usando APIs de sondagem de notificação. |
| Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Sistema |  |  |  |
| Instalação do dispositivo | Não enviar um Relatório de Erros do Windows quando um driver genérico for instalado em um dispositivo |  | Habilitada |
| Instalação do dispositivo | Impedir a criação de um ponto de restauração do sistema durante a atividade do dispositivo que normalmente solicitará essa criação. |  | Habilitada |
| Instalação do dispositivo | Impedir a recuperação de metadados do dispositivo da Internet |  | Habilitada |
| Instalação do dispositivo | Impedir o Windows de enviar um relatório de erros quando um driver de dispositivo solicitar software adicional durante a instalação |  | Habilitada |
| Instalação do dispositivo | Desligar os **balões de Novo Hardware Encontrado** durante a instalação de dispositivos. |  | Habilitada |
| Sistema de Arquivos \\ NTFS | Opções de criação de nome curto | Desabilitado em todos os volumes | Habilitada |
| *Política de grupo | Configurar vinculação entre a Web e os aplicativos com manipuladores de URL do aplicativo |  | Desabilitado. Desabilita a vinculação entre Web e aplicativo, e os URIs HTTP(S) são abertos no navegador padrão em vez de iniciar o aplicativo associado. |
| *Política de grupo | Continuar experiências neste dispositivo. |  | Desabilitado. O dispositivo Windows não é detectável por outros dispositivos e não pode participar de experiências entre dispositivos. |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar o acesso a todos os recursos do Windows Update |  |Habilitada. Se você habilitar esta configuração de política, todos os recursos do Windows Update serão removidos. Isso inclui bloquear o acesso ao site do Windows Update em https://windowsupdate.microsoft.com, do hiperlink do Windows Update no menu Iniciar e também no menu Ferramentas no Internet Explorer. A atualização automática do Windows também está desabilitada; você não receberá atualizações críticas do Windows Update nem será notificado sobre elas. Esta configuração de política também impede que o Gerenciador de Dispositivos instale automaticamente atualizações de driver do site Windows Update. |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar Atualização Automática de Certificados Raiz |  |Habilitada. Se você habilitar esta configuração de política, quando um certificado emitido por uma autoridade raiz não confiável for apresentado, seu computador não contatará o site Windows Update para verificar se a Microsoft adicionou a AC à lista de autoridades confiáveis. OBSERVAÇÃO: use essa política somente se você tiver um meio alternativo para a lista de certificados revogados mais recente. |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar links "Events.asp" do Visualizador de Eventos |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar compartilhamento de dados de personalização de manuscrito |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar relatório de erros de reconhecimento de manuscrito |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar conteúdo do tipo "Você sabia" no Centro de Ajuda e Suporte conteúdo |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar pesquisa do Centro de Ajuda e Suporte na Base de Dados de Conhecimento Microsoft |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar o Assistente para Conexão com a Internet se a URL de conexão fizer referência a Microsoft.com |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar o download da Internet para assistentes de publicação na Web e de encomendas online |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar o serviço de associação de arquivo pela Internet |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar Registro se a URL de conexão fizer referência a Microsoft.com |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar a tarefa "Encomendar Cópias" de fotos |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar a tarefa "Publicar na Web" de arquivos e pastas |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar o Programa de Satisfação do Usuário do Windows Messenger |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar o Programa de Aperfeiçoamento da Experiência do Usuário do Windows |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar testes ativos do Indicador do Status da Conectividade da Rede do Windows |  | Habilitada. Esta configuração de política desativa os testes ativos executados pelo NCSI (Indicador do Status da Conectividade de Rede) do Windows para determinar se o computador está conectado à Internet ou a uma rede mais limitada. Como parte da determinação do nível da conectividade, o NCSI executa um dos dois testes ativos: baixar uma página de um servidor Web dedicado ou fazer uma solicitação de DNS para um endereço dedicado. Se você habilitar essa configuração de política, o NCSI não executará qualquer um dos dois testes ativos. Isso pode reduzir a capacidade do NCSI, e de outros componentes que usam o NCSI, de determinar o acesso à Internet.) OBSERVAÇÃO: há outras políticas que permitem redirecionar os testes de NCSI para recursos internos, caso essa funcionalidade seja desejada. |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar Relatório de Erros do Windows |  | Habilitada |
| Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet | Desativar a procura por drivers de dispositivo no Windows Update |  | Habilitada |
| Logon | Mostrar animação de primeira entrada |  | Desabilitado |
| Logon | Desativar notificações de aplicativos na tela de bloqueio |  | Habilitada |
| Logon | Desligar o som de Inicialização do Windows |  | Habilitada |
| Gerenciamento de energia | Selecionar um plano de energia ativo | Alto Desempenho | Habilitada |
| Recuperação | Permitir restauração do sistema ao estado padrão |  | Desabilitado |
| *Integridade de Armazenamento | Permitir o download de atualizações para o Modelo de Previsão de Falha de Disco |  | Desabilitado. As atualizações não são baixadas para o Modelo de Falha de Previsão de Falha de Disco. |
| *Serviços de Hora do Windows \\ Provedores de Horário | Habilitar Windows NTP Client |  | Desabilitado. Se você desabilitar ou não definir esta configuração de política, o relógio do computador local não sincronizará a hora com os servidores NTP. OBSERVAÇÃO: considere essa configuração com muito cuidado. Os dispositivos Windows que ingressaram em um domínio devem usar **NT5DS**. O controlador de domínio para o controlador do domínio primário pode usar NTP. A função PDCe pode usar NTP. Máquinas virtuais, às vezes, usam "aprimoramentos" ou "serviços de integração". |
| Solução de Problemas e Diagnóstico \\ Manutenção Agendada | Configurar comportamento de manutenção agendada |   | Desabilitado |
| Solução de Problemas e Diagnóstico \\ Diagnóstico de Desempenho de Inicialização do Windows | Configurar Nível de Execução do Cenário |   | Desabilitado |
| Solução de Problemas e Diagnóstico \\ Diagnóstico de Perda de Memória do Windows | Configurar Nível de Execução do Cenário |   | Desabilitado |
| Solução de Problemas e Diagnóstico \\ Detecção e Resolução de Esgotamento de Recursos do Windows | Configurar Nível de Execução do Cenário |   | Desabilitado |
| Solução de Problemas e Diagnóstico \\ Diagnóstico de Desempenho de Desligamento do Windows | Configurar Nível de Execução do Cenário |   | Desabilitado |
| Solução de Problemas e Diagnóstico \\ Diagnóstico de Desempenho em Espera/de Retorno do Windows | Configurar Nível de Execução do Cenário |   | Desabilitado |
| Solução de Problemas e Diagnóstico \\ Diagnóstico do Desempenho da Capacidade de Resposta do Sistema do Windows | Configurar Nível de Execução do Cenário |   | Desabilitado |
| *Perfis do Usuário | Desativar a ID de anúncio |   | Habilitada. Se você habilitar esta configuração de política, a ID de anúncio será desativada. Os aplicativos não podem usar a ID para experiências entre aplicativos. |
| Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Componentes do Windows |  |  |  |
| Adicionar recursos ao Windows 10 | Impedir que o assistente seja executado |  | Habilitada |
| *Privacidade do aplicativo | Evitar a execução do assistente |  | Habilitada |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem os dados da conta | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar os dados da conta, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que aplicativos do Windows acessem o histórico de chamadas | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o histórico de chamadas, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem os contatos | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar os contatos, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que aplicativos do Windows acessem informações de diagnóstico sobre outros aplicativos | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você desabilitar ou não definir esta configuração de política, os funcionários da sua organização poderão decidir se os aplicativos do Windows podem obter informações de diagnóstico sobre outros aplicativos usando Configurações > Privacidade no dispositivo. |
| *Privacidade do aplicativo | Permitir que aplicativos do Windows acessem o email | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Permissão**, os aplicativos do Windows poderão acessar o email, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem a localização | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar a localização, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem mensagens | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar as mensagens, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que aplicativos do Windows acessem movimento | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar os dados de movimento, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que aplicativos do Windows acessem notificações | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar as notificações, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que aplicativos do Windows acessem Tarefas | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar as tarefas, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem o calendário | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o calendário, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem a câmera | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar a câmera, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem o microfone | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o microfone, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows acessem dispositivos confiáveis | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar dispositivos confiáveis, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows se comuniquem com dispositivos desemparelhados | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão se comunicar com dispositivos sem fio não emparelhados, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que aplicativos do Windows acessem rádios | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não terão acesso ao controle de rádios, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows façam chamadas telefônicas | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão fazer chamadas telefônicas, e os funcionários da sua organização não poderão alterá-la. |
| *Privacidade do aplicativo | Permitir que os aplicativos do Windows sejam executados em segundo plano | Padrão para todos os aplicativos: Forçar Negação | Habilitada. Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão ser executados em segundo plano, e os funcionários da sua organização não poderão alterá-la. |
| Políticas de Reprodução Automática | Definir o comportamento padrão de Execução Automática | Não executar comandos de execução automática | Habilitada |
| *Políticas de Reprodução Automática | Desligar Reprodução Automática |   | Habilitada. Se você habilitar esta configuração de política, a Reprodução Automática será desabilitada nas unidades de CD-ROM e mídia removível ou desabilitada em todas as unidades. |
| *Conteúdo de nuvem | Não mostrar dicas do Windows | Habilitada. Esta configuração de política impede que dicas do Windows sejam mostradas aos usuários. |
| *Conteúdo de nuvem | Desativar as experiências do cliente da Microsoft | Habilitada. Se você habilitar esta configuração de política, os usuários não verão mais recomendações personalizadas da Microsoft e notificações sobre as respectivas contas Microsoft. |
| *Coleta de Dados e Versões Prévias | Permitir telemetria | 0 – Segurança [Somente Enterprise] | Habilitada. A definição de um valor igual a 0 se aplica aos dispositivos que executam somente as edições Enterprise, Education, IoT ou Windows Server. |
| *Coleta de Dados e Versões Prévias | Não mostrar notificações de comentários |  | Habilitada |
| *Coleta de Dados e Versões Prévias | Ativar/desativar o controle do usuário sobre builds do Insider  |  | Desabilitado |
| Otimização de Entrega | Modo de download | Modo de download: Simples (99) | 99 = Modo de download simples sem emparelhamento. A Otimização de Entrega baixa usando apenas HTTP e não tenta contatar os serviços de nuvem da Otimização de Entrega. |
| Gerenciador de Janelas da Área de Trabalho |  Não permitir invocação de Flip3D |  | Habilitada |
| Gerenciador de Janelas da Área de Trabalho |  Não permitir animações de janelas |  | Habilitada |
| Gerenciador de Janelas da Área de Trabalho |  Use cor sólida para o fundo da tela inicial |  | Habilitada |
| UI Borda |  Permitir passar o dedo na borda |  | Desabilitado |
| UI Borda |  Desabilitar dicas da Ajuda |  | Habilitada |
| UI Borda | Desativar rastreamento de uso do aplicativo |  | Habilitada |
| *Explorador de Arquivos |  Configurar o Windows Defender SmartScreen |  | Desabilitado. O SmartScreen será desativado para todos os usuários. Os usuários não serão avisados quando tentarem executar aplicativos suspeitos da Internet. OBSERVAÇÃO: se não conectados à internet, isso impedirá que os computadores tentem contatar a Microsoft para obter informações sobre o SmartScreen. |
| Explorador de Arquivos |  Não mostrar a notificação de **novo aplicativo instalado** |  | Habilitada |
| *Localizar meu dispositivo |  Ativar/desativar Localizar Meu Dispositivo |  | Desabilitado. Quando a opção Localizar Meu Dispositivo estiver desativada, o dispositivo e a localização não serão registrados, e o recurso Localizar Meu Dispositivo não funcionará. O usuário também não poderá ver a localização do último uso do digitalizador ativo no dispositivo. |
| Explorador de Arquivos | Desligar armazenamento em cache de imagens em miniatura |  | Habilitada |
| Explorador de Arquivos | Desligar a exibição de entradas de pesquisas recentes na caixa de pesquisa do Explorador de Arquivos |  | Habilitada |
| Explorador de Arquivos | Desativar o armazenamento em cache das miniaturas em arquivos thumbs.db ocultos |  | Habilitada |
| Navegador de Jogos | Desligar download de informações de jogos |  | Habilitada |
| Navegador de Jogos | Desativar atualizações de jogos |  | Habilitada |
| Navegador de Jogos | Desativar rastreamento da última hora de jogos na pasta Jogos |  | Habilitada |
| Homegroup | Impedir que o computador ingresse em um grupo doméstico |  | Habilitada |
| *Internet Explorer | Permitir que os serviços Microsoft forneçam sugestões avançadas enquanto o usuário digita algo na barra de endereços |  | Desabilitado. Os usuários não receberão sugestões avançadas ao digitar algo na barra de endereços. Além disso, os usuários não poderão alterar a configuração de Sugestões. |
| Internet Explorer | Desabilitar a verificação periódica de atualizações de software do Internet Explorer |  | Habilitada |
| Internet Explorer | Desabilitar a exibição da tela inicial |  | Habilitada |
| Internet Explorer | Instalar novas versões do Internet Explorer automaticamente |  | Desabilitado |
| Internet Explorer | Impedir participação no Programa de Aperfeiçoamento da Experiência do Usuário |  | Habilitada |
| Internet Explorer | Impedir a execução do Assistente de Primeira Execução | Ir direto para a home page | Habilitada |
| Internet Explorer | Definir crescimento de processos de guias | Baixa | Habilitada |
| Internet Explorer | Especificar categoria padrão para uma nova guia | Nova página de guia | Habilitada |
| Internet Explorer | Desativar notificações de desempenho de complementos |  | Habilitada |
| *Internet Explorer | Desativar o recurso de preenchimento automático em endereços da Web |  | Habilitada. Se você habilitar esta configuração de política, o usuário não receberá sugestões de correspondências ao inserir endereços Web. O usuário não pode alterar o preenchimento automático para a definição de endereços Web. |
| *Internet Explorer | Desativar a geolocalização do navegador |  | Habilitada. Se você habilitar esta configuração de política, o suporte à geolocalização do navegador será desativado. |
| *Internet Explorer | Desativar Reabrir Última Sessão de Navegação |  | Habilitada |
| Internet Explorer | Desativar Reabrir Última Sessão de Navegação |  | Habilitada |
| *Internet Explorer | Ativar o recurso Sites Sugeridos |  | Desabilitado. Se você desabilitar esta configuração de política, os pontos de entrada e a funcionalidade associada a esse recurso serão desativados. |
| *Internet Explorer \\ Modo de Exibição de Compatibilidade | Desativar Modo de Exibição de Compatibilidade |  | Habilitada. Se você habilitar esta configuração de política, o usuário não poderá usar o botão Modo de Exibição de Compatibilidade nem gerenciar a lista de sites do Modo de Exibição de Compatibilidade. |
| *Internet Explorer \\ Painel de Controle da Internet \\ Página Avançada | Reproduzir animações em páginas da Web |  | Desabilitado |
| *Internet Explorer \\ Painel de Controle da Internet \\ Página Avançada | Executar vídeos em páginas da Web |  | Desabilitado |
| *Internet Explorer \\ Painel de Controle da Internet \\ Página Avançada | Desativar o recurso de virar com previsão de página |  | Habilitada. A Microsoft coleta seu histórico de navegação para aprimorar o funcionamento do recurso de virar a página com previsão de página. Esse recurso não está disponível no Internet Explorer para área de trabalho. Se você habilitar esta configuração de política, o recurso de virar a página com previsão de página será desativado e a próxima página da Web não será carregada em segundo plano. |
| Internet Explorer \\ Configurações da Internet \\ Configurações Avançadas \\ Navegação | Desativar a detecção de número de telefone |  | Habilitada |
| *Localização e sensores | Desativar local |  | Habilitada. Se você habilitar esta configuração de política, o recurso de localização será desativado e todos os programas nesse computador serão impedidos de usar informações de localização do recurso de localização. |
| Localização e sensores | Desativar sensores |  | Habilitada |
| Localização e sensores \\ Localizador do Windows | Desativar Localizador do Windows |  | Habilitada |
| *Mapas | Desativar Download e Atualização Automáticos de Dados do Mapa |  | Habilitada. Se você habilitar esta configuração, o download e a atualização automáticos dos dados do mapa serão desativados. |
| *Mapas | Desativar tráfego de rede não solicitado na página de configurações de Mapas Offline |  | Habilitada. Se você habilitar esta configuração de política, os recursos que geram tráfego de rede na página de configurações dos Mapas Offline serão desativados. Observação: isso poderá desativar toda a página de configurações. |
| *Mensagens | Permitir sincronização de nuvem do serviço de mensagem |  | Desabilitado. Esta configuração de política permite o backup e a restauração de mensagens de texto de celular nos serviços de nuvem da Microsoft. |
| *Microsoft Edge | Permitir sugestões na lista suspensa da barra de endereços |  | Desabilitado |
| *Microsoft Edge | Permitir atualizações de configuração para a Biblioteca de Livros |  | Desabilitado. Desativa as listas de compatibilidade no Microsoft Edge. |
| *Microsoft Edge | Permite Lista de Compatibilidade da Microsoft |  | Desabilitado. Se você desabilitar esta configuração, a Lista de Compatibilidade da Microsoft não será usada durante a navegação no navegador. |
| *Microsoft Edge | Permitir conteúdo da Web na página Nova Guia |  | Desabilitado. Instrui o Microsoft Edge a abrir uma guia com o conteúdo em branco quando uma nova guia é aberta. |
| *Microsoft Edge | Configurar o Preenchimento automático |  | Desabilitado. Desabilita o preenchimento automático na barra de endereços. |
| *Microsoft Edge | Configurar o recurso Não Rastrear |  | Habilitada. Se você habilitar essa configuração, as solicitações Não Rastrear sempre serão enviadas a sites que solicitam informações de rastreamento. |
| *Microsoft Edge | Configurar o Gerenciador de Senhas |  | Desabilitado. Se você desabilitar esta configuração, os funcionários não poderão usar o Gerenciador de Senhas para salvar as respectivas senhas localmente. |
| *Microsoft Edge | Configurar sugestões de pesquisa na barra de endereços |  | Desabilitado. Os usuários não podem ver as sugestões de pesquisa na barra de endereços do Microsoft Edge. |
| *Microsoft Edge | Configurar páginas iniciais |  | Habilitada. Se você habilitar esta configuração, poderá configurar uma ou mais páginas iniciais. Se esta configuração for habilitada, você também precisará incluir URLs para as páginas, separando várias páginas com colchetes angulares neste formato: <support.contoso.com><support.microsoft.com> Windows 10, versão 1703 ou posterior: Caso não deseje enviar o tráfego para a Microsoft, use o valor <about:blank>, que é respeitado para dispositivos ingressados ou não em um domínio, quando se tratar da única URL configurada. |
| *Microsoft Edge | Configurar o Windows Defender SmartScreen |  | Desabilitado. O Microsoft Defender SmartScreen é desativado, e os funcionários não podem ativá-lo. OBSERVAÇÃO: considere essa configuração dentro do ambiente. Se não conectados à internet, isso impedirá que os computadores tentem contatar a Microsoft para obter informações sobre o SmartScreen. |
| *Microsoft Edge | Impedir que a página da Web Primeira Execução abra no Microsoft Edge |  | Habilitada. Os usuários não verão a página Primeira Execução ao abrir o Microsoft Edge pela primeira vez. |
| OneDrive | Impedir o OneDrive de gerar tráfego de rede até o usuário entrar no OneDrive |  | Habilitada. Habilite esta configuração para impedir que o cliente de sincronização do OneDrive, o OneDrive.exe, gere tráfego de rede, por exemplo, verificação de atualizações, até que o usuário entre no OneDrive ou inicie a sincronização de arquivos com o computador local. |
| *OneDrive | Impedir o uso do OneDrive para armazenamento de arquivos |  | Habilitada. A menos que o OneDrive seja usado no local ou externamente. |
| OneDrive | Salvar documentos no OneDrive por padrão |  | Desabilitado. A menos que o OneDrive seja usado no local ou externamente. |
| Feeds RSS | Impedir a descoberta automática de feeds e Web Slices |  | Habilitada |
|*RSS Feeds | Desativar a sincronização em segundo plano de feeds e Web Slices |  | Habilitada. Se você habilitar esta configuração de política, a capacidade de sincronizar feeds e Web Slices em segundo plano será desativada. |
|*Pesquisa | Permitir a Cortana |  | Desabilitado. Quando a Cortana for desativada, os usuários ainda poderão usar a pesquisa para encontrar itens no dispositivo. |
|Pesquisar | Permitir a Cortana acima da tela de bloqueio |   | Desabilitado |
|*Pesquisa | Permitir que a pesquisa e a Cortana usem a localização |  | Desabilitado |
|Pesquisar | Não permitir pesquisas na Web |   | Habilitada |
|*Pesquisa | Não fazer pesquisas na Web nem exibir resultados da Web na Pesquisa |  | Habilitada. Se você habilitar esta configuração de política, as consultas não serão realizadas na Web e os resultados da Web não serão exibidos quando um usuário executar uma consulta na Pesquisa. |
|Pesquisar | Impedir a adição de locais UNC ao índice do Painel de Controle |  | Habilitada |
|Pesquisar | Impedir indexação de arquivos no cache de arquivos offline |  | Habilitada |
|*Pesquisa | Definir quais informações são compartilhadas nas Informações anônimas da Pesquisa |  | Habilitada. Compartilhe informações de uso, mas não compartilhe o histórico de pesquisa, informações da conta Microsoft ou uma localização específica. |
|*Plataforma de Proteção de Software | Desativar Validação AVC Online no Cliente KMS | Habilitada. Se você habilitar esta configuração, o computador será impedido de enviar à Microsoft dados relacionados ao estado de ativação. |
|*Fala | Permitir a Atualização Automática de Dados de Fala |  | Desabilitado. Modelos de fala atualizados não serão verificados periodicamente. |
|*Store | Desabilitar Download e Instalação Automáticos de Atualizações |  | Habilitada. Se você habilitar esta configuração, o download e a instalação automáticos das atualizações do aplicativo serão desativados. |
|*Store | Desativar o download automático de atualizações em dispositivos com o Win8 | Habilitada. Se você habilitar esta configuração, o download automático das atualizações do aplicativo será desativado. |
| Repositório | Desativar a oferta de atualização para a última versão do Windows |  | Habilitada |
|*Sincronizar configurações | Não sincronizar | Permitir que os usuários ativem a sincronização (não selecionada) | Habilitada. Se você habilitar esta configuração de política, a opção "sincronizar configurações" será desativada e nenhum dos grupos "sincronizar configuração" será sincronizado neste dispositivo. |
| Entrada de Texto | Aprimorar reconhecimento de escrita à tinta e digitação |  | Desabilitado |
| Windows Defender Antivírus \\ MAPS | Ingressar no Microsoft MAPS |  | Desabilitado. Se você desabilitar ou não definir esta configuração, não poderá ingressar no Microsoft MAPS. |
| Windows Defender Antivírus \\ MAPS | Enviar amostras de arquivo quando outra análise for necessária | Nunca enviar | Habilitada. Somente se não houver aceitação de dados de diagnóstico do MAPS. |
| Windows Defender Antivírus \\ Relatório | Desativar notificações avançadas |  | Habilitada. Se você habilitar esta configuração, as notificações avançadas do Windows Defender Antivírus não serão exibidas em clientes. |
| Windows Defender Antivírus \\ Atualizações de Assinaturas | Definir a ordem das origens para baixar atualizações de definição | FileShares | Habilitada. Se você habilitar esta configuração, as origens da atualização de definição serão contatadas na ordem especificada. Depois que as atualizações de definição forem baixadas com êxito de uma origem especificada, as origens restantes na lista não serão contatadas. |
| Relatório de Erros do Windows | Enviar automaticamente despejos de memória para relatórios de erro gerados pelo sistema operacional |  | Desabilitado |
| Relatório de Erros do Windows | Desabilitar o Relatório de Erros do Windows |  | Habilitada |
| Gravação e Transmissão de Jogos do Windows | Habilita ou desabilita a Gravação e a Transmissão de Jogos do Windows | | Desabilitado |
| Windows Installer | Controlar tamanho máximo do cache de arquivo de linha de base | 5 | Habilitada |
| Windows Installer | Desabilitar a criação de pontos de verificação de Restauração do Sistema |  | Habilitada |
| Windows Mail | Desativar os recursos das Comunidades |  | Habilitada |
| Windows Media Player | Não Mostrar Caixas de Diálogo de Primeiro Uso |  | Habilitada |
| Windows Media Player | Impedir Compartilhamento de Mídia |  | Habilitada |
| Centro de Mobilidade do Windows | Desativar o Centro de Mobilidade do Windows |  | Habilitada |
| Análise de Confiabilidade do Windows | Configurar os Provedores WMI de Confiabilidade |  | Desabilitado |
| Windows Update | Permitir instalação imediata de Atualizações Automáticas |  | Habilitada |
| Windows Update | Não conectar a localizações do Windows Update na Internet |  | Habilitada. Se você habilitar esta política, essa funcionalidade será desabilitada, e isso poderá causar a interrupção da conexão com serviços públicos, como a Microsoft Store. OBSERVAÇÃO: essa política só se aplica quando o dispositivo está configurado para se conectar a um serviço de atualização da intranet usando a política "Especificar o local do serviço de atualização na intranet da Microsoft". |
| Windows Update | Remover acesso ao uso de todos os recursos do Windows Update |   | Habilitada |
| *Windows Update \\ Windows Update para Empresas | Gerenciar versões prévias | Definir o comportamento de recebimento das versões prévias: | Habilitada. A seleção de Desabilitar versões prévias impedirá as versões prévias de serem instaladas no dispositivo. Isso impedirá que os usuários aceitem participar do Programa Windows Insider por meio de Configurações -> Atualização e Segurança.<br>Desabilitado. Desabilita versões prévias. |
| *Windows Update \\ Windows Update para Empresas | Selecionar quando Versões Prévias e Atualizações de Recursos forem recebidas | Canal Semestral<br>Diferimento: 365 dias<br>Início da pausa: aaa-mm-dd. | Habilitada. Habilite esta política para especificar o nível da Versão Prévia ou das atualizações de recursos a serem recebidas e quando elas serão recebidas. |
| Windows Update \\ Windows Update para Empresas | Selecionar quando as Atualizações de Qualidade serão recebidas | 1. 30 dias<br>2. Pausar início das atualizações de qualidade aaaa-mm-dd | Habilitada |
| Configurações da Política Personalizada de Tráfego Restrita pelo Windows | Impedir o OneDrive de gerar tráfego de rede até o usuário entrar no OneDrive |  | Habilitada. Habilite esta configuração se desejar impedir que o cliente de sincronização do OneDrive, o OneDrive.exe, gere tráfego de rede, por exemplo, verificação de atualizações, até que o usuário entre no OneDrive ou inicie a sincronização de arquivos com o computador local. |
| Configurações da Política Personalizada de Tráfego Restrita pelo Windows | Desativar Notificações do Windows Defender |  | Habilitada. Se você habilitar esta configuração de política, o Windows Defender não enviará notificações com informações críticas sobre a integridade e a segurança do seu dispositivo. |
| Política do Computador Local \\ Configuração do Usuário \\ Modelos Administrativos  |  |  |
|Painel de Controle \\ Opções Regionais e de Idioma | Desativar a oferta de previsões de texto ao digitar |  | Habilitada |
| Desktop | Não adicionar compartilhamentos de documentos recentemente abertos a Locais de Rede |  | Habilitada |
| Desktop | Desativar a janela Aero Shake minimizando o gesto do mouse |  | Habilitada |
| Área de Trabalho \\ Active Directory | Tamanho máximo de pesquisas do Active Directory | 2500 | Habilitada |
| Menu Iniciar e barra de tarefas | Não permitir a fixação de aplicativo da loja na barra de tarefas |  | Habilitada |
| Menu Iniciar e barra de tarefas | Não exibir nem controlar itens nas Listas de Atalhos a partir de locais remotos |  | Habilitada |
| Menu Iniciar e barra de tarefas | Não usar o método baseado em pesquisa ao resolver atalhos do shell |  | Habilitada. O sistema não realiza a pesquisa na unidade final. Ele só exibe uma mensagem explicando que o arquivo não foi encontrado. |
| Menu Iniciar e barra de tarefas | Remover a Barra de Pessoas da barra de tarefas |  | Habilitada. O ícone de pessoas será removido da barra de tarefas, a alternância de configurações correspondente será removida da página de configurações da barra de tarefas, e os usuários não poderão fixar pessoas na barra de tarefas. |
| Menu Iniciar e barra de tarefas | Desligar o recurso de notificações de balões de publicidade |  | Habilitada. Os usuários não podem fixar o aplicativo da Store na barra de tarefas. Se o aplicativo da Store já estiver fixado na barra de tarefas, ele será removido da barra de tarefas na próxima conexão. |
| Menu Iniciar e barra de tarefas | Desativar o rastreamento de usuário |  | Habilitada |
| Menu Iniciar e barra de tarefas \\ Notificações | Desativar notificações de aviso |  | Habilitada |
| Componentes do Windows \\ Conteúdo de Nuvem | Desativar todos os recursos de Destaque do Windows |  | Habilitada |

### <a name="notes-about-network-connectivity-status-indicator"></a>Observações sobre o Indicador de Status da Conectividade de Rede

As configurações de política de grupo acima incluem configurações para desativar a verificação de modo a confirmar se o sistema está conectado à Internet. Se seu ambiente não se conecta à Internet de modo algum, ou se conecta indiretamente, você poderá definir uma configuração de política de grupo para remover o ícone de rede da barra de tarefas. Se você desativar as verificações de conectividade da Internet, um sinalizador amarelo aparecerá no ícone de rede, mesmo que a rede esteja funcionando normalmente. Por esse motivo, talvez você queira remover o ícone de rede da barra de tarefas. Se quiser remover o ícone de rede como uma configuração de política de grupo, você poderá encontrar isso neste local:

| Configuração de política | Item | Subitem | Configuração possível e comentários|
| -------------- | ---- | -------- | ---------------------------- |
| Windows Update ou Windows Update para Empresas | Selecionar quando as Atualizações de Qualidade serão recebidas | 1. 30 dias<br>2. Pausar início das atualizações de qualidade aaaa-mm-dd | Habilitada |
| Política do Computador Local \\ Configuração do Usuário \\ Modelos Administrativos |  |  |  |
| Menu Iniciar e barra de tarefas | Remover o ícone de sistema de rede |  | Habilitada. O ícone de rede não é exibido na área de notificação do sistema. |

Para obter mais informações sobre o NCSI (Indicador de Status da Conexão de Rede), confira [Gerenciar pontos de extremidade de conexão do Windows 10 Enterprise, versão 1903](/windows/privacy/manage-windows-1903-endpoints) e [Gerenciar conexões de componentes do sistema operacional Windows 10 para serviços Microsoft](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services).

### <a name="system-services"></a>Serviços do sistema

Se você estiver considerando a possibilidade de desabilitar os serviços do sistema para conservar recursos, tome muito cuidado para que o serviço que está sendo considerado não seja de modo algum um componente de outro serviço. Observe que alguns serviços não estão na lista porque não podem ser desabilitados de maneira compatível.

Além disso, a maioria dessas recomendações espelha as recomendações para o Windows Server 2016, instalado com a Experiência Desktop em [Diretrizes sobre como desabilitar serviços do sistema no Windows Server 2016 com Experiência Desktop](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md)

Muitos serviços que possam parecer bons candidatos à desabilitação são definidos com o tipo de início de serviço manual. Isso significa que o serviço não será iniciado automaticamente e não será iniciado, a menos que um processo ou um evento dispare uma solicitação para o serviço que está sendo considerado para desabilitação. Os serviços que já estão definidos para tipo de início manual normalmente não são listados aqui.

> [!NOTE]
> Você pode enumerar os serviços em execução com este código de exemplo do PowerShell, gerando apenas o nome curto do serviço:

```powershell
 Get-Service | Where-Object {$_.Status -eq "Running"} | Select-Object -ExpandProperty Name
 ```

| Serviço do Windows | Item | Comentários|
| -------------- | ---- | ---------------------------- |
| CDPUserService | Esse serviço de usuário é usado para cenários de Plataforma de Dispositivos Conectados | esse é um serviço por usuário e, como tal, o *serviço de modelo* deve ser desabilitado. |
| Experiência do Usuário Conectado e Telemetria | Habilita os recursos que dão suporte a experiências de usuário no aplicativo e conectadas. Além disso, esse serviço gerencia a coleção controlada por evento e a transmissão de informações de diagnóstico e de uso (usadas para melhorar a experiência e a qualidade da Plataforma Windows) quando as configurações de opção de privacidade de uso e diagnóstico são habilitadas em Comentários e Diagnóstico. | Considere a possibilidade de desabilitação se estiver em uma rede desconectada. |
| Dados de Contato | Indexa dados de contato para pesquisa rápida de contatos. Se você interromper ou desabilitar esse serviço, os contatos poderão ficar ausentes dos resultados da pesquisa. | esse é um serviço por usuário e, como tal, o *serviço de modelo* deve ser desabilitado. |
| Serviço de Política de Diagnóstico | Habilita a detecção e a solução de problemas, bem como a resolução para componentes do Windows. Se esse serviço for interrompido, o diagnóstico deixará de funcionar. | |
| Gerenciador de Mapas Baixados | O serviço Windows para acesso de aplicativo aos mapas baixados. Esse serviço é iniciado sob demanda pelo aplicativo que acessa os mapas baixados. A desabilitação desse serviço impedirá que os aplicativos acessem mapas. | |
| Serviço de Geolocalização | Monitora o local atual do sistema e gerencia cercas geográficas | |
| Serviço de usuário Transmissão e GameDVR | Esse serviço de usuário é usado para Gravações de Jogo e Transmissões ao Vivo | esse é um serviço por usuário e, como tal, o serviço de modelo deve ser desabilitado. |
| MessagingService | Serviço que dá suporte a mensagens de texto e funcionalidade relacionada. | esse é um serviço por usuário e, como tal, o *serviço de modelo* deve ser desabilitado. |
| Otimizar unidades | Ajuda o computador a ser executado com mais eficiência pela otimização de arquivos em unidades de armazenamento. | Soluções VDI normalmente não se beneficiam da otimização do disco. Essas "unidades" não são unidades tradicionais e, muitas vezes, são apenas uma alocação de armazenamento temporário. |
| Superfetch | Mantém e melhora o desempenho do sistema ao longo do tempo. | Em geral, não melhora o desempenho na VDI, especialmente não persistente, uma vez que o estado do sistema operacional é descartado a cada reinicialização. |
| Serviço de Teclado Virtual e Painel de Manuscrito | Habilita a funcionalidade de caneta e tinta do Teclado Virtual e do Painel de Manuscrito | |
| Relatório de Erros do Windows | Permite que os erros sejam relatados quando os programas param de funcionar ou responder e permite que as soluções existentes sejam entregues. Também permite que os logs sejam gerados para serviços de diagnóstico e reparo. Se esse serviço for interrompido, o relatório de erros poderá não funcionar corretamente e os resultados dos serviços de diagnóstico e os reparos poderão não ser exibidos. | Com a VDI, o diagnóstico geralmente é feito em um cenário offline, e não em produção base. E, além disso, alguns clientes desabilitam o WER mesmo assim. O WER incorre em uma pequena quantidade de recursos para muitas tarefas diferentes, incluindo falha ao instalar um dispositivo ou falha ao instalar uma atualização. |
| Serviço de Compartilhamento de Rede do Windows Media Player | Compartilha bibliotecas do Windows Media Player com outros players na rede e dispositivos de mídia usando Plug and Play Universal | Não é necessário, a menos que os clientes estejam compartilhando bibliotecas do WMP na rede. |
| Serviço de Hotspot Móvel do Windows | Fornece a capacidade de compartilhar uma conexão de dados da rede celular com outro dispositivo. | |
| Windows Search | Fornece indexação de conteúdo, cache de propriedades e resultados da pesquisa para arquivos, emails e outros tipos de conteúdo.                                                                    | Provavelmente não é necessário, especialmente com VDI não persistente |

#### <a name="per-user-services-in-windows"></a>Serviços por usuário no Windows

Os serviços por usuário referem-se a serviços que são criados quando um usuário entra no Windows ou no Windows Server e que são interrompidos e excluídos quando o usuário sai do serviço. Esses serviços são executados no contexto de segurança da conta de usuário – isso fornece melhor gerenciamento de recursos que a abordagem anterior de executar esses tipos de serviço no Explorador, associados a uma conta pré-configurada, ou como tarefas.

[Serviços por usuário no Windows 10 e no Windows Server](/windows/application-management/per-user-services-in-windows)

Se você pretende alterar o valor inicial de um serviço, o método preferencial é abrir um prompt .cmd com privilégios elevados e executar a ferramenta Gerenciador de Controle de Serviço "Sc.exe". Para obter mais informações sobre como usar o "Sc.exe", confira [Sc](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754599(v=ws.11)).

### <a name="scheduled-tasks"></a>Tarefas Agendadas

Assim como outros itens no Windows, verifique se o item não é necessário antes de considerar a desabilitação dele.

A lista a seguir é de tarefas que realizam otimizações ou coletas de dados em computadores que mantêm seu estado entre as reinicializações. Quando uma tarefa de VM da VDI reinicia e descarta todas as alterações desde a última inicialização, as otimizações destinadas a computadores físicos não são úteis.

Você pode obter todas as tarefas agendadas atuais, incluindo as descrições, com o seguinte código do PowerShell:

```powershell
 Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

> [!NOTE]
> Há várias tarefas que não podem ser desabilitadas por meio de script, mesmo em uma execução com privilégios elevados. Recomendamos não desabilitar as tarefas que não podem ser desabilitadas usando um script.

Nome da tarefa agendada:

- Celular
- Consolidador
- Diagnóstico
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- MaintenanceTasks
- MapsToastTask
- Compatibilidade
- Microsoft-Windows-DiskDiagnosticDataCollector
- MNO
- NotificationTask
- PerformRemediation
- ProactiveScan
- ProcessMemoryDiagnosticEvents
- ProgramDataUpdater
- Proxy
- QueueReporting
- RecommendedTroubleshootingScanner
- ReconcileFeatures
- ReconcileLanguageResources
- RefreshCache
- RegIdleBackup
- ResPriStaticDbSync
- RunFullMemoryDiagnostic
- ScanForUpdates
- ScanForUpdatesAsUser
- Agendado
- ScheduledDefrag
- sihpostreboot
- SilentCleanup
- SmartRetry
- SpaceAgentTask
- SpaceManagerTask
- SpeechModelDownloadTask
- Tarefas do SQM
- SR
- StartComponentCleanup
- StartupAppTask
- StorageSense
- SyspartRepair
- Sysprep
- UninstallDeviceTask
- UpdateLibrary
- UpdateModelTask
- UsbCeip
- Notificações de USB
- USO_UxBroker
- WiFi
- WIM-Hash-Management
- WindowsActionDialog
- WinSAT
- Pastas
- WsSwapAssessmentTask
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>Aplicar atualizações do Windows (entre outras)

Seja do Microsoft Update, seja de recursos internos, aplique as atualizações disponíveis, incluindo assinaturas do Windows Defender. Esse é um bom momento para aplicar outras atualizações disponíveis, incluindo o Microsoft Office, se instalado, bem como outras atualizações. Se o PowerShell permanecer na imagem, baixe a ajuda mais recente disponível para o PowerShell executando o comando [Update-Help](/powershell/module/microsoft.powershell.core/update-help?view=powershell-7).

#### <a name="servicing-the-operating-system-and-apps"></a>Manutenção do sistema operacional e de aplicativos

Em algum momento durante o processo de otimização de imagem, as atualizações disponíveis do Windows devem ser aplicadas. Há uma configuração nas Configurações de Atualização do Windows 10 que pode fornecer atualizações adicionais:

![Atualizações adicionais](media/rds-vdi-recommendations-1909/servicing.png)

Essa seria uma boa configuração no caso de você instalar aplicativos Microsoft, como o Microsoft Office na imagem base. Dessa forma, o Office é atualizado quando a imagem é colocada em serviço. Também há atualizações do .NET e alguns componentes de terceiros, como Adobe, cujas atualizações são disponibilizadas pelo Windows Update.

Uma consideração muito importante para VMs de VDI não persistente são as atualizações de segurança, incluindo arquivos de definição de software de segurança. Essas atualizações podem ser liberadas uma vez ou mais de uma vez por dia. Pode haver uma maneira de manter essas atualizações, incluindo o Windows Defender e componentes de terceiros.

Para o Windows Defender, pode ser melhor permitir que as atualizações ocorram, mesmo em VDI não persistente. As atualizações serão aplicadas praticamente a cada sessão de logon, mas são pequenas e não devem ser um problema. Além disso, a VM não ficará desatualizada, pois apenas as últimas atualizações disponíveis serão aplicadas. O mesmo pode ser verdadeiro para arquivos de definição de terceiros.

> [!NOTE]
> Os aplicativos da loja (aplicativos UWP) são atualizados pela Windows Store. As versões modernas do Office, como o Microsoft 365, são atualizadas por meio dos próprios mecanismos quando conectados diretamente à Internet ou por meio de tecnologias de gerenciamento quando não estão conectados.

### <a name="windows-system-startup-event-traces"></a>Rastreamentos de eventos de inicialização do sistema do Windows

O Windows está configurado, por padrão, para coletar e salvar dados limitados de diagnóstico. O objetivo é habilitar o diagnóstico ou gravar dados, caso seja necessária uma solução de problemas adicional. Os rastreamentos automáticos do sistema podem ser encontrados na localização mostrada na seguinte ilustração:

![Rastreamentos do sistema](media/rds-vdi-recommendations-1909/system-traces.png)

Alguns dos rastreamentos exibidos em **Sessões de Rastreamento de Eventos** e **Sessões de Rastreamento de Eventos de Inicialização** não podem e não devem ser interrompidos. Outros, como o rastreamento "WiFiSession", podem ser interrompidos. Para interromper um rastreamento em execução em **Sessões de Rastreamento de Eventos**, clique com o botão direito do mouse no rastreamento e, depois, clique em "Interromper". Use o seguinte procedimento para impedir que os rastreamentos sejam iniciados automaticamente na inicialização:

1. Clique na pasta **Sessões de Rastreamento de Eventos de Inicialização**.

2. Localize o rastreamento de interesse e clique duas vezes nele.

3. Clique na guia **Sessão de Rastreamento**.

4. Clique na caixa rotulada **Habilitado** para remover a marca de seleção.

5. Clique em **Ok**.

Estes são alguns rastreamentos do sistema que podem ser considerados para desabilitação para uso da VDI:

| Nome                    | Comentário                       |
| ----------------------- | ----------------------------- |
| AppModel | Uma coleção de rastreamentos, uma delas é telefone |
| CloudExperienceHostOOBE | |
| DiagLog | |
| NtfsLog | |
| TileStore | |
| UBPM | |
| WiFiDriverIHVSession | Se não estiver usando um dispositivo de Wi-Fi |
| WiFiSession | |
| WinPhoneCritical | |

### <a name="windows-defender-optimization-with-vdi"></a>Otimização do Windows Defender com VDI

A Microsoft publicou recentemente a documentação relacionada ao Windows Defender em um ambiente VDI. Confira [Guia de implantação do Windows Defender Antivírus em um ambiente VDI (Virtual Desktop Infrastructure)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus) para obter mais informações.

O artigo acima contém procedimentos para fazer a manutenção da imagem "ouro" da VDI e de como manter os clientes VDI enquanto estão em execução. Para reduzir a largura de banda da rede quando computadores VDI precisarem atualizar suas assinaturas do Windows Defender, agende as reinicializações para fora do horário comercial quando possível. As atualizações de assinatura do Windows Defender podem estar mantidas internamente em compartilhamentos de arquivos e, quando for conveniente, esses arquivos podem compartilhados nos mesmos segmentos de rede (ou próximos) das máquinas virtuais VDI.

### <a name="client-network-performance-tuning-by-registry-settings"></a>Ajuste de desempenho de rede do cliente nas configurações do Registro

Há algumas configurações do Registro que podem aumentar o desempenho da rede. Isso é especialmente importante em ambientes nos quais a VDI ou o computador tenha uma carga de trabalho que se baseia quase toda na rede. As configurações desta seção são recomendadas para ajustar o desempenho em benefício da rede, configurando armazenamento em cache e buffer adicionais de itens como entradas de diretório.

>[!NOTE]
> Algumas configurações desta seção são somente baseadas no Registro e devem ser incorporadas na imagem base antes que ela seja implantada para uso em produção.

As configurações a seguir estão documentadas nas [Diretrizes de Ajuste de Desempenho do Windows Server 2016](../../administration/performance-tuning/index.md), publicadas em Microsoft.com pelo Grupo de Produtos do Windows.

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

Aplica-se ao Windows 10. O padrão é **0**. Por padrão, o redirecionador SMB limita a taxa de transferência em conexões de rede de alta latência, em alguns casos, para evitar tempos limite relacionados à rede. Definir esse valor de Registro como 1 desabilita essa limitação, permitindo uma taxa de transferência de arquivo mais alta por conexões de rede de alta latência. Considere a possibilidade de definir esse valor como **1**.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax` Aplica-se ao Windows 10. O padrão é **64**, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de metadados de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando muitos arquivos são acessados. Tente aumentar esse valor para **1024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

Aplica-se ao Windows 10. O padrão é **16**, com um intervalo válido de 1 a 4096. Esse valor é usado para determinar a quantidade de informações de diretório que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando diretórios grandes são acessados. Considere aumentar esse valor para **1024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

Aplica-se ao Windows 10. O padrão é **128**, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de informações de nome de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando muitos nomes de arquivo são acessados. Considere aumentar esse valor para **2048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

Aplica-se ao Windows 10. O padrão é **1023**. Esse parâmetro especifica o número máximo de arquivos que deve ser aberto em um recurso compartilhado após o aplicativo fechar o arquivo. Onde muitos milhares de clientes estiverem se conectando a servidores SMB, considere a redução desse valor para **256**.

É possível definir muitas configurações SMB usando os cmdlets [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps) e [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps) do Windows PowerShell. As configurações somente de Registro também podem ser definidas com o Windows PowerShell, como no seguinte exemplo:

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

Configurações adicionais das diretrizes de [Linha de base da funcionalidade limitada de tráfego restrita pelo Windows](https://docs.microsoft.com/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services).

A Microsoft lançou uma linha de base criada usando os mesmos procedimentos das [Linhas de Base de Segurança do Windows](https://docs.microsoft.com/windows/security/threat-protection/windows-security-baselines) para ambientes que não são conectados diretamente à Internet ou que querem reduzir dados enviados à Microsoft e outros serviços.

As configurações da Linha de Base de Funcionalidade Limitada de Tráfego Restrita pelo Windows são indicadas na tabela da Política de Grupo com um asterisco.

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Limpeza de disco (incluindo o uso do assistente para Limpeza de Disco)

A limpeza de disco pode ser especialmente útil com implementações da VDI da imagem mestra/ouro. Depois que a imagem é preparada, atualizada e configurada, uma das últimas tarefas a ser executada é a limpeza de disco. Há uma ferramenta interna chamada "Assistente para Limpeza de Disco" que pode ajudar a limpar a maioria das áreas potenciais de economia de espaço em disco. Em uma VM que tenha muitos poucos itens instalados, mas que foi totalmente corrigida, geralmente, é possível obter cerca de 4 GB de espaço em disco liberado executando a Limpeza de Disco.

Veja a seguir algumas sugestões para diversas tarefas de limpeza de disco. Todas elas devem ser testadas antes da implementação:

1. Execute o Assistente para Limpeza de Disco (com privilégios elevados) após a aplicação de todas as atualizações. Inclua as categorias "Otimização de Entrega" e "Limpeza do Windows Update". Esse processo pode ser automatizado usando a linha de comando `Cleanmgr.exe` com a opção `/SAGESET:11`. A opção `/SAGESET` define valores do Registro que podem ser usados posteriormente para automatizar a limpeza de disco, que usa todas as opções disponíveis no Assistente para Limpeza de Disco.

    1. Em uma VM de teste, em uma instalação limpa, a execução de `Cleanmgr.exe /SAGESET:11` revela que há apenas duas opções de limpeza de disco automática habilitadas por padrão:

        - Arquivos de Programas Baixados

        - Arquivos Temporários da Internet

    2. Se você definir mais opções ou todas as opções, essas opções serão gravadas no Registro, de acordo com o valor do **Índice** fornecido no comando anterior (`Cleanmgr.exe /SAGESET:11`). Nesse caso, vamos usar o valor `11` como nosso índice, para um procedimento de limpeza de disco automatizado posterior.

    3. Depois de executar `Cleanmgr.exe /SAGESET:11`, você verá várias categorias de opções de limpeza de disco. Verifique cada opção e, em seguida, clique em **OK**. O Assistente para Limpeza de Disco desaparecerá, e as configurações serão salvas no Registro.

2. Limpe o armazenamento de Cópias de Sombra de Volume, se houver algum em uso.

    - Abra um prompt de comandos com privilégios elevados e execute o comando `vssadmin list shadows` e, em seguida, o comando `vssadmin list shadowstorage`.

        Se a saída desses comandos for **Nenhum item encontrado que atenda à consulta**, isso significará que não há nenhum armazenamento de VSS em uso.

3. Limpe arquivos e logs temporários. Em um prompt de comandos com privilégios elevados, execute o comando `Del C:\*.tmp /s`, o comando `Del C:\Windows\Temp\.` e o comando `Del %temp%\.`.

4. Exclua os perfis não utilizados no sistema executando `wmic path win32_UserProfile where LocalPath="c:\users\<user>" Delete`.

### <a name="remove-onedrive-components"></a>Remover componentes do OneDrive

Remover OneDrive envolve a remoção do pacote, desinstalação e remoção de arquivos \*.lnk. O seguinte código de exemplo do PowerShell pode ser usado para auxiliar na remoção do OneDrive da imagem e está incluído nos scripts de otimização da VDI do GitHub:

```azurecli
Get-Process -Name OneDrive | Stop-Process -Force -Confirm:$false
Get-Process -Name explorer | Stop-Process -Force -Confirm:$false
if (Test-Path "C:\\Windows\\System32\\OneDriveSetup.exe")`
    { Start-Process "C:\\Windows\\System32\\OneDriveSetup.exe"`
        -ArgumentList "/uninstall"`
        -Wait }
if (Test-Path "C:\\Windows\\SysWOW64\\OneDriveSetup.exe")`
    { Start-Process "C:\\Windows\\SysWOW64\\OneDriveSetup.exe"`
        -ArgumentList "/uninstall"`
        -Wait }
Remove-Item -Path "C:\\Windows\\ServiceProfiles\\LocalService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force
Remove-Item -Path "C:\\Windows\\ServiceProfiles\\NetworkService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force

# Remove the automatic start item for OneDrive from the default user profile registry hive

Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Load HKLM\\Temp C:\\Users\\Default\\NTUSER.DAT" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Delete HKLM\\Temp\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run /v OneDriveSetup /f" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Unload HKLM\\Temp" -Wait Start-Process -FilePath C:\\Windows\\Explorer.exe -Wait
```

## <a name="turn-windows-update-back-on"></a>Ativar o Windows Update novamente

Caso deseje ativar o Windows Update novamente, como no caso da VDI persistente, siga estas etapas:

- Habilite novamente estas configurações da Política de Grupo:

    - Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Sistema \\ Gerenciamento de Comunicação da Internet \\ Configurações de Comunicação da Internet

        - Desligue o acesso a todos os recursos de Windows Update (altere **habilitado** para **não configurado**).

    - Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Componentes do Windows \\ Windows Update

        - Remova o acesso a todos os recursos do Windows Update (altere **habilitado** para **não configurado**)

        - Não se conecte a nenhum local da Internet do Windows Update (altere **habilitado** para **não configurado**).

    - Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Componentes do Windows \\ Windows Update \\ Windows Update para Empresas

        - Selecione quando as atualizações de qualidade são recebidas (altere 'habilitado' para 'não configurado')

    -   Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Componentes do Windows \\ Windows Update \\ Windows Update para Empresas

        - Selecione quando as versões prévias e as atualizações de recursos são recebidas (altere de **habilitado** para **não configurado**)

-  Reabilitar serviços

    - Atualize o serviço Orchestrator – altere **desabilitado** para **Automático (Atraso na Inicialização)** .

    - Edite as seguintes configurações do Registro do Windows:

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState

            - DeferQualityUpdates (altere **1** para **0**)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings

            - PausedQualityDate (exclua qualquer valor existente)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU

            - Desabilitado

-  Reabilitar tarefas agendadas

    - Biblioteca do Agendador de Tarefas \\ Microsoft \\ Windows \\ InstallService\\ ScanForUpdates

    - Biblioteca do Agendador de Tarefas \\ Microsoft \\ Windows \\ InstallService \\ ScanForUpdatesAsUser

Para fazer com que todas essas configurações entrem em vigor, reinicie o dispositivo. Caso não deseje que este dispositivo receba as atualizações de recurso, acesse Configurações \\ Windows Update \\ Opções avançadas \\ escolha quando as atualizações são instaladas e, em seguida, defina manualmente a opção, **Uma atualização de recursos inclui novas funcionalidades e melhorias. Ela pode ser adiada por esta quantidade de dias para um valor diferente de zero, como 180, 365 etc.**

Para perguntas ou questões sobre as informações neste artigo, entre em contato com a equipe de contas da Microsoft, pesquise o blog de VDI da Microsoft, poste uma mensagem nos fóruns da Microsoft ou entre em contato com a Microsoft.

### <a name="references"></a>Referências

- [O que é a VDI (Virtual Desktop Infrastructure)](https://www.citrix.com/glossary/vdi.html)

- [O Sysprep falha depois que você remove ou atualiza aplicativos da Microsoft Store que incluem imagens internas do Windows](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu).