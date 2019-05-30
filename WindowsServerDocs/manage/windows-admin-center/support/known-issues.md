---
title: Problemas conhecidos do Windows Admin Center
description: Problemas conhecidos do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 7bf23c5af5620241574864babd07fd852115a450
ms.sourcegitcommit: 39ab8041d166e6817a95417d6aa30bc7abeeef54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66260270"
---
# <a name="windows-admin-center-known-issues"></a>Problemas conhecidos do Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Se você encontrar um problema não descrito nessa página, [informe-nos](http://aka.ms/WACfeedback).

## <a name="lenovo-xclarity-integrator"></a>Integrador de Lenovo XClarity
Agora, o problema de incompatibilidade anteriormente divulgada da extensão do Lenovo XClarity integrador e Windows Admin Center versão 1904 é resolvido com o Windows Admin Center versão 1904.1. É altamente recomendável que você atualize para a versão mais recente com suporte do Windows Admin Center.

- Lenovo XClarity integrador versão 1.1 da extensão é totalmente compatível com Windows Admin Center 1904.1. É altamente recomendável que você atualize para a versão mais recente do Windows Admin Center e a extensão do Lenovo.
- Por algum motivo, se você precisar continuar usando o Windows Admin Center 1809.5 no momento, você pode usar o integrador de XClarity 1.0.4 que também estará disponível no Windows Admin Center extensão Feed até que o Windows Admin Center 1809.5 não é mais compatível com base em nossos [política de suporte](../support/index.md).

## <a name="installer"></a>Instalador

