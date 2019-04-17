---
title: Gerenciar o Windows Server
description: Aprende sobre ferramentas, recomendações e diretrizes sobre como gerenciar o Windows Server
ms.prod: windows-server-threshold
ms.technology: manage
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 03/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4faadb811927626c26a5b01e2ce0598d40792b68
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066930"
---
# Gerenciar o Windows Server

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar este site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

 <ul class="cardse panelContent cols cols3">
    <li>
        <a href="https://docs.microsoft.com/windows-insider/at-work-pro/wip-4-biz-feedback-hub">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h2>Gerenciar</h2>
                <p>Depois que tiver implantado o Windows Server em seu ambiente, incluindo as funções específicas para os recursos e funções de que você precisa, a próxima etapa será gerenciar esses servidores. O Windows Server inclui várias ferramentas para ajudá-lo a entender seu ambiente do Windows Server, a gerenciar servidores específicos, a ajustar o desempenho e, eventualmente, a automatizar muitas tarefas de gerenciamento. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li> 
</ul> 

## Gerenciar sistemas e ambientes do Windows Server
As ferramentas que você usa para gerenciar as instâncias do Windows Server dependem, em grande parte, dos tipos de sistemas que você implantou (Windows Server com Experiência Desktop versus Server Core), máquinas físicas versus virtuais e onde seus servidores estão localizados. Use as informações a seguir para executar tarefas básicas de gerenciamento no Windows Server.

Use a tabela a seguir para determinar quais ferramentas usar e quando.

| Estou   | Instalar e executar o Windows Admin Center | Executar o Gerenciador do Servidor no Windows Server | Executar o Gerenciador do Servidor no RSAT no Windows 10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Sentado em um computador Windows 10 | X  |                                      | X                                        |
| Sentado em um sistema Windows Server que executa a experiência desktop | X | X | X |
| Sentado em um sistema Windows Server que executa o Server Core |X (instalar no Windows 10, usar para gerenciar o Server Core) | | X |
| Sentado distante do meu sistema Windows Server |X | | X |
| Sentado distante do meu sistema Windows Server, mas ele TEM a experiência desktop |X | Usar o RDS para se conectar remotamente ao servidor e usar o Gerenciador do Servidor | X |

Além das ferramentas mencionadas abaixo, você também pode usar os [Serviços de Área de Trabalho Remota](../remote/remote-desktop-services/welcome-to-rds.md) para acessar servidores locais, remotos e virtuais. Então, você poderá usar o Gerenciador do Servidor para executar tarefas de gerenciamento.

### Gerenciar sistemas locais, sistemas remotos e sistemas sem interface do usuário com o Windows Admin Center
O [Windows Admin Center](../manage/windows-admin-center/overview.md) é um aplicativo de gerenciamento baseado em navegador que permite a administração no local de servidores do Windows com nenhuma dependência do Azure ou nuvem. O Windows Admin Center oferece controle total sobre todos os aspectos da sua infraestrutura de servidor e é útil para o gerenciamento de redes privadas que não estão conectadas à Internet. Você pode instalar o Windows Admin Center no Windows 10, em um servidor de gateway ou diretamente no sistema do Windows Server que você deseja gerenciar.

>[!NOTE]
>O Windows Admin Center é o nome oficial do que usamos para chamar o "Project Honolulu".

### Gerenciar sistemas locais com o Gerenciador do Servidor
[O Gerenciador do Servidor](server-manager/server-manager.md) é um console de gerenciamento incluído na instalação completa do Windows Server. (Não está disponível para instalações que não têm a interface do usuário - o Server Core não inclui o Gerenciador do Servidor). Use o Gerenciador do Servidor para instalar e remover funções de servidor, adicionar e remover servidores remotos, iniciar e parar serviços e exibir dados coletados sobre seu ambiente.

### Gerenciar sistemas remotos e sistemas sem interface do usuário com Ferramentas de Administração de Servidor Remoto (RSAT)
Se o ambiente inclui instalações do Server Core ou de servidores remotos (máquinas locais ou virtuais), você pode usar as [ferramentas de Administração de Servidor Remoto (RSAT)](../remote/remote-server-administration-tools.md) para gerenciar esses sistemas. O RSAT inclui o Gerenciador do Servidor, para que você possa usá-lo para gerenciar todos os seus servidores.

> [!IMPORTANT]
> O RSAT é executado no Windows 10. Você não pode instalar o RSAT no Windows Server Core.

Você também pode gerenciar instalações Server Core usando a linha de comando. Consulte [Tarefas básicas de administração no Server Core](server-core/server-core-administer.md).

