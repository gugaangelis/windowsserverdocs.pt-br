---
title: Novidades para área de trabalho remota no Mac?
description: Saiba mais sobre alterações recentes para o cliente de área de trabalho remota para Mac
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
ms.openlocfilehash: 11da8bc333da7f44b2732266837d2df216142f85
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279117"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>Novidades para o cliente de área de trabalho remota no macOS?

Podemos atualizar o [cliente de área de trabalho remota para macOS](remote-desktop-mac.md), adicionando novos recursos e corrigir problemas regularmente. Confira as últimas atualizações abaixo.

Se você encontrar problemas, você pode sempre em contato conosco por meio de **ajudar a gt _ relatório um problema**.

## <a name="updates-for-version-10210"></a>Atualizações para a versão 10.2.10
*Data de publicação: 3/30/2019*

- Nesta versão, podemos resolvido instabilidade causada pela atualização macOS 10.14.4 recente. Corrigimos também mispaints que apareceu quando decodificar AVC codec dados codificados por um servidor usando um hardware NVIDIA.

## <a name="updates-for-version-1029"></a>Atualizações para a versão 10.2.9
*Data de publicação: 3/6/2019*

- Nesta versão, que corrigimos um problema de conectividade de gateway de área de trabalho remota que pode ocorrer quando o redirecionamento de servidor executa coloque.
- Também podemos tratadas uma regressão de gateway de área de trabalho remota causada pelo 10.2.8 update.

## <a name="updates-for-version-1028"></a>Atualizações para a versão 10.2.8
*Data de publicação: 1/3/2019*

- Problemas de conectividade mostrada ao usar um Gateway de área de trabalho remota resolvidos.
- Avisos de certificado incorreto fixo que foram exibidos ao se conectar.
- Resolvido alguns casos em que a barra de menus e dock seriam desnecessariamente Ocultar ao iniciar aplicativos remotos.
- Reformulada o código de redirecionamento de área de transferência para endereço panes e travamentos que tenham sido afligindo alguns usuários.
- Corrigido um bug que causou o Centro de Conexão desnecessariamente rolar ao iniciar uma conexão.

## <a name="updates-for-version-1027"></a>Atualizações para a versão 10.2.7
*Data de publicação: 2/6/2019*

- Nesta versão, podemos resolvido mispaints de elementos gráficos (causados por um bug de codificação de servidor) que apareceu quando usando o modo AVC444.

## <a name="updates-for-version-1026"></a>Atualizações para a versão 10.2.6
*Data de publicação: 1/28/2019*

- Adicionado suporte para o codec AVC (420 e 444), disponível ao se conectar a versões atuais do Windows 10.
- No ajuste para o modo de janela, uma atualização de janela agora ocorre imediatamente após um redimensionamento para garantir que o conteúdo é renderizado no nível de interpolação correto.
- Corrigido um bug de layout que causou o feeds cabeçalhos a sobreposição para alguns usuários.
- Limpa a interface do usuário do aplicativo preferências.
- Refinada a interface do usuário da área de trabalho de adicionar ou editar.
- Feitas muitas adaptação e conclusão ajustes para as exibições de bloco e lista Conexão central para desktops e feeds.

>[!NOTE]
>Há um bug na pasta macOS 10.14.0 e 10.14.1 que podem causar ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (aninhada profundamente dentro da pasta ~/Library) para consumir uma grande quantidade de espaço em disco. Para resolver esse problema, exclua o conteúdo da pasta e atualizar para o macOS 10.14.2. Observe que um efeito colateral de excluir o conteúdo da pasta é que imagens snapshot atribuídas para indicadores serão excluídas. Essas imagens serão ser regeneradas quando reconectar-se ao computador remoto.

## <a name="updates-for-version-1024"></a>Atualizações para a versão 10.2.4
*Data de publicação: 12/18/2018*

- Adicionado o suporte de modo escuro para macOS Mojave 10.14.
- Uma opção para importar de 8 de área de trabalho remota da Microsoft agora é exibido no Centro de Conexão se estiver vazia.
- Resolvido compatibilidade de redirecionamento de pasta com alguns aplicativos de terceiros enterprise.
- Problemas resolvidos onde os usuários foram obtendo um erro de Gateway de área de trabalho remota 0x30000069 devido à segurança problemas de fallback de protocolo.
- Problemas de renderização progressivas fixo com que alguns usuários foram enfrentando cabem em modo de janela.
- Corrigido um bug que impediu arquivo copiar e colar de copiar a versão mais recente de um arquivo.
- Aprimorada baseada no mouse de rolagem para deltas pequenos de rolagem.

## <a name="updates-for-version-1023"></a>Atualizações para a versão 10.2.3
*Data de publicação: 11/06/2018*

