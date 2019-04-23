---
title: O que há de novo para a área de trabalho remota no Mac?
description: Saiba mais sobre as alterações recentes para o cliente de área de trabalho remota para Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 03/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: d0cf81f24374e81ca28c2d2cfd83a394e096706c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844787"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>O que há de novo para o cliente de área de trabalho remota no macOS?

Podemos atualizar regularmente o [cliente de área de trabalho remota para macOS](remote-desktop-mac.md), adição de novos recursos e corrigir problemas. Confira as últimas atualizações abaixo.

Se você tiver algum problema, você pode sempre em contato conosco por meio **Ajuda > relatar um problema**.

## <a name="updates-for-version-1029"></a>Atualizações para a versão 10.2.9
*Data de publicação: 3/6/2019*

- Nesta versão, que corrigimos um problema de conectividade de gateway de área de trabalho remota que pode ocorrer quando o redirecionamento de servidor usa coloca.
- Também resolvemos uma regressão de gateway de área de trabalho remota causada pelo 10.2.8 atualização.

## <a name="updates-for-version-1028"></a>Atualizações para a versão 10.2.8
*Data de publicação: 3/1/2019*

- Resolver problemas de conectividade que surgiram ao usar um Gateway de área de trabalho remota.
- Corrigidos os avisos de certificado incorreto que foram exibidos ao se conectar.
- Resolvido alguns casos em que a barra de menus e encaixe desnecessariamente ocultaria ao iniciar aplicativos remotos.
- Reformulada o código de redirecionamento de área de transferência ao endereço panes e travamentos que tem sido que atacam alguns usuários.
- Corrigido um bug que causou o Centro de Conexão desnecessariamente rolar ao iniciar uma conexão.

## <a name="updates-for-version-1027"></a>Atualizações para a versão 10.2.7
*Data de publicação: 2/6/2019*

- Nesta versão, nós abordamos mispaints gráficos (causados por um bug de codificação do servidor) que eram exibidos ao usar o modo de AVC444.

## <a name="updates-for-version-1026"></a>Atualizações para a versão 10.2.6
*Data de publicação: 1/28/2019*

- Adicionado suporte para o codec AVC (420 e 444), disponível ao conectar-se às versões atuais do Windows 10.
- Ajustar ao modo de janela, uma atualização de janela agora ocorre imediatamente após um redimensionamento para garantir que o conteúdo é renderizado no nível correto de interpolação.
- Corrigido um bug de layout que causou o feeds cabeçalhos sobrepor para alguns usuários.
- Limpa a interface do usuário do aplicativo preferências.
- Clara a interface do usuário da área de trabalho de adicionar/editar.
- Feitas muita adaptação e conclusão ajustes para as exibições de bloco e lista de centro de Conexão para desktops e feeds.

>[!NOTE]
>Há um bug na pasta macOS 10.14.0 e 10.14.1 que podem causar ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (aninhada profundamente na pasta ~/Library) para consumir uma grande quantidade de espaço em disco. Para resolver esse problema, exclua o conteúdo da pasta e atualizar para o macOS 10.14.2. Observe que um efeito colateral de excluir o conteúdo da pasta que atribuído aos indicadores de imagens de instantâneo serão excluídas. Essas imagens serão regeneradas quando reconectar-se ao PC remoto.

## <a name="updates-for-version-1024"></a>Atualizações para a versão 10.2.4
*Data de publicação: 12/18/2018*

- Adicionado suporte ao modo escuro para macOS Mojave 10.14.
- Uma opção para importar de 8 de área de trabalho remota da Microsoft agora aparece no centro da Conexão se ela estiver vazia.
- Compatibilidade de redirecionamento de pasta endereçados com alguns aplicativos empresariais de terceiros.
- Problemas resolvidos em que os usuários estavam sendo um erro de Gateway de área de trabalho remota 0x30000069 devido à segurança problemas de fallback de protocolo.
- Problemas de renderização progressivo fixa com que alguns usuários estavam enfrentando se ajustar ao modo de janela.
- Corrigido um bug que impedia a cópia do arquivo e cole de copiar a versão mais recente de um arquivo.
- Aprimorado com base no mouse de rolagem para deltas de rolagem pequena.

## <a name="updates-for-version-1023"></a>Atualizações para a versão 10.2.3
*Data de publicação: 11/06/2018*

