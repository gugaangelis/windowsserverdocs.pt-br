---
title: Novidades do cliente para iOS
description: Saiba mais sobre as recentes alterações no cliente da Área de Trabalho Remota para iOS
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/03/2020
ms.localizationpriority: medium
ms.openlocfilehash: 69e33ef5f187672d948b6734a9ae7f68b54b7608
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854649"
---
# <a name="whats-new-in-the-ios-client"></a>Novidades do cliente para iOS

Atualizamos regularmente o [cliente da Área de Trabalho Remota para iOS](remote-desktop-ios.md), adicionando novos recursos e corrigindo problemas. Você encontrará as atualizações mais recentes nesta página.

>[!IMPORTANT]
>Temos o compromisso de tornar este aplicativo o melhor cliente da Área de Trabalho Remota para iOS e valorizamos seus comentários. Você pode relatar problemas em **Ajuda** > **Relatar um Problema**.

## <a name="updates-for-version-1005"></a>Atualizações da versão 10.0.5

*Data da publicação: 09/03/20*

Reunimos algumas correções de bug e atualizações de recurso para essa versão 10.0.5. Confira as novidades:

- Os arquivos RDP abertos agora são importados automaticamente (procure a alternância em Configurações gerais).
- Agora você pode abrir arquivos RDP baseados no iCloud que ainda não foram baixados no aplicativo Arquivos.
- Agora a sessão remota pode se estender embaixo do indicador Início em iPhones (procure a alternância nas configurações de Exibição).
- Adição de suporte para digitar caracteres compostos com vários pressionamentos de tecla, como é.
- Adição de suporte para o teclado flutuante na tela do iPad.
- Adição de suporte para ajustar as propriedades de câmeras redirecionadas em uma sessão remota.
- Correção de um bug no reconhecedor de gesto que fazia o cliente ficar sem resposta quando se conectava a uma sessão remota.
- Agora você pode entrar no modo Alternância de Aplicativos apenas deslizando o dedo para cima (exceto quando você está no modo Toque com a sessão estendida para a área do indicador Início).
- O indicador Início agora ficará oculto automaticamente quando conectado a uma sessão remota e será exibido novamente quando você tocar na tela.
- Adição de um atalho do teclado para acessar as configurações do aplicativo no Centro de Conexão (**Command + ,** ).
- Adição de um atalho do teclado para atualizar todos os workspaces no Centro de Conexão (**Command + R**).
- Conexão do atalho do teclado do sistema para Escape quando conectado a uma sessão remota (**Command + .** ).
- Correção de cenários em que o teclado na tela do Windows na sessão remota era muito pequeno.
- Implementação do foco de teclado automático em todo o Centro de Conexão para tornar a entrada de dados mais contínua.
- Pressionar **Enter** em uma solicitação de credenciais agora faz o prompt ser ignorado e o fluxo atual retomado.
- Correção de um cenário em que o cliente falhava ao pressionar Shift + Option + tecla de seta para a Esquerda, para Cima ou para Baixo.
- Correção de uma falha que ocorria ao remover um dispositivo SwiftPoint.
- Correção de outras falhas relatadas para nós pelos usuários desde a última versão.

Gostaríamos de agradecer todos que relataram bugs e trabalharam conosco para diagnosticar problemas.

## <a name="updates-for-version-1004"></a>Atualizações da versão 10.0.4

*Data da publicação: 03/02/20*

É hora de outra atualização! Queremos agradecer todos que relataram bugs e trabalharam conosco para diagnosticar problemas. Estas são as novidades dessa versão:

- Agora a interface do usuário de confirmação é mostrada ao excluir contas de usuário e gateways.
- A interface do usuário de pesquisa no Centro de Conexão foi ligeiramente refeita.
- A dica de nome de usuário, se existir, agora é mostrada na interface do usuário da solicitação de credenciais ao iniciar de um arquivo RDP ou URI.
- Correção de um problema em que o teclado na tela estendida se estendia embaixo do entalhe do iPhone.
- Correção de um bug em que teclados externos paravam de funcionar após serem desconectado e reconectados.
- Adição de suporte para a tecla Esc em teclados externos.
- Correção de um bug em que caracteres ingleses eram exibidos ao digitar caracteres chineses.
- Correção de um bug em que alguma entrada de chinês permanecia na sessão remota após a exclusão.
- Correção de outras falhas relatadas para nós pelos usuários desde a última versão.

Agradecemos todos os seus comentários enviados para nós por meio da App Store, de comentários no aplicativo e do email. Continuaremos nos concentrando em aprimorar esse aplicativo a cada versão.

## <a name="updates-for-version-1003"></a>Atualizações da versão 10.0.3

