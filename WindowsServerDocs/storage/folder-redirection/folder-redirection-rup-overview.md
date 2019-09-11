---
title: Visão geral de Redirecionamento de pasta, Arquivos offline e Perfis de usuário móvel
description: Uma visão geral das tecnologias de redirecionamento de pasta, Arquivos Offline e perfis de usuário de roaming.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d6c980cd9e7d96261ffe467f4d9da627e3c50102
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867239"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Visão geral de Redirecionamento de pasta, Arquivos offline e Perfis de usuário móvel

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Este tópico discute o redirecionamento de pasta, Arquivos Offline (cache do lado do cliente ou CSC) e perfis de usuário de roaming (às vezes conhecidos como RUP), incluindo as novidades e onde encontrar informações adicionais.

## <a name="technology-description"></a>Descrição da tecnologia

O Redirecionamento de Pastas e os Arquivos Offline são usados juntos para redirecionar o caminho de pastas locais (como a pasta de Documentos) para um local de rede, enquanto armazena em cache o conteúdo localmente para aumentar a velocidade e a disponibilidade. Os Perfis de Usuário Móvel são usados para redirecionar um perfil de usuário para um local de rede. Esses recursos costumavam ser conhecidos como Intellimirror.

- **O redirecionamento** de pasta permite que usuários e administradores redirecionem o caminho de uma pasta conhecida para um novo local, manualmente ou usando política de grupo. O novo local pode ser uma pasta no computador local ou um diretório em um compartilhamento de arquivos. Os usuários interagem com os arquivos na pasta redirecionada, como se ela ainda existisse na unidade local. Por exemplo, você pode redirecionar a pasta Documentos, que normalmente é armazenada na unidade local, para um local de rede. Os arquivos na pasta estarão disponíveis para o usuário em qualquer computador da rede.
- **Arquivos offline torna os**arquivos de rede disponíveis para um usuário, mesmo se a conexão de rede com o servidor estiver indisponível ou lenta.  Quando se trabalha online, o desempenho de acesso aos arquivos tem a velocidade da rede e do servidor. Quando se trabalha offline, os arquivos são recuperados da pasta Arquivos Offline em velocidades de acesso local. Um computador alterna para o Modo Offline quando:
  - O modo **sempre offline** foi habilitado
  - O servidor não está disponível
  - A conexão de rede é mais lenta que um limite configurável
  - O usuário alterna manualmente para o Modo Offline usando o botão **Trabalhar offline** no Windows Explorer
- **Perfis de usuário de roaming**redireciona perfis de usuário para um compartilhamento de arquivos para que os usuários recebam as mesmas configurações de sistema operacional e aplicativo em vários computadores.  Quando um usuário entra em um computador usando uma conta que é configurada com um compartilhamento de arquivos como o caminho do perfil, o perfil do usuário é baixado no computador local e mesclado com o perfil local (se presente). Quando o usuário faz sai do computador, a cópia local do seu perfil, incluindo todas as alterações, é mesclada com a cópia do perfil no servidor. Normalmente, um administrador de rede permite perfis de usuário móveis em contas de domínio.

## <a name="practical-applications"></a>Aplicações práticas

Os administradores podem usar o Redirecionamento de Pastas, os Arquivos Offline e os Perfis de Usuário Móvel para centralizar o armazenamento de dados e configurações do usuário e fornecer aos usuários a capacidade de acessar os dados quando estão offline ou em caso de uma interrupção do servidor ou rede. Algumas aplicações específicas incluem:

- Centralizar os dados de computadores cliente para tarefas administrativas, como usar uma ferramenta de backup baseada em servidor para fazer o backup de pastas e configurações do usuário.
- Permitir que os usuários continuem a acessar os arquivos da rede, mesmo se houver uma interrupção da rede ou servidor.
- Otimizar o uso da largura de banda e melhorar a experiência dos usuários em filiais que acessam pastas e arquivos hospedados em servidores corporativos em outro local.
- Permitir que usuários móveis acessem os arquivos da rede quando trabalham offline ou em redes lentas.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

A tabela a seguir descreve algumas das principais alterações no Redirecionamento de Pastas, nos Arquivos Offline e nos Perfis de Usuário Móvel que estão disponíveis nesta versão.

| Recurso/funcionalidade | Novo ou atualizado? | Descrição |
| --- | --- | --- |
| Modo Sempre Offline | Novo | Fornece acesso mais rápido aos arquivos e menor uso de largura de banda sempre trabalhando offline, mesmo quando está conectado por uma conexão de rede de alta velocidade. |
| Sincronização com reconhecimento de custo | Novo | Ajuda os usuários a evitar que altos custos de uso de dados da sincronização usem conexões limitadas com limites de uso ou durante o roaming na rede de outro provedor. |
| Suporte ao Computador Primário | Novo | Permite limitar o uso de redirecionamento de pasta, perfis de usuário de roaming ou ambos para os computadores primários de um usuário. |

