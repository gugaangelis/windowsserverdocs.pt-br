---
title: Visão geral do Windows Admin Center
description: Saiba como gerenciar o Windows Server usando o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: 2314e336cbf9ad44b07f3f94d7a866b48b5e9bff
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811812"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> Aplica-se a: Windows Admin Center, Windows Admin Center Preview

**Windows Admin Center** (anteriormente conhecido pelo codinome **projeto Paulo**) é uma evolução do Windows Server na caixa de ferramentas de gerenciamento; é um único painel que consolida todos os aspectos do gerenciamento de servidores locais e remotos. Como uma experiência de gerenciamento com base em navegador implantado localmente, isso não requer uma conexão de Internet e o Azure. O Windows Admin Center oferece controle total sobre todos os aspectos da sua implantação, incluindo as redes privadas que não estejam conectadas à Internet.

## <a name="introduction"></a>Introdução

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Infográfico do Windows Admin Center](media/WAC1809Poster_thumb.PNG)

[Baixe o PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## <a name="quick-start"></a>Início rápido

Você pode obter o Windows Admin Center para cima e em execução em seu ambiente, em minutos:

1. [Baixar](https://aka.ms/windowsadmincenter)
2. [Instalar](deploy/install.md)
3. [Introdução](use/get-started.md)

## <a name="contents-at-a-glance"></a>Conteúdo em um relance

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Entender</h3>
            <ul>
            <li><a href="understand/what-is.md">O que é Windows Admin Center?</a>
            <li><a href="understand/faq.md">FAQ</a>
            <li><a href="understand/case-studies.md">Estudos de caso</a>
            <li><a href="understand/related-management.md">Produtos de gerenciamento relacionados</a>
            <li><a href="understand/videos.md">Vídeos</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Planeje</h3>
            <ul>
            <li><a href="plan/installation-options.md">Que tipo de instalação é ideal para você?</a>
            <li><a href="plan/user-access-options.md">Opções de acesso do usuário</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Implantar</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Preparar o ambiente</a>
            <li><a href="deploy/install.md">Instalar Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Habilitar a alta disponibilidade</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configurar</h3>
            <ul>
            <li><a href="configure/settings.md">Configurações do Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Permissões e controle de acesso do usuário</a>
            <li><a href="configure/shared-connections.md">Conexões compartilhadas</a>
            <li><a href="configure/using-extensions.md">Extensões</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Uso</h3>
            <ul>
            <li><a href="use/get-started.md">Iniciar & Adicionar conexões</a>
            <li><a href="use/manage-servers.md">Gerenciar servidores</a>
            <li><a href="use/manage-hyper-converged.md">Gerenciar a infraestrutura hiperconvergente</a>
            <li><a href="use/manage-failover-clusters.md">Gerenciar clusters de failover</a>
            <li><a href="use/manage-virtual-machines.md">Gerenciar máquinas virtuais</a>
            <li><a href="use/logging.md">Registro em log</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Conecte-se ao Azure</h3>
            <ul>
            <li><a href="azure/index.md">Serviços híbridos do Azure</a></li>
            <li><a href="azure/azure-integration.md">Conectar-se o Windows Admin Center do Azure</a></li>
            <li><a href="azure/deploy-wac-in-azure.md">Implantar o Windows Admin Center no Azure</a></li>
            <li><a href="azure/manage-azure-vms.md">Gerenciar VMs do Azure com o Windows Admin Center</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>Suporte</h3>
            <ul>
            <li><a href="support/index.md">Política de suporte</a>
            <li><a href="support/troubleshooting.md">Etapas de solução de problemas comuns</a>
            <li><a href="support/known-issues.md">Problemas conhecidos</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>Extend</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">Visão geral das extensões</a>
            <li><a href="extend/understand-extensions.md">Extensões de Noções básicas sobre</a>
            <li><a href="extend/developing-extensions.md">Desenvolver uma extensão</a>
            <li><a href="extend/publish-extensions.md">Guias</a>
            <li><a href="extend/publish-extensions.md">Extensões de publicação</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="release-history"></a>Histórico de lançamento

Saiba mais sobre as versões mais recentes dos nossos recursos:

- Versão 1904.1 - manutenção atualização para melhorar a estabilidade de plug-ins de gateway.
- Versão [1904](https://aka.ms/wac1904) é a versão mais recente de GA que apresenta a ferramenta Serviços híbridos do Azure e traz recursos que estavam anteriormente no modo de visualização para o canal de GA.
- Versão [1903](https://aka.ms/wac1903) traz as notificações por email do Azure Monitor, a capacidade de adicionar conexões de servidor ou computador do Active Directory e novas ferramentas para gerenciar o Active Directory, DHCP e DNS.
- Versão [1902](https://aka.ms/wac1902) adicionado a uma lista de conexão compartilhada e melhorias ao gerenciamento de SDN (rede) definida pelo software, incluindo novas ferramentas SDN para gerenciar ACLs, conexões de gateway e redes lógicas.
- A versão [1812](https://aka.ms/wac1812) adicionou o tema escuro (na prévia), configurações de energia, informações de BMC e suporte ao PowerShell para gerenciar [extensões](./configure/using-extensions.md#manage-extensions-with-powershell) e [conexões](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags).
- A versão [1809.5](https://aka.ms/wac1809.5) é uma atualização cumulativa de GA que inclui diversas melhorias funcionais e de qualidade e correções de bugs em toda a plataforma e alguns novos recursos na solução de gerenciamento de infraestrutura hiperconvergente.
- A versão [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) era uma versão de GA que trouxe recursos presentes anteriormente na prévia do canal GA.
- A versão [1808](https://aka.ms/WACPreview1808-InsiderBlog) adicionou a ferramenta Aplicativos Instalados, muitos aprimoramentos subjacentes e atualizações importantes para o SDK da prévia.
- A versão [1807](https://aka.ms/WACPreview1807-InsiderBlog) adicionou uma experiência de conexão do Azure simplificada, melhorias na página de inventário de VM, funcionalidade de compartilhamento de arquivos, integração de gerenciamento de atualizações do Azure e muito mais. 
- A versão [1806](https://aka.ms/WACPreview1806-InsiderBlog) adicionou um script do PowerShell de apresentação, gerenciamento de SDN, conexões do 2008 R2, SDN, tarefas agendadas e muitos outros aprimoramentos.
- Versão 1804.25: atualização de manutenção para oferecer suporte aos usuários instalando o Windows Admin Center em ambientes completamente offline.
- Versão [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/): o projeto Honolulu transforma-se no Windows Admin Center e adiciona os recursos de segurança e controle de acesso com base em função. Nossa primeira versão GA.
- Versão [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10): acrescentou o suporte para controle de acesso do Azure AD, logs detalhados, conteúdo redimensionável e diversas melhorias de ferramenta.
- A versão [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802) acrescentou suporte para acessibilidade, localização, implantações de alta disponibilidade, marcação, configurações de host do Hyper-V e autenticação do gateway.
- A versão [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002) adicionou mais recursos de máquina virtual e melhorias de desempenho em toda as ferramentas.
- A versão [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/) adicionou ferramentas aguardadas (área de trabalho remota e PowerShell), além de outras melhorias.
- A versão [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) foi lançada como nossa primeira versão pública.

## <a name="stay-updated"></a>Mantenha-se atualizado

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Siga-no Twitter](https://twitter.com/servermgmt)

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[Leia nossos Blogs](https://blogs.technet.microsoft.com/servermanagement/)
