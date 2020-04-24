---
title: Visão geral do Windows Admin Center
description: Saiba como gerenciar o Windows Server usando o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 01/07/2020
ms.localizationpriority: high
ms.prod: windows-server
ms.openlocfilehash: bb2f6d7fcbf18ef9bc67534982d1a98fdc5172a1
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "79320030"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

O Windows Admin Center é um aplicativo baseado em navegador, implantado localmente, que gerencia os servidores, os clusters e a infraestrutura hiperconvergente do Windows, além de computadores com Windows 10. É fornecido sem custo adicional além do Windows e vem pronto para uso na produção.

Para saber as novidades, confira o [Histórico de versões](support/release-history.md).

## <a name="download-now"></a>Baixar agora

**Baixe o [Windows Admin Center](https://www.microsoft.com/evalcenter/evaluate-windows-admin-center)** no Microsoft Evaluation Center. Embora esteja escrito "Iniciar sua avaliação", essa é a versão em disponibilidade geral para ser usada em produção, incluída na licença do Windows ou do Windows Server.

Para obter ajuda com a instalação, confira [Instalar](deploy/install.md). Para obter dicas de como começar a usar o Windows Admin Center, confira [Introdução](use/get-started.md).

Você pode atualizar as versões que não são prévias do Windows Admin Center usando o Microsoft Update ou baixando e instalando o Windows Admin Center manualmente. Cada versão não prévia do Windows Admin Center será compatível por até 30 dias após o lançamento da próxima versão não prévia. Confira nossa [política de suporte](support/index.md) para saber mais.

## <a name="windows-admin-center-scenarios"></a>Cenários do Windows Admin Center

Aqui estão algumas funções do Windows Admin Center:

|     |     |
| --- | --- |
| ![](media/simple-icon.png)| **Simplificar o gerenciamento de servidores** <br/> Gerencie seus servidores e clusters com versões modernizadas de ferramentas familiares, como o Gerenciador do Servidor. Instale-o em menos de cinco minutos e gerencie os servidores no ambiente imediatamente, sem precisar de configuração adicional. Para obter detalhes, confira [O que é o Windows Admin Center?](understand/what-is.md). |
| ![](media/future-icon.png)| **Trabalhar com soluções híbridas** <br/> A integração com o Azure ajuda a conectar opcionalmente seus servidores locais aos serviços de nuvem relevantes. Para obter detalhes, confira [Serviços híbridos do Azure](azure/index.md) |
| ![](media/secure-icon.png)| **Simplificar o gerenciamento de hiperconvergentes** <br/> Simplifique o gerenciamento dos clusters hiperconvergentes do Azure Stack HCI ou do Windows Server. Use cargas de trabalho simplificadas para criar e gerenciar VMs, volumes de Espaços de Armazenamento Diretos, redes definidas por software e muito mais. Para obter detalhes, confira [Gerenciar a infraestrutura hiperconvergente com o Windows Admin Center](use/manage-hyper-converged.md)|

Veja um vídeo que oferece uma visão geral, seguido de um cartaz com mais detalhes:
>[!VIDEO https://www.youtube.com/embed/WCWxAp27ERk]

[![Cartaz do Windows Admin Center](media/WAC1910Poster_thumb_small.PNG)](media/WAC1910Poster_thumb.png)

[Baixar o PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1910Poster.pdf)


## <a name="contents-at-a-glance"></a>Conceitos básicos

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Noções básicas</h3>
            <ul>
            <li><a href="understand/what-is.md">O que é o Windows Admin Center?</a>
            <li><a href="understand/faq.md">Perguntas frequentes</a>
            <li><a href="understand/case-studies.md">Estudos de caso</a>
            <li><a href="understand/related-management.md">Produtos de gerenciamento relacionados</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Planejar</h3>
            <ul>
            <li><a href="plan/installation-options.md">Que tipo de instalação é ideal para você?</a>
            <li><a href="plan/user-access-options.md">Opções de acesso de usuário</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Implantar</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Preparar o ambiente</a>
            <li><a href="deploy/install.md">Instalar o Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Habilitar a alta disponibilidade</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configurar</h3>
            <ul>
            <li><a href="configure/settings.md">Configurações do Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Controle de acesso de usuário e permissões</a>
            <li><a href="configure/shared-connections.md">Conexões compartilhadas</a>
            <li><a href="configure/using-extensions.md">Extensões</a>
            <li><a href="configure/use-powershell.md">Automatizar com o PowerShell</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Uso</h3>
            <ul>
            <li><a href="use/get-started.md">Iniciar e adicionar conexões</a>
            <li><a href="use/manage-servers.md">Gerenciar servidores</a>
            <li><a href="use/deploy-hyperconverged-infrastructure.md">Implantar a infraestrutura hiperconvergente</a>
            <li><a href="use/manage-hyper-converged.md">Gerenciar a infraestrutura hiperconvergente</a>
            <li><a href="use/manage-failover-clusters.md">Gerenciar clusters de failover</a>
            <li><a href="use/manage-virtual-machines.md">Gerenciar máquinas virtuais</a>
            <li><a href="use/logging.md">Log</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Conecte-se ao Azure</h3>
            <ul>
            <li><a href="azure/index.md">Serviços híbridos do Azure</a></li>
            <li><a href="azure/azure-integration.md">Conectar o Windows Admin Center ao Azure</a></li>
            <li><a href="azure/deploy-wac-in-azure.md">Implantar o Windows Admin Center no Azure</a></li>
            <li><a href="azure/manage-azure-vms.md">Gerenciar VMs do Azure com o Windows Admin Center</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>Suporte</h3>
            <ul>
            <li><a href="support/release-history.md">Histórico de versões</a>
            <li><a href="support/index.md">Política de suporte</a>
            <li><a href="support/troubleshooting.md">Etapas de solução de problemas comuns</a>
            <li><a href="support/known-issues.md">Problemas conhecidos</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>Estender</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">Visão geral das extensões</a>
            <li><a href="extend/understand-extensions.md">Noções básicas das extensões</a>
            <li><a href="extend/developing-extensions.md">Desenvolver uma extensão</a>
            <li><a href="extend/publish-extensions.md">Guias</a>
            <li><a href="extend/publish-extensions.md">Extensões de publicação</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="video-based-learning"></a>Aprendizado baseado em vídeo

Aqui estão alguns vídeos de sessões do Microsoft Ignite 2019:

- [Windows Admin Center: desbloqueie o valor híbrido do Azure](https://aka.ms/WAC-BRK3165)
- [Windows Admin Center: novidades e o que vem a seguir](https://aka.ms/WAC-BRK2048)
- [Monitore, proteja e atualize automaticamente seus servidores locais do Azure com o Windows Admin Center](https://aka.ms/WAC-THR2146)
- [Aumente a produtividade com as extensões de terceiros do Windows Admin Center](https://aka.ms/WAC-THR2140)
- [Seja um especialista no Windows Admin Center: práticas recomendadas para implantação, configuração e segurança](https://aka.ms/WAC-THR2135)
- [Windows Admin Center: melhor em conjunto com o System Center e o Microsoft Azure](https://aka.ms/WAC-THR2176)
- [Como usar os serviços híbridos do Microsoft Azure junto com o Windows Admin Center e o Windows Server](https://aka.ms/WAC-THR2073)
- [P e R ao vivo: gerencie seu ambiente de servidor híbrido com o Windows Admin Center](https://aka.ms/WAC-MLS1055)
- [Roteiro de aprendizagem: tecnologias híbridas de gerenciamento](https://aka.ms/WAC-HybridMgmtTech)
- [Laboratório prático: Windows Admin Center e opção híbrida](https://aka.ms/WAC-HOL2019)

Aqui estão alguns vídeos de sessões do Windows Server Summit 2019:

- [Implantar a opção híbrida com o Windows Admin Center](https://aka.ms/WAC-WSS2019-GoHybridWAC)
- [Novidades no Windows Admin Center v1904](https://aka.ms/WAC-WSS2019-WhatsNewv1904)

E aqui estão alguns recursos adicionais:

- [Gerenciamento de servidores do Windows Admin Center reformulado](https://aka.ms/WAC-ServerMgmtReimagined)
- [Gerencie servidores e máquinas virtuais em qualquer lugar com o Windows Admin Center](https://aka.ms/WAC-Webinar2019)
- [Como começar a usar o Windows Admin Center](https://www.youtube.com/embed/PcQj6ZklmK0)

## <a name="see-how-customers-are-benefitting-from-windows-admin-center"></a>Veja como os clientes estão usando o Windows Admin Center

|     |
| --- |
| "O [Windows Admin Center] reduziu o tempo/esforço de administração do sistema de gerenciamento em mais de 75%."<br> *-Rand Morimoto, Presidente, Convergent Computing* |
| "Graças ao [Windows Admin Center], podemos gerenciar nossos clientes remotamente do portal do HTML5 sem problemas e com total integração ao Azure Active Directory; podemos aumentar a segurança graças à Autenticação Multifator."<br/> *-Silvio Di Benedetto, Fundador e Consultor Sênior, Inside Technologies* |
| "Conseguimos implantar SKUs [Server Core] de forma mais eficaz, melhorando a eficiência de recursos, segurança e automação. Ao mesmo tempo, atingimos um bom grau de produtividade e reduzimos os erros que podem acontecer com a dependência somente de scripts." <br/> *-Guglielmo Mengora, Fundador e CEO, VaiSulWeb* |
| "Com o [Windows Admin Center], os clientes no mercado de pequenas e médias empresas agora têm uma ferramenta fácil de usar para gerenciar sua infraestrutura interna. Isso minimiza os esforços administrativos e economiza muito tempo. E o melhor: não há taxas de licença adicionais para o [Windows Admin Center]!" <br/> *- Helmut Otto, Diretor-Geral, SecureGUARD* |

[Leia mais sobre as empresas que usam o Windows Admin Center em seus ambientes de produção.](understand/case-studies.md)

## <a name="related-products"></a>Produtos relacionados

O Windows Admin Center foi projetado para gerenciar um servidor único ou cluster. Ele complementa, mas não substitui, as soluções existentes de monitoramento e gerenciamento da Microsoft, como as RSAT (Ferramentas de Administração de Servidor Remoto), o System Center, o Intune ou o Azure Stack.

[Saiba como o Windows Admin Center complementa outras soluções de gerenciamento da Microsoft.](understand/related-management.md)

## <a name="stay-updated"></a>Fique por dentro

![](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Siga-nos no Twitter](https://twitter.com/servermgmt)

![](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[Leia nossos blogs](https://blogs.technet.microsoft.com/servermanagement/)
