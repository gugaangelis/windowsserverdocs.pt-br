---
title: Introdução ao cliente da Área de Trabalho do Windows
description: Informações básicas sobre o cliente da Área de Trabalho do Windows.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: c864ba0e51054a553bfd53f845bd4d1c9ff3c8ba
ms.sourcegitcommit: 61767c405da44507bd3433967543644e760b20aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70988234"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introdução ao cliente da Área de Trabalho do Windows

>Aplica-se a: Windows 10 e Windows 7

É possível usar o cliente da Área de Trabalho Remota para que a Área de Trabalho do Windows acesse áreas de trabalho e aplicativos do Windows usando outro dispositivo Windows.

> [!NOTE]
> - Esta documentação não é para o cliente da Conexão de Área de Trabalho Remota (MSTSC) fornecido com o Windows. É para o novo cliente da Área de Trabalho Remota (MSRDC).
> - No momento, esse cliente só dá suporte ao acesso a aplicativos e áreas de trabalho remotos da [Área de Trabalho Virtual do Windows](https://aka.ms/wvd).
> - Curioso sobre as novas versões para o cliente da Área de Trabalho do Windows? Confira as [Novidades do cliente de Área de Trabalho do Windows](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>Instalar o cliente

No momento, é possível baixar o cliente do Windows 64 bits. Atualizaremos essa lista conforme o cliente disponibilizar mais versões do Windows.

- [Cliente do Windows 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)

É possível instalar o cliente do usuário atual, que não requer direitos de administrador, ou seu administrador pode instalar e configurar o cliente para que todos os usuários no dispositivo possam acessá-lo.

Depois de instalado, o cliente pode ser iniciado no menu Iniciar pesquisando **Área de Trabalho Remota**.

## <a name="feeds"></a>Feeds

Obtenha a lista de recursos gerenciados que você pode acessar, como aplicativos e áreas de trabalho, assinando o feed que seu administrador forneceu. Quando você assina, os recursos são disponibilizados no PC local. No momento, o cliente da Área de Trabalho do Windows dá suporte a recursos publicados na Área de Trabalho Virtual do Windows.

### <a name="subscribe-to-a-feed"></a>Assinar um feed

1. Na página principal do cliente, também conhecida como Central de Conexão, toque em **Assinar**.
2. Entre com sua conta quando solicitado.
3. Os recursos serão exibidos na Central de Conexão agrupados por Workspace.

É possível iniciar recursos com um dos seguintes métodos:

- Acesse a Central de Conexão e clique duas vezes em um recurso para iniciá-lo.
- Também é possível acessar o menu Iniciar e procurar uma pasta com o nome do Workspace ou inserir o nome do recurso na barra de pesquisa.

### <a name="workspace-details"></a>Detalhes do Workspace

Após assinar, será possível exibir outras informações sobre um Workspace no painel Detalhes:

- O nome do Workspace
- A URL e o nome de usuário usados para assinar
- O número de aplicativos e áreas de trabalho
- A data/hora da última atualização
- O status da última atualização

Acessar o painel Detalhes:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do Workspace.
2. Selecione **Detalhes** no menu suspenso.
3. O painel Detalhes é exibido no lado direito do cliente.

Depois de assinar, o Workspace será atualizado automaticamente de forma regular. Os recursos podem ser adicionados, alterados ou removidos com base nas alterações realizadas pelo seu administrador.

Também é possível procurar manualmente atualizações para os recursos, quando necessário, selecionando **Atualizar agora** no painel Detalhes.

### <a name="unsubscribe-from-a-feed"></a>Cancelar a assinatura de um feed

Esta seção ensinará como cancelar a assinatura de um feed. É possível cancelar a assinatura para assinar novamente com uma conta diferente ou remover seus recursos do sistema.

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do Workspace.
2. Selecione **Cancelar assinatura** no menu suspenso.
3. Examine a caixa de diálogo e selecione **Continuar**.

## <a name="update-the-client"></a>Atualizar o cliente

A menos que tenha sido desabilitado por um administrador, você será notificado quando uma nova versão do cliente estiver disponível. Essa notificação pode ser mostrada diretamente na Central de Conexão ou na Central de Ações do Windows. Selecione a notificação para iniciar o processo de atualização.

Também é possível pesquisar manualmente novas atualizações para o cliente:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) na barra de comandos na parte superior do cliente.
2. Selecione **Sobre** no menu suspenso.
3. Toque em **Verificar se há atualizações**.
4. Se houver uma atualização disponível, toque em **Instalar atualização** para atualizar o cliente.

## <a name="providing-feedback"></a>Fornecendo comentários

Tem uma sugestão de recursos ou deseja relatar um problema? Conte-nos usando o [Hub de Comentários](feedback-hub://?tabid=2&contextid=883) que também pode ser acessado do cliente:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) na barra de comandos na parte superior do cliente.
2. Selecione **Comentários** no menu suspenso para abrir o Hub de Comentários.
