---
title: Novidades no cliente da Área de Trabalho do Windows
description: Saiba mais sobre as recentes alterações no cliente da Área de Trabalho Remota para a Área de Trabalho do Windows
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 01/29/2020
ms.localizationpriority: medium
ms.openlocfilehash: b2d5215c7089ce1aadbeae68890dca1a0ae1c294
ms.sourcegitcommit: 9077469e372d2aafcad890cbc4e4a24c58a3838c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76889440"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novidades no cliente da Área de Trabalho do Windows

É possível encontrar informações mais detalhadas sobre o cliente da Área de Trabalho do Windows em [Introdução ao cliente da Área de Trabalho do Windows](windowsdesktop.md). Você encontrará as atualizações mais recentes no cliente abaixo.

## <a name="latest-client-versions"></a>Versões do cliente mais recentes

O cliente pode ser configurado para diferentes [grupos de usuários](windowsdesktop-admin.md#configure-user-groups). A tabela a seguir lista as versões atuais disponíveis para cada grupo de usuários:

|Grupo de usuários |Versão  |
|-----------|---------|
|Público     |1.2.605  |
|Participante do Programa Windows Insider    |1.2.605  |

## <a name="updates-for-version-12605"></a>Atualizações para a versão 1.2.605

*Data da publicação: 29/01/2020*

Download: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oHrD), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oJZs), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oXhD)

- Agora você pode selecionar quais exibições usará para as conexões de área de trabalho. Para alterar essa configuração, clique com o botão direito do mouse no ícone da conexão de área de trabalho e selecione **Configurações**.
- Correção de um problema em que as configurações de conexão não exibiam os fatores de escala disponíveis corretos.
- Correção de um problema em que o Narrador não podia ler a caixa de diálogo mostrada enquanto a conexão era iniciada.
- Correção de um problema em que o nome de usuário incorreto exibido quando os nomes Azure Active Directory e Active Directory não eram correspondentes.
- Correção de um problema que fazia o cliente parar de responder ao iniciar uma conexão sem estar conectado a uma rede.
- Correção de um problema que fazia com que o cliente parasse de responder ao anexar um headset.

## <a name="updates-for-version-12535"></a>Atualizações para a versão 1.2.535

*Data da publicação: 04/12/2019*

Download: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jH), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jL), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k27O)

- Agora, você pode acessar informações sobre atualizações diretamente do botão mais opções na barra de comandos na parte superior do cliente.
- Agora, você pode fornecer comentários na barra de comandos do cliente.
- A opção de comentários agora será mostrada apenas se o Hub de Comentários estiver disponível.
- Garantiu que a notificação de atualização não é mostrada quando as notificações estão desabilitadas por meio da política.
- Corrigido um problema que impedia a inicialização de alguns arquivos RDP.
- Corrigida uma falha na inicialização do cliente causada pela corrupção de algumas configurações persistentes.

## <a name="updates-for-version-12431"></a>Atualizações da versão 1.2.431

*Data da publicação: 12/11/2019*

Download: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48kow), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48koA), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48zYj)

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

Download: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3LkSa)

- Melhoria nos idiomas de fallback para a versão localizada. (Por exemplo, FR-CA será exibido corretamente em francês, em vez de inglês.)
- Ao remover uma assinatura, agora o cliente remove de maneira adequada as credenciais salvas do Gerenciador de Credenciais.
- O processo de atualização do cliente agora é autônomo depois de iniciado e o cliente será reiniciado após a conclusão.
- Agora o cliente pode ser usado no Windows 10 no modo S.
- Correção de um problema que fez o processo de atualização falhar para usuários com um espaço no nome de usuário.
- Correção de uma falha que ocorreu ao autenticar durante uma conexão.
- Correção de uma falha que ocorreu ao fechar o cliente.