- Adicionado suporte para a configuração de arquivo "remoteapplicationcmdline" RDP para cenários de aplicativo remoto.
- O título da janela da sessão agora inclui o nome do arquivo RDP (e o nome do servidor) quando iniciado a partir de um arquivo RDP.
- Correção relatada de área de trabalho remota de problemas de desempenho do gateway.
- Falhas de gateway de área de trabalho remota relatado fixado.
- Problemas corrigidos em que poderia ser interrompido a conexão ao conectar-se através de um gateway de área de trabalho remota.
- Melhor tratamento de tela inteira de aplicativos remotos, inteligentemente ocultando a barra de menus e o encaixe.
- Correção de cenários em que os aplicativos remotos permaneciam ocultos depois de ser inicializado.
- Resolvido processamento lento atualizações ao usar o "Ajustar à janela" com aceleração de hardware desabilitada.
- Tratados os erros de criação de banco de dados causados por permissões incorretas quando o cliente é inicializado. 
- Corrigido um problema em que o cliente estava falhando consistentemente na inicialização e não a partir de alguns usuários.
- Correção de um cenário em que as conexões foram importadas incorretamente como a tela inteira de 8 de área de trabalho remota.

## <a name="updates-for-version-1022"></a>Atualizações para a versão 10.2.2
*Data de publicação: 10/09/2018*

- Um novo centro de Conexão que dá suporte a arrastar e soltar, a organização manual das áreas de trabalho, as colunas redimensionáveis no modo de exibição de lista, baseado em coluna de classificação e gerenciamento de grupo mais simples.
- O Centro de Conexão agora lembra o último pivot Active Directory (áreas de trabalho ou Feeds) quando fechar o aplicativo.
- A credencial solicitando a interface do usuário e fluxos foram aperfeiçoados.
- Comentários de Gateway de área de trabalho remota agora são parte do status da interface do usuário está se conectando.
- Importação de configurações do cliente versão 8 foi aprimorado.
- Arquivos RDP que aponta para pontos de extremidade do RemoteApp agora podem ser importados para o Centro de Conexão.
- Otimizações de exibição de retina para cenários de área de trabalho remota de monitor único.
- Suporte para especificar o nível de interpolação de gráficos (que afeta um pouco de desfoque) ao não usar otimizações de Retina.
- suporte de 256 cores para habilitar a conectividade com o Windows 2000.
- Correção de recorte das bordas direita e inferior da tela ao se conectar ao Windows 7, Windows Server 2008 R2 e versões anteriores.
- Copiar um arquivo local para o Outlook (em execução em uma sessão remota) agora adiciona o arquivo como um anexo.
- Corrigido um problema que estava causando lentidão transferências de arquivos com base em área de trabalho se os arquivos de um compartilhamento de rede foi originado.
- Corrigido um bug que estava causando para o Excel (em execução em uma sessão remota) parar de responder ao salvar um arquivo em uma pasta redirecionada.
- Corrigido um problema que fazia com que não há espaço livre a ser relatado para pastas redirecionadas.
- Corrigido um bug que causou miniaturas para consumir muito armazenamento em disco no macOS 10.14.
- Adicionado suporte para a imposição de políticas de redirecionamento de dispositivo de Gateway de área de trabalho remota.
- Corrigido um problema que impediu o windows de sessão de fechamento durante a desconexão de uma conexão usando o Gateway de área de trabalho remota.
- Se a autenticação de nível de rede (NLA) não é imposta pelo servidor, você agora será roteado para a tela de logon se sua senha tiver expirado.
- Correção de problemas de desempenho que são exibidas quando grandes quantidades de dados estava sendo transferidos pela rede.
- Correções de redirecionamento de cartão inteligente.
- Suporte para todos os valores possíveis do "Nível de autenticação" e "EnableCredSspSupport" configurações do arquivo RDP se a chave do ClientSettings.EnforceCredSSPSupport usuário padrão (no domínio com.microsoft.rdc.macos) é definida como 0.
- Suporte para a configuração do arquivo RDP do "Prompt para credenciais no cliente" quando o NLA não for negociada.
- Suporte para logon baseado em cartão inteligente por meio de redirecionamento de cartão inteligente no prompt do Winlogon, quando o NLA não for negociada.
- Corrigido um problema que impediu o download de recursos que têm espaços na URL do feed.

## <a name="updates-for-version-1021"></a>Atualizações para a versão 10.2.1
*Data de publicação: 08/06/2018*

- Computadores ingressados no conectividade habilitada para Azure Active Directory (AAD). Para se conectar ao computador ingressado no AAD, seu nome de usuário deve estar em um dos seguintes formatos: "AzureAD\user" ou "AzureAD\user@domain".
- Resolvido alguns bugs que afetam o uso de cartões inteligentes em uma sessão remota.

## <a name="updates-for-version-1020"></a>Atualizações para a versão 10.2.0
*Data de publicação: 07/24/2018*

