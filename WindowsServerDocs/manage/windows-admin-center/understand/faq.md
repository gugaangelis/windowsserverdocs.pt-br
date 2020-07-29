---
title: Perguntas frequentes sobre o Windows Admin Center
description: Obtenha respostas sobre Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 12/02/2019
ms.prod: windows-server
ms.openlocfilehash: 4125a3f427bd19ae7461aaaef058a558722d1987
ms.sourcegitcommit: b35fbd2a67d7a3395b50b2a3acd0817ba4e36b26
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86891371"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Perguntas frequentes sobre o Windows Admin Center

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Veja as respostas para as perguntas mais frequentes sobre o Windows Admin Center.

## <a name="what-is-windows-admin-center"></a>O que é o Windows Admin Center?

O Windows Admin Center é uma plataforma de GUI e conjunto de ferramentas leve, com base em navegador, para administradores de TI gerenciarem o Windows Server e o Windows 10. É a evolução das ferramentas administrativas familiares nativas, tais como o Gerenciador do Servidor e o MMC (Console de Gerenciamento Microsoft) em uma experiência de modernizada, simplificada, integrada e segura.

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>Posso usar o Windows Admin Center em ambientes de produção?

Sim. Em geral, o Windows Admin Center está em disponibilidade geral e pronto para implantações de produção e de uso amplas. As funcionalidades e ferramentas principais atuais da plataforma atendem aos critérios de versão padrão da Microsoft e à nossa meta de qualidade para usabilidade, confiabilidade, desempenho, acessibilidade, segurança e adoção.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Quanto custa usar o Windows Admin Center?

O Windows Admin Center não tem custo adicional além do Windows. É possível usar o Windows Admin Center (disponível como um download separado) com licenças válidas do Windows Server ou Windows 10 sem custo adicional – ele é licenciado sob um EULA complementar do Windows.

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>Quais versões do Windows Server posso gerenciar com o Windows Admin Center?

O Windows Admin Center é otimizado para o Windows Server 2019 para permitir o uso dos temas principais na versão do Windows Server 2019: cenários de nuvem híbrida e gerenciamento da infraestrutura hiperconvergente em particular. Embora o Windows Admin Center funcione melhor com o Windows Server 2019, ele dá suporte para gerenciar diversas de versões que os clientes já usam: O Windows Server 2012 e versões mais recente são totalmente compatíveis. Também há uma funcionalidade limitada para gerenciar o Windows Server 2008 R2.

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>O Windows Admin Center substitui completamente todas as ferramentas nativas tradicionais e as Ferramentas de Administração de Servidor Remoto?

Não. Embora o Windows Admin Center possa gerenciar muitos cenários comuns, ele não substitui completamente todas as ferramentas tradicionais do MMC (Console de Gerenciamento Microsoft). Para obter uma visão detalhada sobre quais ferramentas estão incluídas no Windows Admin Center, leia mais sobre como [gerenciar servidores](../use/manage-servers.md) em nossa documentação. O Windows Admin Center tem os seguintes recursos principais em sua solução de Gerenciador do Servidor:

* Exibir recursos e a utilização de recursos
* Gerenciamento de certificado
* Gerenciamento de dispositivos
* Visualizador de Eventos
* Explorador de Arquivos
* Gerenciamento de firewall
* Gerenciamento de aplicativos instalados
* Configurar usuários e grupos locais
* Configurações de rede
* Exibir/encerrar processos e criar despejos de processo
* Edição do Registro
* Gerenciar tarefas agendadas
* Gerenciar serviços do Windows
* Habilitar/desabilitar funções e recursos
* Gerenciar VMs Hyper-V e switches virtuais
* Gerenciar o armazenamento
* Gerenciar a Réplica de Armazenamento
* Gerenciar atualizações do Windows
* Console do PowerShell
* Conexão de Área de Trabalho Remota

O Windows Admin Center também fornece estas soluções:

* Gerenciamento do computador: fornece um subconjunto dos recursos do Gerenciador do Servidor para gerenciar computadores cliente com Windows 10
* Gerenciador de Cluster de Failover: dá suporte ao gerenciamento contínuo de clusters de failover e recursos de cluster
* Gerenciador de cluster de hiperconvergência: fornece uma nova experiência totalmente adaptada para espaços de armazenamento diretos e Hyper-V. Contém recursos do Painel e destaca gráficos e alertas para monitoramento.

O Windows Admin Center é um complemento e não substitui o RSAT (Ferramentas de Administração de Servidor Remoto), já que funções como Active Directory, DHCP, DNS e IIS ainda não têm funcionalidades de gerenciamento equivalentes reveladas no Windows Admin Center.

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>O Windows Admin Center pode ser usado para gerenciar o Microsoft Hyper-V Server gratuito?

