---
title: O que é Server Core 2008?
description: Saiba mais sobre a opção de instalação Server Core no Windows Server 2008
ms.prod: windows-server-threshold
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: c1ef71dbc589cfdeac63b46d720c4bdd0a44dbaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815397"
---
#<a name="what-is-server-core-2008"></a>O que é Server Core 2008?
>Aplica-se a: Windows Server 2008

>[!NOTE]
>Essas informações se aplicam ao Windows Server 2008. Para obter informações sobre o Server Core no Windows Server, consulte [qual é a instalação do Server Core no Windows Server](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core). 

A opção Server Core é uma nova opção de instalação mínima que está disponível quando você estiver implantando a edição Standard, Enterprise ou Datacenter do Windows Server 2008. Server Core fornece uma instalação mínima do Windows Server 2008 que dá suporte à instalação apenas determinadas funções de servidor, conforme descrito mais adiante neste capítulo. Compare isso com a opção de instalação completa do Windows Server 2008, que oferece suporte à instalação de todas as funções de servidor disponíveis e também outros produtos da Microsoft ou aplicativos de servidor de terceiros, como o Microsoft Exchange Server ou SAP. 

Antes de continuarmos, a frase "opção de instalação" precisa ser explicada. Normalmente, quando você compra uma cópia do Windows Server 2008, você comprar uma licença para usar determinadas edições ou unidades de manutenção de estoque (SKU). Tabela 1 1 lista as várias edições do Windows Server 2008 que estão disponíveis. A tabela também indica quais opções de instalação (completo, Server Core ou ambos) estão disponíveis para cada edição.

**Tabela 1 1** edições do Windows Server 2008 e o suporte para opções de instalação
| Edição       | Completo          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard (x86 and x64)       | X | X        |
| Windows Server 2008 Enterprise (x86 e x64)       | X | X        |
| Windows Server 2008 Datacenter (x86 e x64)        | X | X       |
| Windows Web Server 2008 (x86 e x64)       | X | X  |
| Windows Server 2008 For Itanium-based systems       | X |     |
| Windows HPC Server 2008 (x64)       | X |   |
| Windows Server 2008 Standard sem Hyper-V (x86 e x64) | X | X |
| Windows Server 2008 Enterprise sem Hyper-V (x86 e x64)  | X | X |
| Windows Server 2008 Standard sem Hyper-V (x86 e x64) | X | X |

Para compreender o que é uma opção"instalação" é, digamos que você tiver adquirido uma licença de volume que permite que você instale uma cópia do Windows Server 2008 Enterprise Edition. Quando você insere sua mídia licenciada por volume em um sistema e começar o processo de instalação, uma das telas, que você verá, conforme mostrado na Figura 1-1, apresenta uma opção de edições e opções de instalação.

![Selecionando uma opção de instalação Server Core para instalar](../media/what-is-server-core-2008/FIg1-1.png)

**Figura 1-1** selecionando uma opção de instalação Server Core para instalar

Na Figura 1-1, seu licenciamento por volume (ou a chave do produto, para a mídia de varejo) oferece duas opções de instalação, você pode escolher entre: a segunda opção (um total instalação do Windows Server 2008 Enterprise) e a opção quinta (um Server Core instalação do Windows Server 2008 Enterprise), com o último selecionado neste exemplo. 

##<a name="full-vs-server-core"></a>Completos vs. Server Core 
Desde os primórdios da plataforma Microsoft Windows, servidores do Windows eram basicamente "tudo" servidores que incluídos todos os tipos de recursos, alguns dos quais você nunca realmente pode usar em seu ambiente de rede. Por exemplo, quando você instalou o Windows Server 2003 em um sistema, os binários para o roteamento e acesso remoto (RRAS) foram instalados no seu servidor, mesmo se você tivesse não é necessário para este serviço (embora você ainda tinha que configurar e habilitar o RRAS antes de ele funcionaria). Windows Server 2008 melhora a versões anteriores ao instalar os binários necessários para uma função de servidor somente se você optar por instalar daquela função específica em seu servidor. No entanto, a opção de instalação completa do Windows Server 2008 ainda instala vários serviços e outros componentes que geralmente não são necessários para um cenário de uso específico. 

