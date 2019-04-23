---
title: Planejamento da implantação do modo de cache hospedado BranchCache
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7232f8732e7476b955115741b5582a585dc6068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890677"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Planejamento da implantação do modo de cache hospedado BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este tópico para planejar a implantação do BranchCache no modo de Cache hospedado.

>[!IMPORTANT]
>Seu servidor de cache hospedado deve estar executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

Antes de implantar o servidor de cache hospedado, você deve planejar os seguintes itens:

- [Planejar a configuração básica do servidor](#bkmk_basic)

- [Planejar o acesso de domínio](#bkmk_domain)

- [Planejar o local e o tamanho do cache hospedado](#bkmk_cachelocation)

- [Planejar o compartilhamento ao qual os pacotes do servidor de conteúdo devem ser copiados](#bkmk_package)

- [Hash de plano e dados de criação nos servidores de conteúdo do pacote](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planejar a configuração básica do servidor
  
Se você estiver planejando usar um servidor existente da filial como seu servidor de cache hospedado, você não precisará executar essa etapa de planejamento, porque o computador já está nomeado e tem uma configuração de endereço IP.

Depois de instalar o Windows Server 2016 em seu servidor de cache hospedado, você deve renomear o computador e atribuir e configurar um endereço IP estático para o computador local.

>[!NOTE]
>Neste guia, o servidor de cache hospedado é denominado HCS1, no entanto, você deve usar um nome de servidor que seja apropriado para sua implantação.

## <a name="bkmk_domain"></a>Planejar o acesso de domínio

Se você estiver planejando usar um servidor existente da filial como seu servidor de cache hospedado, você precisa realizar essa etapa de planejamento, a menos que o computador não está atualmente associado ao domínio.
  
Para fazer logon no domínio, o computador deve ser um computador membro do domínio e a conta de usuário deve ser criada no AD DS antes da tentativa de logon. Além disso, você deve associar o computador ao domínio com uma conta que tenha a associação de grupo apropriado.

## <a name="bkmk_cachelocation"></a>Planejar o local e o tamanho do cache hospedado

Em HCS1, determine onde no seu servidor de cache hospedado você deseja localizar o cache hospedado. Por exemplo, decida o disco rígido, o volume e o local da pasta em que você planeja armazenar o cache.

Além disso, decida qual é a porcentagem de espaço em disco que você deseja alocar para o cache hospedado.

## <a name="bkmk_package"></a>Planejar o compartilhamento ao qual os pacotes do servidor de conteúdo devem ser copiados

Depois de criar pacotes de dados nos servidores de conteúdo, você deve copiá-los pela rede para um compartilhamento no servidor de cache hospedado.

Planeje o local da pasta e permissões de compartilhamento para a pasta compartilhada. Além disso, se seus servidores de conteúdo hospedar uma grande quantidade de dados e os pacotes que você criar serão arquivos grandes, planeje executar a operação de cópia durante o horário de pico – desativando \ para que a largura de banda WAN não é consumida pela operação de cópia em um horário quando outras precisam usar  a largura de banda para operações normais de negócios.

## <a name="bkmk_prehash"></a>Hash de plano e dados de criação nos servidores de conteúdo do pacote

Antes de você pré-hash do conteúdo em seus servidores de conteúdo, você deve identificar as pastas e arquivos que contêm o conteúdo que você deseja adicionar ao pacote de dados. 

Além disso, você deve planejar o local da pasta local onde você pode armazenar os pacotes de dados antes de copiá-los para o servidor de cache hospedado.

Para continuar com este guia, consulte [implantação do modo de Cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md).