- Atualizações incorporadas para conformidade com GDPR.
- MicrosoftAccount\username@domain Agora é aceito como um nome de usuário válido.
- Compartilhamento de área de transferência foi reformulado para ser mais rápida e dar suporte a mais formatos.
- Copiando e colando o texto, imagens ou arquivos entre sessões agora ignora a área de transferência do computador local.
- Você agora pode se conectar por meio de um servidor de Gateway de área de trabalho remota com um certificado não confiável (se você aceitar os prompts de aviso).
- Aceleração de hardware metal agora é usada (onde houver suporte) para acelerar o processamento e otimizar o uso da bateria.
- Ao usar a aceleração de hardware de Metal, que podemos tentar trabalhar alguma mágica para tornar os gráficos de sessão nítida.
- Livrar-se de algumas instâncias em que a windows poderia ser interrompido em torno de depois de serem fechados.
- Bugs corrigidos que estavam impedindo a inicialização de programas do RemoteApp em alguns cenários.
- Corrigido um erro de sincronização de canal do Gateway de área de trabalho remota que resultava em 0x204 erros.
- A forma de cursor do mouse agora atualiza corretamente ao mover para fora de uma sessão ou a janela do RemoteApp.
- Corrigido um bug de redirecionamento de pasta que estava causando a perda de dados ao copiar e colar pastas.
- Corrigido um problema de redirecionamento de pasta que causou a emissão de relatórios incorretos de tamanhos de pastas.
- Corrigida uma regressão que estava impedindo o registro em log em um computador ingressado no AAD, usando uma conta local.
- Bugs corrigidos que estavam causando a sessão de conteúdo da janela a ser recortada.
- Adicionado suporte para certificados de ponto de extremidade de área de trabalho remota que contêm as chaves assimétricas de curva elíptica.
- Corrigido um bug que estava impedindo o download de recursos gerenciados em alguns cenários.
- Corrigido um problema de recorte com a Central de conexão fixo.
- Corrigido as caixas de seleção na folha de propriedades de exibição para trabalhar melhor juntos.
- Bloqueio de taxa de proporção, agora é desabilitado quando a alteração da exibição dinâmica está em vigor.
- Resolveu os problemas de compatibilidade com a infraestrutura de F5.
- Atualizado o tratamento de senhas em branco para garantir que as mensagens corretas são mostradas no tempo de conexão.
- Problemas de compatibilidade com MapInfra Pro de rolagem de mouse fixado.
- Corrigidos alguns problemas de alinhamento no centro da Conexão durante a execução em Mojave.

## <a name="updates-for-version-1018"></a>Atualizações para a versão 10.1.8
*Data de publicação: 05/04/2018*

- Adicionado suporte para alterar a resolução remota redimensionando a janela da sessão!
- Fixos cenários em que o recurso remoto feed download levaria muito tempo.
- Foi resolvido um erro de 0x207 que poderia ocorrer durante a conexão com servidores não atualizados com a atualização de correção CredSSP criptografia oracle (CVE-2018 0886).

## <a name="updates-for-version-1017"></a>Atualizações para a versão 10.1.7
*Data de publicação: 04/05/2018*

- Feitas as correções de segurança para incorporar as atualizações de correção do CredSSP criptografia oracle conforme descrito em 0886 de CVE-2018.
- Melhor RemoteApp ícone e o mouse cursor renderização para tratar mispaints relatadas.
- Resolveu problemas em que o windows RemoteApp apareceram por trás do centro da Conexão.
- Corrigido um problema que ocorreu ao editar recursos locais após a importação de 8 de área de trabalho remota.
- Agora você pode iniciar uma conexão ao pressionar ENTER em um bloco da área de trabalho.
- Quando você estiver no modo de exibição de tela inteira, CMD + M agora corretamente é mapeado para WIN + M.
- A Central de Conexão, preferências e sobre windows agora respondem CMD + M.
- Agora você pode começar a descobrir feeds pressionando ENTER na **adicionando recursos remotos** página.
- Corrigido um problema em que um novo feed de recursos remotos aparecia vazio no Centro de Conexão até depois que você atualizou.
 
## <a name="updates-for-version-1016"></a>Atualizações para a versão 10.1.6
*Data de publicação: 03/26/2018*

- Corrigido um problema em que o windows RemoteApp seriam reordenar em si.
- Resolver um bug que causou algumas janelas de RemoteApp fique preso atrás de sua janela pai.
- Corrigido um problema de deslocamento de ponteiro de mouse que afetava alguns programas RemoteApp.
- Corrigido um problema em que iniciar uma nova conexão forneceu foco a uma sessão existente, em vez de abrir uma janela de nova sessão.
- Corrigimos um erro com uma mensagem de erro - você verá a mensagem correta agora se não foi possível encontrar o seu gateway.
- O atalho Quit (⌘ + Q) agora é consistentemente mostrado na interface do usuário.
- Melhorou a qualidade de imagem ao ampliar no modo "Ajustar à janela".
- Corrigida uma regressão que causou a várias instâncias da pasta base apareça na sessão remota.
- Atualizado o ícone padrão para a área de trabalho lado a lado.
