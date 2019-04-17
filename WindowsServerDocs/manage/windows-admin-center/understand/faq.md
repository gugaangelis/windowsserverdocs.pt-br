---
title: Perguntas frequentes sobre o Windows Admin Center
description: Obtenha respostas sobre Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 04/12/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: 2f1591a32147e3c11ba635f1a11d36b7f38a3470
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296738"
---
# Perguntas frequentes sobre o Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

Veja as respostas para as perguntas mais frequentes sobre o Windows Admin Center.

## O que é o Windows Admin Center?

O Windows Admin Center é uma plataforma de GUI e conjunto de ferramentas leve, com base em navegador, para administradores de TI gerenciarem o Windows Server e o Windows 10. É a evolução das ferramentas administrativas familiares do caixa de entrada, como o Gerenciador do Servidor e o Console de Gerenciamento Microsoft (MMC) em uma experiência de modernizada, simplificada, integrada e segura.

## Posso usar o Windows Admin Center em ambientes de produção?

Sim. Em geral, o Windows Admin Center está disponível e pronto para implantações de produção e uso amplas. Os recursos atuais de plataforma e ferramentas principais atendem aos critérios de versão padrão da Microsoft e a nossa meta de qualidade para usabilidade, confiabilidade, desempenho, acessibilidade, segurança e adoção.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## Quanto custa para usar o Windows Admin Center?

O Windows Admin Center não tem custo adicional além do Windows. Você pode usar o Windows Admin Center (disponível como um download separado) com licenças válidas do Windows Server ou Windows 10 sem custo adicional - ele é licenciado sob um EULA complementar do Windows.

## Quais versões do Windows Server posso gerenciar com o Windows Admin Center?

Windows Admin Center é otimizado para o Windows Server 2019 habilitar temas principais na versão do Windows Server 2019: nuvem híbrida cenários e gerenciamento da infraestrutura hiperconvergente em particular. Embora o Windows Admin Center funcione melhor com o Windows Server 2019, ele oferece suporte para gerenciar uma variedade de versões que os clientes já usam: Windows Server 2012 e mais recente são totalmente compatíveis. Também há funcionalidade limitada para gerenciar o Windows Server 2008 R2.

## O Windows Admin Center substitui completamente todas as ferramentas tradicionais e RSAT?

Não. Embora o Windows Admin Center possa gerenciar muitos cenários comuns, ele não substitui completamente todas as ferramentas do Console de Gerenciamento Microsoft (MMC) tradicional. Para obter uma visão detalhada quais ferramentas estão incluídas no Windows Admin Center, leia mais sobre como [Gerenciar servidores](..\use\manage-servers.md) em nossa documentação. O Windows Admin Center tem os seguintes recursos principais em sua solução de Gerenciador do Servidor:

* Exibir os recursos e a utilização de recursos
* Gerenciamento de certificados
* Gerenciamento de dispositivos
* Visualizador de Eventos
* Explorador de Arquivos
* Gerenciamento de firewall
* Gerenciamento de aplicativos instalados
* Configurar usuários e grupos locais
* Configurações de rede
* Visualizar/encerrar processos e criar dumps do processo
* Edição do registro
* Gerenciando tarefas agendadas
* Gerenciar serviços Windows
* Habilitar/desabilitar funções e recursos
* Gerenciar VMs Hyper-V e switches virtuais
* Gerenciamento de armazenamento
* Gerenciamento de réplica de armazenamento
* Gerenciamento de atualizações do Windows
* Console do PowerShell
* Conexão de área de trabalho remota

O Windows Admin Center também fornece estas soluções:

* Gerenciamento do computador: fornece um subconjunto dos recursos do Gerenciador do Servidor para gerenciar computadores de cliente Windows 10
* Gerenciador de cluster de failover: fornece suporte para o gerenciamento contínuo de clusters de failover e recursos de cluster
* Gerenciador de cluster de hiperconvergência: fornece uma nova experiência totalmente adaptada para espaços de armazenamento direto e Hyper-V. Contém recursos do Painel e destaca gráficos e alertas para monitoramento.

Windows Admin Center é um complemento e não substitui RSAT (ferramentas de administração de servidor remoto) desde funções como o Active Directory, DHCP, DNS, IIS ainda não tem os recursos de gerenciamento equivalente surgem no Windows Admin Center.

## O Windows Admin Center pode ser usado para gerenciar o Microsoft Hyper-V Server gratuito?

Sim. O Windows Admin Center pode ser usado para gerenciar o Microsoft Hyper-V Server 2016 e o Microsoft Hyper-V Server 2012 R2.

