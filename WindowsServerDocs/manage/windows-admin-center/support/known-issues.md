---
title: Problemas conhecidos do Windows Admin Center
description: Problemas conhecidos do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: c05987360256f7b7ed58911c1ded86586fc8b3aa
ms.sourcegitcommit: 7c7fc443ecd0a81bff6ed6dbeeaf4f24582ba339
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2019
ms.locfileid: "74903898"
---
# <a name="windows-admin-center-known-issues"></a>Problemas conhecidos do Windows Admin Center

> Aplica-se a: centro de administração do Windows, versão prévia do centro de administração do Windows

Se você encontrar um problema não descrito nessa página, [informe-nos](https://aka.ms/WACfeedback).

## <a name="installer"></a>Instalador

- Ao instalar o Windows Admin Center usando seu próprio certificado, lembre-se que, se você copiar a impressão digital da ferramenta MMC do Gerenciador de certificado, [conterá um caractere inválido no início.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Como alternativa, digite o primeiro caractere da impressão digital e copiar/colar o restante.

- Não há suporte para o uso da porta abaixo de 1024. No modo de serviço, você pode opcionalmente configurar a porta 80 para redirecionar para a porta especificada.

## <a name="general"></a>Geral

- Se você tiver o centro de administração do Windows instalado como um gateway no **Windows Server 2016** sob uso intenso, o serviço poderá falhar com um erro no log de eventos que contém ```Faulting application name: sme.exe``` e ```Faulting module name: WsmSvc.dll```. Isso ocorre devido a um bug que foi corrigido no Windows Server 2019. O patch para o Windows Server 2016 foi incluído na atualização cumulativa de fevereiro de 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se você tiver o centro de administração do Windows instalado como um gateway e sua lista de conexão parecer estar corrompida, execute as seguintes etapas:

   > [!WARNING]
   >Isso excluirá a lista de conexões e as configurações de todos os usuários do centro de administração do Windows no gateway.

  1. Desinstalar o Windows Admin Center
  2. Exclua a pasta de **Experiência de gerenciamento de servidor** em **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstalar o Windows Admin Center

- Se você deixar a ferramenta aberta e ociosa por um longo período de tempo, talvez você obtenha diversos erros do tipo **Erro: o estado de runspace não é válido para esta operação**. Caso isso aconteça, atualize o navegador. Se você encontrar isso, [envie-nos comentários](https://aka.ms/WACfeedback).

- Pode haver uma pequena variação entre os números de versão do OSS em execução nos módulos do centro de administração do Windows e o que está listado no aviso de software de terceiros.

### <a name="extension-manager"></a>Gerenciador de Extensões

- Ao atualizar o centro de administração do Windows, você deve reinstalar suas extensões.
- Se você adicionar um feed de extensão que está inacessível, não haverá nenhum aviso. [14412861]

## <a name="browser-specific-issues"></a>Problemas específicos do navegador

### <a name="microsoft-edge"></a>Microsoft Edge

- Se você tiver o centro de administração do Windows implantado como um serviço e estiver usando o Microsoft Edge como seu navegador, conectar seu gateway ao Azure poderá falhar após a geração de uma nova janela do navegador. Tente solucionar esse problema adicionando https://login.microsoftonline.com , https://login.live.com e a URL do seu gateway como sites confiáveis e sites permitidos para configurações do bloqueador de pop-ups no navegador do lado do cliente. Para obter mais diretrizes sobre como corrigir isso no [Guia de solução de problemas](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

### <a name="google-chrome"></a>Google Chrome

- Antes da versão 70 (lançado no final de outubro, 2018) o Chrome tinha um [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) em relação ao protocolo WebSockets e à autenticação NTLM. Isso afeta as seguintes ferramentas: Eventos, PowerShell, Área de trabalho remota.

- O Chrome pode gerar diversos avisos de credencial em pop-up, especificamente durante a experiência de conexão de add em um ambiente de **grupo de trabalho** (não pertencentes ao domínio).

- Se você tiver o centro de administração do Windows implantado como um serviço, os pop-ups da URL do gateway precisarão ser habilitados para que qualquer funcionalidade de integração do Azure funcione.

### <a name="mozilla-firefox"></a>Mozilla Firefox

O Windows Admin Center não foi testado com o Mozilla Firefox, mas a maior parte da funcionalidade deve funcionar.

- Instalação do Windows 10: o Mozilla Firefox tem seu próprio repositório de certificados, portanto, você deve importar o ```Windows Admin Center Client``` certificado para o Firefox para usar o centro de administração do Windows no Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilidade WebSocket ao usar um serviço de proxy

Os módulos de Área de trabalho remota, PowerShell e Eventos no Windows Admin Center utilizam o protocolo WebSocket, que geralmente não é suportado ao usar um serviço de proxy. O suporte de WebSocket na compatibilidade de Proxy de aplicativo do Azure AD está em [visualização](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) e procurando feedback sobre compatibilidade.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Suporte para versões do Windows Server anteriores a 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> O centro de administração do Windows requer recursos do PowerShell que não estão incluídos no Windows Server 2012 R2, 2012 ou 2008 R2. Se você for gerenciar o Windows Server com o centro de administração do Windows, será necessário instalar o WMF versão 5,1 ou superior nesses servidores.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você pode [baixar e instalar 5.1 WMF](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>RBAC (Controle de Acesso Baseado em Função)

- A implantação de RBAC não terá êxito em computadores que estão configurados para usar o Controle de aplicativos do Windows Defender (WDAC, anteriormente conhecido como Integridade de código.) [16568455]

- Para usar o RBAC em um cluster, você deve implantar a configuração para cada nó do membro individualmente.

- Quando RBAC for implantado, você poderá obter erros não autorizados que são atribuídos incorretamente na configuração de RBAC. [16369238]

## <a name="server-manager-solution"></a>Solução de Gerenciador do servidor

### <a name="certificates"></a>Certificados

- Não é possível importar o Certificado criptografado .PFX no repositório do usuário atual. [11818622]

### <a name="events"></a>Eventos

- Eventos é afetados pela [compatibilidade de websocket ao usar um serviço de proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Você pode receber um erro que faz referência a "tamanho de pacote" ao exportar os arquivos de log grandes.

  - Para resolver isso, use o seguinte comando em um prompt de comando com privilégios elevados no computador do gateway: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Arquivos

- Carregar ou baixar arquivos grandes que ainda não são suportados. (limite de 100 MB de\~) [12524234]

### <a name="powershell"></a>PowerShell

- O PowerShell é afetado pela [compatibilidade de websocket ao usar um serviço de proxy](#websocket-compatibility-when-using-a-proxy-service)

- Colar com um único clique do botão direito do mouse como no console do PowerShell não funciona. Em vez disso, você verá o menu de contexto do navegador, onde é possível selecionar a opção de colar. CTRL-V funciona também.

- Ctrl-C para copiar não funciona, pois sempre envia o comando de quebra de Ctrl-C para o console. Copiar pelo menu de contexto com o botão direito do mouse funciona.

- Ao diminuir a janela do Windows Admin Center, o conteúdo do terminal reflui, mas quando você aumenta novamente, o conteúdo pode não retornar ao estado anterior. Em caso de problemas, tente usar Clear-Host ou desconectar e reconectar usando o botão acima do terminal.

### <a name="registry-editor"></a>Editor do registro

- A funcionalidade de pesquisa não foi implementada. [13820009]

### <a name="remote-desktop"></a>Área de Trabalho Remota

- Quando o centro de administração do Windows é implantado como um serviço, a ferramenta Área de Trabalho Remota pode falhar ao ser carregada após a atualização do serviço centro de administração do Windows para uma nova versão. Para contornar esse problema, limpe o cache do navegador.   [23824194]

- A ferramenta de Área de Trabalho Remota pode não conseguir se conectar ao gerenciar o Windows Server 2012. [20258278]

- Ao usar o Área de Trabalho Remota para se conectar a um computador que não ingressou no domínio, você deve inserir sua conta no formato de ```MACHINENAME\USERNAME```.

- Algumas configurações podem bloquear o cliente da área de trabalho remota do centro de administração do Windows com a diretiva de grupo. Se você encontrar isso, habilite ```Allow users to connect remotely by using Remote Desktop Services``` em ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- O Área de Trabalho Remota é afetado pela [compatibilidade do WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- A ferramenta de área de trabalho remota não oferece suporte atualmente para copiar/colar qualquer texto, imagem ou arquivo entre a área de trabalho local e a sessão remota.

- Para fazer qualquer ação de copiar/colar na sessão remota, você pode copiar de forma normal (botão direito do mouse + copiar ou Ctrl + C), mas a ação de colar requer o botão direito do mouse + colar (Ctrl + V não funciona)

- Você não pode enviar os seguintes comandos de teclas para a sessão remota
  - Alt+Tab
  - Teclas de função
  - Tecla Windows
  - PrtScn

### <a name="roles-and-features"></a>Funções e recursos

- Ao selecionar funções ou recursos com fontes não disponíveis para a instalação, eles são ignorados. [12946914]

- Se você optar por não reiniciar automaticamente após a instalação de função, não solicitamos novamente. [13098852]

- Se você optar por reiniciar automaticamente, a reinicialização ocorrerá antes que o status seja atualizado para 100%. [13098852]

### <a name="storage"></a>Armazenamento

- Baixo nível: unidades de CD/DVD/disquete não aparecem como volumes de baixo nível.

- Baixo nível: algumas propriedades em Volumes e Discos não estão disponíveis no baixo nível, portanto aparecem como desconhecido ou em branco no painel de detalhes.

- Baixo nível: ao criar um novo volume, ReFS suporta apenas um tamanho de unidade de alocação de 64 K em computadores com Windows 2012 e 2012 R2. Se um volume ReFS é criado com um tamanho de unidade de alocação menor em destinos de baixo nível, a formatação de sistema do arquivo falhará. O novo volume não poderá ser usado. A solução é excluir o volume e usar o tamanho de unidade de alocação de 64 K.

### <a name="updates"></a>Atualizações

- Depois de instalar as atualizações, o status de instalação pode ser armazenado em cache e exigir uma atualização do navegador.

- Você pode encontrar o erro: "o conjunto de chaves não existe" ao tentar configurar o gerenciamento de atualizações do Azure. Nesse caso, tente as seguintes etapas de correção no nó gerenciado-
    1. Pare o serviço ' serviços de criptografia '.
    2. Altere as opções de pasta para mostrar arquivos ocultos (se necessário).
    3. Foi necessário ter a pasta "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" e excluir todo o seu conteúdo.
    4. Reinicie o serviço ' serviços de criptografia '.
    5. Repetir a configuração do Gerenciamento de Atualizações com o centro de administração do Windows

### <a name="virtual-machines"></a>Máquinas virtuais

- Ao gerenciar as máquinas virtuais em um host do Windows Server 2012, a ferramenta de conexão de VM no navegador não conseguirá se conectar à VM. O download do arquivo. rdp para se conectar à VM ainda deve funcionar. [20258278]

- Azure Site Recovery – se a ASR estiver configurada no host fora do WAC, você não poderá proteger uma VM de dentro do WAC [18972276]

- Os recursos avançados disponíveis no Gerenciador do Hyper-V, como o Gerenciador de SAN Virtual, Mover VM, Exportar VM, Replicação de VM atualmente não são suportados.

### <a name="virtual-switches"></a>Switches virtuais

- Switch Embedded Teaming (definido): ao adicionar NICs a uma equipe, eles devem estar na mesma sub-rede.

## <a name="computer-management-solution"></a>Solução de gerenciamento do computador

A solução de gerenciamento do computador contém um subconjunto das ferramentas de solução de Gerenciador do Servidor, portanto, os mesmos problemas conhecidos se aplicam, bem como os seguintes problemas específicos da solução de Gerenciamento do computador:

- Se você usar uma conta da Microsoft ([MSA](https://account.microsoft.com/account/)) ou se usar Azure Active Directory (AAD) para fazer logon em seu computador com Windows 10, deverá usar "gerenciar como" para fornecer credenciais para uma conta de administrador local [16568455]

- Ao tentar gerenciar o localhost, você será solicitado a elevar o processo de gateway. Se você clicar em **não** no pop-up controle de conta de usuário que segue, deverá cancelar a tentativa de conexão e começar novamente.

- O Windows 10 não tem a comunicação remota do WinRM/PowerShell ativada por padrão.
  
  - Para habilitar o gerenciamento do cliente Windows 10, você deve executar o comando ```Enable-PSRemoting``` por um prompt de comandos com privilégios elevados do PowerShell.

  - Você também pode precisar atualizar seu firewall para permitir conexões de fora da sub-rede local com ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Para cenários de redes mais restritivos, consulte [esta documentação](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Solução de Gerenciador de cluster de failover

- Ao gerenciar um cluster, (com hiperconvergência ou traditional) você pode encontrar um erro de **shell não encontrado** . Se isso acontecer, recarregar o navegador ou saia para outra ferramenta e retorne. [13882442]

- Um problema pode ocorrer ao gerenciar um cluster de baixo nível (Windows Server 2012 ou 2012 R2) que ainda não foi configurado completamente. A correção desse problema garante que o recurso do Windows **RSAT-Clustering-PowerShell** foi instalado e habilitado em **cada nó membro** do cluster. Para fazer isso com o PowerShell, digite o comando `Install-WindowsFeature -Name RSAT-Windows-PowerShell` em todos os nós do cluster. [12524664]

- Talvez seja necessário adicionar o Cluster com todo o FQDN para ser descoberto corretamente.

- Ao se conectar a um cluster usando o Windows Admin Center instalado como um gateway e fornecendo um nome de usuário/senha explícita para autenticar, você deve selecionar **Usar essas credenciais para todas as conexões** para que as credenciais fiquem disponíveis para consultar os nós membro.

## <a name="hyper-converged-cluster-manager-solution"></a>Solução de gerenciador de cluster com hiperconvergência

- Alguns comandos como **Drives - Update firmware**, **Servers - Remove** e **Volumes - Open** são desabilitados e não são suportados atualmente.

## <a name="azure-services"></a>Serviços do Azure

### <a name="azure-file-sync-permissions"></a>Sincronização de Arquivos do Azure permissões

Sincronização de Arquivos do Azure requer permissões no Azure que o centro de administração do Windows não forneceu antes da versão 1910. Se você registrou seu gateway do centro de administração do Windows com o Azure usando uma versão anterior à versão 1910 do centro de administração do Windows, será necessário atualizar seu aplicativo Azure Active Directory para obter as permissões corretas para usar Sincronização de Arquivos do Azure na versão mais recente do Centro de administração do Windows. A permissão adicional permite que Sincronização de Arquivos do Azure execute a configuração automática do acesso à conta de armazenamento, conforme descrito neste artigo: [Verifique se sincronização de arquivos do Azure tem acesso à conta de armazenamento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal).

Para atualizar seu aplicativo Azure Active Directory, você pode fazer uma das duas coisas
1. Acesse **configurações** > **Azure** > **cancelar o registro**e registre o centro de administração do Windows com o Azure novamente, verificando se você escolheu criar um novo aplicativo de Azure Active Directory. 
2. Vá para o aplicativo Azure Active Directory e adicione manualmente a permissão necessária ao seu aplicativo Azure Active Directory existente registrado no centro de administração do Windows. Para fazer isso, vá para **configurações** > **modo de exibição de** > **do Azure** no Azure. Na folha **registro de aplicativo** no Azure, acesse **permissões de API**, selecione **Adicionar uma permissão**. Role para baixo para selecionar **Azure Active Directory grafo**, selecione **permissões delegadas**, expanda **diretório**e selecione **Directory. AccessAsUser. All**. Clique em **adicionar permissões** para salvar as atualizações no aplicativo.

### <a name="options-for-setting-up-azure-management-services"></a>Opções para configurar os serviços de gerenciamento do Azure

Os serviços de gerenciamento do Azure, incluindo Azure Monitor, Azure Gerenciamento de Atualizações e central de segurança do Azure, usam o mesmo agente para um servidor local: o Microsoft Monitoring Agent. O Azure Gerenciamento de Atualizações tem um conjunto mais limitado de regiões com suporte e requer que o espaço de trabalho Log Analytics seja vinculado a uma conta de automação do Azure. Devido a essa limitação, se você quiser configurar vários serviços no centro de administração do Windows, deverá configurar o Azure Gerenciamento de Atualizações primeiro e, em seguida, a central de segurança do Azure ou o Azure Monitor. Se você tiver configurado os serviços de gerenciamento do Azure que usam o Microsoft Monitoring Agent e tentar configurar o Azure Gerenciamento de Atualizações usando o centro de administração do Windows, o centro de administração do Windows permitirá que você configure somente o Azure Gerenciamento de Atualizações se o existente os recursos vinculados ao Microsoft Monitoring Agent oferecem suporte a Gerenciamento de Atualizações do Azure. Se não for o caso, você terá duas opções:

1. Acesse o painel de controle > Microsoft Monitoring Agent para [desconectar o servidor das soluções de gerenciamento do Azure existentes](https://docs.microsoft.com/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics) (como Azure monitor ou central de segurança do Azure). Em seguida, configure o Azure Gerenciamento de Atualizações no centro de administração do Windows. Depois disso, você pode voltar para configurar suas outras soluções de gerenciamento do Azure por meio do centro de administração do Windows sem problemas.
2. Você pode [configurar manualmente os recursos do Azure necessários para o azure gerenciamento de atualizações](https://docs.microsoft.com/azure/automation/automation-update-management) e, em seguida, [atualizar manualmente o Microsoft Monitoring Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) (fora do centro de administração do Windows) para adicionar o novo espaço de trabalho correspondente à solução de gerenciamento de atualizações que você deseja usar.
