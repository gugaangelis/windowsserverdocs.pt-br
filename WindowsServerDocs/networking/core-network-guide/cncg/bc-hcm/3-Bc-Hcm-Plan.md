---
title: Planejamento da implantação do modo de cache hospedado BranchCache
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 476aa5f87436cd777ae6aa6fa2db70ace623deb7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956013"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Planejamento da implantação do modo de cache hospedado BranchCache

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este tópico para planejar a implantação do BranchCache no modo de cache hospedado.

>[!IMPORTANT]
>O servidor de cache hospedado deve estar executando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.

Antes de implantar o servidor de cache hospedado, você deve planejar os seguintes itens:

- [Planejar a configuração básica do servidor](#bkmk_basic)

- [Planejar o acesso ao domínio](#bkmk_domain)

- [Planejar o local e o tamanho do cache hospedado](#bkmk_cachelocation)

- [Planejar o compartilhamento no qual os pacotes do servidor de conteúdo devem ser copiados](#bkmk_package)

- [Planejar a criação de pacotes de dados e de hash em servidores de conteúdo](#bkmk_prehash)

## <a name="plan-basic-server-configuration"></a><a name="bkmk_basic"></a>Planejar a configuração básica do servidor

Se você estiver planejando usar um servidor existente em sua filial como seu servidor de cache hospedado, não será necessário executar essa etapa de planejamento, pois o computador já está nomeado e tem uma configuração de endereço IP.

Depois de instalar o Windows Server 2016 em seu servidor de cache hospedado, você deve renomear o computador e atribuir e configurar um endereço IP estático para o computador local.

>[!NOTE]
>Neste guia, o servidor de cache hospedado é denominado HCS1, mas você deve usar um nome de servidor apropriado para sua implantação.

## <a name="plan-domain-access"></a><a name="bkmk_domain"></a>Planejar o acesso ao domínio

Se você estiver planejando usar um servidor existente em sua filial como seu servidor de cache hospedado, não precisará executar essa etapa de planejamento, a menos que o computador não esteja atualmente ingressado no domínio.

Para fazer logon no domínio, o computador deve ser um computador membro do domínio e a conta de usuário deve ser criada no AD DS antes da tentativa de logon. Além disso, você deve ingressar o computador no domínio com uma conta que tenha a associação de grupo apropriada.

## <a name="plan-the-location-and-size-of-the-hosted-cache"></a><a name="bkmk_cachelocation"></a>Planejar o local e o tamanho do cache hospedado

Em HCS1, determine onde o servidor de cache hospedado você deseja localizar o cache hospedado. Por exemplo, decida o disco rígido, o volume e o local da pasta em que você planeja armazenar o cache.

Além disso, decida qual percentual de espaço em disco você deseja alocar para o cache hospedado.

## <a name="plan-the-share-to-which-the-content-server-packages-are-to-be-copied"></a><a name="bkmk_package"></a>Planejar o compartilhamento no qual os pacotes do servidor de conteúdo devem ser copiados

Depois de criar pacotes de dados em seus servidores de conteúdo, você deve copiá-los pela rede para um compartilhamento no seu servidor de cache hospedado.

Planeje o local da pasta e as permissões de compartilhamento para a pasta compartilhada. Além disso, se os servidores de conteúdo hospedarem uma grande quantidade de dados e os pacotes que você criar serão arquivos grandes, planeje executar a operação de cópia fora do horário de pico para que a largura de banda WAN não seja consumida pela operação de cópia durante um período em que outras pessoas precisem usar a largura de banda para operações normais de negócios.

## <a name="plan-prehashing-and-data-package-creation-on-content-servers"></a><a name="bkmk_prehash"></a>Planejar a criação de pacotes de dados e de hash em servidores de conteúdo

Antes de fazer o hash de conteúdo em seus servidores de conteúdo, você deve identificar as pastas e os arquivos que contêm o conteúdo que você deseja adicionar ao pacote de dados.

Além disso, você deve planejar o local da pasta local em que você pode armazenar os pacotes de dados antes de copiá-los para o servidor de cache hospedado.

Para continuar com este guia, consulte [implantação do modo de cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md).
