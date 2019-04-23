---
title: Visão geral da implantação do modo de cache hospedado BranchCache
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 930a9b4872a7a79351055841a5d716dd99df0fa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829507"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Visão geral da implantação do modo de cache hospedado BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este guia para implantar um servidor de cache hospedado do BranchCache em uma filial em que os computadores estão associados a um domínio. Você pode usar este tópico para obter uma visão geral do processo de implantação modo de Cache hospedado do BranchCache.

Esta visão geral inclui a infraestrutura do BranchCache que você precisa, bem como uma simples visão geral passo a passo de implantação.

## <a name="bkmk_components"></a>Infraestrutura de implantação de servidor de Cache hospedada

Nessa implantação, o servidor de cache hospedado é implantado usando pontos de conexão de serviço no Active Directory Domain Services \(AD DS\), e você tem a opção com o BranchCache no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, efetuar o prehash do conteúdo compartilhado na Web e de arquivos com base em servidores de conteúdo, em seguida, pré-carregar o conteúdo em servidores de cache hospedado.

A ilustração a seguir mostra a infraestrutura que é necessário para implantar um servidor de cache hospedado do BranchCache.

![Visão geral do modo de Cache hospedado do BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Embora essa implantação descreve os servidores de conteúdo de um data center na nuvem, você pode usar este guia para implantar um servidor de cache hospedado do BranchCache, independentemente de onde implantar seus servidores de conteúdo no seu escritório principal ou em um local de nuvem.

### <a name="hcs1-in-the-branch-office"></a>HCS1 na filial

Você deve configurar este computador como um servidor de cache hospedado. Se você optar por pré-hash dados do servidor de conteúdo, de modo que você pode pré-carregar o conteúdo em seus servidores de cache hospedado, você pode importar pacotes de dados que contêm o conteúdo de seus servidores Web e de arquivos.

### <a name="web1-in-the-cloud-data-center"></a>WEB1 no Centro de dados de nuvem

WEB1 é um BranchCache\-habilitado o servidor de conteúdo. Se você optar por pré-hash dados do servidor de conteúdo, de modo que você pode pré-carregar o conteúdo em seus servidores de cache hospedado, você pode prehash do conteúdo compartilhado no WEB1, e criar um pacote de dados que você copia HCS1.

### <a name="file1-in-the-cloud-data-center"></a>File1 no Centro de dados de nuvem

File1 é um BranchCache\-habilitado o servidor de conteúdo. Se você optar por pré-hash dados do servidor de conteúdo, de modo que você pode pré-carregar o conteúdo em seus servidores de cache hospedado, você pode prehash do conteúdo compartilhado no FILE1 e crie um pacote de dados que você copiar para HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 no escritório principal

DC1 é um controlador de domínio, e você deve configurar a política de domínio padrão ou outra política que é mais apropriada para sua implantação, com as configurações de diretiva de grupo do BranchCache para habilitar a descoberta de Cache hospedado automática pelo ponto de Conexão de serviço.

Quando os computadores cliente na filial tiverem atualizado de diretiva de grupo e essa configuração de política é aplicada, eles automaticamente localizarem e começarem a usar o servidor de cache hospedado na filial.

### <a name="client-computers-in-the-branch-office"></a>Computadores cliente na filial

Você deve atualizar a diretiva de grupo em computadores cliente para aplicar novas configurações de diretiva de grupo do BranchCache e para permitir que os clientes localizar e usar o servidor de cache hospedado.

## <a name="bkmk_overview"></a>Visão geral de processo da implantação servidor de Cache hospedado

>[!NOTE]
>Os detalhes de como executar essas etapas são fornecidos na seção [implantação do modo de Cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md).

O processo de implantação de um servidor de Cache hospedado do BranchCache ocorre nesses estágios:

>[!NOTE]
>Algumas das etapas a seguir são opcionais, como aquelas etapas que demonstram como pré-hash e pré-carregar conteúdo em servidores de cache hospedado. Quando você implanta o BranchCache no modo de cache hospedado, não é necessário para o conteúdo prehash em sua Web e o arquivo de servidores de conteúdo, para criar um pacote de dados e para importar o pacote de dados para pré-carregar seus servidores de cache hospedado com conteúdo. As etapas são indicadas como opcionais nesta seção e na seção [implantação do modo de Cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md) para que você possa ignorá-los se você preferir.

1. Em HCS1, use comandos do Windows PowerShell para configurar o computador como um servidor de cache hospedado e registrar um ponto de Conexão de serviço no Active Directory.

2. \(Opcional\) em HCS1, se os valores padrão de BranchCache não coincidem com as metas de implantação do servidor e de cache hospedado, configure a quantidade de espaço em disco que você deseja alocar para o cache hospedado. Também configure o local do disco que você prefira para o cache hospedado.

3. \(Opcional\) pré-hash do conteúdo nos servidores de conteúdo, criar pacotes de dados e pré-carregar conteúdo no servidor de cache hospedado.

    > [!NOTE]
    > Realizar o hash prévio e pré-Carregando conteúdo do servidor de cache hospedado é opcional, no entanto, se você escolher efetuar o prehash e pré-carregamento, você deve executar todas as etapas abaixo que são aplicável à sua implantação. \(Por exemplo, se você não tiver servidores Web, você não precisará realizar qualquer uma das etapas relacionadas ao realizar o hash prévio e o pré-carregamento de conteúdo do servidor Web.\)

    1. No WEB1, pré-hash do conteúdo do servidor Web e criar um pacote de dados.

    2. No FILE1, pré-hash do conteúdo do servidor de arquivo e crie um pacote de dados.

    3. A partir do WEB1 e FILE1, copie os pacotes de dados para o servidor de cache hospedado HCS1.

    4. HCS1, importe os pacotes de dados para pré-carregar o cache de dados.

4. No DC1, configure ingressado no domínio branch office computadores de cliente para o modo de cache hospedado por meio da configuração de diretiva de grupo com configurações de política do BranchCache.

5. Em computadores cliente, atualize a diretiva de grupo.

Para continuar com este guia, consulte [BranchCache Hosted Cache de modo de planejamento de implantação](3-Bc-Hcm-Plan.md).