---
title: Visão geral de redirecionamento de pasta, arquivos Offline e perfis de usuários móveis
description: Visão geral das tecnologias de redirecionamento de pasta, arquivos Offline e perfis de usuários móveis.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4e3cf32cd718b906f16fc09901284d8520177df8
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233492"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Visão geral de redirecionamento de pasta, arquivos Offline e perfis de usuários móveis

>Aplica-se a: 10 do Windows, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Este tópico aborda o redirecionamento de pasta, arquivos Offline (cache do cliente ou CSC) e as tecnologias de perfis de usuários móveis (também conhecidos como RUP), incluindo novidades e onde encontrar informações adicionais.

## <a name="technology-description"></a>Descrição da tecnologia

O Redirecionamento de Pastas e os Arquivos Offline são usados juntos para redirecionar o caminho de pastas locais (como a pasta de Documentos) para um local de rede, enquanto armazena em cache o conteúdo localmente para aumentar a velocidade e a disponibilidade. Os Perfis de Usuário Móvel são usados para redirecionar um perfil de usuário para um local de rede. Esses recursos usados para ser referida como Intellimirror.

- **O redirecionamento de pasta** permite aos usuários e administradores redirecionar o caminho de uma pasta conhecido para um novo local, manualmente ou usando a diretiva de grupo. O novo local pode ser uma pasta no computador local ou um diretório em um compartilhamento de arquivo. Os usuários interagem com arquivos na pasta redirecionada como se ela ainda se encontrava na unidade local. Por exemplo, você pode redirecionar a pasta de documentos, que normalmente é armazenada em uma unidade local, para um local de rede. Os arquivos na pasta, em seguida, estão disponíveis para o usuário de qualquer computador na rede.
- **Arquivos off-line** disponibiliza arquivos de rede para um usuário, mesmo se a conexão de rede para o servidor não está disponível ou lento. Quando estiver trabalhando online, o desempenho de acesso do arquivo é na velocidade da rede e servidor. Ao trabalhar offline, os arquivos são recuperados da pasta Arquivos Offline em velocidades de acesso local. Um computador alterna para o modo Offline quando:
  - Modo **Sempre Offline** foi habilitado
  - O servidor não está disponível
  - A conexão de rede for menor do que um limite configurável
  - O usuário manualmente alterna para o modo Offline usando o botão **trabalhar offline** no Windows Explorer
- **Perfis de usuários móveis** redireciona os perfis de usuário para um compartilhamento de arquivos para que os usuários recebem o mesmo sistema operacional e as configurações do aplicativo em vários computadores. Quando um usuário entrar em um computador usando uma conta que está configurada com um compartilhamento de arquivos como o caminho de perfil, o perfil do usuário é baixado para o computador local e mesclado com o perfil de local (se presente). Quando o usuário entra fora do computador, a cópia local do seu perfil, incluindo quaisquer alterações, é mesclada com a cópia do servidor do perfil. Normalmente, um administrador da rede permite Roaming perfis de usuário em contas de domínio.

## <a name="practical-applications"></a>Aplicações práticas

Os administradores podem usar o redirecionamento de pasta, arquivos Offline e perfis de usuários móveis para centralizar o armazenamento de dados do usuário e as configurações e fornecer aos usuários a capacidade de acessar seus dados enquanto offline ou em caso de uma interrupção da rede ou do servidor. Alguns aplicativos específicos incluem:

- Centralize dados de computadores cliente para tarefas administrativas, como o uso de uma ferramenta de backup baseado em servidor para fazer backup de configurações e pastas do usuário.
- Habilite usuários continuar a acessar arquivos de rede, mesmo se houver uma interrupção da rede ou do servidor.
- Otimizar o uso de largura de banda e aprimorar a experiência dos usuários nas filiais que acessam arquivos e pastas que são hospedadas por servidores corporativos localizados fora do local.
- Permitir que os usuários móveis acessar arquivos de rede enquanto estiver trabalhando offline ou em redes lentas.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

A tabela a seguir descreve algumas das principais alterações no redirecionamento de pasta, arquivos Offline e perfis de usuários móveis que estão disponíveis nesta versão.

|Recurso/funcionalidade|Novo ou atualizado?|Descrição|
|---|---|---|
|Modo sempre Offline|Novo|Fornece acesso mais rápido a arquivos e uso de largura de banda inferior sempre trabalhando offline, mesmo quando conectados por meio de uma conexão de rede de alta velocidade.|
|Sincronização de custo|Novo|Ajuda os usuários a evitar custos do uso de dados de alta da sincronização durante o uso de conexões monitorados que tenham os limites de uso ou roaming na rede do provedor de outro.|
|Suporte de computador primário|Novo|Permite que você limite o uso de redirecionamento de pasta, perfis de usuários móveis ou ambos, somente os computadores primário de um usuário.|

