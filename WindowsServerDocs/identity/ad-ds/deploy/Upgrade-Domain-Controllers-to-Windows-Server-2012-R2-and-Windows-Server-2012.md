---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: "Atualizar controladores de domínio para o Windows Server 2012 R2 e Windows Server 2012"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e317c5a939d417bac844c4080223d7b5e0eec149
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Atualizar controladores de domínio para o Windows Server 2012 R2 e Windows Server 2012

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece informações básicas sobre serviços de domínio do Active Directory no Windows Server 2012 R2 e Windows Server 2012 e explica o processo para atualizar os controladores de domínio do Windows Server 2008 ou Windows Server 2008 R2.  
  
-   [Etapas de atualização de controlador de domínio](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradeWorkflow)  
  
-   [O que há de novo no Windows Server 2012?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewEight)  
  
-   [O que há de novo no AD DS no Windows Server 2012 R2?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_NewWS2012R2)  
  
-   [O que há de novo no AD DS no Windows Server 2012?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewAD)  
  
-   [Alterações de instalação de função do AD DS server](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_InstallationChanges)  
  
-   [Recursos preteridos e as mudanças de comportamento relacionadas ao AD DS no Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)  
  
-   [Requisitos de sistema operacional](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_SysReqs)  
  
-   [Suporte a caminhos de atualização in-loco](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradePaths)  
  
-   [Requisitos e recursos de nível funcionais](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_FunctionalLevels)  
  
-   [Interoperabilidade do AD DS com outras funções de servidor e sistemas operacionais Windows](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_ServerRoles)  
  
-   [Funções mestre de operações](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_OpsMasters)  
  
-   [Virtualização de controladores de domínio que executam o Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Virtual)  
  
-   [Administração de servidores do Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Admin)  
  
-   [Compatibilidade de aplicativos](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)  
  
-   [Problemas conhecidos](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_KnownIssues)  
  
## <a name="BKMK_UpgradeWorkflow"></a>Etapas de atualização de controlador de domínio  
A maneira recomendada de um domínio de atualização é promover controladores de domínio que executam versões mais recentes do Windows Server e rebaixar controladores de domínio mais antigos conforme necessário. Esse método é preferível atualizando o sistema operacional de um controlador de domínio existente. Essa lista abrange as etapas gerais a serem seguidas antes de promover um controlador de domínio que executa uma versão mais recente do Windows Server:  
  
