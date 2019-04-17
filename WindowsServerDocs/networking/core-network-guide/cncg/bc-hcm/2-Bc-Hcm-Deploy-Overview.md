---
title: BranchCache hospedado Cache modo visão geral da implantação
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: feab3156b89637f64d1af0250df459533b1662c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache hospedado Cache modo visão geral da implantação

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este guia para implantar um servidor de cache BranchCache hospedado em uma filial em que os computadores estão associados a um domínio. Você pode usar este tópico para obter uma visão geral do processo de implantação BranchCache o modo de Cache hospedado.

Esta visão geral inclui a infraestrutura BranchCache que você precisa, bem como um panorama simples passo a passo da implantação.

## <a name="bkmk_components"></a>Infraestrutura de implantação de servidor de Cache hospedada

Nessa implantação, o servidor de cache hospedado é implantado usando pontos de conexão de serviço no Active Directory Domain Services \(AD DS\) e você tem a opção com BranchCache no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, para prehash o conteúdo compartilhado na Web ou um arquivo com base em servidores de conteúdo, em seguida, pré-carregar o conteúdo em servidores de cache hospedado.

A ilustração a seguir mostra a infraestrutura que é necessário para implantar um servidor de cache BranchCache hospedado.

![Visão geral do modo de Cache hospedado BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Embora essa implantação retratar servidores de conteúdo em um centro de dados na nuvem, você pode usar este guia para implantar um servidor de cache BranchCache hospedado, independentemente de onde você implantar seu servidores de conteúdo no seu escritório principal ou em um local na nuvem.

### <a name="hcs1-in-the-branch-office"></a>HCS1 na filial

Você deve configurar este computador como um servidor de cache hospedado. Se você optar por prehash dados do servidor de conteúdo para que você pode pré-carregar o conteúdo em seus servidores de cache hospedado, você pode importar pacotes de dados que contêm o conteúdo de seus servidores Web e o arquivo.

### <a name="web1-in-the-cloud-data-center"></a>WEB1 na Central de dados na nuvem

WEB1 é um servidor de conteúdo BranchCache\ habilitado. Se você optar por prehash dados do servidor de conteúdo para que você pode pré-carregar o conteúdo em seus servidores de cache hospedado, você pode prehash o conteúdo compartilhado em WEB1 e criar um pacote de dados que você copiar para HCS1.

### <a name="file1-in-the-cloud-data-center"></a>Arquivo1 na Central de dados na nuvem

Arquivo1 é um servidor de conteúdo BranchCache\ habilitado. Se você optar por prehash dados do servidor de conteúdo para que você pode pré-carregar o conteúdo em seus servidores de cache hospedado, você pode prehash o conteúdo compartilhado em arquivo1 e criar um pacote de dados que você copiar para HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 na matriz

DC1 é um controlador de domínio, e você deve configurar a política de domínio padrão ou outra política que seja mais adequada para sua implantação, com as configurações de política de grupo BranchCache para ativar a descoberta de Cache hospedado automática por ponto de Conexão de serviço.

Quando computadores cliente na ramificação tiverem atualizado de política de grupo e esta configuração de política é aplicada, eles automaticamente localizem e começarem a usar o servidor de cache hospedado na filial.

### <a name="client-computers-in-the-branch-office"></a>Computadores cliente na filial

Você deve atualizar a política de grupo em computadores cliente para aplicar as novas configurações de política de grupo BranchCache e permitir que os clientes localizar e usar o servidor de cache hospedado.

## <a name="bkmk_overview"></a>Visão geral do processo hospedado Cache Server deployment

>[!NOTE]
>Os detalhes de como realizar essas etapas são fornecidos na seção [implantação do modo de Cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md).

O processo de implantação de um servidor de Cache hospedado BranchCache ocorre em desses estágios:

>[!NOTE]
>Algumas das etapas a seguir são opcionais, como essas etapas que demonstram como prehash e pré-carregar conteúdo em servidores de cache hospedado. Quando você implanta BranchCache no modo de cache hospedado, você não é necessária para prehash conteúdo em seu arquivo e Web servidores de conteúdo, para criar um pacote de dados e importar o pacote de dados para pré-carregar os servidores de cache hospedado com o conteúdo. As etapas são indicadas como opcionais nesta seção e na seção [implantação do modo de Cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md) para que você pode ignorá-los se você preferir.

1. Em HCS1, use comandos do Windows PowerShell para configurar o computador como um servidor de cache hospedado e registrar um ponto de Conexão de serviço no Active Directory.

2. \(Optional\) em HCS1, se os valores padrão de BranchCache não coincidem suas metas de implantação para o servidor e o cache hospedado, configure a quantidade de espaço em disco que você deseja alocar para o cache hospedado. Também defina o local do disco que você prefere para o cache hospedado.

3. \(Optional\) prehash conteúdo nos servidores de conteúdo, criar pacotes de dados e conteúdo de pré-carregamento no servidor do cache hospedado.

    > [!NOTE]
    > Prehashing e pré-Carregando conteúdo em seu servidor de cache hospedado é opcional, no entanto, se você optar por prehash e pré-carregamento, você deve executar todas as etapas abaixo que são aplicável a sua implantação. \ (Por exemplo, se você não tiver servidores Web, você não precisa realizar qualquer uma das etapas relacionadas a prehashing e pré-carregamento de conteúdo do servidor Web. \)

    1. No WEB1, prehash o conteúdo do servidor Web e crie um pacote de dados.

    2. Em arquivo1, prehash conteúdo de servidor de arquivos e criar um pacote de dados.

    3. Na WEB1 e arquivo1, copie os pacotes de dados para o servidor de cache hospedado HCS1.

    4. No HCS1, importe os pacotes de dados para pré-carregar o cache de dados.

4. Em DC1, configure ingressado no domínio cliente computadores filiais para o modo de cache hospedado por meio da política de grupo configuração com configurações de política BranchCache.

5. Em computadores cliente, atualize a política de grupo.

Para continuar com este guia, consulte [BranchCache hospedado Cache do modo de planejamento de implantação](3-Bc-Hcm-Plan.md).