## <a name="always-offline-mode"></a>Modo Sempre Offline

A partir do Windows 8 e do Windows Server 2012, os administradores podem configurar a experiência para os usuários de Arquivos Offline sempre trabalharem offline, mesmo quando estiverem conectados por meio de uma conexão de rede de alta velocidade. O Windows atualiza os arquivos no cache de Arquivos Offline sincronizando a cada hora em segundo plano, por padrão.

### <a name="what-value-does-always-offline-mode-add"></a>Qual valor o modo sempre offline adiciona?

O modo Sempre Offline oferece os seguintes benefícios:

- Os usuários experimentam um acesso mais rápido aos arquivos em pastas redirecionadas, como a pasta Documentos.
- A largura de banda da rede é reduzida, diminuindo os custos em conexões WAN onerosas ou conexões monitoradas, como uma rede móvel 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>Como o modo sempre offline alterou as coisas?

Antes do Windows 8, o Windows Server 2012, os usuários fariam a transição entre os modos online e offline, dependendo da disponibilidade e das condições da rede, mesmo quando o modo de link lento (também conhecido como modo de conexão lenta) estava habilitado e definido como um 1 milissegundo limite de latência.

Com o modo sempre offline, os computadores nunca se transformam no modo online quando a configuração **Configurar modo de vínculo lento** política de grupo está configurada e o parâmetro de limite de **latência** é definido como 1 milissegundo. As alterações são sincronizadas em segundo plano a cada 120 minutos, por padrão, mas a sincronização é configurável usando a configuração de Política de Grupo do **Configure Background Sync** .

