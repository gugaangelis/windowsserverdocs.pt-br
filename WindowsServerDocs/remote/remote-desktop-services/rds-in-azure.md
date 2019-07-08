---
title: Implantar o RDS de forma contínua com o ARM e o Azure Marketplace
description: Saiba como criar uma pequena implantação de RDS no Azure usando modelos de ARM e o Azure Marketplace.
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
ms.openlocfilehash: 218e61e5ebe110502ebe139b27607bfeff104fde
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712623"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Implantar o RDS de forma contínua com o ARM e o Azure Marketplace

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

O RDS (Serviços de Área de Trabalho Remota) é a plataforma ideal para hospedar de maneira econômica aplicativos e áreas de trabalho do Windows. Você pode usar uma [oferta do Azure Marketplace](#basic-rds-through-the-azure-marketplace) ou um [modelo de início rápido](#customized-rds-using-quickstart-templates) para criar rapidamente uma implantação IaaS do RDS no Azure. O Azure Marketplace cria um domínio de teste para você, fazendo dele um mecanismo simples e fácil para testes e prova de conceito. Os modelos de início rápido, por outro lado, permitem usar um domínio existente, o que faz deles uma ótima ferramenta para criar um ambiente de produção. Após configurar, você pode se conectar às áreas de trabalho e aplicativos publicados de várias plataformas e dispositivos, usando os aplicativos de Área de Trabalho Remota da Microsoft para Windows, Mac, iOS e Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>RDS básico usando o Azure Marketplace

Criar a implantação usando o Azure Marketplace é a maneira mais rápida de colocar tudo em funcionamento. Quando tudo estiver concluído, seu ambiente será semelhante à [arquitetura de RDS básica](desktop-hosting-logical-architecture.md#basic-deployment). A oferta cria todos os componentes de RDS necessários. Você precisa apenas fornecer algumas informações. 

Você precisará fornecer as informações a seguir quando implantar a oferta do Marketplace:
- Nome de usuário e senha de administrador. Trata-se de um novo usuário que gerenciará a implantação.
- Nome DNS e nome de domínio do AD. São os NOVOS recursos que são criados. Certifique-se de que os nomes sejam significativos.
- Tamanho da VM. Você pode escolher o tamanho das VMs a serem usadas para os pontos de extremidade de RDSH. Você pode também pode alterar os tamanhos manualmente após a implantação inicial para ajudá-lo a otimizar as VMs para suas cargas de trabalho e custo.

Use estas etapas para criar sua implantação do RDS de volume pequeno no Azure Marketplace: 

1. Iniciar a implantação de RDS do Azure Marketplace:
   1. Entre no [portal do Azure](https://portal.azure.com).
   2. Clique em **Novo** para adicionar a implantação.
   3. Digite "RDS" no campo de pesquisa e pressione Enter.
   4. Clique em **Serviços de Área de Trabalho Remota (RDS) – Básico – Desenvolvimento/Teste** e, em seguida, clique em **Criar**.
   5. Siga as etapas no portal para criar e implantar o RDS. Você adicionará os detalhes importantes da configuração, como as informações listadas acima. 
2. Conecte-se à sua implantação. Quando a implantação for concluída, verifique na seção de saídas as etapas finais para concluir e conectar-se à sua implantação.
   1. Baixe e execute [este script do PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) em seu dispositivo de teste para instalar os certificados necessários para conectar-se à implantação do RDS. 
   
      Esta etapa só é necessária durante a fase de teste. Quando implantar o RDS no Azure em produção, não deixe de seguir as práticas recomendadas, como comprar e usar um certificado SSL confiável publicamente em seus servidores Web.

   2. Quando solicitado, faça logon na conta do Azure. Selecione a assinatura do Azure, o grupo de recursos e o endereço IP público criados para essa nova implantação.
   3. Quando o script for concluído, a página da Web da área de trabalho remota será iniciada no navegador padrão. Você pode verificar a página da Web da área de trabalho remota comparando a URL da página com o endereço DNS fornecido durante a implantação. 
   
      Entre usando as credenciais de administrador criadas durante a implantação para ver a área de trabalho padrão publicada para você. Você também pode enviar aos usuários o site da área de trabalho remota para testar os aplicativos e áreas de trabalho deles.

      > [!TIP]
      > Esqueceu o usuário administrador ou o nome de domínio? Você pode voltar para o novo Grupo de recursos no portal, clique em **Implantações** e, em seguida, exiba os parâmetros inseridos.

Agora que tem uma implantação do RDS, você pode [adicionar e gerenciar usuários](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>RDS personalizado usando modelos de início rápido

Você pode usar modelos do Azure Resource Manager para implantar o RDS no Azure. Isso é especialmente útil quando você quer uma implantação de RDS básica, mas tem componentes (como o AD) que deseja usar. Diferente da oferta do Marketplace, você pode fazer mais personalizações, como usar o AD existente em uma rede virtual, usar uma imagem do sistema operacional personalizada para VMs de RDSH e proporcionar alta disponibilidade para componentes do RDS. Depois de adicionar alta disponibilidade para cada componente, seu ambiente será semelhante à [arquitetura de RDS altamente disponível](desktop-hosting-logical-architecture.md#highly-available-deployment).

Use estas etapas para criar sua implantação do RDS de volume pequeno com um modelo de RDS do Azure: 

1. Escolha seu modelo de início rápido do Azure:
   1. Vá para o site dos [Modelos de Início Rápido de RDS do Azure](https://aka.ms/rdautomation).
   2. Escolha o modelo que corresponde ao que você está tentando fazer. Verifique se você atende aos pré-requisitos do modelo específico. (Por exemplo, se quiser usar uma imagem personalizada para suas VMs, verifique se que você já carregou essa imagem para uma conta de armazenamento do Azure.)
   3. Clique em **Implantar no Azure**.
   4. Você precisará fornecer alguns detalhes (como o nome do usuário administrador, o nome de domínio do AD) no portal do Azure. Isso varia de acordo com o modelo escolhido.
   5. Clique em **Comprar**.
2. Conecte-se à sua implantação. 
   1. Baixe e execute [este script do PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) em seu dispositivo de teste para instalar os certificados necessários para conectar-se à implantação do RDS. 
   
      Esta etapa só é necessária durante a fase de teste. Quando implantar o RDS no Azure em produção, não deixe de seguir as práticas recomendadas, como comprar e usar um certificado SSL confiável publicamente em seus servidores Web.

   2. Quando solicitado, faça logon na conta do Azure. Selecione a assinatura do Azure, o grupo de recursos e o endereço IP público criados para essa nova implantação.
   3. Quando o script for concluído, a página da Web da área de trabalho remota será iniciada no navegador padrão. Você pode verificar a página da Web da área de trabalho remota comparando a URL da página com o endereço DNS fornecido durante a implantação. 
   
      Entre usando as credenciais de administrador criadas durante a implantação para ver a área de trabalho padrão publicada para você. Você também pode enviar aos usuários o site da área de trabalho remota para testar os aplicativos e áreas de trabalho deles.

      > [!TIP]
      > Esqueceu o usuário administrador ou o nome de domínio? Você pode voltar para o novo Grupo de recursos no portal, clique em **Implantações** e, em seguida, exiba os parâmetros inseridos.

Agora que tem uma implantação do RDS, você pode [adicionar e gerenciar usuários](rds-user-management.md).
