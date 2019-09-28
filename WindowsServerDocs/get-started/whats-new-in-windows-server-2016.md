---
title: Novidades no Windows Server 2016
description: Quais são os novos recursos de computação, identidade, gerenciamento, automação, rede, segurança, armazenamento.
ms.prod: windows-server
ms.date: 05/21/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 2827f332-44d4-4785-8b13-98429087dcc7
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 76cfd0f0cca18fb072883a9e14fae420516bd329
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391365"
---
# <a name="whats-new-in-windows-server-2016"></a>Novidades no Windows Server 2016

>Aplica-se a: Windows Server 2016

![Ícone mostrando um jornal](media/whats-new.png) Para saber mais sobre os recursos mais recentes do Windows, confira [Novidades no Windows Server](whats-new-in-windows-server.md). O conteúdo desta seção descreve as novidades e as alterações no Windows Server&reg; 2016. Os novos recursos e alterações listados aqui são os que têm maior probabilidade de ter um impacto maior ao trabalhar com esta versão.

## <a name="computevirtualizationvirtualizationmd"></a>[Compute](../virtualization/virtualization.md)

A área de virtualização inclui produtos e recursos de virtualização para profissionais de TI projetarem, implantarem e manterem o Windows Server.  

### <a name="general"></a>Geral  
Máquinas físicas e virtuais beneficiam-se de maior precisão de tempo devido a melhorias no tempo de Win32 e nos Serviços de sincronização de tempo do Hyper-V. O Windows Server pode hospedar serviços que são compatíveis com futuras regulamentações que exigem uma precisão de 1ms em relação ao UTC.  

### <a name="hyper-v"></a>Hyper-V  
-   [Novidades no Hyper-V do Windows Server 2016](../virtualization/hyper-v/What-s-new-in-Hyper-V-on-Windows.md). Este tópico explica as funcionalidades novas e alteradas da função Hyper-V no Windows Server 2016, Hyper-V cliente em execução no Windows 10 e Microsoft Hyper-V Server 2016.  

