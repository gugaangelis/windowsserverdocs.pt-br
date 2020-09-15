---
title: O que é o Server Core 2008?
description: Saiba mais sobre a opção de instalação Server Core no Windows Server 2008
ms.date: 11/01/2017
ms.topic: article
author: pronichkin
ms.author: artemp
ms.openlocfilehash: 443c0307529a21a6a23da996486a2e6c40894af1
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077783"
---
# <a name="what-is-server-core-2008"></a>O que é o Server Core 2008?
>Aplica-se a: Windows Server 2008

>[!NOTE]
>Essas informações se aplicam ao Windows Server 2008. Para obter informações sobre o Server Core no Windows Server, consulte [o que é a instalação Server Core no Windows Server](./what-is-server-core.md).

A opção Server Core é uma nova opção de instalação mínima que está disponível quando você está implantando a edição Standard, Enterprise ou Datacenter do Windows Server 2008. O Server Core fornece uma instalação mínima do Windows Server 2008 que oferece suporte à instalação de determinadas funções de servidor, conforme descrito posteriormente neste capítulo. Compare isso com a opção de instalação completa do Windows Server 2008, que dá suporte à instalação de todas as funções de servidor disponíveis e também a outros aplicativos de servidor da Microsoft ou de terceiros, como o Microsoft Exchange Server ou o SAP.

Antes de continuarmos, a frase "opção de instalação" precisa ser explicada. Normalmente, quando você adquire uma cópia do Windows Server 2008, você compra uma licença para usar determinadas edições ou unidades de manutenção de ações (SKUs). A tabela 1-1 lista as várias edições do Windows Server 2008 que estão disponíveis. A tabela também indica quais opções de instalação (Full, Server Core ou both) estão disponíveis para cada edição.

**Tabela 1-1** Edições 2008 do Windows Server e seu suporte para opções de instalação

| Edição       | Completo          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard (x86 e x64)       | X | X        |
| Windows Server 2008 Enterprise (x86 e x64)       | X | X        |
| Windows Server 2008 Datacenter (x86 e x64)        | X | X       |
| Windows Web Server 2008 (x86 e x64)       | X | X  |
| Windows Server 2008 para sistemas baseados em Itanium       | X |     |
| Windows HPC Server 2008 (somente x64)       | X |   |
| Windows Server 2008 Standard sem Hyper-V (x86 e x64) | X | X |
| Windows Server 2008 Enterprise sem Hyper-V (x86 e x64)  | X | X |
| Windows Server 2008 Standard sem Hyper-V (x86 e x64) | X | X |

Para entender o que é uma "opção de instalação", digamos que você tenha comprado uma licença de volume que permite instalar uma cópia do Windows Server 2008 Enterprise Edition. Quando você insere a mídia licenciada por volume em um sistema e inicia o processo de instalação, uma das telas que você verá, como mostra a Figura 1-1, apresenta uma opção de edições e opções de instalação.

![Selecionando uma opção de instalação do Server Core para instalar](../media/what-is-server-core-2008/FIg1-1.png)

**Figura 1-1** Selecionando uma opção de instalação do Server Core para instalar

Na Figura 1-1, sua licença de volume (ou chave do produto para mídia de varejo) fornece duas opções de instalação que podem ser escolhidas: a segunda opção (uma instalação completa do Windows Server 2008 Enterprise) e a quinta opção (uma instalação Server Core do Windows Server 2008 Enterprise), com a última selecionada neste exemplo.

## <a name="full-vs-server-core"></a>Núcleo completo versus Server Core
Desde os primórdios da plataforma Microsoft Windows, os servidores Windows eram essencialmente servidores "tudo" que incluíam todos os tipos de recursos, alguns dos quais você pode nunca usar em seu ambiente de rede. Por exemplo, quando você instalou o Windows Server 2003 em um sistema, os binários do serviço de roteamento e acesso remoto (RRAS) foram instalados no servidor, mesmo que você não tenha a necessidade desse serviço (embora ainda precise configurar e habilitar o RRAS antes que ele funcione). O Windows Server 2008 aprimora as versões anteriores instalando os binários necessários para uma função de servidor somente se você optar por instalar essa função em particular no servidor. No entanto, a opção de instalação completa do Windows Server 2008 ainda instala muitos serviços e outros componentes que geralmente não são necessários para um cenário de uso específico.

