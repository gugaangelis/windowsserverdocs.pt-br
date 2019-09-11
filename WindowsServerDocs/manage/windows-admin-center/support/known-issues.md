---
title: Problemas conhecidos do Windows Admin Center
description: Problemas conhecidos do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: b222cd4b97beecd25c14b9f8f39627bf46cb7716
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869542"
---
# <a name="windows-admin-center-known-issues"></a>Problemas conhecidos do Windows Admin Center

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Se você encontrar um problema não descrito nessa página, [informe-nos](http://aka.ms/WACfeedback).

## <a name="lenovo-xclarity-integrator"></a>Integrador Lenovo XClarity

O problema de incompatibilidade divulgado anteriormente da extensão do integrador do Lenovo XClarity e do centro de administração do Windows versão 1904 agora é resolvido com a versão 1904,1 do centro de administração do Windows. É altamente recomendável que você atualize para a versão mais recente com suporte do centro de administração do Windows.

- A extensão do integrador Lenovo XClarity versão 1,1 é totalmente compatível com o centro de administração do Windows 1904,1. É altamente recomendável que você atualize para a versão mais recente do centro de administração do Windows e a extensão do Lenovo.
- Por qualquer motivo, se você precisar continuar usando o centro de administração do Windows 1809,5 por enquanto, você pode usar o XClarity Integrator 1.0.4 que também estará disponível no feed de extensão do centro de administração do Windows até que o centro de administração do Windows 1809,5 não tenha mais suporte com base em nossa [política de suporte](../support/index.md).

## <a name="installer"></a>Instalador

- Ao instalar o Windows Admin Center usando seu próprio certificado, lembre-se que, se você copiar a impressão digital da ferramenta MMC do Gerenciador de certificado, [conterá um caractere inválido no início.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Como alternativa, digite o primeiro caractere da impressão digital e copiar/colar o restante.

- Não há suporte para o uso da porta abaixo de 1024. No modo de serviço, você pode opcionalmente configurar a porta 80 para redirecionar para a porta especificada.

- Se o serviço de Windows Update (wuauserv) for interrompido e desabilitado, o instalador falhará. [19100629]

### <a name="upgrade"></a>Atualizar

- Ao atualizar o centro de administração do Windows no modo de serviço de uma versão anterior, se você usar o msiexec no modo silencioso, poderá encontrar um problema em que a regra de firewall de entrada para a porta do centro de administração do Windows seja excluída.
  - Para recriar a regra, execute o seguinte comando em um console do PowerShell com privilégios \<elevados, substituindo a porta > pela porta configurada para o centro de administração do Windows (padrão 443.)

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>Geral

- Se você tiver o centro de administração do Windows instalado como um gateway no **Windows Server 2016** sob uso intenso, o serviço poderá falhar com um erro no log de ```Faulting application name: sme.exe``` eventos ```Faulting module name: WsmSvc.dll```que contém e. Isso ocorre devido a um bug que foi corrigido no Windows Server 2019. O patch para o Windows Server 2016 foi incluído na atualização cumulativa de fevereiro de 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se você tiver o centro de administração do Windows instalado como um gateway e sua lista de conexão parecer estar corrompida, execute as seguintes etapas:

   > [!WARNING]
   >Isso excluirá a lista de conexões e as configurações de todos os usuários do centro de administração do Windows no gateway.

  1. Desinstalar o Windows Admin Center
  2. Exclua a pasta de **Experiência de gerenciamento de servidor** em **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstalar o Windows Admin Center

- Se você deixar a ferramenta aberta e ociosa por um longo período de tempo, você poderá obter vários **erros: O estado do runspace não é válido para erros** de operação. Caso isso aconteça, atualize o navegador. Se você encontrar isso, [envie-nos comentários](http://aka.ms/WACfeedback).

- Você pode encontrar um **erro 500** quando atualizar páginas com URLs muito longos. [12443710]

- Em algumas ferramentas, o verificador ortográfico do seu navegador pode marcar determinados valores de campo como incorretos. [12425477]

- Em algumas ferramentas, botões de comando podem não refletir as alterações de estado imediatamente após a que está sendo clicado e a ferramenta de interface do usuário pode não refletir automaticamente alterações para determinadas propriedades. Você pode clicar em **Atualizar** para recuperar o estado mais recente do servidor de destino. [11445790]

- Filtragem de marca na lista de conexões – se você selecionar conexões usando as caixas de seleção de multiseleção e, em seguida, filtrar sua lista de conexões por marcas, a seleção original persiste, portanto, qualquer ação selecionada será aplicada a todos os computadores selecionados anteriormente. [18099259]

- Pode haver uma pequena variação entre os números de versão do OSS em execução nos módulos do centro de administração do Windows e o que está listado no aviso de software de terceiros.

### <a name="extension-manager"></a>Gerenciador de extensões

- Ao atualizar o centro de administração do Windows, você deve reinstalar suas extensões.
- Se você adicionar um feed de extensão que está inacessível, não haverá nenhum aviso. [14412861]

## <a name="browser-specific-issues"></a>Problemas específicos do navegador

### <a name="microsoft-edge"></a>Microsoft Edge

- Em alguns casos, você pode encontrar tempos de carregamento longos ao usar o Microsoft Edge para acessar um gateway do Windows Admin Center pela Internet. Isso pode ocorrer em máquinas virtuais do Azure onde o gateway do Windows Admin Center está usando um certificado autoassinado. [13819912]

- Quando estiver usando o Azure Active Directory como seu provedor de identidade e um Windows Admin Center for configurado com um certificado autoassinado ou não confiável, você não pode concluir a autenticação AAD no Microsoft Edge.  [15968377]

- Se você tiver o centro de administração do Windows implantado como um serviço e estiver usando o Microsoft Edge como seu navegador, conectar seu gateway ao Azure poderá falhar após a geração de uma nova janela do navegador. Tente contornar esse problema adicionando https://login.microsoftonline.com , https://login.live.com e a URL do seu gateway como sites confiáveis e sites permitidos para configurações de bloqueador de pop-up no navegador do lado do cliente. Para obter mais diretrizes sobre como corrigir isso no [Guia de solução de problemas](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

- Se você tiver o centro de administração do Windows instalado no modo de área de trabalho, a guia navegador no Microsoft Edge não exibirá o favicon. [17665801]

### <a name="google-chrome"></a>Google Chrome

- Antes da versão 70 (lançado no final de outubro, 2018) o Chrome tinha um [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) em relação ao protocolo WebSockets e à autenticação NTLM. Isso afeta as seguintes ferramentas: Eventos, PowerShell Área de Trabalho Remota.

- O Chrome pode gerar diversos avisos de credencial em pop-up, especificamente durante a experiência de conexão de add em um ambiente de **grupo de trabalho** (não pertencentes ao domínio).

- Se você tiver o centro de administração do Windows implantado como um serviço, os pop-ups da URL do gateway precisarão ser habilitados para que qualquer funcionalidade de integração do Azure funcione. Esses serviços incluem o adaptador de rede do Azure, o Azure Gerenciamento de Atualizações e o Azure Site Recovery.

### <a name="mozilla-firefox"></a>Mozilla Firefox

O Windows Admin Center não foi testado com o Mozilla Firefox, mas a maior parte da funcionalidade deve funcionar.

- Instalação do Windows 10: O Mozilla Firefox tem seu próprio repositório de certificados, portanto, você deve ```Windows Admin Center Client``` importar o certificado para o Firefox para usar o centro de administração do Windows no Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilidade WebSocket ao usar um serviço de proxy

Os módulos de Área de trabalho remota, PowerShell e Eventos no Windows Admin Center utilizam o protocolo WebSocket, que geralmente não é suportado ao usar um serviço de proxy. O suporte de WebSocket na compatibilidade de Proxy de aplicativo do Azure AD está em [visualização](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) e procurando feedback sobre compatibilidade.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Suporte para versões do Windows Server anteriores a 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> O centro de administração do Windows requer recursos do PowerShell que não estão incluídos no Windows Server 2012 R2, 2012 ou 2008 R2. Se você for gerenciar o Windows Server com o centro de administração do Windows, será necessário instalar o WMF versão 5,1 ou superior nesses servidores.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você pode [baixar e instalar 5.1 WMF](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>RBAC (controle de acesso baseado em função)

- A implantação de RBAC não terá êxito em computadores que estão configurados para usar o Controle de aplicativos do Windows Defender (WDAC, anteriormente conhecido como Integridade de código.) [16568455]

- Para usar o RBAC em um cluster, você deve implantar a configuração para cada nó do membro individualmente.

- Quando RBAC for implantado, você poderá obter erros não autorizados que são atribuídos incorretamente na configuração de RBAC. [16369238]

## <a name="server-manager-solution"></a>Solução de Gerenciador do servidor

### <a name="server-settings"></a>Configurações do servidor

- Se você modificar uma configuração e tentar sair sem salvar, a página avisará sobre as alterações não salvas, mas continuará a sair. Você pode acabar em um estado em que a guia Configurações selecionada não corresponde ao conteúdo da página. [19905798] [19905787]

### <a name="certificates"></a>Certificados

- Não é possível importar o Certificado criptografado .PFX no repositório do usuário atual. [11818622]

### <a name="devices"></a>Dispositivos

- Ao navegar pela tabela com o teclado, a seleção pode saltar para a parte superior do grupo de tabelas. [16646059]

### <a name="events"></a>Events

- Eventos é afetados pela [compatibilidade de websocket ao usar um serviço de proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Você pode receber um erro que faz referência a "tamanho de pacote" ao exportar os arquivos de log grandes. [16630279]

  - Para resolver isso, use o seguinte comando em um prompt de comando com privilégios elevados no computador do gateway:```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Arquivos

- Carregar ou baixar arquivos grandes que ainda não são suportados. (\~limite de 100 MB) [12524234]

### <a name="powershell"></a>PowerShell

- O PowerShell é afetado pela [compatibilidade de websocket ao usar um serviço de proxy](#websocket-compatibility-when-using-a-proxy-service)

- Colar com um único clique do botão direito do mouse como no console do PowerShell não funciona. Em vez disso, você verá o menu de contexto do navegador, onde é possível selecionar a opção de colar. CTRL-V funciona também.

- Ctrl-C para copiar não funciona, pois sempre envia o comando de quebra de Ctrl-C para o console. Copiar pelo menu de contexto com o botão direito do mouse funciona.

- Ao diminuir a janela do Windows Admin Center, o conteúdo do terminal reflui, mas quando você aumenta novamente, o conteúdo pode não retornar ao estado anterior. Em caso de problemas, tente usar Clear-Host ou desconectar e reconectar usando o botão acima do terminal.

### <a name="registry-editor"></a>Editor do Registro

- A funcionalidade de pesquisa não foi implementada. [13820009]

### <a name="remote-desktop"></a>Área de Trabalho Remota

- A ferramenta de Área de Trabalho Remota pode não conseguir se conectar ao gerenciar o Windows Server 2012. [20258278]

- Ao usar o área de trabalho remota para se conectar a um computador que não está ingressado no domínio, você deve inserir sua ```MACHINENAME\USERNAME``` conta no formato.

- Algumas configurações podem bloquear o cliente da área de trabalho remota do centro de administração do Windows com a diretiva de grupo. Se você encontrar isso, habilite ```Allow users to connect remotely by using Remote Desktop Services``` em```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- O Área de Trabalho Remota é afetado pela [compatibilidade do WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- A ferramenta de área de trabalho remota não oferece suporte atualmente para copiar/colar qualquer texto, imagem ou arquivo entre a área de trabalho local e a sessão remota.

- Para fazer qualquer ação de copiar/colar na sessão remota, você pode copiar de forma normal (botão direito do mouse + copiar ou Ctrl + C), mas a ação de colar requer o botão direito do mouse + colar (Ctrl + V não funciona)

- Você não pode enviar os seguintes comandos de teclas para a sessão remota
  - Alt+Tab
  - Teclas de função
  - Tecla Windows
  - PrtScn

- Aplicativo remoto – depois de habilitar a ferramenta de aplicativo remoto de Área de Trabalho Remota configurações, a ferramenta pode não aparecer na lista de ferramentas ao gerenciar um servidor com experiência desktop. [18906904]

### <a name="roles-and-features"></a>Funções e recursos

- Ao selecionar funções ou recursos com fontes não disponíveis para a instalação, eles são ignorados. [12946914]

- Se você optar por não reiniciar automaticamente após a instalação de função, não solicitamos novamente. [13098852]

- Se você optar por reiniciar automaticamente, a reinicialização ocorrerá antes que o status seja atualizado para 100%. [13098852]

### <a name="storage"></a>Armazenamento

- A busca de informações de cota pode falhar sem uma notificação de erro (ainda haverá um erro no console do navegador) [18962274]

- Nível inferior: Unidades de DVD/CD/disquete não aparecem como volumes no nível inferior.

- Nível inferior: Algumas propriedades em volumes e discos não estão disponíveis no nível inferior para que pareçam desconhecidas ou em branco no painel de detalhes.

- Nível inferior: Ao criar um novo volume, o ReFS só dá suporte a um tamanho de unidade de alocação de 64K em computadores Windows 2012 e 2012 R2. Se um volume ReFS é criado com um tamanho de unidade de alocação menor em destinos de baixo nível, a formatação de sistema do arquivo falhará. O novo volume não poderá ser usado. A solução é excluir o volume e usar o tamanho de unidade de alocação de 64 K.

### <a name="updates"></a>Atualizações

- Depois de instalar as atualizações, o status de instalação pode ser armazenado em cache e exigir uma atualização do navegador.

- Você pode encontrar o erro: "O conjunto de chaves não existe" ao tentar configurar o gerenciamento de atualizações do Azure. Nesse caso, tente as seguintes etapas de correção no nó gerenciado-
    1. Pare o serviço ' serviços de criptografia '.
    2. Altere as opções de pasta para mostrar arquivos ocultos (se necessário).
    3. Foi necessário ter a pasta "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" e excluir todo o seu conteúdo.
    4. Reinicie o serviço ' serviços de criptografia '.
    5. Repetir a configuração do Gerenciamento de Atualizações com o centro de administração do Windows

### <a name="virtual-machines"></a>Máquinas Virtuais

- Ao gerenciar as máquinas virtuais em um host do Windows Server 2012, a ferramenta de conexão de VM no navegador não conseguirá se conectar à VM. O download do arquivo. rdp para se conectar à VM ainda deve funcionar. [20258278]

- Azure Site Recovery – se a ASR estiver configurada no host fora do WAC, você não poderá proteger uma VM de dentro do WAC [18972276]

- Os recursos avançados disponíveis no Gerenciador do Hyper-V, como o Gerenciador de SAN Virtual, Mover VM, Exportar VM, Replicação de VM atualmente não são suportados.

### <a name="virtual-switches"></a>Comutadores Virtuais

- Alternar equipe inserida (SET): Ao adicionar NICs a uma equipe, eles devem estar na mesma sub-rede.

## <a name="computer-management-solution"></a>Solução de gerenciamento do computador

A solução de gerenciamento do computador contém um subconjunto das ferramentas de solução de Gerenciador do Servidor, portanto, os mesmos problemas conhecidos se aplicam, bem como os seguintes problemas específicos da solução de Gerenciamento do computador:

- Se você usar uma conta da Microsoft ([MSA](https://account.microsoft.com/account/)) ou se usar Azure Active Directory (AAD) para fazer logon em seu computador com Windows 10, deverá especificar credenciais "gerenciar-como" para gerenciar seu computador local [16568455]

- Ao tentar gerenciar o localhost, você será solicitado a elevar o processo de gateway. Se você clicar em **não** no menu pop-up do Controle de conta de usuário que aparecer, o Windows Admin Center não conseguirá exibi-la novamente. Nesse caso, saia do processo de gateway clicando com o botão direito do mouse no ícone do Windows Admin Center na bandeja do sistema e escolha Sair. Em seguida, reinicie o Windows Admin Center no Menu Iniciar.

- O Windows 10 não tem comunicação remota do WinRM /PowerShell por padrão
  
  - Para habilitar o gerenciamento do cliente Windows 10, você deve executar o comando ```Enable-PSRemoting``` por um prompt de comandos com privilégios elevados do PowerShell.

  - Você também pode precisar atualizar seu firewall para permitir conexões de fora da sub-rede local com ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Para cenários de redes mais restritivos, consulte [esta documentação](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Solução de Gerenciador de cluster de failover

- Ao gerenciar um cluster, (com hiperconvergência ou traditional) você pode encontrar um erro de **shell não encontrado** . Se isso acontecer, recarregar o navegador ou saia para outra ferramenta e retorne. [13882442]

- Um problema pode ocorrer ao gerenciar um cluster de baixo nível (Windows Server 2012 ou 2012 R2) que ainda não foi configurado completamente. A correção desse problema garante que o recurso do Windows **RSAT-Clustering-PowerShell** foi instalado e habilitado em **cada nó membro** do cluster. Para fazer isso com o PowerShell, digite o comando `Install-WindowsFeature -Name RSAT-Windows-PowerShell` em todos os nós do cluster. [12524664]

- Talvez seja necessário adicionar o Cluster com todo o FQDN para ser descoberto corretamente.

- Ao se conectar a um cluster usando o Windows Admin Center instalado como um gateway e fornecendo um nome de usuário/senha explícita para autenticar, você deve selecionar **Usar essas credenciais para todas as conexões** para que as credenciais fiquem disponíveis para consultar os nós membro.

## <a name="hyper-converged-cluster-manager-solution"></a>Solução de gerenciador de cluster com hiperconvergência

- Alguns comandos como **Drives - Update firmware**, **Servers - Remove** e **Volumes - Open** são desabilitados e não são suportados atualmente.