-   [Contêineres do Windows](https://msdn.microsoft.com/virtualization/windowscontainers/containers_welcome):  A compatibilidade com contêiner do Windows Server 2016 adiciona melhorias de desempenho, gerenciamento de rede simplificado e compatibilidade com contêineres do Windows no Windows 10. Para obter informações adicionais sobre contêineres, confira [Contêineres: Docker, Windows e tendências](https://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/).  

### <a name="nano-server"></a>Nano Server  
Novidades no [Nano Server](getting-started-with-nano-server.md). O Nano Server agora tem um módulo atualizado para a criação de imagens do Nano Server, inclusive mais separação da funcionalidade de máquina virtual de host e convidado físico, assim como suporte para edições diferentes do Windows Server.   

Também há aprimoramentos para o Console de Recuperação, incluindo a separação das regras de firewall de entrada e saída, bem como a capacidade para reparar a configuração do WinRM.  

### <a name="shielded-virtual-machines"></a>Máquinas virtuais blindadas  
O Windows Server 2016 fornece uma nova máquina virtual blindada com base em Hyper-V para proteção de qualquer máquina virtual de 2ª geração de uma malha comprometida. Entre os recursos introduzidos no Windows Server 2016 estão os seguintes:  

- Novo modo de "Suporte de criptografia" que oferece mais proteções do que para uma máquina virtual comum, mas menos do que o modo "Blindado", embora ainda suporte vTPM, criptografia de disco, criptografia de tráfego da migração ao vivo e outros recursos, incluindo conveniências de administração de malha direta como conexões de console da máquina virtual e Powershell Direct.  

- Suporte completo para a conversão de máquinas virtuais não blindadas de 2ª geração para máquinas virtuais existentes, incluindo criptografia de disco automatizada.

- O Gerenciador de máquina Virtual do Hyper-V agora pode exibir as malhas nas quais um virtual blindado está autorizado a ser executado, fornecendo uma maneira para o administrador da malha abrir o protetor de chave da máquina virtual blindada (KP) e exibir as malhas nas quais tem permissão de ser executado.  

- Você pode alternar os modos de Atestado em um Serviço Guardião de Host em execução. Agora você pode alternar rapidamente entre o atestado menos seguro, mas mais simples baseado no Active Directory e o atestado baseado em TPM.  

- As ferramentas de diagnóstico de ponta a ponta baseadas no Windows PowerShell que são capazes de detectar erros ou configurações incorretas nos hosts protegidos do Hyper-V e no Serviço Guardião de Host.  

- Um ambiente de recuperação que oferece um meio para solucionar problemas e reparar com segurança máquinas virtuais blindadas dentro da malha em que são normalmente executadas além de oferecer o mesmo nível de proteção que a própria máquina virtual blindada.

- Suporte ao Serviço Guardião de Host para Active Directory seguro existente – você pode direcionar o Serviço Guardião de Host para usar uma floresta existente do Active Directory como seu Active Directory, em vez de criar sua própria instância do Active Directory

Para obter mais detalhes e instruções para trabalhar com máquinas virtuais blindadas, consulte [Guia de validação de VMs blindadas e malha protegida para Windows Server 2016 (TPM)](https://aka.ms/shieldedvms).  

## <a name="identity-and-accessidentityidentity-and-accessmd"></a>[Identidade e acesso](../identity/Identity-and-Access.md)  
Novos recursos na identidade melhoram a capacidade para as organizações protegerem ambientes do Active Directory e ajuda a migrar para implantações apenas na nuvem e implantações híbridas, onde alguns aplicativos e serviços são hospedados na nuvem e outros são hospedados no local.  

### <a name="active-directory-certificate-services"></a>Serviços de Certificados do Active Directory  
O AD CS (Serviços de Certificados do Active Directory) no Windows Server 2016 aumentam a compatibilidade com o atestado de chaves do TPM: agora você pode usar o KSP de cartão inteligente para atestado de chave e dispositivos que não ingressaram no domínio agora podem usar o registro de NDES para obter certificados que podem ser atestados para chaves em um TPM.  

### <a name="active-directory-domain-services"></a>Active Directory Domain Services  
Os Active Directory Domain Services incluem aprimoramentos para ajudar as organizações a proteger ambientes do Active Directory e fornecer experiências de gerenciamento de identidade melhores para dispositivos corporativos e pessoais. Para saber mais, confira [Novidades no AD DS (Active Directory Domain Services) no Windows Server 2016](../identity/whats-new-active-directory-domain-services.md).   

### <a name="active-directory-federation-services"></a>Serviços de Federação do Active Directory (AD FS)  
Novidades nos Serviços de Federação do Active Directory. O AD FS (Serviços de Federação do Active Directory) no Windows Server 2016 inclui novos recursos que permitem que você configure o AD FS para autenticar usuários armazenados em diretórios LDAP (Lightweight Directory Access Protocol). Para obter mais informações, confira [Novidades no AD FS do Windows Server 2016](../identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server.md).  

### <a name="web-application-proxy"></a>Proxy de aplicativo Web  
A versão mais recente do Proxy de aplicativo Web se concentra em novos recursos que habilitam a publicação e a pré-autenticação para mais aplicativos e experiência do usuário aprimorada. Confira a lista completa dos novos recursos que incluem a pré-autenticação para aplicativos de cliente avançado como o Exchange ActiveSync e domínios curinga para a publicação mais fácil de aplicativos do SharePoint. Para obter mais informações, confira [Proxy de aplicativo Web no Windows Server 2016](../remote/remote-access/web-application-proxy/web-application-proxy-windows-server.md).  

##  <a name="administrationadministrationmanage-windows-servermd"></a>[Administração](../administration/manage-windows-server.md)  
A área de gerenciamento e automação concentra-se na ferramenta e nas informações de referência para profissionais de TI que desejam executar e gerenciar o Windows Server 2016, incluindo o Windows PowerShell.

O Windows PowerShell 5.1 contém novos recursos significativos, incluindo suporte para o desenvolvimento com classes e novos recursos de segurança, que ampliam seu uso, melhoram sua usabilidade e permitem controlar e gerenciar ambientes baseados em Windows de forma mais fácil e abrangente. Confira [Novos cenários e recursos no WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/scenarios-features) para obter mais detalhes.

Novas adições para o Windows Server 2016 incluem: a capacidade de executar o PowerShell.exe localmente no Nano Server (não mais apenas remoto), novos cmdlets de usuários locais e grupos para substituir a GUI, adicionado o suporte à depuração do PowerShell e adicionado o suporte no Nano Server para o log de segurança e transcrição e JEA.

Veja alguns outros novos recursos de administração:

### <a name="powershell-desired-state-configuration-dsc-in-windows-management-framework-wmf-5"></a>DSC (Configuração de estado desejado) do PowerShell no WMF (Windows Management Framework) 5
O Windows Management Framework 5 inclui atualizações para DSC (Configuração de estado desejado) do Windows PowerShell, Windows Remote Management (WinRM) e Windows Management Instrumentation (WMI).

Para saber mais sobre como testar os recursos de DSC do Windows Management Framework 5, confira a série de postagens de blog discutidas em [Validar recursos do PowerShell DSC](https://blogs.msdn.microsoft.com/powershell/2015/07/06/validate-features-of-powershell-dsc/). Para baixar, confira [Windows Management Framework 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### <a name="packagemanagement-unified-package-management-for-software-discovery-installation-and-inventory"></a>Gerenciamento de pacote unificado PackageManagement para descoberta de software, instalação e inventário
O Windows Server 2016 e o Windows 10 inclui um novo recurso PackageManagement (anteriormente chamado OneGet) que permite que os profissionais de TI ou DevOps automatizem a detecção de software, instalação e inventário (SDII), local ou remotamente, independentemente da tecnologia de instalador e onde o software está localizado. 

Para saber mais, confira [https://github.com/OneGet/oneget/wiki](https://github.com/OneGet/oneget/wiki).

### <a name="powershell-enhancements-to-assist-digital-forensics-and-help-reduce-security-breaches"></a>Aprimoramentos do PowerShell para auxiliar o trabalho forense digital e ajudar a reduzir as violações de segurança
Para ajudar a equipe responsável pela investigação de sistemas comprometidos - às vezes conhecida como "equipe azul" - adicionamos registro em log do PowerShell e outras funcionalidades de computação forense digital, e adicionamos funcionalidades para ajudar a reduzir as vulnerabilidades de scripts, como o PowerShell restrito e APIs CodeGeneration protegidas.

Para obter mais informações, confira [PowerShell ♥ a equipe azul](https://blogs.msdn.microsoft.com/powershell/2015/06/09/powershell-the-blue-team/).

## <a name="networkingnetworkingnetworkingmd"></a>[Rede](../networking/Networking.md)  
Esta área aborda os produtos e recursos de rede para o profissional de TI desenvolver, implantar e manter o Windows Server 2016.  

### <a name="software-defined-networking"></a>Redes definidas por software
Você pode agora espelhar e rotear o tráfego para soluções de virtualização novas ou existentes. Junto com um firewall distribuído e grupos de segurança de rede, isso permite que você segmente e proteja dinamicamente as cargas de trabalho de maneira semelhante ao Azure. Em segundo lugar, você pode implantar e gerenciar toda a pilha de redes definidas pelo software (SDN) usando o System Center Virtual Machine Manager. Por fim, você pode usar o Docker para gerenciar o sistema de rede do contêiner do Windows Server e associar políticas de SDN não apenas a máquinas virtuais, mas também aos contêineres. Para obter mais informações, confira [Planejar a infraestrutura de rede definida por software](../networking/sdn/plan/plan-a-software-defined-network-infrastructure.md).

### <a name="tcp-performance-improvements"></a>Aprimoramentos de desempenho de TCP
A ICW (Janela de Congestionamento Inicial) padrão aumentou de quatro para 10 e a TFO (Abertura Rápida de TCP) foi implementada. A TFO reduz a quantidade de tempo necessária para estabelecer uma conexão TCP, e o ICW maior permite que objetos maiores sejam transferido no pico inicial. Essa combinação pode reduzir consideravelmente o tempo necessário para transferir um objeto da Internet entre o cliente e a nuvem.

Para melhorar o comportamento do TCP durante a recuperação de perda de pacotes, implementamos o TLP (TCP Tail Loss Probe) e o RACK (Confirmação recente). O TLP ajuda a converter RTOs (Tempo limite de retransmissão) em Recuperações rápidas, e o RACK reduz o tempo necessário para Recuperação rápida a fim de retransmitir um pacote perdido. 

## <a name="security-and-assurancesecuritysecurity-and-assurancemd"></a>[Segurança e garantia](../security/Security-and-Assurance.md)  
Inclui recursos e soluções de segurança para profissionais de TI implantarem em seu data center e ambiente de nuvem. Para obter informações sobre a segurança no Windows Server 2016 em geral, confira [Garantia e segurança](../security/Security-and-Assurance.md).  

### <a name="just-enough-administration"></a>Administração Just Enough  
A Administração Just Enough no Windows Server 2016 é a tecnologia de segurança que permite a administração delegada para qualquer coisa que possa ser gerenciada com o Windows PowerShell. Os recursos incluem o suporte para execução sob uma identidade de rede, conexão através do PowerShell Direct, cópia de arquivos de ou para pontos de extremidade de JEA e configuração do console do PowerShell para inicialização em um contexto de JEA por padrão. Para obter mais detalhes, consulte [JEA no GitHub](https://aka.ms/JEA).

### <a name="credential-guard"></a>Credential Guard
O Credential Guard usa segurança baseada em virtualização para isolar segredos para que apenas o software de sistema privilegiado possa acessá-los. confira [Proteger as credenciais de domínio derivadas com o Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).

###  <a name="remote-credential-guard"></a>Credential Guard remoto
O Credential Guard inclui suporte para sessões RDP, para que as credenciais do usuário permaneçam no lado do cliente e não sejam expostas no lado do servidor. Também fornece Logon único para Área de Trabalho Remota. Confira [Proteger credenciais de domínio derivadas com o Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).   

### <a name="device-guard-code-integrity"></a>Device Guard (Integridade de código)
O Device Guard fornece KMCI (integridade de código no modo kernel) e UMCI (integridade de código no modo de usuário) criando políticas que especificam qual código pode ser executado no servidor. Confira [Introdução ao Windows Defender Device Guard: políticas de integridade de código e segurança baseada em virtualização](https://docs.microsoft.com/windows/device-security/device-guard/introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies).


### <a name="windows-defender"></a>Windows Defender  
[Visão geral do Windows Defender para Windows Server 2016](../security/windows-defender/windows-defender-overview-windows-server.md). O Windows Server Antimalware está instalado e habilitado por padrão no Windows Server 2016, mas a interface do usuário para o Windows Server Antimalware não está instalada. No entanto, o Windows Server Antimalware atualizará as definições de antimalware e protegerá o computador sem a interface do usuário. Se você precisar da interface do usuário do Windows Server Antimalware, pode instalá-lo após a instalação do sistema operacional usando o Assistente de Adição de Funções e Recursos.

### <a name="control-flow-guard"></a>Proteção de Fluxo de Controle
O CFG (Proteção de Fluxo de Controle) é um recurso de segurança de plataforma que foi criado para combater as vulnerabilidades de corrupção da memória. Para saber mais, confira [Control Flow Guard (Proteção de Fluxo de Controle)](https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx).


## <a name="storagestoragestoragemd"></a>[Armazenamento](../storage/storage.md)

O armazenamento no Windows Server 2016 inclui novos recursos e aprimoramentos de armazenamento definidos pelo software, bem como para servidores de arquivos tradicionais. Veja abaixo alguns dos recursos novos. Para conferir outros aprimoramentos e detalhes, confira [Novidades no Armazenamento no Windows Server 2016](../storage/whats-new-in-storage.md).

### <a name="storage-spaces-direct"></a>Espaços de Armazenamento Diretos

Os Espaços de Armazenamento Direto habilitam a criação de armazenamento altamente disponível e escalonável usando servidores com armazenamento local. Ele simplifica a implantação e o gerenciamento de sistemas de armazenamento definidos por software e desbloqueia o uso de novas classes de dispositivos de disco, como dispositivos de disco SATA SSD e NVMe, que anteriormente não eram possíveis com Espaços de Armazenamento clusterizados com discos compartilhados.

Para saber mais, confira [Espaços de Armazenamento Diretos](../storage/storage-spaces/storage-spaces-direct-overview.md).

### <a name="storage-replica"></a>Réplica de Armazenamento

A Réplica de Armazenamento habilita a replicação síncrona em nível de bloco independente de armazenamento entre servidores ou clusters para recuperação de desastre, bem como a expansão de um cluster de failover entre sites. A replicação síncrona habilita o espelhamento de dados em locais físicos com volumes consistentes com falha para garantir perda zero de dados no nível do sistema de arquivos. A replicação assíncrona permite a extensão de site além das dimensões metropolitanas com a possibilidade de perda de dados.

Para saber mais, confira [Réplica de armazenamento](../storage/storage-replica/storage-replica-overview.md).

### <a name="storage-quality-of-service-qos"></a>QoS (Qualidade de armazenamento do serviço)

Agora você pode usar QoS (qualidade do serviço de armazenamento) para monitorar centralmente e de ponta a ponta o desempenho do armazenamento e criar políticas usando o clusters do Hyper-V e CSV no Windows Server 2016.

Para saber mais, confira [Qualidade do serviço de armazenamento](../storage/storage-qos/storage-qos-overview.md).

## <a name="failover-clusteringfailover-clusteringwhats-new-in-failover-clusteringmd"></a>[Clustering de failover](../failover-clustering/whats-new-in-failover-clustering.md)

O Windows Server 2016 inclui diversos recursos novos e aprimoramentos para vários servidores agrupados em um único cluster tolerante a falhas usando o recurso de Clustering de Failover. Algumas das adições estão listadas abaixo; para obter uma lista mais completa, confira [Novidades em Clustering de Failover no Windows Server 2016](../failover-clustering/whats-new-in-failover-clustering.md).

### <a name="cluster-operating-system-rolling-upgrade"></a>Atualização sem interrupção do sistema operacional do cluster

Atualização sem interrupção do sistema operacional do cluster permite ao administrador atualizar o sistema operacional dos nós do cluster do Windows Server 2012 R2 para o Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho de Servidor de Arquivos de Escalabilidade Horizontal. Usando esse recurso, as penalidades de tempo de inatividade em SLAs (Contratos de Nível de Serviço) podem ser evitadas.

Para saber mais, confira [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).

### <a name="cloud-witness"></a>Testemunha de nuvem

Testemunha de nuvem é um novo tipo de testemunha de quorum de Cluster de Failover no Windows Server 2016 que utiliza o Microsoft Azure como o ponto de arbitragem. A Testemunha de nuvem, como qualquer outra testemunha de quorum, obtém um voto e pode participar dos cálculos de quorum. Você pode configurar a testemunha de nuvem como uma testemunha de quorum usando o Configurar um Assistente para Quorum do Cluster.

Para saber mais, confira [Implantar testemunha de nuvem](../failover-clustering/deploy-cloud-witness.md).

### <a name="health-service"></a>Serviço de Integridade

O Serviço de Integridade aprimorar monitoramento de rotina, as operações e a experiência de manutenção de recursos de cluster em Espaços de Armazenamento Diretos.

Para saber mais, confira [Serviço de Integridade](../failover-clustering/health-service-overview.md).

## <a name="application-development"></a>Desenvolvimento de aplicativo

### <a name="internet-information-services-iis-100"></a>IIS (Serviços de Informações da Internet) 10.0
Os novos recursos fornecidos pelo servidor Web IIS 10.0 no Windows Server 2016 incluem:

- Suporte para o protocolo HTTP/2 na pilha de rede e integrado com o IIS 10.0, permitindo que os sites IIS 10.0 atendam automaticamente solicitações HTTP/2 para configurações compatíveis. Isso permite vários aprimoramentos sobre HTTP/1.1 como reutilização mais eficiente de conexões e menor latência, melhorando os tempos de carregamento de páginas da Web. 
- Capacidade de executar e gerenciar o IIS 10.0 no servidor Nano. Confira [IIS no servidor Nano](iis-on-nano-server.md).
- Suporte para Cabeçalhos de Host com Caracteres Curinga, permitindo que os administradores configurem um servidor Web para um domínio e, em seguida, que o servidor Web atenda solicitações de qualquer subdomínio.
- Um novo módulo de PowerShell (IISAdministration) para gerenciar o IIS. 

Para mais detalhes, confira [IIS](https://iis.net/learn).

### <a name="distributed-transaction-coordinator-msdtc"></a>Coordenador de Transações Distribuídas (MSDTC)
Três novos recursos são adicionados no Microsoft Windows 10 e Windows Server 2016:

- Uma nova interface para Reingressar do Gerenciador de Recursos pode ser usada por um gerenciador de recursos para determinar o resultado de uma transação em dúvida, depois que um banco de dados for reiniciado devido a um erro. Confira [IResourceManagerRejoinable::Rejoin](https://msdn.microsoft.com/library/mt203799(v=vs.85).aspx) para obter detalhes.

- O limite de nome DSN é ampliado de 256 bytes para 3072 bytes. Confira [IDtcToXaHelperFactory::Create](https://msdn.microsoft.com/library/ms686861(v=vs.85).aspx), [IDtcToXaHelperSinglePipe::XARMCreate](https://msdn.microsoft.com/library/ms679248(v=vs.85).aspx) ou [IDtcToXaMapper::RequestNewResourceManager](https://msdn.microsoft.com/library/ms680310(v=vs.85).aspx) para obter detalhes.

- Rastreamento aprimorado permitindo que você defina uma chave do Registro para incluir um caminho de arquivo de imagem no nome do arquivo de log de rastreamento para poder determinar qual o arquivo de log de rastreamento verificar. Confira [Como habilitar o rastreamento de diagnóstico para MS DTC em um computador baseado no Windows](https://support.microsoft.com/en-us/kb/926099) para obter detalhes sobre como configurar o rastreamento para MSDTC.



## <a name="see-also"></a>Consulte também  
-   [Notas de versão: Problemas importantes no Windows Server 2016](Windows-Server-2016-GA-Release-Notes.md)  