Essa é a razão pela qual a Microsoft criou uma segunda opção de instalação — Server Core – para o Windows Server 2008: para eliminar quaisquer serviços e outros recursos que não são essenciais para o suporte de certas funções de servidor usadas com frequência. Por exemplo, um servidor DNS (sistema de nomes de domínio) realmente não precisa do Windows Internet Explorer instalado nele, pois você não iria querer procurar a Web de um servidor DNS por motivos de segurança. E um servidor DNS nem precisa de uma GUI (interface gráfica do usuário), pois você pode gerenciar praticamente todos os aspectos do DNS por meio da linha de comando usando o poderoso comando Dnscmd.exe ou usando remotamente o snap-in do MMC (console de gerenciamento Microsoft) do DNS.

Para evitar isso, a Microsoft decidiu distribuir tudo do Windows Server 2008 que não era absolutamente essencial para a execução de serviços de rede de núcleo como Active Directory Domain Services (AD DS), DNS, protocolo DHCP, arquivo e impressão e algumas outras funções de servidor. O resultado é a nova opção de instalação do Server Core, que pode ser usada para criar um servidor que dá suporte a apenas um número limitado de funções e recursos.

## <a name="the-server-core-gui"></a>A GUI do Server Core
Ao terminar de instalar o Server Core em um sistema e fazer logon pela primeira vez, você estará em um pouco de surpresa. A Figura 1-2 mostra a interface do usuário Server Core após o primeiro logon.

