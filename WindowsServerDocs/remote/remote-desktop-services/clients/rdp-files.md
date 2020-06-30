---
title: Configurações do arquivo RDP com suporte da Área de Trabalho Remota
description: Saiba mais sobre as configurações do arquivo RDP para a Área de Trabalho Remota
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 06/16/2020
ms.localizationpriority: medium
ms.openlocfilehash: 4606938a6c01e20c847b3a6c198de8a1c61c59f0
ms.sourcegitcommit: 5bc5aaf341c711113ca03d1482f933b05b146007
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85094515"
---
# <a name="supported-remote-desktop-rdp-file-settings"></a>Configurações do arquivo RDP com suporte da Área de Trabalho Remota

A tabela a seguir inclui a lista de configurações compatíveis de arquivo do RDP que você pode usar com os clientes de Área de Trabalho Remota. Ao definir as configurações, marque [Comparações de cliente](./remote-desktop-app-compare.md) para ver a quais redirecionamentos cada cliente dá suporte.

A tabela também realça quais configurações são compatíveis como propriedades personalizadas com a Área de Trabalho Virtual do Windows. Você pode consultar [esta documentação](https://go.microsoft.com/fwlink/?linkid=2098243&clcid=0x409) que detalha como usar o PowerShell para personalizar as propriedades de pools de hosts de Área de Trabalho Virtual do Windows.

## <a name="connection-information"></a>Informações de conexão

| Configuração de RDP                        | Descrição            | Valores                 | Valor padrão          | Área de Trabalho Virtual do Windows |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| full address:s:value | Nome do PC:</br>Essa configuração especifica o nome ou endereço IP do computador remoto ao qual você quer conectar.</br></br>Essa é a única configuração necessária em um arquivo do RDP. | Um nome, um endereço IPv4 ou endereço IPv6 válidos. | | |
| alternate full address:s:value | Especifica um nome alternativo ou um endereço IP do computador remoto. | Um nome, um endereço IPv4 ou endereço IPv6 válidos. | | |
| username:s:value | Especifica o nome da conta de usuário que será usada para entrar no computador remoto. | Qualquer nome de usuário válido. | | |
| domain:s:value | Especifica o nome do domínio no qual a conta de usuário que será usada para entrar no computador remoto está localizada. | Um nome de domínio válido, como "CONTOSO". | | |
| gatewayhostname:s:value | Especifica o nome de host do Gateway de Área de Trabalho Remota. | Um nome, um endereço IPv4 ou endereço IPv6 válidos. | | |
| gatewaycredentialssource:i:value | Especifica o método de autenticação do Gateway de Área de Trabalho Remota. | - 0: Solicitar senha (NTLM)</br>- 1: Usar cartão inteligente</br>- 2: Use as credenciais para o usuário conectado no momento.</br>- 3: Solicitar as credenciais do usuário e usar a autenticação básica</br>- 4: Permitir ao usuário selecionar mais tarde</br>- 5: Usar a autenticação baseada em cookies | 0 | |
| gatewayprofileusagemethod:i:value | Especifica se deve usar configurações padrão do Gateway de Área de Trabalho Remota. | - 0: Usar o modo de perfil padrão, conforme especificado pelo administrador</br>- 1: Usar configurações explícitas, conforme especificado pelo usuário | 0 | |
| gatewayusagemethod:i:value | Especifica quando usar um Gateway de Área de Trabalho Remota para a conexão. | - 0: Não usar um Gateway de Área de Trabalho Remota</br>- 1: Sempre usar um Gateway de Área de Trabalho Remota</br>- 2: Usar um Gateway de Área de Trabalho Remota se uma conexão direta não puder ser feita com o Host da Sessão RD</br>- 3: Usar as configurações padrão do Gateway de Área de Trabalho Remota</br>- 4: Não usar um Gateway de Área de Trabalho Remota, ignorar o gateway para endereços locais</br>Definir esse valor da propriedade como 0 ou 4 é efetivamente equivalente, mas definir essa propriedade como 4 habilita a opção para ignorar endereços locais. | 0 | |
| promptcredentialonce:i:value | Determina se as credenciais do usuário são salvas e usadas para ambos o Gateway de Área de Trabalho Remota e o computador remoto. | - 0: Sessão remota não usará as mesmas credenciais</br>- 1: Sessão remota usará as mesmas credenciais | 1 | |
| authentication level:i:value | Define as configurações de nível de autenticação do servidor. | - 0: Se a autenticação do servidor falhar, conectar ao computador sem aviso (Conectar e não me avisar)</br>- 1: Se a autenticação do servidor falhar, não estabelecer uma conexão (Não conectar)</br>- 2: Se a autenticação do servidor falhar, mostrar um aviso e permite conectar ou recusar a conexão (Avisar-me)</br>- 3: Nenhum requisito de autenticação especificado. | 3 | |
| enablecredsspsupport:i:value | Determina se o cliente usará o CredSSP (Provedor de Suporte de Segurança de Credencial) para a autenticação, se ele estiver disponível. | - 0: RDP não usará CredSSP, mesmo se o sistema operacional der suporte a CredSSP</br>- 1: RDP usará CredSSP se o sistema operacional der suporte a CredSSP | 1 | X |
| disableconnectionsharing:i:value | Determina se o cliente se reconecta a qualquer sessão desconectada existente ou iniciará uma nova conexão quando uma nova conexão for iniciada. | - 0: Reconectar a qualquer sessão existente</br>- 1: Iniciar nova conexão | 0 | X |
| alternate shell:s:value | Especifica um programa a ser iniciado automaticamente na sessão remota como shell em vez de explorador. | Caminho válido para um arquivo executável, como "C:\ProgramFiles\Office\word.exe" | | X |

## <a name="session-behavior"></a>Comportamento da sessão

| Configuração de RDP                        | Descrição            | Valores                 | Valor padrão          | Área de Trabalho Virtual do Windows |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| autoreconnection enabled:i:value | Determina se o cliente tentará se reconectar automaticamente ao computador remoto caso a conexão caia, como quando houver uma interrupção da conectividade de rede. | - 0: Cliente não tenta reconectar automaticamente</br>- 1: Cliente tenta reconectar automaticamente | 1 | X |
| bandwidthautodetect:i:value | Determina se a detecção automática do tipo de rede está habilitada | - 0: Desabilitar a detecção automática do tipo de rede</br>- 1: Habilitar a detecção automática do tipo de rede | 1 | X |
| networkautodetect:i:value | Determina se a detecção automática de largura de banda da rede deve ou não ser usada. Requer que bandwidthautodetect seja definido como 1. | - 0: Não usar detecção automática de largura de banda da rede</br> - 1: Usar detecção automática de largura de banda da rede | 1 | X |
| compression:i:value | Determina se a compactação em massa é habilitada quando durante a transmissão por RDP para o computador local.|- 0: Desabilitar a compactação em massa de RDP</br>- 1: Habilitar a compactação em massa de RDP | 1 | X |
| videoplaybackmode:i:value| Determina se a conexão usará streaming de multimídia eficiente de RDP para a reprodução de vídeo.|- 0: Não usar streaming de multimídia eficiente de RDP para reprodução de vídeo</br>- 1: Usar streaming de multimídia eficiente de RDP para reprodução de vídeo, quando possível | 1 | X |

## <a name="device-redirection"></a>Redirecionamento de dispositivo

| Configuração de RDP                        | Descrição            | Valores                 | Valor padrão          | Área de Trabalho Virtual do Windows |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| audiocapturemode:i:value | Redirecionamento de microfone:</br>Indica se o redirecionamento da entrada de áudio está habilitado. | - 0: Desabilitar a captura de áudio do dispositivo local</br>- 1: Habilitar a captação de áudio do dispositivo local e o redirecionamento para um aplicativo de áudio na sessão remota | 0 | X |
| encode redirected video capture:i:value | Habilita ou desabilita a codificação de vídeo redirecionado. | - 0: Desabilite a codificação de vídeo redirecionado</br>- 1: Habilite a codificação de vídeo redirecionado | 1 | X |
| redirected video capture encoding quality:i:value | Controla a qualidade do vídeo codificado. | - 0: Vídeo de alta compactação. A qualidade pode ser prejudicada quando há muito movimento. </br>- 1: Compactação média.</br>- 2: Vídeo de baixa compactação com alta qualidade da imagem. | 0 | X |
| audiomode:i:value | Local de saída de áudio:</br>Determina se o computador local ou remoto reproduz áudio. | - 0: Reproduzir sons no computador local (reproduzir neste computador)</br>- 1: Reproduzir sons no computador remoto (reproduzir no computador remoto)</br>- 2: Não reproduzir sons (Não reproduzir) | 0 | X |
| camerastoredirect:s:value | Redirecionamento de câmera:</br>Configura quais câmeras serão redirecionadas. Essa configuração usa uma lista delimitada por ponto e vírgula das interfaces KSCATEGORY_VIDEO_CAMERA de câmeras habilitadas para redirecionamento. | - *: Redirecione todas as câmeras</br> - Lista de câmeras, como camerastoredirect:s:\\?\usb#vid_0bda&pid_58b0&mi</br>- É possível excluir uma câmera específica, prefixando a cadeia de caracteres de link simbólico com "-" | Não redirecionar nenhuma câmera | X |
| devicestoredirect:s:value | Redirecionamento de dispositivo USB:</br>Determina quais dispositivos no computador local serão redirecionados e estarão disponíveis na sessão remota. | – *: Redirecionar todos os dispositivos com suporte, incluindo aqueles conectados posteriormente</br> – ID de hardware válida para um ou mais dispositivos | Não redirecionar nenhum dispositivo | X |
| drivestoredirect:s:value | Redirecionamento de unidade/armazenamento:</br>Determina quais unidades de disco no computador local serão redirecionados e estarão disponíveis na sessão remota. | – Nenhum valor especificado: não redirecionar unidades</br>- *: Redirecionar todas as unidades de disco, incluindo unidades conectadas mais tarde</br>– DynamicDrives: redirecionar todas as unidades que estão conectadas mais tarde</br>– A unidade e os rótulos para uma ou mais unidades, tais como "drivestoredirect:s:C:;E:;": redirecionar as unidades especificadas | Não redirecionar nenhuma unidade | X |
| redirectclipboard:i:value | Redirecionamento da área de transferência:</br>Determina se o redirecionamento de área de transferência fica habilitado. | - 0: Área de transferência no computador local não fica disponível na sessão remota</br>- 1: Área de transferência no computador local fica disponível na sessão remota | 1 | X |
| redirectprinters:i:value | Redirecionamento de impressora:</br>Determina se as impressoras configuradas no computador local serão redirecionadas e estarão disponíveis na sessão remota | - 0: As impressoras no computador local não ficam disponíveis na sessão remota</br>- 1: As impressoras no computador local ficam disponíveis na sessão remota | 1 | X |
| redirectsmartcards:i:value | Redirecionamento de cartão inteligente:</br>Determina se dispositivos de cartão inteligente no computador local serão redirecionados e estarão disponíveis na sessão remota. |- 0: O dispositivo de cartão inteligente no computador local não fica disponível na sessão remota</br>- 1: O dispositivo de cartão inteligente no computador local fica disponível na sessão remota | 1 | X |

## <a name="display-settings"></a>Configurações de vídeo

| Configuração de RDP                        | Descrição            | Valores                 | Valor padrão          | Área de Trabalho Virtual do Windows |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| use multimon:i:value | Determina se a sessão remota usará uma ou várias telas do computador local. | - 0: Não habilitar suporte a várias telas</br>- 1: Habilitar suporte a várias telas | 0 | X |
| selectedmonitors:s:value | Especifica quais telas locais usar da sessão remota. As telas selecionadas devem ser contíguas. Requer que use Multimon seja definido como 1.</br></br>Disponível somente em clientes MSTSC (Caixa de Entrada do Windows) e MSRDC (Área de Trabalho do Windows). | Lista separada por vírgula de IDs de tela específicas do computador. As IDs podem ser recuperadas chamando mstsc.exe /l. A primeira ID listada será definida como a tela primária na sessão. | Todas as telas | X |
| maximizetocurrentdisplays:i:value | Determina qual tela a sessão remota passará para tela inteira ao maximizar. Requer que use Multimon seja definido como 1.</br></br>Disponível somente no cliente MSRDC (Área de Trabalho do Windows). | - 0: A sessão é exibida em tela inteira em telas inicialmente selecionadas ao maximizar</br>- 1: A sessão passa dinamicamente para tela inteira nas telas tocadas pela janela da sessão ao maximizar | 0 | X |
| singlemoninwindowedmode:i:value | Determina se uma sessão remota de várias telas alterna automaticamente para tela única ao sair da tela inteira. Requer que use Multimon seja definido como 1.</br></br>Disponível somente no cliente MSRDC (Área de Trabalho do Windows). | - 0: A sessão retém todas as telas ao sair da tela inteira</br>- 1: A sessão alterna para tela única ao sair da tela inteira | 0 | X |
| screen mode id:i:value | Determina se a janela da sessão remota aparece em tela inteira quando você inicia a conexão. | - 1: A sessão remota será exibida em uma janela</br>- 2: A sessão remota será exibida em tela inteira | 2 | X |
| smart sizing:i:value | Determina se o computador local dimensiona ou não o conteúdo da sessão remota para se ajustar ao tamanho da janela. | - 0: O conteúdo da janela local não será ajustado quando redimensionado</br>- 1: O conteúdo da janela local será ajustado quando redimensionado | 0 | X |
| dynamic resolution:i:value | Determina se a resolução da sessão remota é atualizada automaticamente quando a janela local é redimensionada. | - 0: A resolução da sessão permanece estática durante a sessão</br>- 1: A resolução de sessão é atualizada à medida que a janela local é redimensionada | 1 | X |
| desktop size id:i:value | Especifica as dimensões da área de trabalho da sessão remota com base em um conjunto de opções predefinidas. Essa configuração é anulada se desktopheight e desktopwidth são especificadas.| -0: 640×480</br>- 1: 800×600</br>- 2: 1024×768</br>- 3: 1280×1024</br>- 4: 1600×1200 | 1 | X |
| desktopheight:i:value | Especifica a altura da resolução (em pixels) da sessão remota. | Valor numérico entre 200 e 8192 | Corresponde ao computador local | X |
| desktopwidth:i:value | Especifica a largura da resolução (em pixels) da sessão remota. | Valor numérico entre 200 e 8192 | Corresponde ao computador local | X |
| desktopscalefactor:i:value | Especifica o fator de escala da sessão remota para que o conteúdo pareça maior. | Valor numérico da lista a seguir: 100, 125, 150, 175, 200, 250, 300, 400, 500 | 100 | X |

## <a name="remoteapp"></a>RemoteApp

| Configuração de RDP                        | Descrição            | Valores                 | Valor padrão          | Área de Trabalho Virtual do Windows |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| remoteapplicationcmdline:s:value | Parâmetros de linha de comando opcionais para o RemoteApp. | Parâmetros da linha de comando válidos. | | |
| remoteapplicationexpandcmdline:i:value | Determina se as variáveis de ambiente contidas no parâmetro de linha de comando do RemoteApp devem ser expandidas localmente ou remotamente. | - 0: Variáveis de ambiente devem ser expandidas com os valores do computador local</br>- 1: Variáveis de ambiente devem ser expandidas com os valores do computador remoto | 1 | |
| remoteapplicationexpandworkingdir:i:value | Determina se as variáveis de ambiente contidas no parâmetro de diretório de trabalho do RemoteApp devem ser expandidas localmente ou remotamente. | - 0: Variáveis de ambiente devem ser expandidas com os valores do computador local</br> - 1: Variáveis de ambiente devem ser expandidas com os valores do computador remoto.</br>O diretório de trabalho do RemoteApp é especificado através do shell de parâmetro do diretório de trabalho. | 1 | |
| remoteapplicationfile:s:value | Especifica um arquivo a ser aberto no computador remoto pelo RemoteApp.</br>Para arquivos locais a serem abertos, também é necessário habilitar o redirecionamento de unidade para a unidade de origem. | Caminho de arquivo válido. | | |
| remoteapplicationicon:s:value | Especifica o arquivo de ícone a ser exibido na UI de cliente ao iniciar um RemoteApp. Se nenhum nome de arquivo for especificado, o cliente usará o ícone padrão da Área de Trabalho Remota. Somente há suporte para arquivos ".ico". | Caminho de arquivo válido. | | |
| remoteapplicationmode:i:value | Determina se uma conexão é iniciada como uma sessão do RemoteApp. | - 0: Não iniciar uma sessão do RemoteApp</br>- 1: Iniciar uma sessão do RemoteApp | 1 | |
| remoteapplicationname:s:value | Especifica o nome do RemoteApp na interface cliente ao iniciar o RemoteApp.| Nome de exibição do aplicativo. Por exemplo: "Excel 2016" | | |
| remoteapplicationprogram:s:value | Especifica o nome do executável ou alias do RemoteApp. | Alias ou nome válido. For exemplo, "EXCEL" | | |
