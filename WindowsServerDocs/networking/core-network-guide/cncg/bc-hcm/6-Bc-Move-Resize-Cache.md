---
title: Mover e redimensionar o cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cb75e06b5da8ff95fcf763b22c5160ea200035f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853527"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Mova e redimensione o Cache hospedado \(opcional\)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para mover o cache hospedado para a unidade e a pasta que você prefira e para especificar a quantidade de espaço em disco que o servidor de cache hospedado pode usar para o cache hospedado.

Esse procedimento é opcional. Se o padrão local do cache \(% windir %\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) e tamanho – o que é de 5% do disco rígido total espaço – são apropriadas para sua implantação, você não precise alterá-los.

Você deve ser membro do grupo Administradores para executar esse procedimento.

### <a name="to-move-and-resize-the-hosted-cache"></a>Mover e redimensionar o cache hospedado

1. Abra o Windows PowerShell com privilégios de administrador.

2. Digite o seguinte comando para mover o cache hospedado em outro local no computador local e, em seguida, pressione ENTER.

    > [!IMPORTANT]
    > Antes de executar o comando a seguir, substitua os valores de parâmetro, como – Path e – MoveTo, com valores apropriados para sua implantação.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Digite o seguinte comando para redimensionar o hospedado de cache – especificamente o datacache \- no computador local. Pressione ENTER.

    > [!IMPORTANT]
    > Antes de executar o comando a seguir, substitua os valores de parâmetro, como \-percentual, com valores apropriados para sua implantação.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Para verificar a configuração do servidor de cache hospedado, digite o seguinte comando e pressione ENTER.

    ``` 
    Get-BCStatus
    ``` 

    Os resultados do comando exibem o status de todos os aspectos de sua instalação do BranchCache. A seguir estão algumas das configurações do BranchCache e o valor correto para cada item:

    -   DataCache | CacheFileDirectoryPath: Exibe o local do disco rígido que corresponde ao valor fornecido com o parâmetro – MoveTo do comando SetBCCache. Por exemplo, se você tiver fornecido o valor d:\\datacache, que o valor é exibido na saída do comando.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume: Exibe o número que corresponde ao valor fornecido com o parâmetro – percentual do comando SetBCCache. Por exemplo, se você tiver fornecido o valor 20, esse valor é exibido na saída do comando.

Para continuar com este guia, consulte [Prehash e pré-carregar conteúdo no servidor de Cache hospedado &#40;opcional&#41;](7-Bc-Prehash-Preload.md).