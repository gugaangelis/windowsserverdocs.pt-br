---
title: Visão geral de Redirecionamento de pasta, Arquivos offline e Perfis de usuário móvel
description: Uma visão geral das tecnologias de redirecionamento de pasta, arquivos Offline e perfis de usuário móvel.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 89506d0f7445f0df230945f45a31d4f58390c5c1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812414"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Visão geral de Redirecionamento de pasta, Arquivos offline e Perfis de usuário móvel

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Este tópico discute o redirecionamento de pasta, arquivos Offline (cache do cliente ou CSC) e tecnologias de perfis de usuário móvel (também conhecido como RUP), incluindo o que há de novo e onde encontrar informações adicionais.

## <a name="technology-description"></a>Descrição da tecnologia

O Redirecionamento de Pastas e os Arquivos Offline são usados juntos para redirecionar o caminho de pastas locais (como a pasta de Documentos) para um local de rede, enquanto armazena em cache o conteúdo localmente para aumentar a velocidade e a disponibilidade. Os Perfis de Usuário Móvel são usados para redirecionar um perfil de usuário para um local de rede. Esses recursos costumavam ser conhecidos como Intellimirror.

- **Redirecionamento de pasta** permite aos usuários e administradores redirecionem o caminho de uma pasta conhecida para um novo local, manualmente ou usando a diretiva de grupo. O novo local pode ser uma pasta no computador local ou um diretório em um compartilhamento de arquivos. Os usuários interagem com os arquivos na pasta redirecionada, como se ela ainda existisse na unidade local. Por exemplo, você pode redirecionar a pasta Documentos, que normalmente é armazenada na unidade local, para um local de rede. Os arquivos na pasta estarão disponíveis para o usuário em qualquer computador da rede.
- **Arquivos off-line** disponibiliza arquivos disponíveis para um usuário de rede, mesmo se a conexão de rede para o servidor está indisponível ou lenta. Quando se trabalha online, o desempenho de acesso aos arquivos tem a velocidade da rede e do servidor. Quando se trabalha offline, os arquivos são recuperados da pasta Arquivos Offline em velocidades de acesso local. Um computador alterna para o Modo Offline quando:
  - **Sempre Offline** modo foi habilitado
  - O servidor não está disponível
  - A conexão de rede é mais lenta que um limite configurável
  - O usuário alterna manualmente para o Modo Offline usando o botão **Trabalhar offline** no Windows Explorer
- **Perfis de usuário móvel** redireciona os perfis de usuário para um compartilhamento de arquivos para que os usuários recebem o mesmo sistema operacional e configurações do aplicativo em vários computadores. Quando um usuário entra em um computador usando uma conta que está configurada com um compartilhamento de arquivos como o caminho do perfil, o perfil do usuário é baixado para o computador local e mesclado com o perfil local (se houver). Quando o usuário faz sai do computador, a cópia local do seu perfil, incluindo todas as alterações, é mesclada com a cópia do perfil no servidor. Normalmente, um administrador de rede permite que os perfis de usuário móvel em contas de domínio.

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
| Suporte ao Computador Primário | Novo | Permite que você limite o uso do Redirecionamento de Pastas, dos Perfis de Usuário Móvel ou dos dois a somente os computadores principais do usuário. |

## <a name="always-offline-mode"></a>Modo Sempre Offline

Começando com o Windows 8 e Windows Server 2012, os administradores podem configurar a experiência dos usuários de arquivos Offline para trabalhar sempre offline, mesmo quando eles estão conectados por meio de uma conexão de rede de alta velocidade. O Windows atualiza os arquivos no cache de Arquivos Offline sincronizando a cada hora em segundo plano, por padrão.

### <a name="what-value-does-always-offline-mode-add"></a>Qual valor é sempre Offline modo Adicionar?

O modo Sempre Offline oferece os seguintes benefícios:

- Os usuários experimentam um acesso mais rápido aos arquivos em pastas redirecionadas, como a pasta Documentos.
- A largura de banda da rede é reduzida, diminuindo os custos em conexões WAN onerosas ou conexões monitoradas, como uma rede móvel 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>Como o modo sempre Offline mudou as coisas?

Antes do Windows 8, Windows Server 2012, os usuários precisavam alternar entre os modos Online e Offline, dependendo da disponibilidade de rede e condições, mesmo quando o modo de vínculo lento (também conhecido como o modo de Conexão lenta) estava habilitado e configurado como um 1 milissegundo. limite de latência.

