---
title: Visão geral de Redirecionamento de pasta, Arquivos offline e Perfis de usuário móvel
description: Uma visão geral de tecnologias de Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis.
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 75954af63c8eda759d181e5b90b8f58d26596e05
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766239"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Visão geral de Redirecionamento de pasta, Arquivos offline e Perfis de usuário móvel

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Este tópico discute as tecnologias Redirecionamento de Pastas, Arquivos Offline (armazenamento em cache no lado do cliente ou CSC) e Perfis de Usuários Móveis (também chamados RUP), abordando as novidades no e em que lugar encontrar informações adicionais.

## <a name="technology-description"></a>Descrição da tecnologia

O Redirecionamento de Pastas e os Arquivos Offline são usados juntos para redirecionar o caminho de pastas locais (como a pasta de Documentos) para um local de rede, enquanto armazena em cache o conteúdo localmente para aumentar a velocidade e a disponibilidade. Os Perfis de Usuário Móvel são usados para redirecionar um perfil de usuário para um local de rede. Esses recursos costumavam ser conhecidos como Intellimirror.

- O **Redirecionamento de Pasta** permite que usuários e administradores redirecionem o caminho de uma pasta conhecida para uma nova localização de maneira manual ou com o uso de Política de Grupo. O novo local pode ser uma pasta no computador local ou um diretório em um compartilhamento de arquivos. Os usuários interagem com os arquivos na pasta redirecionada, como se ela ainda existisse na unidade local. Por exemplo, você pode redirecionar a pasta Documentos, que normalmente é armazenada na unidade local, para um local de rede. Os arquivos na pasta estarão disponíveis para o usuário em qualquer computador da rede.
- O recurso **Arquivos Offline** disponibiliza arquivos de rede para um usuário, mesmo que a conexão de rede com o servidor não esteja disponível ou esteja lenta. Quando se trabalha online, o desempenho de acesso aos arquivos tem a velocidade da rede e do servidor. Quando se trabalha offline, os arquivos são recuperados da pasta Arquivos Offline em velocidades de acesso local. Um computador alterna para o Modo Offline quando:
  - O modo **Sempre Offline** foi habilitado
  - O servidor não está disponível
  - A conexão de rede é mais lenta que um limite configurável
  - O usuário alterna manualmente para o Modo Offline usando o botão **Trabalhar offline** no Windows Explorer
- O recurso **Perfis de Usuário Móvel** redireciona os perfis de usuário para um compartilhamento de arquivo, para que os usuários recebam as mesmas configurações de sistema operacional e de aplicativos em vários computadores. Quando um usuário entra em um computador usando uma conta que está configurada com um compartilhamento de arquivo como o caminho do perfil, o perfil do usuário é baixado para o computador local e mesclado com o perfil local (se presente). Quando o usuário faz sai do computador, a cópia local do seu perfil, incluindo todas as alterações, é mesclada com a cópia do perfil no servidor. Normalmente, um administrador de rede habilita Perfis de Usuários Móveis em contas de domínio.

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
| Sincronização com reconhecimento de custo | Novo | Ajuda os usuários a evitar os custos do uso elevado de dados da sincronização ao usar conexões monitoradas que têm limites de uso ou fazer roaming na rede de outro provedor. |
| Suporte ao Computador Primário | Novo | Permite que você limite o uso do Redirecionamento de Pastas, dos Perfis de Usuários Móveis ou dos dois a somente os computadores principais do usuário. |

## <a name="always-offline-mode"></a>Modo Sempre Offline

Do Windows 8 e do Windows Server 2012 em diante, os administradores podem configurar a experiência dos usuários com Arquivos Offline para funcionar sempre offline, mesmo que estejam em uma conexão de rede de alta velocidade. O Windows atualiza os arquivos no cache de Arquivos Offline sincronizando a cada hora em segundo plano, por padrão.

### <a name="what-value-does-always-offline-mode-add"></a>Que valor o modo Sempre Offline agrega?

O modo Sempre Offline oferece os seguintes benefícios:

- Os usuários experimentam um acesso mais rápido aos arquivos em pastas redirecionadas, como a pasta Documentos.
- A largura de banda da rede é reduzida, diminuindo os custos em conexões WAN onerosas ou conexões monitoradas, como uma rede móvel 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>Como o modo Sempre Offline mudou as coisas?

Antes do Windows 8 e do Windows Server 2012, os usuários precisavam alternar entre os modos Online e Offline, dependendo da disponibilidade e das condições da rede, mesmo quando o modo de Vínculo Lento (também chamado de modo de Conexão Lenta) estava habilitado e configurado com um limite de latência de 1 milissegundo.