Esse é o motivo que a Microsoft criou uma segunda opção de instalação — Server Core — para o Windows Server 2008: para eliminar todos os serviços e outros recursos que não são essenciais para o suporte de determinadas usados com frequência as funções de servidor. Por exemplo, um servidor de sistema de nome de domínio (DNS) realmente não precisa instalado nele, porque você não iria querer navegar na Web de um servidor DNS, por motivos de segurança do Windows Internet Explorer. E um servidor DNS não precisa sequer uma interface gráfica do usuário (GUI), porque você pode gerenciar praticamente todos os aspectos do DNS da linha de comando usando o comando Dnscmd.exe poderoso, ou remotamente usando o Console de gerenciamento Microsoft (MMC) do DNS snap-in do.

Para evitar isso, a Microsoft decidiu remover tudo, desde o Windows Server 2008 que foi não absolutamente essencial para a execução de serviços principais da rede, como os serviços de domínio do Active Directory (AD DS), DNS, configuração de protocolo DHCP (Dynamic Host), arquivo e impressão, e um algumas outras funções de servidor. O resultado é a nova opção de instalação Server Core, que pode ser usada para criar um servidor que oferece suporte a apenas um número limitado de funções e recursos. 

##<a name="the-server-core-gui"></a>O GUI do Server Core
Ao concluir a instalação Server Core em um sistema e fazer logon pela primeira vez, você está em um pouco surpreendente. Figura 1-2 mostra a interface do usuário de núcleo do servidor após o primeiro logon.

