---
title: Novidades do cliente para macOS
description: Saiba mais sobre as recentes alterações no cliente da Área de Trabalho Remota para MAC
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/08/2020
ms.localizationpriority: medium
ms.openlocfilehash: e8769dd5784bcbac5c5384316564150801c1eb0e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946509"
---
# <a name="whats-new-in-the-macos-client"></a>Novidades do cliente para macOS

Atualizamos regularmente o [cliente da Área de Trabalho Remota para macOS](remote-desktop-mac.md), adicionando novos recursos e corrigindo problemas. Veja onde você encontrará as atualizações mais recentes.

Caso tenha problemas, entre em contato conosco a qualquer momento navegando em **Ajuda** > **Relatar um Problema**.

## <a name="updates-for-version-1039"></a>Atualizações da versão 10.3.9

*Data da publicação: 06/04/20*

Nessa versão, fizemos algumas alterações para aprimorar a interoperabilidade com o [serviço de Área de Trabalho Virtual do Windows](https://azure.microsoft.com/services/virtual-desktop/). Além disso, incluímos as seguintes atualizações:

- Control+Option+Delete agora dispara a sequência Ctrl+Alt+Del (anteriormente era preciso pressionar a tecla Fn).
- Correção do esquema de cores de notificação do modo de teclado para o modo claro.
- Resolução de cenários em que as conexões iniciadas usando a propriedade de arquivo RDP GatewayAccessToken não funcionavam.

>[!NOTE]
>Esta é a última versão que será compatível com o macOS 10.12.

## <a name="updates-for-version-1038"></a>Atualizações da versão 10.3.8

*Data da publicação: 12/02/20*

É hora de nossa primeira versão de 2020!

Com essa atualização, você pode alternar entre os modos Scancode (Ctrl+Command+K) e Unicode (Ctrl+Command+U) ao inserir a entrada de teclado. O modo Unicode permite que os caracteres estendidos sejam digitados usando a tecla Option em um teclado Mac. Por exemplo, em um teclado Mac dos EUA, a Option+2 inserirá o símbolo de marca comercial (&trade;). Você também pode inserir caracteres com acento no modo Unicode. Por exemplo, em um teclado Mac dos EUA, pressionar Option+E e a tecla "A" ao mesmo tempo inserirá o caractere "á" na sessão remota.

Outras atualizações dessa versão incluem:

- Limpeza da experiência e da interface do usuário de atualização do workspace.
- Resolução de um problema de redirecionamento de cartão inteligente que fazia a sessão remota parar de responder na tela de entrada quando a mensagem "Verificando Status" era exibida.
- Redução do tempo para criar arquivos temporários usados para copiar e colar arquivos baseados na área de transferência.
- Arquivos temporários usados para copiar e colar arquivos de área de transferência agora são excluídos automaticamente quando você sai do aplicativo, em vez de depender do macOS para excluí-los.
- As ações de indicador de computador agora são renderizadas no canto superior direito das miniaturas.
- Realização de correções para resolver problemas relatados pela telemetria de falhas.

## <a name="updates-for-version-1037"></a>Atualizações para a versão 10.3.7

*Data da publicação: 06/01/20*

Em nossa atualização final do ano, ajustamos alguns códigos e corrigimos os seguintes comportamentos:

- A cópia desses itens da sessão remota para um compartilhamento de rede ou uma unidade USB não cria mais arquivos vazios.
- A especificação de uma senha vazia em uma conta de usuário não gera mais um aviso de certificado duplo.

## <a name="updates-for-version-1036"></a>Atualizações para a versão 10.3.6

*Data da publicação: 06/01/20*

Nesta versão, abordamos um problema que criava arquivos de comprimento zero sempre que você copiava uma pasta da sessão remota para o computador local usando a opção Copiar e colar arquivo.

## <a name="updates-for-version-1035"></a>Atualizações para a versão 10.3.5

*Data da publicação: 06/01/20*

Fizemos esta atualização com a ajuda de todos que relataram problemas. Nesta versão, fizemos as seguintes alterações:

- As pastas redirecionadas agora podem ser marcadas como somente leitura para impedir que seu conteúdo seja alterado na sessão remota.
- Abordamos um erro do tipo 0x607 que apareceu durante a conexão usando cenários de Gateway de Área de Trabalho Remota sobre HTTPS.
- Os casos em que os usuários eram solicitados a fornecer credenciais duas vezes foram corrigidos.
- Os casos em que os usuários recebiam o prompt de aviso do certificado duas vezes foram corrigidos.
- Foi realizada a adição de heurística para melhorar a rolagem baseada em trackpad.
- O cliente não mostrará mais o grupo “Áreas de trabalho salvas” se não houver grupos criados pelo usuário.
- A interface do usuário foi atualizada para os blocos na exibição do PC.
- Foram feitas correções para tratar de falhas enviadas a nós por meio da telemetria do aplicativo.

> [!NOTE]
> Agora aceitamos comentários nesta versão para o cliente Mac somente por meio do [UserVoice](https://remotedesktop.uservoice.com/forums/287834-remote-desktop-for-mac).

## <a name="updates-for-version-1034"></a>Atualizações para a versão 10.3.4

*Data da publicação: 18/11/2019*

Estamos trabalhando intensamente ouvindo seus comentários e reunimos uma coleção de correções de bugs e atualizações de recursos.

- Ao se conectar por meio de um gateway de área de trabalho remota com autenticação multifator, a conexão do gateway será mantida aberta para evitar o surgimento de múltiplos prompts de MFA.
- Toda a interface do usuário do cliente agora é totalmente acessível pelo teclado, com suporte a VoiceOver.
- Os arquivos copiados para a área de transferência na sessão remota agora são transferidos somente ao colar para o computador local.
- As URLs copiadas para a área de transferência na sessão remota agora são coladas corretamente no computador local.
- O fator de escala de comunicação remota para dar suporte a telas Retina agora está disponível para cenários com vários monitores.
- Foi abordado um problema de compatibilidade com servidores de Área de Trabalho Remota baseados em FreeRDP que estavam causando problemas de conectividade em cenários de redirecionamento.
- Resolvida a questão de compatibilidade de redirecionamento de cartão inteligente com versões futuras do Windows 10.
- Resolvido um problema específico do macOS 10.15 em que o espaço disponível incorreto era relatado para pastas redirecionadas.
- As conexões de PC publicadas são representadas com um novo ícone na guia Workspaces.
- "Feeds" agora são chamados de "Workspaces" e "Desktops" agora são chamados de "PCs".
- Corrigidas as inconsistências e bugs na manipulação de conta de usuário na interface do usuário de preferências.
- Muitas correções de bugs para fazer com que as coisas sejam executadas de modo mais estável e confiável.

## <a name="updates-for-version-1033"></a>Atualizações para a versão 10.3.3

*Data da publicação: 18/11/2019*

Reunimos uma atualização de recurso e corrigimos bugs para a versão 10.3.3.

Primeiro, adicionamos os padrões do usuário para desabilitar o cartão inteligente, a área de transferência, o microfone, a câmera e o redirecionamento de pasta:

- ClientSettings.DisableSmartcardRedirection
- ClientSettings.DisableClipboardRedirection
- ClientSettings.DisableMicrophoneRedirection
- ClientSettings.DisableCameraRedirection
- ClientSettings.DisableFolderRedirection

Em seguida, as correções de bugs:

- Resolvido um problema que estava fazendo com que redimensionamentos de janela de sessão programática não fossem detectados.
- Corrigido um problema em que o conteúdo da janela da sessão parecia pequeno ao se conectar no modo de janela (com a exibição dinâmica habilitada).
- Foi abordada uma cintilação inicial que ocorria ao se conectar a uma sessão no modo de janela com a exibição dinâmica habilitada.
- Corrigidas as pinturas de gráficos incorretas que ocorriam quando conectado ao Windows 7 depois de alternar o ajuste à janela com a exibição dinâmica habilitada.
- Corrigido um bug que fazia com que um nome de dispositivo incorreto fosse enviado para a sessão remota (interrompendo o licenciamento em alguns aplicativos de terceiros).
- Resolvido um problema em que as janelas do aplicativo remoto ocupavam um monitor inteiro quando maximizadas.
- Resolvido um problema em que a interface do usuário de permissões de acesso aparecia sob as janelas locais.
- Realizada a limpeza do código de desligamento para garantir que o cliente seja fechado de modo mais confiável.

## <a name="updates-for-version-1032"></a>Atualizações para a versão 10.3.2

*Data da publicação: 18/11/2019*

Nesta versão, corrigimos um bug que colocava a exibição em baixa resolução durante a conexão com uma sessão

## <a name="updates-for-version-1031"></a>Atualizações para a versão 10.3.1

*Data da publicação: 18/11/2019*

Reunimos algumas correções para abordar as regressões que acabaram fazendo parte da versão 10.3.0.

- Problemas de conectividade abordados com servidores de gateway de área de trabalho remota que estavam usando chaves assimétricas de 4.096 bits.
- Corrigido um bug que fazia com que o cliente deixasse de responder aleatoriamente ao baixar recursos de feed.
- Corrigido um bug que fazia com que o cliente falhasse ao abrir.
- Corrigido um bug que fazia com que o cliente falhasse ao importar conexões da Área de Trabalho Remota, versão 8.

## <a name="updates-for-version-1030"></a>Atualizações para a versão 10.3.0

*Data da publicação: 27/08/2019*

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
- Um ícone de aplicativo novo, simplificado e limpo.

## <a name="updates-for-version-10213"></a>Atualizações para a versão 10.2.13

*Data da publicação: 8/5/2019*

- Correção de um travamento que ocorria ao conectar por meio de um Gateway de Área de Trabalho Remota.
- Inclusão de um aviso de privacidade para a caixa de diálogo "Adicionar Feed".

## <a name="updates-for-version-10212"></a>Atualizações para a versão 10.2.12

*Data da publicação: 16/4/2019*

- Resolução de desconexões aleatórias (com código de erro 0x904) que ocorriam ao conectar por meio de um Gateway de Área de Trabalho Remota.
- Correção de um bug que esvaziava a lista de resoluções nas preferências após a instalação.
- Correção de um bug que causava falha do computador cliente se determinadas resoluções fossem adicionadas à lista de resoluções.
- Solução de um loop de prompt de autenticação ADAL ao conectar para implantações de Área de Trabalho Virtual do Windows.

## <a name="updates-for-version-10210"></a>Atualizações para a versão 10.2.10

*Data da publicação: 30/3/2019*

- Nesta versão, abordamos a instabilidade causada pela recente atualização do macOS 10.14.4. Também corrigimos imagens incorretas que apareciam ao decodificar dados de codec de AVC codificados por um servidor usando o hardware da NVIDIA.

## <a name="updates-for-version-1029"></a>Atualizações para a versão 10.2.9

*Data da publicação: 6/3/2019*

- Nesta versão, corrigimos um problema de conectividade de gateway de área de trabalho remota que pode ocorrer durante o redirecionamento do servidor.
- Também resolvemos uma regressão de gateway de área de trabalho remota causada pela atualização 10.2.8.

## <a name="updates-for-version-1028"></a>Atualizações para a versão 10.2.8

*Data da publicação: 1/3/2019*

- Foram resolvidos problemas de conectividade que surgiram ao usar um Gateway de Área de Trabalho Remota.
- Foram corrigidos avisos de certificado incorreto que eram exibidos ao conectar.
- Foram resolvidos alguns casos em que a barra de menus e encaixe ocultava desnecessariamente ao iniciar aplicativos remotos.
- Foi reformulado o código de redirecionamento de área de transferência para resolver falhas e travamentos que têm incomodado alguns usuários.
- Foi corrigido um bug que causava desnecessariamente a rolagem do Connection Center o ao iniciar uma conexão.

## <a name="updates-for-version-1027"></a>Atualizações para a versão 10.2.7

*Data da publicação: 6/2/2019*

- Nesta versão, abordamos imagens incorretas em gráficos (causados por um bug de codificação do servidor) que apareciam ao usar o modo de AVC444.

## <a name="updates-for-version-1026"></a>Atualizações para a versão 10.2.6

*Data da publicação: 28/1/2019*

- Foi adicionado suporte para o codec AVC (420 e 444), disponível ao conectar às versões atuais do Windows 10.
- No modo Ajustar à janela, uma atualização de janela agora ocorre imediatamente após um redimensionamento para garantir que o conteúdo seja renderizado no nível de interpolação correto.
- Correção de um bug de layout que causava sobreposição de cabeçalhos de feeds para alguns usuários.
- Limpeza da interface do usuário Preferências do aplicativo.
- Aperfeiçoamento da interface do usuário Adicionar/Editar da área de trabalho.
- Realização de vários ajustes de adaptação e conclusão para as exibições de lista e de bloco do Connection Center para desktops e feeds.

>[!NOTE]
>Há um bug no macOS 10.14.0 e 10.14.1 que pode fazer com que a pasta ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (aninhada profundamente na pasta ~/Library) consuma uma grande quantidade de espaço em disco. Para resolver esse problema, exclua o conteúdo da pasta e atualize para o macOS 10.14.2. Observe que um efeito colateral de excluir o conteúdo da pasta é a posterior exclusão das imagens de instantâneo atribuídas a indicadores. Essas imagens serão regeneradas ao reconectar ao PC remoto.

## <a name="updates-for-version-1024"></a>Atualizações para a versão 10.2.4

*Data da publicação: 18/12/2018*

- Inclusão de suporte ao modo escuro para macOS Mojave 10.14.
- Uma opção para importar da Área de Trabalho Remota 8 da Microsoft agora aparece no Connection Center se ela estiver vazia.
- Solução da compatibilidade de redirecionamento de pasta com alguns aplicativos empresariais de terceiros.
- Resolução de problemas em que os usuários estavam obtendo um 0x30000069 Erro de Gateway de Área de Trabalho Remota devido à problemas de fallback do protocolo de segurança.
- Correção de problemas de renderização progressiva enfrentados por alguns usuários ao ajustar para o modo de janela.
- Correção de um bug que impedia que a cópia e colagem de arquivo copiasse a versão mais recente de um arquivo.
- Aprimoramento da rolagem com base no mouse para deltas de rolagem pequena.

## <a name="updates-for-version-1023"></a>Atualizações para a versão 10.2.3

*Data da publicação: 06/11/2018*

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

*Data da publicação: 09/10/2018*

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

*Data da publicação: 06/08/2018*

- Habilitação de conectividade para PCs associados ao AAD (Azure Active Directory). Para conectar um computador associado ao AAD, o nome de usuário deve estar em um dos seguintes formatos: "AzureAD\user" ou "AzureAD\user@domain".
- Solução de alguns bugs que afetavam o uso de cartões inteligentes em uma sessão remota.

## <a name="updates-for-version-1020"></a>Atualizações para a versão 10.2.0

*Data da publicação: 24/07/2018*

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
- Correção das caixas de verificação na guia Exibir da janela Adicionar uma Área de Trabalho para trabalhar melhor em conjunto.
- Bloqueio da taxa de proporção agora fica desabilitado quando a alteração da exibição dinâmica está em vigor.
- Solução de problemas de compatibilidade com a infraestrutura F5.
- Atualização do tratamento de senhas em branco para garantir que as mensagens corretas sejam mostradas no tempo de conexão.
- Correção de problemas de compatibilidade de rolagem do mouse com MapInfra Pro.
- Correção de alguns problemas de alinhamento no Connection Center durante a execução no Mojave.

## <a name="updates-for-version-1018"></a>Atualizações para a versão 10.1.8

*Data da publicação: 04/05/2018*

- Inclusão de suporte para alterar a resolução remota redimensionando a janela da sessão!
- Correção de cenários em que o download de feed de recurso remoto levaria tempo excessivamente longo.
- Resolução de um erro 0x207 que poderia ocorrer ao conectar a servidores não corrigidos com a atualização de correção Oracle de criptografia CredSSP (CVE-2018 0886).

## <a name="updates-for-version-1017"></a>Atualizações para a versão 10.1.7

*Data da publicação: 05/04/2018*

- Correções de segurança feitas para incorporar as atualizações de correção Oracle de criptografia CredSSP conforme descrito em CVE-2018-0886.
- Aperfeiçoamento da renderização do cursor do mouse e do ícone RemoteApp para tratar imagens incorretas relatadas.
- Solução de problemas em que janelas RemoteApp apareciam por trás do Connection Center.
- Correção de um problema que ocorria ao editar recursos locais após a importação da Área de Trabalho Remota 8.
- Agora é possível iniciar uma conexão ao pressionar ENTER em um bloco da área de trabalho.
- Quando você estiver no modo de exibição de tela inteira, CMD+M agora será mapeado corretamente para WIN+M.
- O Connection Center, as Preferências e Sobre o Windows agora respondem a CMD+M.
- Agora é possível começar a descobrir feeds pressionando ENTER na página **Adicionar Recursos Remotos*.
- Correção de um problema em que um novo feed de recursos remotos aparecia vazio no Connection Center até que você atualizasse.

## <a name="updates-for-version-1016"></a>Atualizações para a versão 10.1.6

*Data da publicação: 26/03/2018*

- Correção de um problema em que as janelas do RemoteApp se reordenavam sozinhas.
- Resolução de um bug que fazia com que algumas janelas de RemoteApp ficassem presas atrás da janela pai.
- Solução de um problema de deslocamento de ponteiro de mouse que afetava alguns programas RemoteApp.
- Correção de um problema em que iniciar uma nova conexão trazia o foco para uma sessão existente, em vez de abrir uma janela de nova sessão.
- Corrigimos um erro com uma mensagem de erro - você verá a mensagem correta agora se não foi possível encontrar o gateway.
- O atalho Sair (⌘ + Q) agora é consistentemente exibido na interface do usuário.
- Aperfeiçoamento da qualidade de imagem ao ampliar no modo "Ajustar à janela".
- Correção de uma regressão que causava o aparecimento de várias instâncias da pasta base na sessão remota.
- Atualização do ícone padrão para blocos da área de trabalho.
