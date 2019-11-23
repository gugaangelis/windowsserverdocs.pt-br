---
title: Visão geral da implantação do modo de cache hospedado BranchCache
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc6ade92eb5fe04271033973911ccb98e871d236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406378"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Visão geral da implantação do modo de cache hospedado BranchCache

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este guia para implantar um servidor de cache hospedado do BranchCache em uma filial em que os computadores são ingressados em um domínio. Você pode usar este tópico para obter uma visão geral do processo de implantação do modo de cache hospedado do BranchCache.

Esta visão geral inclui a infraestrutura do BranchCache de que você precisa, bem como uma visão geral passo a passo simples de implantação.

## <a name="bkmk_components"></a>Infraestrutura de implantação do servidor de cache hospedado

Nessa implantação, o servidor de cache hospedado é implantado usando pontos de conexão de serviço no Active Directory Domain Services \(AD DS\), e você tem a opção com o BranchCache no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012, para fazer o prehash do conteúdo compartilhado em servidores de conteúdo baseados na Web e em arquivos, e então pré-carregar o conteúdo em servidores de cache hospedado.

A ilustração a seguir mostra a infraestrutura necessária para implantar um servidor de cache hospedado do BranchCache.

![Visão geral do modo de cache hospedado do BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Embora essa implantação descreva os servidores de conteúdo em uma nuvem data center, você pode usar este guia para implantar um servidor de cache hospedado do BranchCache, independentemente de onde você implanta seus servidores de conteúdo – em seu escritório principal ou em um local de nuvem.

### <a name="hcs1-in-the-branch-office"></a>HCS1 na filial

Você deve configurar este computador como um servidor de cache hospedado. Se você optar por fazer o hash de dados do servidor de conteúdo para poder pré-carregar o conteúdo em seus servidores de cache hospedados, poderá importar pacotes de dados que contêm o conteúdo de seus servidores Web e de arquivos.

### <a name="web1-in-the-cloud-data-center"></a>WEB1 na nuvem data center

WEB1 é um servidor de conteúdo habilitado para\-do BranchCache. Se você optar por prefazer o hash de dados do servidor de conteúdo para poder pré-carregar o conteúdo em seus servidores de cache hospedados, você poderá prearmazenar o conteúdo compartilhado em WEB1 e, em seguida, criar um pacote de dados que você copia para o HCS1.

### <a name="file1-in-the-cloud-data-center"></a>FILE1 na nuvem data center

ARQUIVO1 é um servidor de conteúdo de\-habilitado para BranchCache. Se você optar por prefazer o hash de dados do servidor de conteúdo para poder pré-carregar o conteúdo em seus servidores de cache hospedados, você poderá prefazer o hash do conteúdo compartilhado no ARQUIVO1 e, em seguida, criar um pacote de dados que você copia para HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 no escritório principal

DC1 é um controlador de domínio e você deve configurar a política de domínio padrão ou outra política mais apropriada para sua implantação, com o BranchCache Política de Grupo configurações para habilitar a descoberta automática de cache hospedado pelo ponto de conexão de serviço.

Quando os computadores cliente na ramificação tiverem Política de Grupo atualizados e essa configuração de política for aplicada, ele automaticamente localiza e começa a usar o servidor de cache hospedado na filial.

### <a name="client-computers-in-the-branch-office"></a>Computadores cliente na filial

Você deve atualizar Política de Grupo em computadores cliente para aplicar novas configurações de Política de Grupo do BranchCache e para permitir que os clientes localizem e usem o servidor de cache hospedado.

## <a name="bkmk_overview"></a>Visão geral do processo de implantação do servidor de cache hospedado

>[!NOTE]
>Os detalhes de como executar essas etapas são fornecidos na seção implantação do [modo de cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md).

O processo de implantação de um servidor de cache hospedado do BranchCache ocorre nestes estágios:

>[!NOTE]
>Algumas das etapas a seguir são opcionais, como as etapas que demonstram como prehash e pré-carregamento de conteúdo em servidores de cache hospedados. Quando você implanta o BranchCache no modo de cache hospedado, não é necessário colocar o conteúdo de pré-hash em seus servidores de conteúdo da Web e de arquivo, criar um pacote de dados e importar o pacote de dados para pré-carregar seus servidores de cache hospedados com o conteúdo. As etapas são indicadas como opcionais nesta seção e na seção [implantação do modo de cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md) para que você possa ignorá-las se preferir.

1. No HCS1, use os comandos do Windows PowerShell para configurar o computador como um servidor de cache hospedado e registrar um ponto de conexão de serviço no Active Directory.

2. \(\) opcionais em HCS1, se os valores padrão do BranchCache não corresponderem às metas de implantação do servidor e do cache hospedado, configure a quantidade de espaço em disco que você deseja alocar para o cache hospedado. Configure também o local do disco que você prefere para o cache hospedado.

3. \(conteúdo de pré-hash\) opcional em servidores de conteúdo, criar pacotes de dados e pré-carregar conteúdo no servidor de cache hospedado.

    > [!NOTE]
    > O pré-hash e o pré-carregamento de conteúdo em seu servidor de cache hospedado são opcionais. no entanto, se você optar por prehash e Preload, deverá executar todas as etapas abaixo aplicáveis à sua implantação. \(por exemplo, se você não tiver servidores Web, não precisará executar qualquer uma das etapas relacionadas ao prehash e ao pré-carregamento do conteúdo do servidor Web.\)

    1. Em WEB1, o conteúdo do servidor Web de prehash e criar um pacote de dados.

    2. Em ARQUIVO1, conteúdo de servidor de arquivos prehash e criar um pacote de dados.

    3. Em WEB1 e ARQUIVO1, copie os pacotes de dados para o servidor de cache hospedado HCS1.

    4. No HCS1, importe os pacotes de dados para pré-carregar o cache de dados.

4. No DC1, configure computadores cliente da filial ingressada no domínio para o modo de cache hospedado Configurando Política de Grupo com as configurações de política do BranchCache.

5. Em computadores cliente, atualize Política de Grupo.

Para continuar com este guia, consulte [planejamento de implantação do modo de cache hospedado do BranchCache](3-Bc-Hcm-Plan.md).