![Interface de usuário do Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**Figura 1-2** interface de usuário do Server Core

Não há nenhuma área de trabalho! Ou seja, não há nenhum shell do Windows Explorer, com seu menu Iniciar, na barra de tarefas e outros recursos que você pode estar acostumado a ver. Tudo o que você precisa é de um prompt de comando, o que significa que você precisa que fazer a maior parte do trabalho de configuração de uma instalação Server Core digitando comandos uma por vez, que é lento, ou usando scripts e arquivos em lotes, que podem ajudar a acelerar e simplificar seu configurati nas tarefas por meio da automatização-los. Você também pode executar algumas tarefas de configuração inicial usando arquivos de resposta quando você executa uma instalação autônoma do Server Core. 

Para os administradores que são especialistas em usando ferramentas de linha de comando como Netsh.exe, Dfscmd.exe e Dnscmd.exe, configurando e gerenciando uma instalação Server Core podem ser fácil, até mesmo divertida. Para aqueles que não são especialistas em, no entanto, nem tudo está perdido. Você ainda pode usar as ferramentas padrão do MMC do Windows Server 2008 para gerenciar uma instalação Server Core. Basta usá-los em um sistema diferente, executando a instalação completa do Windows Server 2008 ou Windows Vista com Service Pack 1. 

Você aprenderá mais sobre como configurar e gerenciar uma instalação Server Core em capítulos 3 a 6 deste livro, enquanto os capítulos lidam com a forma de gerenciar funções de servidor específicas e outros componentes. Para saber mais sobre as várias ferramentas de linha de comando do Windows e como usá-las, existem dois bons recursos, consulte:
* A seção de referência de comandos do (biblioteca técnica do Windows Server 2008) 
* *Do Windows da linha de comando Administrator's Pocket Consultant* por William (Microsoft Press, 2008) 

Tabela 1 e 2 lista os principais aplicativos de GUI, junto com seus executáveis, que estão disponíveis em uma instalação Server Core.

**Tabela 1 e 2** aplicativos de GUI disponíveis em uma instalação Server Core
| Aplicativo de GUI | Executável com o caminho |
| -------------   | -------------       | 
| Prompt de comando | %WINDIR%\System32\Cmd.exe |
| Ferramenta de diagnóstico de suporte da Microsoft | %WINDIR%\System32\MSdt.exe |
| Bloco de notas | %WINDIR%\System32\Notepad.exe |
| Editor do registro | %WINDIR%\System32\Regedt32.exe |
| Informações do sistema | %WINDIR%\System32\MSinfo32.exe |
| Gerenciador de tarefas | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

Isso é uma pequena lista muito! Agora, aqui está uma lista de elementos de interface do usuário que não estão incluídos no Server Core:
* O shell da área de trabalho do Windows Explorer (Explorer.exe) e recursos de suporte, como temas 
* Todos os consoles do MMC 
* Todos os utilitários do painel de controle, com exceção de opções regionais e de idioma (intl. cpl) e a data e hora (timedate. cpl) 
* Todos os mecanismos de renderização Hypertext Markup Language (HTML), incluindo o Internet Explorer e a Ajuda HTML 
* Windows Mail 
* Windows Media Player 
* A maioria dos acessórios como Paint, Calculadora e o Wordpad

O .NET Framework também não está presente no Server Core, o que significa que não há suporte para código gerenciado em execução em uma instalação Server Core. Somente código nativo — código escrito usando interfaces de programação de aplicativo (APIs) do Windows — podem ser executados em Server Core. Em resumo, os aplicativos de GUI que dependem de um .NET Framework ou sobre o Explorer.exe shell não executará no Server Core. 

>[!NOTE]
>Como o Windows PowerShell requer o .NET Framework, é possível instalar o Windows PowerShell no Server Core. No entanto, você pode, gerenciar uma instalação Server Core remotamente usando o Windows PowerShell desde que você use apenas os comandos de PowerShell WMI.

##<a name="supported-server-roles"></a>Funções de servidor compatíveis 
Uma instalação Server Core inclui apenas um número limitado de funções de servidor em comparação com uma instalação completa do Windows Server 2008. Tabela 1 a 3 compara as funções disponíveis para instalações Full e Server Core do Windows Server 2008 Enterprise Edition. 

**Tabela 1 a 3** comparação das funções de servidor para instalações Full e Server Core do Windows Server 2008 Enterprise Edition
| Função do servidor  | Disponível na instalação completa  | Disponível em Server Core  |
| ------------- | :-------------: | :------------: |
| Serviços de certificados do Active Directory (AD CS)  | X |  |
| Serviços de Domínio do Active Directory (AD DS)  | X  | X |
| Serviços de Federação do Active Directory (AD FS)  | X  |  |
| AD LDS (Active Directory Lightweight Directory Services)  | X  | X |
| AD RMS (Active Director Rights Management Services)  | X  |  |
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
| Serviços de Implantação do Windows  | X |  |

Embora as funções disponíveis para Server Core geralmente são as mesmas, independentemente da arquitetura (x86 ou x64) e a edição do produto, existem algumas exceções:
* A função Hyper-V (de virtualização) está disponível somente se você comprar o Windows Server 2008 com a mídia do produto Hyper-V (Hyper-V está disponível somente para x64 versões). Se você não precisar dessa função, você pode adquirir Windows Server 2008 sem mídia do produto Hyper-V em vez disso. 
* A função Serviços de arquivo no Standard Edition está limitada a uma raiz do sistema de arquivos distribuído (DFS) autônomo e não oferece suporte a cross DFS-R (replicação). 
* Antes de instalar a função Serviços de mídia de Streaming no Server Core, você precisa baixar e instalar o pacote apropriado autônoma de atualização do Microsoft (arquivo. msu) para a arquitetura do seu servidor (x86 ou x64) do Microsoft Download Center.
* A função de servidor Web (IIS) não dão suporte ao ASP.NET. Isso ocorre porque não há suporte para o .NET Framework no Server Core, que limita o que você pode fazer com um servidor Web do Server Core. 

##<a name="supported-optional-features"></a>Suporte para recursos opcionais
Uma instalação Server Core também dá suporte a apenas um subconjunto limitado dos recursos disponíveis em uma instalação completa do Windows Server 2008. Tabela 1-4 compara os recursos disponíveis para instalações Full e Server Core do Windows Server 2008 Enterprise Edition.

**Tabela 1-4** comparação dos recursos para instalações Full e Server Core do Windows Server 2008 Enterprise Edition

| Recurso  | Disponível na instalação completa  | Disponível em Server Core  |
| ------------- | :-------------: | :------------: |
| Recursos do .NET framework 3.0  | X  |  |
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
| Vários caminhos e/s  | X  | X |
| Balanceamento de Carga de Rede  | X  | X |
| Protocolo PNRP  | X  |  |
| Quality Windows Audio-Video Experience  | X |  |
| Assistência Remota  | X  |  |
| Compactação Diferencial Remota | X  |  |
| Ferramentas de Administração de Servidor Remoto  | X  |  |
| Gerenciador de armazenamento removível | X  | X |
| RPC sobre Proxy HTTP | X  |  |
| Serviços TCP/IP Simples  | X  |  |
| Servidor SMTP  | X  |  |
| Serviços SMNP  | X  | X  |
| Gerenciador de Armazenamento para Redes SAN  | X  |  |
| Subsistema para Aplicativos Baseados em UNIX | X | X  |
| Cliente Telnet  | X | X  |
| Servidor Telnet  | X   |  |
| Cliente TFTP  | X   |  |
| Banco de Dados Interno do Windows  | X  |  |
| Windows PowerShell  | X  |  |
| Serviço de ativação de produto do Windows  | X   |  |
| Recursos de Backup do Windows Server  | X  | X  |
| Gerenciador de Recursos de Sistema do Windows  | X  |  |
| Servidor WINS  | X | X |
| Serviço de LAN sem fio | X  |  |

Novamente, há alguns pontos que você precisa saber sobre os recursos disponíveis no Server Core:
* Alguns recursos podem exigir hardware especial para funcionar corretamente (ou) no Server Core. Esses recursos incluem a criptografia de unidade de disco BitLocker, Clustering de Failover, e/s de vários caminhos, balanceamento de carga de rede e armazenamento removível. 
* Clustering de failover não está disponível na Standard Edition.

##<a name="server-core-architecture"></a>Arquitetura do Server Core
Se aprofundar no Server Core, vejamos rapidamente a arquitetura de uma instalação Server Core do Windows Server 2008, comparando-o com o de uma instalação completa. Em primeiro lugar, lembre-se de que o Server Core não é uma versão diferente do Windows Server 2008, mas simplesmente uma opção de instalação que você pode selecionar ao instalar o Windows Server 2008 em um sistema. Isso significa o seguinte:
* O kernel em uma instalação Server Core é o mesmo encontrado em uma instalação completa da mesma arquitetura de hardware (x86 ou x64) e a edição. 
* Se um binário estiver presente em uma instalação Server Core, uma instalação completa da mesma arquitetura de hardware (x86 ou x64) e edição tem a mesma versão do binário específico (com duas exceções discutidas posteriormente). 
* Se uma configuração específica (por exemplo, uma exceção de firewall específicas ou o tipo de inicialização de um serviço específico) tem uma determinada configuração de padrão em uma instalação Server Core, essa configuração é configurada exatamente da mesma forma em uma instalação completa do mesmo arquitetura de hardware (x86 ou x64) e edição.

Figura 1 a 3 mostra uma exibição simplificada da arquitetura de uma instalação completa e uma instalação Server Core do Windows Server 2008. A linha pontilhada indica a arquitetura do Server Core, enquanto todo o diagrama representa a arquitetura de uma instalação completa. 

O diagrama ilustra a arquitetura modular do Windows Server 2008, com o núcleo do servidor que está sendo construído após um subconjunto dos recursos principais de sistema operacional. Para a mesma edição e a arquitetura de hardware, todos os arquivos presentes em uma instalação limpa do Server Core também está presente em uma instalação completa, com exceção dos dois arquivos especiais (Scregedit.wsf e Oclist.exe), que estão presentes apenas no Server Core. Esses arquivos especiais foram incluídos no Server Core para simplificar a configuração inicial de uma instalação Server Core e a adição ou remoção de funções e componentes opcionais. 

![As arquiteturas de instalações do Server Core e completo](../media/what-is-server-core-2008/Fig1-3.png)

**Figura 1 a 3** as arquiteturas de instalações do Server Core e completo

##<a name="driver-support"></a>Suporte de driver
Diagrama da arquitetura do Server Core, mostrado na Figura 1 a 3, obviamente, é simplificado; uma coisa que ele não mostra é a diferença no suporte de driver de dispositivo entre Server Core e instalações completas. Uma instalação completa do Windows Server 2008 contém milhares de drivers nativos para diferentes tipos de dispositivos, que permitem que você instalar produtos em uma ampla variedade de diferentes configurações de hardware. (Sistemas operacionais de cliente como o Windows Vista incluir drivers ainda mais para dar suporte a dispositivos, como câmeras digitais e scanners que normalmente não são usados com os servidores.) 

Se um novo dispositivo é conectado a (ou instalado em) uma instalação completa do Windows Server 2008, o subsistema de Plug and Play (PnP) primeiro verifica se um driver de caixa de entrada para o dispositivo está presente. Se um driver compatível da caixa de entrada for encontrado, o subsistema de PnP automaticamente instala o driver e o dispositivo e opera. Em uma instalação completa do Windows Server 2008, um balão de notificação de pop-up pode ser exibida, indicando que o driver foi instalado e o dispositivo está pronto para uso. 

Em uma instalação Server Core, o processo de instalação do driver é o mesmo (o subsistema de PnP está presente no Server Core) com duas qualificações. Primeiro, o Server Core inclui apenas uma quantidade mínima de drivers de caixa de entrada e apenas para os seguintes tipos de dispositivos:
* Um driver de vídeo padrão da matriz de elementos gráficos de vídeo (VGA) 
* Drivers de dispositivos de armazenamento 
* Drivers para adaptadores de rede

Observe que, para cada uma das categorias de três dispositivo mostradas aqui, o Server Core inclui os mesmos drivers de caixa de entrada que são encontrados em uma instalação completa correspondente (para a mesma arquitetura de hardware). 

Além disso, quando o subsistema de PnP instala automaticamente um driver para um novo dispositivo, ele faz isso silenciosamente — nenhuma notificação de balão pop-up é exibida. Por que não? Porque não há nenhuma GUI no Server Core não há nenhuma barra de tarefas, portanto, não há nenhuma área de notificação na barra de tarefas! 

Então, o que fazer quando você adiciona a função Serviços de impressão para uma instalação Server Core e você deseja instalar uma impressora? Adicionar o driver de impressora manualmente ao servidor — Server Core não tem nenhum driver de impressão na caixa.

##<a name="service-footprint"></a>Superfície de serviço
Como o Server Core é uma instalação mínima, ele tem uma superfície menor do serviço de sistema que uma instalação completa correspondente da mesma arquitetura de hardware e a edição. Por exemplo, cerca de 75 serviços do sistema são instalados por padrão em uma instalação completa do Windows Server 2008, do que aproximadamente 50 são configurados para inicialização automática. Por outro lado, o núcleo do servidor tem apenas cerca de 70 serviços instalados por padrão e menos de 40 desses iniciar automaticamente. 

1 a 5 da tabela lista os serviços que são instalados por padrão em uma instalação Server Core, com o modo de inicialização e a conta usada por cada serviço.

**Tabela 1 a 5** serviços do sistema instalados por padrão no Server Core

| Nome do serviço  | Nome de exibição  | Modo de inicialização  | Conta  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Experiência de aplicativo  | Automático | LocalSystem |
| AppMgmt  | Gerenciamento de aplicativos  | Manual | LocalSystem |
| BFE | Mecanismo de filtragem básica  | Automático | LocalService |
| BITS | BITS  | Automático | LocalSystem |
| Navegador | Pesquisador de computadores  | Manual | LocalSystem |
| CertPropSvc | Propagação de certificado  | Manual | LocalSystem |
| COMSysApp  | Aplicativo de sistema COM+  | Manual | LocalSystem |
| CryptSvc  | Serviços de criptografia  | Automático | Network-Service |
| DcomLaunch  | Iniciador de processo do servidor DCOM  | Automático | LocalSystem |
| DHCP  | Cliente DHCP  | Automático | LocalService |
| DnsCache | Cliente DNS  | Automático | Network-Service |
| DPS  | Serviço de diretiva de diagnóstico  | Automático | LocalService |
| Log de eventos | Log de eventos do Windows  | Automático | LocalService |
| EventSystem  | Sistema de eventos COM+  | Automático | LocalService |
| FCRegSvc  | Serviço de registro de plataforma Microsoft Fibre Channel  | Manual | LocalService |
| gpsvc  | Cliente de Diretiva de Grupo  | Automático | LocalSystem |
| hidserv | Acesso de dispositivos de Interface Humana  | Manual | LocalSystem |
| hkmsvc  | Gerenciamento de certificado e chave de integridade  | Manual | LocalSystem |
| IKEEXT  | Módulos de Chave IKE e AuthIP IPsec  | Automático | LocalSystem |
| iphlpsvc  | Auxiliar de IP  | Automático | LocalSystem |
| KeyIso | Isolamento de chave CNG  | Manual | LocalSystem |
| KtmRm  | KtmRm para Coordenador de Transações Distribuídas  | Automático | Network-Service |
| LanmanServer  | Servidor  | Automático | LocalSystem |
| LanmanWorkstation  | Workstatione  | Automático | LocalService |
| lltdsvc  | Mapeador de descoberta de topologia de camada de link  | Manual | LocalService |
| lmhosts  | Auxiliar de NetBIOS TCP/IP  | Automático | LocalService |
| MpsSvc  | Firewall do Windows  | Automático | LocalService |
| MSDTC  | Coordenador de Transações Distribuídas  | Automático | Network-Service |
| MSiSCSI  | Serviço iniciador Microsoft iSCSI  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| napagent  | Agente de proteção de acesso à rede  | Manual | Network-Service |
| Netlogon  | Netlogon  | Manual | LocalSystem |
| netprofm  | Serviço de lista de rede  | Automático | LocalService |
| NlaSvc  | Reconhecimento de local de rede  | Automático | Network-Service |
| nsi  | Serviço de Interface de rede Store  | Automático | LocalService |
| CCP  | Alertas e Logs de desempenho  | Manual | LocalService |
| PlugPlay  | Plug and Play  | Automático | LocalSystem |
| PolicyAgent  | Agente de Política IPsec  | Automático | Network-Service |
| ProfSvc  | Serviço de perfil do usuário  | Automático | LocalSystem |
| ProtectedStorage  | Armazenamento protegido  | Manual | LocalSystem |
| RemoteRegistry  | Registro Remoto  | Automático | LocalService |
| RpcSs  | RPC (Chamada de Procedimento Remoto)  | Automático | Network-Service |
| RSoPProv | Conjunto de provedor de diretivas resultante  | Manual | LocalSystem |
| sacsvr  | Auxiliar do Console de administração especial  | Manual | LocalSystem |
| SamSs  | Gerenciador de contas de segurança  | Automático | LocalSystem |
| SCardSvr | Cartão inteligente  | Manual | LocalService |
| Agendamento | Agendador de Tarefas  | Automático | LocalSystem |
| SCPolicySvc | Política de remoção de cartão inteligente  | Manual | LocalSystem |
| seclogon | Logon secundário  | Automático | LocalSystem |
| SENS | Serviço de notificação de eventos do sistema  | Automático | LocalSystem |
| SessionEnv | Configuração dos Serviços de Terminal  | Manual | LocalSystem |
| slsvc  | Licenciamento de software | Automático | Network-Service |
| SNMPTRAP  | Interceptação SNMP  | Manual | LocalService |
| swprv  | Provedor de cópias de sombra de Software da Microsoft | Manual | LocalSystem |
| TBS | Serviços de Base do TPM  | Manual | LocalService |
| TermService  | Serviços de Terminal | Automático | Network-Service |
| TrustedInstaller | Instalador de módulos do Windows  | Automático | LocalSystem |
| UmRdpService | Redirecionador de portas do modo do usuário de serviços de terminal  | Manual | LocalSystem |
| vds | Disco virtual  | Manual | LocalSystem |
| VSS | Cópia de sombra de volume  | Manual | LocalSystem |
| W32Time | Tempo do Windows  | Automático | LocalService |
| WcsPlugInService  | Sistema de cores do Windows  | Manual | LocalService |
| WdiServiceHost  | Host de serviço de diagnóstico  | Manual | LocalService |
| WdiSystemHost  | Host do sistema de diagnóstico  | Manual | LocalSystem |
| Wecsvc | Coletor de Eventos do Windows  | Manual | Network-Service |
| WinHttpAuto-ProxySvc  | Serviço de descoberta automática de Proxy da Web do WinHTTP  | Automático | LocalService |
| Winmgmt | Instrumentação de Gerenciamento do Windows | Automático | LocalSystem |
| WinRM  | Windows Remote Management (WS-Management) | Automático | Network-Service |
| wmiApSrv  | Adaptador de desempenho WMI  | Manual | LocalSystem |
| wuauserv | Windows Update | Automático | LocalSystem |