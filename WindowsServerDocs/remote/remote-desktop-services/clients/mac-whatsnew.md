---
title: Novidades do cliente para macOS
description: Saiba mais sobre as recentes alterações no cliente da Área de Trabalho Remota para MAC
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
ms.date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: 9ae58103b00941bb71d447641b1cdab7c02fa20b
ms.sourcegitcommit: 51eaab0f860312d97293fd90f3e632e7caee3df1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70151027"
---
# <a name="whats-new-in-the-macos-client"></a>Novidades do cliente para macOS

Atualizamos regularmente o [cliente da Área de Trabalho Remota para macOS](remote-desktop-mac.md), adicionando novos recursos e corrigindo problemas. Confira as últimas atualizações abaixo.

Caso tenha problemas, você sempre pode entrar em contato por meio de **Ajuda > Relatar um problema**.

## <a name="updates-for-version-1030"></a>Atualizações para a versão 10.3.0
*Data de publicação: 27/08/2019*

Há algumas semanas desde a última atualização, mas trabalhamos duro durante esse período. A versão 10.3.0 traz alguns recursos novos e muitas correções subjacentes.

 - Agora é possível redirecionar a câmera ao se conectar ao Windows 10 1809, Windows Server 2019 e posterior.
 - No Mojave e no Catalina, adicionamos uma nova caixa de diálogo que solicita sua permissão para usar o microfone e a câmera para redirecionamento de dispositivo.
 - O fluxo de assinatura do feed foi reescrito para ser mais simples e mais rápido.
 - O redirecionamento da área de transferência agora inclui RTF (Rich Text Format).
 - Ao inserir sua senha, você tem a opção de revelá-la com uma caixa de seleção "Mostrar senha".
 - Cenários abordados em que a janela de sessão passava de um monitor ao outro.
 - A Central de Conexão exibe ícones de aplicativos remotos de alta resolução (quando disponíveis).
 - Cmd + A é mapeado para Ctrl + A quando os atalhos da área de transferência do Mac estão sendo usados.
 - Cmd + R agora atualiza todos os feeds assinados.
 - Novas opções de clique secundário foram adicionadas para expandir ou recolher todos os grupos ou feeds na Central de Conexão.
 - Adicionada uma nova opção de clique secundário para alterar o tamanho do ícone na guia Feeds da Central de Conexão.
 - Um novo ícone de aplicativo simplificado e mais limpo.

## <a name="updates-for-version-10213"></a>Atualizações para a versão 10.2.13

*Data de publicação: 8/5/2019*

- Correção de um travamento que ocorria ao conectar por meio de um Gateway de Área de Trabalho Remota.
- Inclusão de um aviso de privacidade para a caixa de diálogo "Adicionar Feed".

## <a name="updates-for-version-10212"></a>Atualizações para a versão 10.2.12

*Data de publicação: 16/4/2019*

- Resolução de desconexões aleatórias (com código de erro 0x904) que ocorriam ao conectar por meio de um Gateway de Área de Trabalho Remota.
- Correção de um bug que esvaziava a lista de resoluções nas preferências após a instalação.
- Correção de um bug que causava falha do computador cliente se determinadas resoluções fossem adicionadas à lista de resoluções.
- Solução de um loop de prompt de autenticação ADAL ao conectar para implantações de Área de Trabalho Virtual do Windows.

## <a name="updates-for-version-10210"></a>Atualizações para a versão 10.2.10

*Data de publicação: 30/3/2019*

- Nesta versão, abordamos a instabilidade causada pela recente atualização do macOS 10.14.4. Também corrigimos imagens incorretas que apareciam ao decodificar dados de codec de AVC codificados por um servidor usando o hardware da NVIDIA.

## <a name="updates-for-version-1029"></a>Atualizações para a versão 10.2.9

*Data de publicação: 6/3/2019*

- Nesta versão, corrigimos um problema de conectividade de gateway de área de trabalho remota que pode ocorrer durante o redirecionamento do servidor.
- Também resolvemos uma regressão de gateway de área de trabalho remota causada pela atualização 10.2.8.

## <a name="updates-for-version-1028"></a>Atualizações para a versão 10.2.8

*Data de publicação: 1/3/2019*