## Posso implantar o Windows Admin Center em um computador Windows 10?

Sim, o Windows Admin Center pode ser instalado no Windows 10 (versão 1709 ou posterior) em execução no modo de desktop.  Windows Admin Center também pode ser instalado em um servidor com o Windows Server 2016 ou maior no modo de gateway e, em seguida, acessado por meio de um navegador da web em um computador Windows 10. [Saiba mais sobre as opções de instalação](..\plan\installation-options.md).

## Ouvi que usa o Windows Admin Center PowerShell nos bastidores, posso ver os scripts reais que ele usa?

Sim! o [recurso de Showscript](..\use\get-started.md#view-powershell-scripts-used-in-windows-admin-center) foi adicionada ao Windows Admin Center Preview 1806 e agora está incluído no canal GA.

## Há planos para o Windows Admin Center gerenciar o Windows Server 2008 R2 ou anterior?

Windows Admin Center agora dá suporte à funcionalidade **limitada** para gerenciar o Windows Server 2008 R2. O Windows Admin Center depende dos recursos e das tecnologias de plataforma do PowerShell que não existem no Windows Server 2008 R2 e anteriores, tornando o suporte completo impraticável. Windows Server 2008/2008 R2 estão se aproximando do fim do suporte em janeiro de 2020 para que a Microsoft recomenda que os clientes [Mover para o Azure ou atualizar para a versão mais recente do Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## Há planos para o Windows Admin Center gerenciar conexões do Linux?

Estamos investigando devido à demanda dos clientes, mas atualmente, não há nenhum plano para entregar e suporte pode consistir em somente uma conexão de console por SSH.

## Quais navegadores da Web são suportados pelo Windows Admin Center?

As versões mais recentes do Microsoft Edge (Windows 10, versão 1709 ou posterior) e navegadores Google Chrome são testadas e suportadas no Windows 10. [Problemas conhecidos do modo de exibição navegador específicos](..\support\known-issues.md#browser-specific-issues). Outros navegadores da web modernos ou outras plataformas atualmente não fazem parte da nossa matriz de teste e, portanto, não estão *oficialmente* compatíveis.

## Como o Windows Admin Center processa a segurança?

O tráfego do navegador para o gateway do Windows Admin Center usa HTTPS. O tráfego do gateway para servidores gerenciados é do PowerShell padrão e WMI por WinRM. Oferecemos suporte a LAPS (Solução de Senha de Administrador Local) com base no recurso de delegação restrita, controle de acesso do gateway usando AD ou Azure AD e controle de acesso com base em função para gerenciar os servidores de destino.

## Windows Admin Center usar CredSSP?

Sim, em alguns casos, o Windows Admin Center requer CredSSP. Isso é necessário para transmitir suas credenciais para autenticação para computadores além do servidor específico que você está direcionando para gerenciamento. Por exemplo, se você estiver gerenciando máquinas virtuais no **servidor B**, mas deseja armazenar os arquivos de vhdx para as máquinas virtuais em um compartilhamento de arquivo hospedado por **servidor C**, o Windows Admin Center deve usar CredSSP para autenticar com o **servidor C** para acessar o compartilhamento de arquivos.

Windows Admin Center trata a configuração do CredSSP automaticamente depois de solicitar consentimento de você. Antes de configurar CredSSP, o Windows Admin Center verificará para certificar-se de que o sistema tem o CredSSP recentes [atualizações](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018). Enquanto CredSSP estiver habilitada, haverá um selo na visão geral do servidor e uma opção para desabilitá-lo-

![CredSSP na visão geral do servidor](../media/CredSSP-overview.png)

CredSSP atualmente é usado nas seguintes áreas:

- Usando desagregada armazenamento SMB na ferramenta de máquinas virtuais (no exemplo acima.)
- Usando as atualizações de ferramenta no cluster de Hiperconvergência ou Failover soluções de gerenciamento, que executa a [Atualização do Cluster-Aware](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## Existem dependências de nuvem?

O Windows Admin Center não requer acesso à Internet e não exige o Microsoft Azure. O Windows Admin Center gerencia o Windows Server e instâncias do Windows em qualquer lugar: em sistemas físicos ou em máquinas virtuais em qualquer hipervisor, ou executando em qualquer nuvem. Embora a integração com diversos serviços do Azure seja adicionada ao longo do tempo, esses serão os recursos de valor agregados opcionais e não um requisito para usar o Windows Admin Center.

## Há outras dependências ou pré-requisitos?

O Windows Admin Center pode ser instalado no Windows 10 Fall Anniversary Update (1709) ou mais recente, ou no Windows Server 2016 ou mais recente. Para gerenciar o Windows Server 2008 R2, 2012 ou 2012 R2, é necessário instalar o Windows Management Framework 5.1 nesses servidores. Não há nenhuma outra dependência. O IIS não é necessário, os operadores não são necessários, o SQL Server não é necessário.

## E quanto a extensibilidade e suporte de terceiros 3rd?

Windows Admin Center tem um SDK disponível para que qualquer pessoa pode gravar seu próprios extensão. Como uma plataforma, nossa prioridade foi a ampliação do ecossistema e permitir a extensibilidade de parceiro. [Leia mais sobre o SDK do Windows Admin Center](..\extend\extensibility-overview.md).

## Pode gerenciar a infraestrutura hiperconvergente com o Windows Admin Center?

Sim. Windows Admin Center oferece suporte o gerenciamento de clusters hiperconvergentes executando o Windows Server 2016 ou Windows Server 2019. A solução de Gerenciador de cluster hiperconvergente no Windows Admin Center foi anteriormente na visualização, mas agora está **disponível em geral**, com algumas novas funcionalidades na visualização. Para obter mais informações, [Leia mais sobre o gerenciamento da infraestrutura hiperconvergente](..\use\manage-hyper-converged.md).

## O Windows Admin Center requer o System Center?

Não. O Windows Admin Center é um complemento do System Center, mas o System Center não é necessário. [Leia mais sobre o Windows Admin Center e o System Center](related-management.md#system-center).

## O Windows Admin Center pode substituir o System Center Virtual Machine Manager (SCVMM)?

O Windows Admin Center e o SCVMM são complementares; o Windows Admin Center se destina a substituir os tradicionais snap-ins do Console de Gerenciamento Microsoft (MMC) e a experiência de administração do servidor.  O Windows Admin Center não substituirá os aspectos de monitoramento do SCVMM. [Leia mais sobre o Windows Admin Center e o System Center](related-management.md#system-center).

## O que é o Windows Admin Center Preview e qual versão é a certa para mim?

Há duas versões do Windows Admin Center disponíveis para download:

### WindowsAdmin Center

* Para administradores de TI que não podem atualizar com frequência ou para quem deseja mais tempo de validação para os lançamentos usados na produção, esta versão é para você. Nossa versão (GA) geralmente disponível atual é Windows Admin Center 1904.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Para obter a versão mais recente, [Baixe aqui](https://aka.ms/WACDownload).

### Windows Admin Center Preview

>[!NOTE]
>A versão atual do GA (Windows Admin Center 1904) contém todas as funcionalidades de visualização anterior.
>A visualização de Insider retornará nos próximos meses.

* Para administradores de TI que desejam os melhores e mais recentes recursos em um cadência regular, esta versão é para você. Nosso objetivo é fornecer a atualização subsequente libera cada mês mais ou menos. A plataforma de núcleo continua a estar pronto para produção e a licença fornece os direitos de uso de produção. No entanto, observe que você verá a introdução das novas ferramentas e recursos que estão marcados claramente como visualização e são adequados para avaliação e teste.
* Para obter a versão mais recente do Insider Preview, participantes registrados podem baixar o Windows Admin Center Preview diretamente da [Página de download do Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), sob o menu suspenso Downloads adicionais. Se você ainda não se registrou como um Insider, consulte o [Guia de Introdução ao Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) no portal Windows Insider para Empresas.

## Por que o "Windows Admin Center" foi escolhido como o nome final "Project Honolulu"?

O Windows Admin Center é o nome oficial do produto para "Project Honolulu" e reforçam nossa visão de uma experiência integrada para administradores de TI em uma ampla variedade de cenários centrais administrativos e de gerenciamento. Também destaca o foco no cliente para as necessidades do usuário administrativo da TI como elemento central do que investimos e o que podemos fornecer.

## Onde posso saber mais sobre o Windows Admin Center ou obter mais detalhes sobre os tópicos acima?

A [página de início](https://aka.ms/WindowsAdminCenter) é o melhor ponto de partida e tem links para o conteúdo de documentação recentemente categorizada, local de download, como fornecer comentários, informações de referência e outros recursos.

## Qual é o histórico de versão do Windows Admin Center?

[Exiba o histórico de versão.](..\overview.md#release-history)

## Estou tendo um problema com o Windows Admin Center, onde posso obter ajuda?

Consulte nosso [guia de solução de problemas](..\use\troubleshooting.md) e nossa lista de [problemas conhecidos](..\use\known-issues.md).
