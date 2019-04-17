---
title: Criar conteúdo pacotes de dados de servidor de Web ou um arquivo de conteúdo (opcional)
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f814bbac5c74081259d8eef6deda79d914bfec7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Criar conteúdo pacotes de dados de servidor de Web ou um arquivo de conteúdo (opcional)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para prehash conteúdo na Web e servidores de arquivos e, em seguida, criar pacotes de dados para importar em seu servidor de cache hospedado. 

Esse procedimento é opcional, porque não é necessário para conteúdo prehash e pré-carregamento em seus servidores de cache hospedado. Se você não pré-carregar conteúdo, dados são adicionados para o cache hospedado automaticamente como clientes baixá-lo sobre a conexão WAN.

Este procedimento fornece instruções para prehashing conteúdo em servidores de arquivos e servidores Web. Se você não tiver um desses tipos de conteúdo servidores, você não precisa realizar as instruções para esse tipo de servidor de conteúdo.

>[!IMPORTANT]
>Antes de executar este procedimento, você deve instalar e configurar BranchCache em seus servidores de conteúdo. Além disso, se você planeja alterar o segredo do servidor em um servidor de conteúdo, fazê-lo antes de hash pre\ conteúdo – modificar o segredo do servidor invalida hashes previously\ gerado.

Para executar este procedimento, você deve ser um membro do grupo Administradores.

## <a name="to-create-content-server-data-packages"></a>Para criar pacotes de dados de servidor de conteúdo

1. Em cada servidor de conteúdo, localize as pastas e arquivos que você deseja prehash e adicionar a um pacote de dados. Identificar ou crie uma pasta onde você deseja salvar o pacote de dados neste procedimento.

2. No computador servidor, abra o Windows PowerShell com privilégios de administrador.

3. Siga um ou mais destes procedimentos, dependendo dos tipos de servidores de conteúdo que você tem:

    > [!NOTE]
    > O valor do – caminho de parâmetro é a pasta onde o conteúdo está localizado. Você deve substituir os valores de exemplo nos comandos abaixo com um local de pasta válida em seu servidor de conteúdo que contém dados que você deseja prehash e adicionar a um pacote.
  
    - Se o conteúdo que você deseja prehash estiver instalado em um servidor de arquivos, digite o seguinte comando e pressione ENTER.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Se o conteúdo que você deseja prehash estiver em um servidor Web, digite o seguinte comando e pressione ENTER.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Crie o pacote de dados, executando o seguinte comando em cada um dos seus servidores de conteúdo. Substitua o exemplo valor \(D:\\temp\) para o destino parâmetro – com o local que você identificou ou criadas no início deste procedimento.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Do servidor de conteúdo, acessar o compartilhamento em seus servidores de cache hospedado onde deseja pré-carregar conteúdo e copiar os pacotes de dados para os compartilhamentos nos servidores do cache hospedado.

Para continuar com este guia, consulte [importar pacotes de dados no servidor de Cache hospedado #40 opcional &; 41; ](9-Bc-Import-Data.md).

