---
title: Introdução ao centro de administração do Windows
description: Introdução ao centro de administração do Windows
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 02/15/2019
ms.openlocfilehash: c824b2ae8c43be4b5b33b79ce9ddb75dd03c9a9e
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990527"
---
# <a name="get-started-with-windows-admin-center"></a>Introdução ao centro de administração do Windows

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

> [!Tip]
> Conhecendo o Windows Admin Center agora?
> [Saiba mais sobre o Windows Admin Center](../overview.md) ou [baixe-o agora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Centro de administração do Windows instalado no Windows 10

> [!IMPORTANT]
> Você deve ser membro do grupo do administrador local para usar o centro de administração do Windows no Windows 10

### <a name="selecting-a-client-certificate"></a>Selecionando um certificado de cliente

Na primeira vez que você abrir o centro de administração do Windows no Windows 10, certifique-se de selecionar o certificado do *cliente do centro de administração do Windows* (caso contrário, você receberá um erro HTTP 403 dizendo "não é possível chegar a esta página").

No Microsoft Edge, quando você receber uma solicitação com esta caixa de diálogo:

1. Clique em **mais opções**

    ![Selecione uma caixa de certificado com mais opções realçadas](../media/launch-cert-1.png)

2. Selecione o certificado rotulado **cliente do centro de administração do Windows** e clique em **OK**

    ![Selecione uma caixa de certificado mostrando os certificados disponíveis](../media/launch-cert-2.png)

3. Certifique-se de **que sempre permitir acesso** esteja selecionado e clique em **permitir**

    ! Caixa de diálogo credencial necessária] (.. launch-cert-3.png/Media/)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Conectando-se a nós e clusters gerenciados

Depois de concluir a instalação do centro de administração do Windows, você pode adicionar servidores ou clusters a serem gerenciados na página Visão geral principal.

 **Adicionar um único servidor ou um cluster como um nó gerenciado**

1. Clique em **+ Adicionar** em **todas as conexões**.

   ![Windows Admin Center – Página Todas as Conexões](../media/launch/addserver0.png)

2. Escolha Adicionar um servidor, cluster, computador Windows ou uma VM do Azure:

   ![Centro de administração do Windows – página Adicionar recursos](../media/launch/ChooseConnectionType.png)

3. Digite o nome do servidor ou cluster a ser gerenciado e clique em **Enviar**. O servidor ou cluster será adicionado à sua lista de conexões na página Visão geral.

   ![Centro de administração do Windows-página servidores](../media/launch/addserver2.png)

   **--OU--**

**Importação em massa de vários servidores**

 1. Na página **Adicionar conexão do servidor** , escolha a guia **importar servidores** .

    ![Centro de administração do Windows – guia importar servidores](../media/launch/import-servers.png)

 2. Clique em **procurar** e selecione um arquivo de texto que contenha uma vírgula ou uma nova linha separada por uma lista de FQDNs para os servidores que você deseja adicionar.

