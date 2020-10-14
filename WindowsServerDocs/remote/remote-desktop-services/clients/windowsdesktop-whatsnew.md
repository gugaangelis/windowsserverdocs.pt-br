---
title: Novidades no cliente da Área de Trabalho do Windows
description: Saiba mais sobre as recentes alterações no cliente da Área de Trabalho Remota para a Área de Trabalho do Windows
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 10/09/2020
ms.localizationpriority: medium
ms.openlocfilehash: 96a307b06b0a29cfb66ba52ca5d8ea031bd979e7
ms.sourcegitcommit: 6931830a70c5849d8f884cdc7bd4f5afc1a00cce
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91955757"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novidades no cliente da Área de Trabalho do Windows

É possível encontrar informações mais detalhadas sobre o cliente da Área de Trabalho do Windows em [Introdução ao cliente da Área de Trabalho do Windows](windowsdesktop.md). Você encontrará as atualizações mais recentes no cliente neste artigo.

## <a name="supported-client-versions"></a>Versões do cliente com suporte

O cliente pode ser configurado para diferentes [grupos de usuários](windowsdesktop-admin.md#configure-user-groups). A tabela a seguir lista as versões atuais disponíveis para cada grupo de usuários:

|Grupo de usuários |Última versão  |Versão mínima com suporte |
|-----------|----------------|--------------------------|
|Público     |1.2.1364        |1.2.945                   |
|Participante do Programa Windows Insider    |1.2.1364        |1.2.945                   |

## <a name="updates-for-version-121364"></a>Atualizações para a versão 1.2.1364

*Data da publicação: 22/09/2020*

Download: Windows [64 bits](https://go.microsoft.com/fwlink/?linkid=2139369), [Windows 32 bits](https://go.microsoft.com/fwlink/?linkid=2139456), [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2139370)

- Correção de um problema em que o SSO (logon único) não funcionava no Windows 7.
- Corrigida a falha de conexão que acontecia ao chamar ou ingressar em uma chamada de equipe enquanto outro aplicativo tinha um fluxo de áudio aberto em modo exclusivo e quando a otimização de mídia para o Teams estava habilitada.
- Corrigida uma falha ao enumerar dispositivos de áudio ou vídeo no Teams com a otimização de mídia para o Teams habilitada.
- Adicionado um link "Precisa de ajuda com as configurações?" para a página de configurações da área de trabalho.
- Corrigido um problema com o botão "Assinar" que acontecia ao usar temas escuros de alto contraste.
- É permitido um limite de até 20 credenciais por aplicativo.

## <a name="updates-for-version-121275"></a>Atualizações para a versão 1.2.1275

*Data da publicação: 25/08/2020*

Download: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4FpYR), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4FpYS), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4Fg3H)

- Funcionalidade adicionada para detectar automaticamente nuvens soberanas da identidade do usuário.
- Funcionalidade adicionada para habilitar assinaturas de URL personalizadas para todos os usuários.
- Corrigido um problema com a fixação de aplicativo na barra de tarefas do feed.
- Correção de uma falha ao assinar com URL.
- Experiência aprimorada ao arrastar janelas de aplicativos remotos com toque ou caneta.
- Correção de um problema relacionado à localização.

## <a name="updates-for-version-121186"></a>Atualizações para a versão 1.2.1186

*Data da publicação: 28/07/2020*

- Agora você pode se inscrever em workspaces com várias contas de usuário usando a opção de menu de estouro ( **...** ) na barra de comandos na parte superior do cliente. Para diferenciar workspaces, os títulos de workspace agora incluem o nome de usuário, assim como todos os títulos de atalhos de aplicativos.
- Foram adicionadas informações adicionais às mensagens de erro de assinatura para aprimorar a solução de problemas.
- O estado recolhido/expandido dos workspaces agora é preservado durante uma atualização.
- Foi adicionado um botão **Enviar Diagnóstico e Fechar** à caixa de diálogo **Informações de Conexão**.
- Corrigido um problema com as teclas CTRL + SHIFT em sessões remotas.