## <a name="always-offline-mode"></a>Modo sempre Offline

Iniciando com o Windows 8 e Windows Server 2012, os administradores podem configurar a experiência dos usuários dos arquivos Offline sempre trabalhar offline, mesmo quando eles estão conectados por meio de uma conexão de rede de alta velocidade. Arquivos no cache de arquivos off-line de atualizações do Windows sincronizando hourly em segundo plano, por padrão.

### <a name="what-value-does-always-offline-mode-add"></a>Qual valor significa sempre Offline modo Adicionar?

O modo sempre Offline fornece os seguintes benefícios:

- Os usuários experimentam acesso mais rápido a arquivos nas pastas redirecionadas, como a pasta de documentos.
- Largura de banda de rede é reduzida, diminuindo os custos em conexões de WAN caros ou monitorados, como uma rede móvel de 4 G.

### <a name="how-has-always-offline-mode-changed-things"></a>Como o modo sempre Offline mudou coisas?

Antes do Windows 8, Windows Server 2012, os usuários seriam fazer a transição entre os modos Online e Offline, dependendo da disponibilidade da rede e condições, mesmo quando o modo de vínculo lento (também conhecido como o modo de Conexão lenta) foi habilitado e definido como um 1 milissegundo limite de latência.

Com o modo sempre Offline, computadores nunca fazer a transição para o modo Online quando a configuração de **Configurar o modo de vínculo lento** de diretiva de grupo está configurada e o parâmetro de limite de **latência** é definido como 1 milissegundo. As alterações são sincronizadas em segundo plano cada 120 minutos, por padrão, mas sincronização é configurável usando a configuração de diretiva de grupo **Configurar sincronização de plano de fundo** .