- Adicionado suporte para o ajuste do arquivo RDP "remoteapplicationcmdline" para cenários de aplicativo remoto.
- O título da janela da sessão agora inclui o nome do arquivo RDP (e nome do servidor) quando iniciado a partir de um arquivo RDP.
- Corrigido RD relatado problemas de desempenho do gateway.
- Gateway de área de trabalho remota relatado fixo falha.
- Correção de problemas em que poderia ser interrompido a conexão ao se conectar por meio de um gateway de área de trabalho remota.
- Melhor tratamento de aplicativos remotos de tela inteira, ocultando inteligente a barra de menus e dock.
- Fixos cenários onde aplicativos remotos permanecem ocultos depois de ser inicializado.
- Resolvido renderização lenta atualizações ao usar "Ajustar à janela" com aceleração de hardware desabilitada.
- Manipulado erros de criação de banco de dados causados por permissões incorretas quando o cliente é iniciado. 
- Corrigido um problema em que o cliente estava falhando consistentemente na inicialização e não iniciar para alguns usuários.
- Corrigido um cenário onde conexões incorretamente foram importadas em tela inteira de 8 de área de trabalho remota.

## <a name="updates-for-version-1022"></a>Atualizações para a versão 10.2.2
*Data de publicação: 10/09/2018*

- Um novo centro de Conexão que dá suporte a arrastar e soltar, organização manual das áreas de trabalho, redimensionáveis colunas no modo de exibição de lista, com base em coluna de classificação e gerenciamento de grupo mais simples.
- A Central de Conexão agora lembra o último pivô ativo (Desktops ou Feeds) quando fechando o aplicativo.
- Ter sido revisadas a credencial solicitando que a interface do usuário e fluxos.
- Comentários do Gateway de área de trabalho remota agora são parte do status da conexão da interface do usuário.
- Importar as configurações do cliente versão 8 foi aprimorado.
- Arquivos RDP apontando para pontos de extremidade do RemoteApp agora podem ser importados para o Centro de Conexão.
- Otimizações de exibição retina para cenários de área de trabalho remota único monitor.
- Suporte para especificar o nível de interpolação de elementos gráficos (que afeta pouco de desfoque) quando não estiver usando as otimizações de Retina.
- suporte de 256 cores para habilitar a conectividade para o Windows 2000.
- Corrigido recorte das bordas direita e inferior da tela ao se conectar ao Windows 7, Windows Server 2008 R2 e versões anteriores.
- Copiar um arquivo local para o Outlook (em execução em uma sessão remota) agora adiciona o arquivo como um anexo.
- Corrigido um problema que foi lento transferências de arquivos com base em área de trabalho se os arquivos de origem de um compartilhamento de rede.
- Resolvido um bug que estava causando para o Excel (em execução em uma sessão remota) congelada ao salvar um arquivo em uma pasta redirecionada.
- Corrigido um problema que estava fazendo com que não há espaço livre a ser relatado para pastas redirecionadas.
- Corrigido um bug que causou miniaturas consumir muito armazenamento em disco em macOS 10.14.
- Adicionado suporte para impor políticas de redirecionamento de dispositivo do Gateway de área de trabalho remota.
- Corrigido um problema que impediu o windows de sessão de fechamento ao desconectar-se de uma conexão usando o Gateway de área de trabalho remota.
- Se a autenticação de nível de rede (NLA) não é imposto pelo servidor, você agora serão roteados para a tela de logon se sua senha expirou.
- Correção de problemas de desempenho mostrada quando grandes quantidades de dados estava sendo transferidos através da rede.
- Correções de redirecionamento de cartão inteligente.
- Suporte para todos os valores possíveis do "EnableCredSspSupport" e "Nível de autenticação" configurações de arquivo RDP se a chave do ClientSettings.EnforceCredSSPSupport usuário padrão (no domínio com.microsoft.rdc.macos) é definida como 0.
- Suporte para a configuração de arquivo RDP "Prompt para credenciais no cliente" quando NLA não for negociada.
- Suporte para logon baseada em cartão inteligente por meio de redirecionamento de cartão inteligente no prompt de Winlogon quando NLA não for negociada.
- Corrigido um problema que impediu o download de feed recursos que têm espaços na URL.

## <a name="updates-for-version-1021"></a>Atualizações para a versão 10.2.1
*Data de publicação: 08/06/2018*

- Conectividade habilitada para Azure Active Directory (AAD) ingressou computadores. Para se conectar a um AAD ingressados no computador, seu nome de usuário deve estar em um dos seguintes formatos: "AzureAD\user" ou "AzureAD\user@domain".
- Resolvido alguns bugs que afetam o uso de cartões inteligentes em uma sessão remota.

## <a name="updates-for-version-1020"></a>Atualizações para a versão 10.2.0
*Data de publicação: 07/24/2018*