## <a name="updates-for-version-121104"></a>Atualizações para a versão 1.2.1104

*Data da publicação: 23/06/2020*

- Foi atualizada a lógica de descoberta automática para a opção **Assinar** para dar suporte à versão integrada do Azure Resource Manager da Área de Trabalho Virtual do Windows. Os clientes com apenas recursos da Área de Trabalho Virtual do Windows não precisam mais dar consentimento para a Área de Trabalho Virtual do Windows (clássica).
- Suporte aprimorado para dispositivos de alto DPI com fator de escala de até 400%.
- Correção de um problema em que a caixa de diálogo Desconectar não aparecia.
- Correção de um problema em que as dicas de ferramentas da barra de comandos permaneciam visíveis por mais tempo do que o esperado.
- Correção de uma falha quando você tentava assinar imediatamente após uma atualização.
- Correção de uma falha de análise incorreta de data e hora em alguns idiomas.

## <a name="updates-for-version-121026"></a>Atualizações para a versão 1.2.1026

*Data da publicação: 27/05/2020*

- Agora, ao assinar, é possível escolher a conta em vez de digitar o endereço de email.
- Foi adicionada uma nova opção **Assinar com URL** que permite especificar a URL do workspace que você está assinando ou aproveitar, quando disponível, a [descoberta de email](../rds-email-discovery.md) nos casos em que não conseguimos localizar seus recursos automaticamente. Isso é parecido com o processo de assinatura em outros clientes da Área de Trabalho Remota. Isso pode ser usado para assinar diretamente os workspaces da Área de Trabalho Virtual do Windows.
- Foi adicionado suporte para assinar um workspace usando um novo [esquema de URI](remote-desktop-uri.md) que pode ser enviado em um email para os usuários ou adicionado a um site de suporte.
- Foi adicionada uma nova caixa de diálogo **Informações da conexão** que fornece detalhes de cliente, rede e servidor para sessões de área de trabalho e de aplicativo. Você pode acessar a caixa de diálogo na barra de conexão no modo de tela inteira ou no menu de sistema quando em janela.
- As sessões de área de trabalho iniciadas no modo de janela agora sempre maximizam em vez de ficar em tela inteira ao maximizar a janela. Use a opção **Tela inteira** no menu de sistema para entrar em tela inteira.
- O prompt Cancelar assinatura agora exibe um ícone de aviso e mostra os nomes do workspace como uma lista com marcadores.
- Foi adicionada a seção Detalhes a mais caixas de diálogo de erro para ajudar a diagnosticar problemas.
- Foi adicionado um carimbo de data/hora à seção Detalhes das caixas de diálogo de erro.
- Corrigido um problema em que a configuração **desktop size id** do arquivo do RDP não funcionava corretamente.
- Corrigido um problema em que a configuração de exibição **Atualizar a resolução ao redimensionar** não se aplicava após iniciar a sessão.
- Problemas de localização corrigidos no painel de configurações da área de trabalho.
- Foi corrigido o tamanho da caixa de foco ao alternar entre controles no painel de configurações da área de trabalho.
- Corrigido um problema que fazia com que os nomes de recursos ficassem difíceis de ler no modo de alto contraste.
- Corrigido um problema que fazia com que a notificação de atualização na central de ações fosse mostrada mais de uma vez por dia.

## <a name="updates-for-version-12945"></a>Atualizações para a versão 1.2.945

*Data da publicação: 28/04/2020*

