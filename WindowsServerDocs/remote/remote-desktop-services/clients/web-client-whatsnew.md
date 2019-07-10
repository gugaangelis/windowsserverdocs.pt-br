---
title: O que há de novo para o cliente Web da Área de Trabalho Remota?
description: Saiba mais sobre as recentes alterações no cliente da Web da Área de Trabalho Remota
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
ms.openlocfilehash: 5be9b05da1e78cc54e12254f43d0f44f7ff65c5d
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66804886"
---
# <a name="whats-new-for-the-remote-desktop-web-client"></a>O que há de novo no cliente da Web da Área de Trabalho Remota?

Atualizamos regularmente o [cliente da Web da Área de Trabalho Remota](remote-desktop-web-client.md) adicionando novos recursos e corrigindo problemas. Confira as últimas atualizações abaixo.

> [!NOTE]
> Alteramos o sistema de controle de versão para o cliente da Web. Começando com a versão 1.0.18.0, todas as versões de lançamento do cliente da Web irão conter números (no formato "W.X.Y.Z"). Os números de versão para o cliente Web de Área de Trabalho Remota sempre terminarão com um 0 (por exemplo, W.X.Y.0). Cada versão do cliente Web de Área de Trabalho Remota Virtual do Windows alterará o último dígito até a próxima versão do cliente Web da Área de Trabalho Remota (por exemplo, 1.0.18.1).

## <a name="updates-for-version-10180"></a>Atualizações para a versão 1.0.18.0
*Data de publicação: 14/5/2019*

- Configuração do Método de Inicialização de Recursos adicionada à guia Configurações, permitindo que os usuários abram recursos no navegador ou baixem um arquivo .rdp para lidar com outro cliente. Essa configuração pode ser definida pelo seu administrador. Os detalhes adicionais relacionados às configurações de administrador desse recurso podem ser encontrados na [documentação de configuração do cliente Web](remote-desktop-web-client-admin.md).
- Corrigidos os problemas de processamento de cor, possibilitando cores mais vivas em sua sessão remota.
- Revisadas as mensagens de erro relacionadas a erros de feed de recurso remoto. 
- Adicionado suporte para mais atalhos do office, como colar especial (Ctrl+Alt+V).
- Adicionado o atalho de teclado para os usuários chamarem a tecla Windows na sessão remota (Alt+F3)
- Atualizadas as mensagens de erro para usuários que tentavam a autenticação usando uma senha expirada.
- Atualizada a interface do feed na página Todos os Recursos.
- Resolvido o problema das caixas de diálogo que se sobrepunham durante a reconexão da sessão.
- Corrigido o dimensionamento do ícone de recurso remoto na barra de tarefas do recurso.

## <a name="updates-for-version-1011"></a>Atualizações para a versão 1.0.11
*Data de publicação: 22/02/2019*

- Conexão habilitada para o Agente de Área de Trabalho Remota sem um Gateway de Área de Trabalho Remota no Windows Server 2019.
- Classificou os feeds alfabeticamente (por exemplo, RemoteApps primeiro, Desktops em segundo).
- Corrigidos vários bugs de acessibilidade melhorando a compatibilidade de leitor de tela.
- Atualizadas as nossas ferramentas de compilação.
- Várias correções de bugs.

## <a name="updates-for-version-107"></a>Atualizações para a versão 1.0.7
*Data de publicação: 24/01/2019*

- Agora há suporte para uso offline em redes internas.
- Melhor renderização nos navegadores diferentes do Microsoft Edge.
- Limite implementado para repetição de tentativas de recuperação de feed para evitar DoS.
- Corrigidos bugs de acessibilidade, permitindo que os usuários com deficiência visual usem o cliente da Web.
- Aprimoradas as mensagens de erro exibidas para o usuário nos erros de feed.
- Adicionados os atalhos Ctrl+Alt+End (Windows) e fn+control+option+delete (Mac) para chamar Ctrl+Alt+Del no computador remoto.
- Telemetria aprimorada para eventos de falha.
- Aprimorado o pipeline e as ferramentas de build.
- Várias correções de bugs.

## <a name="updates-for-version-101"></a>Atualizações para a versão 1.0.1
*Data de publicação: 29/10/2018*

- Adicionada uma opção para **Capturar informações de suporte** na página Sobre para diagnosticar problemas.
- Agora há suporte para o modo InPrivate.
- Suporte aprimorado para teclados diferentes do inglês.
- Corrigido um problema em que as dicas de ferramenta com caracteres diferentes do inglês apareciam de forma incorreta.
- Corrigido o problema de renderização de gráficos que afetava os usuários do Chrome.
- Atualizado o redirecionamento de fuso horário com suporte total ao horário de verão.
- Aprimorada a mensagem de erro para o erro de falta de memória.
- Várias correções de bugs.

## <a name="updates-for-version-100"></a>Atualizações para a versão 1.0.0
*Data de publicação: 16/07/2018*

- O Cliente da Web da Área de Trabalho Remota agora está disponível em geral.
- Os administradores podem desligar globalmente a telemetria para o cliente da Web.
- Várias correções de bugs.

## <a name="updates-for-version-090"></a>Atualizações para a versão 0.9.0
*Data de publicação: 05/07/2018*

- Nova experiência de entrada no cliente da Web.
- Não há mais solicitação do fornecimento de credenciais ao iniciar uma conexão de Área de Trabalho Remota ou de aplicativo (Logon único).
- O cliente da Web foi movido para uma nova URL: <https://server_FQDN/RDWeb/webclient/index.html>
- Adicionado o redirecionamento de fuso horário.
- Várias correções de bugs.

## <a name="updates-for-version-081"></a>Atualizações para a versão 0.8.1
*Data de publicação: 17/05/2018*

- Atualizações para solucionar a correção de criptografia Oracle CredSSP descrita em CVE-2018-0886.
- Corrigidas falhas de conexão para alguns idiomas quando a impressão está habilitada.
- Aprimorada a mensagem de erro quando um gateway não faz parte da implantação.
- Adicionadas opções de **Ajuda** e **Comentários**.

## <a name="updates-for-version-080"></a>Atualizações para a versão 0.8.0
*Data de publicação: 28/03/2018*

- Versão prévia pública inicial do cliente da Web.
- Copiar/colar o texto da área de transferência com **Ctrl+C** e **Ctrl+V**.
- Imprimir em um arquivo PDF.
- Localizado em 18 idiomas.
 