Para obter mais informações, consulte [Habilitar o modo Sempre Offline para fornecer acesso mais rápido aos arquivos](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronização com reconhecimento de custo

Com a sincronização com reconhecimento de custos, o Windows desabilita a sincronização em segundo plano quando o usuário está usando uma conexão de rede limitada, como uma rede móvel 4G, e o assinante está próximo ou acima do limite de largura de banda ou está em roaming na rede de outro provedor.

> [!NOTE]
> As conexões de rede limitadas geralmente têm latências de rede de ida e volta que são mais lentas do que o valor padrão de latência de 35 milissegundo para fazer a transição para o modo offline (conexão lenta) no Windows 8, no Windows Server 2019, no Windows Server 2016 e no Windows Server 2012. Portanto, essas conexões geralmente fazem a transição para o modo Offline (Conexão Lenta) automaticamente.

### <a name="what-value-does-cost-aware-synchronization-add"></a>Qual valor a sincronização com reconhecimento de custos adiciona?

A sincronização com reconhecimento de custos ajuda os usuários a evitar custos de uso de dados altamente inesperados ao usar conexões limitadas com limites de uso ou ao fazer roaming na rede de outro provedor.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Como a sincronização com reconhecimento de custos mudou as coisas?

Antes do Windows 8 e do Windows Server 2012, os usuários que queriam minimizar as tarifas ao usar Arquivos Offline em conexões de rede limitadas tinham que controlar o uso dos dados usando as ferramentas do provedor de rede móvel. Os usuários podiam alternar manualmente para o modo Offline quando estavam em roaming, estavam perto do limite de largura de banda ou acima do limite.

Com a sincronização com reconhecimento de custos, o Windows rastreia automaticamente os limites de uso de roaming e largura de banda durante conexões limitadas. Quando o usuário está em roaming, perto do limite de largura de banda ou acima limite, o Windows alterna para o modo Offline e impede todas as sincronizações. Os usuários ainda podem iniciar manualmente a sincronização, e os administradores podem substituir a sincronização com reconhecimento de custo para usuários específicos, como executivos.

Para obter mais informações, consulte [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Computadores primários para Redirecionamento de Pastas e Perfis de Usuário Móvel

Agora você pode designar um conjunto de computadores, conhecidos como computadores primários, para cada usuário de domínio, que permite controlar quais computadores usam o redirecionamento de pasta, perfis de usuário de roaming ou ambos. Designar computadores primários é um método simples e eficaz de associar os dados e as configurações do usuário a determinados computadores ou dispositivos, simplificar a supervisão do administrador, melhorar a segurança dos dados e ajudar a proteger os perfis de usuário contra danos.

### <a name="what-value-do-primary-computers-add"></a>Que valor os computadores primários adicionam?

Há quatro principais benefícios em designar computadores primários para usuários:

- O administrador pode especificar quais computadores podem ser usados pelos usuários para acessar os dados e as configurações redirecionadas. Por exemplo, o administrador pode optar por mover os dados e as configurações do usuário entre o laptop e a área de trabalho de um usuário e não fazer roaming das informações quando esse usuário fizer logon em qualquer outro computador, como um computador da sala de conferência.
- Designar os computadores primários reduz o risco de segurança e privacidade em deixar dados pessoais ou corporativos residuais em computadores onde o usuário faz logon. Por exemplo, um gerente geral que faz logon no computador de um funcionário para acesso temporário não deixa por trás de dados pessoais ou corporativos.
- Os computadores primários permitem que o administrador reduza o risco de um perfil configurado incorretamente ou corrompido, o que poderia resultar do roaming entre diferentes sistemas configurados, como entre os computadores baseados em x86 e em x64.
- A quantidade de tempo necessária para a primeira entrada do usuário em um computador não primário, como um servidor, é mais rápida porque o perfil de usuário móvel do usuário e/ou as pastas redirecionadas não são baixadas. Os tempos de saída também são reduzidos, porque as alterações do perfil de usuário não precisam ser carregadas para o compartilhamento de arquivos.

### <a name="how-have-primary-computers-changed-things"></a>Como os computadores primários alteraram as coisas?

Para limitar o download de dados particulares do usuário para os computadores principais, as tecnologias de Redirecionamento de Pastas e Perfis de Usuário Móvel executam as seguintes verificações de lógica quando um usuário faz logon em um computador:

1. O sistema operacional Windows verifica as novas configurações de Política de Grupo (**baixar perfis móveis somente em computadores primários** e **redirecionar pastas somente em computadores primários**) para determinar se o atributo **msDS-Primary-Computer** está ativo Os serviços de domínio de diretório (AD DS) devem influenciar a decisão de mover o perfil do usuário ou aplicar o redirecionamento de pasta.
2. Se a configuração da política habilitar o suporte a computadores primários, o Windows verificará se o esquema AD DS dá suporte ao atributo **msDS-Primary-Computer**. Em caso afirmativo, o Windows determinará se o computador ao qual o usuário está conectado está designado como um computador primário para o usuário da seguinte maneira:
    1. Se o computador for um dos computadores primários do usuário, o Windows aplicará os perfis de usuário de roaming e as configurações de redirecionamento de pasta.
    2. Se o computador não for um dos computadores primários do usuário, o Windows carregará o perfil local em cache do usuário, se estiver presente, ou criará um novo perfil local. O Windows também remove todas pastas redirecionadas existentes de acordo com a ação de remoção que foi especificada pela configuração da Política de Grupo aplicada anteriormente, que é retida na configuração do Redirecionamento de Pastas local.

Para obter mais informações, consulte [implantar computadores primários para redirecionamento de pasta e perfis de usuário móvel](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Requisitos de hardware

O Redirecionamento de Pastas, os Arquivos Offline e os Perfis de Usuário Móvel exigem um computador baseado em x64 ou em x86 e não são compatíveis com o Windows em computadores baseados em ARM (WOA).

## <a name="software-requirements"></a>Requisitos de software

Para designar computadores primários, seu ambiente deve satisfazer aos seguintes requisitos:

- O esquema de Active Directory Domain Services (AD DS) deve ser atualizado para incluir o esquema e as condições do Windows Server 2012 (a instalação de um controlador de domínio do Windows Server 2012 ou posterior atualiza automaticamente o esquema). Para obter mais informações sobre como atualizar o esquema de AD DS, consulte [Atualizar controladores de domínio para o Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e ingressar no domínio de Active Directory que você está gerenciando.

## <a name="more-information"></a>Mais informações

Para obter informações adicionais relacionadas, consulte os seguintes recursos.

| Tipo de conteúdo | Referências |
| --- | --- |
| Avaliação do produto | [Suporte a operadores de informações com armazenamento e serviços de arquivos confiáveis](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[O que há de novo no arquivos offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 e Windows Server 2008 R2)<br>[O que há de novo no Arquivos Offline para Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Alterações no arquivos offline no Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine) |
| Implantação | [Implantar perfis de usuário de redirecionamento de pasta, Arquivos Offline e roaming](deploy-folder-redirection.md)<br>[Implementando uma solução de centralização de dados do usuário final: Validação e implantação de tecnologia de Arquivos Offline e redirecionamento de pasta](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Gerenciando guia de implantação de dados de usuário de roaming](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guia Passo a Passo de Configuração de Novos Recursos de Arquivos Offline para Computadores com o Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Usando o redirecionamento de pasta](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implementando o redirecionamento de pasta](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Ferramentas e configurações | [Arquivos offline no MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Referência de política de grupo de arquivos offline](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Recursos da comunidade | [Fórum de armazenamento e serviços de arquivo](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Ei, equipe de scripts! Como posso trabalhar com o recurso Arquivos Offline no Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Ei, equipe de scripts! Como habilitar e desabilitar Arquivos Offline?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Tecnologias relacionadas|[Identidade e acesso no Windows Server](../../identity/identity-and-access.md)<br>[Armazenamento no Windows Server](../storage.md)<br>[Gerenciamento de servidor e acesso remoto](../../remote/index.md) |