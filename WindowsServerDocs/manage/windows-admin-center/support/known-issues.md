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
ms.openlocfilehash: b0159d88251c7f4f6422dffd8f1e1414e1f30f33
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296857"
---
# Problemas conhecidos do Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

Se você encontrar um problema não descrito nessa página, [informe-nos](http://aka.ms/WACfeedback).

## Lenovo XClarity integrador
Existe uma incompatibilidade entre uma combinação de versão específica do Lenovo XClarity integrador e o Windows Admin Center. Se você atualmente usa ou planeja usar a extensão Lenovo XClarity integrador no Centro de administração do Windows, veja o que você precisa saber:

- Lenovo XClarity integrador 1.0.4 de versão de extensão é totalmente compatível com o Windows Admin Center 1809.5.
- Lenovo XClarity integrador versão extensão 1.0.4 expôs um problema de compatibilidade no Windows Admin Center 1904. Os engenheiros da Microsoft e Lenovo são investigando juntos, e esperamos para fornecer uma solução assim que possível. Todas as atualizações serão publicadas aqui no site de documentos do Windows Admin Center, e você também pode consultar a [página de suporte do Lenovo](https://support.lenovo.com/solutions/ht507549) para referência.
- Se você for um usuário frequente de funcionalidade de XClarity do Lenovo, você pode qualquer mantenha no Windows Admin Center 1809.5 para continuar a usar XClarity integrador 1.0.4, ou você pode atualizar para o Windows Admin Center 1904 e usar separadamente o administrador XClarity autônomo software como uma solução alternativa por enquanto.


## Instalador

- Ao instalar o Windows Admin Center usando seu próprio certificado, lembre-se que, se você copiar a impressão digital da ferramenta MMC do Gerenciador de certificado, [conterá um caractere inválido no início.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Como alternativa, digite o primeiro caractere da impressão digital e copiar/colar o restante.

- Não há suporte para usar a porta abaixo 1024. No modo de serviço, você pode configurar opcionalmente porta 80 para redirecionar para a porta especificada.

- Se o serviço Windows Update (wuauserv) é parado e desativado, o instalador falhará. [19100629]

### Atualizar, Atualização (Upgrade)

- Ao atualizar o Windows Admin Center no modo de serviço de uma versão anterior, se você usar msiexec em modo silencioso, você pode encontrar um problema em que a regra de firewall de entrada para a porta do Windows Admin Center é excluída.
  - Para recriar a regra, execute o seguinte comando em um console do PowerShell com privilégios elevados, substituindo \<port> com a porta configurada para o Windows Admin Center (padrão 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## Geral

- Se você tiver o Windows Admin Center está instalado como um gateway no **Windows Server 2016** em uso intenso, o serviço pode falhar com um erro no log de eventos que contém ```Faulting application name: sme.exe``` e ```Faulting module name: WsmSvc.dll```. Isso é devido a um bug que foi corrigido no Windows Server 2019. O patch para o Windows Server 2016 foi incluído atualização cumulativa a fevereiro de 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se você tiver o Windows Admin Center está instalado como um gateway e sua lista de conexão parece estar corrompido, execute as seguintes etapas:

>[!WARNING]
>Isso excluirá a lista de conexão e as configurações para todos os usuários do Windows Admin Center no gateway.

  1. Desinstalar o Windows Admin Center
  2. Exclua a pasta de **Experiência de gerenciamento de servidor** em **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstalar o Windows Admin Center

- Se você deixar a ferramenta aberta e ociosa por um longo período de tempo, talvez você obtenha diversos erros do tipo **Erro: o estado de runspace não é válido para esta operação**. Caso isso aconteça, atualize o navegador. Se você encontrar isso, [envie-nos comentários](http://aka.ms/WACfeedback).

- Você pode encontrar um **erro 500** quando atualizar páginas com URLs muito longos. [12443710]

- Em algumas ferramentas, o verificador ortográfico do seu navegador pode marcar determinados valores de campo como incorretos. [12425477]

- Em algumas ferramentas, botões de comando podem não refletir as alterações de estado imediatamente após a que está sendo clicado e a ferramenta de interface do usuário pode não refletir automaticamente alterações para determinadas propriedades. Você pode clicar em **Atualizar** para recuperar o estado mais recente do servidor de destino. [11445790]

- Filtragem de marca na lista de conexão - se você selecionar conexões usando as caixas de seleção de seleção múltipla, filtrar sua lista de conexão por marcas, a seleção original persiste, portanto, qualquer ação que você selecionar serão aplicadas a todos os computadores selecionados anteriormente. [18099259]

- Pode haver pequenas variações entre números de versão em execução nos módulos do Windows Admin Center e que está listado no aviso de Software de terceiros 3ª sistemas operacionais.

### Gerenciador de extensão

- Quando você atualiza o Windows Admin Center, você deve reinstalar suas extensões.
- Se você adicionar uma feed de extensão que está inacessível, não há nenhum aviso. [14412861]

## Problemas específicos do navegador

### Microsoft Edge

- Em alguns casos, você pode encontrar tempos de carregamento longos ao usar o Microsoft Edge para acessar um gateway do Windows Admin Center pela Internet. Isso pode ocorrer em máquinas virtuais do Azure onde o gateway do Windows Admin Center está usando um certificado autoassinado. [13819912]

- Quando estiver usando o Azure Active Directory como seu provedor de identidade e um Windows Admin Center for configurado com um certificado autoassinado ou não confiável, você não pode concluir a autenticação AAD no Microsoft Edge.  [15968377]

- Se você tiver o Windows Admin Center implantado como um serviço, e você estiver usando o Microsoft Edge como navegador, conectar seu gateway para Azure pode falhar após a geração de uma nova janela do navegador. Tente contornar esse problema, adicionando https://login.microsoftonline.com, https://login.live.com, e a URL do seu gateway como sites confiáveis e sites permitidos para configurações de pop-ups do navegador do lado cliente. Para obter mais orientações sobre como corrigir isso na [guia de solução de problemas](troubleshooting.md#azlogin). [17990376]

- Se você tiver o Windows Admin Center está instalado no modo de área de trabalho, a guia do navegador no Microsoft Edge não exibe o favicon. [17665801]

### Google Chrome

- Antes da versão 70 (liberado tardia outubro de 2018) Chrome tinha um [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) em relação ao protocolo WebSocket e a autenticação NTLM. Isso afeta as seguintes ferramentas: Eventos, PowerShell, Área de trabalho remota.

- O Chrome pode gerar diversos avisos de credencial em pop-up, especificamente durante a experiência de conexão de add em um ambiente de **grupo de trabalho** (não pertencentes ao domínio).

- Se você tiver o Windows Admin Center implantado como um serviço, pop-ups da URL gateway precisam estar habilitado para qualquer funcionalidade de integração do Azure trabalhar. Esses serviços incluem o adaptador de rede do Azure, o gerenciamento de atualização do Azure e o Azure Site Recovery.

### Mozilla Firefox

O Windows Admin Center não foi testado com o Mozilla Firefox, mas a maior parte da funcionalidade deve funcionar.

- Instalação do Windows 10: o Mozilla Firefox tem seu próprio repositório de certificados, portanto, você deve importar o certificado do ```Windows Admin Center Client``` no Firefox para usar o Windows Admin Center no Windows 10.

<a id="websockets"></a>

## Compatibilidade de WebSocket ao usar um serviço de proxy

Os módulos de Área de trabalho remota, PowerShell e Eventos no Windows Admin Center utilizam o protocolo WebSocket, que geralmente não é suportado ao usar um serviço de proxy. O suporte de WebSocket na compatibilidade de Proxy de aplicativo do Azure AD está em [visualização](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) e procurando feedback sobre compatibilidade.

## Suporte para versões do Windows Server antes de 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Windows Admin Center exige recursos do PowerShell que não estão incluídos no Windows Server 2012 R2, 2012 ou 2008 R2. Se você deseja gerenciar Windows Server esses com o Windows Admin Center, você precisará instalar WMF versão 5.1 ou posterior nesses servidores.

Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior.

Se não estiver instalado, você pode [baixar e instalar o WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

<a id="rbacknownissues"></a>

## Controle de acesso com base em função (RBAC)

- A implantação de RBAC não terá êxito em computadores que estão configurados para usar o Controle de aplicativos do Windows Defender (WDAC, anteriormente conhecido como Integridade de código.) [16568455]

- Para usar o RBAC em um cluster, você deve implantar a configuração para cada nó do membro individualmente.

- Quando RBAC for implantado, você poderá obter erros não autorizados que são atribuídos incorretamente na configuração de RBAC. [16369238]

## Solução de Gerenciador do servidor

### Configurações do servidor

- Se você modificar uma configuração, em seguida, tenta navegar para longe sem salvar, a página será avisa você sobre as alterações não salvas, mas continuar sair. Você pode acabar em um estado onde a guia Configurações selecionado não coincide com o conteúdo da página. [19905798] [19905787]

### Certificados

- Não é possível importar o Certificado criptografado .PFX no repositório do usuário atual. [11818622]

### Dispositivos

- Ao navegar pela tabela com seu teclado, a seleção pode pular para a parte superior do grupo da tabela. [16646059]

### Eventos

- Eventos é afetados pela [compatibilidade de websocket ao usar um serviço de proxy.](#websockets)

- Você pode receber um erro que faz referência a "tamanho de pacote" ao exportar os arquivos de log grandes. [16630279]

  - Para resolver esse problema, use o seguinte comando em um prompt de comando elevado no computador de gateway: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### Arquivos

- Carregar ou baixar arquivos grandes que ainda não são suportados. (\limite de ~100 mb) [12524234]

### PowerShell

- O PowerShell é afetado pela [compatibilidade de websocket ao usar um serviço de proxy](#websockets)

- Colar com um único clique do botão direito do mouse como no console do PowerShell não funciona. Em vez disso, você verá o menu de contexto do navegador, onde é possível selecionar a opção de colar. CTRL-V funciona também.

- Ctrl-C para copiar não funciona, pois sempre envia o comando de quebra de Ctrl-C para o console. Copiar pelo menu de contexto com o botão direito do mouse funciona.

- Ao diminuir a janela do Windows Admin Center, o conteúdo do terminal reflui, mas quando você aumenta novamente, o conteúdo pode não retornar ao estado anterior. Em caso de problemas, tente usar Clear-Host ou desconectar e reconectar usando o botão acima do terminal.

### Editor do registro

- A funcionalidade de pesquisa não foi implementada. [13820009]

### Área de Trabalho Remota

- A ferramenta de área de trabalho remota talvez não consiga se conectar ao gerenciar o Windows Server 2012. [20258278]

- Ao usar a área de trabalho remota para se conectar a um computador que não seja ingressado no domínio, você deve digitar sua conta no ```MACHINENAME\USERNAME``` formato.

- Algumas configurações podem bloquear o cliente de área de trabalho remota do Windows Admin Center com política de grupo. Se você encontrar isso, habilitar ```Allow users to connect remotely by using Remote Desktop Services``` em ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Área de trabalho remota é afetada pela [compatibilidade de websocket.](#websockets)

- A ferramenta de área de trabalho remota não oferece suporte atualmente para copiar/colar qualquer texto, imagem ou arquivo entre a área de trabalho local e a sessão remota.

- Para fazer qualquer ação de copiar/colar na sessão remota, você pode copiar de forma normal (botão direito do mouse + copiar ou Ctrl + C), mas a ação de colar requer o botão direito do mouse + colar (Ctrl + V não funciona)

- Você não pode enviar os seguintes comandos de teclas para a sessão remota
  - Alt+Tab
  - Teclas de função
  - Tecla Windows
  - PrtScn

- Aplicativo remoto – depois de habilitar a ferramenta de App remoto nas configurações de área de trabalho remota, a ferramenta pode não aparecer na lista ferramenta ao gerenciar um servidor com experiência Desktop. [18906904]

### Funções e recursos

- Ao selecionar funções ou recursos com fontes não disponíveis para a instalação, eles são ignorados. [12946914]

- Se você optar por não reiniciar automaticamente após a instalação de função, não solicitamos novamente. [13098852]

- Se você optar por reiniciar automaticamente, a reinicialização ocorrerá antes que o status seja atualizado para 100%. [13098852]

### Armazenamento

- Buscando informações de cota pode falhar sem uma notificação de erro (ainda haverá um erro no console do navegador) [18962274]

- Baixo nível: unidades de CD/DVD/disquete não aparecem como volumes de baixo nível.

- Baixo nível: algumas propriedades em Volumes e Discos não estão disponíveis no baixo nível, portanto aparecem como desconhecido ou em branco no painel de detalhes.

- Baixo nível: ao criar um novo volume, ReFS suporta apenas um tamanho de unidade de alocação de 64 K em computadores com Windows 2012 e 2012 R2. Se um volume ReFS é criado com um tamanho de unidade de alocação menor em destinos de baixo nível, a formatação de sistema do arquivo falhará. O novo volume não poderá ser usado. A solução é excluir o volume e usar o tamanho de unidade de alocação de 64 K.

### Atualizações

- Depois de instalar atualizações, o status de instalação podem ser armazenados em cache e exigir uma atualização do navegador.

- Você pode encontrar o erro: "O conjunto de chaves não existe" ao tentar configurar o gerenciamento de atualização do Azure. Nesse caso, tente as seguintes etapas de correção no nó gerenciado-
    1. Pare ' Serviços de criptografia '.
    2. Alterar as opções de pasta para mostrar arquivos ocultos (se necessário).
    3. Tem a pasta "% allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" e excluir todo o seu conteúdo.
    4. Reinicie '' Serviços de criptografia.
    5. Repita a configuração de gerenciamento de atualizações com o Windows Admin Center

### Máquinas virtuais

- Ao gerenciar as máquinas virtuais em um host do Windows Server 2012, conecte-se a VM do navegador ferramenta falhará se conectar à VM. Baixar o arquivo. rdp para se conectar à VM ainda deve funcionar. [20258278]

- Azure Site Recovery – se ASR está configurada no host fora WAC, não será possível proteger uma VM de dentro do WAC [18972276]

- Os recursos avançados disponíveis no Gerenciador do Hyper-V, como o Gerenciador de SAN Virtual, Mover VM, Exportar VM, Replicação de VM atualmente não são suportados.

### Switches virtuais

- Switch Embedded Teaming (definido): ao adicionar NICs a uma equipe, eles devem estar na mesma sub-rede.

## Solução de gerenciamento do computador

A solução de gerenciamento do computador contém um subconjunto das ferramentas de solução de Gerenciador do Servidor, portanto, os mesmos problemas conhecidos se aplicam, bem como os seguintes problemas específicos da solução de Gerenciamento do computador:

- Se você usar uma Account da Microsoft ([MSA](https://account.microsoft.com/account/)) ou se você usar o Azure Active Directory (AAD) para fazer logon no computador Windows 10, você deve especificar "gerenciar-como" credenciais para gerenciar o computador local [16568455]

- Ao tentar gerenciar o localhost, você será solicitado a elevar o processo de gateway. Se você clicar em **não** no menu pop-up do Controle de conta de usuário que aparecer, o Windows Admin Center não conseguirá exibi-la novamente. Nesse caso, saia do processo de gateway clicando com o botão direito do mouse no ícone do Windows Admin Center na bandeja do sistema e escolha Sair. Em seguida, reinicie o Windows Admin Center no Menu Iniciar.

- O Windows 10 não tem comunicação remota do WinRM /PowerShell por padrão
  
  - Para habilitar o gerenciamento do cliente Windows 10, você deve executar o comando ```Enable-PSRemoting``` por um prompt de comandos com privilégios elevados do PowerShell.

  - Você também pode precisar atualizar seu firewall para permitir conexões de fora da sub-rede local com ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Para cenários de redes mais restritivos, consulte [esta documentação](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## Solução de Gerenciador de cluster de failover

- Ao gerenciar um cluster, (com hiperconvergência ou traditional) você pode encontrar um erro de **shell não encontrado** . Se isso acontecer, recarregar o navegador ou saia para outra ferramenta e retorne. [13882442]

- Um problema pode ocorrer ao gerenciar um cluster de baixo nível (Windows Server 2012 ou 2012 R2) que ainda não foi configurado completamente. A correção desse problema garante que o recurso do Windows **RSAT-Clustering-PowerShell** foi instalado e habilitado em **cada nó membro** do cluster. Para fazer isso com o PowerShell, digite o comando `Install-WindowsFeature -Name RSAT-Windows-PowerShell` em todos os nós do cluster. [12524664]

- Talvez seja necessário adicionar o Cluster com todo o FQDN para ser descoberto corretamente.

- Ao se conectar a um cluster usando o Windows Admin Center instalado como um gateway e fornecendo um nome de usuário/senha explícita para autenticar, você deve selecionar **Usar essas credenciais para todas as conexões** para que as credenciais fiquem disponíveis para consultar os nós membro.

## Solução de gerenciador de cluster com hiperconvergência

- Alguns comandos como **Drives - Update firmware**, **Servers - Remove** e **Volumes - Open** são desabilitados e não são suportados atualmente.