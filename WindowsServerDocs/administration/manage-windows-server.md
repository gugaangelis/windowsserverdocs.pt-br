---
title: Management
description: Saiba mais sobre ferramentas, recomendações e diretrizes sobre como gerenciar o Windows Server
ms.prod: windows-server-threshold
layout: LandingPage
ms.technology: manage
ms.topic: landing-page
author: lizap
ms.author: elizapo
ms.localizationpriority: high
ms.openlocfilehash: e6a5357e3e33b3d3318a3e281bbb5c80be842155
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63721374"
---
# <a name="management"></a>Management


>[!TIP]
> Está procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar informações específicas neste site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions).

<hr />

Depois que tiver implantado o Windows Server em seu ambiente, incluindo as funções específicas para os recursos e funções de que você precisar, a próxima etapa será gerenciar esses servidores. O Windows Server inclui várias ferramentas para ajudá-lo a entender seu ambiente do Windows Server, a gerenciar servidores específicos, a ajustar o desempenho e, eventualmente, a automatizar muitas tarefas de gerenciamento. 

As ferramentas que você usa para gerenciar as instâncias do Windows Server dependem, em grande parte, dos tipos de sistemas que você implantou (Windows Server com Experiência Desktop versus Server Core), computadores físicos versus máquinas virtuais e onde seus servidores estão localizados. Use as informações a seguir para executar tarefas básicas de gerenciamento no Windows Server.

Use a tabela a seguir para determinar quais ferramentas usar e quando.

| Eu estou   | Instalar e executar o Windows Admin Center | Executar o Gerenciador do Servidor no Windows Server | Executar o Gerenciador do Servidor nas Ferramentas de Administração de Servidor Remoto no Windows 10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Sentado em um computador Windows 10 | X  |                                      | X                                        |
| Sentado em um sistema Windows Server que executa a experiência desktop | X | X | X |
| Sentado em um sistema Windows Server que executa o Server Core |X (instalar no Windows 10, usar para gerenciar o Server Core) | | X |
| Sentado distante do meu sistema Windows Server |X | | X |
| Sentado distante do meu sistema Windows Server, mas ele TEM a experiência desktop |X | Usar o RDS para se conectar remotamente ao servidor e usar o Gerenciador do Servidor | X |

Além das ferramentas mencionadas abaixo, você também pode usar os [Serviços de Área de Trabalho Remota](../remote/remote-desktop-services/welcome-to-rds.md) para acessar servidores locais, remotos e virtuais. Então, você poderá usar o Gerenciador do Servidor para executar tarefas de gerenciamento.

<HR />

<ul class="cardsI panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Gerenciar sistemas e ambientes do Windows Server</h3>
<HR />
                        <p><h3><a href="../manage/windows-admin-center/overview.md">Gerenciar sistemas locais, sistemas remotos e sistemas sem interface do usuário com o Windows Admin Center</a></h3>Um aplicativo de gerenciamento baseado em navegador que permite a administração local de servidores Windows sem dependência do Azure ou nuvem. Windows Admin Center (anteriormente chamado de "Project Honolulu") oferece controle total sobre todos os aspectos da sua infraestrutura de servidor e é particularmente útil para gerenciamento em redes privadas que não estejam conectadas à Internet. Você pode instalar o Windows Admin Center no Windows 10, em um servidor de gateway ou diretamente no sistema do Windows Server que você deseja gerenciar.</p>
<HR />
                        <p><h3><a href="server-manager/server-manager.md">Gerenciar sistemas locais com o Gerenciador do Servidor</a></h3>Um console de gerenciamento incluído na instalação completa do Windows Server. (Ele não está disponível para instalações que não têm interface do usuário – o Server Core não inclui o Gerenciador do Servidor.) Use o Gerenciador do Servidor para instalar e remover funções de servidor, adicionar e remover servidores remotos, iniciar e interromper serviços e exibir dados coletados sobre seu ambiente.</p>
<HR />
                        <p><h3><a href="../remote/remote-server-administration-tools.md">Gerenciar sistemas remotos e sistemas sem interface do usuário com o RSAT (Ferramentas de Administração de Servidor Remoto)</a></h3>Se o ambiente incluir instalações do Server Core ou servidores remotos (local ou máquinas virtuais), você poderá usar as Ferramentas de Administração de Servidor Remoto para gerenciar esses sistemas. As Ferramentas de Administração de Servidor Remoto incluem o Gerenciador do Servidor, para que você possa usá-lo para gerenciar todos os seus servidores. Observe que as Ferramentas de Administração de Servidor Remoto são executadas no Windows 10. Não é possível instalar as Ferramentas de Administração de Servidor Remoto no Windows Server Core. Você também pode gerenciar instalações Server Core usando a linha de comando. Consulte <a href="server-core/server-core-administer.md">Tarefas básicas de administração no Server Core</a>