1.  Verifique se o servidor de destino atende aos [requisitos do sistema](https://technet.microsoft.com/library/dn303418.aspx).  
  
2.  Verifique se [compatibilidade de aplicativos](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).  
  
3.  Verifique se as configurações de segurança. Para obter mais informações, consulte [obsoleto recursos e as mudanças de comportamento relacionam ao AD DS no Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) e [segura as configurações padrão no Windows Server 2008 e Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault).  
  
4.  Verifique a conectividade para o servidor de destino do computador onde você pretende executar a instalação.  
  
5.  Verifique a disponibilidade das funções de mestre de operações necessárias:  
  
    -   Para instalar o primeiro controlador de domínio que executa o Windows Server 2012 em uma floresta e domínio existente, a máquina em que você executar a instalação precisa ter conectividade com o mestre de esquema para executar adprep /forestprep e o mestre de infraestrutura para executar adprep /domainprep.  
  
    -   Para instalar o primeiro controlador de domínio em um domínio em que o esquema de floresta já foi estendido, você só precisa de conectividade com o mestre de infraestrutura.  
  
    -   Para instalar ou remover um domínio em uma floresta existente, você precisa de conectividade com o mestre de nomeação de domínio.  
  
    -   Qualquer instalação de controlador de domínio também exige conectividade com o mestre de RID.  
  
    -   Se você estiver instalando o primeiro controlador de domínio somente leitura em uma floresta existente, você precisa conectividade com o mestre de infraestrutura para cada partição de diretório do aplicativo, também conhecido como um contexto de nomenclatura não do domínio ou NDNC.  
  
6.  Certifique-se de fornecer as credenciais necessárias para executar a instalação do AD DS.  
  
    |Ação de instalação|Requisitos de credenciais|  
    |-----------------------|---------------------------|  
    |Instalar uma nova floresta|Administrador local no servidor de destino|  
    |Instalar um novo domínio em uma floresta existente|Administradores corporativos|  
    |Instalar um controlador de domínio adicional em um domínio existente|Administradores de domínio|  
    |Executar adprep /forestprep|Administradores de esquema, administradores de empresa e administradores de domínio|  
    |Executar adprep /domainprep|Administradores de domínio|  
    |Executar adprep /domainprep /gpprep|Administradores de domínio|  
    |Executar adprep /rodcprep|Administradores corporativos|  
  
    Você pode delegar permissões para instalar o AD DS. Para obter mais informações, consulte [tarefas de gerenciamento de instalação](https://technet.microsoft.com/library/cc773327(WS.10).aspx).  
  
Instruções de etapas-a-passo para promover novos e controladores de domínio do Windows Server 2012 réplica usando os cmdlets do Windows PowerShell e Gerenciador do servidor podem ser encontradas nos seguintes links:  
  
-   [Instalar os serviços de domínio do Active Directory (nível 100)](https://technet.microsoft.com/library/hh472162.aspx)  
  
-   [Instalar uma nova floresta Windows Server 2012 do Active Directory (nível 200)](https://technet.microsoft.com/library/jj574166.aspx)  
  
-   [Instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente (nível 200)](https://technet.microsoft.com/library/jj574134.aspx)  
  
-   [Instalar um novo filho do Windows Server 2012 do Active Directory ou o domínio de árvore (nível 200)](https://technet.microsoft.com/library/jj574105.aspx)  
  
-   [Instalar um controlador de domínio somente leitura do servidor 2012 Active Directory do Windows (RODC) (nível 200)](https://technet.microsoft.com/library/jj574152.aspx)  
  
-   [Guia passo a passo de configuração o Windows Server 2012 controlador de domínio (en-US)](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  
  
## <a name="BKMK_WhatsNewEight"></a>O que há de novo no Windows Server 2012?  
Novos recursos listados por função de servidor e área de tecnologia estão listados na tabela a seguir. Para mais white papers, demonstrações em vídeos e apresentações sobre outros recursos no Windows Server 2012, consulte [servidor e a plataforma de nuvem](https://www.microsoft.com/server-cloud/default.aspx).  
  
||||  
|-|-|-|  
|[Serviços de certificados do Active Directory (AD CS)](https://technet.microsoft.com/library/hh831373.aspx)|[Active Directory Rights Management Services (AD RMS)](https://technet.microsoft.com/library/hh831554.aspx)|[Criptografia de unidade de disco BitLocker](https://technet.microsoft.com/library/hh831412.aspx)|  
|[BranchCache](https://technet.microsoft.com/library/jj127252.aspx)|[Dynamic Host Configuration (protocolo DHCP)](https://technet.microsoft.com/library/jj200226.aspx)|[Sistema de nomes de domínio (DNS)](https://technet.microsoft.com/library/jj200224.aspx)|  
|[Cluster de failover](https://technet.microsoft.com/library/hh831414.aspx)|[Gerenciador de recursos do servidor de arquivos](https://technet.microsoft.com/library/hh831746.aspx)|[Política de grupo](https://technet.microsoft.com/library/jj574108.aspx)|  
|[Hyper-V](https://technet.microsoft.com/library/hh831410.aspx)|[Gerenciamento de endereço IP (IPAM)](https://technet.microsoft.com/library/jj200214.aspx)|[Autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx)|  
|[Gerenciado contas de serviço](https://technet.microsoft.com/library/hh831451.aspx)|[Rede](https://technet.microsoft.com/library/jj200215.aspx)|[Serviços de área de trabalho remota](https://technet.microsoft.com/library/hh831527.aspx)|  
|[Auditoria de segurança](https://technet.microsoft.com/library/hh849638.aspx)|[Gerenciador do servidor](https://blogs.technet.com/b/servermanager/archive/2012/06/27/server-manager-power-of-many-simplicity-of-one.aspx)|[Cartões inteligentes](https://technet.microsoft.com/library/hh849637.aspx)|  
|[TLS/SSL (Schannel SSP)](https://technet.microsoft.com/library/hh831771.aspx)|[Serviços de implantação do Windows](https://technet.microsoft.com/library/hh974416.aspx)|[Windows PowerShell 3.0](https://technet.microsoft.com/library/hh857339)|  
  
### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a>Manutenção automática e alterações reiniciar comportamento depois que as atualizações são aplicadas pelo Windows Update  
Antes do lançamento do Windows 8, Windows Update gerenciado sua própria agenda interna para verificar se há atualizações e para baixar e instalá-los. Ele necessário que o agente de atualização do Windows foi sempre em execução em segundo plano, consumindo memória e outros recursos do sistema.  
  
Windows 8 e Windows Server 2012 apresentam um novo recurso chamado [manutenção automática](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). A manutenção automática consolida muitos recursos diferentes que cada usado para gerenciar seu próprio agendamento e execução lógica. Essa consolidação permite todos esses componentes usam muito menos recursos do sistema, funcionam de forma consistente, respeitar a nova [modo de espera conectado](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) estado novos tipos de dispositivo e consuma menos bateria em dispositivos portáteis.  
  
Como o Windows Update é uma parte da manutenção automática no Windows 8 e Windows Server 2012, sua própria agenda interna para definir um dia e hora para instalar as atualizações não é mais eficaz. Para ajudar a garantir consistente e previsível reiniciar o comportamento para todos os dispositivos e computadores em sua empresa, incluindo aqueles que executar o Windows 8 e Windows Server 2012, consulte Microsoft KB artigo [2885694](https://support.microsoft.com/kb/2885694) (ou consulte cumulativo de outubro de 2013 [2883201](https://support.microsoft.com/kb/2883201)), em seguida, definir configurações de política descritas na postagem do blog WSUS [habilitando uma experiência mais previsível do Windows Update para Windows 8 e Windows Server 2012 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx).  
  

## <a name="BKMK_NewWS2012R2"></a>O que há de novo no AD DS no Windows Server 2012 R2?  
A tabela a seguir resume os novos recursos para o AD DS no Windows Server 2012 R2, com um link para obter mais informações detalhadas onde ele está disponível. Para obter uma explicação mais detalhada de alguns recursos, incluindo suas necessidades, consulte [What's New no Active Directory no Windows Server 2012 R2](https://technet.microsoft.com/library/dn268294.aspx).  
  
|Recurso|Descrição|  
|-----------|---------------|  
|[Ingresso no local de trabalho](https://technet.microsoft.com/library/dn280945.aspx)|Permite que os operadores de informações participar de seus dispositivos pessoais com sua empresa para acessar serviços e recursos da empresa.|  
|[Proxy de aplicativo Web](https://technet.microsoft.com/library/dn280942.aspx)|Fornece acesso ao aplicativo da web usando um novo serviço de função de acesso remoto.|  
|[Serviços de Federação do Active Directory](https://technet.microsoft.com/library/hh831502.aspx)|AD FS tem implantação simplificada e melhorias para permitir que os usuários podem acessar recursos de dispositivos pessoais e ajudar os departamentos de TI a gerenciar o controle de acesso.|  
|[Exclusividade SPN e UPN](https://technet.microsoft.com/library/dn535779.aspx)|Controladores de domínio que executam o Windows Server 2012 R2 bloqueiam a criação de nomes principais serviço duplicado (SPNs) e nomes principais do usuário (UPNs).|  
|[Winlogon automático reiniciar logon (ARSO)](https://technet.microsoft.com/library/dn535772.aspx)|Permite bloquear aplicativos de tela a ser reiniciado e está disponível em dispositivos Windows 8.1.|  
|[Atestado de chave do TPM](https://technet.microsoft.com/library/dn581921.aspx)|Permite CAs atestar criptograficamente em um certificado emitido que a chave privada do certificado solicitante está realmente protegida por um Trusted Platform Module (TPM).|  
|[Gerenciamento e proteção de credenciais](https://technet.microsoft.com/library/dn408190.aspx)|Novos credenciais proteção e domínio autenticação controles para reduzir o roubo de credenciais.|  
|[Depreciação do serviço de duplicação de arquivos (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|O nível funcional de domínio do Windows Server 2003 também foi preterido porque no nível funcional, FRS é usado para replicar SYSVOL. Que significa que, quando você cria um novo domínio em um servidor que executa o Windows Server 2012 R2, o nível funcional do domínio deve ser o Windows Server 2008 ou mais recente. Você ainda pode adicionar um controlador de domínio que executa o Windows Server 2012 R2 a um domínio existente que tem o Windows Server 2003 nível funcional do domínio; Você simplesmente não pode criar um novo domínio nesse nível.|  
|[Novos níveis funcionais de domínio e da floresta](../active-directory-functional-levels.md)|Há novos níveis funcionais para Windows Server 2012 R2. Novos recursos estão disponíveis no Windows Server 2012 R2 DFL.|  
|[Alterações de Otimizador de consulta LDAP](https://technet.microsoft.com/library/dn535775.aspx)|Melhoria de desempenho na eficiência da pesquisa LDAP e tempo de pesquisa LDAP de consultas complexas.|  
|[Melhorias de evento da falha 1644](https://technet.microsoft.com/library/dn535775.aspx)|Estatísticas de resultados de pesquisa LDAP foram adicionadas para eventos da falha 1644 ID para ajudar na solução de problemas.|  
|[Melhoria de taxa de transferência do Active Directory replicação](https://technet.microsoft.com/library/dn535775.aspx)|Ajusta a replicação do AD taxa de transferência máxima de 40Mbps para cerca de 600 Mbps|  
  
## <a name="BKMK_WhatsNewAD"></a>O que há de novo no AD DS no Windows Server 2012?  
A tabela a seguir resume os novos recursos para o AD DS no Windows Server 2012, com um link para obter mais informações detalhadas onde ele está disponível. Para obter uma explicação mais detalhada de alguns recursos, incluindo suas necessidades, consulte [What's New nos serviços de domínio do Active Directory (AD DS)](https://technet.microsoft.com/library/hh831477.aspx).  
  
|Recurso|Descrição|  
|-----------|---------------|  
|Consulte Active ativação baseada no Directory (AD BA) [visão geral de ativação de Volume](https://technet.microsoft.com/library/hh831612.aspx)|Simplifica a tarefa de configurar a distribuição e o gerenciamento de licenças de software de volume.|  
|[Serviços de Federação do Active Directory (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|Adiciona a função instalar por meio do Gerenciador de servidores, simplificado confiança-setup, gerenciamento de confiança automáticas, suporte ao protocolo SAML e muito mais.|  
|Eventos de alinhamento de página perdida do Active Directory|Evento NTDS ISAM 530 com erro jet-1119 é registrado para detectar página perdido flush eventos para bancos de dados do Active Directory.|  
|[No Active Directory reciclar Bin Interface do usuário](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Centro Administrativo do Active Directory (ADAC) inclui o gerenciamento de GUI do recurso de bin reciclagem originalmente introduzido no Windows Server 2008 R2.|  
|[Active cmdlets de replicação de diretório e topologia Windows PowerShell](https://technet.microsoft.com/library/hh831757.aspx)|Dá suporte a criação e gerenciamento de sites do Active Directory, links de site, objetos de conexão e mais usando o Windows PowerShell.|  
|[Controle de acesso dinâmico](https://technet.microsoft.com/library/hh831717.aspx)|Nova plataforma de autorização baseada em declarações que aprimora o modelo de controle de acesso herdada.|  
|[Interface do usuário de política de senha refinadas](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|ADAC adiciona suporte a interface gráfica do usuário para a edição de criação e a atribuição do PSOs adicionados originalmente no Windows Server 2008.|  
|[Contas de serviço gerenciado do grupo (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|Um nova entidade tipo de segurança conhecido como um gMSA. Serviços em execução em vários hosts podem ser executados com a mesma conta gMSA.|  
|[Ingresso em domínio Offline DirectAccess](https://technet.microsoft.com/library/jj574150.aspx)|Estende offline do ingresso no domínio, incluindo os pré-requisitos do DirectAccess.|  
|[A rápida implantação por meio de clonagem de controlador de domínio virtual](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|Controladores de domínio virtualizados podem ser implantadas rapidamente por meio da clonagem controladores de domínio virtual existentes usando os cmdlets do Windows PowerShell.|  
|[Alterações feitas no pool marca d'](https://technet.microsoft.com/library/jj574229.aspx)|Adiciona novos eventos de monitoramento e cotas para proteger contra consumo excessivo de pool de RID global. Dobra o tamanho do pool RID global, opcionalmente, se o pool original fica esgotado.|  
|Serviço de tempo protegido|Aumenta a segurança para W32tm removendo segredos da transmissão, removendo as funções de hash MD5 e exigir que o servidor para autenticar com os clientes de tempo do Windows 8|  
|[Proteção de reversão USN para controladores de domínio virtualizados](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|Acidentalmente restaurar backups de instantâneo de controladores de domínio virtualizados não faz com que a reversão do USN.|  
|[Visualizador do Windows PowerShell histórico](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|Permitir que os administradores exibir os comandos do Windows PowerShell executados ao usar ADAC.|  
  
### <a name="BKMK_"></a>Manutenção automática e alterações reiniciar comportamento depois que as atualizações são aplicadas pelo Windows Update  
Antes do lançamento do Windows 8, Windows Update gerenciado sua própria agenda interna para verificar se há atualizações e para baixar e instalá-los. Ele necessário que o agente de atualização do Windows foi sempre em execução em segundo plano, consumindo memória e outros recursos do sistema.  
  
Windows 8 e Windows Server 2012 apresentam um novo recurso chamado [manutenção automática](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). A manutenção automática consolida muitos recursos diferentes que cada usado para gerenciar seu próprio agendamento e execução lógica. Essa consolidação permite todos esses componentes usam muito menos recursos do sistema, funcionam de forma consistente, respeitar a nova [modo de espera conectado](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) estado novos tipos de dispositivo e consuma menos bateria em dispositivos portáteis.  
  
Como o Windows Update é uma parte da manutenção automática no Windows 8 e Windows Server 2012, sua própria agenda interna para definir um dia e hora para instalar as atualizações não é mais eficaz. Para ajudar a garantir o comportamento de reinicialização consistente e previsível para todos os dispositivos e computadores em sua empresa, incluindo aqueles que executam o Windows 8 e Windows Server 2012, você pode configurar as seguintes configurações de política de grupo:  
  
-   **Configuração do computador | Políticas | Modelos administrativos | Componentes do Windows | Atualização do Windows | Configurar as atualizações automáticas**  
  
-   **Configuração do computador | Políticas | Modelos administrativos | Componentes do Windows | Atualização do Windows | Nenhuma reinicialização automática com usuários conectados**  
  
-   **Configuração do computador | Políticas | Modelos administrativos | Componentes do Windows | Agendador de tarefas de manutenção | Intervalo aleatório de manutenção**  
  
A tabela a seguir lista alguns exemplos de como definir essas configurações para fornecer um comportamento desejado de reinicialização.  
  
|||  
|-|-|  
|**Cenário**|**Configurações recomendadas**|  
|**WSUS gerenciado**<br /><br />-Instalar atualizações de uma vez por semana<br />-Reinicializar sextas-feiras às 11 horas|Configurar computadores para instalar automaticamente, impedir a reinicialização automática até o tempo desejado<br /><br />**Política**: configurar as atualizações automáticas (habilitadas)<br /><br />Configurar a atualização automática: 4 - download automático das atualizações e agendar a instalação<br /><br />**Política**: nenhuma reinicialização automática com os usuários com logon (desabilitado)<br /><br />**Prazos WSUS**: defina como sextas-feiras às 11 horas|  
|**WSUS gerenciado**<br /><br />-Irregular instala em diferentes horas ou dias|Definir grupos de destino para diferentes grupos de máquinas que devem ser atualizados juntos<br /><br />As etapas acima de uso para o cenário anterior<br /><br />Definir prazos diferentes para grupos de destino diferente|  
|**Não gerenciados para o WSUS - sem suporte para prazos**<br /><br />-Irregular instala em momentos diferentes|**Política**: configurar as atualizações automáticas (habilitadas)<br /><br />Configurar a atualização automática: 4 - download automático das atualizações e agendar a instalação<br /><br />**Chave do registro:** habilitar a chave do registro abordada no artigo KB Microsoft [2835627](https://support.microsoft.com/kb/2835627)<br /><br />**Política:** atraso aleatório a manutenção automática (habilitado)<br /><br />Defina **intervalo aleatório de manutenção Regular** para PT6H atraso aleatório 6 horas para fornecer o comportamento a seguir:<br /><br />-Atualizações instalará no tempo de manutenção configurado além de um intervalo aleatório<br /><br />-Reiniciar para cada computador será realizado exatamente 3 dias mais tarde<br /><br />Como alternativa, definir um tempo de manutenção diferente para cada grupo de computadores|  
  
Para obter mais informações sobre por que a equipe de engenharia do Windows implementados essas alterações, consulte [minimizando reinicia após a atualização automática no Windows Update](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx).  
  
## <a name="BKMK_InstallationChanges"></a>Alterações de instalação de função do AD DS server  
No Windows Server 2003 por meio do Windows Server 2008 R2, você executou o x86 ou X64 versão da ferramenta de linha de comando Adprep.exe antes de executar o Assistente de instalação do Active Directory, o Dcpromo.exe e o Dcpromo.exe tinha variantes opcionais para instalar da mídia ou de instalação autônoma.  
  
A partir do Windows Server 2012, instalações de linha de comando são executadas usando o módulo ADDSDeployment no Windows PowerShell. Com base em GUI promoções são executadas no Gerenciador do servidor usando um Assistente de configuração completamente novo AD DS. Para simplificar o processo de instalação, ADPREP foi integrada à instalação do AD DS e é executado automaticamente, conforme necessário. O Assistente de configuração com base em Windows PowerShell AD DS direciona automaticamente as funções mestre de esquema e infraestrutura nos domínios onde os controladores de domínio estão sendo adicionados, então remotamente executa os comandos ADPREP necessários nos controladores de domínio relevantes.  
  
Verificações de pré-requisitos no Assistente de instalação do AD DS identificam possíveis erros antes de começa a instalação. Condições de erro podem ser corrigidas para eliminar preocupações de uma atualização parcialmente completa. O assistente também exporta um script do Windows PowerShell que contém todas as opções especificadas durante a instalação gráfica.  
  
Juntos, as alterações de instalação do AD DS simplificam o processo de instalação da função de controlador de domínio e reduzem a probabilidade de erros administrativos, especialmente quando você estiver implantando vários controladores de domínio em todos os domínios e regiões globais.  
Mais informações detalhadas sobre a interface gráfica do usuário e instalações baseados em Windows PowerShell, incluindo a sintaxe de linha de comando e instruções do Assistente passo a passo, consulte [instalar o Active Directory Domain Services](https://technet.microsoft.com/library/hh472162.aspx). Para os administradores que deseja controlar a introdução de alterações no esquema em uma floresta do Active Directory independente da instalação do controladores de domínio do Windows Server 2012 em uma floresta existente, os comandos de Adprep.exe ainda podem ser executados em um prompt de comando com privilégios elevados.  
  
## <a name="BKMK_DeprecatedFeatures"></a>Recursos preteridos e as mudanças de comportamento relacionadas ao AD DS no Windows Server 2012  
Há algumas alterações relacionadas ao AD DS:  
  
-   **Substituição de Adprep32.exe**  
  
    Há apenas uma versão de Adprep.exe e pode ser executado conforme necessário em servidores de 64 bits que executam o Windows Server 2008 ou posterior. Ele pode ser executado remotamente e deve ser executado remotamente se que direcionadas operações função mestre está hospedada em um sistema operacional de 32 bits ou o Windows Server 2003.  
  
-   **Substituição de Dcpromo.exe**  
  
    Dcpromo foi preterido embora no Windows Server 2012 apenas ele ainda pode ser executado com um arquivo de resposta ou parâmetros de linha de comando para conceder tempo organizações para fazer a transição automação existente para as novas opções de instalação do Windows PowerShell.  
  
-   **LMHash está desabilitada em contas de usuário**  
  
    Segura padrões de segurança modelos no Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012 habilitar a política de NoLMHash que é desabilitada nos modelos de segurança do Windows 2000 e controladores de domínio do Windows Server 2003. Desabilitar a política NoLMHash para clientes dependentes LMHash conforme necessário, usando as etapas no artigo KB [946405](https://support.microsoft.com/kb/946405).  
  
A partir do Windows Server 2008, controladores de domínio também têm as seguintes configurações padrão segura, em comparação com controladores de domínio que executam o Windows Server 2003 ou Windows 2000.  
  
|||||  
|-|-|-|-|  
|Tipo de criptografia ou a política|Padrão do Windows Server 2008|Windows Server 2012 e Windows Server 2008 R2 padrão|Comentário|  
|AllowNT4Crypto|Desabilitado|Desabilitado|Clientes de bloco de mensagens de servidor (SMB) de terceiros podem ser incompatíveis com as configurações de segurança padrão em controladores de domínio. Em todos os casos, essas configurações podem ficar relaxadas para permitir a interoperabilidade, mas apenas à custa da segurança. Para obter mais informações, consulte [942564 do artigo](https://go.microsoft.com/fwlink/?LinkId=164558) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=164558).|  
|DES|Habilitado|Desabilitado|[Artigo 977321](https://go.microsoft.com/fwlink/?LinkId=177717) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=177717)|  
|Proteção CBT/estendido para autenticação integrada|N/D|Habilitado|Consulte [Microsoft Security Advisory (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) e [976918 do artigo](https://go.microsoft.com/fwlink/?LinkId=178251) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=178251).<br /><br />Revise e instale o hotfix no [977073 do artigo](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) na Base de dados de Conhecimento da Microsoft conforme necessário.|  
|LMv2|Habilitado|Desabilitado|[Artigo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=178251)|  
  
## <a name="BKMK_SysReqs"></a>Requisitos de sistema operacional  
Requisitos mínimos do sistema para o Windows Server 2012 estão listados na tabela a seguir. Para obter mais informações sobre os requisitos de sistema e informações de pré-instalação, consulte [instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Não existem requisitos adicionais do sistema para instalar uma nova floresta do Active Directory, mas você deve adicionar memória suficiente para armazenar em cache o conteúdo do banco de dados do Active Directory para melhorar o desempenho de controladores de domínio, as solicitações de cliente LDAP e aplicativos habilitados para Active Directory. Se você estiver atualizando um controlador de domínio existente ou adicionando um novo controlador de domínio a uma floresta existente, revise a seção próxima para garantir que o servidor atenda aos requisitos de espaço em disco.  
  
|||  
|-|-|  
|Processador|1,4 processador de Ghz de 64 bits|  
|RAM|512 MB|  
|Requisitos de espaço livre em disco|32 GB|  
|Resolução de tela|800 x 600 ou superior|  
|Diversos|Unidade de DVD, o teclado, o acesso à Internet|  
  
### <a name="BKMK_DiskSpaceDCWin8"></a>Requisitos de espaço em disco para atualizar controladores de domínio  
Esta seção aborda os requisitos de espaço em disco apenas para atualizar controladores de domínio do Windows Server 2008 ou Windows Server 2008 R2. Para saber mais sobre requisitos de espaço em disco para atualizar controladores de domínio para versões anteriores do Windows Server, consulte [disco requisitos de espaço para atualizar para o Windows Server 2008](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) ou [disco requisitos de espaço para atualizar para o Windows Server 2008 R2](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2).  
  
Dimensione o disco que hospeda os Active Directory banco de dados e arquivos de log para acomodar as extensões de esquema de orientado por aplicativo e personalizados, aplicativo e índices iniciadas pelo administrador, além de espaço para os objetos e atributos que você será adicionado à pasta ao longo da vida útil de implantação do controlador de domínio (geralmente de 5 a 8 anos). Dimensionamento diretamente em tempo de implantação é geralmente um bom investimento em comparação comparado a maiores custos de toque necessários para expandir o armazenamento em disco após a implantação. Para obter mais informações, consulte [planejamento da capacidade para serviços de domínio do Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx).  
  
Em controladores de domínio que você pretende atualizar, certifique-se de que a unidade que hospeda o Active Directory banco de dados (NTDS. DIT) tem espaço livre em disco que representa pelo menos 20% do NTDS. Arquivo DIT antes de começar a atualização do sistema operacional. Se houver espaço em disco insuficiente no volume, a atualização pode falhar e o relatório de compatibilidade de atualização retorna um erro indicando que o espaço em disco insuficiente:  
  
Nesse caso, você pode experimentar uma desfragmentação offline do banco de dados do Active Directory recapturar espaço adicional e tente novamente a atualização. Para obter mais informações, consulte [compactar o arquivo de banco de dados do diretório (desfragmentação Offline)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx).  
  
### <a name="available-skus"></a>SKUs disponíveis  
Existem 4 edições do Windows Server: Foundation, Essentials, padrão e Datacenter.   
As duas edições que dão suporte a função AD DS são padrão e Datacenter.  
  
Em versões anteriores, as edições do Windows Server difere no suporte a funções de servidor, contagens de processador e suporte de grande quantidade de memória. As edições padrão e Datacenter do Windows Server suportam todos os recursos e o hardware subjacente mas variam em seus direitos de virtualização - duas instâncias virtuais são permitidas para Standard edition e ilimitadas instâncias virtuais são permitidas para Datacenter edition.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Windows client e sistemas operacionais Windows Server que têm suporte para ingressar em domínios do Windows Server  
A seguir Windows client e sistemas operacionais Windows Server são compatíveis para computadores membros do domínio com controladores de domínio que executam o Windows Server 2012 ou posterior:  
  
-   Sistemas operacionais: Windows 8.1, Windows 8, Windows 7, Windows Vista 
  
    Computadores que executam o Windows 8.1 ou Windows 8 também são capazes de ingressar em domínios com controladores de domínio execução versão anterior do Windows Server, incluindo o Windows Server 2003 ou posterior. Nesse caso, no entanto, alguns recursos do Windows 8 podem exigir configuração adicional ou podem não estar disponíveis. Para obter mais informações sobre esses recursos e outras recomendações para gerenciamento de clientes do Windows 8 em domínios de nível inferior, consulte [computadores membro de executando o Windows 8 em domínios do Windows Server 2003](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx).  
  
-   Sistemas operacionais: Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2, Windows Server 2003  
  
## <a name="BKMK_UpgradePaths"></a>Suporte a caminhos de atualização in-loco  
Controladores de domínio que executam versões de 64 bits do Windows Server 2008 ou Windows Server 2008 R2 podem ser atualizados para o Windows Server 2012. Não é possível atualizar controladores de domínio que executam o Windows Server 2003 ou versões de 32 bits do Windows Server 2008. Para substituí-los, instalar controladores de domínio que executam uma versão posterior do Windows Server no domínio e, em seguida, remova os controladores de domínio do Windows Server 2003.  
  
|Se você estiver executando essas edições|Você pode atualizar essas edições|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard com SP2<br /><br />OR<br /><br />Windows Server 2008 Enterprise com SP2|Windows Server 2012 Standard<br /><br />OR<br /><br />O Windows Server 2012 Datacenter|  
|O Windows Server 2008 Datacenter com SP2|O Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|Padrão do Windows Server 2008 R2 com SP1<br /><br />OR<br /><br />Windows Server 2008 R2 Enterprise com SP1|Windows Server 2012 Standard<br /><br />OR<br /><br />O Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter com SP1|O Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
Para saber mais sobre os caminhos de atualização com suporte, consulte [versões de avaliação e atualizar opções para o Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Observe que você não pode converter um controlador de domínio que executa uma versão de avaliação do Windows Server 2012 diretamente para uma versão de varejo. Em vez disso, instale um controlador de domínio adicional em um servidor que executa uma versão de varejo e remover o AD DS do controlador de domínio que executa a versão de avaliação.  
  
Devido a um problema conhecido, você não pode atualizar um controlador de domínio que executa uma instalação Server Core do Windows Server 2008 R2 para uma instalação Server Core do Windows Server 2012. A atualização irá travar em uma tela preta sólida tarde no processo de atualização. Reiniciar esses controladores de domínio expõe uma opção no arquivo boot.ini para reverter para a versão anterior do sistema operacional. Uma reinicialização adicional aciona a reversão automática para a versão anterior do sistema operacional. Até que uma solução esteja disponível, é recomendável que você instale um novo controlador de domínio executando uma instalação Server Core do Windows Server 2012 em vez de um controlador de domínio existente que executa uma instalação Server Core do Windows Server 2008 R2 a atualização in-loco. Para obter mais informações, consulte o artigo KB [2734222](https://support.microsoft.com/kb/2734222).  
  
## <a name="BKMK_FunctionalLevels"></a>Requisitos e recursos de nível funcionais  
 Windows Server 2012 requer um nível funcional da floresta do Windows Server 2003. Ou seja, antes de adicionar um controlador de domínio que executa o Windows Server 2012 para uma floresta do Active Directory existente, o nível funcional da floresta deve ser Windows Server 2003 ou posterior. Isso significa que os controladores de domínio que executam o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 podem operar na mesma floresta, mas os controladores de domínio que executam o Windows 2000 Server não são compatíveis e bloqueará a instalação de um controlador de domínio que executa o Windows Server 2012. Se a floresta contém controladores de domínio que executam o Windows Server 2003 ou posterior mas floresta funcional nível ainda é Windows 2000, a instalação também está bloqueada.  
  
Controladores de domínio do Windows 2000 devem ser removidos antes da adição de controladores de domínio do Windows Server 2012 à floresta. Nesse caso, considere seguir o fluxo de trabalho:  
  
1.  Instale os controladores de domínio que executam o Windows Server 2003 ou posterior. Esses controladores de domínio podem ser implantadas em uma versão de avaliação do Windows Server. Esta etapa também exige [executando adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) para o sistema operacional liberar como pré-requisito.  
  
2.  Remova os controladores de domínio do Windows 2000. Especificamente, normalmente rebaixar ou remover forçadamente os controladores de domínio do Windows Server 2000 do domínio e usados Active Directory usuários e computadores para remover as contas de controlador de domínio para todos os controladores de domínio removido.  
  
3.  Aumente o nível funcional da floresta para o Windows Server 2003 ou posterior.  
  
4.  Instale os controladores de domínio que executam o Windows Server 2012.  
  
5.  Remova os controladores de domínio que executam versões anteriores do Windows Server.  
  
Um novo recurso permite que o novo nível de funcional de domínio do Windows Server 2012: o **suporte KDC para declarações, autenticação composta e proteção Kerberos** política de modelo administrativo KDC tem duas configurações (**sempre fornecer declarações** e **falhar solicitações de autenticação unarmored**) que exigem o nível funcional de domínio do Windows Server 2012.  
  
O nível funcional da floresta Windows Server 2012 não fornece os novos recursos, mas garante que qualquer novo domínio criado na floresta automaticamente funcionará no nível funcional de domínio do Windows Server 2012. O nível funcional de domínio do Windows Server 2012 não fornece outros novos recursos além do suporte do KDC para declarações, autenticação composta e proteção Kerberos. Mas isso garante que qualquer controlador de domínio no domínio seja executado em Windows Server 2012. Para saber mais sobre outros recursos que estão disponíveis em diferentes níveis funcionais, consulte [níveis funcionais de Noções básicas sobre serviços Active Directory domínio (AD DS)](../active-directory-functional-levels.md).  
  
Depois de definir o nível funcional da floresta para um determinado valor, você não pode reverter ou diminuir o nível funcional da floresta, com as seguintes exceções: depois que você elevar o nível funcional da floresta para o Windows Server 2012, é possível reduzi-lo ao Windows Server 2008 R2. Se não tiver sido habilitada a Lixeira do Active Directory, você também pode diminuir o nível funcional do Windows Server 2012 para Windows Server 2008 R2 ou Windows Server 2008 ou do Windows Server 2008 R2 floresta para o Windows Server 2008. Se o nível funcional da floresta é definido como Windows Server 2008 R2, ele não pode ser revertido, por exemplo, ao Windows Server 2003.  
  
Depois de definir o nível funcional do domínio para um determinado valor, você não pode reverter ou diminuir o nível funcional de domínio, com as seguintes exceções: quando você aumentar o nível funcional de domínio para Windows Server 2008 R2 ou Windows Server 2012, e se o nível funcional da floresta for Windows Server 2008 ou inferior, você tem a opção de reverter o nível funcional do domínio de volta para o Windows Server 2008 ou Windows Server 2008 R2. Você pode diminuir o nível funcional de domínio somente do Windows Server 2012 para Windows Server 2008 R2 ou Windows Server 2008 ou do Windows Server 2008 R2, Windows Server 2008. Se o nível funcional do domínio é definido como Windows Server 2008 R2, ele não pode ser revertido, por exemplo, ao Windows Server 2003.  
  
Para obter mais informações sobre os recursos que estão disponíveis em níveis inferiores funcionais, consulte [níveis funcionais de Noções básicas sobre serviços Active Directory domínio (AD DS)](../active-directory-functional-levels.md).  
  
Além dos níveis funcionais, um controlador de domínio que executa o Windows Server 2012 fornece recursos adicionais que não estão disponíveis em um controlador de domínio que executa uma versão anterior do Windows Server. Por exemplo, um controlador de domínio que executa o Windows Server 2012 pode ser usado para clonagem de controlador de domínio virtual, enquanto um controlador de domínio que executa uma versão anterior do Windows Server não é possível. Mas clonagem de controlador de domínio virtual e proteções de controlador de domínio virtual no Windows Server 2012 não têm os requisitos de nível funcionais.  
  
> [!NOTE]  
> Microsoft Exchange Server 2013 requer um nível funcional da floresta do Windows server 2003 ou superior.  
  
## <a name="BKMK_ServerRoles"></a>Interoperabilidade do AD DS com outras funções de servidor e sistemas operacionais Windows  
AD DS não é compatível com os seguintes sistemas operacionais Windows:  
  
-   Windows MultiPoint Server  
  
-   Windows Server 2012 Essentials  
  
AD DS não pode ser instalado em um servidor que também executa as seguintes funções de servidor ou serviços de função:  
  
-   Hyper-V Server  
  
-   Agente de Conexão de área de trabalho remota  
  
## <a name="BKMK_OpsMasters"></a>Funções mestre de operações  
Alguns recursos novos no Windows Server 2012 afetam funções mestre de operações:  
  
-   O emulador do PDC deve estar executando o Windows Server 2012 para dar suporte à clonagem controladores de domínio virtual. Há pré-requisitos adicionais para clonagem controladores de domínio. Para obter mais informações, consulte [virtualização os serviços de domínio do Active Directory (AD DS)](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Novos objetos de segurança são criados quando o emulador do PDC executa o Windows Server 2012.  
  
-   O mestre RID tem novo RID emissão e a funcionalidade de monitoramento. As melhorias incluem melhor log de eventos, limites mais apropriados, e a capacidade de - em uma emergência - aumentar o RID geral pool de alocação em um bit. Para obter mais informações, consulte [Gerenciando emissão d'](../../ad-ds/manage/Managing-RID-Issuance.md).  
  
> [!NOTE]  
> Embora eles não são funções mestre de operações, outra alteração na instalação do AD DS é que a função de servidor DNS e o catálogo global são instaladas por padrão em todos os controladores de domínio que executam o Windows Server 2012.  
  
## <a name="BKMK_Virtual"></a>Controladores de domínio de virtualização  
Melhorias no início do AD DS no Windows Server 2012 Habilitar virtualização mais segura de controladores de domínio e a capacidade de clonar controladores de domínio. Clonagem de controladores de domínio por sua vez permite a implantação rápida de controladores de domínio adicionais em um novo domínio e outros benefícios. Para obter mais informações, consulte [Introdução aos serviços de domínio do Active Directory & #40; AD DS e 41; Virtualização & #40; Nível de 100 & #41; ](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
## <a name="BKMK_Admin"></a>Administração de servidores do Windows Server 2012  
Use o [Remote Server Administration ferramentas para Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) gerenciar controladores de domínio e outros servidores que executam o Windows Server 2012. Você pode executar o Windows Server 2012 administração ferramentas de servidor remoto em um computador que executa o Windows 8.  
  
## <a name="BKMK_AppCompat"></a>Compatibilidade de aplicativos  
A tabela a seguir abrange aplicativos comuns do Microsoft Active Directory integrado. A tabela aborda quais versões do Windows Server que os aplicativos podem ser instalados e se a introdução de controladores de domínio do Windows Server 2012 afeta a compatibilidade de aplicativos.  
  
|Produto|Anotações|  
|-----------|---------|  
|[O Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|O Configuration Manager 2007 com SP2 (inclui o Configuration Manager 2007 R2 e o Configuration Manager 2007 R3):<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter **Observação:** embora elas serão compatíveis totalmente como clientes, não há nenhum plano para adicionar suporte para implantar essas como sistemas operacionais usando o recurso de implantação de sistema operacional do Configuration Manager 2007. Além disso, nenhuma servidores de site ou sistemas de site terá suporte em qualquer SKU do Windows Server 2012.|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|Não há suporte para o Microsoft Office SharePoint Server 2007 para instalação no Windows Server 2012.|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|SharePoint 2010 Service Pack 2 é necessária para instalar e operar <br />SharePoint 2010 em servidores do Windows Server 2012<br /><br />SharePoint 2010 Foundation Service Pack 2 é necessária para instalar e operar SharePoint 2010 Foundation nos servidores do Windows Server 2012<br /><br />O processo de instalação do SharePoint Server 2010 (sem service packs) Falha no Windows Server 2012<br /><br />O SharePoint Server 2010 pré-requisito instalador (PrerequisiteInstaller.exe) falha com erro "Este programa tem problemas de compatibilidade". Clicar em "Executar o programa sem Obtendo ajuda" exibe o erro "Verificando se o SharePoint pode ser instalado & #124; SharePoint Server 2010 (sem service packs) não pode ser instalado no Windows Server 2012."|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|Requisitos mínimos para um servidor de banco de dados em um farm<br /><br />A edição de 64 bits do Windows Server 2008 R2 Service Pack 1 (SP1) padrão, empresa, ou Datacenter ou a edição de 64 bits do Windows Server 2012 Standard ou Datacenter<br /><br />Requisitos mínimos para um único servidor com o banco de dados interno:<br /><br />A edição de 64 bits do Windows Server 2008 R2 Service Pack 1 (SP1) padrão, empresa, ou Datacenter ou a edição de 64 bits do Windows Server 2012 Standard ou Datacenter<br /><br />Requisitos mínimos para servidores web front-end e servidores de aplicativos em um farm:<br /><br />A edição de 64 bits do Windows Server 2008 R2 Service Pack 1 (SP1) padrão, empresa, ou Datacenter ou a edição de 64 bits do Windows Server 2012 Standard ou Datacenter.|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Configuration Manager Service Pack 1:<br /><br />Microsoft adicionará os seguintes sistemas operacionais nossa matriz de suporte do cliente com o lançamento do Service Pack 1:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter<br /><br />Todas as funções de servidor do site - incluindo servidores de site, provedores SMS e pontos de gerenciamento - podem ser implantadas em servidores com as seguintes edições do sistema operacional:<br /><br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[O Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 requer com o Windows Server 2008 R2 ou Windows Server 2012. Ele não pode ser executado em uma instalação Server Core. Ele pode ser executado em [servidores virtuais](https://technet.microsoft.com/library/gg399035.aspx).|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|Lync Server 2010 pode ser instalado em uma nova instalação (não uma atualização) Windows Server 2012 se [atualizações cumulativas de outubro de 2012 para o Lync Server](https://support.microsoft.com/?kbid=2493736) estão instalados. Não há suporte para atualizar o sistema operacional para o Windows Server 2012 para uma instalação existente do Lync Server 2010. Servidor de bate-papo de grupo do Microsoft Lync Server 2010 também não tem suporte no Windows Server 2012.|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Sistema Center 2012 Endpoint Protection Service Pack 1 irá atualizar a matriz de suporte de cliente para incluir os seguintes sistemas operacionais<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|FEP 2010 com Update Rollup 1 irá atualizar a matriz de suporte de cliente para incluir os seguintes sistemas operacionais:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|Forefront Threat Management Gateway (TMG)|TMG tem suporte para ser executado somente no Windows Server 2008 e Windows Server 2008 R2. Para obter mais informações, consulte [requisitos do sistema para Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx).|  
|Windows Server Update Services|Esta versão do WSUS já dá suporte a computadores baseados em Windows Server 2012 ou computadores baseados no Windows 8 como clientes.|  
|Windows Server Update Services 3.0|Artigo de atualização KB [2734608](https://support.microsoft.com/kb/2734608) permite que os servidores que executam o Windows Server Update Services (WSUS) 3.0 SP2 fornecem atualizações para computadores que executam o Windows 8 ou Windows Server 2012: **Observação:** exigem que os clientes com ambientes autônomos WSUS 3.0 SP2 ou System Center Configuration Manager 2007 Service Pack 2 ambientes com o WSUS 3.0 SP2 [2734608](https://support.microsoft.com/kb/2734608) gerenciar computadores baseados no Windows 8 ou computadores baseados em Windows Server 2012 como clientes de forma adequada.|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|Windows Server 2012 Standard e Datacenter têm suporte para as seguintes funções: mestre de esquema, servidor de catálogo global, controlador de domínio, caixa de correio e um cliente de função de servidor de acesso<br /><br />Nível funcional da floresta: Windows Server 2003 ou posterior<br /><br />Fonte: Requisitos do sistema do Exchange 2013|  
|Exchange 2010|[Fonte: Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />Exchange 2010 com Service Pack 3 podem ser instalados em servidores de membro do Windows Server 2012.<br /><br />[Requisitos de sistema do Exchange 2010](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) lista o mais recente com suporte mestre, global catálogo e domínio controlador esquema como Windows Server 2008 R2.<br /><br />Nível funcional da floresta: Windows Server 2003 ou posterior|  
|SQL Server 2012|Fonte: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />SQL Server 2012 RTM tem suporte no Windows Server 2012.|  
|SQL Server 2008 R2|Fonte: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requer o SQL Server 2008 R2 com Service Pack 1 ou posterior para instalar o Windows Server 2012.|  
|SQL Server 2008|Fonte: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requer o SQL Server 2008 com Service Pack 3 ou posterior para instalar o Windows Server 2012.|  
|SQL Server 2005|Fonte: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Não tem suporte para instalar o Windows Server 2012.|  
  
## <a name="BKMK_KnownIssues"></a>Problemas conhecidos  
A tabela a seguir lista os problemas conhecidos relacionados à instalação do AD DS.  
  
||||  
|-|-|-|  
|Título e o número do artigo KB|Área de tecnologia afetada|Descrição do problema|  
|[2830145](https://support.microsoft.com/kb/2830145): SID S-1-18-1 e SID S-1-18-2 não podem ser mapeadas no Windows 7 ou Windows Server 2008 R2 com base em computadores em um ambiente de domínio|Compatibilidade do AD DS/aplicativo de gerenciamento|Aplicativos que mapeiam o SID S-1-18-1 e SID S-1-18-2, que são novas no Windows Server 2012, podem falhar porque os SIDs não puder ser resolvido em computadores baseados em Windows Server 2008 R2 ou Windows 7. Para resolver esse problema, instale o hotfix nos computadores baseados em Windows Server 2008 R2 e Windows 7 no domínio.|  
|[2737129](https://support.microsoft.com/kb/2737129): preparação de política de grupo não é realizada quando você prepara automaticamente um domínio existente para o Windows Server 2012|Instalação do AD DS|Adprep /domainprep /gpprep não é executado automaticamente como parte da instalação o primeiro controlador de domínio que executa o Windows Server 2012 em um domínio. Se ele nunca tiver sido executado anteriormente no domínio, ele deve ser executado manualmente.|  
|[2737416](https://support.microsoft.com/kb/2737416): Windows implantação de controlador de domínio baseados no PowerShell se repete avisos|Instalação do AD DS|Avisos podem aparecer durante a validação de pré-requisito e reaparecem durante a instalação.|  
|[2737424](https://support.microsoft.com/kb/2737424): "O formato do nome do domínio especificado é inválido" Erro ao tentar remover os serviços de domínio do Active Directory de um controlador de domínio|Instalação do AD DS|Esse erro é exibido se você estiver removendo o último DC em um domínio em que ainda existem contas RODC pré-criado. Isso afeta o Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008.|  
|[2737463](https://support.microsoft.com/kb/2737463): controlador de domínio não for iniciado, c00002e2 erro ocorrer, ou "Escolha uma opção" é exibido|Instalação do AD DS|Um controlador de domínio não for iniciado porque um administrador usada Dism.exe, Pkgmgr.exe ou Ocsetup.exe para remover a função DirectoryServices-DomainController.|  
|[2737516](https://support.microsoft.com/kb/2737516): limitações de verificação de IFM no Gerenciador do servidor do Windows Server 2012|Instalação do AD DS|Verificação de IFM pode ter limitações conforme explicado no artigo KB.|  
|[2737535](https://support.microsoft.com/kb/2737535): install-AddsDomainController cmdlet retorna o parâmetro definido erro para RODC|Instalação do AD DS|Você pode receber um erro ao tentar anexar um servidor a uma conta RODC se você especificar argumentos que já são preenchidos na conta RODC pré-criados.|  
|[2737560](https://support.microsoft.com/kb/2737560): "Não é possível executar a verificação de conflito de esquema do Exchange" Falha de verificação de erro e pré-requisitos|Instalação do AD DS|Verificação de pré-requisitos falha quando você configura o Windows Server 2012 DC primeiro em um domínio existente como controladores de domínio estão faltando a SeServiceLogonRight para o serviço de rede ou porque o WMI ou DCOM protocolos estão bloqueados.|  
|[2737797](https://support.microsoft.com/kb/2737797): AddsDeployment módulo com o argumento - Whatif mostra resultados incorretos de DNS|Instalação do AD DS|O parâmetro - WhatIf mostra DNS server não será instalado, mas ele será.|  
|[2737807](https://support.microsoft.com/kb/2737807): botão próximo não está disponível na página Opções de controlador de domínio|Instalação do AD DS|O botão Avançar está desabilitado na página Opções de controlador de domínio porque o endereço IP do alvo DC não é mapeado para um site ou sub-rede existente, ou a senha DSRM não é digitada e confirmada corretamente.|  
|[2737935](https://support.microsoft.com/kb/2737935): instalação do active Directory paralisado no estágio de "Criando o objeto de configurações NTDS"|Instalação do AD DS|A instalação trava porque a senha de administrador local corresponde a senha de administrador do domínio, ou problemas de rede impedirem a replicação crítica de concluir.|  
|[2738060](https://support.microsoft.com/kb/2738060): "Acesso negado" mensagem de erro quando você cria um domínio filho remotamente usando instalar AddsDomain|Instalação do AD DS|Você recebe o erro quando você executa ADDSDomain de instalação com o cmdlet Invoke-Command se a DNSDelegationCredential tiver uma senha incorreta.|  
|[2738697](https://support.microsoft.com/kb/2738697): "o servidor não é operacional" domínio controlador erro de configuração quando você configurar um servidor usando o Gerenciador do servidor|Instalação do AD DS|Você receber esse erro ao tentar instalar o AD DS em um computador de grupo de trabalho, porque a autenticação NTLM está desabilitada.|  
|[2738746](https://support.microsoft.com/kb/2738746): você receber erros de acesso negado após você faz logon em uma conta de domínio do administrador local|Instalação do AD DS|Quando você fizer logon usando uma conta de administrador local em vez de conta de administrador interno e, em seguida, crie um novo domínio, a conta não é adicionada ao grupo Admins. do domínio.|  
|[2743345](https://support.microsoft.com/kb/2743345): "o sistema não pode localizar o arquivo especificado" Erro de /gpprep Adprep ou falhas de ferramenta|Instalação do AD DS|Você receber esse erro quando você executa adprep /gpprep porque o mestre de infraestrutura é implementa um namespace separado|  
|[2743367](https://support.microsoft.com/kb/2743367): Adprep "não um aplicativo Win32 válido" Erro no Windows Server 2003, versão de 64 bits|Instalação do AD DS|Você receber esse erro porque o Windows Server 2012 Adprep não pode ser executado no Windows Server 2003.|  
|[2753560](https://support.microsoft.com/kb/2753560): ADMT 3.2 e PES 3.1 erros de instalação no Windows Server 2012|ADMT|ADMT 3.2 não pode ser instalado no Windows Server 2012 por design.|  
|[2750857](https://support.microsoft.com/kb/2750857): replicação do DFS relatórios de diagnóstico não são exibidos corretamente no Internet Explorer 10|Replicação do DFS|Relatório de diagnóstico de replicação DFS não é exibida corretamente devido às alterações no Internet Explorer 10.|  
|[2741537](https://support.microsoft.com/kb/2741537): atualizações de política de grupo remoto estão visíveis para os usuários|Política de grupo|Isso ocorre porque as tarefas agendadas são executadas no contexto de cada usuário que fez logon. O design do Agendador de tarefas do Windows requer um prompt interativo neste cenário.|  
|[2741591](https://support.microsoft.com/kb/2741591): arquivos ADM não estão presentes no SYSVOL na opção Status de infraestrutura do GPMC|Política de grupo|Replicação de GP pode relatar "replicação em andamento" porque o Status da infraestrutura GPMC não segue as regras de filtragem personalizadas.|  
|[2737880](https://support.microsoft.com/kb/2737880): "o serviço não pode ser iniciado" Erro durante a configuração do AD DS|Virtuais clonagem de DC|Você receber esse erro durante a instalação ou removendo o AD DS ou clonagem, porque o serviço de servidor de função DS está desabilitado.|  
|[2742836](https://support.microsoft.com/kb/2742836): dois concessões DHCP são criadas para cada controlador de domínio quando você usa o v CC clonagem recurso|Virtuais clonagem de DC|Isso ocorre porque o controlador de domínio clonados recebeu uma concessão antes de clonagem e novamente quando clonagem foi concluída.|  
|[2742844](https://support.microsoft.com/kb/2742844): clonagem falhará e o servidor de controlador de domínio é reiniciado no DSRM no Windows Server 2012|Virtuais clonagem de DC|O controlador de domínio clonado começa no DSRM porque clonagem falha para qualquer um dos vários motivos listados no artigo KB.|  
|[2742874](https://support.microsoft.com/kb/2742874): clonagem de controlador de domínio não recriar todos os nomes de entidade de serviço|Virtuais clonagem de DC|Alguns SPNs três partes não serão recriados no controlador de domínio clonado devido a uma limitação do processo de alteração de nome de domínio.|  
|[2742908](https://support.microsoft.com/kb/2742908): "há servidores de logon estão disponíveis" erro após clonagem controlador de domínio|Virtuais clonagem de DC|Você receber esse erro ao tentar fazer logon depois clonagem um controlador de domínio virtualizado porque clonagem falhou e o controlador de domínio é iniciado no DSRM. Faça logon como. \administrator para solucionar a falha de clonagem.|  
|[2742916](https://support.microsoft.com/kb/2742916): clonagem de controlador de domínio falha com erro 8610 no DCPROMO|Virtuais clonagem de DC|Clonagem falhará porque o emulador do PDC não realizou duplicação de entrada da partição do domínio, provavelmente porque a função foi transferida.|  
|[2742927](https://support.microsoft.com/kb/2742927): "Índice estava fora do intervalo" Erro de nova AdDcCloneConfig|Virtuais clonagem de DC|Você recebe o erro depois que você executar o cmdlet New-ADDCCloneConfigFile enquanto clonagem virtuais controladores de domínio, porque o cmdlet não foi executado em um prompt de comando com privilégios elevados ou porque seu token de acesso não contém o grupo de administradores.|  
|[2742959](https://support.microsoft.com/kb/2742959): clonagem de controlador de domínio falha com erro 8437: "parâmetro inválido foi especificado para esta operação de replicação"|Virtuais clonagem de DC|Clonagem falhou porque foi especificado um nome de clone inválido ou um nome NetBIOS duplicado.|  
|[2742970](https://support.microsoft.com/kb/2742970): DC clonagem falha com nenhum DSRM, computador de origem e clone duplicado|Virtuais clonagem de DC|O controlador de domínio virtual clonado é inicializado no modo de diretório serviços reparo (DSRM), usando um nome duplicado como o controlador de domínio de origem porque o arquivo DCCloneConfig.xml não foi criado no local correto ou o controlador de domínio de origem foi reiniciado antes de clonagem.|  
|[2743278](https://support.microsoft.com/kb/2743278): clonagem erro 0x80041005 de controlador de domínio|Virtuais clonagem de DC|O controlador de domínio clonado é inicializado no DSRM porque apenas um servidor WINS foi especificado. Se qualquer servidor WINS for especificado, servidores preferencial e WINS alternativo devem ser especificados.|  
|[2745013](https://support.microsoft.com/kb/2745013): mensagem de erro "O servidor não é operacional" Se você executar AdDcCloneConfigFile de novo no Windows Server 2012|Virtuais clonagem de DC|Você receber esse erro depois de executar o cmdlet New-ADDCCloneConfigFile porque o servidor não é possível contatar um servidor de catálogo global.|  
|[2747974](https://support.microsoft.com/kb/2747974): o controlador de domínio clonagem evento 2224 fornece orientação incorreta|Virtuais clonagem de DC|ID de evento 2224 incorretamente estados que contas de serviço gerenciado devem ser removidas antes de clonagem. Autônomo MSAs deverá ser removido, mas MSAs grupo não bloqueiam clonagem.|  
|[2748266](https://support.microsoft.com/kb/2748266): não é possível desbloquear uma unidade criptografada pelo BitLocker após a atualização para o Windows 8|BitLocker|Você receberá um erro "Aplicativo não encontrado" quando você tentar desbloquear uma unidade em um computador que foi atualizado do Windows 7.|  
  
## <a name="see-also"></a>Consulte também  
[Recursos de avaliação do Windows Server 2012](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Guia de avaliação do Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[Instalar e implantar o Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
  