### Gerenciar atualizações de sistemas Windows Server
Você pode usar o [Windows Server Update Services (WSUS)](windows-server-update-services/get-started/windows-server-update-services-wsus.md) para gerenciar e implantar atualizações feitas nos sistemas presentes em seu ambiente do Windows Server.

## Coletar informações sobre o seu ambiente
Muitas das decisões que você toma como um administrador dependem dos dados sobre os sistemas e os usuários em seu ambiente. Use as informações e ferramentas a seguir para coletar os dados.

Comece com [Configurar os dados de diagnóstico do Windows em sua organização](/windows/configuration/configure-windows-diagnostic-data-in-your-organization) para obter informações sobre dados de diagnóstico que pode ser coletados do Windows 10 e do Windows Server.

### [Coleta de Eventos de Instalação e Inicialização](get-started-with-setup-and-boot-event-collection.md)
A Coleta de Eventos de Instalação e Inicialização permite a você designar um computador "coletor" que possa coletar uma variedade de eventos importantes que ocorrem em outros computadores quando eles inicializam ou passam pelo processo de instalação. Você pode analisar posteriormente os eventos coletados com os cmdlets do Visualizador de Eventos, Analisador de Mensagem, Wevtutil ou Windows PowerShell. 

### [SIL (Log de Inventário de Software)](software-inventory-logging/get-started-with-software-inventory-logging.md)

O Log de Inventário de Software no Windows Server é um recurso com um conjunto simples de cmdlets do PowerShell que ajudam os administradores de servidor a recuperar uma lista dos softwares Microsoft instalados em seus servidores. Ele também fornece a capacidade de coletar e encaminhar esses dados periodicamente pela rede para um servidor Web de destino, usando o protocolo HTTPS, para agregação. O gerenciamento do recurso, principalmente para coleta e encaminhamento por hora, é igualmente feito com comandos do PowerShell.

### [Log de Acesso do Usuário (UAL)](user-access-logging/get-started-with-user-access-logging.md)

O Log de Acesso do Usuário agrega eventos exclusivos de dispositivo do cliente e de solicitação de usuário registrados em um computador que executa o Windows Server 2012, o Windows Server 2012 R2 ou o Windows Server 2016 em um banco de dados local. Esses registros são disponibilizados (através de uma consulta por um administrador de servidor) para recuperar quantidades e instâncias por função de servidor, usuário, dispositivo, servidor local e data. Além disso, o UAL também permite que os desenvolvedores de software não Microsoft instrumentem os eventos do UAL para serem agregados. 

## Ajustar seu ambiente do Windows Server para desempenho
Use as seguintes informações para ajudar a ajustar seu ambiente para desempenho.

### [Diretrizes de ajuste de desempenho](performance-tuning/index.md)
Examine um conjunto de diretrizes que você pode usar para ajustar as configurações de servidor no Windows Server 2016 e obtenha ganhos de eficiência de energia ou de desempenho incrementais, especialmente quando a natureza da carga de trabalho variar pouco ao longo do tempo.

### [Microsoft Server Performance Advisor](server-performance-advisor/microsoft-server-performance-advisor.md)

Com o Microsoft Server Performance Advisor (SPA), você pode coletar métricas para diagnosticar discretamente problemas de desempenho em servidores Windows sem adicionar agentes de software ou reconfigurar servidores de produção. O SPA gera relatórios de desempenho abrangentes e gráficos históricos com recomendações.


## Automatizar o gerenciamento do Windows Server

O Windows Server inclui um conjunto de comandos e módulos do Windows PowerShell que você pode usar para automatizar tarefas de gerenciamento.

### [Windows PowerShell](/powershell/scripting/powershell-scripting?view=powershell-5.1)
O Windows PowerShell é um shell de linha de comando e uma linguagem de scripts projetado para permitir que você automatize rapidamente as tarefas administrativas. 

### [Comandos do Windows](windows-commands/windows-commands.md)

As ferramentas de linha de comando do Windows são usadas para executar tarefas administrativas no Windows. Você pode usar a referência de comando para se familiarizar com as ferramentas de linha de comando, para saber mais sobre o shell de comando e para automatizar as tarefas de linha de comando usando arquivos de lote ou ferramentas de script.

## Windows Server Insider Preview
### [Insights do Sistema](..\manage\system-insights\overview.md)
Insights do sistema é um novo recurso que apresenta a análise de previsão nativamente para o Windows Server. Esses recursos de previsão localmente analisar dados de sistema do Windows Server, como contadores de desempenho ou eventos ETW, ajudando os administradores de TI a detectar e processar proativamente comportamento problemático em suas implantações de endereços. 
