---
title: Otimizando o Windows 10, versão 1803, para uma função VDI (Virtual Desktop Infrastructure)
description: Configurações e definições recomendadas para minimizar a sobrecarga para desktops com Windows 10 (1803) usados como imagens de VDI
ms.prod: windows-server
ms.reviewer: robsmi
ms.technology: remote-desktop-services
ms.author: jaimeo, robsmi
ms.topic: article
author: jaimeo
manager: dougkim
ms.openlocfilehash: c08e7621285ceb8d122629c26ce5e160ee849737
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182162"
---
# <a name="optimizing-windows-10-version-1803-for-a-virtual-desktop-infrastructure-vdi-role"></a>Otimizando o Windows 10, versão 1803, para uma função VDI (Virtual Desktop Infrastructure)

Este artigo ajuda você a escolher configurações para o Windows 10, versão 1803 (build 17134), que devem resultar no melhor desempenho em um ambiente VDI (Virtualized Desktop Infrastructure). Todas as configurações neste guia são *recomendações a serem consideradas*, não sendo, de modo algum, requisitos.

Em um ambiente VDI, as principais maneiras de otimizar o desempenho do Windows 10 são minimizando as redefinições de elementos gráficos do aplicativo, as atividades em segundo plano sem grande vantagem para o ambiente VDI e, normalmente, reduzindo os processos de execução para o mínimo necessário. Uma meta secundária é reduzir o uso do espaço em disco na imagem base para o mínimo necessário. Com as implementações da VDI, a menor base possível, ou tamanho "gold" de imagem, pode reduzir ligeiramente o uso da memória no hipervisor, bem como provocar uma pequena redução nas operações gerais de rede necessárias para fornecer a imagem de área de trabalho ao consumidor.

> [!NOTE]
> As configurações recomendadas aqui podem ser aplicadas em outra instalação do Windows 10, versão 1803, incluindo em dispositivos físicos ou outros virtuais. Nenhuma recomendação neste tópico deve afetar a capacidade de suporte do Windows 10, versão 1803.

