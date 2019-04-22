---
title: Criar pacotes de dados do servidor de conteúdo para conteúdo da Web e do arquivo (opcional)
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8cd284a83736d17859968947f381af171fd6bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817167"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Criar pacotes de dados do servidor de conteúdo para conteúdo da Web e do arquivo (opcional)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para pré-hash do conteúdo na Web e servidores de arquivos e, em seguida, criar pacotes de dados para importar em seu servidor de cache hospedado. 

Esse procedimento é opcional, porque você não é necessários para prehash e pré-carregamento de conteúdo em seus servidores de cache hospedado. Se você não pré-carregar conteúdo, dados são adicionados ao cache hospedado automaticamente como clientes baixá-lo sobre a conexão de WAN.

Esse procedimento fornece instruções para realizar o hash prévio conteúdo em servidores de arquivos e servidores Web. Se você não tiver um desses tipos de servidores de conteúdo, você não precisa executar as instruções para esse tipo de servidor de conteúdo.

>[!IMPORTANT]
>Antes de executar este procedimento, você deve instalar e configurar o BranchCache no seus servidores de conteúdo. Além disso, se você pretende alterar o segredo do servidor em um servidor de conteúdo, fazê-lo antes de pré\-invalida de hash de conteúdo – modificar o segredo do servidor anteriormente\-gerado hashes.

Para executar esse procedimento, é necessário ser membro do grupo Administradores.

## <a name="to-create-content-server-data-packages"></a>Para criar pacotes de dados de servidor de conteúdo

1. Em cada servidor de conteúdo, localize as pastas e arquivos que você deseja pré-hash e adicionar a um pacote de dados. Identifique ou crie uma pasta onde você deseja salvar seu pacote de dados neste procedimento.

2. No computador do servidor, abra o Windows PowerShell com privilégios de administrador.

3. Siga um ou ambos destes procedimentos, dependendo dos tipos de servidores de conteúdo que você tem:

    > [!NOTE]
    > O valor – caminho para o parâmetro é a pasta onde se encontra seu conteúdo. Você deve substituir os valores de exemplo nos comandos abaixo com um local de pasta válido no servidor de conteúdo que contém dados que você deseja pré-hash e adicionar a um pacote.
  
    - Se o conteúdo que você deseja efetuar o prehash estiver em um servidor de arquivos, digite o seguinte comando e pressione ENTER.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Se o conteúdo que você deseja efetuar o prehash estiver em um servidor Web, digite o seguinte comando e pressione ENTER.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Crie o pacote de dados, executando o seguinte comando em cada um dos seus servidores de conteúdo. Substitua o valor de exemplo \(unidade d:\\temp\) para o parâmetro – destino com o local que você identificou ou criado no início deste procedimento.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Do servidor de conteúdo, acessar o compartilhamento em seus servidores de cache hospedado, onde você deseja pré-carregar conteúdo e copie os pacotes de dados para os compartilhamentos em servidores de cache hospedado.

Para continuar com este guia, consulte [importar pacotes de dados no servidor de Cache hospedado &#40;opcional&#41;](9-Bc-Import-Data.md).

