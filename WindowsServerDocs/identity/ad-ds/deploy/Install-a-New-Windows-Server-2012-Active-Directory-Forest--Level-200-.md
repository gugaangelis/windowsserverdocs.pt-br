---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: "Instalar uma nova floresta Windows Server 2012 do Active Directory (nível 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b8a7502a1b9d27b0f61353f2544182a64d311496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Instalar uma nova floresta Windows Server 2012 do Active Directory (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica o novos serviços de domínio do Active Directory do Windows Server 2012 domínio controlador promoção recurso em um nível introdutório. No Windows Server 2012, AD DS substitui a ferramenta Dcpromo com o Gerenciador do servidor e sistema de implantação com base em Windows PowerShell.  
  
-   [Administração simplificada de serviços de domínio do Active Directory](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Visão geral técnica](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [Implantando uma floresta com o Gerenciador do servidor](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Implantando uma floresta com o Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Administração simplificada de serviços de domínio do Active Directory  
Windows Server 2012 apresenta a próxima geração de administração simplificada domínio serviços do Active Directory e é o mais radical domínio novamente concepção desde o Windows 2000 Server. Administração simplificada do AD DS leva lições aprendidas com 12 anos do Active Directory e proporciona uma experiência de administrativa mais intuitiva mais com suporte, mais flexível, arquitetos e administradores. Isso significa criar novas versões do existente tecnologias, bem como estender as capacidades dos componentes lançados no Windows Server 2008 R2.  
  
### <a name="what-is-ad-ds-simplified-administration"></a>Qual é o AD DS simplificado administração?  
Administração simplificada do AD DS é uma releitura da implantação de domínio. Alguns desses recursos incluem:  
  
-   Implantação de função do AD DS agora faz parte da nova arquitetura Gerenciador do servidor e permite a instalação remota.  
  
-   O mecanismo de implantação e configuração do AD DS agora está em Windows PowerShell, mesmo quando usando uma configuração de gráfica.  
  
-   Promoção agora inclui a verificação de pré-requisitos que valida a preparação de floresta e domínio novo controlador de domínio, reduzindo a chance de promoções com falha.  
  
-   Floresta do Windows Server 2012 funcional nível não implementar novos recursos e o nível funcional do domínio é necessário somente para um subconjunto dos novos recursos de Kerberos, liberando os administradores das frequentes precisa para um ambiente de controlador de domínio homogêneo.  
  
### <a name="purpose-and-benefits"></a>Finalidade e os benefícios  
Essas alterações podem aparecer mais complexos, não mais simples. Em reformular o processo de implantação do AD DS, porém, havia oportunidade para unir várias etapas e práticas recomendadas para ações menos, mais fácil. Por exemplo, isso significa que a configuração gráfica de um novo controlador de domínio da réplica agora está oito caixas de diálogo, em vez do doze anterior. Criar uma nova floresta do Active Directory exige um *único* comando do Windows PowerShell com apenas *um* argumento: o nome do domínio.  
  
Por que há tal ênfase no Windows PowerShell no Windows Server 2012? Conforme computação distribuída evolui, o Windows PowerShell permite que um único mecanismo para configuração e manutenção de ambos os gráficos e interfaces de linha de comando. Ela permite que os scripts completos de qualquer componente com a mesma cidadania de primeira classe para o profissional de TI que concede a uma API para desenvolvedores. Conforme computação baseada em nuvem se tornar todos os lugares, o Windows PowerShell também finalmente traz a capacidade de administrar remotamente um servidor, em que um computador sem interface gráfica tem os mesmos recursos de gerenciamento como um com um mouse e monitor.  
  
Um administrador do AD DS veterano deverá encontrar seu conhecimento anterior altamente relevante. Um administrador início encontrará uma curva de aprendizado muito hierarquias.  
  
## <a name="BKMK_TechOverview"></a>Visão geral técnica  
  
### <a name="what-you-should-know-before-you-begin"></a>O que você deve saber antes de começar  
Este tópico pressupõe familiaridade com versões anteriores dos serviços de domínio do Active Directory e não fornece detalhes básicos em torno de sua finalidade e funcionalidade. Para obter mais informações sobre o AD DS, consulte as páginas do Portal do TechNet vinculadas abaixo:  
  
-   [Serviços de domínio do Active Directory para Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Serviços de domínio do Active Directory para Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Referência técnica do Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>Descrições funcionais  
  
#### <a name="ad-ds-role-installation"></a>Instalação do AD DS função  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
Instalação do Active Directory serviços de domínio usa Gerenciador do servidor e o Windows PowerShell, como todas as outras funções de servidor e recursos no Windows Server 2012. O programa Dcpromo.exe não fornece mais opções de configuração de GUI.  
  
Você pode usar um assistente gráfico no Gerenciador do servidor ou o módulo ServerManager para Windows PowerShell em instalações locais e remotas. Executando várias instâncias desses assistentes ou cmdlets e segmentando servidores diferentes, você pode implantar AD DS para vários controladores de domínio simultaneamente, tudo a partir de um único console. Embora esses novos recursos não são compatíveis com o Windows Server 2008 R2 ou sistemas operacionais anteriores, você ainda pode usar o aplicativo de Dism.exe introduzido no Windows Server 2008 R2 para instalação da função local do clássico de linha de comando.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>Configuração do AD DS função  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Configuração de serviços de domínio do diretório ativa "anteriormente conhecida como DCPROMO" é um enquanto uma operação discreta de instalação da função. Depois de instalar a função AD DS, o administrador configura o servidor como um controlador de domínio usando um assistente separado dentro de Gerenciador do servidor ou usando o módulo ADDSDeployment Windows PowerShell.  
  
Configuração de função do AD DS baseia doze anos de experiência de campo e configura agora controladores de domínio com base nas recomendações mais recentes da Microsoft. Por exemplo, o sistema de nomes de domínio e catálogos globais instalar por padrão em todos os controladores de domínio.  
  
O Assistente de configuração do Gerenciador do servidor AD DS mescla várias caixas de diálogo individuais menos prompts e não é mais oculta as configurações em um modo "Avançado". O processo de promoção inteira é uma caixa de diálogo expansão durante a instalação. O assistente e o módulo ADDSDeployment Windows PowerShell mostram alterações importantes e questões de segurança, com links para mais informações.  
  
O Dcpromo.exe permanece no Windows Server 2012 para instalações autônomas de linha de comando apenas e não executa o Assistente para instalação gráfica. Ele é altamente recomendável que você interromper o uso de Dcpromo.exe para instalações autônomas e substituí-lo com o módulo ADDSDeployment, conforme o executável preterido agora não será incluído na próxima versão do Windows.  
  
Esses novos recursos não são compatíveis com versões anteriores ao Windows Server 2008 R2 ou sistemas operacionais mais antigos.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe não contém um assistente gráfico e não instala binários de função ou recurso. Tentar executar o Dcpromo.exe do retorna de shell Explorer:  
>   
> "O Assistente de instalação dos serviços de domínio Active Directory é realocar no Gerenciador do servidor. Para obter mais informações, consulte https://go.microsoft.com/fwlink/?LinkId=220921."  
>   
> Tentativa de executar o Dcpromo.exe /Unattend ainda instala os binários, como em sistemas operacionais anteriores, mas avisa:  
>   
> "O dcpromo operação autônoma é substituída pelo módulo de ADDSDeployment para Windows PowerShell. Para obter mais informações, consulte https://go.microsoft.com/fwlink/?LinkId=220924."  
>   
> Windows Server 2012 reprova o dcpromo.exe e não serão incluído com versões futuras do Windows, nem ele receberá ainda mais melhorias nesse sistema operacional. Os administradores devem interromper o uso e alterne para os módulos do Windows PowerShell com suporte caso desejem criar controladores de domínio na linha de comando.  
  
#### <a name="prerequisite-checking"></a>Verificação de pré-requisitos  
Configuração do controlador de domínio também implementa uma fase de verificação de pré-requisito que avalia a floresta e domínio antes de continuar com a promoção de controlador de domínio. Isso inclui a disponibilidade de função FSMO, privilégios de usuário, esquema estendida compatibilidade e outros requisitos. Esse novo design diminui os problemas onde promoção de controlador de domínio é iniciado e, em seguida, interrompe midway com um erro fatal configuração. Isso reduz a possibilidade de metadados do controlador de domínio órfã na floresta ou um servidor que incorretamente acredita que ele é um controlador de domínio.  
  
## <a name="BKMK_SMForest"></a>Implantando uma floresta com o Gerenciador do servidor  
Esta seção explica como instalar o primeiro controlador de domínio em um domínio raiz da floresta usando o Gerenciador do servidor em um computador Windows Server 2012 gráfico.  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>Processo de instalação de função do Gerenciador do servidor AD DS  
O diagrama a seguir ilustra o processo de instalação da função Serviços de domínio do Active Directory, começando com você executando ServerManager.exe e final logo antes da promoção do controlador de domínio.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>Server Pool e adicionar funções  
Todos os computadores Windows Server 2012 acessíveis no computador que executa o Gerenciador do servidor são qualificados para o pool. Depois que agrupado, você selecionar os servidores de instalação remota do AD DS ou outras opções de configuração possíveis dentro do Gerenciador do servidor.  
  
Para adicionar servidores, escolha um destes procedimentos:  
  
-   Clique em **adicionar outros servidores para gerenciar** no bloco de boas-vindas do painel  
  
-   Clique no **gerenciar** menu e selecione **adicionar servidores**  
  
-   Clique com botão direito **todos os servidores** e escolha **adicionar servidores**  
  
Isso abre a caixa de diálogo Adicionar servidores:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
Isso oferece três maneiras de adicionar servidores ao pool de para uso ou agrupamento:  
  
-   Pesquisa do Active Directory (usa LDAP, requer que os computadores pertencem a um domínio, permite a filtragem de sistema operacional e dá suporte a curingas)  
  
-   Pesquisa DNS (alias DNS usa ou endereço IP por meio de transmissão ARP ou NetBIOS ou pesquisa WINS, não permitir curingas de filtragem ou o suporte do sistema operacional)  
  
-   Importar (usa uma lista de arquivos de texto de servidores separados por RC/AL)  
  
Clique em **Localizar agora** para retornar uma lista de servidores do domínio do Active Directory mesmo que o computador tenha ingressado em, clique em um ou mais nomes de servidor da lista de servidores. Clique na seta para a direita para adicionar os servidores para o **selecionadas** lista. Use o **adicionar servidores** caixa de diálogo Adicionar servidores selecionados para grupos de função de painel. Ou clique em **gerenciar**e clique em **criar grupo de servidores**, ou clique em **criar grupo de servidores** no painel **bem-vindo ao Gerenciador do servidor** bloco para criar grupos de servidores personalizado.  
  
> [!NOTE]  
> O procedimento de adicionar servidores não valida que um servidor é acessível ou online. No entanto, todos os servidores inacessíveis sinalizar na exibição de capacidade de gerenciamento no Gerenciador do servidor após a próxima atualização  
  
Você pode instalar funções remotamente em qualquer Windows Server 2012 computadores adicionados pool, conforme mostrado:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
Você não poderá gerenciar totalmente servidores que executam sistemas operacionais anteriores ao Windows Server 2012. O **adicionar funções e recursos** seleção está em execução ServerManager Windows PowerShell módulo **Install-WindowsFeature**.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
Você também pode usar o painel do Gerenciador do servidor em um controlador de domínio existentes para selecionar a instalação do servidor remoto AD DS com a função já pré-selecionado direito clicando no bloco de painel de controle do AD DS e selecionando **adicionar AD DS para outro servidor**. Isso está invocando **Install-WindowsFeature AD--serviços de domínio**.  
  
O computador que estiver executando o Gerenciador do servidor no próprio pool automaticamente. Para instalar a função do AD DS aqui, basta clicar o **gerenciar** menu e clique em **adicionar funções e recursos**.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>Tipo de instalação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
O **tipo de instalação** caixa de diálogo fornece uma opção que não oferece suporte a serviços de domínio do Active Directory: a **serviços de área de trabalho remota cenário com base em instalação**. Essa opção só permite que os serviços de área de trabalho remota em uma carga de trabalho distribuída de vários servidor. Se você marcá-la, não é possível instalar o AD DS.  
  
Sempre deixar a seleção padrão in-loco ao instalar o AD DS: **instalação baseada em função ou recurso com base em**.  
  
#### <a name="server-selection"></a>Seleção de servidor  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
O **seleção de servidor** caixa de diálogo permite que você escolha de um dos servidores adicionados anteriormente para o pool, desde que seja acessível. O servidor local que executa o Gerenciador do servidor está disponível automaticamente.  
  
Além disso, você pode selecionar arquivos VHD do Hyper-V offline com o sistema operacional Windows Server 2012 e Gerenciador do servidor adiciona a função-los diretamente por meio de manutenção do componente. Isso permite que você provisione servidores virtuais com os componentes necessários antes de configurá-los ainda mais.  
  
#### <a name="server-roles-and-features"></a>Recursos e funções de servidor  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
Selecione o **Active Directory Domain Services** função se você pretende promover um controlador de domínio. Todos os recursos de administração do Active Directory e serviços necessários instalam automaticamente, mesmo se eles ostensivamente fazem parte de outra função ou não aparecem selecionados na interface do Gerenciador do servidor.  
  
Gerenciador do servidor também apresenta uma caixa de diálogo informativa que mostra quais recursos de gerenciamento essa função implicitamente instala; Isso equivale ao **- IncludeManagementTools** argumento.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
Adicionais **recursos** pode ser adicionada aqui conforme desejado.  
  
#### <a name="active-directory-domain-services"></a>Serviços de domínio do Active Directory  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
O **Active Directory Domain Services** caixa de diálogo fornece informações limitadas sobre os requisitos e as práticas recomendadas. Ela principalmente atua como uma confirmação de que você escolheu a função AD DS "se essa tela não aparecer, você não selecionou AD DS.  
  
#### <a name="confirmation"></a>Confirmação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
O **confirmação** caixa de diálogo é o ponto de verificação final antes do início da instalação da função. Ele oferece uma opção para reiniciá-lo conforme necessário após a instalação da função, mas a instalação do AD DS não requer uma reinicialização.  
  
Ao clicar em **instalar**, você confirmar que você está pronto para começar a instalação da função. Você não pode cancelar uma instalação da função depois que ele é iniciado.  
  
#### <a name="results"></a>Resultados  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
O **resultados** caixa de diálogo mostra o progresso da instalação atual e o status atual de instalação. Instalação da função continua independentemente se o Gerenciador do servidor é fechado.  
  
Verificando os resultados da instalação ainda é uma prática recomendada. Se você fechar o **resultados** caixa de diálogo antes da conclusão da instalação, você pode verificar os resultados usando o sinalizador de notificação do Gerenciador do servidor. Gerenciador do servidor também mostra uma mensagem de aviso para todos os servidores que tiver instalado a função AD DS, mas não ainda mais foi configurado como controladores de domínio.  
  
**Notificações de tarefa**  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**Detalhes do AD DS**  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**Detalhes da tarefa**  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>Promover o controlador de domínio  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
No final da instalação da função AD DS, você pode continuar com a configuração usando o **promover esse servidor para um controlador de domínio** link. Isso é necessário para tornar o servidor um controlador de domínio, mas não é necessário executar o Assistente de configuração imediatamente. Por exemplo, você somente pode querer provisionar servidores com os binários do AD DS antes de enviá-los para outro filial para configuração posterior. Adicionando a função do AD DS antes do envio, você economiza tempo quando chegar a seu destino. Você também pode seguir a prática recomendada de não manter um controlador de domínio offline por dias ou semanas. Por fim, isso permite que você atualize componentes antes de promoção de controlador de domínio, economizando pelo menos uma reinicialização subsequente.  
  
Selecionar este link posteriormente invoca os cmdlets ADDSDeployment: **addsforest instalar**, **instalar addsdomain**, ou **instalar addsdomaincontroller**.  
  
### <a name="uninstallingdisabling"></a>Desinstalar/desabilitando  
Remova a função do AD DS como qualquer outra função, independentemente se promovido o servidor para um controlador de domínio. No entanto, removendo a função AD DS requer uma reinicialização após a conclusão.  
  
Remoção de função de serviços de domínio do diretório ativa é diferente da instalação, que requer rebaixamento de controlador de domínio para que ele possa concluir. Isso é necessário para impedir que um controlador de domínio tenham seus binários de função desinstalados sem limpeza de metadados adequados na floresta. Para obter mais informações, consulte [rebaixando controladores de domínio e domínios & #40; Nível 200 & #41; ](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> Depois de promoção para um controlador de domínio não é suportada e impedir que o servidor seja inicializado normalmente, removendo as funções do AD DS com Dism.exe ou o módulo DISM do Windows PowerShell.  
>   
> Ao contrário do Gerenciador do servidor ou o módulo de implantação do AD DS para o Windows PowerShell, o DISM é um sistema de manutenção nativo que tem nenhum conhecimento inerente do AD DS ou sua configuração. Não use Dism.exe ou o módulo DISM do Windows PowerShell para desinstalar a função AD DS, a menos que o servidor não é um controlador de domínio.  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Criar um domínio de raiz da floresta do AD DS com o Gerenciador do servidor  
O diagrama a seguir ilustra o processo de configuração de serviços de domínio do Active Directory, no caso em que você instalou a função AD DS anteriormente e começar a **Assistente de configuração dos serviços do Active Directory domínio** usando o Gerenciador do servidor.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>Configuração da implantação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
Gerenciador do servidor começa cada promoção de controlador de domínio com o **implantação configuração** página. As opções restantes e campos obrigatórios alterar essa página e as páginas subsequentes, dependendo de qual operação de implantação que você selecionar.  
  
Para criar uma nova floresta do Active Directory, clique em **adicionar uma nova floresta**. Você deve fornecer um nome de domínio raiz válido; o nome não pode ser rotulada única (por exemplo, o nome deve ser *contoso.com* ou semelhantes e não apenas *contoso*) e deve usar permitidos requisitos de nomenclatura do domínio DNS.  
  
Para obter mais informações sobre nomes de domínio válido, consulte o artigo KB [convenções no Active Directory de nomenclatura para computadores, domínios, sites e UOs](https://support.microsoft.com/kb/909264).  
  
> [!WARNING]  
> Não crie novos florestas do Active Directory com o mesmo nome de um nome DNS externo. Por exemplo, se sua URL de DNS de Internet é http://contoso.com, você deve escolher um nome diferente para sua floresta interna evitar problemas de compatibilidade futura. Esse nome deve ser exclusivo e improváveis para tráfego da web. Por exemplo: corp.contoso.com.  
  
Uma nova floresta não precisa novas credenciais de conta de administrador do domínio. O processo de promoção de controlador de domínio usa as credenciais de conta de administrador interno do controlador de domínio primeiro usado para criar a raiz da floresta. Há nenhuma maneira (por padrão) para desativar ou de bloqueio de conta de administrador interno e pode ser o ponto de entrada único em uma floresta se outras contas de domínio administrativo são inutilizáveis. É essencial saber a senha antes de implantar uma nova floresta.  
  
**DomainName** requer um nome DNS de domínio totalmente qualificado válido e é necessária.  
  
#### <a name="domain-controller-options"></a>Opções de controlador de domínio  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
O **opções de controlador de domínio** permite que você configure o **nível funcional da floresta** e **nível funcional do domínio** para domínio raiz da floresta de novo. Por padrão, essas configurações são Windows Server 2012 em um domínio raiz da nova floresta. O nível funcional da floresta Windows Server 2012 não fornece nenhuma funcionalidade de novo sobre o nível funcional da floresta Windows Server 2008 R2. O nível funcional de domínio do Windows Server 2012 é necessário somente para implementar as novas configurações de Kerberos "sempre fornecer declarações" e "Falha solicitações de autenticação unarmored". Um uso principal para níveis funcionais no Windows Server 2012 é restringir participação em controladores de domínio para domínio que atendem aos requisitos do sistema operacional mínimo permitido. Em outras palavras, você pode especificar o domínio podem hospedar o Windows Server 2012 domínio funcional nível somente controladores de domínio que executam o Windows Server 2012.  Windows Server 2012 implementa um sinalizador de controlador de domínio novo chamado **DS_WIN8_REQUIRED** no **DSGetDcName** função de logon de rede que exclusivamente localiza controladores de domínio do Windows Server 2012. Isso permite a flexibilidade de uma floresta mais homogênea ou heterogênea em termos de quais sistemas operacionais têm permissão para ser executado em controladores de domínio.  
  
Para obter mais informações sobre o controlador de domínio local, examinar [funções de serviço de diretório](https://msdn.microsoft.com/library/ms675900(VS.85).aspx).  
  
A funcionalidade de controlador de domínio só configurável é a opção de servidor DNS. A Microsoft recomenda que todos os controladores de domínio fornecem serviços DNS para alta disponibilidade em ambientes distribuídos, por isso, essa opção é selecionada por padrão ao instalar um controlador de domínio em qualquer modo ou domínio. O Catálogo Global e opções de controlador de domínio somente leitura não estão disponíveis ao criar um novo domínio raiz; o primeiro controlador de domínio deve ser um GC e não pode ser um controlador de domínio somente leitura (RODC).  
  
Especificado **senha do modo de restauração dos serviços de diretório** devem cumprir a política de senha aplicada ao servidor, que, por padrão, não exige uma senha forte; somente um não-vazia. Sempre escolha uma senha forte e complexa ou preferencialmente, uma senha.  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>Opções de DNS e credenciais de delegação de DNS  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
O **DNS opções** página permite que você configurar a delegação de DNS e fornecer credenciais administrativas de DNS alternativas.  
  
Você não pode configurar opções de DNS ou delegação no Assistente de configuração de serviços de domínio Active Directory ao instalar uma nova floresta raiz domínio do Active Directory em que você selecionou o **servidor DNS** sobre o **opções de controlador de domínio** página. O **delegação DNS criar** opção está disponível durante a criação de uma zona DNS raiz nova floresta em uma infraestrutura de servidor DNS existente. Essa opção permite que você forneça credenciais administrativas do alternativas DNS que tem os direitos para atualizar a zona DNS.  
  
Para obter mais informações sobre se você precisa criar uma delegação de DNS, consulte [delegação de zona de Noções básicas sobre](https://technet.microsoft.com/library/cc771640.aspx).  
  
#### <a name="additional-options"></a>Opções adicionais  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
O **opções adicionais** página mostra o nome NetBIOS do domínio e permite que você para ignorá-la. Por padrão, o nome NetBIOS do domínio corresponde ao rótulo de extrema esquerda do nome do domínio totalmente qualificado fornecido no **implantação configuração** página. Por exemplo, se você tiver fornecido o nome de domínio totalmente qualificado do corp.contoso.com, o nome de domínio padrão NetBIOS é CORP.  
  
Se o nome é 15 caracteres ou menos e não entre em conflito com outro nome NetBIOS, sejam alterado. Se ele está em conflito com outro nome NetBIOS, um número é acrescentado ao nome. Se o nome tiver mais de 15 caracteres, o assistente fornece uma sugestão exclusiva, truncada. Em ambos os casos, o assistente primeiro valida o nome não ainda estiver em uso por meio de uma pesquisa WINS e NetBIOS transmitido.  
  
Para obter mais informações sobre nomes de domínio válido, consulte o artigo KB [convenções no Active Directory de nomenclatura para computadores, domínios, sites e UOs](https://support.microsoft.com/kb/909264).  
  
#### <a name="paths"></a>Caminhos  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
O **caminhos** página permite substituir os locais de pastas padrão do AD DS banco de dados, os logs de transação de banco de dados, e o SYSVOL compartilhar. Os locais padrão estão sempre em subpastas da pasta % systemroot % (ou seja, C:\Windows).  
  
#### <a name="review-options-and-view-script"></a>Opções de revisão e exibir Script  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
O **opções de revisão** página permite que você validar as configurações e certifique-se de que eles atender às suas necessidades antes de iniciar a instalação. Isso não é a última oportunidade de parar a instalação ao usar o Gerenciador do servidor. Isso é simplesmente uma opção para confirmar as configurações antes de continuar a configuração  
  
O **opções de revisão** página no Gerenciador do servidor também oferece um recurso opcional **Exibir Script** botão para criar um arquivo de texto Unicode que contém a configuração ADDSDeployment atual como um único script do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de configuração de serviços de domínio Active Directory para configurar as opções, exportar a configuração e, em seguida, cancelá-lo. Esse processo cria um exemplo válido e sintaticamente correto para ainda mais a modificação ou uso direto. Por exemplo:  
  
```powershell 
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSForest `  
-CreateDNSDelegation `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainName "corp.contoso.com" `  
-DomainNetBIOSName "CORP" `  
-ForestMode "Win2012" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NoRebootOnCompletion:$false `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Gerenciador do servidor normalmente preenche todos os argumentos com valores ao promover e não dependem de padrões (como eles podem mudar entre as versões futuras do Windows ou service packs). A única exceção a isso é o **- safemodeadministratorpassword** argumento (que é omitido deliberadamente do script). Para forçar um prompt de confirmação, omita o valor ao executar o cmdlet interativamente.  
  
#### <a name="prerequisites-check"></a>Seleção de pré-requisitos  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
O **pré-requisitos verificar** é um novo recurso na configuração de domínio do AD DS. Essa nova fase valida se a configuração do servidor é capaz de dar suporte a uma nova floresta do AD DS.  
  
Ao instalar um novo domínio raiz, o servidor Manager Assistente domínio Active Directory Services configuração invoca uma série de testes modulares. Esses testes alertarão-lo com opções de reparo sugeridos. Você pode executar os testes quantas vezes for necessário. O processo de controlador de domínio não pode continuar até que todos os pré-requisitos testes passar.  
  
O **pré-requisitos verificar** superfícies também informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.  
  
Para obter mais informações sobre as verificações de pré-requisito específicas, consulte [pré-requisito verificando](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
#### <a name="installation"></a>Instalação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
Quando o **instalação** página exibe, a configuração de controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas exibem nesta página e são gravadas nos logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> Você pode executar vários assistentes de configuração do AD DS e instalação da função simultaneamente no console do Gerenciador do servidor mesmo.  
  
#### <a name="results"></a>Resultados  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
O **resultados** página mostra o sucesso ou fracasso de promoção e todas as informações administrativas importantes. O controlador de domínio é reinicializado automaticamente após 10 segundos.  
  
## <a name="BKMK_PSForest"></a>Implantando uma floresta com o Windows PowerShell  
Esta seção explica como instalar o primeiro controlador de domínio em um domínio raiz da floresta usando o Windows PowerShell em um computador Core Windows Server 2012.  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Processo de instalação do Windows PowerShell AD DS função  
Implementando alguns cmdlets de implantação simples ServerManager em seus processos de implantação, você ainda mais percebe que a visão do AD DS simplificado administração.  
  
A figura a seguir ilustra o processo de instalação da função Serviços de domínio do Active Directory, você está executando a partir **PowerShell.exe** e final logo antes da promoção do controlador de domínio.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|ServerManager Cmdlet|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|Install-WindowsFeature/adicionar-WindowsFeature|***-Name***<br /><br />*-Reiniciar*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Fonte<br /><br />*-ComputerName*<br /><br />-Credenciais<br /><br />-/LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> Tempo não necessário, o argumento **- IncludeManagementTools** é altamente recomendável durante a instalação dos binários de função do AD DS  
  
O módulo ServerManager expõe partes de instalação, status e remoção de função do novo módulo DISM para Windows PowerShell. Essa divisão em camadas simplifica o máximo de tarefas e reduz a necessidade para uso direto de potente (mas perigosa quando usado incorretamente) módulo DISM.  
  
Use **Get-Command** para exportar os aliases e cmdlets no ServerManager.  
  
```powershell  
Get-Command -module ServerManager  
```  
  
Por exemplo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
Para adicionar a função Serviços de domínio do Active Directory, basta executar o **Install-WindowsFeature** com o nome da função AD DS como um argumento. Como o Gerenciador do servidor, todos os serviços necessários implícita para a função AD DS instalar automaticamente.  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
Se também quiser que as ferramentas de gerenciamento do AD DS instaladas, e isso é altamente recomendável - fornecem o **- IncludeManagementTools** argumento:  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
Por exemplo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
Para listar todos os recursos e funções com seu status de instalação, use **Get-WindowsFeature** sem argumentos. Especificar **- ComputerName** argumento para o status de instalação de um servidor remoto.  
  
```powershell  
Get-WindowsFeature  
```  
  
Porque **Get-WindowsFeature** não tem uma filtragem mecanismo, você deve usar **Where-Object** com um pipeline para encontrar recursos específicos. O pipeline é um canal usado entre vários cmdlets para transmitir dados e o cmdlet Where-Object atua como um filtro. Integrados **$_** variável atua como objeto de atual transmissão através do pipeline com quaisquer propriedades pode conter.  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
Por exemplo localizar todos os recursos que contêm "Active Dir" em suas **nome de exibição** propriedade, use:  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
Outros exemplos ilustrados abaixo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
Para obter mais informações sobre mais operações do Windows PowerShell com pipelines e Where-Object, consulte [Piping e o Pipeline no Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Observe também que o Windows PowerShell 3.0 simplificado significativamente os argumentos de linha de comando necessários nessa operação do pipeline. Windows PowerShell 2.0 exigiria:  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
Usando o pipeline do Windows PowerShell, você pode criar resultados legíveis. Por exemplo:  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
Observe como usar o **Select Object** cmdlet com o **- expandproperty** argumento retorna dados interessantes:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> O **Select Object - expandproperty** argumento retarda o desempenho geral do instalação um pouco.  
  
### <a name="BKMK_PS"></a>Criar um domínio de raiz da floresta do AD DS com o Windows PowerShell  
Para instalar uma nova floresta do Active Directory usando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```powershell  
Install-addsforest  
```  
  
O **instalar AddsForest** cmdlet só tem duas fases (verificação de pré-requisitos e instalação). As duas figuras a seguir mostram a fase de instalação com o argumento mínimo necessário de **- domainname**.  
  
|||  
|-|-|  
|ADDSDeployment Cmdlet|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|Instalar Addsforest|-Confirmar<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*-DomainMode*<br /><br />***-DomainName***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Força<br /><br />*-InstallDNS*<br /><br />*-/LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> O **- DomainNetBIOSName** argumento é necessário se você quiser alterar o nome de 15 caracteres gerado automaticamente com base no prefixo de nome de domínio DNS ou se o nome exceder 15 caracteres.  
  
O Gerenciador do servidor equivalente **implantação configuração** ADDSDeployment cmdlet e os argumentos são:  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
Os argumentos de cmdlet Server Manager domínio controlador opções ADDSDeployment equivalente são:  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
O **instalar ADDSForest** argumentos siga os mesmos padrões como Gerenciador do servidor, se não for especificado.  
  
O **SafeModeAdministratorPassword** operação do argumento é especial:  
  
-   Se *não especificado* como um argumento, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo criar uma nova floresta denominado corp.contoso.com e ser solicitado a inserir e confirmar uma senha mascarada:  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   Se especificado *com um valor*, o valor deve ser uma cadeia de caracteres segura. Isso não é o uso preferencial ao executar o cmdlet interativamente.  
  
Por exemplo, você pode manualmente solicitar uma senha usando o **Read-Host** cmdlet para solicitar ao usuário uma cadeia de caracteres segura:  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Como a opção anterior não confirme a senha, tenha muito cuidado: a senha não estiver visível.  
  
Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora isso é recomendado.  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Por fim, você poderia armazenar a senha ofuscada em um arquivo e reutilizá-lo mais tarde, sem a senha de texto não criptografado nunca aparece. Por exemplo:  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Fornecendo ou armazenar uma senha de texto clara ou ofuscados não é recomendado. Qualquer pessoa executando esse comando em um script ou observando sabe a senha DSRM do controlador de domínio. Qualquer pessoa com acesso ao arquivo poderia inverter essa senha ofuscada. Com esse conhecimento, eles podem fazer logon em um controlador de domínio iniciado no DSRM e eventualmente representar o controlador de domínio em si, elevar seus privilégios de nível mais alto em uma floresta do Active Directory. Um conjunto adicional de etapas usando **Cryptography** para criptografar o arquivo de texto dados são aconselhável, mas fora do escopo. A prática recomendada é evitar totalmente o armazenamento de senhas.  
  
O cmdlet ADDSDeployment oferece uma opção adicional para ignorar a configuração automática de configurações do cliente DNS, encaminhadores e dicas de raiz. Você não pode ignorar essa opção de configuração ao usar o Gerenciador do servidor. Esse argumento é importante somente se você instalou a função de servidor DNS antes de configurar o controlador de domínio:  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
O **DomainNetBIOSName** operação também é especial:  
  
-   Se o **DomainNetBIOSName** argumento não for especificado com um nome de domínio NetBIOS e o nome de domínio de rótulo único prefixo no **DomainName** argumento é 15 caracteres ou menos, em seguida, promoção continua com um nome gerado automaticamente.  
  
-   Se o **DomainNetBIOSName** argumento não for especificado com um nome de domínio NetBIOS e o nome de domínio de rótulo único prefixo no **DomainName** argumento é 16 caracteres ou mais e depois promoção falha.  
  
-   Se o **DomainNetBIOSName** argumento é especificado com um nome de domínio NetBIOS de 15 caracteres ou menos, em seguida, promoção continua com esse nome especificado.  
  
-   Se o **DomainNetBIOSName** argumento é especificado com um nome de domínio NetBIOS de 16 caracteres ou mais e, em seguida, promoção falha.  
  
O argumento de cmdlet Server Manager adicionais opções ADDSDeployment equivalente é:  
  
```powershell  
-domainnetbiosname <string>  
```  
  
O Gerenciador do servidor equivalente **caminhos** ADDSDeployment cmdlet argumentos são:  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
Use opcional **Whatif** argumento com o **instalar ADDSForest** cmdlet para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos do cmdlet.  
  
Por exemplo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
Você não pode ignorar a **Verificar pré-requisito** quando usar o Gerenciador do servidor, mas você pode ignorar o processo ao usar o cmdlet do AD DS implantação usando o argumento a seguir:  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft desestimula ignorar a verificação de pré-requisitos como ele pode levar a uma promoção de controlador de domínio parcial ou danificado floresta do AD DS.  
  
Observe como, assim como Gerenciador do servidor, **instalar ADDSForest** lembra que promoção reinicializará o servidor automaticamente.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
Para aceitar o prompt de reinicialização automaticamente, use o **-forçar** ou **-confirmar: $false** argumentos com qualquer cmdlet ADDSDeployment Windows PowerShell. Para impedir que o servidor reiniciar automaticamente no final da promoção, use o **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> Não é recomendado substituindo a reinicialização. Reinicialize o controlador de domínio para funcionar corretamente.  
  
## <a name="see-also"></a>Consulte também  
[Serviços de domínio do Active Directory (Portal do TechNet)](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Serviços de domínio do Active Directory para Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Serviços de domínio do Active Directory para Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Referência técnica do Windows Server (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Centro Administrativo do Active Directory: Introdução (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Administração do Active Directory com o Windows PowerShell (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[Peça a equipe de serviços de diretório (Blog de suporte técnico oficial comerciais da Microsoft)](http://blogs.technet.com/b/askds)  
  