*Data da publicação: 16/01/20*

É 2020 e hora do nosso primeiro lançamento do ano, o que significa que há novos recursos e correções de bug. Veja o que está incluído nessa atualização:

- Suporte para iniciar conexões de arquivos RDP e URIs RDP.
- Os cabeçalhos do workspace agora podem ser recolhidos.
- Agora há suporte para a aplicação de zoom e panorâmica ao mesmo tempo no modo Ponteiro do Mouse.
- Um gesto de pressionar e segurar no modo Ponteiro do Mouse agora vai disparar um clique com o botão direito do mouse na sessão remota.
- Remoção do gesto de forçar toque para clicar com o botão direito do mouse no modo Ponteiro do Mouse.
- A tela de alternador na sessão agora dá suporte à desconexão, mesmo que nenhum aplicativo esteja conectado.
- Agora há suporte para light dismiss na tela de alternador na sessão.
- Computadores e aplicativos não são mais reordenados automaticamente na tela de alternador na sessão.
- Ampliação da área de teste de clique para o menu de reticências do modo de exibição de miniatura do computador.
- A página de configurações Dispositivos de Entrada agora contém um link para os dispositivos com suporte.
- Correção de um bug que fez a interface do usuário de permissões Bluetooth ser exibida repetidamente no momento da inicialização para alguns usuários.
- Correção de outras falhas relatadas para nós pelos usuários desde a última versão.

## <a name="updates-for-version-1002"></a>Atualizações da versão 10.0.2

*Data da publicação: 20/12/19*

Estamos trabalhando bastante para corrigir bugs e adicionar recursos úteis. Veja as novidades dessa versão:

- Suporte para entrada em japonês e chinês em teclados de hardware.
- A exibição de lista de computadores agora mostra o nome amigável da conta de usuário associada, se houver.
- A interface do usuário de permissões na experiência de primeira execução agora é renderizada corretamente no modo claro.
- Correção de uma falha que acontecia sempre que alguém pressionava as teclas Option e seta para cima ou para baixo ao mesmo tempo em um teclado de hardware.
- Atualização do layout de teclado na tela usado na interface do usuário de prompt de senha para facilitar a localização da tecla Barra invertida.
- Correção de outras falhas relatadas para nós pelos usuários desde a última versão.

## <a name="updates-for-version-1001"></a>Atualizações da versão 10.0.1

*Data da publicação: 15/12/19*

Veja as novidades dessa versão:

- Suporte para o serviço de Área de Trabalho Virtual do Windows.
- Atualização da interface do usuário do Centro de Conexão.
- Atualização da interface do usuário na sessão.

## <a name="updates-for-version-1000"></a>Atualizações para a versão 10.0.0

*Data da publicação: 13/12/2019*

Já faz mais de um ano desde a última atualização do Cliente de Área de Trabalho Remota para iOS. No entanto, estamos de volta com uma nova e empolgante atualização e haverá muito mais atualizações a serem feitas regularmente daqui em diante. Veja as novidades da versão 10.0.0:

- Suporte para o serviço de Área de Trabalho Virtual do Windows.
- Uma nova interface do usuário do centro de conexão.
- Uma nova interface do usuário na sessão que pode alternar entre computadores e aplicativos conectados.
- Novo layout para o teclado virtual auxiliar.
- Suporte ao teclado externo aprimorado.
- Suporte ao mouse Bluetooth SwiftPoint.
- Suporte ao redirecionamento de microfone.
- Suporte ao redirecionamento de armazenamento local.
- Suporte ao redirecionamento de câmera (disponível somente para Windows 10, versão 1809 ou posterior).
- Suporte para novos dispositivos iPhone e iPad.
- Suporte a temas claros e escuros.
- Controle se o seu telefone pode ser bloqueado quando conectado a um PC ou aplicativo remoto.
- Agora você pode recolher a barra de conexão em sessão pressionando e segurando o botão do logotipo da Área de Trabalho Remota.

## <a name="updates-for-version-8142"></a>Atualizações para a versão 8.1.42

*Data da publicação: 20/06/2018*

- Correção de bugs e melhorias de desempenho.

## <a name="updates-for-version-8141"></a>Atualizações para a versão 8.1.41

*Data da publicação: 28/03/2018*

- Atualizações para solucionar a correção de criptografia Oracle CredSSP descrita em CVE-2018-0886.

## <a name="how-to-report-issues"></a>Como relatar problemas

Temos o compromisso de tornar este aplicativo o melhor possível e valorizamos seus comentários. Você pode relatar problemas navegando em **Configurações** > **Relatar um Problema** no cliente.