- Adição de novas opções de configurações de exibição para conexões da área de trabalho disponíveis ao clicar com o botão direito do mouse em um ícone de área de trabalho no Centro de Conexão.
  - Agora há três opções de configuração de exibição: **Todas as exibições**, **Exibição única** e **Exibições selecionadas**.
  - Agora mostramos apenas configurações disponíveis quando uma configuração de exibição é selecionada.
  - No modo Exibição selecionada, uma nova opção **Maximizar para exibições atuais** permite que você altere dinamicamente as exibições usadas para a sessão sem se reconectar. Quando habilitada, a maximização da sessão a faz entrar em modo de tela inteira em todas as exibições abrangidas pela janela de sessão.
  - Adicionamos uma nova **Exibição única quando está em modo de janela** para todas as exibições e modos de exibição selecionados. A opção alterna sua sessão automaticamente para uma exibição única quando você sai do modo de tela inteira e retorna automaticamente para várias exibições quando você maximiza a janela.
- Adicionamos um novo grupo **Configurações de exibição** ao menu de sistema que é exibido quando você clica com o botão direito do mouse na barra de título de uma sessão de área de trabalho do modo de janela. Isso permitirá que você altere algumas configurações dinamicamente durante uma sessão. Por exemplo, você pode alterar as novas configurações de **Modo de exibição única quando está em modo de janela** e **Maximizar para exibições atuais**.
- Quando você sair da tela inteira, a janela da sessão voltará ao local original de quando você entrou em tela inteira pela primeira vez.
- A atualização em segundo plano para Workspaces foi alterada para ocorrer a cada quatro horas, em vez de a cada hora. Agora ocorre uma atualização automaticamente ao iniciar o cliente.
- A redefinição dos dados do usuário na página Sobre agora realiza o redirecionamento para o Centro de Conexão quando concluída, em vez de fechar o cliente.
- Os itens no menu de sistema para conexões de área de trabalho foram reordenados e o tópico de Ajuda agora aponta para a documentação do cliente.
- Resolvemos alguns problemas de acessibilidade com navegação por guias e leitores de tela.
- Corrigido um problema em que a caixa de diálogo de autenticação do Azure Active Directory aparecia atrás da janela da sessão.
- Correção de um problema de cintilação e redução ao arrastar uma janela de sessão da área de trabalho entre exibições de diferentes fatores de escala.
- Correção de um erro que ocorreu ao redirecionar câmeras.
- Correção de várias falhas para aprimorar a confiabilidade.

## <a name="updates-for-version-12790"></a>Atualizações para a versão 1.2.790

*Data da publicação: 24/03/2020*

- A ação "Renovar" para workspaces foi renomeada para "Atualizar" para fins de consistência com outros clientes da Área de Trabalho Remota.
- Agora você pode atualizar um workspace diretamente do respectivo menu de contexto.
- A atualização manual de um workspace agora verifica se todo o conteúdo local foi atualizado.
- Agora você pode redefinir os dados do usuário do cliente da página Sobre sem precisar desinstalar o aplicativo.
- Você também pode redefinir os dados de usuário do cliente usando msrdcw.exe /reset com um parâmetro opcional /f para ignorar o prompt.
- Agora, procuramos automaticamente uma atualização do cliente ao navegar até a página Sobre.
- A cor dos botões foi atualizada para fins de consistência.

## <a name="updates-for-version-12675"></a>Atualizações para a versão 1.2.675

*Data da publicação: 25/02/2020*

- Agora as conexões com a Área de Trabalho Virtual do Windows serão bloqueadas se o arquivo RDP não tiver a assinatura ou se uma das propriedades signscope tiver sido modificada.
- Quando um workspace está vazio ou foi removido, o Centro de Conexão não parece mais estar vazio.
- Adição da ID da atividade e do código de erro em mensagens de desconexão para aprimorar a solução de problemas. Você pode copiar a mensagem da caixa de diálogo com **CTRL+C**.
- Correção de um problema que fazia as configurações de conexão da área de trabalho não detectarem exibições.
- As atualizações do cliente pararam de reiniciar automaticamente o PC.
- Ícones sem janela não devem mais aparecer na barra de tarefas.

