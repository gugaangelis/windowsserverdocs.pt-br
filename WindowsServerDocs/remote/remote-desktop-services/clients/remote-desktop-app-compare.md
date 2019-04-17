---
title: Área de trabalho remota - comparar os aplicativos de cliente
description: Saiba como os diferentes aplicativos de área de trabalho remota comparam quando se trata de funções e recursos suportados.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc20d1a2c51abddb8ae014efc621f4f0b36c3677
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297405"
---
# Compare os aplicativos de cliente

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Geralmente está perguntamos como os diferentes aplicativos de cliente de área de trabalho remota comparam uns aos outros. Todos eles fazem a mesma coisa? Aqui estão as respostas para essas perguntas.

## Suporte de redirecionamento

As tabelas a seguir comparam o suporte para o dispositivo e outros redirecionamentos no aplicativo de Conexão de área de trabalho remota, o aplicativo Universal, aplicativo Android, aplicativo iOS, macOS cliente de aplicativo e web. Estas tabelas abrangem os redirecionamentos de que você pode acessar uma vez em uma sessão remota. 

Se você remoto em sua área de trabalho pessoal, há redirecionamentos adicionais que você pode definir nas **Configurações adicionais** para a sessão. Se sua área de trabalho remota ou aplicativos são gerenciados por sua organização, o administrador pode habilitar ou desabilitar redirecionamentos por meio de configurações de política de grupo.

### Redirecionamento de entrada