<HR />
                        <p><h3><a href="windows-server-update-services/get-started/windows-server-update-services-wsus.md">Gerenciar atualizações de sistemas Windows Server</a></h3>Use o WSUS (Windows Server Update Services) para gerenciar e implantar atualizações para os sistemas em seu ambiente do Windows Server.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Coletar informações sobre o seu ambiente</h3>
<HR />
                        <p><h3><a href="get-started-with-setup-and-boot-event-collection.md">Coleta de Eventos de Instalação e Inicialização</a></h3>A Coleta de Eventos de Instalação e Inicialização permite a você designar um computador "coletor" que possa coletar uma variedade de eventos importantes que ocorrem em outros computadores quando eles inicializam ou passam pelo processo de instalação. Você pode analisar posteriormente os eventos coletados com os cmdlets do Visualizador de Eventos, Analisador de Mensagem, Wevtutil ou Windows PowerShell. </p>
<HR />
                        <p><h3><a href="software-inventory-logging/get-started-with-software-inventory-logging.md">SIL (Log de Inventário de Software)</a></h3>O Log de Inventário de Software no Windows Server é um recurso com um conjunto simples de cmdlets do PowerShell que ajudam os administradores de servidor a recuperar uma lista dos softwares Microsoft instalados em seus servidores. Ele também fornece a capacidade de coletar e encaminhar esses dados periodicamente pela rede para um servidor Web de destino, usando o protocolo HTTPS, para agregação. O gerenciamento do recurso, principalmente para coleta e encaminhamento por hora, é igualmente feito com comandos do PowerShell.</p>
<HR />
                        <p><h3><a href="user-access-logging/get-started-with-user-access-logging.md">UAL (Log de Acesso do Usuário)</a></h3>O Log de Acesso do Usuário agrega eventos exclusivos de dispositivo do cliente e de solicitação de usuário registrados em um computador que executa o Windows Server 2012, o Windows Server 2012 R2 ou o Windows Server 2016 em um banco de dados local. Esses registros são disponibilizados (através de uma consulta por um administrador de servidor) para recuperar quantidades e instâncias por função de servidor, usuário, dispositivo, servidor local e data. Além disso, o UAL também permite que os desenvolvedores de software não Microsoft instrumentem os eventos do UAL para serem agregados. </a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Ajustar seu ambiente do Windows Server para desempenho</h3>
<HR />
                        <p><h3><a href="performance-tuning/index.md">Diretrizes de ajuste de desempenho</a></h3>Examine um conjunto de diretrizes que você pode usar para ajustar as configurações de servidor no Windows Server e obtenha ganhos de eficiência de energia ou de desempenho incrementais, especialmente quando a natureza da carga de trabalho varia pouco ao longo do tempo.</p>
<HR />
                        <p><h3><a href="server-performance-advisor/microsoft-server-performance-advisor.md">Microsoft Server Performance Advisor</a></h3>Com o SPA (Server Performance Advisor) da Microsoft, você pode coletar métricas para diagnosticar discretamente problemas de desempenho em servidores Windows sem adicionar agentes de software ou reconfigurar servidores de produção. O SPA gera relatórios de desempenho abrangentes e gráficos históricos com recomendações.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatizar o gerenciamento do Windows Server</h3>
<HR />
                        <p><h3><a href="https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-5.1">Windows PowerShell</a></h3>O Windows PowerShell é um shell de linha de comando e linguagem de scripts projetado para permitir que você automatize rapidamente as tarefas administrativas. </p>
<HR />
                        <p><h3><a href="windows-commands/windows-commands.md">Comandos do Windows</a></h3>As ferramentas de linha de comando do Windows são usadas para executar tarefas administrativas no Windows. Você pode usar a referência de comando para se familiarizar com as ferramentas de linha de comando, para saber mais sobre o shell de comando e para automatizar as tarefas de linha de comando usando arquivos de lote ou ferramentas de script.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatizar o gerenciamento do Windows Server</h3>
<HR />
                        <p><h3><a href="..\manage\system-insights\overview.md">Insights do sistema</h3></a>Análises preditivas nativas analisam localmente os dados de sistema do Windows Server, tais como contadores de desempenho e eventos de ETW, ajudando os administradores de TI a detectarem e solucionarem proativamente comportamentos problemáticos em implantações.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>