Para obter mais informações, consulte [Habilitar o sempre modo Offline para fornecer mais rápido acesso aos arquivos](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronização de custo

Com a sincronização de reconhecimento de custo, Windows desabilita a sincronização de plano de fundo quando o usuário está usando uma conexão de rede monitorados, como uma rede de 4G móvel e o assinante está perto ou através de seus limites de largura de banda, ou em roaming na rede do provedor de outro.

>[!NOTE]
>Conexões de rede monitorados geralmente têm latências de ida e volta de rede que são mais lentas do que o valor de latência de 35 milissegundo padrão para a transição para o modo Offline (Conexão lenta) no Windows 8, Windows Server 2012 e Windows Server 2016. Portanto, essas conexões geralmente a transição para o modo Offline (Conexão lenta) automaticamente.

### <a name="what-value-does-cost-aware-synchronization-add"></a>Qual valor o reconhecimento de custo sincronização adicionar?

Sincronização de custo ajuda os usuários a evitar custos do uso de dados de alta inesperadamente durante o uso de conexões monitorados que tenham os limites de uso ou roaming na rede do provedor de outro.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Como a sincronização de custo mudou coisas?

Antes do Windows 8 e Windows Server 2012, os usuários que queriam minimizar taxas durante o uso de arquivos off-line em conexões de rede monitorados tinham que rastrear o uso de seus dados usando as ferramentas do provedor de rede móvel. Os usuários poderiam alternar manualmente para o modo Offline quando eles foram roaming, perto de seu limite de largura de banda ou sobre seus limites.

Com a sincronização de reconhecimento de custo, Windows rastreia automaticamente os limites de uso de largura de banda e de roaming enquanto em conexões de monitorados. Quando o usuário estiver em roaming, perto de seu limite de largura de banda, ou sobre seus limites, Windows alterna para o modo Offline e impede que toda a sincronização. Os usuários podem ainda manualmente Iniciar sincronização e administradores podem substituir a sincronização de reconhecimento de custo para usuários específicos, como executivos.

Para obter mais informações, consulte [Ativar sincronização de arquivo em segundo plano em redes monitorados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Computadores principais para o redirecionamento de pasta e perfis de usuários móveis

Agora você pode designar um conjunto de computadores, conhecidos como computadores principais, para cada usuário de domínio, que permite que você controle quais computadores usar o redirecionamento de pasta, perfis de usuários móveis ou ambos. Designar computadores principais é um método simple e eficiente para associar dados de usuário e configurações determinados computadores ou dispositivos, simplificar a supervisão do administrador, melhorar a segurança dos dados e ajudar a proteger os perfis de usuário de corrupção.

### <a name="what-value-do-primary-computers-add"></a>Qual valor adicionar computadores principais?

Há quatro principais benefícios para designar computadores primários para usuários:

- O administrador pode especificar quais usuários podem usar para acessar seus dados redirecionado e configurações de computadores. Por exemplo, o administrador pode escolher para a transmissão de dados de usuário e as configurações entre desktop e laptop de um usuário e não se movimentem as informações quando esse usuário fizer logon em qualquer outro computador, como um computador da sala de conferência.
- Designar computadores principais reduz a segurança e o risco de privacidade de deixando residuais dados pessoais ou corporativos em computadores em que o usuário tiver feito logon. Por exemplo, gerente geral que faça logon no computador de um funcionário para acesso temporário não deixar para trás quaisquer dados pessoais ou corporativos.
- Computadores principais permitem que o administrador reduzir o risco de um configurado incorretamente ou caso contrário perfil corrompido, o que possam resultar de forma diferente de roaming entre sistemas configurados, tais como entre computadores baseados em x86 e x64.
- A quantidade de tempo necessária para primeiro entrar um usuário em um computador não principal, como um servidor, é mais rápida porque o perfil de usuário móvel do usuário e/ou pastas redirecionadas não são baixadas. Tempos de saída também são reduzidos, pois as alterações no perfil de usuário não precisam ser carregados para o compartilhamento de arquivo.

### <a name="how-have-primary-computers-changed-things"></a>Como os computadores principais mudaram coisas?

Para limitar a baixando os dados particulares do usuário aos computadores principais, as tecnologias de redirecionamento de pasta e perfis de usuários móveis executam as seguintes verificações lógica quando um usuário entrar em um computador:

1. O sistema operacional Windows verifica as configurações novas de diretiva de grupo (**Download de perfis móveis principais somente em computadores** e **pastas de redirecionamento principais somente em computadores**) para determinar se o **MsDS-principal-computador** atributo ativo Serviços de domínio Directory (AD DS) deve influenciar a decisão de roaming de perfil do usuário ou aplicar o redirecionamento de pasta.
2. Se a configuração de diretiva habilita o suporte de computador primário, o Windows verifica que o esquema do AD DS suporta o atributo **MsDS-principal-computador** . Se contiver, o Windows determina se o computador que o usuário está se conectando é designado como um computador principal para o usuário da seguinte maneira:
    1. Se o computador é um dos computadores de principal do usuário, o Windows aplica as configurações de redirecionamento de pasta e perfis de usuários móveis.
    2. Se o computador não é um dos computadores de principal do usuário, Windows carrega o perfil do usuário armazenado em cache local, se houver, ou ele cria um novo perfil local. Windows também remove quaisquer pastas redirecionadas existentes de acordo com a ação de remoção que foi especificado pela configuração da diretiva de grupo aplicada anteriormente, é mantida na configuração de redirecionamento de pasta local.

Para obter mais informações, consulte [Implantar computadores principal para o redirecionamento de pasta e perfis de usuários móveis](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Requisitos de hardware

Redirecionamento de pasta, arquivos Offline e perfis de usuários móveis exigem um computador baseado em x86 ou x64, e eles não são suportados pelo Windows em computadores com base em WOA ARM.

## <a name="software-requirements"></a>Requisitos de software

Para designar computadores principais, seu ambiente deve atender aos seguintes requisitos:

- O esquema do Active Directory Domain Services (AD DS) deve ser atualizado para incluir o esquema do Windows Server 2012 e condições (instalar um controlador de domínio posterior ou o Windows Server 2012 automaticamente atualiza o esquema). Para obter mais informações sobre como atualizar o esquema do AD DS, consulte [Atualizar controladores de domínio para o Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Computadores cliente devem executar 10 do Windows, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e estar associados ao domínio do Active Directory que você está gerenciando.

## <a name="more-information"></a>Mais informações

Para obter informações adicionais relacionadas, consulte os recursos a seguir.

|Tipo de conteúdo|Referências|
|---|---|
|Avaliação do produto|[Apoiando os gestores de informações com os serviços de arquivo confiável e armazenamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[O que há de novo no arquivos Offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (O Windows 7 e Windows Server 2008 R2)<br>[O que há de novo nos arquivos Offline para Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Alterações nos arquivos Offline no Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine)|
|Implantação|[Implantar o redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](deploy-folder-redirection.md)<br>[Implementando uma solução de centralização de dados do usuário final: redirecionamento de pasta e validação de tecnologia de arquivos Offline e implantação](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Gerenciando o guia de implantação do usuário dados de Roaming](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Configurando um novo Offline arquivos de recursos do guia passo a passo de computadores Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Usando o redirecionamento de pasta](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implementando o redirecionamento de pasta](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003)|
|Ferramentas e configurações|[Arquivos offline no MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Referência de diretiva de grupo de arquivos offline](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000)|
|Recursos da comunidade|[Fórum de Armazenamento e Serviços de Arquivo](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Ei, equipe de scripts! Como trabalhar com o recurso de arquivos Offline no Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Ei, equipe de scripts! Como habilitar e desabilitar arquivos Offline?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)|
Tecnologias relacionadas|[Identidade e acesso no Windows Server](../../identity/identity-and-access.md)<br>[Armazenamento no Windows Server](../storage.md)<br>[Acesso remoto e gerenciamento de servidor](../../remote/index.md)|