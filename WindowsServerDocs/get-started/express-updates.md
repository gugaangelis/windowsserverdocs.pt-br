---
title: Atualizações expressas do Windows Server 2016 habilitado novamente para novembro de 2018 atualizar
description: Fornece informações sobre as atualizações do Express no Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867437"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Atualizações expressas do Windows Server 2016 habilitado novamente para novembro de 2018 atualizar

>Por Joel Frauenheim

>Aplica-se a: Windows Server 2016

Começando com 13 de novembro de 2018 Update terça-feira, o Windows novamente publicará as atualizações do Express para Windows Server 2016. Atualizações expressas do Windows Server 2016 interrompido em meados de 2017, depois que um problema significativo foi encontrado que mantidas as atualizações de instalação correta. Enquanto o problema foi corrigido em novembro de 2017, a equipe de atualização demorou uma abordagem conservadora para publicação dos pacotes do Express para garantir que a maioria dos clientes teria a atualização de 14 de novembro de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) instalados em seu servidor ambientes e não ser afetados pelo problema.

Os administradores de sistema do WSUS e o System Center Configuration Manager (SCCM) precisam estar ciente de que em novembro de 2018 elas mais uma vez verá dois pacotes para a atualização do Windows Server 2016: uma atualização completa e uma atualização do Express. Os administradores do sistema que desejam usar a expressa para seus ambientes de servidor precisam confirmar que o dispositivo foi uma atualização completa desde 14 de novembro de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) para garantir que a atualização do Express foi instalado corretamente. Qualquer dispositivo que não tenha sido atualizado desde que a atualização de 14 de novembro de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) verão falhas repetidas que consomem largura de banda e recursos de CPU em um loop infinito se a atualização expressa é tentada.  Correção para esse estado seria para o administrador do sistema interromper o envio por push a atualização do Express e enviar por push uma recente atualização completa para interromper o loop de falha.

Com 13 de novembro de 2018, os clientes de atualização do Express verá uma redução imediata de tamanho de pacote entre o seu sistema de gerenciamento e os pontos de extremidade do Windows Server 2016.  