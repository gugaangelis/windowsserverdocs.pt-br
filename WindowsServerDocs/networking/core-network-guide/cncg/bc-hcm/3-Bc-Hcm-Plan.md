---
title: BranchCache hospedado Cache modo planejamento de implantação
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e645dd96ec85e3a23df6717cfa43d7627cb938e7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache hospedado Cache modo planejamento de implantação

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este tópico para planejar a implantação do BranchCache no modo de Cache hospedado.

>[!IMPORTANT]
>O servidor de cache hospedado deve estar executando o Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016.

Antes de implantar seu servidor de cache hospedado, você deve planejar os seguintes itens:

- [Planejar a configuração básica de servidor](#bkmk_basic)

- [Planeje o acesso ao domínio](#bkmk_domain)

- [Planeje o local e o tamanho do cache hospedado](#bkmk_cachelocation)

- [Planeje o compartilhamento para que os pacotes de servidor de conteúdo devem ser copiados](#bkmk_package)

- [Plano prehashing e dados do pacote de criação em servidores de conteúdo](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planejar a configuração básica de servidor
  
Se você estiver planejando usar um servidor existente em sua filial como seu servidor de cache hospedado, você não precisa executar essa etapa de planejamento, porque o computador já é chamado e tem uma configuração de endereço IP.

Depois de instalar o Windows Server 2016 em seu servidor de cache hospedado, você deve renomear o computador e atribuir e configurar um endereço IP estático para o computador local.

>[!NOTE]
>Neste guia, o servidor de cache hospedado é chamado HCS1, no entanto, você deve usar um nome de servidor apropriado para a implantação.

## <a name="bkmk_domain"></a>Planeje o acesso ao domínio

Se você estiver planejando usar um servidor existente em sua filial como seu servidor de cache hospedado, você não precisa executar essa etapa de planejamento, a menos que o computador no momento não tenha ingressado no domínio.
  
Para fazer logon no domínio, o computador deve ser um computador membro do domínio e a conta de usuário deve ser criada no AD DS antes da tentativa de logon. Além disso, você deve ingressar o computador ao domínio com uma conta que tenha a associação ao grupo apropriado.

## <a name="bkmk_cachelocation"></a>Planeje o local e o tamanho do cache hospedado

Na HCS1, determine onde em seu servidor de cache hospedado você deseja localizar o cache hospedado. Por exemplo, decida o disco rígido, o volume e o local da pasta onde pretende armazenar o cache.

Além disso, decida que percentual do espaço em disco que você quiser alocar para o cache hospedado.

## <a name="bkmk_package"></a>Planeje o compartilhamento para que os pacotes de servidor de conteúdo devem ser copiados

Depois de criar pacotes de dados em seus servidores de conteúdo, você deve copiá-los pela rede para um compartilhamento em seu servidor de cache hospedado.

Planeje o local da pasta e permissões de compartilhamento para a pasta compartilhada. Além disso, se seus servidores conteúdos hospedarem uma grande quantidade de dados e os pacotes que você criar estarão arquivos grandes, planeje executar a operação de cópia em horários de pico – desativando \ para que a largura de banda WAN não é consumida pela operação de cópia durante um tempo quando outros precisam usar a largura de banda para operações comerciais normais.

## <a name="bkmk_prehash"></a>Plano prehashing e dados do pacote de criação em servidores de conteúdo

Antes de você prehash conteúdo em seus servidores de conteúdo, você deve identificar as pastas e arquivos que contêm conteúdo que você deseja adicionar ao pacote de dados. 

Além disso, você deve planejar o local da pasta local onde você pode armazenar os pacotes de dados antes de copiá-los para o servidor de cache hospedado.

Para continuar com este guia, consulte [implantação do modo de Cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md).
