---
title: Mova e redimensione o Cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a3ed1d6de1281575b33cdf75a5970d2ed033085
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Mova e redimensione o Cache hospedado \(Optional\)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para mover o cache hospedado para a unidade e a pasta que você preferir e especifique a quantidade de espaço em disco que o servidor de cache hospedado pode usar para o cache hospedado.

Esse procedimento é opcional. Se o padrão cache local \(%windir%\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) e o tamanho – que é 5% do espaço total no disco rígido – são apropriados para a implantação, você não precise alterá-las.

Você deve ser um membro do grupo Administradores para executar este procedimento.

### <a name="to-move-and-resize-the-hosted-cache"></a>Para mover e redimensionar o cache hospedado

1. Abra o Windows PowerShell com privilégios de administrador.

2. Digite o seguinte comando para mover o cache hospedado para outro local no computador local e pressione ENTER.

    > [!IMPORTANT]
    > Antes de executar o comando a seguir, substitua os valores de parâmetro, como – caminho e – MoveTo, valores que são apropriados para a implantação.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Digite o seguinte comando para redimensionar hospedado cache – especificamente o datacache \ - no computador local. Pressione ENTER.

    > [!IMPORTANT]
    > Antes de executar o comando a seguir, substitua os valores de parâmetro, como \-Percentage, os valores que são apropriados para a implantação.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Para verificar a configuração do servidor de cache hospedado, digite o seguinte comando e pressione ENTER.

    ``` 
    Get-BCStatus
    ``` 

    Os resultados do comando de exibem o status para todos os aspectos da instalação do BranchCache. A seguir estão algumas das configurações do BranchCache e o valor correto para cada item:

    -   DataCache | CacheFileDirectoryPath: Exibe o local do disco rígido que corresponde ao valor fornecido com o parâmetro – MoveTo do comando SetBCCache. Por exemplo, se você forneceu o valor D:\\datacache, esse valor é exibido na saída do comando.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume: Exibe o número que corresponde ao valor fornecido com o percentual parâmetro – do comando SetBCCache. Por exemplo, se você forneceu o valor 20, esse valor é exibido na saída do comando.

Para continuar com este guia, consulte [Prehash e pré-carregamento conteúdo sobre o servidor de Cache hospedado #40 opcional &; 41; ](7-Bc-Prehash-Preload.md).