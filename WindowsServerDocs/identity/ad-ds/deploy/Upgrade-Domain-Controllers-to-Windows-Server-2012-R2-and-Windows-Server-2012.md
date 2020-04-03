---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: Fazer upgrade de controladores de domínio para o Windows Server 2012 R2 e o Windows Server 2012
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f8e5164ee1b5729d30536ae61df7cf3579e57fe6
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822719"
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Fazer upgrade de controladores de domínio para o Windows Server 2012 R2 e o Windows Server 2012

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece informações básicas sobre Active Directory Domain Services no Windows Server 2012 R2 e no Windows Server 2012 e explica o processo de atualização dos controladores de domínio do Windows Server 2008 ou do Windows Server 2008 R2.  
  
## <a name="domain-controller-upgrade-steps"></a><a name="BKMK_UpgradeWorkflow"></a>Etapas de atualização do controlador de domínio  
A maneira recomendada de atualizar um domínio é promover controladores de domínio que executam versões mais recentes do Windows Server e rebaixam controladores de domínio anteriores conforme necessário. Esse método é preferível para atualizar o sistema operacional de um controlador de domínio existente. Esta lista aborda as etapas gerais a serem seguidas antes de promover um controlador de domínio que executa uma versão mais recente do Windows Server:  
  
1. Verifique se o servidor de destino atende aos [requisitos de sistema](https://technet.microsoft.com/library/dn303418.aspx).  
2. Verifique [Application compatibility](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).  
3. Verifique as configurações de segurança. Para obter mais informações, consulte [Recursos preteridos e alterações de comportamento relacionadas ao AD DS no Windows Server 2012 e no [Secure default settings in Windows Server 2008 e no Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault).  
4. Verifique a conectividade com o servidor de destino a partir do computador em que você planeja executar a instalação.  
5. Verifique a disponibilidade das funções mestras de operações necessárias:  

   - Para instalar o primeiro DC que executa o Windows Server 2012 em um domínio e floresta existentes, o computador em que você executa a instalação precisa de conectividade com o mestre de esquema para executar adprep/forestprep e o mestre de infraestrutura para executar adprep/domainprep.  
   - Para instalar o primeiro controlador de domínio em um domínio onde o esquema de árvore já está estendido, é necessário apenas ter conectividade com o mestre de infraestrutura.  
   - Para instalar ou remover um domínio em uma floresta existente, é necessário ter conectividade com o mestre de nomeação de domínio.  
   - Qualquer instalação de controlador de domínio também requer conectividade com o mestre RID.  
   - Caso esteja instalando o primeiro controlador de domínio somente leitura em uma floresta existente, será necessário ter conectividade com o mestre de infraestrutura para cada partição do diretório de aplicativos, também conhecida como um contexto de nomeação não relacionado a domínio ou NDNC.  

6. Certifique-se de fornecer as credenciais necessárias para executar a instalação do AD DS.  

   |Ação de instalação|Requisitos de credenciais|  
   |-----------------------|---------------------------|  
   |Instalar uma nova floresta|Administrador local no servidor de destino|  
   |Instalar um novo domínio em uma floresta existente|Administrador corporativo|  
   |Instalar um controlador de domínio adicional em um domínio existente|Administradores do domínio|  
   |Executar adprep/forestprep|Administradores de esquema, administradores corporativos e administradores do domínio|  
   |Executar adprep/domainprep|Administradores do domínio|  
   |Executar adprep/domainprep/gpprep|Administradores do domínio|  
   |Executar adprep/rodcprep|Administrador corporativo|  

   Você pode delegar permissões de instalação do AD DS. Para obter mais informações, consulte as [tarefas de gerenciamento de instalação](https://technet.microsoft.com/library/cc773327(WS.10).aspx).  

Os links a seguir contêm instruções passo a passo para promover controladores de domínio (novos e réplicas) do Windows Server 2012 usando cmdlets do Windows PowerShell e o Gerenciador do Servidor:  

- [Instalar o Active Directory Domain Services (nível 100)](https://technet.microsoft.com/library/hh472162.aspx)  
- [Instalar uma nova floresta de Active Directory do Windows Server 2012 (nível 200)](https://technet.microsoft.com/library/jj574166.aspx)  
- [Instalar um controlador de domínio de réplica do Windows Server 2012 em um domínio existente (nível 200)](https://technet.microsoft.com/library/jj574134.aspx)  
- [Instalar um novo Windows Server 2012 Active Directory domínio filho ou de árvore (nível 200)](https://technet.microsoft.com/library/jj574105.aspx)  
- [Instalar um RODC (controlador de domínio somente leitura) do Windows Server 2012 Active Directory (nível 200)](https://technet.microsoft.com/library/jj574152.aspx)  
- [Guia passo a passo para a configuração do controlador de domínio do Windows Server 2012 (en-US)](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  

## <a name="windows-update-considerations"></a>Considerações sobre Windows Update

Antes do lançamento do Windows 8, o Windows Update gerenciava seu próprio cronograma interno para verificar, baixar e instalar atualizações. O Windows Update Agent precisava sempre ser executado em segundo plano, consumindo memória e recursos de outros sistemas.  
  
O Windows 8 e Windows Server 2012 introduzem um novo recurso chamado [manutenção automática](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). A Manutenção Automática consolida diversos recursos diferentes que antes eram usados para gerenciar suas próprias agendas e lógica de execução. Esta consolidação permite que todos esses componentes usem muito menos recursos do sistema, funcionem de maneira consistente, respeitem o novo estado [Connected Standby (Modo de espera conectado)](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) para novos tipos de dispositivo e consumam menos bateria em dispositivos portáteis.  
  
Como o Windows Update faz parte da Manutenção Automática no Windows 8 e Windows Server 2012, sua própria agenda interna não é mais usada para definir o dia e a hora para instalar atualizações. Para ajudar a garantir um comportamento de reinicialização consistente e previsível para todos os computadores e dispositivos da sua empresa, incluindo aqueles com Windows 8 e Windows Server 2012, consulte o artigo [2885694](https://support.microsoft.com/kb/2885694) da base de dados da Microsoft (ou consulte o apanhado acumulado de outubro de 2013 [2883201](https://support.microsoft.com/kb/2883201)) e depois defina as configurações de política descritas na publicação do blog WSUS [Enabling a more predictable Windows Update experience for Windows 8 and Windows Server 2012 (KB 2885694) (Habilitando uma experiência mais previsível do Windows Update no Windows 8 e Windows Server 2012 (KB 2885694))](https://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx).  

## <a name="whats-new-in-ad-ds-in-windows-server-2012-r2"></a><a name="BKMK_NewWS2012R2"></a>O que há de novo na AD DS no Windows Server 2012 R2?

A tabela a seguir resume os novos recursos do AD DS no Windows Server 2012 R2, com um link para informações mais detalhadas (quando disponíveis). Para obter uma explicação mais detalhada de alguns recursos, incluindo seus requisitos, consulte [Novidades no Active Directory no Windows Server 2012 R2](https://technet.microsoft.com/library/dn268294.aspx).  

|Recurso|Descrição|  
|-----------|---------------|  
|[Workplace Join](https://technet.microsoft.com/library/dn280945.aspx)|Permite que os operadores de informações ingressem com seus dispositivos pessoais para acessar os recursos e serviços da empresa.|  
|[Proxy de aplicativo Web](https://technet.microsoft.com/library/dn280942.aspx)|Oferece acesso ao aplicativo Web usando um novo serviço de função de Acesso Remoto.|  
|[Serviços de Federação do Active Directory (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|O AD FS traz uma implantação simplificada e melhorias para permitir que os usuários acessem recursos com dispositivos pessoais e ajuda os departamentos de TI a gerenciar o controle de acesso.|  
|[Exclusividade de SPN e UPN](https://technet.microsoft.com/library/dn535779.aspx)|Os controladores de domínio com Windows Server 2012 R2 bloqueiam a criação de SPNs (Nomes de entidade de serviço) e UPNs (Nomes de entidade de usuário) duplicados.|  
|[ARSO (Logon de Reinicialização Automática) do Winlogon](https://technet.microsoft.com/library/dn535772.aspx)|Habilita a reinicialização de aplicativos que bloqueiam a tela, também disponível para dispositivos com Windows 8.1.|  
|[Atestado de chave de TPM](https://technet.microsoft.com/library/dn581921.aspx)|Habilita as CAs a atestarem criptograficamente um certificado emitido indicando que a chave privada do solicitante do certificado é de fato protegida por um TPM (Trusted Platform Module).|  
|[Proteção e gerenciamento de credenciais](https://technet.microsoft.com/library/dn408190.aspx)|Nova proteção de credenciais e controles de autenticação de domínio para reduzir o risco de roubo de credenciais.|  
|[Substituição do FRS (serviço de replicação de arquivo)](https://technet.microsoft.com/library/dn535775.aspx)|O nível funcional de domínio do Windows Server 2003 também foi preterido pois, no nível funcional, o FRS é usado para replicar o SYSVOL. Isso significa que, ao criar um novo domínio em um servidor que executa o Windows Server 2012 R2, o nível do domínio funcional deve ser do Windows Server 2008 ou mais recente. Você ainda pode adicionar um controlador de domínio que executa o Windows Server 2012 R2 a um domínio existente que tenha um nível funcional de domínio do Windows Server 2003; Você simplesmente não pode criar um novo domínio nesse nível.|  
|[Novos níveis funcionais de domínio e floresta](../active-directory-functional-levels.md)|Existem novos níveis funcionais no Windows Server 2012 R2. Novos recursos estão disponíveis no Windows Server 2012 R2 DFL.|  
|[Alterações do otimizador de consulta LDAP](https://technet.microsoft.com/library/dn535775.aspx)|Melhoria no desempenho da eficiência de pesquisa LDAP e tempo de pesquisa LDAP de consultas complexas.|  
|[Melhorias no evento 1644](https://technet.microsoft.com/library/dn535775.aspx)|As estatísticas de resultado de pesquisa LDAP foram adicionadas ao ID do evento 1644 para auxiliar na solução de problemas.|  
|[Melhoria da taxa de transferência de replicação Active Directory](https://technet.microsoft.com/library/dn535775.aspx)|Ajustes da produtividade de replicação máxima de AD de 40 Mbps para cerca de 600 Mbps|  

## <a name="whats-new-in-ad-ds-in-windows-server-2012"></a><a name="BKMK_WhatsNewAD"></a>O que há de novo na AD DS no Windows Server 2012?

A tabela a seguir resume os novos recursos do AD DS no Windows Server 2012, com um link para informações mais detalhadas (quando disponíveis). Para obter uma explicação mais detalhada de alguns recursos, incluindo seus requisitos, consulte [What ' s New in Active Directory Domain Services (AD DS)](https://technet.microsoft.com/library/hh831477.aspx).  
  
|Recurso|Descrição|  
|-----------|---------------|  
|AD BA (ativação baseada no Active Directory); consulte [Visão Geral de Ativação de Volume](https://technet.microsoft.com/library/hh831612.aspx)|Simplifica a tarefa de configuração de distribuição e gerenciamento de licenças de software por volume.|  
|[Serviços de Federação do Active Directory (AD FS) (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|Adiciona instalação de funções pelo Gerenciador do Servidor, configuração de confiança simplificada, gerenciamento de confiança automático, suporte ao protocolo SAML e muito mais.|  
|Eventos de liberação de páginas perdidas do Active Directory|O evento ISAM NTDS 530 com erro de jet -1119 é registrado para detectar eventos de liberação de páginas perdidas em bancos de dados do Active Directory.|  
|[Active Directory a interface do usuário da lixeira](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|O ADAC (Centro Administrativo do Active Directory) adiciona gerenciamento de GUI do recurso de lixeira introduzido originalmente no Windows Server 2008 R2.|  
|[Active Directoryndo cmdlets do Windows PowerShell de replicação e topologia](https://technet.microsoft.com/library/hh831757.aspx)|Suporte à criação e ao gerenciamento de sites do Active Directory, links de sites, objetos de conexão e muito mais usando o Windows PowerShell.|  
|[Controle de acesso dinâmico](https://technet.microsoft.com/library/hh831717.aspx)|Nova plataforma de autorização baseada em declaração que melhora o antigo modelo de controle de acesso.|  
|[Interface do usuário da política de senha refinada](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|O ADAC adiciona suporte a GUI para criação, edição e atribuição de PSOs originalmente adicionados no Windows Server 2008.|  
|[Contas de serviço gerenciado de grupo (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|Um novo tipo de entidade de segurança, conhecido como uma gMSA. Os serviços em execução em vários hosts podem ser executados sob a mesma conta gMSA.|  
|[Ingresso no domínio offline do DirectAccess](https://technet.microsoft.com/library/jj574150.aspx)|Amplia o ingresso em domínio offline ao incluir pré-requisitos do DirectAccess.|  
|[Implantação rápida por meio da clonagem do DC (controlador de domínio virtual)](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|Os controladores de domínio virtualizados podem ser implantados rapidamente pela clonagem dos controladores de domínio virtuais existentes usando cmdlets do Windows PowerShell.|  
|[Alterações no pool de RID](https://technet.microsoft.com/library/jj574229.aspx)|Adiciona novos eventos de monitoramento e cotas para impedir o consumo excessivo do pool RID global. Como opção, dobra o tamanho do pool RID global caso o pool original se esgote.|  
|Serviço de tempo de segurança|Melhora a segurança de W32tm ao remover segredos da transmissão, remover as funções de hash MD5 e exigir a autenticação do servidor com clientes de tempo do Windows 8|  
|[Reversão do USN proteção para DCs virtualizados](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|A restauração acidental de backups de instantâneos de controladores de domínio virtualizados não leva mais à reversão de USN.|  
|[Visualizador de histórico do Windows PowerShell](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|Permite que os administradores exibam os comandos do Windows PowerShell executados ao usar o ADAC.|  
  
### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a><a name="BKMK_"></a>Manutenção automática e alterações para o comportamento de reinicialização depois que as atualizações são aplicadas pelo Windows Update

Antes do lançamento do Windows 8, o Windows Update gerenciava seu próprio cronograma interno para verificar, baixar e instalar atualizações. O Windows Update Agent precisava sempre ser executado em segundo plano, consumindo memória e recursos de outros sistemas.  
  
O Windows 8 e Windows Server 2012 introduzem um novo recurso chamado [manutenção automática](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). A Manutenção Automática consolida diversos recursos diferentes que antes eram usados para gerenciar suas próprias agendas e lógica de execução. Esta consolidação permite que todos esses componentes usem muito menos recursos do sistema, funcionem de maneira consistente, respeitem o novo estado [Connected Standby (Modo de espera conectado)](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) para novos tipos de dispositivo e consumam menos bateria em dispositivos portáteis.  
  
Como o Windows Update faz parte da Manutenção Automática no Windows 8 e Windows Server 2012, sua própria agenda interna não é mais usada para definir o dia e a hora para instalar atualizações. Para ajudar a garantir um comportamento de reinicialização consistente e previsível para todos os computadores e dispositivos da sua empresa, incluindo aqueles com Windows 8 e Windows Server 2012, defina as seguintes configurações de Política de Grupo:  

- **Configuração do computador | Políticas | Modelos Administrativos | Componentes do Windows | Windows Update | Configurar Atualizações Automáticas**  
- **Configuração do computador | Políticas | Modelos Administrativos | Componentes do Windows | Windows Update | Sem reinicialização automática com usuários conectados**  
- **Configuração do computador | Políticas | Modelos Administrativos | Componentes do Windows | Agendador de manutenção | Atraso aleatório na manutenção**  

A tabela a seguir lista alguns exemplos de como definir essas configurações para obter o comportamento de reinicialização desejado.  

|||  
|-|-|  
|**Cenário**|**Configuração (ões) recomendada (s)**|  
|**Gerenciado pelo WSUS**<br /><br />-Instalar atualizações uma vez por semana<br />-Reinicializar sexta-feiras em 23h|Configurar as máquinas para instalação automática, impedir a reinicialização automática até a hora desejada<br /><br />**Política**: Configurar as atualizações automáticas (Habilitado)<br /><br />Configurar a atualização automática: 4-baixar automaticamente e agendar a instalação<br /><br />**Política**: sem reinicialização automática com usuários conectados (desabilitado)<br /><br />**Datas limites de WSUS**: definido para sextas-feitas às 23h|  
|**Gerenciado pelo WSUS**<br /><br />-O uptais é instalado em diferentes horas/dias|Definir grupos de destino para diferentes grupos de máquinas que devem ser atualizados em conjunto<br /><br />Usar as etapas acima para o cenário anterior<br /><br />Definir datas limites diferentes para grupos de destino diferentes|  
|**Não gerenciado pelo WSUS-sem suporte para prazos finais**<br /><br />-O escalonamento é instalado em momentos diferentes|**Política**: Configurar as atualizações automáticas (Habilitado)<br /><br />Configurar a atualização automática: 4-baixar automaticamente e agendar a instalação<br /><br />**Chave do Registro:** Habilitar a chave do Registro discutida no artigo da Base de Dados de Conhecimento da Microsoft [2835627](https://support.microsoft.com/kb/2835627)<br /><br />**Política:** Atraso aleatório na manutenção automática (Habilitado)<br /><br />Defina **Atraso aleatório na manutenção regular** de PT6H para atraso aleatório de seis horas para obter o seguinte comportamento:<br /><br />-As atualizações serão instaladas no tempo de manutenção configurado mais um atraso aleatório<br /><br />-A reinicialização de cada máquina ocorrerá exatamente três dias depois<br /><br />Como alternativa, defina um horário de manutenção diferente para cada grupo de máquinas|  

Para ver mais informações sobre por que a equipe de desenvolvimento do Windows implementou essas mudanças, consulte [Minimizing restarts after automatic updating in Windows Update (Minimizando as reinicializações depois de atualizações automáticas no Windows Update)](https://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx).  

## <a name="ad-ds-server-role-installation-changes"></a><a name="BKMK_InstallationChanges"></a>Alterações de instalação da função de servidor AD DS

Do Windows Server 2003 ao Windows Server 2008 R2, era necessário executar a versão x86 ou X64 da ferramenta de linha de comando Adprep.exe antes de executar o Assistente de Instalação do Active Directory, Dcpromo.exe. Além disso, Dcpromo.exe possuía variantes opcionais para instalação a partir de uma mídia ou instalação autônoma.  
  
A partir do Windows Server 2012, as instalações por linha de comando são realizadas usando o Módulo ADDSDeployment no Windows PowerShell. As promoções baseadas em GUI são executadas no Gerenciador do Servidor usando um Assistente de Configuração do AD DS completamente novo. Para simplificar o processo de instalação o, ADPREP foi integrado à instalação do AD DS e é executado automaticamente quando necessário. O assistente de configuração AD DS baseado no Windows PowerShell destina-se automaticamente às funções mestre de esquema e infraestrutura nos domínios em que os DCs estão sendo adicionados e executa remotamente os comandos da ADPREP necessários nos controladores de domínio relevantes.  
  
As verificações de pré-requisitos no Assistente de Instalação do AD DS identificam erros potenciais antes do início da instalação. As condições de erro podem ser corrigidas para eliminar problemas de uma atualização parcialmente concluída. O assistente também exporta um script do Windows PowerShell contendo todas as opções que foram especificadas durante a instalação gráfica.  
  
Consideradas em conjunto, as alterações de instalação do AD DS simplificam o processo de instalação de funções de controlador de domínio e reduzem a probabilidade de erros administrativos, especialmente quando ocorre a implantação de vários controladores de domínio em regiões e domínios globais.  
Para obter informações mais detalhadas sobre instalações baseadas em Windows PowerShell e GUI, incluindo sintaxe de linha de comando e instruções passo a passo do assistente, consulte [Instalar os Serviços de Domínio do Active Directory](https://technet.microsoft.com/library/hh472162.aspx). Para administradores que desejam controlar a introdução de alterações de esquema em uma floresta do Active Directory independentemente da instalação de controladores de domínio do Windows Server 2012 em uma floresta existente, os comandos do Adprep.exe ainda podem ser executados em um prompt de comando elevado.  

## <a name="deprecated-features-and-behavior-changes-related-to-ad-ds-in-windows-server-2012"></a><a name="BKMK_DeprecatedFeatures"></a>Recursos preteridos e alterações de comportamento relacionadas a AD DS no Windows Server 2012

Há algumas alterações relacionadas ao AD DS:  

- **Substituição de adprep32. exe**
   - Há apenas uma versão do Adprep.exe, que pode ser executada quando necessário em servidores de 64 bits que executam o Windows Server 2008 ou mais recente. Ela pode ser executada remotamente, e isso deve ser feito quando uma função mestra de operações direcionada é hospedada em um sistema operacional de 32 bits ou no Windows Server 2003.  
- **Reprovação do Dcpromo. exe**
   - O Dcpromo é preterido, embora no Windows Server 2012 apenas ele ainda possa ser executado com um arquivo de resposta ou parâmetros de linha de comando para dar às organizações tempo para fazer a transição da automação existente para as novas opções de instalação do Windows PowerShell.  
- **O LMHash está desabilitado nas contas de usuário**
  - Os padrões de segurança em modelos de segurança do Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012 habilitam a política NoLMHash, desabilitada nos modelos de segurança dos controladores de domínio do Windows 2000 e Windows Server 2003. Desabilite a política NoLMHash para clientes que dependem de LMHash conforme necessário, utilizando as etapas no artigo KB [946405](https://support.microsoft.com/kb/946405).  

A partir do Windows Server 2008, os controladores de domínio também têm as seguintes configurações padrão seguras, em comparação com os controladores de domínio que executam o Windows Server 2003 ou o Windows 2000.

|||||  
|-|-|-|-|  
|Tipo ou política de criptografia|Padrão do Windows Server 2008|Padrão do Windows Server 2012 e Windows Server 2008 R2|Comentário|  
|AllowNT4Crypto|Desabilitada|Desabilitada|Os clientes do protocolo SMB podem ser incompatíveis com as configurações padrão seguras em controladores de domínio. Em todos os casos, essas configurações podem ser reduzidas a fim de permitir a interoperabilidade (porém, em detrimento da segurança). Para obter mais informações, consulte o [artigo 942564](https://go.microsoft.com/fwlink/?LinkId=164558) na base de dados de conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=164558).|  
|DES|Habilitada|Desabilitada|[Artigo 977321](https://go.microsoft.com/fwlink/?LinkId=177717) na base de dados de conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=177717)|  
|Proteção CBT/estendida para autenticação integrada|N/D|Habilitada|Consulte [consultoria de segurança da Microsoft (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) e [artigo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) na base de dados de conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=178251).<br /><br />Examine e instale o hotfix no [artigo 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) na base de dados de conhecimento Microsoft, conforme necessário.|  
|LMv2|Habilitada|Desabilitada|[Artigo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) na base de dados de conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=178251)|  

## <a name="operating-system-requirements"></a><a name="BKMK_SysReqs"></a>Requisitos de sistema operacional

Os requisitos mínimos do sistema para o Windows Server 2012 estão listados na tabela a seguir. Para obter mais informações sobre requisitos de sistema e informações de pré-instalação, consulte [Instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Não há requisitos de sistema adicionais para instalação de uma nova floresta do Active Directory, mas você deve adicionar memória suficiente para armazenar em cache o conteúdo do banco de dados do Active Directory, de modo a melhorar o desempenho de controladores de domínio, solicitações de cliente LDAP e aplicativos habilitados pelo Active Directory. Se estiver atualizando um controlador de domínio existente ou adicionando um novo controlador de domínio a uma floresta existente, examine a próxima seção para verificar se o servidor cumpre com os requisitos de espaço em disco.  

|||  
|-|-|  
|Processador|Processador de 1,4 Ghz e 64 bits|  
|RAM|512 MB|  
|Requisitos de espaço livre no disco|32 GB|  
|Resolução da tela|800 x 600 ou mais|  
|Diversos|Unidade de DVD, teclado, acesso à Internet|  

### <a name="disk-space-requirements-for-upgrading-domain-controllers"></a><a name="BKMK_DiskSpaceDCWin8"></a>Requisitos de espaço em disco para atualizar controladores de domínio

Esta seção aborda os requisitos de espaço em disco apenas para a atualização de controladores de domínio do Windows Server 2008 ou do Windows Server 2008 R2. Para saber mais sobre os requisitos de espaço em disco para atualização de controladores de domínio em versões anteriores do Windows Server, consulte [Requisitos de espaço em disco para atualizar para o Windows Server 2008](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) ou [Requisitos de espaço em disco para atualizar para o Windows Server 2008 R2](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2).  
  
Forneça espaço no disco que hospeda o banco de dados do Active Directory e os arquivos de log para permitir o armazenamento das extensões de esquema orientadas a aplicativos e personalizadas, índices iniciados por administradores e aplicativos e objetos/atributos que serão adicionados ao diretório durante o tempo de implantação do controlador de domínio (normalmente de cinco a oito anos). O dimensionamento correto no momento da implantação costuma ser um bom investimento quando comparado aos grandes custos gerados para expandir o armazenamento em disco após a implantação. Para obter mais informações, consulte [Planejamento de capacidade para os Serviços de Domínio do Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx).  
  
Em controladores de domínio que serão atualizados, antes de começar a atualização do sistema operacional, verifique se a unidade que hospeda o banco de dados Active Directory (NTDS.DIT) possui espaço livre em disco que represente pelo menos 20% do arquivo NTDS.NIT. Se não houver espaço livre em disco suficiente no volume, poderá ocorrer falha na atualização e o relatório de compatibilidade de atualização retornará um erro indicando que não há espaço livre suficiente no disco:  
  
Nesse caso, você poderá tentar uma desfragmentação offline do banco de dados Active Directory para recuperar espaço e então tentar novamente a atualização. Para saber mais, consulte [Compactar o arquivo de banco de dados de diretório (desfragmentação offline)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx).  
  
### <a name="available-skus"></a>SKUs disponíveis

Existem quatro edições do Windows Server: Foundation, Essentials, Standard e Datacenter.
As duas edições com suporte à função do AD DS são Standard e Datacenter.  
  
Nas versões anteriores, as edições do Windows Server eram diferentes em relação ao suporte a funções de servidor, número de processadores e suporte a uma grande quantidade de memória. As edições Standard e Datacenter do Windows Server dão suporte a todos os recursos e ao hardware subjacente, mas variam em seus direitos de virtualização-duas instâncias virtuais são permitidas para a edição Standard e as instâncias virtuais ilimitadas são permitidas para o datacenter versão.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Sistemas operacionais Windows Client e Windows Server dão suporte ao ingresso em domínios do Windows Server

Os sistemas operacionais Windows Client e Windows Server a seguir contam com suporte a computadores membros de domínios com controladores de domínios que executam o Windows Server 2012 ou mais recente:  
  
- Sistemas operacionais cliente: Windows 8.1, Windows 8, Windows 7, Windows Vista
   - Computadores que executam o Windows 8.1 ou Windows 8 também podem ingressar em domínios que possuem controladores de domínio com versões anteriores do Windows Server, incluindo Windows Server 2003 ou mais recente. Neste caso, porém, alguns recursos do Windows 8 poderão exigir configurações adicionais ou não estar disponíveis. Para obter mais informações sobre esses recursos e outras recomendações para o gerenciamento de clientes do Windows 8 em domínios de nível inferior, consulte [Execução de computadores de membro do Windows 8 em domínios do Windows Server 2003](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx).  
- Sistemas operacionais do servidor: Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003  

## <a name="supported-in-place-upgrade-paths"></a><a name="BKMK_UpgradePaths"></a>Caminhos de atualização in-loco com suporte

Controladores de domínio que executam versões de 64 bits do Windows Server 2008 ou Windows Server 2008 R2 podem ser atualizados para o Windows Server 2012. Não é possível atualizar controladores de domínio que executam o Windows Server 2003 ou versões 32 bits do Windows Server 2008. Para substituí-los, instale controladores de domínio que executam uma versão mais recente do Windows Server no domínio e, então, remova os controladores de domínio com Windows Server 2003.  

|Se você está executando estas edições|É possível atualizar para estas edições|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard com SP2<br /><br />ou do<br /><br />Windows Server 2008 Enterprise com SP2|Windows Server 2012 Standard<br /><br />ou do<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 Datacenter com SP2|Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|Windows Server 2008 R2 Standard com SP1<br /><br />ou do<br /><br />Windows Server 2008 R2 Enterprise com SP1|Windows Server 2012 Standard<br /><br />ou do<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter com SP1|Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
Para obter mais informações sobre caminhos de atualização com suporte, consulte [Versões de avaliação e opções de atualização para o Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Observação: não é possível converter um controlador de domínio que executa uma versão de avaliação do Windows Server 2012 diretamente em um versão comercial. Em vez disso, instale um controlador de domínio adicional em um servidor que execute uma versão de avaliação e remova o AD DS do controlador de domínio executado na versão de avaliação.  
  
Devido a um problema conhecido, não é possível atualizar um controlador de domínio que executa uma instalação Server Core do Windows Server 2008 R2 para uma instalação Server Core do Windows Server 2012. A atualização parará em uma tela totalmente preta no fim do processo de atualização. A reinicialização desses controladores de domínio expõe uma opção no arquivo boot.ini para reverter para a versão anterior do sistema operacional. Uma reinicialização adicionai aciona uma reversão automática para a versão anterior do sistema operacional. Até que uma solução esteja disponível, é recomendável instalar um novo controlador de domínio executando uma instalação do Server Core do Windows Server 2012 em vez de fazer a atualização in-loco de um controlador de domínio existente que executa uma instalação Server Core do Windows Server 2008 R2. Para obter mais informações, consulte o artigo da base de dados de conhecimento [2734222](https://support.microsoft.com/kb/2734222).  

## <a name="functional-level-features-and-requirements"></a><a name="BKMK_FunctionalLevels"></a>Recursos e requisitos de nível funcional

O Windows Server 2012 requer um nível funcional de floresta do Windows Server 2003. Ou seja, para poder adicionar um controlador de domínio que executa o Windows Server 2012 a uma floresta Active Directory existente, o nível funcional da floresta deve ser Windows Server 2003 ou superior. Isso significa que o controlador de domínio que executa o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 pode operar na mesma floresta, mas os controladores de domínio que executam o Windows 2000 Server não contam com suporte e bloquearão a instalação de um controlador de domínio que executa o Windows Server 2012. Caso a floresta contenha controladores de domínio que executam o Windows Server 2003 ou posterior, mas o nível funcional da floresta ainda seja Windows 2000, a instalação também será bloqueada.  
  
Os controladores de domínio do Windows 2000 devem ser removidos antes da adição de controladores de domínio do Windows Server 2012 à floresta. Neste caso, considere o seguinte fluxo de trabalho:  

1. Instale controladores de domínio que executam o Windows Server 2003 ou posterior. Esses controladores de domínio podem ser implantados em uma versão de avaliação do Windows Server. Esta etapa também requer [executar o adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) para a versão do sistema operacional como um pré-requisito.  
2. Remova os controladores de domínio do Windows 2000. Especificamente, realize o rebaixamento normal ou a remoção forçada dos controladores de domínio do Windows Server 2000 do domínio e dos usuários e computadores do Active Directory utilizados para remover as contas de todos os controladores de domínio removidos.  
3. Eleve o nível funcional da floresta para Windows Server 2003 ou superior.  
4. Instale controladores de domínio que executem o Windows Server 2012.  
5. Remova controladores de domínio que executam versões anteriores do Windows Server.  

O novo nível funcional de domínio do Windows Server 2012 permite um novo recurso: o **suporte do KDC para declarações, autenticação composta e** política de modelo administrativo do KDC de proteção de Kerberos tem duas configurações (**sempre fornecer declarações** e **falhas de solicitações de autenticação não protegidas**) que exigem o nível funcional de domínio do Windows Server 2012.  
  
O nível funcional de floresta do Windows Server 2012 não fornece novos recursos, mas garante que qualquer novo domínio criado na floresta operará automaticamente no nível funcional de domínio do Windows Server 2012. O nível funcional de domínio do Windows Server 2012 não fornece outros recursos novos além do suporte do KDC para declarações, autenticação composta e proteção Kerberos. Mas garante que qualquer controlador de domínio no domínio execute o Windows Server 2012. Para obter mais informações sobre outros recursos que estão disponíveis em diferentes níveis funcionais, consulte [Noções básicas sobre níveis funcionais dos AD DS (Serviços de Domínio do Active Directory)](../active-directory-functional-levels.md).  
  
Depois de definir o nível funcional da floresta para um determinado valor, não será possível reverter ou reduzir o nível funcional da floresta, com as seguintes exceções: depois de aumentar o nível funcional da floresta para o Windows Server 2012, você poderá diminuí-lo para o Windows Server 2008 R2. Se Active Directory Lixeira não tiver sido habilitada, você também poderá diminuir o nível funcional da floresta do Windows Server 2012 para o Windows Server 2008 R2 ou Windows Server 2008 ou do Windows Server 2008 R2 de volta para o Windows Server 2008. Se o nível funcional da floresta estiver definido como Windows Server 2008 R2, ele não poderá ser revertido, por exemplo, para o Windows Server 2003.  
  
Depois de definir o nível funcional do domínio para um determinado valor, não será possível reverter ou reduzir o nível funcional do domínio, com as seguintes exceções: ao aumentar o nível funcional do domínio para o Windows Server 2008 R2 ou o Windows Server 2012, e se a floresta função o nível de nal é o Windows Server 2008 ou inferior, você tem a opção de reverter o nível funcional do domínio para o Windows Server 2008 ou o Windows Server 2008 R2. Você pode reduzir o nível funcional do domínio somente do Windows Server 2012 para o Windows Server 2008 R2 ou Windows Server 2008 ou do Windows Server 2008 R2 para o Windows Server 2008. Se o nível funcional do domínio estiver definido como Windows Server 2008 R2, ele não poderá ser revertido, por exemplo, para o Windows Server 2003.  
  
Para saber mais sobre os recursos disponíveis em níveis funcionais inferiores, consulte [Compreendendo os níveis funcionais AD DS (Serviços de Domínio do Active Directory)](../active-directory-functional-levels.md).  
  
Além dos níveis funcionais, um controlador de domínio que executa o Windows Server 2012 fornece recursos adicionais que não estão disponíveis em um controlador de domínio que executa uma versão anterior do Windows Server. Por exemplo, um controlador de domínio que executa o Windows Server 2012 pode ser usado para clonagem do controlador de domínio virtual, enquanto um controlador de domínio que executa uma versão anterior do Windows Server não pode. Mas a clonagem do controlador de domínio virtual e as proteções do controlador de domínio virtual no Windows Server 2012 não têm nenhum requisito de nível funcional.  

> [!NOTE]  
> A floresta do Microsoft Exchange Server 2013 deve ter nível funcional Windows Server 2003 ou superior.  

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a><a name="BKMK_ServerRoles"></a>AD DS interoperabilidade com outras funções de servidor e sistemas operacionais Windows

O AD DS não é compatível com os seguintes sistemas operacionais Windows:  
  
- Windows MultiPoint Server  
- Windows Server 2012 Essentials  
  
O AD DS não pode ser instalado em um servidor que também executa as seguintes funções de servidor ou serviços de função:  
  
- Hyper-V Server  
- Agente de Conexão de Área de Trabalho Remota  
  
## <a name="operations-master-roles"></a><a name="BKMK_OpsMasters"></a>Funções de mestre de operações

Alguns novos recursos do Windows Server 2012 afetam as funções do mestre de operações:  

- O emulador de PDC deve estar executando o Windows Server 2012 para dar suporte à clonagem de controladores de domínio virtuais. Há mais pré-requisitos para clonagem de DCs. Para saber mais, consulte [Virtualização do AD DS (Serviços de Domínio do Active Directory)](https://technet.microsoft.com/library/hh831734.aspx).  
- Novas entidades de segurança são criadas quando o emulador de PDC executa o Windows Server 2012.  
- O Mestre RID contém a nova funcionalidade de emissão e monitoramento de RID. Os aprimoramentos incluem melhorias no registro de log de evento, limites mais adequados e a capacidade de, em uma emergência, aumentar a alocação global do pool de RID em um bit. Para obter mais informações, consulte [Managing RID Issuance](../../ad-ds/manage/Managing-RID-Issuance.md).  

> [!NOTE]  
> Embora não sejam funções de mestre de operações, outra alteração na instalação AD DS é que a função de servidor DNS e o catálogo global são instalados por padrão em todos os controladores de domínio que executam o Windows Server 2012.  

## <a name="virtualizing-domain-controllers"></a><a name="BKMK_Virtual"></a>Virtualizando controladores de domínio

Melhorias no AD DS a partir do Windows Server 2012 permitem a virtualização mais segura de controladores de domínio e a capacidade de clonar controladores de domínio. A clonagem de controladores de domínio, por sua vez, permite a rápida implantação de mais controladores de domínio em um novo domínio, além de outros benefícios. Para obter mais informações, consulte [introdução a &#40;Active Directory Domain Services&#41; nível &#40;de virtualização de AD DS 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  

## <a name="administration-of-windows-server-2012-servers"></a><a name="BKMK_Admin"></a>Administração de servidores do Windows Server 2012

Use o [ferramentas de administração de servidor remoto para o Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) para gerenciar controladores de domínio e outros servidores que executam o Windows Server 2012. Você pode executar o Ferramentas de Administração de Servidor Remoto do Windows Server 2012 em um computador que executa o Windows 8.  

## <a name="application-compatibility"></a><a name="BKMK_AppCompat"></a>Compatibilidade de aplicativos

A tabela a seguir mostra aplicativos da Microsoft comuns integrados ao Active Directory. A tabela abrange as versões do Windows Server nas quais os aplicativos podem ser instalados e informa se a introdução de controladores de domínio do Windows Server 2012 afeta a compatibilidade dos aplicativos.  

|Produto|Anotações|  
|-----------|---------|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|O SharePoint 2010 Service Pack 2 é obrigatório para a instalação e operação <br />do SharePoint 2010 em servidores com Windows Server 2012<br /><br />O SharePoint 2010 Foundation Service Pack 2 é obrigatório para a instalação e operação do SharePoint 2010 Foundation em servidores com Windows Server 2012<br /><br />O processo de instalação do SharePoint Server 2010 (sem Service Packs) apresenta uma falha no Windows Server 2012<br /><br />O instalador de pré-requisito do SharePoint Server 2010 (PrerequisiteInstaller. exe) falha com o erro "este programa tem problemas de compatibilidade". Clicar em "executar o programa sem obter ajuda" exibe o erro "verificando se o SharePoint pode &#124; ser instalado, o sharepoint Server 2010 (sem Service Packs) não pode ser instalado no Windows Server 2012".|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|Requisitos mínimos para um servidor de banco de dados em um farm<br /><br />A edição de 64 bits do Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise ou Datacenter ou edição de 64 bits do Windows Server 2012 Standard ou Datacenter<br /><br />Requisitos mínimos para um servidor único com banco de dados interno:<br /><br />A edição de 64 bits do Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise ou Datacenter ou edição de 64 bits do Windows Server 2012 Standard ou Datacenter<br /><br />Requisitos mínimos para servidores Web front-end e servidores de aplicativos em um farm:<br /><br />A edição de 64 bits do Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise ou Datacenter ou edição de 64 bits do Windows Server 2012 Standard ou Datacenter.|  
|[Configuration Manager 2012](https://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2012 Service Pack 1:<br /><br />A Microsoft adicionará os seguintes sistemas operacionais à matriz de suporte a clientes com o lançamento do Service Pack 1:<br /><br />-Windows 8 pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 datacenter<br /><br />Todas as funções de servidor (incluindo servidores de sites, provedores de SMS e pontos de gerenciamento) podem ser implantadas nos servidores com as seguintes edições do sistema operacional:<br /><br />-Windows Server 2012 Standard<br />-Windows Server 2012 datacenter|  
|[Configuration Manager de ponto de extremidade da Microsoft (Branch atual)](https://docs.microsoft.com/configmgr/core/plan-design/configs/supported-configurations)|[Sistemas operacionais com suporte para servidores do sistema de site Configuration Manager](https://docs.microsoft.com/configmgr/core/plan-design/configs/supported-operating-systems-for-site-system-servers).|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|O Lync Server 2013 requer Windows Server 2008 R2 ou Windows Server 2012. Ele não pode ser executado em uma instalação Server Core. Ele pode ser executado em [servidores virtuais](https://technet.microsoft.com/library/gg399035.aspx).|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|O Lync Server 2010 poderá ser instalado em uma instalação nova (não atualizada) do Windows Server 2012 caso as [atualizações cumulativas do Lync Server de outubro de 2012](https://support.microsoft.com/?kbid=2493736) sejam instaladas. Não há suporte à atualização do sistema operacional para Windows Server 2012 a fim de contemplar uma instalação existente do Lync Server 2010. Também não há suporte ao Servidor de Chat de Grupo do Microsoft Lync Server 2010 no Windows Server 2012.|  
|[System Center 2012 Endpoint Protection](https://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|O System Center 2012 Endpoint Protection Service Pack 1 atualizará a matriz de suporte a clientes para incluir os seguintes sistemas operacionais<br /><br />-Windows 8 pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 datacenter|  
|[System Center 2012 Forefront Endpoint Protection](https://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|O FEP 2010 com Pacote Cumulativo de Atualizações 1 atualizará a matriz de suporte a clientes para incluir os seguintes sistemas operacionais:<br /><br />-Windows 8 pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 datacenter|  
|Forefront Threat Management Gateway (TMG)|A execução do TMG é possível somente no Windows Server 2008 e Windows Server 2008 R2. Para obter mais informações, consulte [Requisitos de sistema para o Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx).|  
|Windows Server Update Services|Esta versão do WSUS já oferece suporte para computadores baseados em Windows 8 ou computadores baseados em Windows Server 2012 como clientes.|  
|Windows Server Update Services 3.0|Atualização do artigo [2734608](https://support.microsoft.com/kb/2734608) do KB permite que servidores que executam o Windows Server Update Services (WSUS) 3,0 SP2 forneçam atualizações para computadores que executam o Windows 8 ou o windows Server 2012: **Observação:** os clientes com ambientes autônomos do wsus 3,0 SP2 ou Configuration Manager 2007 ambientes do Windows Server Service Pack 2 com o WSUS 3,0 SP2 exigem o [2734608](https://support.microsoft.com/kb/2734608) para gerenciar corretamente computadores baseados no Windows 8 ou computadores baseados no Microsoft Azure|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|As edições Standard e Datacenter do Windows Server 2012 oferecem suporte às seguintes funções: mestre de esquema, servidor de catálogo global, controlador de domínio, caixa de correio e função de servidor Acesso para Cliente<br /><br />Nível funcional de floresta: Windows Server 2003 ou mais recente<br /><br />Fonte: Requisitos do sistema do Exchange 2013|  
|Exchange 2010|[Fonte: Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />O Exchange 2010 com Service Pack 3 pode ser instalado em servidores membros do Windows Server 2012.<br /><br />Os[Requisitos de sistema do Exchange 2010](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) listam o mestre de esquema mais recente com suporte, o catálogo global e o controlador de domínio como Windows Server 2008 R2.<br /><br />Nível funcional de floresta: Windows Server 2003 ou mais recente|  
|SQL Server 2012|Fonte: Base de dados de conheicimento [2681562](https://support.microsoft.com/kb/2681562)<br /><br />O SQL Server 2012 RTM conta com suporte no Windows Server 2012.|  
|SQL Server 2008 R2|Fonte: Base de dados de conheicimento [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requer o SQL Server 2008 R2 com Service Pack 1 ou posterior para instalação no Windows Server 2012.|  
|SQL Server 2008|Fonte: Base de dados de conheicimento [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requer o SQL Server 2008 com Service Pack 3 ou posterior para instalação no Windows Server 2012.|  
|SQL Server 2005|Fonte: Base de dados de conheicimento [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Sem suporte à instalação no Windows Server 2012.|  

## <a name="known-issues"></a><a name="BKMK_KnownIssues"></a>Problemas conhecidos

A tabela a seguir apresenta problemas conhecidos relacionados à instalação do AD DS.  

||||  
|-|-|-|  
|Número e título do artigo da KB|Área tecnológica influenciada|Problema/descrição|  
|[2830145](https://support.microsoft.com/kb/2830145): Não é possível mapear o SID S-1-18-1 e SID S-1-18-2 em computadores com Windows 7 ou Windows Server 2008 R2 em um ambiente de domínio|Gerenciamento do AD DS/Compatibilidade do aplicativo|Aplicativos que mapeiam o SID S-1-18-1 e SID S-1-18-2, que são novos no Windows Server 2012, podem falhar, pois os SIDs não poderão ser resolvidos em computadores com Windows 7 ou Windows Server 2008 R2. Para resolver este problema, instale o hotfix em computadores com o Windows 7 e Windows Server 2008 R2 no domínio.|  
|[2737129](https://support.microsoft.com/kb/2737129): A preparação de Política de Grupo não é executada quando você prepara automaticamente um domínio existente para o Windows Server 2012|Instalação do AD DS|Adprep/domainprep/gpprep não é automaticamente executado como parte da instalação do primeiro controlador de domínio que executa o Windows Server 2012 em um domínio. Caso ele nunca tenha sido executado no domínio, a execução deverá ser manual.|  
|[2737416](https://support.microsoft.com/kb/2737416): Avisos de repetições de implantação do controlador de domínio baseado em Windows PowerShell|Instalação do AD DS|Os avisos podem aparecer durante a validação de pré-requisitos e surgir novamente durante a instalação.|  
|[2737424](https://support.microsoft.com/kb/2737424): Erro "O formato do nome de domínio especificado é inválido" ao tentar remover os Serviços de Domínio Active Directory de um controlador de domínio|Instalação do AD DS|Este erro aparece quando você remove o último controlador de domínio em um domínio que ainda contém contas RODC pré-criadas. Isso afeta o Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008.|  
|[2737463](https://support.microsoft.com/kb/2737463): O controlador de domínio não inicia, o erro c00002e2 ocorre ou a mensagem "Selecione uma opção" é exibida|Instalação do AD DS|Um controlador de domínio não é iniciado porque um administrador usou Dism.exe, Pkgmgr.exe ou Ocsetup.exe para remover a função DirectoryServices-DomainController.|  
|[2737516](https://support.microsoft.com/kb/2737516): Limitações de verificação IFM no Gerenciador do Servidor do Windows Server 2012|Instalação do AD DS|A verificação IFM pode apresentar limitações, conforme explica no artigo da KB.|  
|[2737535](https://support.microsoft.com/kb/2737535): Cmdlet Install-AddsDomainController retorna erro de conjunto de parâmetros para RODC|Instalação do AD DS|Você poderá receber um erro ao tentar conectar um servidor a uma conta RODC caso especifique argumentos que já foram preenchidos na conta RODC pré-criada.|  
|[2737560](https://support.microsoft.com/kb/2737560): Erro "Não é possível realizar a verificação de conflitos de esquema do Exchange" e falha na verificação de pré-requisitos|Instalação do AD DS|A verificação de pré-requisitos falha quando você configura o primeiro controlador de domínio do Windows Server 2012 DC em um domínio existente, pois os controladores de domínio não possuem SeServiceLogonRight para o Serviço de Rede ou porque os protocolos WMI ou DCOM estão bloqueados.|  
|[2737797](https://support.microsoft.com/kb/2737797): O módulo AddsDeployment com argumento -Whatif mostra resultados de DNS incorretos|Instalação do AD DS|O parâmetro-WhatIf mostra que o servidor DNS não será instalado, mas será.|  
|[2737807](https://support.microsoft.com/kb/2737807): O botão Avançar não está disponível na página Opções do Controlador de Domínio|Instalação do AD DS|O botão Avançar é desabilitado na página Opções do Controlador de Domínio porque o endereço IP do controlador de domínio de destino não faz o mapeamento para uma sub-rede ou site existente, ou porque a senha de DSRM não foi digitada e confirmada corretamente.|  
|[2737935](https://support.microsoft.com/kb/2737935): A instalação do Active Directory fica parada no estágio "Criando o objeto de Configurações NTDS"|Instalação do AD DS|A instalação não continua porque a senha do administrador local corresponde à senha do administrador do domínio ou porque há problemas de rede que impedem a conclusão da replicação crítica.|  
|[2738060](https://support.microsoft.com/kb/2738060): Mensagem de erro "Acesso negado" ao criar um domínio filho remotamente usando Install-AddsDomain|Instalação do AD DS|Você recebe um erro ao executar Install-ADDSDomain com o cmdlet Invoke-Command quando DNSDelegationCredential possui uma senha inválida.|  
|[2738697](https://support.microsoft.com/kb/2738697): Erro de configuração do controlador de domínio "O servidor não está operacional" quando você configura um servidor usando o Gerenciador do Servidor|Instalação do AD DS|Você recebe este erro quando tenta instalar o AD DS em um computador do grupo de trabalho porque a autenticação NTLM está desabilitada.|  
|[2738746](https://support.microsoft.com/kb/2738746): Você recebe erros de acesso negado após fazer logon em uma conta de domínio de administrador local|Instalação do AD DS|Quando você faz logon usando uma conta de administrador local em vez de uma conta de administrador interna e cria um novo domínio, a conta não é adicionada ao grupo Administradores do Domínio.|  
|[2743345](https://support.microsoft.com/kb/2743345): Erro "O sistema não pode encontrar o arquivo especificado" do Adprep/gpprep ou falha da ferramenta|Instalação do AD DS|Você recebe este erro quando executa o adprep/gpprep, pois o mestre da infraestrutura implementa um namespace não contíguo|  
|[2743367](https://support.microsoft.com/kb/2743367): Erro "não é um aplicativo Win32 válido" do addprep no Windows Server 2003, versão de 64 bits|Instalação do AD DS|Você recebe este erro pois o Adprep do Windows Server 2012 não pode ser executado no Windows Server 2003.|  
|[2753560](https://support.microsoft.com/kb/2753560): Erros de instalação de ADMT 3.2 e PES 3.1 no Windows Server 2012|ADMT|Por padrão, o ADMT 3.2 não pode ser instalado no Windows Server 2012.|  
|[2750857](https://support.microsoft.com/kb/2750857): Relatórios de diagnóstico de Replicação do DFS não são exibidos corretamente no Internet Explorer 10|Replicação do DFS|O relatório de Replicação DFS não é exibido corretamente em função de alterações no Internet Explorer 10.|  
|[2741537](https://support.microsoft.com/kb/2741537): Atualizações de Política de Grupo Remota são visíveis aos usuários|Política de Grupo|Isso ocorre em função de tarefas agendadas executadas no contexto de cada usuário que fez logon. O projeto do Agendador de Tarefas do Windows requer um prompt interativo neste cenário.|  
|[2741591](https://support.microsoft.com/kb/2741591): Os arquivos ADM não estão presentes no SYSVOL na opção de status de infraestrutura GPMC|Política de Grupo|A replicação de GP pode relatar "replicação em andamento" porque o status de infraestrutura do GPMC não segue regras de filtragem personalizadas.|  
|[2737880](https://support.microsoft.com/kb/2737880): Erro "Não é possível iniciar o serviço" durante a configuração do AD DS|Clonagem de controlador de domínio virtual|Você recebe este erro ao instalar/remover o AD DS ou realizar clonagens, pois o serviço Servidor de Função de SD está desabilitado.|  
|[2742836](https://support.microsoft.com/kb/2742836): Duas concessões de DHCP são criadas para cada controlador de domínio quando você usa o recurso de clonagem de controlador de domínio virtual|Clonagem de controlador de domínio virtual|Isso ocorre porque o controlador de domínio clonado recebeu uma concessão antes da clonagem e recebeu outra quando a clonagem foi concluída.|  
|[2742844](https://support.microsoft.com/kb/2742844): O controlador de domínio falha e o servidor é reiniciado em DSRM no Windows Server 2012|Clonagem de controlador de domínio virtual|O controlador de domínio clonado é iniciado em DSRM, pois houve uma falha de clonagem por qualquer um dos motivos listados no artigo da KB.|  
|[2742874](https://support.microsoft.com/kb/2742874): A clonagem do controlador de domínio não cria todos os nomes de entidades de serviço novamente|Clonagem de controlador de domínio virtual|Alguns SPNs de três partes não são criados novamente no controlador de domínio clonado em função de uma limitação do processo de renomeação de domínio.|  
|[2742908](https://support.microsoft.com/kb/2742908): Erro "Nenhum servidor de logon disponível" após clonar controlador de domínio|Clonagem de controlador de domínio virtual|Você recebe este erro quando tenta fazer logon após clonar um controlador de domínio virtualizado, pois há uma falha de clonagem e o controlador de domínio é iniciado em DSRM. Faça logon como .\administrator para corrigir a falha de clonagem.|  
|[2742916](https://support.microsoft.com/kb/2742916): Falha na clonagem do controlador de domínio com erro 8610 em dcpromo.log|Clonagem de controlador de domínio virtual|Ocorre uma falha na clonagem porque o emulador PDC não realizou replicação de entrada da partição do domínio, provavelmente em função da transferência da função.|  
|[2742927](https://support.microsoft.com/kb/2742927): Erro "O índice está fora do intervalo" em New-AdDcCloneConfig|Clonagem de controlador de domínio virtual|Você recebe este erro após executar o cmdlet New-ADDCCloneConfigFile no momento da clonagem de controladores de domínio virtuais, porque o cmdlet não foi executado a partir de um prompt de comando elevado ou porque o token de acesso não contém o grupo Administradores.|  
|[2742959](https://support.microsoft.com/kb/2742959): Falha na clonagem do controlador de domínio com erro 8437: "Um parâmetro inválido foi especificado para esta operação de replicação"|Clonagem de controlador de domínio virtual|Houve uma falha na clonagem porque foi especificado um nome inválido para o clone ou um nome de NetBIOS duplicado.|  
|[2742970](https://support.microsoft.com/kb/2742970): Falha na clonagem de controlador de domínio sem DSRM, origem duplicada e computador clone|Clonagem de controlador de domínio virtual|O controlador de domínio virtual clonado é iniciado no DSRM (Modo de Reparo de Serviços de Diretório) usando um nome duplicado como controlador de domínio de origem, pois o arquivo DCCloneConfig.xml não foi criado no local correto ou porque o controlador de domínio de origem foi reiniciado antes da clonagem.|  
|[2743278](https://support.microsoft.com/kb/2743278): Erro de clonagem do controlador de domínio 0x80041005|Clonagem de controlador de domínio virtual|O controlador de domínio virtual é iniciado em DSRM porque somente um servidor WINS foi especificado. Caso seja especificado algum servidor WINS, será necessário especificar os servidores WINS preferencial e alternativo.|  
|[2745013](https://support.microsoft.com/kb/2745013): Mensagem de erro "O servidor não está operacional" quando New-AdDcCloneConfigFile é executado no Windows Server 2012|Clonagem de controlador de domínio virtual|Você recebe este erro após executar o cmdlet New-ADDCCloneConfigFile porque o servidor não pode entrar em contato com um servidor de catálogo global.|  
|[2747974](https://support.microsoft.com/kb/2747974): O evento 2224 de clonagem do controlador de domínio fornece orientações incorretas|Clonagem de controlador de domínio virtual|O evento com ID 2224 informa incorretamente que as contas de serviço gerenciado devem ser removidas antes da clonagem. As MSAs autônomas devem ser removidas, mas as MSAs de grupo não bloqueiam a clonagem.|  
|[2748266](https://support.microsoft.com/kb/2748266): Não é possível desbloquear uma unidade criptografada com BitLocker após a atualização para Windows 8|BitLocker|Você recebe um erro "aplicativo não encontrado" ao tentar desbloquear uma unidade em um computador que foi atualizado do Windows 7.|  

## <a name="see-also"></a>Veja também

[Recursos de avaliação do Windows Server 2012](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Guia de avaliação do Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[Instalar e implantar o Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