> [!TIP]
> Um script que implementa as otimizações abordadas neste tópico – bem como um arquivo de exportação de GPO que você pode importar com **LGPO.exe** – está disponível em [TheVDIGuys](https://github.com//TheVDIGuys) no GitHub.

## <a name="vdi-optimization-principles"></a>Princípios da otimização da VDI

Um ambiente VDI apresenta uma sessão de área de trabalho completa, incluindo aplicativos, a um usuário de computador em uma rede. Os ambientes VDI geralmente usam uma imagem base de sistema operacional que se torna a base para as áreas de trabalho subsequentemente apresentada aos usuários para o trabalho. Há variações das implementações da VDI, como "persistente", "não persistente" e "sessão da área de trabalho". O tipo persistente preserva as alterações no sistema operacional da área de trabalho da VDI de uma sessão para a outra. O tipo não persistente não preserva as alterações no sistema operacional da área de trabalho da VDI de uma sessão para a outra. Para o usuário, essa área de trabalho é pouco diferente daquela de outro dispositivo físico ou virtual, exceto pelo fato de que ela é acessada por uma rede.

As configurações de otimização podem ocorrer em um dispositivo de referência. Uma VM é um lugar ideal para gerar a imagem, pois você pode salvar o estado, criar pontos de verificação e fazer backups, além de outras tarefas úteis. Comece instalando o sistema operacional padrão na VM base e otimize a VM base para uso da VDI removendo aplicativos desnecessários, instalando atualizações do Windows, instalando outras atualizações, excluindo arquivos temporários, aplicando configurações etc.

Há outros tipos de VDI, como persistente e RDS (Serviços de Área de Trabalho Remota). Uma discussão detalhada sobre essas tecnologias está fora do escopo deste tópico, que foca nas configurações de imagem base do Windows com referência a outros fatores no ambiente, como otimização do host.

### <a name="persistent-vdi"></a>VDI persistente

A VDI persistente é, no nível básico, uma VM que salva o estado do sistema operacional entre as reinicializações. Outras camadas de software da solução VDI fornecem aos usuários acesso fácil e contínuo às respectivas VMs atribuídas, muitas vezes, com uma solução de logon único.

Há várias implementações diferentes da VDI persistente:

- A máquina virtual tradicional, onde a VM tem seu próprio arquivo de disco virtual, é inicializada normalmente, salva alterações de uma sessão para outra e é, essencialmente, apenas uma VM comum. A única diferença está em como o usuário acessa essa VM. Pode haver um portal da Web ao qual o usuário se conecta que o direciona automaticamente para uma ou mais de suas VMs de VDI atribuídas.

- A máquina virtual persistente baseada em imagem, com discos virtuais pessoais. Nesse tipo de implementação, há uma imagem base/gold em um ou mais servidores host. Uma VM é criada, assim como um ou mais discos virtuais são criados e atribuídos a esse disco para armazenamento persistente.

    - Quando a VM é iniciada, uma cópia da imagem base é lida na memória da VM. Ao mesmo tempo, um disco virtual persistente é atribuído a essa VM, com todas as alterações do sistema operacional anterior mescladas por meio de um processo complexo.

    - As alterações, como gravações do log de eventos, gravações de log etc., são redirecionadas para o disco virtual de leitura/gravação atribuído a essa VM.

    - Nessa circunstância, o sistema operacional e a manutenção do aplicativo podem funcionar normalmente usando o software de manutenção tradicional, como o Windows Server Update Services ou outras tecnologias de gerenciamento.

### <a name="non-persistent-vdi"></a>VDI não persistente

Quando uma implementação de VDI não persistente é baseada em uma imagem base ou "gold", as otimizações são executadas principalmente na imagem base e, em seguida, por meio das configurações e políticas locais.

Com a VDI não persistente baseada em imagem, a imagem base é somente leitura. Quando uma VM de VDI não persistente é iniciada, uma cópia da imagem base é transmitida para a VM. A atividade que ocorre durante a inicialização e daí em diante até a próxima reinicialização é redirecionada para um local temporário. Normalmente, os usuários recebem locais de rede para armazenar seus dados. Em alguns casos, o perfil do usuário é mesclado com a VM padrão para fornecer aos usuários suas configurações.

Um aspecto importante da VDI não persistente que se baseia em uma só imagem é a manutenção. As atualizações para o sistema operacional geralmente são fornecidas uma vez por mês.
Com a VDI baseada em imagem, há um conjunto de processos a ser executado de modo a obter atualizações para a imagem:

- Em um determinado host, todas as VMs nesse host que são derivadas da imagem base devem ser desligadas ou desativadas. Isso significa que os usuários são redirecionados para outras VMs.

- A imagem base é aberta e iniciada. Todas as atividades de manutenção são executadas, como atualizações do sistema operacional, atualizações do .NET, atualizações de aplicativo etc.

- Todas as configurações novas que precisam ser aplicadas são aplicadas nesse momento.

- Qualquer outra manutenção é realizada nesse momento.

- A imagem base é desligada.

- A imagem base é validada e definida para voltar à produção.

– Os usuários têm permissão para fazer logon novamente.

> [!NOTE]
> O Windows 10 executa, periodicamente, um conjunto de tarefas de manutenção de modo automático. Há uma tarefa agendada que é definida para ser executada, por padrão, às 3:00, horário local, todos os dias. Essa tarefas agendada executa uma lista de tarefas, incluindo a limpeza do Windows Update. Com este comando do PowerShell, você pode exibir todas as categorias de manutenção que ocorrem automaticamente:
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

Um dos desafios de uma VDI não persistente é que quase toda a atividade do sistema operacional é descartada quando um usuário faz logoff. O estado e/ou perfil do usuário pode ser salvo, mas a máquina virtual em si descarta quase todas as alterações que foram feitas desde a última reinicialização. Portanto, as otimizações destinadas a um computador Windows que salva o estado de uma sessão para outra não são aplicáveis.

Dependendo da arquitetura da VM de VDI, itens como PreFetch e SuperFetch não vão ajudar de uma sessão para outra, pois todas as otimizações são descartadas na reinicialização da VM. A indexação pode ser um desperdício parcial de recursos, pois seria qualquer otimização de disco, como uma desfragmentação tradicional.

### <a name="to-sysprep-or-not-sysprep"></a>Para sysprep ou não sysprep

O Windows 10 tem uma funcionalidade interna chamada [Ferramenta de Preparação do Sistema](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) (geralmente abreviada para "Sysprep"). A ferramenta Sysprep é usada para preparar uma imagem personalizada do Windows 10 para duplicação. O processo Sysprep garante que o sistema operacional resultante seja adequadamente exclusivo para ser executado no ambiente de produção.
Há argumentos a favor e contra a execução de Sysprep. No caso da VDI, talvez seja conveniente a capacidade de personalizar o perfil do usuário padrão que será usado como o modelo de perfil para usuários subsequentes que fazem logon usando essa imagem. Você pode ter os aplicativos que deseja instalados, mas também pode controlar as configurações por aplicativo.

A alternativa é usar um ISO padrão a partir do qual instalar, possivelmente usando um arquivo de resposta de instalação autônoma, bem como uma sequência de tarefas para instalar ou remover aplicativos. Também é possível usar uma sequência de tarefas para definir as configurações da política local, talvez usando a ferramenta [LGPO (Utilitário de Objeto da Política de Grupo Local)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

#### <a name="vdi-optimization-categories"></a>Categorias de otimização da VDI

- Configurações globais do sistema operacional

    - Limpeza do aplicativo UWP

    - Limpeza dos recursos opcionais

    - Configurações de política local

    - Serviços do sistema

    -   Tarefas Agendadas

    -   Aplicar atualizações do Windows

    -   Rastreamentos automáticos do Windows

    -   Limpeza de disco antes da finalização da imagem (validação)

-   Configurações do usuário

-   Configurações do host/hipervisor

- [Ajustando o desempenho de rede do Windows 10 usando as configurações do registro](#tuning-windows-10-network-performance-by-using-registry-settings)

- Configurações adicionais das diretrizes de [Linha de base da funcionalidade limitada de tráfego restrita pelo Windows](https://go.microsoft.com/fwlink/?linkid=828887).

- [Limpeza de disco](#disk-cleanup-including-using-the-disk-cleanup-wizard)

### <a name="universal-windows-platform-app-cleanup"></a>Limpeza de aplicativo da Plataforma Universal do Windows

Uma das metas de uma imagem de VDI é ser tão pequena quanto possível. Uma maneira de reduzir o tamanho da imagem é remover os aplicativos UWP que não serão usados no ambiente. Com aplicativos UWP, há os principais arquivos de aplicativo, também conhecidos como conteúdo. Há uma pequena quantidade de dados armazenados em cada perfil de usuário para configurações específicas do aplicativo. Também há uma pequena quantidade de dados no perfil Todos os Usuários.

Conectividade e tempo são tudo no que diz respeito à limpeza de aplicativo UWP. Se você implantar sua imagem base em um dispositivo sem conectividade de rede, o Windows 10 não poderá se conectar à Microsoft Store e baixará aplicativos e tentará instalá-los enquanto você estiver tentando desinstalá-los.

Se você modificar seu .WIM base usado para instalar o Windows 10 e remover aplicativos UWP desnecessários do .WIM antes da instalação, os aplicativos não serão instalados logo no início e o tempo de criação do seu perfil deverá ser mais curto. Mais adiante nesta seção, você encontrará informações sobre como remover aplicativos UWP do seu arquivo .WIM de instalação.

Uma boa estratégia de VDI é provisionar os aplicativos que deseja na imagem base e limitar ou bloquear o acesso subsequente à Microsoft Store. Os aplicativos da Store são atualizados periodicamente em segundo plano em computadores normais. Os aplicativos UWP podem ser atualizados durante a janela de manutenção quando outras atualizações são aplicadas.

#### <a name="delete-the-payload-of-uwp-apps"></a>Excluir o conteúdo de aplicativos UWP

Os aplicativos UWP que não são necessários continuam no sistema de arquivos consumindo um pequeno volume de espaço em disco. Em aplicativos que nunca serão necessários, o conteúdo de aplicativos UWP indesejados pode ser removido da imagem base usando comandos do PowerShell.

Na verdade, se forem removidos do arquivo .WIM de instalação usando os links fornecidos mais adiante nesta seção, deverá ser possível começar com uma lista bem enxuta de aplicativo UWP.

Execute o seguinte comando para enumerar os aplicativos UWP provisionados a partir de um sistema operacional Windows 10 em execução, como nesta saída de exemplo truncada do PowerShell:

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0
    Architecture : neutral
    ResourceId   : \~
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
```

Os aplicativos UWP que são provisionados para um sistema podem ser removidos durante a instalação do sistema operacional como parte de uma sequência de tarefas, ou posteriormente, depois que o sistema operacional estiver instalado. Esse pode ser o método preferido, pois torna modular todo o processo geral de criar ou manter uma imagem. Depois de desenvolver os scripts, se algo mudar em um build subsequente, você edita um script existente em vez de repetir o processo do zero. Veja a seguir alguns links para informações sobre este tópico:

[Removendo aplicativos nativos do Windows 10 durante uma sequência de tarefas](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence)

[Removendo aplicativos internos do arquivo WIM do Windows 10 com o Powershell – versão 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)

[Windows 10 1607: impedindo aplicativos de retornarem durante a implantação da atualização de recursos](/archive/blogs/mniehaus/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update)

Execute o comando do PowerShell [Remove-AppxProvisionedPackage](/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) para remover o conteúdo do aplicativo UWP:

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

Cada aplicativo UWP deve ser avaliado para aplicabilidade em cada ambiente exclusivo. Você desejará fazer uma instalação padrão do Windows 10, versão 1803, e observar quais aplicativos estão sendo executados e consumindo memória. Por exemplo, talvez você queira considerar a remoção de aplicativos que são iniciados automaticamente, ou aplicativos que exibem informações automaticamente no menu Iniciar, como Clima e Notícias, e que podem não ser de uso no seu ambiente.

Um dos aplicativos UWP "nativos", chamado Fotos, tem uma configuração padrão chamada **Mostrar uma notificação quando novos álbuns estiverem disponíveis**.  O aplicativo Fotos pode usar aproximadamente 145 MB de memória; especificamente, memória do conjunto de trabalho privado, mesmo se não estiver sendo usado.  Alterar a configuração **Mostrar uma notificação quando novos álbuns estiverem disponíveis** para todos os usuários não é prático nesse momento, portanto, a recomendação é remover o aplicativo Fotos se ele não for necessário nem desejado.

### <a name="clean-up-optional-features"></a>Limpar recursos opcionais

#### <a name="managing-optional-features-with-powershell"></a>Gerenciando recursos opcionais com o PowerShell

 Para enumerar os recursos do Windows atualmente instalados, execute este comando do PowerShell:

```powershell
Get-WindowsOptionalFeature -Online
```

Você pode habilitar ou desabilitar um recurso opcional específico do Windows, como neste exemplo:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay"
```

Para saber mais, confira o [fórum do Windows PowerShell](https://docs.microsoft.com/answers/topics/windows-server-powershell.ht).

#### <a name="enable-or-disable-windows-features-by-using-dism"></a>Habilitar ou desabilitar recursos do Windows usando o DISM

Você pode usar a ferramenta **Dism.exe** para enumerar e controlar os recursos opcionais do Windows. Você pode configurar um script Dism.exe para ser executado durante uma sequência de tarefas que instala o sistema operacional.

### <a name="local-policy-settings"></a>Configurações de política local

Muitas otimizações para Windows 10 em um ambiente VDI podem ser feitas usando a política do Windows. As configurações listadas aqui podem ser aplicadas localmente à imagem base. Desse modo, se as configurações equivalentes não forem especificadas de alguma outra maneira, como por política de grupo, elas ainda serão aplicadas.

Algumas decisões podem ser baseadas nas especificações do ambiente, por exemplo:

-   O ambiente VDI tem permissão para acessar a Internet?

-   A solução VDI é persistente ou não persistente?

As configurações a seguir especificamente não contam ou entram em conflito com qualquer configuração que tenha relação com a segurança. Essas configurações foram escolhidas para remover as configurações que podem não se aplicar aos ambientes VDI.

> [!NOTE]
> Nesta tabela de configurações de política de grupo, os itens marcados com um asterisco são da [Linha de base da funcionalidade limitada de tráfego restrita pelo Windows](https://go.microsoft.com/fwlink/?linkid=828887).

| Configuração de política | Item | Subitem | Configuração possível e comentários |
|--|--|--|--|
| **Política do Computador Local \\ Configuração do Computador \\ Configurações do Windows \\ Configurações de Segurança** |  |  |  |
| **Políticas do Gerenciador de Listas de Redes** | Todas as propriedades de redes | Local de rede | Usuário não pode alterar o local |
| **Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Painel de Controle** |  |  |  |
| \***Painel de Controle** | Permitir dicas online |  | Desabilitada (configurações não contatarão os serviços de conteúdo da Microsoft para recuperar dicas e conteúdo de ajuda) |
| **\*Painel de Controle**\\ Personalização | Não exibir a tela de bloqueio |  | Habilitada (essa configuração de política controla se a tela de bloqueio é exibida para os usuários. Se você habilitar essa configuração de política, os usuários que não precisam pressionar Ctrl+Alt+Del antes de entrar verão o respectivo bloco selecionado após bloqueio do computador). |
| **\*Painel de Controle**\\ Personalização | Forçar uma imagem de logon e tela de bloqueio padrão específica | [![Imagem da interface do usuário para definir o caminho para a imagem da tela de bloqueio](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | Habilitada (essa configuração permite que você especifique a tela de bloqueio padrão e a imagem de logon mostradas quando nenhum usuário está conectado, bem como defina a imagem especificada como o padrão para todos os usuários – ela substitui a imagem padrão). Uma imagem não complexa, de baixa resolução, faz com que menos dados sejam transmitidos pela rede toda vez que a imagem é renderizada. |
| **\*Painel de Controle**\\ Opções Regionais e de Idiomas\\Personalização de Manuscrito | Desativar aprendizado automático |  | Habilitada (se você habilitar essa configuração de política, o aprendizado automático é interrompido e os dados armazenados são excluídos. Os usuários não podem definir essa configuração no Painel de Controle). |
| **Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Rede** |  |  |  |
| **BITS (serviço de Transferência Inteligente em Segundo Plano)** | Não permitir que o cliente BITS use o Windows Branch Cache |  | Habilitada |
| **BITS (serviço de Transferência Inteligente em Segundo Plano)** | Não permitir que o computador aja como um cliente de cache de sistemas pares do BITS |  | Habilitada |
| **BITS (serviço de Transferência Inteligente em Segundo Plano)** | Não permitir que o computador aja como um servidor de cache de sistemas pares do BITS |  | Habilitada |
| **BITS (serviço de Transferência Inteligente em Segundo Plano)** | Permitir cache de sistemas pares do BITS |  | Desabilitado |
| **BranchCache** | Ativar o BranchCache |  | Desabilitado |
| \***Fontes** | Habilitar Provedores de Fontes |  | Desabilitada (o Windows não se conecta a um provedor de fontes online e enumera apenas as fontes instaladas localmente). |
| **Autenticação do Hotspot** | Habilitar a autenticação do hotspot |  | Desabilitado |
| **Serviços de rede ponto a ponto da Microsoft** | Desativar os Serviços de rede ponto a ponto da Microsoft |  | Habilitada |
| **Indicador de Status da Conectividade de Rede** (observe que há outras configurações nesta seção que podem ser usadas em redes isoladas) | Especificar a sondagem passiva | Desabilitar a sondagem passiva (caixa de seleção) | Habilitada (use essa configuração se estiver em uma rede isolada ou usando endereços IP estáticos). |
| **Arquivos Offline** | Autorizar ou desautorizar o uso do recurso Arquivos Offline |  | Desabilitado |
| **\*Configurações TCPIP**\\ Tecnologias de transição IPv6 | Definir o Estado Teredo | Estado Desabilitado | Habilitada (no estado desabilitado, não há presença de interfaces Teredo no host). |
| **\*Serviço WLAN**\\ Configurações de WLAN | Permitir que o Windows se conecte automaticamente a hotspots abertos sugeridos, a redes compartilhadas por contatos e a hotspots que oferecem serviços pagos |  | Desabilitada (**Conectar a hotspots abertos sugeridos**, **Conectar a redes compartilhadas por meus contatos** e **Habilitar serviços pagos** serão desativadas e os usuários nesse dispositivo serão impedidos de habilitá-las). |
| **Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Menu Iniciar e Barra de Tarefas** |  |  |  |
| \***Notificações** | Desabilitar o uso da rede para notificações |  | Habilitada (se você habilitar essa configuração de política, os aplicativos e recursos do sistema não poderão receber notificações do WNS pela rede nem usando APIs de sondagem de notificação). |
| **Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Sistema** |  |  |  |
| **Instalação do Dispositivo** | Não enviar um Relatório de Erros do Windows quando um driver genérico for instalado em um dispositivo |  | Habilitada |
| **Instalação do Dispositivo** | Impedir a criação de um ponto de restauração do sistema durante atividade do dispositivo que normalmente solicitaria essa criação |  | Habilitada |
| **Instalação do Dispositivo** | Impedir a recuperação de metadados do dispositivo da Internet |  | Habilitada |
| **Instalação do Dispositivo** | Impedir o Windows de enviar um relatório de erros quando um driver de dispositivo solicitar software adicional durante a instalação |  | Habilitada |
| **Instalação do Dispositivo** | Desativar os balões de **Novo Hardware Encontrado** durante a instalação de dispositivos |  | Habilitada |
| **Sistema de arquivos**\\NTFS | Opções de criação de nome curto | Desabilitado em todos os volumes | Habilitada |
| \***Política de Grupo** | Configurar vinculação entre a Web e os aplicativos com manipuladores de URL do aplicativo |  | Desabilitado (desabilita a vinculação entre a Web e os aplicativos, os URIs http(s) serão abertos no navegador padrão em vez de iniciar o aplicativo associado.) |
| \***Política de Grupo** | Continuar experiências neste dispositivo |  | Desabilitada (o dispositivo Windows não é detectável por outros dispositivos e não pode participar de experiências entre dispositivos). |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar o acesso a todos os recursos do Windows Update |  | Habilitada (se você habilitar essa configuração de política, todos os recursos do Windows Update serão removidos. Isso inclui bloquear o acesso ao site do Windows Update em https://windowsupdate.microsoft.com, do hiperlink do Windows Update no menu Iniciar e também no menu Ferramentas no Internet Explorer. A atualização automática do Windows também está desabilitada; você não será notificado nem receberá atualizações críticas do Windows Update. Essa configuração de política também impede que o Gerenciador de Dispositivos instale automaticamente atualizações de driver do site Windows Update). |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar Atualização Automática de Certificados Raiz |  | Habilitada (se você habilitar essa configuração de política, quando for apresentado um certificado emitido por uma autoridade raiz não confiável, seu computador não contatará o site Windows Update para verificar se a Microsoft adicionou a autoridade de certificação à sua lista de autoridades confiáveis).    **OBSERVAÇÃO:** use essa política somente se você tiver um meio alternativo para a lista de certificados revogados mais recente. |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar links "Events.asp" do Visualizador de Eventos |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar compartilhamento de dados de personalização de manuscrito |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar relatório de erros de reconhecimento de manuscrito |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar conteúdo do tipo "Você sabia" no Centro de Ajuda e Suporte conteúdo |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar pesquisa do Centro de Ajuda e Suporte na Base de Dados de Conhecimento Microsoft |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar o Assistente para Conexão com a Internet se a URL de conexão fizer referência a Microsoft.com |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar o download da Internet para assistentes de publicação na Web e de encomendas online |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar o serviço de associação de arquivo pela Internet |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar Registro se a URL de conexão fizer referência a Microsoft.com |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar a tarefa "Encomendar Cópias" de fotos |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar a tarefa "Publicar na Web" de arquivos e pastas |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar o Programa de Satisfação do Usuário do Windows Messenger |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar o Programa de Aperfeiçoamento da Experiência do Usuário do Windows |  | Habilitada |
| **\*Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar testes ativos do Indicador do Status da Conectividade da Rede do Windows |  | Habilitada (Essa configuração de política desativa os testes ativos executados pelo NCSI, o Indicador do Status da Conectividade da Rede do Windows, para determinar se o seu computador está conectado à Internet ou a uma rede mais limitada. Como parte de determinar o nível da conectividade, o NCSI executa um dos dois testes ativos: baixar uma página de um servidor Web dedicado ou fazer uma solicitação de DNS para um endereço dedicado. Se você habilitar essa configuração de política, o NCSI não executará qualquer um dos dois testes ativos. Isso pode reduzir a capacidade do NCSI, e de outros componentes que usam o NCSI, de determinar o acesso à Internet.) OBSERVAÇÃO: há outras políticas que permitem redirecionar os testes de NCSI para recursos internos, caso essa funcionalidade seja desejada. |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar Relatório de Erros do Windows |  | Habilitada |
| **Gerenciamento de Comunicação da Internet**\\ Configurações de Comunicação da Internet | Desativar a procura por drivers de dispositivo no Windows Update |  | Habilitada |
| **Logon** | Mostrar animação de primeira entrada |  | Desabilitado |
| **Logon** | Desativar notificações de aplicativos na tela de bloqueio |  | Habilitada |
| **Logon** | Desativar som de Inicialização do Windows |  | Habilitada |
| **Gerenciamento de Energia** | Selecionar um plano de energia ativo | Alto Desempenho | Habilitada |
| **Recuperação** | Permitir restauração do sistema ao estado padrão |  | Desabilitado |
| \***Integridade de Armazenamento** | Permitir o download de atualizações para o Modelo de Previsão de Falha de Disco |  | Desabilitada (As atualizações não serão baixadas para o Modelo de Falha de Previsão de Falha de Disco.) |
| \***Serviços de Tempo do Windows**\\ Provedores de Tempo | Habilitar Windows NTP Client |  | Desabilitada (Se você desabilitar ou não definir essa configuração de política, o relógio do computador local não sincronizará a hora com os servidores NTP.) **OBSERVAÇÃO**: *considere essa configuração com muito cuidado*. Os dispositivos Windows que ingressaram em um domínio devem usar **NT5DS**. O controlador de domínio para o controlador do domínio primário pode usar NTP. A função PDCe pode usar NTP. Máquinas virtuais, às vezes, usam "aprimoramentos" ou "serviços de integração". |
| **Solução de Problemas e Diagnóstico**\\ Manutenção Agendada | Configurar Comportamento de Manutenção Programada |  | Desabilitado |
| **Solução de Problemas e Diagnóstico**\\ Diagnóstico de Desempenho de Inicialização do Windows | Configurar Nível de Execução do Cenário |  | Desabilitado |
| **Solução de Problemas e Diagnóstico**\\ Diagnóstico de Desempenho de Inicialização do Windows | Configurar Nível de Execução do Cenário |  | Desabilitado |
| **Solução de Problemas e Diagnóstico**\\ Detecção e Resolução de Exaustão de Recursos do Windows | Configurar Nível de Execução do Cenário |  | Desabilitado |
| **Solução de Problemas e Diagnóstico**\\ Diagnóstico de Desempenho de Desligamento do Windows | Configurar Nível de Execução do Cenário |  | Desabilitado |
| **Solução de Problemas e Diagnóstico**\\ Diagnóstico de Desempenho do Windows em Espera/Reiniciado | Configurar Nível de Execução do Cenário |  | Desabilitado |
| **Solução de Problemas e Diagnóstico**\\ Diagnóstico do Desempenho da Capacidade de Resposta do Sistema Windows | Configurar Nível de Execução do Cenário |  | Desabilitado |
| \***Perfis do Usuário** | Desativar a ID de anúncio |  | Habilitada (Se você habilitar essa configuração de política, o ID de anúncio será desativada. Os aplicativos não podem usar a ID para experiências entre aplicativos.) |
| **Política do Computador Local \\ Configuração do Computador \\ Modelos Administrativos \\ Componentes do Windows** |  |  |  |
| **Adicionar recursos ao Windows 10** | Evitar a execução do assistente |  | Habilitada |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem os dados da conta | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar as informações da conta e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que aplicativos do Windows acessem o histórico de chamadas | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o histórico de chamadas e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem os contatos | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar os contatos e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que aplicativos do Windows acessem informações de diagnóstico sobre outros aplicativos | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você desabilitar ou não definir essa configuração de política, os funcionários da sua organização poderão decidir se os aplicativos do Windows poderão obter informações de diagnóstico sobre outros aplicativos usando Configurações \> Privacidade no dispositivo.) |
| \***Privacidade do Aplicativo** | Permitir que aplicativos do Windows acessem o email | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção "Forçar Negação", os aplicativos do Windows poderão acessar o email e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem a localização | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o local e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem mensagens | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o local e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que aplicativos do Windows acessem movimento | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar dados de movimento e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que aplicativos do Windows acessem notificações | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar notificações e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que aplicativos do Windows acessem Tarefas | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar as tarefas e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem o calendário | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o calendário e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem a câmera | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar a câmera e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem o microfone | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar o microfone e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows acessem dispositivos confiáveis | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão acessar dispositivos confiáveis e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows se comuniquem com dispositivos desemparelhados | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão se comunicar com dispositivos sem fio desemparelhados e os funcionários da organização não poderão alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que aplicativos do Windows acessem rádios | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não terão acesso ao controle de rádios e os funcionários da organização não poderão alterá-la.) |
| **Privacidade do Aplicativo** | Permitir que os aplicativos do Windows façam chamadas telefônicas | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Os aplicativo do Windows não podem fazer chamadas telefônicas e os funcionários da organização não podem alterá-la.) |
| \***Privacidade do Aplicativo** | Permitir que os aplicativos do Windows sejam executados em segundo plano | Padrão para todos os aplicativos: Forçar Negação | Habilitada (Se você escolher a opção **Forçar Negação**, os aplicativos do Windows não poderão ser executados em segundo plano e os funcionários da organização não poderão alterá-la.) |
| **Políticas de Reprodução Automática** | Definir o comportamento padrão de Execução Automática | Não executar comandos de execução automática | Habilitada |
| \***Políticas de Reprodução Automática** | Desativar Reprodução Automática |  | Habilitada (Se você habilitar essa configuração de política, a Reprodução Automática será desabilitada nas unidades de CD-ROM e mídia removível ou desabilitada em todas as unidades.) |
| \***Conteúdo de Nuvem** | Não mostrar dicas do Windows |  | Habilitada (Essa configuração de política impede que dicas do Windows sejam mostradas aos usuários.) |
| \***Conteúdo de Nuvem** | Desativar as experiências do cliente da Microsoft |  | Habilitada (Se você habilitar essa configuração de política, os usuários não verão mais recomendações personalizadas da Microsoft e notificações sobre a respectiva conta da Microsoft.) |
| \***Coleta de Dados e Versões Prévias** | Permitir Telemetria | 0 – Segurança [Somente Empresa] | Habilitada (Configurar um valor 0 se aplica a dispositivos que executam apenas as edições Enterprise, Education, IoT ou Windows Server.) |
| \***Coleta de Dados e Versões Prévias** | Não mostrar notificações de comentários |  | Habilitada |
| \***Coleta de Dados e Versões Prévias** | Ativar/desativar o controle do usuário sobre builds do Insider |  | Desabilitado |
| **Otimização de Entrega** | Modo de download | Modo de Download: Simples (99) | 99 = Modo de download simples sem emparelhamento. A Otimização de Entrega baixa usando apenas HTTP e não tenta contatar os serviços de nuvem da Otimização de Entrega. |
| **Gerenciador de Janelas da Área de Trabalho** | Não permitir invocação de Flip 3D |  | Habilitada |
| **Gerenciador de Janelas da Área de Trabalho** | Não permitir animações de janelas |  | Habilitada |
| **Gerenciador de Janelas da Área de Trabalho** | Use cor sólida para o fundo da tela inicial |  | Habilitada |
| **UI Borda** | Permitir passar o dedo na borda |  | Desativar |
| **UI Borda** | Desabilitar dicas de ajuda |  | Habilitada |
| \***Explorador de Arquivos** | Configurar o Windows Defender SmartScreen |  | Desabilitada (O SmartScreen será desativado para todos os usuários. Os usuários não serão avisados se tentarem executar aplicativos suspeitos da Internet.) |
|  |  |  | **OBSERVAÇÃO**: se não conectados à internet, isso impedirá que os computadores tentem contatar a Microsoft para obter informações sobre o SmartScreen. |
| **Explorador de Arquivos** | Não mostrar a notificação de **novo aplicativo instalado** |  | Habilitada |
| \***Localizar Meu Dispositivo** | Ativar/desativar Localizar Meu Dispositivo |  | Desabilitada (Quando a opção Localizar Meu Dispositivo está desligada, o dispositivo e seu local não são registrados e o recurso não funciona. O usuário também não poderá exibir o local do último uso de seu digitalizador ativo no dispositivo.) |
| **Navegador de Jogos** | Desativar download de informações de jogos |  | Habilitada |
| **Navegador de Jogos** | Desativar atualizações de jogos |  | Habilitada |
| **Navegador de Jogos** | Desativar rastreamento da última hora de jogos na pasta Jogos |  | Habilitada |
| **Grupo Doméstico** | Impedir que o computador ingresse em um grupo doméstico |  | Habilitada |
| \***Internet Explorer** | Permitir que os serviços Microsoft forneçam sugestões avançadas enquanto o usuário digita algo na barra de endereços |  | Desabilitada (Os usuários não receberão sugestões avançadas ao digitar na barra de endereços. Além disso, os usuários não poderão alterar a configuração Sugestões.) |
| **Internet Explorer** | Desabilitar a verificação periódica de atualizações de software do Internet Explorer |  | Habilitada |
| **Internet Explorer** | Desabilitar a exibição da tela inicial |  | Habilitada |
| **Internet Explorer** | Instalar novas versões do Internet Explorer automaticamente |  | Desabilitado |
| **Internet Explorer** | Impedir participação no Programa de Aperfeiçoamento da Experiência do Usuário |  | Habilitada |
| **Internet Explorer** | Impedir a execução do Assistente de Primeira Execução | Ir direto para a home page | Habilitada |
| **Internet Explorer** | Definir crescimento de processos de guias | Baixa | Habilitada |
| **Internet Explorer** | Especificar categoria padrão para uma nova guia | Nova página de guia | Habilitada |
| **Internet Explorer** | Desativar notificações de desempenho de complementos |  | Habilitada |
| \***Internet Explorer** | Desativar o recurso de preenchimento automático em endereços da Web |  | Habilitada (Se você habilitar essa configuração de política, não serão feitas sugestões de correspondências para o usuário quando ele inserir endereços da Web. O usuário não pode alterar o preenchimento automático para a configuração de endereços da web.) |
| \***Internet Explorer** | Desativar a geolocalização do navegador |  | Habilitada (Se você habilitar essa configuração de diretiva, o suporte à localização geográfica do navegador será desativado.) |
| **Internet Explorer** | Desativar Reabrir Última Sessão de Navegação |  | Habilitada |
| \***Internet Explorer** | Ativar o recurso Sites Sugeridos |  | Desabilitada (Se você desabilitar essa configuração de política, os pontos de entrada e a funcionalidade associada a esse recurso serão desativados.) |
| **\*Internet Explorer**\\ Modo de Exibição de Compatibilidade | Desativar Modo de Exibição de Compatibilidade |  | Habilitada (Se você habilitar essa configuração de política, o usuário não poderá usar o botão Modo de Exibição de Compatibilidade nem gerenciar a lista de sites do Modo de Exibição de Compatibilidade.) |
| **Internet Explorer**\\ Painel de Controle da Internet<strong>\\</strong> Página Avançada | Reproduzir animações em páginas da Web |  | Desabilitado |
| **Internet Explorer**\\ Painel de Controle da Internet\\ Página Avançada | Executar vídeos em páginas da Web |  | Desabilitado |
| **\*Internet Explorer**\\ Painel de Controle da Internet\\ Página Avançada | Desativar o recurso de virar com previsão de página |  | Habilitada (A Microsoft coleta seu histórico de navegação para aprimorar o funcionamento do recurso de virar com previsão de página. Esse recurso não está disponível no Internet Explorer para área de trabalho. Se você habilitar essa configuração de política, o recurso de virar com previsão de página será desativado e a próxima página da Web não será carregada em segundo plano.) |
| **Internet Explorer**\\ Configurações da Internet\\ Configurações Avançadas\\ Navegação | Desativar a detecção de número de telefone |  | Habilitada |
| **Internet Explorer**\\ Configurações da Internet\\ Configurações Avançadas\\ Multimídia | Permitir que o Internet Explorer reproduza arquivos de mídia que usem codecs alternativos |  | Desabilitado |
| \***Local e sensores** | Desativar local |  | Habilitada (Se você habilitar essa configuração de política, o recurso de local será desativado, e todos os programas nesse computador serão impedidos de usar informações de local do recurso de local.) |
| **Local e sensores** | Desativar sensores |  | Habilitada |
| **Locais e sensores /** Localizador do Windows | Desativar Localizador do Windows |  | Habilitada |
| \***Mapas** | Desativar Download e Atualização Automáticos de Dados do Mapa |  | Habilitada (se você habilitar essa configuração, o download e a atualização automáticos dos dados do mapa serão desativados). |
| \***Mapas** | Desativar tráfego de rede não solicitado na página de configurações de Mapas Offline |  | Habilitada (Se você habilitar essa configuração de política, os recursos que geram tráfego de rede na página de configurações Mapas Offline serão desativados. Observação: isso pode desativar a página inteira de configurações.) |
| \***Mensagens** | Permitir sincronização de nuvem do serviço de mensagem |  | Desabilitada (Essa configuração de política permite o backup e a restauração de mensagens de texto de celular para serviços de nuvem da Microsoft.) |
| \***Microsoft Edge** | Permitir sugestões na lista suspensa da barra de endereços |  | Desabilitado |
| \***Microsoft Edge** | Permitir atualizações de configuração para a Biblioteca de Livros |  | Desabilitada (Desativa as listas de compatibilidade no Microsoft Edge.) |
| \***Microsoft Edge** | Permite Lista de Compatibilidade da Microsoft |  | Desabilitada (se você desabilitar essa configuração, a Lista de Compatibilidade da Microsoft não será usada durante a navegação no navegador.) |
| \***Microsoft Edge** | Permitir conteúdo da Web na página Nova Guia |  | Desabilitada (Direciona o Microsoft Edge a abrir com conteúdo em branco quando uma nova guia é aberta.) |
| \***Microsoft Edge** | Configurar o Preenchimento automático |  | Desabilitada (Desabilita o preenchimento automático na barra de endereços.) |
| \***Microsoft Edge** | Configurar o recurso Não Rastrear |  | Habilitada (Se você habilitar essa configuração, as solicitações Não Rastrear sempre serão enviadas a sites solicitando informações de rastreamento.) |
| \***Microsoft Edge** | Configurar o Gerenciador de Senhas |  | Habilitada (Se você desabilitar essa configuração, os funcionários não poderão usar o Gerenciador de Senhas para salvar suas senhas localmente.) |
| \***Microsoft Edge** | Configurar sugestões de pesquisa na barra de endereços |  | Desabilitada (Os usuários não veem sugestões de pesquisa na Barra de endereços do Microsoft Edge.) |
| \***Microsoft Edge** | Configurar páginas iniciais |  | Habilitada (Se você habilitar essa configuração, será possível configurar uma ou mais páginas iniciais.) Se essa configuração for habilitada, você também deverá incluir URLs para as páginas, separando várias páginas usando sinais de maior e menor neste formato: \<support.contoso.com\>\<support.microsoft.com\> Windows 10, versão 1703 ou posterior: Caso não deseje enviar o tráfego para a Microsoft, use o valor \<about:blank\>, que é respeitado para dispositivos ingressados ou não em um domínio, quando se tratar da única URL configurada. |
| \***Microsoft Edge** | Configurar o Windows Defender SmartScreen |  | Desabilitada (O Windows Defender SmartScreen é desativado e os funcionários não podem ativá-lo.) |
|  |  |  | **OBSERVAÇÃO**: considere essa configuração dentro do ambiente. Se não conectados à internet, isso impedirá que os computadores tentem contatar a Microsoft para obter informações sobre o SmartScreen. |
| \***Microsoft Edge** | Impedir que a página da Web Primeira Execução abra no Microsoft Edge |  | **Habilitada** (Os usuários não verão a página Primeira Execução ao abrir o Microsoft Edge pela primeira vez.) |
| **OneDrive** | Impedir o OneDrive de gerar tráfego de rede até o usuário entrar no OneDrive |  | **Habilitada** (Habilite essa configuração para impedir que o cliente de sincronização do OneDrive, o OneDrive.exe, gere tráfego de rede, como verificação por atualizações, por exemplo, até que o usuário entre no OneDrive ou inicie a sincronização de arquivos com o computador local.) |
| \***OneDrive** | Impedir o uso do OneDrive para armazenamento de arquivos |  | **Habilitada** (A menos que o OneDrive seja usado no local ou externamente.) |
| **OneDrive** | Salvar documentos no OneDrive por padrão |  | **Desabilitada** (A menos que o OneDrive seja usado no local ou externamente.) |
| **RSS Feeds** | Impedir a descoberta automática de feeds e Web Slices |  | **Habilitada** |
| \***RSS Feeds** | Desativar a sincronização em segundo plano de feeds e Web Slices |  | **Habilitada** (Se você habilitar essa configuração de politica, a capacidade de sincronizar feeds e Web Slices em segundo plano será desativada.) |
| \***Pesquisa** | Permitir a Cortana |  | **Desabilitada** (Quando a Cortana é desativada, os usuários ainda podem usar a pesquisa para encontrar itens no dispositivo.) |
| **Pesquisa** | Permitir a Cortana acima da tela de bloqueio |  | **Desabilitada** |
| \***Pesquisa** | Permitir que a pesquisa e a Cortana usem a localização |  | **Desabilitada** |
| **Pesquisa** | Não permitir pesquisas na Web |  | **Habilitada** |
| \***Pesquisa** | Não pesquisar na Web nem exibir resultados da Web na Pesquisa |  | **Habilitada** (Se você habilitar essa configuração de política, as consultas não serão realizadas na Web e os resultados da Web não serão exibidos quando um usuário executar uma consulta em Pesquisa.) |
| **Pesquisa** | Impedir a adição de locais UNC ao índice do Painel de Controle |  | **Habilitada** |
| **Pesquisa** | Impedir indexação de arquivos no cache de arquivos offline |  | **Habilitada** |
| \***Pesquisa** | Definir quais informações são compartilhadas na Pesquisa | Informações anônimas | **Habilitada** (Compartilhe informações de uso, mas não compartilhe o histórico de pesquisa, informações de conta da Microsoft ou local específico.) |
| \***Plataforma de Proteção de Software** | Desativar Validação AVC Online no Cliente KMS |  | **Habilitada** (Habilitar essa configuração impede o computador de enviar à Microsoft dados relacionados ao seu estado de ativação.) |
| \***Fala** | Permitir a Atualização Automática de Dados de Fala |  | **Desabilitada** (Não será verificada periodicamente em busca de modelos de fala atualizados.) |
| \***Store** | Desabilitar Download e Instalação Automáticos de Atualizações |  | **Habilitada** (Se você habilitar essa configuração, o download e a instalação automáticos das atualizações do aplicativo serão desativados.) |
| \***Store** | Desativar o download automático de atualizações em dispositivos com o Win8 |  | **Habilitada** (Se você habilitar essa configuração, o download automático das atualizações do aplicativo será desativado.) |
| **Store** | Desativar a oferta de atualização para a última versão do Windows |  | **Habilitada** |
| \***Sincronizar configurações** | Não sincronizar | Permitir que os usuários ativem a sincronização (não selecionada) | **Habilitada** (Se você habilitar essa configuração de política, "sincronizar configurações" será desativada e nenhum dos grupos "sincronizar configuração" será sincronizado nesse dispositivo.) |
| **Entrada de Texto** | Aprimorar reconhecimento de escrita à tinta e digitação |  | **Desabilitada** |
| **Windows Defender Antivírus**\\ MAPS | Ingressar no Microsoft MAPS |  | **Desabilitada** (Se você desabilitar ou não definir essa configuração, não será possível ingressar no Microsoft MAPS.) |
| **Windows Defender Antivírus**\\ MAPS | Enviar amostras de arquivo quando outra análise for necessária | Nunca enviar | **Habilitada** (Somente se não tiver aceito dados de diagnóstico do MAPS.) |
| **Windows Defender Antivírus**\\ Relatório | Desativar notificações avançadas |  | **Habilitada** (Se você habilitar essa configuração, as notificações avançadas do Windows Defender Antivírus não serão exibidas em clientes.) |
| **Windows Defender Antivírus**\\ Atualizações de Assinaturas | Definir a ordem das origens para baixar atualizações de definição | FileShares | **Habilitada** (Se você habilitar essa configuração, as origens da atualização de definição serão contatadas na ordem especificada. Depois que as atualizações de definição forem baixadas com êxito de uma origem especificada, as origens restantes na lista não serão contatadas.) |
| **Relatório de Erros do Windows** | Enviar automaticamente despejos de memória para relatórios de erro gerados pelo sistema operacional |  | **Desabilitada** |
| **Relatório de Erros do Windows** | Desabilitar o Relatório de Erros do Windows |  | **Habilitada** |
| **Gravação e Transmissão de Jogos do Windows** | Habilita ou desabilita a Gravação e a Transmissão de Jogos do Windows |  | **Desabilitada** |
| **Windows Installer** | Controlar tamanho máximo do cache de arquivo de linha de base | 5 | **Habilitada** |
| **Windows Installer** | Desabilitar a criação de pontos de verificação de Restauração do Sistema |  | **Habilitada** |
| **Windows Mail** | Desativar os recursos das Comunidades |  | **Habilitada** |
| **Windows Media Player** | Não Mostrar Caixas de Diálogo de Primeiro Uso |  | **Habilitada** |
| **Windows Media Player** | Impedir Compartilhamento de Mídia |  | **Habilitada** |
| **Centro de Mobilidade do Windows** | Desativar o Centro de Mobilidade do Windows |  | **Habilitada** |
| **Análise de Confiabilidade do Windows** | Configurar os Provedores WMI de Confiabilidade |  | **Desabilitada** |
| **Windows Update** | Permitir instalação imediata de Atualizações Automáticas |  | **Habilitada** |
| **Windows Update** | Não conectar a localizações do Windows Update na Internet |  | **Habilitada** (Habilitar essa política desabilitará essa funcionalidade e poderá causar a interrupção da conexão com serviços públicos, como a Windows Store.) **Observação**: essa política só se aplica quando o dispositivo está configurado para se conectar a um serviço de atualização da intranet usando a política "Especificar o local do serviço de atualização na intranet da Microsoft". |
| **Windows Update** | Remover acesso ao uso de todos os recursos do Windows Update |  | **Habilitada** |
| **\*Windows Update**\\ Windows Update para Empresas | Gerenciar versões prévias | Definir o comportamento de recebimento das versões prévias: | **Habilitada** (A seleção de **Desabilitar versões prévias** impedirá as versões prévias de serem instaladas no dispositivo. Isso impedirá que os usuários aceitem participar do Programa Windows Insider por meio de Configurações - \> Atualizar e Segurança.) |
|  |  | Desabilitar versões prévias |  |
| **\*Windows Update**\\ Windows Update para Empresas | Selecionar quando Versões Prévias e Atualizações de Recursos forem recebidas | Canal Semestral | **Habilitada** (Habilite essa política para especificar o nível da Versão Prévia ou as atualizações de recursos a serem recebidos, e quando.) |
|  |  | Diferimento: 365 dias, |  |
|  |  | Início da pausa: aaaa-mm-dd |  |
| **Windows Update**\\ Windows Update para Empresas | Selecionar quando as Atualizações de Qualidade serão recebidas | 1. 30 dias 2. Pausar início das atualizações de qualidade aaaa-mm-dd | **Habilitada** |
| **Configurações da Política Personalizada de Tráfego Restrita pelo Windows** | Impedir o OneDrive de gerar tráfego de rede até o usuário entrar no OneDrive |  | **Habilitada** (Habilite essa configuração se quiser impedir que o cliente de sincronização do OneDrive , o OneDrive.exe, gere tráfego de rede, como verificação por atualizações, por exemplo, até que o usuário entre no OneDrive ou inicie a sincronização de arquivos com o computador local.) |
| **Configurações da Política Personalizada de Tráfego Restrita pelo Windows** | Desativar Notificações do Windows Defender |  | **Habilitada** (Se você habilitar essa configuração de política, o Windows Defender não enviará notificações com informações críticas sobre a integridade e a segurança do seu dispositivo.) |
| **Política do Computador Local \\ Configuração do Usuário \\ Modelos Administrativos** |  |  |  |
| **Painel de Controle**\\ Opções Regionais e de Idioma | Desativar a oferta de previsões de texto ao digitar |  | **Habilitada** |
| **Área de trabalho** | Não adicionar compartilhamentos de documentos recentemente abertos a Locais de Rede |  | **Habilitada** |
| **Área de trabalho** | Desativar a janela Aero Shake minimizando o gesto do mouse |  | **Habilitada** |
| **Área de Trabalho**/Active Directory | Tamanho máximo de pesquisas do Active Directory | 2500 | **Habilitada** |
| **Menu Iniciar e Barra de Tarefas** | Não permitir a fixação de aplicativo da loja na barra de tarefas |  | **Habilitada** |
| **Menu Iniciar e Barra de Tarefas** | Não exibir nem controlar itens nas Listas de Atalhos a partir de locais remotos |  | **Habilitada** |
| **Menu Iniciar e Barra de Tarefas** | Não usar o método baseado em pesquisa ao resolver atalhos do shell |  | **Habilitada** (O sistema não realiza a pesquisa na unidade final. Ele apenas exibe uma mensagem explicando que o arquivo não foi encontrado.) |
| **Menu Iniciar e Barra de Tarefas** | Remover a Barra de Pessoas da barra de tarefas |  | **Habilitada** (O ícone de pessoas será removido da barra de tarefas, o botão de configurações correspondente será removido da página de configurações da barra de tarefas e os usuários não poderão fixar pessoas na barra de tarefas.) |
| **Menu Iniciar e Barra de Tarefas** | Desligar o recurso de notificações de balões de publicidade |  | **Habilitada** (Os usuários não podem fixar o aplicativo da loja na barra de tarefas. Se o aplicativo da loja já estiver fixado na Barra de Tarefas, ele será removido na próxima conexão dos usuários.) |
| **Menu Iniciar e Barra de Tarefas** | Desativar o rastreamento de usuário |  | **Habilitada** |
| **Menu Iniciar e Barra de Tarefas**/Notificações | Desativar notificações de aviso |  | **Habilitada** |
| **Componentes do Windows**/Conteúdo de Nuvem | Desativar todos os recursos de Destaque do Windows |  | **Habilitada** |
| **UI Borda** | Desativar rastreamento de uso do aplicativo |  | **Habilitada** |
| **Explorador de Arquivos** | Desligar armazenamento em cache de imagens em miniatura |  | **Habilitada** |
| **Explorador de Arquivos** | Desligar a exibição de entradas de pesquisas recentes na caixa de pesquisa do Explorador de Arquivos |  | **Habilitada** |
| **Explorador de Arquivos** | Desativar o armazenamento em cache das miniaturas em arquivos thumbs.db ocultos |  | **Habilitada** ||

### <a name="notes-about-network-connectivity-status-indicator"></a>Observações sobre o indicador de Status da Conectividade de Rede

As configurações de política de grupo acima incluem configurações para desativar a verificação de modo a confirmar se o sistema está conectado à Internet. Se seu ambiente não se conecta à Internet de modo algum, ou se conecta indiretamente, você poderá definir uma configuração de política de grupo para remover o ícone de rede da barra de tarefas. Se você desativar as verificações de conectividade da Internet, um sinalizador amarelo aparecerá no ícone de rede, mesmo que a rede esteja funcionando normalmente. Por esse motivo, talvez você queira remover o ícone de rede da barra de tarefas. Se quiser remover o ícone de rede como uma configuração de política de grupo, você poderá encontrar isso neste local:

| Windows Update ou Windows Update para Empresas | Selecionar quando as Atualizações de Qualidade serão recebidas | 1. 30 dias 2. Pausar início das atualizações de qualidade aaaa-mm-dd | Habilitada |
|--|--|--|--|
| **Política do Computador Local \\ Configuração do Usuário \\ Modelos Administrativos** |  |  |  |
| **Menu Iniciar e Barra de Tarefas** | Remover o ícone de sistema de rede |  | **Habilitada** (O ícone de sistema de rede não é exibido na área de notificação do sistema.) |

Para saber mais sobre o NCSI (Indicador de Status da Conexão de Rede), confira: [O ícone de Status da Conexão de Rede](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog)

### <a name="system-services"></a>Serviços do sistema

Se você estiver considerando desabilitar os serviços do sistema para conservar recursos, tome muito cuidado para que o serviço que está sendo considerado não seja de alguma forma um componente de algum outro serviço.

Além disso, a maioria dessas recomendações espelha as recomendações para o Windows Server 2016 com Experiência Desktop. Para saber mais, confira [Diretrizes sobre como desabilitar serviços do sistema no Windows Server 2016 com Experiência Desktop](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md).

Observe que muitos serviços que podem parecer bons candidatos à desabilitação são definidos como tipo de início de serviço manual. Isso significa que o serviço não será iniciado automaticamente e não é iniciado, a menos que um aplicativo ou serviço específico dispare uma solicitação para o serviço que está sendo considerado para desabilitação. Os serviços que já estão definidos para tipo de início manual normalmente são listados aqui.

| Serviço do Windows | Item | Comentário |
|--|--|--|
| CDPUserService | Esse serviço de usuário é usado para cenários de Plataforma de Dispositivos Conectados | OBSERVAÇÃO: esse é um serviço por usuário e, como tal, o *serviço de modelo* deve ser desabilitado. |
| Experiência do Usuário Conectado e Telemetria | Habilita os recursos que dão suporte a experiências de usuário no aplicativo e conectadas. Além disso, esse serviço gerencia a coleção controlada por evento e a transmissão de informações de diagnóstico e de uso (usadas para melhorar a experiência e a qualidade da Plataforma Windows) quando as configurações de opção de privacidade de uso e diagnóstico são habilitadas em Comentários e Diagnóstico. | Considere desabilitar se estiver na rede desconectada |
| Dados de Contato | Indexa dados de contato para pesquisa rápida de contatos. Se você interromper ou desabilitar esse serviço, os contatos poderão ficar ausentes dos resultados da pesquisa. | (PimIndexMaintenanceSvc) OBSERVAÇÃO: esse é um serviço por usuário e, como tal, o *serviço de modelo* deve ser desabilitado. |
| Serviço de Política de Diagnóstico | Habilita a detecção e a solução de problemas, bem como a resolução para componentes do Windows. Se esse serviço for interrompido, o diagnóstico deixará de funcionar. |  |
| Gerenciador de Mapas Baixados | O serviço Windows para acesso de aplicativo aos mapas baixados. Esse serviço é iniciado sob demanda pelo aplicativo que acessa os mapas baixados. A desabilitação desse serviço impedirá que os aplicativos acessem mapas. |  |
| Serviço de Geolocalização | Monitora o local atual do sistema e gerencia cercas geográficas |  |
| Serviço de usuário Transmissão e GameDVR | Esse serviço de usuário é usado para Gravações de Jogo e Transmissões ao Vivo | OBSERVAÇÃO: esse é um serviço por usuário e, como tal, o serviço de modelo deve ser desabilitado. |
| MessagingService | Serviço que dá suporte a mensagens de texto e funcionalidade relacionada. | OBSERVAÇÃO: esse é um serviço por usuário e, como tal, o *serviço de modelo* deve ser desabilitado. |
| Otimizar unidades | Ajuda o computador a ser executado com mais eficiência pela otimização de arquivos em unidades de armazenamento. | Soluções VDI normalmente não se beneficiam da otimização do disco. Essas "unidades" não são unidades tradicionais e, muitas vezes, são apenas uma alocação de armazenamento temporário. |
| Superfetch | Mantém e melhora o desempenho do sistema ao longo do tempo. | Em geral, não melhora o desempenho na VDI, especialmente não persistente, uma vez que o estado do sistema operacional é descartado a cada reinicialização. |
| Serviço de Teclado Virtual e Painel de Manuscrito | Habilita a funcionalidade de caneta e tinta do Teclado Virtual e do Painel de Manuscrito |  |
| Relatório de Erros do Windows | Permite que os erros sejam relatados quando os programas param de funcionar ou responder e permite que as soluções existentes sejam entregues. Também permite que os logs sejam gerados para serviços de diagnóstico e reparo. Se esse serviço for interrompido, o relatório de erros poderá não funcionar corretamente e os resultados dos serviços de diagnóstico e os reparos poderão não ser exibidos. | Com a VDI, o diagnóstico geralmente é feito em um cenário offline, e não em produção base. E, além disso, alguns clientes desabilitam o WER mesmo assim. O WER incorre em uma pequena quantidade de recursos para muitas tarefas diferentes, incluindo falha ao instalar um dispositivo ou falha ao instalar uma atualização. |
| Serviço de Compartilhamento de Rede do Windows Media Player | Compartilha bibliotecas do Windows Media Player com outros players na rede e dispositivos de mídia usando Plug and Play Universal | Não é necessário, a menos que os clientes estejam compartilhando bibliotecas do WMP na rede. |
| Serviço de Hotspot Móvel do Windows | Fornece a capacidade de compartilhar uma conexão de dados da rede celular com outro dispositivo. |  |
| Windows Search | Fornece indexação de conteúdo, cache de propriedades e resultados da pesquisa para arquivos, emails e outros tipos de conteúdo. | Provavelmente não é necessário, especialmente com VDI não persistente |

#### <a name="per-user-services-in-windows"></a>Serviços por usuário no Windows

[Serviços por usuário](/windows/application-management/per-user-services-in-windows) são serviços criados quando um usuário entra no Windows ou Windows Server e são interrompidos e excluídos quando o usuário sai. Esses serviços são executados no contexto de segurança da conta de usuário – isso fornece melhor gerenciamento de recursos que a abordagem anterior de executar esses tipos de serviço no Explorador, associados a uma conta pré-configurada, ou como tarefas.

### <a name="scheduled-tasks"></a>Tarefas Agendadas

Como outros itens no Windows, certifique-se de que o item não seja necessário antes de desabilitá-lo.

A lista a seguir é de tarefas que realizam otimizações ou coletas de dados em computadores que mantêm seu estado entre as reinicializações. Quando uma tarefa de VM da VDI reinicia e descarta todas as alterações desde a última inicialização, as otimizações destinadas a computadores físicos não são úteis.

Você pode obter todas as tarefas agendadas atuais, incluindo descrições, com o seguinte código do PowerShell:

`Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description |Export-CSV -Path C:\Temp\W10_1803_SchTasks.csv -NoTypeInformation`

Os valores válidos de **Nome da Tarefa Agendada** incluem:

- Tarefa de Atualização Autônoma do OneDrive v2
- Avaliador de Compatibilidade da Microsoft
- ProgramDataUpdater
- StartupAppTask
- CleanupTemporaryState
- Proxy
- UninstallDeviceTask
- ProactiveScan
- Consolidador
- UsbCeip
- Verificação de Integridade de Dados
- Verificação de Integridade de Dados para Recuperação de Pane
- ScheduledDefrag
- SilentCleanup
- Microsoft-Windows-DiskDiagnosticDataCollector
- Diagnóstico
- StorageSense
- DmClient
- DmClientOnScenarioDownload
- Histórico de Arquivos (modo de manutenção)
- ScanForUpdates
- ScanForUpdatesAsUser
- SmartRetry
- Notificações
- WindowsActionDialog
- Rede Celular do WinSAT
- MapsToastTask
- ProcessMemoryDiagnosticEvents
- RunFullMemoryDiagnostic
- Analisador de Metadados MNO
- LPRemove
- GatherNetworkInfo
- WiFiTask
- Tarefas do SQM
- AnalyzeSystem
- MobilityManager
- VerifyWinRE
- RegIdleBackup
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- IndexerAutomaticMaintenance
- SpaceAgentTask
- SpaceManagerTask
- HeadsetButtonPress
- SpeechModelDownloadTask
- ResPriStaticDbSync
- WsSwapAssessmentTask
- SR
- SynchronizeTimeZone
- Notificações de USB
- QueueReporting
- UpdateLibrary
- Início Agendado
- sih
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>Aplicar atualizações do Windows entre outras

Seja do Microsoft Update, seja de recursos internos, aplique as atualizações disponíveis, incluindo assinaturas do Windows Defender. Esse é um bom momento para aplicar outras atualizações disponíveis, incluindo aquelas do Microsoft Office, se instalado.

### <a name="automatic-windows-traces"></a>Rastreamentos automáticos do Windows

O Windows está configurado, por padrão, para coletar e salvar dados limitados de diagnóstico. O objetivo é habilitar o diagnóstico, ou gravar dados, caso seja necessária uma solução de problemas adicional. Você pode encontrar os rastreamentos automáticos do sistema iniciando o aplicativo Gerenciamento do Computador, expandindo **Ferramentas do Sistema**, **Desempenho**, **Conjuntos de Coletores de Dados** e selecionando **Sessões de Rastreamento de Eventos**.

Alguns dos rastreamentos exibidos em **Sessões de Rastreamento de Eventos** e **Sessões de Rastreamento de Eventos de Inicialização** não podem e não devem ser interrompidos. Outros, como o rastreamento **WiFiSession**, podem ser interrompidos. Para interromper um rastreamento em execução em **Sessões de Rastreamento de Eventos**, clique com o botão direito no rastreamento e selecione **Interromper**. Para impedir que rastreamentos iniciem automaticamente na inicialização, siga estas etapas:

1.  Selecione a pasta **Sessões de Rastreamento de Eventos de Inicialização**

2.  Localize o rastreamento de interesse e clique duas vezes nele.

3.  Selecione a guia **Sessão de Rastreamento**.

4.  Desmarque a caixa rotulada **Habilitada**.

5.  Selecione **OK**.

Veja a seguir alguns rastreamentos de sistema que você pode pensar em desabilitar para uso da VDI:

| Nome | Comentário |
|--|--|
| AppModel | Uma coleção de rastreamentos, uma delas é telefone |
| CloudExperienceHostOOBE |  |
| DiagLog |  |
| NtfsLog |  |
| TileStore |  |
| UBPM |  |
| WiFiDriverIHVSession | Se não estiver usando um dispositivo de Wi-Fi |
| WiFiSession |  |

#### <a name="servicing-the-operating-system-and-apps"></a>Manutenção do sistema operacional e de aplicativos

Em algum momento durante o processo de otimização de imagem, as atualizações disponíveis do Windows devem ser aplicadas. Você pode definir o Windows Update para instalar atualizações de outros produtos da Microsoft, bem como do Windows. Para isso, abra **Configurações do Windows**, selecione **Atualização e Segurança** e selecione **Opções Avançadas**. Selecione **Fornecer atualizações para outros produtos Microsoft quando eu atualizar o Windows** para defini-la como **Ativada**.

Essa seria uma boa configuração no caso de você instalar aplicativos Microsoft, como o Microsoft Office na imagem base. Dessa forma, o Office é atualizado quando a imagem é colocada em serviço. Também há atualizações do .NET e determinados componentes não pertencentes à Microsoft, como Adobe, cujas atualizações são disponibilizadas pelo Windows Update.

Uma consideração muito importante para VMs de VDI não persistente são as atualizações de segurança, incluindo arquivos de definição de software de segurança. Essas atualizações podem ser liberadas uma vez ou até mais de uma vez por dia. Pode haver uma maneira de reter essas atualizações, incluindo o Windows Defender e componentes não relacionados à Microsoft.

Para o Windows Defender, pode ser melhor permitir que as atualizações ocorram, mesmo em VDI não persistente. As atualizações serão aplicadas praticamente a cada sessão de logon, mas são pequenas e não devem ser um problema. Além disso, a VM não será ocultada pelas atualizações, pois apenas a última disponível será aplicada. O mesmo s aplica aos arquivos de definição não relacionados à Microsoft.

> [!NOTE]
> Os aplicativos da loja (aplicativos UWP) são atualizados pela Windows Store. As versões modernas do Office, como o Office 365, são atualizadas por meio de seus próprios mecanismos quando conectados diretamente à Internet, ou por meio de tecnologias de gerenciamento.

### <a name="windows-defender-optimization-with-vdi"></a>Otimização do Windows Defender com VDI

A Microsoft publicou recentemente a documentação relacionada ao Windows Defender em um ambiente VDI. Confira [Guia de implantação para o Windows Defender Antivírus em um ambiente VDI (Virtual Desktop Infrastructure)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus) para obter detalhes.

O artigo acima contém procedimentos para fazer a manutenção da imagem gold da VDI e de como manter os clientes VDI enquanto estão em execução. Para reduzir a largura de banda da rede quando computadores VDI precisarem atualizar suas assinaturas do Windows Defender, agende as reinicializações para fora do horário comercial quando possível. As atualizações de assinatura do Windows Defender podem estar mantidas internamente em compartilhamentos de arquivos e, quando for conveniente, esses arquivos podem compartilhados nos mesmos segmentos de rede (ou próximos) das máquinas virtuais VDI.

Veja o documento listado no início desta seção para obter muito mais informações sobre como otimizar o Windows Defender com VDI.

### <a name="tuning-windows-10-network-performance-by-using-registry-settings"></a>Ajustando o desempenho de rede do Windows 10 usando as configurações do registro

Isso é especialmente importante em ambientes onde o computador físico ou a VDI tem uma carga de trabalho que se baseia quase toda na rede. As configurações nesta seção declinam o desempenho em favor da rede, configurando armazenamento em cache e buffer de itens como entradas no diretório, entre outros.

Observe que algumas configurações nesta seção são *apenas baseadas no registro* e devem ser incorporadas na imagem base antes que ela seja implantada para uso em produção.

As configurações a seguir estão documentadas nas informações das [Diretrizes de Ajuste de Desempenho do Windows Server 2016](/windows-server/administration/performance-tuning/), publicadas em Microsoft.com pelo Grupo de Produtos do Windows.

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DisableBandwidthThrottling

Aplica-se ao Windows 10. O padrão é **0**. Por padrão, o redirecionador SMB limita a taxa de transferência em conexões de rede de alta latência, em alguns casos, para evitar tempos limite relacionados à rede. Definir esse valor de registro como **1** desabilita essa limitação, permitindo uma taxa de transferência de arquivo mais alta por conexões de rede de alta latência. Portanto, você deve considerar essa configuração.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\FileInfoCacheEntriesMax

Aplica-se ao Windows 10. O padrão é **64**, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de metadados de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando muitos arquivos são acessados. Tente aumentar esse valor para **1024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DirectoryCacheEntriesMax

Aplica-se ao Windows 10. O padrão é **16**, com um intervalo válido de 1 a 4096. Esse valor é usado para determinar a quantidade de informações de diretório que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando diretórios grandes são acessados. Considere aumentar esse valor para **1024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\FileNotFoundCacheEntriesMax

Aplica-se ao Windows 10. O padrão é **128**, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de informações de nome de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando muitos nomes de arquivo são acessados. Considere aumentar esse valor para **2048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DormantFileLimit

Aplica-se ao Windows 10. O padrão é **1023**. Esse parâmetro especifica o número máximo de arquivos que deve ser aberto em um recurso compartilhado após o aplicativo fechar o arquivo. Onde muitos milhares de clientes estiverem se conectando a servidores SMB, considere a redução desse valor para **256**.

É possível definir muitas configurações SMB usando os cmdlets [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration) e [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration) do Windows PowerShell. Você pode definir as configurações somente de registro usando também o Windows PowerShell, como no exemplo a seguir:

`Set-ItemProperty -Path "HKLM:\\SYSTEM\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters" RequireSecuritySignature -Value 0 -Force`

### <a name="additional-settings-from-the-windows-restricted-traffic-limited-functionality-baseline-guidance"></a>Configurações adicionais das diretrizes de Linha de Base da Funcionalidade Limitada de Tráfego Restrita pelo Windows

A Microsoft lançou uma linha de base criada usando os mesmos procedimentos das [Linhas de Base de Segurança do Windows](/windows/device-security/windows-security-baselines) para ambientes que não são conectados diretamente à Internet ou que querem reduzir dados enviados à Microsoft e outros serviços.

As configurações [Linha de Base da Funcionalidade Limitada de Tráfego Restrita pelo Windows](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services) são marcadas na tabela Política de Grupo com um asterisco.

### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Limpeza de disco (incluindo o uso do assistente para Limpeza de Disco)

A limpeza de disco pode ser especialmente útil com implementações de VDI da imagem mestra. Depois que a imagem mestra é preparada, atualizada e configurada, uma das últimas tarefas a ser executada é a limpeza do disco. O assistente para Limpeza de Disco incorporado ao Windows pode ajudar a limpar a maioria das áreas potenciais de salvamentos no espaço em disco.

> [!NOTE]
> O assistente para Limpeza de Disco não está mais sendo desenvolvido. O Windows usará outros métodos para fornecer funções de limpeza de disco.



Veja a seguir algumas sugestões para diversas tarefas de limpeza de disco. Você deve testá-las antes de implementar qualquer uma delas:

1. Execute o assistente para Limpeza de Disco (elevado) após a aplicação de todas as atualizações. Inclua as categorias **Otimização de Entrega** e **Limpeza do Windows Update**. Você pode automatizar esse processo com **Cleanmgr.exe** com a opção **/SAGESET:11**. Essa opção define valores de registro que podem ser usados posteriormente para automatizar a limpeza de disco usando todas as opções disponíveis no assistente para Limpeza de Disco.

   1.  Em uma VM de teste, em uma instalação limpa, executar **Cleanmgr.exe /SAGESET:11** revela que há apenas duas opções de limpeza de disco automática habilitadas por padrão:

   - Arquivos de Programas Baixados

   - Arquivos Temporários da Internet

   2. Se você definir mais opções, ou todas as opções, essas opções serão gravadas no registro, de acordo com o valor de índice fornecido no comando anterior (**Cleanmgr.exe /SAGESET:11**). Neste exemplo, usamos o valor *11* como nosso índice para um procedimento subsequente de limpeza de disco automatizada.

   3. Depois de executar **Cleanmgr.exe /SAGESET:11**, você verá um número de categorias de opções de limpeza de disco. É possível selecionar todas as opções e, em seguida, **OK**. Você observará que o assistente para Limpeza de Disco apenas desaparece. No entanto, as configurações selecionadas são salvas no registro e podem ser invocadas executando **Cleanmgr.exe /SAGERUN:11**.

2. Limpe o armazenamento da Cópia de Sombra de Volume, se algum estiver em uso. Para isso, execute os seguintes comandos em um prompt com privilégios elevados:

   - **vssadmin list shadows**

   - **vssadmin list shadowstorage**

       Se a saída desses comandos for *Nenhum item encontrado que atenda à consulta*, isso significa que não há armazenamento de VSS em uso.

3. Limpe arquivos e logs temporários. Em um prompt de comandos com privilégios elevados, execute estes comandos:

   - **Del C:\\\*.tmp /s**

   - **Del C:\\Windows\\Temp\\.**

   - **Del %temp%\\.**

4. Exclua qualquer perfil não usado no sistema com este comando:

   **wmic path win32_UserProfile where LocalPath="c:\\\\users\\\\\<user\>" Delete**

### <a name="remove-onedrive"></a>Remover o OneDrive

Remover OneDrive envolve a remoção do pacote, desinstalação e remoção de arquivos \*.lnk. Você pode usar o seguinte código de exemplo do PowerShell para auxiliar na remoção do OneDrive da imagem:

```azurecli

Taskkill.exe /F /IM "OneDrive.exe"
Taskkill.exe /F /IM "Explorer.exe"`
    if (Test-Path "C:\\Windows\\System32\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\System32\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
    if (Test-Path "C:\\Windows\\SysWOW64\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\SysWOW64\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
Remove-Item -Path
"C:\\Windows\\ServiceProfiles\\LocalService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force
Remove-Item -Path "C:\\Windows\\ServiceProfiles\\NetworkService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force \# Remove the automatic start item for OneDrive from the default user profile registry hive
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Load HKLM\\Temp C:\\Users\\Default\\NTUSER.DAT" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Delete HKLM\\Temp\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run /v OneDriveSetup /f" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Unload HKLM\\Temp" -Wait Start-Process -FilePath C:\\Windows\\Explorer.exe -Wait
```

Para perguntas ou questões sobre as informações neste artigo, entre em contato com a equipe de contas da Microsoft, pesquise o blog de VDI da Microsoft, poste uma mensagem nos fóruns da Microsoft ou entre em contato com a Microsoft.