| Redirecionamento | Área de Trabalho Remota<br> Conexão | Universal | Android | iOS | macOS | cliente da Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Teclado    | X                             | X         | X       | X   | X     | X          |
| Mouse       | X                             | X         | X       | X *    | X     | X          |
| Touch       | X                             | X         | X       | X   |       |            |
| Other       | Caneta                           |           |         |     |       |            |
* Exiba a [lista de dispositivos de entrada com suporte para o cliente de Beta do iOS de área de trabalho remota](remote-desktop-ios.md#supported-input-devices).

### Redirecionamento de porta   

| Redirecionamento | Área de Trabalho Remota <br>Conexão | Universal | Android | iOS | macOS | Cliente da Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Porta serial | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Ao habilitar o redirecionamento de porta USB, todos os dispositivos USB conectados à porta USB são reconhecidos automaticamente na sessão remota.

### Outro redirecionamento (dispositivos, etc.)



| Redirecionamento         | Conexão de Área de Trabalho Remota | Universal   | Android | iOS         | macOS                                    | Cliente da Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Câmeras             | X                         |             |         |             |                                          |               |
| Área de Transferência           | X                         | texto, imagem | texto    | texto, imagem | X                                        | texto          |
| Unidade/armazenamento local | X                         |             | X       |             | x                                        |               |
| Localização            | X                         |             |         |             |                                          |               |
| Microfones         | X                         |X            |         |             | X                                        |               |
| Printers            | X                         |             |         |             | X (XÍCARAS somente)                            | Imprimir PDF     |
| Scanners            | X                         |             |         |             |                                          |               |
| Cartões inteligentes         | X                         |             |         |             | X (autenticação do Windows não é compatível) |               |
| Alto-falantes            | X                         | X           | X       | X           | X                                        | X (exceto o IE) |

* Para o redirecionamento de impressora - o aplicativo macOS suporta o driver de impressora fotocompositora Publisher por padrão. Eles não têm suporte redirecionando drivers da impressora nativo.

### Configurações de RDP com suporte
Esta tabela inclui a lista de configurações de arquivo RDP com suporte que podem ser usados com os clientes do Windows e HTML. Um "x" na coluna plataforma indica que a configuração tem suporte. Observe que isso não é uma lista completa de configurações com suporte para os clientes do Windows e o HTML5. Continuaremos a atualizar esta tabela para incluir mais configurações RDP com suporte para os clientes do Windows e o HTML5, bem como o macOS, iOS e Android clientes.

| Configuração de RDP                        | Descrição            | Valores                 | Valor padrão          | Área de trabalho Virtual do Windows | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| endereço completo: s:value alternativo | Especifica um nome alternativo ou endereço IP do computador remoto. | Qualquer nome válido ou o endereço IP do computador remoto, por exemplo, "10.10.15.15" | | x | x | x |
| shell: s:value alternativo        | Determina se um programa inicia automaticamente quando você se conecta com o RDP. Essa configuração corresponde ao arquivo e caminho caixa de nome programa na guia Programas do cliente Conexão de área de trabalho remota. Para especificar um shell alternativo, insira um caminho válido para um arquivo executável para o valor, por exemplo "" C:\ProgramFiles\Office\word.exe"". Essa configuração também determina o caminho ou o alias do aplicativo remoto para ser iniciado em tempo de conexão se RemoteApplicationMode está habilitado. | Por exemplo, "C:\ProgramFiles\Office\word.exe" || x | x | x |
| audiocapturemode:i:Value | Indica se o redirecionamento de entrada/saída de áudio está habilitado. Essa configuração corresponde à caixa de áudio remoto do cliente da conexão de área de trabalho remota. | Captura de áudio de desabilitar (0) do dispositivo local; (1) habilitar a captura de áudio do dispositivo local e redirecionamento para um aplicativo de áudio na sessão remota | 0 | x | x | |
| audiomode:i:Value | Determina qual máquina (isto é, local ou remota) reproduz áudio. | Sons (0) no computador local (reproduzir no computador); (1) reproduzir sons no computador remoto (reproduzir no computador remoto); (2) não sons (não play) | 0 | x | x | x |
| nível de autenticação: i:value | Define as configurações de nível de autenticação de servidor. | (0) se a autenticação de servidor falhar, conectar ao computador sem aviso (Connect e não avisar); (1) se a autenticação de servidor falhar, não estabelecer uma conexão (não se conectar); (2) se a autenticação de servidor falhar, mostrar um aviso e permitir que me conectar ou recusar a conexão (avisar); (3) nenhuma exigência de autenticação é especificada. | 3 | x | x ||
| autoreconnection habilitado: i:value | Essa configuração determina se o computador cliente tentará automaticamente reconectar ao computador remoto se a conexão for interrompida; Por exemplo, quando há uma interrupção de conectividade de rede. Essa configuração corresponde à reconectar se a conexão for solto caixa de seleção na guia experiência em opções na Conexão de área de trabalho remota (RDC).| Computador de cliente (0) não tenta se reconectar automaticamente; (1) computador cliente tenta se reconectar automaticamente| 1 | x | x | x |
| bandwidthautodetect:i:Value | Determina se a detecção de tipo de rede automática está habilitada | Detecção do tipo de rede automática desabilitar (0); (1) ativar a detecção de tipo de rede automática | 1 | x | x | x |
| compactação: i:value | Essa configuração determina se a compactação de em massa é habilitada quando ele é transmitido pelo RDP para o computador local.|Compactação de massa desabilitar RDP (0); (1) ativar a compactação de massa RDP | 1 | x | x | x |
| id do tamanho da área de trabalho: i:value | Especifica as dimensões da área de trabalho remota de sessão de um conjunto de opções predefinidas. Essa configuração será substituída se desktopheight ou desktopwidth são especificados.| (0) 640 x 480; (1) 800 x 600; (2) 1024 x 768; (3) 1280 x 1024; (4) 1600 x 1200 | 0 | x | x | x |
| desktopheight:i:Value | Essa configuração determina a altura de resolução (em pixels) no computador remoto quando você se conectar usando a Conexão de área de trabalho remota (RDC). Essa configuração corresponde à seleção na configurationslider theDisplay na underOptions theDisplaytab no RDC. | Valor numérico de 200 a 2048 | O valor padrão é definido como a resolução no computador local | x | x | x |
| desktopwidth:i:Value | Essa configuração determina a largura de resolução (em pixels) no computador remoto quando você se conectar usando a Conexão de área de trabalho remota (RDC). Essa configuração corresponde à seleção na configurationslider theDisplay na underOptions theDisplaytab no RDC. | Valor numérico de 200 para 4096 | O valor padrão é definido como a resolução no computador local | x | x | x |
| disableclipboardredirection:i:Value | Essa configuração determina se o redirecionamento de área de transferência é habilitado ao se conectar ao computador remoto. | Redirecionamento de área de transferência (0) está habilitada; (1) redirecionamento de área de transferência não está habilitado | x | x | x |
| disableconnectionsharing:i:Value | Determina se o cliente da área de trabalho remoto reconectado a qualquer conexões abertas existentes ou iniciar uma nova conexão quando um RemoteApp ou a área de trabalho é iniciada | Reconexão (0) para qualquer sessão existente; (1) iniciar nova conexão | 0 | x | x | x |
| disableprinterredirection:i:Value | Essa configuração determina se o redirecionamento de impressora imprimir fácil está habilitado ao se conectar ao computador remoto. | Redirecionamento de impressora imprimir fácil (0) está habilitada; (1) redirecionamento de impressora de impressão fácil está desabilitado | x | x | x |
| domínio: s:value | Essa configuração especifica o nome do domínio em que a conta de usuário que será usada para fazer logon no computador remoto usando a Conexão de área de trabalho remota (RDC) está localizada. O valor dessa configuração, juntamente com o valor da configuração do nome de usuário, será exibido na caixa de nametext ousuário na theGeneraltab underOptionsin RDC. | Um nome de domínio válido, por exemplo, "CONTOSO" | Nenhum valor padrão | x | x | x |
| drivestoredirect:s:Value | Determina quais unidades de disco locais no computador cliente será redirecionada e estará disponível na sessão remota. | Nenhum valor especificado - redirecionar todas as unidades; * - Redirecionar todas as unidades de disco, incluindo unidades conectadas posterior; DynamicDrives - redirecionar as unidades que estão conectadas posterior; A unidade e rótulos para uma ou mais unidades - redirecionar as unidades especificadas.| Nenhum valor especificado - redirecionar todas as unidades | x | x    | |
| enablecredsspsupport:i:Value | Essa configuração determina se o RDP usará o provedor de suporte de segurança de credenciais (CredSSP) para autenticação se ele estiver disponível. | RDP (0) não usará CredSSP, mesmo se o sistema operacional suporta CredSSP; (1) RDP usará CredSSP se o sistema operacional suporte CredSSP | 1 | x | x | |
| endereço completo: s:value | Essa configuração especifica o nome ou endereço IP do computador remoto que você deseja se conectar | Um nome de computador válido, endereço IPv4 ou IPv6 endereço - Especifica o computador remoto ao qual você deseja se conectar. | | x | x | x |
| GatewayCredentialsSource:i:Value | Especifica ou recupera o método de autenticação de Gateway de área de trabalho remota. | (0) solicitar senha (NTLM); (1) uso Smartcard; (4) permitem que o usuário selecione posteriormente | 0 | x | x | x |
| gatewayhostname:s:Value | Especifica o nome de host de Gateway de área de trabalho remota. | Endereço do servidor de gateway válido. ||x|x|x|
| gatewayprofileusagemethod:i:Value | Especifica se é preciso usar configurações padrão do Gateway de área de trabalho remota | O modo de perfil padrão, conforme especificado pelo administrador; de uso (0) (1) use configurações explícitas, conforme especificado pelo usuário. | 0 | x | x | x |
| gatewayusagemethod:i:Value | Especifica quando usar o servidor de Gateway de área de trabalho remota | (0) não usam um servidor de Gateway de área de trabalho remota; (1) sempre usar um servidor de Gateway de área de trabalho remota; (2) usar um servidor de Gateway de área de trabalho remota se uma conexão direta não pode ser feita para o Host de sessão de área de trabalho remota; (3) usar as configurações de servidor de Gateway de área de trabalho remota padrão; (4) não usar um Gateway de área de trabalho remota, ignorar o servidor para endereços locais; Definir esse valor de propriedade para 0 ou 4 são é efetivamente equivalente, mas definir essa propriedade como 4 ativado a opção de Ignorar endereços locais. | | x | x | x |
| networkautodetect:i:Value | Determina se é necessário usar a detecção de largura de banda de rede automática ou não. Requer o optionbandwidthautodetectto esteja definido e correlaciona withconnection type7. | (0) não usar a detecção de largura de banda de rede automática; (1) usar a detecção de largura de banda de rede automática | 1 | x ||x|
| PromptCredentialOnce:i:Value | Determina se as credenciais do usuário são salvos e usadas para o Gateway de área de trabalho remota e o computador remoto.|Sessão remota (0) não usará as mesmas credenciais; (1) sessão remota usará as mesmas credenciais|1|x|x||
| redirectclipboard:i:Value | Essa configuração determina se a área de transferência no computador local será redirecionado e estará disponível na sessão remota. Essa configuração corresponde à caixa theClipboardcheck na theLocal Resourcestab underOptionsin RDC. | Área de transferência (0) no computador local não está disponível em uma sessão remota; (1) transferência no computador local está disponível na sessão remota|1|x|x|x|
| redirectdrives:i:Value | Essa configuração determina se a unidades no computador cliente será redirecionada e estará disponível na sessão remota. Essa configuração corresponde à seleções forDrivesunderMoreon theLocal Resourcestab underOptionsin RDC.|(0) as unidades no computador local não estão disponíveis na sessão remota; (1) as unidades no computador local estão disponíveis na sessão remota|0|x|x| |
| redirectprinters:i:Value | Essa configuração determina se impressoras configuradas no computador cliente será redirecionada e estará disponível na sessão remota quando você se conectar a um computador remoto usando a Conexão de área de trabalho remota (RDC). Essa configuração corresponde à seleção na caixa thePrinterscheck theLocal Resourcestab underOptionsin RDC. | (0) as impressoras no computador local não estão disponíveis na sessão remota; (1) as impressoras no computador local estão disponíveis na sessão remota|1|x|x|x|
| redirectsmartcards:i:Value | Essa configuração determina se os dispositivos de cartão inteligente no computador cliente será redirecionada e estará disponível na sessão remota quando você se conectar a um computador remoto usando a Conexão de área de trabalho remota (RDC). Essa configuração corresponde à seleção na theSmart cardscheck caixa underMore em theLocal Resourcestab underOptionsin RDC.|(0) o dispositivo de cartão inteligente no computador local não está disponível na sessão remota; (1) o dispositivo de cartão inteligente no computador local está disponível na sessão remota|1|x|x||
| remoteapplicationcmdline:s:Value | Parâmetros de linha de comando opcional para o RemoteApp.||x|x|x|
| remoteapplicationexpandcmdline:i:Value| Determina se as variáveis de ambiente contidas no parâmetro de linha de comando RemoteApp devem ser expandidas local ou remotamente.|Variáveis de ambiente (0) devem ser expandidas para os valores do computador local; (1) variáveis de ambiente devem ser expandidas no computador remoto para os valores do computador remoto||x|x|x|
| remoteapplicationexpandworkingdir | Determina se as variáveis de ambiente contidas no parâmetro RemoteApp trabalho diretório devem ser expandidas local ou remotamente. | Variáveis de ambiente (0) devem ser expandidas para os valores do computador local; (1) variáveis de ambiente devem ser expandidas no computador remoto para os valores do computador remoto. Observação: O diretório de trabalho do RemoteApp é especificado por meio do parâmetro de diretório de trabalho do shell.||x|x|x|
|remoteapplicationfile:s:Value | Especifica um arquivo a ser aberto no computador remoto, o RemoteApp. Observação: Para arquivos locais para ser aberto, você também deve habilitar o redirecionamento de unidade para a unidade de origem.||x|x|x|
|remoteapplicationicon:s:Value | Especifica o arquivo de ícone a ser exibido na interface do cliente ao iniciar um RemoteApp. Se nenhum nome de arquivo for especificado, o cliente usará o ícone de área de trabalho remota padrão. Apenas os arquivos ". ico" são compatíveis.||x|x|x|
|remoteapplicationmode:i:Value | Determina se uma conexão RemoteApp é iniciado como uma sessão do RemoteApp.| (0) não iniciar uma sessão do RemoteApp; (1) iniciar uma sessão do RemoteApp|1|x|x|x|
|remoteapplicationname:s:Value | Especifica o nome do RemoteApp na interface do cliente ao iniciar o RemoteApp.| Por exemplo, "Excel 2016"|x|x|x|
|remoteapplicationprogram:s:Value | Especifica o nome de alias ou o executável do RemoteApp. | Por exemplo, "EXCEL" |x|x|x|
|id do modo de tela: i:value | Essa configuração determina se a janela de sessão remota é exibida em tela inteira quando você se conectar ao computador remoto usando a Conexão de área de trabalho remota (RDC). Essa configuração corresponde à seleção na configurationslider theDisplay na theDisplaytab underOptionsin RDC.|(1) a sessão remota será exibido em uma janela; (2) a sessão remota será exibida em tela inteira|2|x|x|x|
|dimensionamento: i:value inteligente | Essa configuração determina se o computador cliente pode dimensionar o conteúdo no computador remoto para ajustar o tamanho da janela do computador cliente.|(0) a exibição da janela cliente não será dimensionada quando redimensionado; (1) a exibição da janela cliente será dimensionada quando redimensionado|0|x|x||
| Use multimon:i:value | Essa configuração define o suporte a vários monitores quando você se conectar ao computador remoto usando a Conexão de área de trabalho remota (RDC).|(0) não habilitar o suporte a vários monitores; (1) Habilitar suporte a vários monitores|0|x|x||
| username:s:Value | Essa configuração especifica o nome da conta de usuário que será usado para fazer logon no computador remoto usando a Conexão de área de trabalho remota (RDC). O valor dessa configuração, juntamente com o valor da configuração do domínio, será exibido no namebox ousuário na theGeneraltab underOptionsin RDC.| Qualquer nome de usuário válido. ||x|x|x|
| videoplaybackmode:i:Value| Essa configuração determina se a Conexão de área de trabalho remota (RDC) será usado RDP eficiente multimídia de streaming para reprodução de vídeo.|(0) não use RDP eficiente multimídia de streaming para reprodução de vídeo; (1) usar RDP eficiente multimídia de streaming para reprodução de vídeo quando possível|1|x|x||
| workspaceid:s:Value | Essa configuração define o RemoteApp e ID de área de trabalho associado com o arquivo RDP que contém essa configuração. | Um RemoteApp válido e a ID de Conexão de área de trabalho|x|x||