## <a name="updates-for-version-12605"></a>Atualizações para a versão 1.2.605

*Data da publicação: 29/01/2020*

- Agora você pode selecionar quais exibições usará para as conexões de área de trabalho. Para alterar essa configuração, clique com o botão direito do mouse no ícone da conexão de área de trabalho e selecione **Configurações**.
- Correção de um problema em que as configurações de conexão não exibiam os fatores de escala disponíveis corretos.
- Correção de um problema em que o Narrador não podia ler a caixa de diálogo mostrada enquanto a conexão era iniciada.
- Correção de um problema em que o nome de usuário incorreto exibido quando os nomes Azure Active Directory e Active Directory não eram correspondentes.
- Correção de um problema que fazia o cliente parar de responder ao iniciar uma conexão sem estar conectado a uma rede.
- Correção de um problema que fazia com que o cliente parasse de responder ao anexar um headset.

## <a name="updates-for-version-12535"></a>Atualizações para a versão 1.2.535

*Data da publicação: 04/12/2019*

- Agora, você pode acessar informações sobre atualizações diretamente do botão mais opções na barra de comandos na parte superior do cliente.
- Agora, você pode fornecer comentários na barra de comandos do cliente.
- A opção de comentários agora será mostrada apenas se o Hub de Comentários estiver disponível.
- Garantiu que a notificação de atualização não é mostrada quando as notificações estão desabilitadas por meio da política.
- Corrigido um problema que impedia a inicialização de alguns arquivos RDP.
- Corrigida uma falha na inicialização do cliente causada pela corrupção de algumas configurações persistentes.

## <a name="updates-for-version-12431"></a>Atualizações da versão 1.2.431

*Data da publicação: 12/11/2019*

- As versões de 32 bits e ARM64 do cliente já estão disponíveis!
- Agora o cliente salva todas as alterações feitas na barra de conexão (como a posição, o tamanho e o estado de fixação) e aplica essas alterações em todas as sessões iniciadas.
- Informações de gateway e caixas de diálogo de status de conexão atualizadas.
- Resolvido um problema que fazia com que duas credenciais solicitassem acesso ao mesmo tempo ao tentar se conectar depois de expirado o token do Azure Active Directory.
- Agora, no Windows 7, os usuários que tinham suas credenciais salvas são devidamente solicitados a fornecê-las novamente quando o servidor as desautoriza.
- Agora o prompt do Azure Active Directory aparece na frente da janela de conexão ao reconectar.
- Agora os itens fixados na barra de tarefas são atualizados durante uma atualização do feed.
- Aprimorada a rolagem no Centro de Conexão ao usar o toque.
- Removida a linha vazia do menu suspenso de resolução.
- Entradas desnecessárias removidas do Gerenciador de Credenciais do Windows.
- Agora as sessões de desktop são dimensionadas corretamente ao sair do modo de tela inteira.
- Agora a caixa de diálogo de desconexão do RemoteApp aparece no primeiro plano quando você retoma a sessão depois de entrar no modo de suspensão.
- Resolvidos problemas de acessibilidade como a navegação por teclado.

## <a name="updates-for-version-12247"></a>Atualizações para a versão 1.2.247

*Data da publicação: 17/09/2019*

- Melhoria nos idiomas de fallback para a versão localizada. (Por exemplo, FR-CA será exibido corretamente em francês, em vez de inglês.)
- Ao remover uma assinatura, agora o cliente remove de maneira adequada as credenciais salvas do Gerenciador de Credenciais.
- O processo de atualização do cliente agora é autônomo depois de iniciado e o cliente será reiniciado após a conclusão.
- Agora o cliente pode ser usado no Windows 10 no modo S.
- Correção de um problema que fez o processo de atualização falhar para usuários com um espaço no nome de usuário.
- Correção de uma falha que ocorreu ao autenticar durante uma conexão.
- Correção de uma falha que ocorreu ao fechar o cliente.