![Interface do usuário Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**Figura 1-2** Interface do usuário Server Core

Não há área de trabalho! Ou seja, não há nenhum shell do Windows Explorer, com o menu Iniciar, a barra de tarefas e os outros recursos que você pode estar acostumado a ver. Tudo o que você tem é um prompt de comando, o que significa que você precisa fazer a maior parte do trabalho de configurar uma instalação do Server Core digitando comandos um de cada vez, o que é lento ou usando scripts e arquivos em lotes, o que pode ajudá-lo a acelerar e simplificar as tarefas de configuração, automatizando-as. Você também pode executar algumas tarefas de configuração iniciais usando arquivos de resposta ao executar uma instalação autônoma do Server Core.

Para os administradores que são especialistas no uso de ferramentas de linha de comando como Netsh.exe, Dfscmd.exe e Dnscmd.exe, configurar e gerenciar uma instalação do Server Core pode ser fácil, até mesmo divertido. No entanto, para aqueles que não são especialistas, tudo não é perdido. Você ainda pode usar as ferramentas padrão do MMC do Windows Server 2008 para gerenciar uma instalação do Server Core. Você só precisa usá-los em um sistema diferente que esteja executando uma instalação completa do Windows Server 2008 ou do Windows Vista com Service Pack 1.

Você aprenderá mais sobre a configuração e o gerenciamento de uma instalação do Server Core nos capítulos 3 a 6 deste livro, enquanto os capítulos posteriores lidam com a maneira de gerenciar funções de servidor específicas e outros componentes. Para saber mais sobre as várias ferramentas de linha de comando do Windows e como usá-las, há dois bons recursos a serem consultados:
* A seção de referência de comando da biblioteca técnica do Windows Server 2008 ()
* *O consultor de bolso do administrador de linha de comando do Windows* por William R. Stanek (Microsoft Press, 2008)

A tabela 1-2 lista os principais aplicativos de GUI, junto com seus executáveis, que estão disponíveis em uma instalação Server Core.

**Tabela 1-2** Aplicativos de GUI disponíveis em uma instalação do Server Core

| Aplicativo de GUI | Executável com caminho |
| -------------   | -------------       |
| Prompt de comando | % WINDIR% \System32\Cmd.exe |
| Ferramenta de diagnóstico de Suporte da Microsoft | % WINDIR% \System32\MSdt.exe |
| Bloco de notas | % WINDIR% \System32\Notepad.exe |
| Editor do registro | % WINDIR% \System32\Regedt32.exe |
| Informações do sistema | % WINDIR% \System32\MSinfo32.exe |
| Gerenciador de tarefas | % WINDIR% \System32\Taskmgr.exe |
| Windows Installer | % WINDIR% \System32\MSiexec.exe |

Essa é uma lista bem curta! Agora, aqui está uma lista de elementos da interface do usuário que não estão incluídos no Server Core:
* O Windows Explorer desktop Shell (Explorer.exe) e quaisquer recursos de suporte, como temas
* Todos os consoles do MMC
* Todos os utilitários do painel de controle, com exceção das opções regionais e de idioma (Intl.cpl) e data e hora (Timedate.cpl)
* Todos os mecanismos de renderização de linguagem HTML (HTML), incluindo o Internet Explorer e a ajuda HTML
* Windows Mail
* Windows Media Player
* A maioria dos acessórios, como o Paint, a calculadora e o WordPad

O .NET Framework também não está presente no Server Core, o que significa que não há suporte para a execução de código gerenciado em uma instalação do Server Core. Somente código nativo — código escrito usando as APIs (interfaces de programação de aplicativo) do Windows — pode ser executado no Server Core. Em resumo, todos os aplicativos de GUI que dependem do .NET Framework ou no Shell do Explorer.exe não serão executados no Server Core.

>[!NOTE]
>Como o Windows PowerShell requer o .NET Framework, não é possível instalar o Windows PowerShell no Server Core. No entanto, é possível gerenciar uma instalação do Server Core remotamente usando o Windows PowerShell, desde que você use apenas comandos WMI do PowerShell.

## <a name="supported-server-roles"></a>Funções de servidor com suporte
Uma instalação do Server Core inclui apenas um número limitado de funções de servidor comparadas com uma instalação completa do Windows Server 2008. A tabela 1-3 compara as funções disponíveis para as instalações completa e Server Core do Windows Server 2008 Enterprise Edition.

**Tabela 1-3** Comparação de funções de servidor para instalações completas e Server Core do Windows Server 2008 Enterprise Edition

| Função de servidor  | Disponível em instalação completa  | Disponível no Server Core  |
| ------------- | :-------------: | :------------: |
| AD CS (Serviços de Certificados do Active Directory)  | X |  |
| Active Directory Domain Services (AD DS)  | X  | X |
| Serviços de Federação do Active Directory (AD FS)  | X  |  |
| AD LDS (Active Directory Lightweight Directory Services)  | X  | X |
| AD RMS (Active Directory Rights Management Services)  | X  |  |
| Servidor de Aplicativos  | X  |  |
| Servidor DHCP  | X  | X |
| Servidor DNS  | X  | X |
| Servidor de Fax  | X  |  |
| Serviços de arquivo  | X  | X |
| Hyper-V  | X | X |
| Network Policy and Access Services  | X  |  |
| Serviços de impressão  | X  | X |
| Serviços de Mídia de Fluxo Contínuo  | X  | X |
| Serviços de Terminal  | X  |  |
| Serviços UDDI  | X  |  |
| Servidor Web (IIS) | X | X |
| Windows Deployment Services  | X |  |

Embora as funções disponíveis para o Server Core sejam geralmente as mesmas, independentemente da arquitetura (x86 ou x64) e da edição do produto, há algumas exceções:
* A função Hyper-V (virtualização) estará disponível somente se você comprou o Windows Server 2008 com a mídia do produto Hyper-v (o Hyper-V está disponível apenas para versões x64). Se você não precisar dessa função, poderá comprar o Windows Server 2008 sem a mídia do produto Hyper-V em vez disso.
* A função serviços de arquivo na Standard Edition é limitada a uma raiz autônoma de Sistema de Arquivos Distribuído (DFS) e não oferece suporte a replicação entre arquivos (DFS-R).
* Antes de instalar a função serviços de mídia de streaming no Server Core, você precisa baixar e instalar o pacote Microsoft Update autônomo apropriado (arquivo. msu) para a arquitetura do seu servidor (x86 ou x64) do centro de download da Microsoft.
* A função do servidor Web (IIS) não dá suporte a ASP.NET. Isso ocorre porque o .NET Framework não tem suporte no Server Core, que limita o que você pode fazer com um servidor Web Server Core.

## <a name="supported-optional-features"></a>Recursos opcionais com suporte
Uma instalação do Server Core também dá suporte a apenas um subconjunto limitado dos recursos disponíveis em uma instalação completa do Windows Server 2008. A tabela 1-4 compara os recursos disponíveis para as instalações completa e Server Core do Windows Server 2008 Enterprise Edition.

**Tabela 1-4** Comparação de recursos para instalações completas e do Server Core do Windows Server 2008 Enterprise Edition

| Recurso  | Disponível em instalação completa  | Disponível no Server Core  |
| ------------- | :-------------: | :------------: |
| Recursos do .NET Framework 3.0  | X  |  |
| Criptografia de Unidade de Disco BitLocker  | X  | X |
| Extensões de servidor BITS  | X  |  |
| Kit de administração do Gerenciador de conexões  | X |  |
| Experiência Desktop  | X |  |
| Clustering de failover  | X  | X |
| Gerenciamento de Política de Grupo  | X  |  |
| Cliente de Impressão via Internet  | X  |  |
| Internet Storage Name Server  | X  |  |
| Monitor de Porta LPR  | X  |  |
| Enfileiramento de Mensagens  | X  |  |
| E/s de vários caminhos  | X  | X |
| Network Load Balancing  | X  | X |
| Protocolo PNRP  | X  |  |
| Quality Windows Audio-Video Experience  | X |  |
| Assistência Remota  | X  |  |
| Compactação Diferencial Remota | X  |  |
| Ferramentas de Administração de Servidor Remoto  | X  |  |
| Gerenciador de armazenamento removível | X  | X |
| RPC sobre proxy HTTP | X  |  |
| Serviços TCP/IP Simples  | X  |  |
| Servidor SMTP  | X  |  |
| Serviços SMNPs  | X  | X  |
| Gerenciador de Armazenamento para Redes SAN  | X  |  |
| Subsistema para Aplicativos Baseados em UNIX | X | X  |
| Cliente Telnet  | X | X  |
| Servidor Telnet  | X   |  |
| Cliente TFTP  | X   |  |
| Banco de Dados Interno do Windows  | X  |  |
| Windows PowerShell  | X  |  |
| Serviço de ativação de produto do Windows  | X   |  |
| Recursos de Backup do Windows Server  | X  | X  |
| Gerenciador de recursos de sistema do Windows  | X  |  |
| Servidor WINS  | X | X |
| Serviço de LAN sem fio | X  |  |

Novamente, há alguns pontos que você precisa saber sobre os recursos disponíveis no Server Core:
* Alguns recursos podem exigir hardware especial para funcionar corretamente (ou) no Server Core. Esses recursos incluem Criptografia de Unidade de Disco BitLocker, clustering de failover, e/s de vários caminhos, balanceamento de carga de rede e armazenamento removível.
* O clustering de failover não está disponível na Standard Edition.

## <a name="server-core-architecture"></a>Arquitetura do Server Core
Aprofundando-se no Server Core, vamos examinar rapidamente a arquitetura de uma instalação Server Core do Windows Server 2008, comparando-a com a instalação completa. Primeiro, lembre-se de que o Server Core não é uma versão diferente do Windows Server 2008, mas simplesmente uma opção de instalação que você pode selecionar ao instalar o Windows Server 2008 em um sistema. Isso significa o seguinte:
* O kernel em uma instalação Server Core é o mesmo encontrado em uma instalação completa da mesma arquitetura de hardware (x86 ou x64) e edição.
* Se um binário estiver presente em uma instalação do Server Core, uma instalação completa da mesma arquitetura de hardware (x86 ou x64) e edição terá a mesma versão desse binário específico (com duas exceções discutidas posteriormente).
* Se uma determinada configuração (por exemplo, uma exceção de firewall específica ou o tipo de inicialização de um serviço específico) tiver uma determinada configuração padrão em uma instalação Server Core, essa configuração será configurada exatamente da mesma forma em uma instalação completa da mesma arquitetura de hardware (x86 ou x64) e edição.

A Figura 1-3 mostra uma visão simplificada da arquitetura de uma instalação completa e uma instalação do Server Core do Windows Server 2008. A linha pontilhada indica a arquitetura do Server Core, enquanto o diagrama inteiro representa a arquitetura de uma instalação completa.

O diagrama ilustra a arquitetura modular do Windows Server 2008, com o Server Core que está sendo construído sobre um subconjunto dos recursos principais do sistema operacional. Para a mesma arquitetura e edição de hardware, todos os arquivos presentes em uma instalação limpa do Server Core também estão presentes em uma instalação completa, com exceção de dois arquivos especiais (Scregedit. wsf e Oclist.exe), que estão presentes apenas no Server Core. Esses arquivos especiais foram incluídos no Server Core para simplificar a configuração inicial de uma instalação do Server Core e a adição ou remoção de funções e componentes opcionais.

![As arquiteturas do Server Core e das instalações completas](../media/what-is-server-core-2008/Fig1-3.png)

**Figura 1-3** As arquiteturas do Server Core e das instalações completas

## <a name="driver-support"></a>Suporte a driver
O diagrama arquitetônico do Server Core mostrado na Figura 1-3 é obviamente simplificado; uma coisa que ele não mostra é a diferença no suporte a drivers de dispositivo entre o Server Core e as instalações completas. Uma instalação completa do Windows Server 2008 contém milhares de drivers integrados para diferentes tipos de dispositivos, o que permite que você instale produtos em uma ampla variedade de configurações de hardware diferentes. (Os sistemas operacionais cliente, como o Windows Vista, incluem ainda mais drivers para dar suporte a dispositivos como câmeras digitais e scanners que normalmente não são usados com servidores.)

Se um novo dispositivo estiver conectado ao (ou instalado em) uma instalação completa do Windows Server 2008, o subsistema de Plug and Play (PnP) primeiro verificará se um driver na caixa para o dispositivo está presente. Se for encontrado um driver in-box compatível, o subsistema PnP instalará automaticamente o driver e o dispositivo funcionará. Em uma instalação completa do Windows Server 2008, uma notificação de pop-up de balão pode ser exibida, indicando que o driver foi instalado e o dispositivo está pronto para uso.

Em uma instalação do Server Core, o processo de instalação do driver é o mesmo (o subsistema PnP está presente no Server Core) com duas qualificações. Primeiro, o Server Core inclui apenas um número mínimo de drivers na caixa e apenas para os seguintes tipos de dispositivos:
* Um driver de vídeo VGA (Video Graphics Array) padrão
* Drivers para dispositivos de armazenamento
* Drivers para adaptadores de rede

Observe que, para cada uma das três categorias de dispositivo mostradas aqui, o Server Core inclui os mesmos drivers na caixa encontrados em uma instalação completa correspondente (para a mesma arquitetura de hardware).

Além disso, quando o subsistema PnP instala automaticamente um driver para um novo dispositivo, ele faz isso silenciosamente – nenhuma notificação de pop-up de balão é exibida. Por que não? Como não há nenhuma GUI no Server Core, não há nenhuma barra de tarefas, portanto, não há nenhuma área de notificação na barra de tarefas!

Então, o que fazer quando você adiciona a função de serviços de impressão a uma instalação do Server Core e deseja instalar uma impressora? Você adiciona o driver de impressora manualmente ao servidor — o Server Core não tem drivers de impressão na caixa.

## <a name="service-footprint"></a>Superfície do serviço
Como o Server Core é uma instalação mínima, ele tem uma superfície de serviço de sistema menor do que uma instalação completa correspondente da mesma arquitetura e edição de hardware. Por exemplo, cerca de 75 serviços do sistema são instalados por padrão em uma instalação completa do Windows Server 2008, dos quais aproximadamente 50 são configurados para inicialização automática. Por outro lado, o Server Core tem apenas cerca de 70 serviços instalados por padrão e menos de 40 deles são iniciados automaticamente.

A tabela 1-5 lista os serviços que são instalados por padrão em uma instalação do Server Core, com o modo de inicialização para e a conta usada por cada serviço.

**Tabela 1-5** Serviços do sistema instalados por padrão no Server Core

| Nome do serviço  | Nome de exibição  | Modo de inicialização  | Conta  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Experiência do aplicativo  | Auto | LocalSystem |
| AppMgmt  | Gerenciamento de aplicativos  | Manual | LocalSystem |
| BFE | Mecanismo de Filtragem Base  | Auto | LocalService |
| BITS | BITS  | Auto | LocalSystem |
| Navegador | Pesquisador de Computadores  | Manual | LocalSystem |
| CertPropSvc | Propagação de Certificado  | Manual | LocalSystem |
| COMSysApp  | Aplicativo de Sistema COM+  | Manual | LocalSystem |
| CryptSvc  | Serviços criptográficos  | Auto | Serviço de rede |
| DcomLaunch  | Inicializador de Processo do Servidor DCOM  | Auto | LocalSystem |
| Dhcp  | Cliente DHCP  | Auto | LocalService |
| Dnscache | Cliente DNS  | Auto | Serviço de rede |
| DPS  | Serviço de Política de Diagnóstico  | Auto | LocalService |
| Log | Log de eventos do Windows  | Auto | LocalService |
| EventSystem  | Sistema de Eventos COM+  | Auto | LocalService |
| FCRegSvc  | Serviço de registro do Microsoft Fibre Channel Platform  | Manual | LocalService |
| gpsvc  | Cliente de Diretiva de Grupo  | Auto | LocalSystem |
| hidserv | Acesso dispositivos de interface humana  | Manual | LocalSystem |
| hkmsvc  | Gerenciamento de certificado e chave de integridade  | Manual | LocalSystem |
| IKEEXT  | Módulos de Chave IKE e AuthIP IPsec  | Auto | LocalSystem |
| iphlpsvc  | Auxiliar de IP  | Auto | LocalSystem |
| KeyIso | Isolamento de Chave CNG  | Manual | LocalSystem |
| KtmRm  | KtmRm para Coordenador de Transações Distribuídas  | Auto | Serviço de rede |
| LanmanServer  | Servidor  | Auto | LocalSystem |
| LanmanWorkstation  | Estação de trabalho  | Auto | LocalService |
| lltdsvc  | Mapeador da Descoberta de Topologia da Camada de Link  | Manual | LocalService |
| lmhosts  | Auxiliar NetBIOS TCP/IP  | Auto | LocalService |
| MpsSvc  | Firewall do Windows  | Auto | LocalService |
| MSDTC  | Coordenador de Transações Distribuídas  | Auto | Serviço de rede |
| MSiSCSI  | Serviço Iniciador Microsoft iSCSI  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| napagent  | Agente de proteção de acesso à rede  | Manual | Serviço de rede |
| Netlogon  | Netlogon  | Manual | LocalSystem |
| netprofm  | Serviço da Lista de Redes  | Auto | LocalService |
| NlaSvc  | Reconhecimento de localizações de rede  | Auto | Serviço de rede |
| nsi  | Serviço de interface NSI  | Auto | LocalService |
| pla  | Logs e Alertas de Desempenho  | Manual | LocalService |
| PlugPlay  | Plug and Play  | Auto | LocalSystem |
| PolicyAgent  | Agente de Política IPsec  | Auto | Serviço de rede |
| ProfSvc  | Serviço Perfil do Usuário  | Auto | LocalSystem |
| ProtectedStorage  | Armazenamento protegido  | Manual | LocalSystem |
| RemoteRegistry  | Registro Remoto  | Auto | LocalService |
| RpcSs  | RPC (Chamada de Procedimento Remoto)  | Auto | Serviço de rede |
| RSoPProv | Conjunto Resultante do Provedor de Políticas  | Manual | LocalSystem |
| sacsvr  | Assistente do Console de Administração Especial  | Manual | LocalSystem |
| SamSs  | Gerenciador de Contas de Segurança  | Auto | LocalSystem |
| SCardSvr | Cartão inteligente  | Manual | LocalService |
| Agenda | Agendador de Tarefas  | Auto | LocalSystem |
| SCPolicySvc | Política de Remoção de Cartão Inteligente  | Manual | LocalSystem |
| seclogon | Logon Secundário  | Auto | LocalSystem |
| SENS | Serviço de Notificação de Eventos do Sistema  | Auto | LocalSystem |
| SessionEnv | Configuração de Serviços de Terminal  | Manual | LocalSystem |
| slsvc  | Licenciamento de software | Auto | Serviço de rede |
| SNMPTRAP  | Interceptação SNMP  | Manual | LocalService |
| swprv  | Provedor de Cópias de Sombra de Software da Microsoft | Manual | LocalSystem |
| TBS | Serviços base do TPM  | Manual | LocalService |
| TermService  | Serviços de Terminal | Auto | Serviço de rede |
| TrustedInstaller | Instalador de Módulos do Windows  | Auto | LocalSystem |
| UmRdpService | Redirecionador de porta de UserMode de serviços de terminal  | Manual | LocalSystem |
| vds | Disco Virtual  | Manual | LocalSystem |
| VSS | Cópias de Sombra de Volume  | Manual | LocalSystem |
| W32Time | Tempo do Windows  | Auto | LocalService |
| WcsPlugInService  | Sistema de cores do Windows  | Manual | LocalService |
| WdiServiceHost  | Host de Serviço de Diagnóstico  | Manual | LocalService |
| WdiSystemHost  | Host do Sistema de Diagnóstico  | Manual | LocalSystem |
| Wecsvc | Coletor de Eventos do Windows  | Manual | Serviço de rede |
| WinHttpAuto-ProxySvc  | Serviço de Descoberta Automática de Proxy Web do WinHTTP  | Auto | LocalService |
| Winmgmt | Instrumentação de Gerenciamento do Windows | Auto | LocalSystem |
| WinRM  | Gerenciamento Remoto do Windows (WS-Management) | Auto | Serviço de rede |
| wmiApSrv  | Adaptador de Desempenho WMI  | Manual | LocalSystem |
| wuauserv | Windows Update | Auto | LocalSystem |