> [!Note]
> O arquivo. csv criado [exportando suas conexões com o PowerShell](#use-powershell-to-import-or-export-your-connections-with-tags) contém informações adicionais além dos nomes de servidor e não é compatível com esse método de importação.

  **--OU--**

**Adicionar servidores pesquisando Active Directory**

 1. Na página **Adicionar conexão do servidor** , escolha a guia **Pesquisar Active Directory** .

    ![Centro de administração do Windows – guia pesquisa Active Directory](../media/launch/search-ad.png)

 2. Insira seus critérios de pesquisa e clique em **Pesquisar**. Há suporte para curingas (*).

 3. Após a conclusão da pesquisa-selecione um ou mais dos resultados, opcionalmente, adicione marcas e clique em **Adicionar**.

## <a name="authenticate-with-the-managed-node"></a>Autenticar com o nó gerenciado ##

O centro de administração do Windows dá suporte a vários mecanismos de autenticação com um nó gerenciado. O logon único é o padrão.

**Logon único**

Você pode usar suas credenciais atuais do Windows para autenticar com o nó gerenciado. Esse é o padrão, e o centro de administração do Windows tenta o logon quando você adiciona um servidor.

**Logon único quando implantado como um serviço no Windows Server**

Se você tiver instalado o centro de administração do Windows no Windows Server, a configuração adicional será necessária para o logon único.  [Configurar seu ambiente para delegação](../configure/user-access-control.md)

**--OU--**

**Usar *gerenciar como* para especificar credenciais**

Em **todas as conexões**, selecione um servidor na lista e escolha **gerenciar como** para especificar as credenciais que serão usadas para autenticar o nó gerenciado:

![Todas as conexões, gerenciar como opção](../media/launch-use-6.png)

Se o centro de administração do Windows estiver sendo executado no modo de serviço no Windows Server, mas você não tiver a delegação Kerberos configurada, deverá inserir novamente suas credenciais do Windows:

![Página especificar suas credenciais](../media/launch-use-7.png)

Você pode aplicar as credenciais a todas as conexões, que as armazenará em cache para essa sessão específica do navegador. Se você recarregar o navegador, deverá inserir novamente as credenciais de **gerenciar como** .

**Solução de senha de administrador local (LAPSos)**

Se seu ambiente usa [lapsos](/previous-versions/mt227395(v=msdn.10))e você tem o centro de administração do Windows instalado em seu PC com Windows 10, você pode usar as credenciais de lapso para autenticar com o nó gerenciado. **Se você usar esse cenário,** [forneça comentários](https://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Usando marcas para organizar suas conexões

Você pode usar marcas para identificar e filtrar servidores relacionados na sua lista de conexões.  Isso permite que você veja um subconjunto dos servidores na lista de conexões.  Isso é especialmente útil se você tiver muitas conexões.

### <a name="edit-tags"></a>Editar marcas

* Selecione um servidor ou vários servidores na lista todas as conexões
* Em **todas as conexões**, clique em **Editar marcas**

![Centro de administração do Windows – opção Editar marcas](../media/launch/tags-5.png)

O painel **Editar marcas de conexão** permite que você modifique, adicione ou remova marcas de suas conexões selecionadas:

* Para adicionar uma nova marca às conexões selecionadas, selecione **adicionar marca** e insira o nome da marca que você deseja usar.

* Para marcar as conexões selecionadas com um nome de marca existente, marque a caixa ao lado do nome da marca que você deseja aplicar.

* Para remover uma marca de todas as conexões selecionadas, desmarque a caixa ao lado da marca que você deseja remover.

* Se uma marca for aplicada a um subconjunto das conexões selecionadas, a caixa de seleção será mostrada em um estado intermediário. Você pode clicar na caixa para verificá-la e aplicar a marca a todas as conexões selecionadas ou clicar novamente para desmarcar e remover a marca de todas as conexões selecionadas.

![Centro de administração do Windows – página Editar marcas de conexão](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrar conexões por marca

Depois que as marcas tiverem sido adicionadas a uma ou mais conexões de servidor, você poderá exibir as marcas na lista de conexões e filtrar a lista de conexões por marcas.

* Para filtrar por uma marca, selecione o ícone de filtro ao lado da caixa de pesquisa.

   ![Centro de administração do Windows – filtrar usando a caixa de pesquisa](../media/launch/tags-7.png)

   * Você pode selecionar "or", "and" ou "Not" para modificar o comportamento do filtro das marcas selecionadas.

   ![Central de administração do Windows-página Filtrar conexões](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usar o PowerShell para importar ou exportar suas conexões (com marcas)

[!INCLUDE [ps-connections](../includes/ps-connections.md)]

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Exibir scripts do PowerShell usados no centro de administração do Windows

Depois de se conectar a um servidor, cluster ou PC, você pode examinar os scripts do PowerShell que capacitam as ações da interface do usuário no centro de administração do Windows. De dentro de uma ferramenta, clique no ícone do PowerShell na barra de aplicativos superior. Selecione um comando de interesse na lista suspensa para navegar até o script do PowerShell correspondente.

![Exibir scripts do PowerShell para página de visão geral](../media/launch/showscript.png)