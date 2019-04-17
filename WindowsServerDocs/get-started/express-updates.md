---
title: Express atualizações para o Windows Server 2016 ativado novamente para novembro de 2018 atualizar
description: Fornece informações sobre as atualizações do Express no Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507844"
---
# Express atualizações para o Windows Server 2016 ativado novamente para novembro de 2018 atualizar

>Por Joel Frauenheim

>Aplica-se a: Windows Server 2016

Começando com 13 de novembro de 2018 terça-feira de atualização, Windows novamente publicará Express atualizações para o Windows Server 2016. Express atualizações para o Windows Server 2016 interrompido em meados 2017 após uma questão importante foi encontrada que mantidos as atualizações de instalação correta. Enquanto o problema foi corrigido em novembro de 2017, a equipe de atualização levou uma abordagem conservadora para publicar os pacotes Express para garantir que a maioria dos clientes deve ter a atualização de 14 de novembro de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) instalada em seus ambientes de servidor e não ser afetados pelo problema.

Os administradores de sistema para WSUS e o System Center Configuration Manager (SCCM) precisam estar ciente de que em novembro de 2018, uma vez aparecerá novamente dois pacotes para a atualização do Windows Server 2016: uma atualização completa e uma atualização Express. Os administradores de sistema que desejam usar Express para seus ambientes de servidor precisam confirmar que o dispositivo teve uma atualização completa desde 14 de novembro de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) para garantir que a atualização Express instalado corretamente. Qualquer dispositivo que não foram atualizado desde que a atualização de 14 de novembro de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) verá repetidas falhas que consomem largura de banda e recursos da CPU em um loop infinito se a atualização Express for tentada.  Correção para esse estado seria para o administrador do sistema parar de enviar a atualização Express e por push uma atualização completa recente para interromper o loop de falha.

Com 13 de novembro de 2018 clientes de atualização Express verá uma redução imediata do tamanho do pacote entre o sistema de gerenciamento e os pontos de extremidade do Windows Server 2016.  