Com o modo Sempre Offline, os computadores nunca alternam para o modo Online quando a configuração de Política de Grupo **Configurar modo de vínculos lentos** está definida e o parâmetro de limite de **Latência** está definido como 1 milissegundo. As alterações são sincronizadas em segundo plano a cada 120 minutos, por padrão, mas a sincronização é configurável usando a configuração de Política de Grupo do **Configure Background Sync** .

Para obter mais informações, consulte [Enable the Always Offline Mode to Provide Faster Access to Files](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronização com reconhecimento de custo

Com a sincronização com reconhecimento de custo, o Windows desabilita a sincronização em segundo plano quando o usuário está usando uma conexão de rede limitada, como uma rede móvel 4G, e o assinante está perto do limite de largura de banda ou usando roaming na rede de outro provedor.

> [!NOTE]
> As conexões de rede limitadas costumam apresentar latências de rede de viagem de ida e volta mais lentas do que o valor de latência padrão de 35 milissegundos para a transição para o modo Offline (Conexão Lenta) no Windows 8, no Windows Server 2019, no Windows Server 2016 e no Windows Server 2012. Portanto, essas conexões geralmente fazem a transição para o modo Offline (Conexão Lenta) automaticamente.

### <a name="what-value-does-cost-aware-synchronization-add"></a>Que valor a sincronização com reconhecimento de custos agrega?

A sincronização com reconhecimento de custo ajuda os usuários a evitarem os custos do uso elevado de dados da sincronização inesperados ao usar conexões monitoradas que têm limites de uso ou fazer roaming na rede de outro provedor.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Como a sincronização com reconhecimento de custos mudou as coisas?

Antes do Windows 8 e do Windows Server 2012, os usuários que queriam minimizar o valor ao usar Arquivos Offline em conexões de rede limitadas precisavam controlar o uso de dados usando ferramentas do provedor de rede móvel. Os usuários podiam alternar manualmente para o modo Offline quando estavam em roaming, estavam perto do limite de largura de banda ou acima do limite.

Com a sincronização com reconhecimento de custo, o Windows acompanha automaticamente os limites de uso de largura de banda e de roaming em conexões limitadas. Quando o usuário está em roaming, perto do limite de largura de banda ou acima limite, o Windows alterna para o modo Offline e impede todas as sincronizações. Os usuários ainda podem iniciar manualmente a sincronização, e os administradores podem substituir a sincronização com reconhecimento de custo para usuários específicos, como executivos.

Para obter mais informações, consulte [Enable Background File Synchronization on Metered Networks](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Computadores primários para Redirecionamento de Pastas e Perfis de Usuário Móvel

Agora você pode designar um conjunto de computadores, chamados de computadores primários, para cada usuário do domínio. Isso permite controlar os computadores que devem usar Redirecionamento de Pastas, Perfis de Usuários Móveis ou ambos. Designar computadores primários é um método simples e eficaz de associar os dados e as configurações do usuário a determinados computadores ou dispositivos, simplificar a supervisão do administrador, melhorar a segurança dos dados e ajudar a proteger os perfis de usuário contra danos.

### <a name="what-value-do-primary-computers-add"></a>Que valor os computadores primários agregam?

Há quatro principais benefícios em designar computadores primários para usuários:

- O administrador pode especificar quais computadores podem ser usados pelos usuários para acessar os dados e as configurações redirecionadas. Por exemplo, o administrador pode escolher para fazer com que os dados e as configurações do usuário usem perfil móvel entre o desktop e o laptop de um usuário, e não das informações quando esse usuário faz logon em qualquer outro computador, como um computador de sala de conferência.
- Designar os computadores primários reduz o risco de segurança e privacidade em deixar dados pessoais ou corporativos residuais em computadores onde o usuário faz logon. Por exemplo, um gerente geral que fez logon no computador do funcionário para acesso temporário não deixa para trás nenhum dado pessoal ou corporativo.
- Os computadores primários permitem que o administrador reduza o risco de um perfil configurado incorretamente ou corrompido, o que poderia resultar do roaming entre diferentes sistemas configurados, como entre os computadores baseados em x86 e em x64.
- A quantidade de tempo necessária para um usuário conectar-se pela primeira vez em um computador não primário, como um servidor, é menor porque o perfil de usuário móvel e/ou as pastas redirecionadas do usuário não são baixadas. Os tempos de saída também são reduzidos, porque as alterações do perfil de usuário não precisam ser carregadas para o compartilhamento de arquivos.

### <a name="how-have-primary-computers-changed-things"></a>Como os computadores primários mudaram as coisas?

Para limitar o download de dados particulares do usuário para os computadores principais, as tecnologias de Redirecionamento de Pastas e Perfis de Usuário Móvel executam as seguintes verificações de lógica quando um usuário faz logon em um computador:

1. O sistema operacional Windows verifica as novas configurações de Política de Grupo (**Baixar perfis móveis somente nos computadores primários** e **Redirecionar pastas somente nos computadores primários**) para determinar se o atributo **msDS-Primary-Computer** no AD DS (Active Directory Domain Services) deve influenciar a decisão de tornar o perfil do usuário móvel ou aplicar o Redirecionamento de Pastas.
2. Se a configuração da política habilitar o suporte a computadores primários, o Windows verificará se o esquema AD DS dá suporte ao atributo **msDS-Primary-Computer** . Em caso afirmativo, o Windows determinará se o computador ao qual o usuário está conectado está designado como um computador primário para o usuário da seguinte maneira:
    1. Se o computador é um dos computadores primários do usuário, o Windows aplica as configurações de Perfis de Usuários Móveis e de Redirecionamento de Pastas.
    2. Se o computador não é um dos computadores primários do usuário, o Windows carrega o perfil em cache local do usuário (quando presente) ou ele cria um perfil local. O Windows também remove todas pastas redirecionadas existentes de acordo com a ação de remoção que foi especificada pela configuração da Política de Grupo aplicada anteriormente, que é retida na configuração do Redirecionamento de Pastas local.

Para obter mais informações, consulte [Deploy Primary Computers for Folder Redirection and Roaming User Profiles](deploy-primary-computers.md).

## <a name="hardware-requirements"></a>Requisitos de hardware

O Redirecionamento de Pastas, os Arquivos Offline e os Perfis de Usuário Móvel exigem um computador baseado em x64 ou em x86 e não são compatíveis com o Windows em computadores baseados em ARM (WOA).

## <a name="software-requirements"></a>Requisitos de software

Para designar computadores primários, seu ambiente deve satisfazer aos seguintes requisitos:

- O esquema AD DS (Active Directory Domain Services) deve ser atualizado para incluir esquema e condições do Windows Server 2012 (a instalação de um controlador de domínio do Windows Server 2012 ou posterior atualizaria o esquema automaticamente). Para obter mais informações sobre como atualizar o esquema do AD DS, confira [Atualizar controladores de domínio para o Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Os computadores cliente devem executar o Windows 10, o Windows 8.1, o Windows 8, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 e ingressar no domínio do Active Directory que você está gerenciando.

## <a name="more-information"></a>Mais informações

Para obter informações adicionais relacionadas, consulte os seguintes recursos.

| Tipo de conteúdo | Referências |
| --- | --- |
| Avaliação do produto | [Como dar suporte a Profissionais da Informação com Serviços e Armazenamento de Arquivo Confiáveis](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Novidades em Arquivos Offline](</previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 e Windows Server 2008 R2)<br>[Novidades em Arquivos Offline para o Windows Vista](</previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Alterações a Arquivos Offline no Windows Vista](/previous-versions/technet-magazine/cc137753(v=msdn.10)) (TechNet Magazine) |
| Implantação | [Implantar Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis](deploy-folder-redirection.md)<br>[Como implementar uma solução de centralização de dados do usuário final: validação e implantação da tecnologia de Arquivos Offline e Redirecionamento de Pastas](https://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Guia de implantação de como gerenciar a implantação de dados de usuário de roaming](</previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guia Passo a Passo de Configuração de Novos Recursos de Arquivos Offline para Computadores com o Windows 7](</previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Como implantar o Redirecionamento de Pastas](</previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Como implementar o Redirecionamento de Pastas](</previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Ferramentas e configurações | [Arquivos Offline no MSDN](/previous-versions/windows/desktop/offlinefiles/offline-files-portal)<br>[Referência de Política de Grupo de Arquivos Offline](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Recursos da comunidade | [Fórum de Armazenamento e Serviços de Arquivo](/answers/topics/windows-server-storage.html)<br>[Hey, Scripting Guy! Como trabalhar com o recurso Arquivos Offline no Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Hey, Scripting Guy! Como habilitar e desabilitar Arquivos Offline?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Tecnologias relacionadas|[Identidade e Acesso no Windows Server](../../identity/identity-and-access.yml)<br>[Armazenamento no Windows Server](../storage.yml)<br>[Acesso remoto e gerenciamento de servidor](../../remote/index.yml) |