- Foram resolvidos problemas de conectividade que surgiram ao usar um Gateway de Área de Trabalho Remota.
- Foram corrigidos avisos de certificado incorreto que eram exibidos ao conectar.
- Foram resolvidos alguns casos em que a barra de menus e encaixe ocultava desnecessariamente ao iniciar aplicativos remotos.
- Foi reformulado o código de redirecionamento de área de transferência para resolver falhas e travamentos que têm incomodado alguns usuários.
- Foi corrigido um bug que causava desnecessariamente a rolagem do Connection Center o ao iniciar uma conexão.

## <a name="updates-for-version-1027"></a>Atualizações para a versão 10.2.7

*Data de publicação: 6/2/2019*

- Nesta versão, abordamos imagens incorretas em gráficos (causados por um bug de codificação do servidor) que apareciam ao usar o modo de AVC444.

## <a name="updates-for-version-1026"></a>Atualizações para a versão 10.2.6

*Data de publicação: 28/1/2019*

- Foi adicionado suporte para o codec AVC (420 e 444), disponível ao conectar às versões atuais do Windows 10.
- No modo Ajustar à janela, uma atualização de janela agora ocorre imediatamente após um redimensionamento para garantir que o conteúdo seja renderizado no nível de interpolação correto.
- Correção de um bug de layout que causava sobreposição de cabeçalhos de feeds para alguns usuários.
- Limpeza da interface do usuário Preferências do aplicativo.
- Aperfeiçoamento da interface do usuário Adicionar/Editar da área de trabalho.
- Realização de vários ajustes de adaptação e conclusão para as exibições de lista e de bloco do Connection Center para desktops e feeds.

>[!NOTE]
>Há um bug no macOS 10.14.0 e 10.14.1 que pode fazer com que a pasta ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (aninhada profundamente na pasta ~/Library) consuma uma grande quantidade de espaço em disco. Para resolver esse problema, exclua o conteúdo da pasta e atualize para o macOS 10.14.2. Observe que um efeito colateral de excluir o conteúdo da pasta é a posterior exclusão das imagens de instantâneo atribuídas a indicadores. Essas imagens serão regeneradas ao reconectar ao PC remoto.

## <a name="updates-for-version-1024"></a>Atualizações para a versão 10.2.4

*Data de publicação: 18/12/2018*

- Inclusão de suporte ao modo escuro para macOS Mojave 10.14.
- Uma opção para importar da Área de Trabalho Remota 8 da Microsoft agora aparece no Connection Center se ela estiver vazia.
- Solução da compatibilidade de redirecionamento de pasta com alguns aplicativos empresariais de terceiros.
- Resolução de problemas em que os usuários estavam obtendo um 0x30000069 Erro de Gateway de Área de Trabalho Remota devido à problemas de fallback do protocolo de segurança.
- Correção de problemas de renderização progressiva enfrentados por alguns usuários ao ajustar para o modo de janela.
- Correção de um bug que impedia que a cópia e colagem de arquivo copiasse a versão mais recente de um arquivo.
- Aprimoramento da rolagem com base no mouse para deltas de rolagem pequena.

## <a name="updates-for-version-1023"></a>Atualizações para a versão 10.2.3

*Data de publicação: 06/11/2018*

- Inclusão de suporte para a configuração do arquivo RDP "remoteapplicationcmdline" para cenários de aplicativo remoto.
- O título da janela da sessão agora inclui o nome do arquivo RDP (e o nome do servidor) quando iniciado a partir de um arquivo RDP.
- Correção de problemas relatados de desempenho do gateway de área de trabalho remota.
- Correção de problemas relatados de falhas do gateway de área de trabalho remota.
- Correção de problemas em que a conexão poderia ser interrompida ao conectar por meio de um gateway de área de trabalho remota.
- Melhor tratamento de aplicativos remotos de tela inteira, inteligentemente ocultando a barra de menus e o encaixe.
- Correção de cenários em que os aplicativos remotos permaneciam ocultos após serem iniciados.
- Resolução de atualizações de processamento lento ao usar "Ajustar à janela" com aceleração de hardware desabilitada.
- Tratamento de erros de criação de banco de dados causados por permissões incorretas quando o cliente é inicializado.
- Correção de um problema em que o cliente estava falhando consistentemente na inicialização e não iniciava para alguns usuários.
- Correção de um cenário em que as conexões eram importadas incorretamente como de tela inteira da Área de Trabalho Remota 8.

## <a name="updates-for-version-1022"></a>Atualizações para a versão 10.2.2

*Data de publicação: 09/10/2018*

