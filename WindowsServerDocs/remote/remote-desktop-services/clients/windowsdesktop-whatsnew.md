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
manager: daveba
ms.author: helohr
ms.date: 11/12/2019
ms.localizationpriority: medium
ms.openlocfilehash: db9c2b64e018b41b053974b5459bd320098a6d2d
ms.sourcegitcommit: 315f015102c42c6fa7694e76adecdfb448390391
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74019590"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novidades no cliente da Área de Trabalho do Windows

É possível encontrar informações mais detalhadas sobre o cliente da Área de Trabalho do Windows em [Introdução ao cliente da Área de Trabalho do Windows](windowsdesktop.md). Você encontrará as atualizações mais recentes no cliente abaixo.

## <a name="latest-client-versions"></a>Versões do cliente mais recentes

O cliente pode ser configurado para diferentes [grupos de usuários](windowsdesktop-admin.md#configure-user-groups). A tabela a seguir lista as versões atuais disponíveis para cada grupo de usuários:

|Grupo de usuários |Versão  |
|-----------|---------|
|Public     |1.2.431  |
|Participante do Programa Windows Insider    |1.2.431  |

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

- Correção de uma falha que ocorreu ao autenticar durante uma conexão.
- Correção de uma falha que ocorreu ao fechar o cliente.

## <a name="updates-for-version-12246"></a>Atualizações da versão 1.2.246

*Data da publicação: 28/08/2019*

- Melhoria nos idiomas de fallback para a versão localizada. (Por exemplo, FR-CA será exibido corretamente em francês, em vez de inglês.)
- Ao remover uma assinatura, agora o cliente remove de maneira adequada as credenciais salvas do Gerenciador de Credenciais.
- O processo de atualização do cliente agora é autônomo depois de iniciado e o cliente será reiniciado após a conclusão.
- Agora o cliente pode ser usado no Windows 10 no modo S.
- Correção de um problema que fez o processo de atualização falhar para usuários com um espaço no nome de usuário.