- Atualizações incorporadas para conformidade GDPR.
- MicrosoftAccount\username@domainAgora é aceito como um nome de usuário válido.
- Compartilhamento de área de transferência foi reescrito para ser mais rápido e dar suporte a mais formatos.
- Copiar e colar texto, imagens ou arquivos entre sessões agora ignora a área de transferência do computador local.
- Agora você pode conectar por meio de um servidor de Gateway de área de trabalho remota com um certificado não confiável (se você aceitar os prompts de aviso).
- Aceleração de hardware metal agora é usada (onde compatível) para acelerar o processamento e otimizar o uso da bateria.
- Ao usar a aceleração de hardware Metal que tentamos funcionam alguns mágica para tornar os gráficos de sessão nítida.
- Livrar-se de algumas instâncias onde windows seriam travar ao redor depois de ser fechado.
- Fixos bugs que foram impedindo a inicialização de programas do RemoteApp em alguns cenários.
- Corrigido um erro de sincronização de canal de Gateway de área de trabalho remota foi resultando em 0x204 erros.
- A forma de cursor de mouse agora atualiza corretamente ao sair de uma sessão ou janela RemoteApp.
- Corrigido um bug de redirecionamento de pasta que estava causando a perda de dados quando copiar e colar pastas.
- Corrigido um problema de redirecionamento de pasta que causou o relatório incorreto de tamanhos de pasta.
- Corrigido uma regressão que impedia o logon em um computador ingressado no AAD usando uma conta local.
- Bugs corrigidos que estavam causando a sessão do conteúdo da janela seja cortado.
- Adicionado suporte para certificados de ponto de extremidade de área de trabalho remota que contenham chaves assimétricas curva elíptica.
- Corrigido um bug que impedia o download de recursos gerenciados em alguns cenários.
- Resolvido um problema de recorte com o Centro de conexão fixados.
- Corrigido as caixas de seleção na folha de propriedades de exibição para funcionam melhor juntos.
- Taxa de proporção bloqueio agora está desabilitado quando a alteração de exibição dinâmica estiver em vigor.
- Resolvido problemas de compatibilidade com infraestrutura F5.
- Atualizada a manipulação de senhas em branco para garantir que as mensagens corretas são mostradas em tempo de se conectar.
- Mouse fixo rolagem problemas de compatibilidade com MapInfra Pro.
- Corrigido alguns problemas de alinhamento no Centro de Conexão durante a execução em Mojave.

## <a name="updates-for-version-1018"></a>Atualizações para a versão 10.1.8
*Data de publicação: 05/04/2018*

- Adicionado o suporte para alterar a resolução remota redimensionando a janela de sessão!
- Fixos cenários onde o recurso remoto feed download poderiam levar um tempo excessivamente longo.
- Resolvido o erro 0x207 que pode ocorrer quando se conectar com servidores não corrigidas com a atualização de correção do oracle CredSSP criptografia (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Atualizações para a versão 10.1.7
*Data de publicação: 04/05/2018*

- Feitas correções de segurança para incorporar atualizações de correção CredSSP criptografia oracle conforme descrito em CVE-2018-0886.
- Aprimorada RemoteApp ícone e mouse cursor renderização para resolver mispaints relatados.
- Resolvido problemas onde RemoteApp windows apareceram atrás do Centro de Conexão.
- Corrigido um problema que ocorreu ao editar recursos locais após a importação de 8 de área de trabalho remota.
- Agora você pode iniciar uma conexão pressionando ENTER em um bloco da área de trabalho.
- Quando você estiver no modo de exibição de tela inteira, CMD + M agora corretamente mapeia para WIN + M.
- A Central de Conexão, preferências e sobre windows agora respondem a CMD + M.
- Agora você pode começar a descobrir feeds pressionando ENTER na página **Adicionar recursos remotos** .
- Corrigido um problema em que um novo feed de recursos remotos mostrou vazio no Centro de Conexão até depois que você atualizados.
 
## <a name="updates-for-version-1016"></a>Atualizações para a versão 10.1.6
*Data de publicação: 26/03/2018*

- Corrigido um problema em que o windows RemoteApp seriam reordenar propriamente ditos.
- Resolvido um bug que causou algumas janelas RemoteApp fique preso por trás de sua janela pai.
- Resolvido um problema de deslocamento de ponteiro de mouse que afetados alguns programas do RemoteApp.
- Corrigido um problema em que a partir de uma nova conexão deu ao foco para uma sessão existente, em vez de abrir uma janela de nova sessão.
- Corrigimos um erro com uma mensagem de erro - você verá a mensagem correta agora se não foi possível encontrar seu gateway.
- O atalho de sair (⌘ + Q) agora é mostrado consistentemente da interface do usuário.
- Aprimorada a qualidade da imagem quando alongamento no modo "Ajustar à janela".
- Corrigido uma regressão que causou a várias instâncias da pasta doméstica para aparecer na sessão remota.
- Atualizado o ícone padrão para blocos da área de trabalho.