Sim. O Windows Admin Center pode ser usado para gerenciar o Microsoft Hyper-V Server 2016 e o Microsoft Hyper-V Server 2012 R2.

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>Posso implantar o Windows Admin Center em um computador Windows 10?

Sim, o Windows Admin Center pode ser instalado no Windows 10 (versão 1709 ou posterior) em execução no modo de desktop.  O Windows Admin Center também pode ser instalado em um servidor com o Windows Server 2016 ou posterior no modo de gateway e, em seguida, acessá-lo por meio de um navegador da Web de um computador Windows 10. [Saiba mais sobre as opções de instalação](../plan/installation-options.md).

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>Ouvi dizer que usa o Windows Admin Center usa o PowerShell nos bastidores, posso ver os scripts reais que ele usa?

Sim! o [recurso Showscript](../use/get-started.md#view-powershell-scripts-used-in-windows-admin-center) foi adicionado no Windows Admin Center Preview 1806 e agora está incluído no canal de disponibilidade geral.

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>Há planos para o Windows Admin Center gerenciar o Windows Server 2008 R2 ou anterior?

O Windows Admin Center **não é mais compatível** com a funcionalidade para gerenciar o Windows Server 2008 R2. O Windows Admin Center depende das funcionalidades e das tecnologias de plataforma do PowerShell que não existem no Windows Server 2008 R2 e anteriores, tornando o suporte completo impraticável. Se você ainda não fez isso, a Microsoft recomenda [migrar para o Azure ou atualizar para a última versão do Windows Server](https://www.microsoft.com/cloud-platform/windows-server-2008).

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>Há planos para o Windows Admin Center gerenciar conexões do Linux?

Estamos investigando devido à demanda dos clientes, mas, no momento, não há nenhum plano para disponibilizar o recurso e o suporte pode consistir em somente uma conexão de console por SSH.

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>Quais navegadores da Web são compatíveis com o Windows Admin Center?

As versões mais recentes do Microsoft Edge (Windows 10, versão 1709 ou posterior), Google Chrome e [Microsoft Edge Insider](https://microsoftedgeinsider.com) são testadas e compatíveis com o Windows 10. [Exiba problemas conhecidos específicos do navegador](../support/known-issues.md#browser-specific-issues). Outros navegadores da Web modernos ou outras plataformas atualmente não fazem parte da nossa matriz de teste e, portanto, não são *oficialmente* compatíveis.

## <a name="how-does-windows-admin-center-handle-security"></a>Como o Windows Admin Center lida com a segurança?

O tráfego do navegador para o gateway do Windows Admin Center usa HTTPS. O tráfego do gateway para servidores gerenciados é do PowerShell padrão e WMI por WinRM. Oferecemos suporte a LAPS (Solução de Senha de Administrador Local) delegação restrita com base em recurso, controle de acesso do gateway usando o AD ou o Azure AD e, por fim, controle de acesso com base em função para gerenciar os servidores de destino.

## <a name="does-windows-admin-center-use-credssp"></a>O Windows Admin Center usa CredSSP?

Sim, em alguns casos o Windows Admin Center requer CredSSP. Isso é necessário passar suas credenciais para autenticação para computadores além do servidor específico que você está direcionando para gerenciamento. Por exemplo, se você estiver gerenciando máquinas virtuais no **servidor B**, mas desejar armazenar os arquivos vhdx dessas máquinas virtuais em um compartilhamento de arquivo hospedado pelo **servidor C**, o Windows Admin Center precisará usar o CredSSP para autenticar com o **servidor C** a fim de acessar o compartilhamento de arquivo.

O Windows Admin Center lida com a configuração do CredSSP automaticamente depois de solicitar seu consentimento. Antes de configurar o CredSSP, o Windows Admin Center verificará para garantir que o sistema tenha as [atualizações](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018) recentes do CredSSP. 

O CredSSP é usado atualmente nas seguintes áreas:

- Uso de armazenamento SMB desagregado na ferramenta de máquinas virtuais (o exemplo acima).
- Uso da ferramenta Atualizações nas soluções de gerenciamento de cluster de failover ou hiperconvergente, que executa [Atualização com Suporte a Cluster](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## <a name="are-there-any-cloud-dependencies"></a>Existem dependências de nuvem?

O Windows Admin Center não requer acesso à Internet e não exige o Microsoft Azure. O Windows Admin Center gerencia o Windows Server e instâncias do Windows em qualquer lugar: em sistemas físicos ou em máquinas virtuais em qualquer hipervisor ou executando em qualquer nuvem. Embora a integração com diversos serviços do Azure seja adicionada ao longo do tempo, esses serão recursos de valor agregado opcionais, não um requisito para usar o Windows Admin Center.

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>Há outras dependências ou pré-requisitos?

O Windows Admin Center pode ser instalado no Windows 10 Fall Anniversary Update (1709) ou mais recente ou no Windows Server 2016 ou mais recente. Para gerenciar o Windows Server 2008 R2, 2012 ou 2012 R2, é necessário instalar o Windows Management Framework 5.1 nesses servidores. Não há nenhuma outra dependência. O IIS não é necessário, os operadores não são necessários, o SQL Server não é necessário.

## <a name="what-about-extensibility-and-3rd-party-support"></a>E quanto à extensibilidade e suporte de terceiros?

O Windows Admin Center tem um SDK disponível para que qualquer pessoa possa escrever sua própria extensão. Como uma plataforma, nossa prioridade desde o início foi ampliar o ecossistema e permitir a extensibilidade de parceiros. [Leia mais sobre o SDK do Windows Admin Center](../extend/extensibility-overview.md).

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Posso gerenciar a infraestrutura hiperconvergente com o Windows Admin Center?

Sim. O Windows Admin Center dá suporte para o gerenciamento de clusters hiperconvergentes executando o Windows Server 2016 ou o Windows Server 2019. A solução de gerenciador de cluster hiperconvergente no Windows Admin Center estava anteriormente no modo de versão prévia, mas agora está em **disponibilidade geral**, com algumas novas funcionalidades em versão prévia. Para obter mais informações, [leia mais sobre o gerenciamento de infraestrutura hiperconvergente](../use/manage-hyper-converged.md).

## <a name="does-windows-admin-center-require-system-center"></a>O Windows Admin Center requer o System Center?

Não. O Windows Admin Center é um complemento do System Center, mas o System Center não é necessário. [Leia mais sobre o Windows Admin Center e o System Center](related-management.md#system-center).

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>O Windows Admin Center pode substituir o SCVMM (System Center Virtual Machine Manager)?

O Windows Admin Center e o SCVMM são complementares; o Windows Admin Center se destina a substituir os tradicionais snap-ins do MMC (Console de Gerenciamento Microsoft) e a experiência de administração do servidor.  O Windows Admin Center não é destinado a substituir os aspectos de monitoramento do SCVMM. [Leia mais sobre o Windows Admin Center e o System Center](related-management.md#system-center).

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>O que é o Windows Admin Center Preview e qual versão é a certa para mim?

Há duas versões do Windows Admin Center disponíveis para download:

### <a name="windows-admin-center"></a>Windows Admin Center

* Para administradores de TI que não podem atualizar com frequência ou para quem deseja mais tempo de validação para os lançamentos usados na produção, esta versão é para você. Nossa versão atual em GA (disponibilidade geral) é o Windows Admin Center 1910.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Para obter a versão mais recente, [baixe-a aqui](https://aka.ms/WACDownload).

### <a name="windows-admin-center-preview"></a>Windows Admin Center Preview

* Para administradores de TI que desejam os melhores e mais recentes recursos em uma cadência regular, esta versão é para você. Nosso objetivo é fornecer lançamentos de atualização subsequentes todos os meses ou com frequência próxima disso. A plataforma principal continua pronta para produção e a licença fornece os direitos de uso de produção. No entanto, observe que você vê a introdução das novas ferramentas e recursos que estão marcados claramente como VERSÃO PRÉVIA e são adequados para avaliação e teste.
* Para obter a versão mais recente do Insider Preview, os participantes registrados do Insider podem baixar o Windows Admin Center Preview diretamente da [página de download do Windows Server Insider Preview](https://microsoft.com/en-us/software-download/windowsinsiderpreviewserver), no menu suspenso Downloads Adicionais. Caso ainda não tenha se registrado como um Insider, confira o [Guia de Introdução ao Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) no portal Windows Insider para Empresas.

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>Por que o "Windows Admin Center" foi escolhido como o nome final do "Project Honolulu"?

O Windows Admin Center é o nome oficial do produto para o "Project Honolulu" e reforça nossa visão de uma experiência integrada para administradores de TI em uma ampla variedade de cenários principais administrativos e de gerenciamento. Ele também realça o foco no cliente e nas necessidades do usuário administrador de TI como elemento central para o que investimos e o que podemos fornecer.

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>Onde posso saber mais sobre o Windows Admin Center ou obter mais detalhes sobre os tópicos acima?

A [página de início](https://aka.ms/WindowsAdminCenter) é o melhor ponto de partida e tem links para o conteúdo de documentação recentemente categorizada, localização de download, como fornecer comentários, informações de referência e outros recursos.

## <a name="what-is-the-version-history-of-windows-admin-center"></a>O que é o histórico de versão do Windows Admin Center?

[Exiba o histórico de versão aqui.](../support/release-history.md)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>Estou tendo um problema com o Windows Admin Center, onde posso obter ajuda?

Confira nosso [guia de solução de problemas](../use/troubleshooting.md) e nossa lista de [problemas conhecidos](../use/known-issues.md).