- Um Connection Center totalmente novo que dá suporte a arrastar e soltar, organização manual das áreas de trabalho, colunas redimensionáveis no modo de exibição de lista, classificação e gerenciamento com base em coluna e a gerenciamento de grupo simplificado.
- O Connection Center agora lembra o último pivô ativo (Áreas de Trabalho ou Feeds) ao fechar o aplicativo.
- A interface do usuário de solicitação de credencial e fluxos foram revisados.
- O feedback de Gateway de Área de Trabalho Remota agora faz parte da interface do usuário de status da conexão.
- A importação de configurações do cliente versão 8 foi aperfeiçoada.
- Os arquivos RDP que apontam para pontos de extremidade do RemoteApp agora podem ser importados para o Connection Center.
- Otimizações de exibição de retina para cenários de Área de Trabalho Remota de monitor único.
- Suporte para especificar o nível de interpolação de gráficos (que afeta o desfoque) quando nenhuma otimização de retina é usada.
- Suporte de 256 cores para habilitar a conectividade com o Windows 2000.
- Correção de recorte das bordas direita e inferior da tela ao conectar ao Windows 7, Windows Server 2008 R2 e versões anteriores.
- Copiar um arquivo local para o Outlook (executado em uma sessão remota) agora adiciona o arquivo como um anexo.
- Correção de um problema que estava causando lentidão em transferências de arquivos com base em quadro de colagem se os arquivos fossem originados de um compartilhamento de rede.
- Correção de um bug que estava fazendo com que o Excel (executado em uma sessão remota) parasse de responder ao salvar um arquivo em uma pasta redirecionada.
- Correção de um problema que fazia com que não houvesse espaço livre a ser informado para pastas redirecionadas.
- Correção de um bug que fazia com que miniaturas consumissem muito armazenamento em disco no macOS 10.14.
- Inclusão de suporte para a imposição de políticas de redirecionamento de dispositivo de Gateway de Área de Trabalho Remota.
- Correção de um problema que impedia o fechamento das janelas de sessão ao desconectar de uma conexão usando o Gateway de Área de Trabalho Remota.
- Se a NLA (Autenticação no Nível da Rede) não for imposta pelo servidor, agora você será roteado para a tela de logon se a senha tiver expirado.
- Correção de problemas de desempenho que surgem durante a transferência de grandes quantidades de dados pela rede.
- Correções de redirecionamento de cartão inteligente.
- Suporte para todos os valores possíveis nas configurações do arquivo RDP "EnableCredSspSupport" e "Nível de Autenticação" se a chave padrão do usuário ClientSettings.EnforceCredSSPSupport (no domínio com.microsoft.rdc.macos) for definida como 0.
- Suporte para a configuração do arquivo RDP do "Prompt para Credenciais sobre o Cliente" quando a NLA não for negociada.
- Suporte para logon baseado em cartão inteligente por meio de redirecionamento de cartão inteligente no prompt do Winlogon, quando a NLA não for negociada.
- Corrigido um problema que impedia o download de recursos de feed que têm espaços na URL.

## <a name="updates-for-version-1021"></a>Atualizações para a versão 10.2.1

*Data de publicação: 06/08/2018*

- Habilitação de conectividade para PCs associados ao AAD (Azure Active Directory). Para conectar um computador associado ao AAD, o nome de usuário deve estar em um dos seguintes formatos: "AzureAD\usuário" ou "AzureAD\user@domain".
- Solução de alguns bugs que afetavam o uso de cartões inteligentes em uma sessão remota.

## <a name="updates-for-version-1020"></a>Atualizações para a versão 10.2.0

*Data de publicação: 24/07/2018*

