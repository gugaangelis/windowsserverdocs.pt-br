---
title: Implantar perfeitamente RDS com ARM e Azure Marketplace
description: Saiba como criar uma pequena implantação de RDS no Azure usando modelos ARM e o Azure Marketplace.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f72ceb6-6f90-48f6-bfc3-bdad63984ce7
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 02/10/2017
ms.openlocfilehash: e4de3d4ac14a0dbc5500fd7ab8bd8f1568f3da53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823077"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Implantar perfeitamente RDS com ARM e Azure Marketplace

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2

Serviços de área de trabalho remota (RDS) é a plataforma ideal para hospedar uma maneira econômica de aplicativos e áreas de trabalho do Windows. Você pode usar um [oferta do Azure Marketplace](#basic-rds-through-the-azure-marketplace) ou um [modelo de início rápido](#Customized-RDS-using-Quickstart-templates) para criar rapidamente um RDS na implantação de IaaS do Azure. O Azure marketplace cria um domínio de teste para você, tornando-o um mecanismo simple e fácil para teste e à prova de conceito. Os modelos de início rápido, por outro lado, permitem que você use um domínio existente, tornando-os uma ótima ferramenta para criar um ambiente de produção. Uma vez configurado, você pode conectar as áreas de trabalho publicadas e os aplicativos de várias plataformas e dispositivos, usando os aplicativos de área de trabalho remota Microsoft para Windows, Mac, iOS e Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>Básica do RDS por meio do Marketplace do Azure

Criando a implantação por meio do Marketplace do Azure é a maneira mais rápida de começar a trabalhar e em execução. Quando tudo estiver concluído, o seu ambiente será semelhante a [arquitetura RDS básica](desktop-hosting-logical-architecture.md#basic-deployment). A oferta cria todos os componentes RDS que você precisa, tudo o que você precisa fazer é fornecer algumas informações. 

Você precisará fornecer as informações a seguir quando você implantar a oferta do Marketplace:
- Nome de usuário administrador e senha. Isso é um novo usuário que irá gerenciar a implantação.
- Nome DNS e nome de domínio do AD. Esses são os novos recursos que são criados. Verifique se que os nomes são significativos.
- Tamanho da VM. Você pode escolher o tamanho das VMs a ser usado para os pontos de extremidade RDSH. Manualmente, você pode alterar os tamanhos após a implantação inicial para ajudá-lo a otimizar as VMs para suas cargas de trabalho e custo.

Use estas etapas para criar sua implantação do RDS de superfície pequena do Azure Marketplace: 

1. Inicie a implantação de RDS do Azure Marketplace:
   1. Entrar a [portal do Azure](https://portal.azure.com).
   2. Clique em **New** adicionar a implantação.
   3. Digite "RDS" no campo de pesquisa e pressione Enter.
   4. Clique em **serviços de área de trabalho remota (RDS) – Basic – desenvolvimento/teste**e, em seguida, clique em **criar**.
   5. Siga as etapas no portal para criar e implantar RDS. Você adicionará os detalhes de configuração de chave, como as informações listadas acima. 
2. Conecte-se à sua implantação. Quando a implantação for concluída, verifique a seção de saídas para as etapas finais concluir e conecte-se à sua implantação.
   1. Baixe e execute [este script do PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) em seu dispositivo de teste para instalar todos os certificados necessários para conectar-se à implantação do RDS. 
   
      Essa etapa só é necessária durante a fase de teste. Quando você implanta o RDS no Azure em produção, certifique-se de seguir as práticas recomendadas, como comprar e usar um certificado SSL confiável publicamente em seus servidores web.

   2. Quando solicitado, entre em sua conta do Azure. Selecione a assinatura do Azure, o grupo de recursos e o endereço IP público criado para essa nova implantação.
   3. Quando o script for concluído, a página da Web da área de trabalho remota é iniciado no navegador padrão. Você pode verificar a página da Web da área de trabalho remota, comparando a URL da página para o endereço DNS fornecido durante a implantação. 
   
      Entrar com as credenciais de administrador criada durante a implantação para ver a área de trabalho padrão publicada para você. Você também pode enviar aos usuários o site da Web da área de trabalho remota para testar seus aplicativos e áreas de trabalho.

      > [!TIP]
      > Esquecer o usuário administrador ou o nome de domínio? Você pode ir para o novo grupo de recursos no portal, clique em **implantações**e, em seguida, exibir os parâmetros que você inseriu.

Agora que você tem uma implantação do RDS, você pode [adicionar e gerenciar usuários](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>RDS personalizados usando modelos de início rápido

Você pode usar modelos do Azure Resource Manager para implantar RDS no Azure. Isso é especialmente útil se você quiser uma implantação de RDS básica, mas tem componentes existentes (como AD) que você deseja usar. Ao contrário do mercado a oferecer, você pode fazer personalizações adicionais, como o uso de um AD existente em uma rede virtual, usando uma imagem personalizada do sistema operacional para VMs de RDSH e a disposição em camadas sobre a alta disponibilidade para componentes RDS. Depois de adicionar a alta disponibilidade para cada componente, seu ambiente será semelhante a [arquitetura do RDS altamente disponível](desktop-hosting-logical-architecture.md#highly-available-deployment).

Use estas etapas para criar sua implantação do RDS de superfície pequena com um modelo de RDS do Azure: 

1. Escolha seu modelo de início rápido do Azure:
   1. Vá para o [RDS Azure Quickstart Templates](https://aka.ms/rdautomation) site.
   2. Escolha o modelo que corresponda ao qual você está tentando fazer. Verifique se que você atende aos pré-requisitos para esse modelo específico. (Por exemplo, se desejar usar uma imagem personalizada para suas VMs, verifique se que você já carregou essa imagem para uma conta de armazenamento do Azure.)
   3. Clique em **implantar no Azure**.
   4. Você precisará fornecer alguns detalhes (como o nome de usuário administrador, o nome de domínio do AD) no portal do Azure. Isso varia de acordo com o modelo que você escolher.
   5. Clique em **compra**.
2. Conecte-se à sua implantação. 
   1. Baixe e execute [este script do PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) em seu dispositivo de teste para instalar todos os certificados necessários para conectar-se à implantação do RDS. 
   
      Essa etapa só é necessária durante a fase de teste. Quando você implanta o RDS no Azure em produção, certifique-se de seguir as práticas recomendadas, como comprar e usar um certificado SSL confiável publicamente em seus servidores web.

   2. Quando solicitado, entre em sua conta do Azure. Selecione a assinatura do Azure, o grupo de recursos e o endereço IP público criado para essa nova implantação.
   3. Quando o script for concluído, a página da Web da área de trabalho remota é iniciado no navegador padrão. Você pode verificar a página da Web da área de trabalho remota, comparando a URL da página para o endereço DNS fornecido durante a implantação. 
   
      Entrar com as credenciais de administrador criada durante a implantação para ver a área de trabalho padrão publicada para você. Você também pode enviar aos usuários o site da Web da área de trabalho remota para testar seus aplicativos e áreas de trabalho.

      > [!TIP]
      > Esquecer o usuário administrador ou o nome de domínio? Você pode ir para o novo grupo de recursos no portal, clique em **implantações**e, em seguida, exibir os parâmetros que você inseriu.

Agora que você tem uma implantação do RDS, você pode [adicionar e gerenciar usuários](rds-user-management.md).
