---
title: Visão geral do Windows Admin Center
description: Saiba como gerenciar o Windows Server usando o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: d941e9884dced40ce750645b662d8df73503bc41
ms.sourcegitcommit: 475292afc919c6d17569f05007a97bc6b92dd225
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2019
ms.locfileid: "9267772"
---
# WindowsAdmin Center

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Bem-vindo ao Windows Admin Center!

**O Windows Admin Center** (codinome **Project Honolulu**) é uma evolução das ferramentas de gerenciamento de caixa de entrada do Windows Server; é um único painel que consolida todos os aspectos do gerenciamento de servidores locais e remotos. Como uma experiência de gerenciamento com base em navegador implantado localmente, isso não requer uma conexão de Internet e o Azure. O Windows Admin Center oferece controle total sobre todos os aspectos da sua implantação, incluindo as redes privadas que não estejam conectadas à Internet.

## Introdução

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Infográfico do Windows Admin Center](media/WAC1809Poster_thumb.PNG)

[Baixar o PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## Início rápido

Você pode configurar e começar a usar o Windows Admin Center em poucos minutos:

1. [Baixar](https://aka.ms/windowsadmincenter)
2. [Instalar](deploy/install.md)
3. [Comece agora](use/get-started.md)

## Conteúdo resumido

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Entenda</h3>
            <ul>
            <li><a href="understand/what-is.md">O que é o Windows Admin Center?</a>
            <li><a href="understand/faq.md">Perguntas frequentes</a>
            <li><a href="understand/case-studies.md">Estudos de caso</a>
            <li><a href="understand/related-management.md">Produtos de gerenciamento relacionadas</a>
            <li><a href="understand/videos.md">Vídeos</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Planejar</h3>
            <ul>
            <li><a href="plan/installation-options.md">Que tipo de instalação é ideal para você?</a>
            <li><a href="plan/user-access-options.md">Opções de acesso de usuário</a>
            <li><a href="plan/azure-integration-options.md">Quais opções de integração do Azure existem?</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Implantar</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Preparar o ambiente</a>
            <li><a href="deploy/install.md">Instalação do Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Habilitar a alta disponibilidade</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configurar</h3>
            <ul>
            <li><a href="configure/settings.md">Configurações do Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Controle de acesso de usuário e permissões</a>
            <li><a href="configure/using-extensions.md">Extensões</a>
            <li><a href="configure/azure-integration.md">Integração com o Windows Azure</a>
            <li><a href="configure/manage-azure-vms.md">Gerenciar VMs do Azure com o Windows Admin Center</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Use</h3>
            <ul>
            <li><a href="use/get-started.md">Inicializar e adicionar conexões</a>
            <li><a href="use/manage-servers.md">Gerenciar servidores</a>
            <li><a href="use/manage-hyper-converged.md">Gerenciar a infraestrutura de hyper-convergiu</a>
            <li><a href="use/manage-failover-clusters.md">Gerenciar clusters de failover</a>
            <li><a href="use/manage-virtual-machines.md">Gerenciar máquinas virtuais</a>
            <li><a href="use/azure-services.md">Aproveite os serviços do Azure</a>
            <li><a href="use/troubleshooting.md">Etapas de solução de problemas comuns</a>
            <li><a href="use/logging.md">Registrar em log</a>
            <li><a href="use/known-issues.md">Problemas conhecidos</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Estender</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">Visão geral das extensões</a>
            <li><a href="extend/understand-extensions.md">Extensões de compreensão</a>
            <li><a href="extend/developing-extensions.md">Desenvolver uma extensão</a>
            <li><a href="extend/publish-extensions.md">Guias</a>
            <li><a href="extend/publish-extensions.md">Extensões de publicação</a>
            </ul>
        </td>
    </tr>

</table>

## Histórico de versão

Saiba mais sobre as versões mais recentes dos nossos recursos:

- A versão [1903] (https://aka.ms/wac1903) traz notificações por email do Azure Monitor, a capacidade de adicionar conexões de servidor ou de computador do Active Directory e as novas ferramentas para gerenciar o Active Directory, DHCP e DNS.
- A versão [1902] (https://aka.ms/wac1902) adicionou uma lista de conexão compartilhada e melhorias ao gerenciamento de rede definida por software (SDN), incluindo novas ferramentas de SDN para gerenciar ACLs, conexões de gateway e redes lógicas.
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

## Fique atualizado

<a target="_blank" class="mscom-link twitter-follow-link" title="Siga-nos no Twitter" aria-label="Follow us on Twitter" data-info="Twitter" href="https://twitter.com/servermgmt"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" alt="Follow us on Twitter" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR"></picture></a>
 | 
<a target="_blank" class="mscom-link blogs-follow-link" title="Leia nossa Blogs" aria-label="Visit our Blogs" data-info="Blogs" href="https://blogs.technet.microsoft.com/servermanagement/"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" alt="Follow us on Blogs" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw"></picture></a>