- Ao instalar o Windows Admin Center usando seu próprio certificado, lembre-se que, se você copiar a impressão digital da ferramenta MMC do Gerenciador de certificado, [conterá um caractere inválido no início.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Como alternativa, digite o primeiro caractere da impressão digital e copiar/colar o restante.

- Não há suporte para o usando a porta abaixo de 1024. No modo de serviço, você pode configurar opcionalmente a porta 80 para redirecionar para a porta especificada.

- Se o serviço de atualização do Windows (wuauserv) é interrompido e desabilitado, o instalador falhará. [19100629]

### <a name="upgrade"></a>Atualizar, Atualização (Upgrade)

- Ao atualizar o Windows Admin Center no modo de serviço de uma versão anterior, se você usar o msiexec no modo silencioso, você poderá encontrar um problema em que a regra de firewall de entrada para a porta do Windows Admin Center é excluída.
  - Para recriar a regra, execute o comando a seguir de um console do PowerShell com privilégios elevados, substituindo \<porta > com a porta configurada para o Windows Admin Center (padrão 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>Geral

- Se você tiver o Windows Admin Center instalado como um gateway em **Windows Server 2016** em pleno uso, o serviço poderá falhar com um erro no log de eventos que contém ```Faulting application name: sme.exe``` e ```Faulting module name: WsmSvc.dll```. Isso ocorre devido a um bug que foi corrigido no Windows Server 2019. O patch para o Windows Server 2016 foi incluído a atualização cumulativa de fevereiro de 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se você tiver o Windows Admin Center instalado como um gateway e sua lista de conexão parece estar corrompido, execute as seguintes etapas:

>[!WARNING]
>Isso excluirá a lista de conexão e as configurações para todos os usuários de Windows Admin Center no gateway.

  1. Desinstalar o Windows Admin Center
  2. Exclua a pasta de **Experiência de gerenciamento de servidor** em **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstalar o Windows Admin Center

- Se você deixar a ferramenta de aberto e ocioso por um longo período de tempo, você pode receber vários **erro: O estado do espaço de execução não é válido para esta operação** erros. Caso isso aconteça, atualize o navegador. Se você encontrar isso, [enviar comentários](http://aka.ms/WACfeedback).

- Você pode encontrar um **erro 500** quando atualizar páginas com URLs muito longos. [12443710]

- Em algumas ferramentas, o verificador ortográfico do seu navegador pode marcar determinados valores de campo como incorretos. [12425477]

- Em algumas ferramentas, botões de comando podem não refletir as alterações de estado imediatamente após a que está sendo clicado e a ferramenta de interface do usuário pode não refletir automaticamente alterações para determinadas propriedades. Você pode clicar em **Atualizar** para recuperar o estado mais recente do servidor de destino. [11445790]

- Filtragem de marca na lista de conexão - se você selecionar as conexões usando as caixas de seleção de seleção múltipla e filtrar a lista de conexão por marcas, a seleção original persiste para que qualquer ação que você selecionar serão aplicadas a todas as máquinas selecionadas anteriormente. [18099259]

- Pode haver variação pequena entre números de versão dos sistemas operacionais em execução em módulos do Windows Admin Center e o que está listado no 3º aviso de Software de terceiros.

### <a name="extension-manager"></a>Gerenciador de extensões

- Quando você atualiza o Windows Admin Center, você deverá reinstalar as extensões.
- Se você adicionar uma extensão do feed que está inacessível, não há nenhum aviso. [14412861]

## <a name="browser-specific-issues"></a>Problemas específicos do navegador

### <a name="microsoft-edge"></a>Microsoft Edge

- Em alguns casos, você pode encontrar tempos de carregamento longos ao usar o Microsoft Edge para acessar um gateway do Windows Admin Center pela Internet. Isso pode ocorrer em máquinas virtuais do Azure onde o gateway do Windows Admin Center está usando um certificado autoassinado. [13819912]

- Quando estiver usando o Azure Active Directory como seu provedor de identidade e um Windows Admin Center for configurado com um certificado autoassinado ou não confiável, você não pode concluir a autenticação AAD no Microsoft Edge.  [15968377]

- Se você tiver o Windows Admin Center implantado como um serviço e você estiver usando o Microsoft Edge como navegador, conectar-se o seu gateway para o Azure pode falhar após a geração de uma nova janela do navegador. Tentar contornar esse problema adicionando https://login.microsoftonline.com, https://login.live.com, e a URL do seu gateway como sites confiáveis e sites permitidos para configurações do Bloqueador de pop-up do navegador de lado do cliente. Para obter instruções sobre como corrigir isso na [guia de solução](troubleshooting.md#azlogin). [17990376]

- Se você tiver o Windows Admin Center instalado no modo de área de trabalho, a guia do navegador Microsoft Edge não será exibido o favicon. [17665801]

### <a name="google-chrome"></a>Google Chrome

- Antes da versão 70 (lançado final de outubro de 2018) Chrome tinha uma [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) sobre o protocolo WebSocket e a autenticação NTLM. Isso afeta as seguintes ferramentas: Eventos, PowerShell, área de trabalho remota.

- O Chrome pode gerar diversos avisos de credencial em pop-up, especificamente durante a experiência de conexão de add em um ambiente de **grupo de trabalho** (não pertencentes ao domínio).

- Se você tiver o Windows Admin Center implantado como um serviço, pop-ups da URL do gateway precisam ser habilitada para qualquer funcionalidade de integração do Azure trabalhar. Esses serviços incluem o adaptador de rede do Azure, o gerenciamento de atualizações do Azure e o Azure Site Recovery.

### <a name="mozilla-firefox"></a>Mozilla Firefox

O Windows Admin Center não foi testado com o Mozilla Firefox, mas a maior parte da funcionalidade deve funcionar.

- Instalação do Windows 10: Mozilla Firefox tem seu próprio repositório de certificados, portanto, você deve importar o ```Windows Admin Center Client``` certificado para o Firefox para usar o Windows Admin Center no Windows 10.

<a id="websockets"></a>

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilidade de WebSocket ao usar um serviço de proxy

Os módulos de Área de trabalho remota, PowerShell e Eventos no Windows Admin Center utilizam o protocolo WebSocket, que geralmente não é suportado ao usar um serviço de proxy. O suporte de WebSocket na compatibilidade de Proxy de aplicativo do Azure AD está em [visualização](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) e procurando feedback sobre compatibilidade.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Suporte para versões do Windows Server antes de 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Windows Admin Center requer recursos do PowerShell que não estão incluídos no Windows Server 2012 R2, 2012 ou 2008 R2. Se você gerenciar Windows Server esses recursos com Windows Admin Center, você precisará instalar o WMF versão 5.1 ou posterior nesses servidores.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você pode [baixar e instalar 5.1 WMF](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

<a id="rbacknownissues"></a>

## <a name="role-based-access-control-rbac"></a>Controle de acesso (RBAC) baseado em função

- A implantação de RBAC não terá êxito em computadores que estão configurados para usar o Controle de aplicativos do Windows Defender (WDAC, anteriormente conhecido como Integridade de código.) [16568455]

- Para usar o RBAC em um cluster, você deve implantar a configuração para cada nó do membro individualmente.

- Quando RBAC for implantado, você poderá obter erros não autorizados que são atribuídos incorretamente na configuração de RBAC. [16369238]

## <a name="server-manager-solution"></a>Solução de Gerenciador do servidor

### <a name="server-settings"></a>Configurações do servidor

- Se você modificar uma configuração, tente sair sem salvar, a página irá avisá-lo sobre as alterações não salvas, mas continuar a navegar para longe. Você pode acabar em um estado em que o guia de configurações é selecionada não coincide com o conteúdo da página. [19905798] [19905787]

### <a name="certificates"></a>Certificados

- Não é possível importar o Certificado criptografado .PFX no repositório do usuário atual. [11818622]

### <a name="devices"></a>Dispositivos

- Ao navegar por meio da tabela com o teclado, a seleção pode pular para a parte superior do grupo de tabela. [16646059]

### <a name="events"></a>Eventos

- Eventos é afetados pela [compatibilidade de websocket ao usar um serviço de proxy.](#websockets)

- Você pode receber um erro que faz referência a "tamanho de pacote" ao exportar os arquivos de log grandes. [16630279]

  - Para resolver esse problema, use o seguinte comando no prompt de comandos com privilégios elevados no computador do gateway: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Arquivos

- Carregar ou baixar arquivos grandes que ainda não são suportados. (\~limite de 100 mb) [12524234]

### <a name="powershell"></a>PowerShell

- O PowerShell é afetado pela [compatibilidade de websocket ao usar um serviço de proxy](#websockets)

- Colar com um único clique do botão direito do mouse como no console do PowerShell não funciona. Em vez disso, você verá o menu de contexto do navegador, onde é possível selecionar a opção de colar. CTRL-V funciona também.

- Ctrl-C para copiar não funciona, pois sempre envia o comando de quebra de Ctrl-C para o console. Copiar pelo menu de contexto com o botão direito do mouse funciona.

- Ao diminuir a janela do Windows Admin Center, o conteúdo do terminal reflui, mas quando você aumenta novamente, o conteúdo pode não retornar ao estado anterior. Em caso de problemas, tente usar Clear-Host ou desconectar e reconectar usando o botão acima do terminal.

### <a name="registry-editor"></a>Editor do Registro

- A funcionalidade de pesquisa não foi implementada. [13820009]

### <a name="remote-desktop"></a>Área de Trabalho Remota

- A ferramenta da área de trabalho remota pode falhar ao se conectar ao gerenciar o Windows Server 2012. [20258278]

- Ao usar a área de trabalho remota para se conectar a um computador que não está ingressado no domínio, você deve inserir sua conta de ```MACHINENAME\USERNAME``` formato.

- Algumas configurações podem bloquear o cliente de área de trabalho remota do Windows Admin Center com diretiva de grupo. Se você encontrar isso, habilite ```Allow users to connect remotely by using Remote Desktop Services``` em ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Área de trabalho remota é afetada pelo [compatibilidade do websocket.](#websockets)

- A ferramenta de área de trabalho remota não oferece suporte atualmente para copiar/colar qualquer texto, imagem ou arquivo entre a área de trabalho local e a sessão remota.

- Para fazer qualquer ação de copiar/colar na sessão remota, você pode copiar de forma normal (botão direito do mouse + copiar ou Ctrl + C), mas a ação de colar requer o botão direito do mouse + colar (Ctrl + V não funciona)

- Você não pode enviar os seguintes comandos de teclas para a sessão remota
  - Alt+Tab
  - Teclas de função
  - Tecla Windows
  - PrtScn

- Aplicativo remoto – depois de habilitar a ferramenta de aplicativo remoto das configurações de área de trabalho remota, a ferramenta pode não aparecer na lista de ferramenta ao gerenciar um servidor com experiência Desktop. [18906904]

### <a name="roles-and-features"></a>Funções e recursos

- Ao selecionar funções ou recursos com fontes não disponíveis para a instalação, eles são ignorados. [12946914]

- Se você optar por não reiniciar automaticamente após a instalação de função, não solicitamos novamente. [13098852]

- Se você optar por reiniciar automaticamente, a reinicialização ocorrerá antes que o status seja atualizado para 100%. [13098852]

### <a name="storage"></a>Armazenamento

- Buscando informações de cota pode falhar sem uma notificação de erro (ainda haverá um erro no console do navegador) [18962274]

- Nível inferior: Unidades de CD/DVD/flexível não aparecem como volumes em nível inferior.

- Nível inferior: Algumas propriedades em discos e Volumes não estão disponíveis de baixo nível para que eles apareçam em branco no painel de detalhes ou desconhecido.

- Nível inferior: Ao criar um novo volume, o ReFS suporta apenas um tamanho de unidade de alocação de 64K no Windows 2012 e 2012 R2 máquinas. Se um volume ReFS é criado com um tamanho de unidade de alocação menor em destinos de baixo nível, a formatação de sistema do arquivo falhará. O novo volume não poderá ser usado. A solução é excluir o volume e usar o tamanho de unidade de alocação de 64 K.

### <a name="updates"></a>Atualizações

- Depois de instalar atualizações, status de instalação podem ser armazenados em cache e exigem uma atualização do navegador.

- Você pode encontrar o erro: "O conjunto de chaves não existe" ao tentar configurar o gerenciamento de atualização do Azure. Nesse caso, tente as seguintes etapas de correção no nó gerenciado-
    1. Pare ' Serviços de criptografia '.
    2. Alterar opções de pasta para mostrar arquivos ocultos (se necessário).
    3. Temos a pasta "% allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" e excluir todo o seu conteúdo.
    4. Reinicie o '' Serviços de criptografia.
    5. Repita a configuração do gerenciamento de atualizações com Windows Admin Center

### <a name="virtual-machines"></a>Máquinas virtuais

- Ao gerenciar as máquinas virtuais em um host do Windows Server 2012, conecte-se a VM no navegador ferramenta não conseguirão se conectar à VM. Baixando o arquivo. rdp para se conectar à VM ainda deve funcionar. [20258278]

- O Azure Site Recovery – se o ASR está configurado no host fora do WAC, não será possível proteger uma VM a partir WAC [18972276]

- Os recursos avançados disponíveis no Gerenciador do Hyper-V, como o Gerenciador de SAN Virtual, Mover VM, Exportar VM, Replicação de VM atualmente não são suportados.

### <a name="virtual-switches"></a>Comutadores Virtuais

- Agrupamento incorporado (SET) do comutador: Ao adicionar NICs a uma equipe, eles devem ser na mesma sub-rede.

## <a name="computer-management-solution"></a>Solução de gerenciamento do computador

A solução de gerenciamento do computador contém um subconjunto das ferramentas de solução de Gerenciador do Servidor, portanto, os mesmos problemas conhecidos se aplicam, bem como os seguintes problemas específicos da solução de Gerenciamento do computador:

- Se você usar uma Account da Microsoft ([MSA](https://account.microsoft.com/account/)) ou se você usar o Azure Active Directory (AAD) para fazer logon no você computador Windows 10, você deve especificar "gerenciar-como" credenciais para gerenciar sua máquina local [16568455]

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