- Atualizações incorporadas para conformidade com GDPR.
- MicrosoftAccount\username@domain agora é aceito como um nome de usuário válido.
- O compartilhamento de área de transferência foi reformulado para ser mais rápido e dar suporte a mais formatos.
- Copiar e colar texto, imagens ou arquivos entre sessões agora ignora a área de transferência do computador local.
- Agora é possível conectar por meio de um servidor de Gateway de Área de Trabalho Remota com um certificado não confiável (se você aceitar os prompts de aviso).
- Aceleração de hardware Metal agora é usada (onde houver suporte) para acelerar o processamento e otimizar o uso da bateria.
- Ao usar a aceleração de hardware Metal, podemos tentar fazer alguma mágica para aumentar a nitidez dos gráficos da sessão.
- Eliminação de algumas instâncias em que as janelas poderiam travar após serem fechadas.
- Correção de bugs que estavam impedindo a inicialização de programas do RemoteApp em alguns cenários.
- Correção de um erro de sincronização de canal do Gateway de Área de Trabalho Remota que resultava em erros 0x204.
- Forma de cursor do mouse agora atualiza corretamente quando movida para fora de uma sessão ou da janela do RemoteApp.
- Correção de um bug de redirecionamento de pasta que estava causando perda de dados ao copiar e colar pastas.
- Correção de um problema de redirecionamento de pasta que causava relatórios incorretos de tamanhos de pastas.
- Correção de uma regressão que estava impedindo o logon em um computador associado ao AAD usando uma conta local.
- Correção de bugs que estavam cortando o conteúdo da janela da sessão.
- Inclusão de suporte para certificados de ponto de extremidade de Área de Trabalho Remota que contêm as chaves assimétricas de curva elíptica.
- Correção de um bug que estava impedindo o download de recursos gerenciados em alguns cenários.
- Correção de um problema de recorte com o Connection Center fixo.
- Correção das caixas de seleção na folha de propriedades de exibição para trabalhar melhor em conjunto.
- Bloqueio da taxa de proporção agora fica desabilitado quando a alteração da exibição dinâmica está em vigor.
- Solução de problemas de compatibilidade com a infraestrutura F5.
- Atualização do tratamento de senhas em branco para garantir que as mensagens corretas sejam mostradas no tempo de conexão.
- Correção de problemas de compatibilidade de rolagem do mouse com MapInfra Pro.
- Correção de alguns problemas de alinhamento no Connection Center durante a execução no Mojave.

## <a name="updates-for-version-1018"></a>Atualizações para a versão 10.1.8

*Data de publicação: 04/05/2018*

- Inclusão de suporte para alterar a resolução remota redimensionando a janela da sessão!
- Correção de cenários em que o download de feed de recurso remoto levaria tempo excessivamente longo.
- Resolução de um erro 0x207 que poderia ocorrer ao conectar a servidores não corrigidos com a atualização de correção Oracle de criptografia CredSSP (CVE-2018 0886).

## <a name="updates-for-version-1017"></a>Atualizações para a versão 10.1.7

*Data de publicação: 05/04/2018*

- Correções de segurança feitas para incorporar as atualizações de correção Oracle de criptografia CredSSP conforme descrito em CVE-2018-0886.
- Aperfeiçoamento da renderização do cursor do mouse e do ícone RemoteApp para tratar imagens incorretas relatadas.
- Solução de problemas em que janelas RemoteApp apareciam por trás do Connection Center.
- Correção de um problema que ocorria ao editar recursos locais após a importação da Área de Trabalho Remota 8.
- Agora é possível iniciar uma conexão ao pressionar ENTER em um bloco da área de trabalho.
- Quando você estiver no modo de exibição de tela inteira, CMD+M agora será mapeado corretamente para WIN+M.
- O Connection Center, as Preferências e Sobre o Windows agora respondem a CMD+M.
- Agora é possível começar a descobrir feeds pressionando ENTER na página **Adicionar Recursos Remotos**.
- Correção de um problema em que um novo feed de recursos remotos aparecia vazio no Connection Center até que você atualizasse.

## <a name="updates-for-version-1016"></a>Atualizações para a versão 10.1.6

*Data de publicação: 26/03/2018*

- Correção de um problema em que as janelas do RemoteApp se reordenavam sozinhas.
- Resolução de um bug que fazia com que algumas janelas de RemoteApp ficassem presas atrás da janela pai.
- Solução de um problema de deslocamento de ponteiro de mouse que afetava alguns programas RemoteApp.
- Correção de um problema em que iniciar uma nova conexão trazia o foco para uma sessão existente, em vez de abrir uma janela de nova sessão.
- Corrigimos um erro com uma mensagem de erro - você verá a mensagem correta agora se não foi possível encontrar o gateway.
- O atalho Sair (⌘ + Q) agora é consistentemente exibido na interface do usuário.
- Aperfeiçoamento da qualidade de imagem ao ampliar no modo "Ajustar à janela".
- Correção de uma regressão que causava o aparecimento de várias instâncias da pasta base na sessão remota.
- Atualização do ícone padrão para blocos da área de trabalho.