Com o modo sempre Offline, os computadores nunca alternam para o modo Online quando o **configurar o modo de vínculos lentos** configuração de diretiva de grupo está configurada e o **latência** parâmetro de limite é definido como 1 milissegundo. As alterações são sincronizadas em segundo plano a cada 120 minutos, por padrão, mas a sincronização é configurável usando a configuração de Política de Grupo do **Configure Background Sync** .

Para obter mais informações, consulte [Enable the Always Offline Mode to Provide Faster Access to Files](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronização com reconhecimento de custo

Com a sincronização com reconhecimento de custo, o Windows desabilita a sincronização em segundo plano quando o usuário está usando uma conexão de rede limitada, como uma rede móvel 4G, e o assinante está perto do limite de largura de banda ou usando roaming na rede de outro provedor.

> [!NOTE]
> Conexões de rede limitadas costumam apresentar latências de rede de ida e volta mais lentas do que o valor de latência de 35 milissegundos padrão para a transição para o modo Offline (Conexão lenta) no Windows 8, Windows Server 2019, Windows Server 2016 e Windows Server 2012. Portanto, essas conexões geralmente fazem a transição para o modo Offline (Conexão Lenta) automaticamente.

### <a name="what-value-does-cost-aware-synchronization-add"></a>O valor agregado sincronização com reconhecimento de custo?

A sincronização com reconhecimento de custo ajuda os usuários a evitarem os custos do uso elevado de dados da sincronização inesperados ao usar conexões monitoradas que têm limites de uso ou fazer roaming na rede de outro provedor.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Como a sincronização com reconhecimento de custo mudou as coisas?

Antes do Windows 8 e Windows Server 2012, os usuários que desejavam minimizar os custos ao usar arquivos Offline em conexões de rede limitadas precisavam controlar o uso de dados usando as ferramentas do provedor de rede móvel. Os usuários podiam alternar manualmente para o modo Offline quando estavam em roaming, estavam perto do limite de largura de banda ou acima do limite.

Com a sincronização com reconhecimento de custo, o Windows acompanha automaticamente os limites de uso de largura de banda e de roaming em conexões limitadas. Quando o usuário está em roaming, perto do limite de largura de banda ou acima limite, o Windows alterna para o modo Offline e impede todas as sincronizações. Os usuários ainda podem iniciar manualmente a sincronização, e os administradores podem substituir a sincronização com reconhecimento de custo para usuários específicos, como executivos.

Para obter mais informações, consulte [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Computadores primários para Redirecionamento de Pastas e Perfis de Usuário Móvel

Agora você pode designar um conjunto de computadores, chamados de computadores primários, para cada usuário de domínio, que permite que você controle os computadores que usarão redirecionamento de pasta, perfis de usuário móvel ou ambos. Designar computadores primários é um método simples e eficaz de associar os dados e as configurações do usuário a determinados computadores ou dispositivos, simplificar a supervisão do administrador, melhorar a segurança dos dados e ajudar a proteger os perfis de usuário contra danos.

### <a name="what-value-do-primary-computers-add"></a>Qual valor fazer adicionar computadores primários?

Há quatro principais benefícios em designar computadores primários para usuários:

- O administrador pode especificar quais computadores podem ser usados pelos usuários para acessar os dados e as configurações redirecionadas. Por exemplo, o administrador pode escolher para fazer roaming de dados e configurações do usuário entre o desktop e o laptop de um usuário e não fazer roaming das informações quando esse usuário faz logon em qualquer outro computador, como um computador de sala de conferência.
- Designar os computadores primários reduz o risco de segurança e privacidade em deixar dados pessoais ou corporativos residuais em computadores onde o usuário faz logon. Por exemplo, um gerente geral que fez logon no computador do funcionário para acesso temporário não deixa para trás nenhum dado pessoal ou corporativo.
- Os computadores primários permitem que o administrador reduza o risco de um perfil configurado incorretamente ou corrompido, o que poderia resultar do roaming entre diferentes sistemas configurados, como entre os computadores baseados em x86 e em x64.
- A quantidade de tempo necessária para um usuário entrar pela primeira vez em um computador não primário, como um servidor, é menor porque o perfil de usuário móvel e/ou as pastas redirecionadas do usuário não são baixadas. Os tempos de saída também são reduzidos, porque as alterações do perfil de usuário não precisam ser carregadas para o compartilhamento de arquivos.

### <a name="how-have-primary-computers-changed-things"></a>Como os computadores primários mudaram coisas?

Para limitar o download de dados particulares do usuário para os computadores principais, as tecnologias de Redirecionamento de Pastas e Perfis de Usuário Móvel executam as seguintes verificações de lógica quando um usuário faz logon em um computador:

1. O sistema operacional Windows verifica as novas configurações de Política de Grupo (**Baixar perfis móveis somente nos computadores primários** e **Redirecionar pastas somente nos computadores primários**) para determinar se o atributo **msDS-Primary-Computer** no AD DS (Serviços de Domínio Active Directory) deve influenciar a decisão de tornar o perfil do usuário móvel ou aplicar o Redirecionamento de Pasta.
2. Se a configuração da política habilitar o suporte a computadores primários, o Windows verificará se o esquema AD DS dá suporte ao atributo **msDS-Primary-Computer**. Em caso afirmativo, o Windows determinará se o computador ao qual o usuário está conectado está designado como um computador primário para o usuário da seguinte maneira:
    1. Se o computador é um dos computadores primários do usuário, o Windows aplica as configurações de Perfis de Usuário Móvel e de Redirecionamento de Pastas.
    2. Se o computador não é um dos computadores primários do usuário, o Windows carrega o perfil em cache local do usuário, quando presente, ou ele cria um novo perfil local. O Windows também remove todas pastas redirecionadas existentes de acordo com a ação de remoção que foi especificada pela configuração da Política de Grupo aplicada anteriormente, que é retida na configuração do Redirecionamento de Pastas local.

Para obter mais informações, consulte [Deploy Primary Computers for Folder Redirection and Roaming User Profiles](deploy-primary-computers.md).

## <a name="hardware-requirements"></a>Requisitos de hardware

O Redirecionamento de Pastas, os Arquivos Offline e os Perfis de Usuário Móvel exigem um computador baseado em x64 ou em x86 e não são compatíveis com o Windows em computadores baseados em ARM (WOA).

## <a name="software-requirements"></a>Requisitos de software

Para designar computadores primários, seu ambiente deve satisfazer aos seguintes requisitos:

- O esquema de serviços de domínio Active Directory (AD DS) deve ser atualizado para incluir o esquema do Windows Server 2012 e condições (instalação de um Windows Server 2012 ou posterior controlador de domínio automaticamente atualiza o esquema). Para obter mais informações sobre como atualizar o esquema do AD DS, consulte [atualizar controladores de domínio para o Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e ser ingressado no domínio do Active Directory que você está gerenciando.

## <a name="more-information"></a>Mais informações

Para obter informações adicionais relacionadas, consulte os seguintes recursos.

| Tipo de conteúdo | Referências |
| --- | --- |
| Avaliação do produto | [Suporte a operadores de informações com serviços de arquivo confiável e armazenamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Quais são as novidades em arquivos Offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 e Windows Server 2008 R2)<br>[Quais são as novidades em arquivos Offline para o Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[As alterações em arquivos Offline no Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine) |
| Implantação | [Implantar o redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](deploy-folder-redirection.md)<br>[Implementando uma solução de centralização de dados do usuário final: Redirecionamento de pasta e validação de tecnologia de arquivos Offline e implantação](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Gerenciando o guia de implantação de dados de usuário de Roaming](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guia Passo a Passo de Configuração de Novos Recursos de Arquivos Offline para Computadores com o Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Usando o redirecionamento de pasta](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implementando o redirecionamento de pasta](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Ferramentas e configurações | [Arquivos offline no MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Referência de política de grupo de arquivos offline](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Recursos da comunidade | [Fórum de armazenamento e serviços de arquivo](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Hey, Scripting Guy! Como trabalhar com o recurso de arquivos Offline no Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Hey, Scripting Guy! Como habilitar e desabilitar arquivos Offline?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Tecnologias relacionadas|[Identidade e acesso no Windows Server](../../identity/identity-and-access.md)<br>[Armazenamento no Windows Server](../storage.md)<br>[Gerenciamento de acesso e o servidor remoto](../../remote/index.md) |