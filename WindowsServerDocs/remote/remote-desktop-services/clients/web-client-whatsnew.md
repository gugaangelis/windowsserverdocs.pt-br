---
title: O que há de novo para o cliente da web de área de trabalho remota?
description: Saiba mais sobre as alterações recentes para o cliente da web de área de trabalho remota
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
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 15218af2f084e9c998d89250aace1d763d03b42a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976335"
---
# <a name="whats-new-for-the-remote-desktop-web-client"></a>O que há de novo para o cliente da web de área de trabalho remota?

Podemos atualizar regularmente o [cliente da web de área de trabalho remota](remote-desktop-web-client.md), adição de novos recursos e corrigir problemas. Confira as últimas atualizações abaixo.

   >[!NOTE]
    >Alteramos o sistema de controle de versão para o cliente web. Começando com a versão 1.0.18.0, todas as versões de lançamento do cliente da web irá conter números (no formato de "W.x.y. z"). Números de versão para o cliente da web de área de trabalho remota sempre terminará com um 0 (por exemplo, W.X.Y.0). Cada versão do cliente de área de trabalho Virtual do Windows web alterará o último dígito até a próxima versão da área de trabalho remoto da web cliente (por exemplo, 1.0.18.1).

## <a name="updates-for-version-10180"></a>Atualizações para a versão 1.0.18.0
*Data de publicação: 5/14/2019*

- Configuração do método de inicialização de recurso adicionada na guia Configurações, permitindo que os usuários abrir recursos no navegador ou baixar um arquivo. rdp para tratar com outro cliente. Essa configuração pode ser configurada pelo seu administrador. Fornece detalhes sobre as configurações de administrador para esse recurso pode ser encontrado na [documentação de instalação do cliente web](remote-desktop-web-client-admin.md).
- Cor fixa mais vivas de renderização de problemas, permitindo que as cores em sua sessão remota.
- Mensagens de erro revisado relacionadas a erros de feed de recurso remoto. 
- Adicionado suporte para mais atalhos do office, como Colar especial (Ctrl + Alt + V).
- Atalho de teclado adicionados para os usuários o invoquem a chave do Windows na sessão remota (Alt+F3)
- Mensagem de erro atualizado para os usuários que tentarem autenticar usando uma senha expirada.
- Feed atualizado da interface do usuário na página de todos os recursos.
- Reconecte resolvidos diálogos sobrepostos que ocorreram durante a sessão.
- Corrigido o dimensionamento do ícone de recurso remoto na barra de tarefas do recurso. 

## <a name="updates-for-version-1011"></a>Atualizações para a versão 1.0.11
*Data de publicação: 2/22/2019*

- Conexão habilitada para o agente de área de trabalho sem um Gateway de área de trabalho remota no Windows Server 2019.
- Em ordem alfabética feeds (ou seja, os RemoteApps primeiro, áreas de trabalho segundo).
- Corrigido vários bugs de acessibilidade, melhorando a compatibilidade de leitor de tela.
- Atualizadas as nossas ferramentas de compilação.
- Várias correções de bugs.

## <a name="updates-for-version-107"></a>Atualizações para a versão 1.0.7
*Data de publicação: 1/24/2019*

- Agora há suporte para uso offline em redes internas.
- Melhor renderização nos navegadores não - Microsoft Edge.
- Limite implementado para repetição de feed de recuperação tenta evitar DoS.
- Bugs de acessibilidade fixo, permitindo que os usuários com deficiência visual usar o cliente da web.
- Aprimorada a mensagens de erro exibidas ao usuário para erros de feed.
- Adicionado Ctrl + Alt + End (Windows) e fn + controle + opção + atalhos de exclusão (Mac) para invocar o Ctrl + Alt + Del no computador remoto.
- Telemetria aprimorada para eventos de falha. 
- Aprimorado o nosso pipeline de build e as ferramentas de compilação.
- Várias correções de bugs.

## <a name="updates-for-version-101"></a>Atualizações para a versão 1.0.1
*Data de publicação: 10/29/2018*

- Adicionada uma opção para **captura informações de suporte** na página sobre para diagnosticar problemas.
- Agora há suporte para o modo inPrivate.
- Suporte aprimorado para teclados inglês.
- Corrigido um problema em que as dicas de ferramenta com caracteres estendidos mostraram incorretamente.
- Problema de renderização de gráficos fixa que Chrome usuários afetados.
- Atualizado o redirecionamento de fuso horário com suporte total do horário de verão.
- Aprimorada a mensagem de erro para o erro de falta de memória.
- Várias correções de bugs.

## <a name="updates-for-version-100"></a>Atualizações para a versão 1.0.0
*Data de publicação: 07/16/2018*

- Cliente da Web da área de trabalho remota agora está geralmente disponível.
- Os administradores podem desligar globalmente ao telemetria para o cliente web.
- Várias correções de bugs.

## <a name="updates-for-version-090"></a>Atualizações para a versão 0.9.0
*Data de publicação: 07/05/2018*

- Nova entrada na experiência do cliente da web.
- Não há mais solicitado a fornecer credenciais ao iniciar uma conexão de área de trabalho ou aplicativo (logon único).
- Movido o cliente da web para uma nova URL: **https://server_FQDN/RDWeb/webclient/index.html**
- Redirecionamento de zona de tempo adicionadas.
- Várias correções de bugs.

## <a name="updates-for-version-081"></a>Atualizações para a versão 0.8.1
*Data de publicação: 05/17/2018*

- Atualizações para tratar de correção de oracle criptografia CredSSP descrita em 0886 de CVE-2018.
- Correção de falhas de conexão para algumas linguagens quando a impressão é habilitada.
- Mensagem de erro aprimorada quando um gateway não é parte da implantação.
- **Ajudar** e **comentários** opções foram adicionadas.

## <a name="updates-for-version-080"></a>Atualizações para a versão 0.8.0
*Data de publicação: 03/28/2018*

- Versão prévia pública inicial do cliente web.
- Copiar/colar o texto por meio da área de transferência com **CTRL + C** e **CTRL + V**.
- Imprimir em um arquivo PDF.
- Localizado em 18 idiomas.
 
