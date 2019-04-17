---
title: Configurar o suporte para uma rede sem fio
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c5c98727b81bf37fdb3f90c612270462a51908c8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-support-for-a-wireless-network"></a>Configurar o suporte para uma rede sem fio

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode configurar o sistema operacional para dar suporte a uma rede sem fio. Os seguintes requisitos devem ser atendidos para habilitar o suporte sem fio no servidor:  
  
-   O servidor deve ter um adaptador de rede com fio instalado.  
  
-   O driver correto para o adaptador de rede sem fio deverá estar pré-instalado se o adaptador de rede não é suportado pelo sistema operacional.  
  
-   A capacidade de habilitar e desabilitar o adaptador de rede sem fio deve ser disponibilizada. Os métodos para fazer isso podem incluir um botão físico no servidor ou uma interface do usuário personalizada no painel. O manual do produto deve fornecer os etapas para habilitar e desabilitar o adaptador de rede sem fio.  
  
-   A capacidade de selecionar uma rede sem fio e se conectar a ela deve ser disponibilizada. Isso deve ser feito com a adição de uma interface do usuário personalizada para o painel. O manual do produto deve fornecer os etapas para selecionar e se conectar a uma rede sem fio.  
  
-   Se a capacidade de dar suporte a uma rede sem fio ad hoc for necessária, uma interface do usuário estendida no painel deve ser fornecida. A interface do usuário pode ser um botão ou um link que inicia a configurar um Assistente de rede sem fio Ad hoc no painel de controle no Windows Server 2008 R2.  
  
## <a name="additional-considerations"></a>Considerações adicionais  
 As seguintes informações também devem ser consideradas ao configurar o suporte para uma rede sem fio:  
  
-   O servidor deve estar conectado à rede com um cabo para executar a instalação.  
  
-   Um computador de rede no qual uma restauração bare-metal é realizada deve estar conectado à rede com um cabo.  
  
-   O servidor deve estar conectado à rede com um cabo para executar uma restauração bare-metal do servidor.  
  
-   Se uma rede ad hoc for criada no servidor, o adaptador de rede sem fio será dedicado para a rede ad-hoc para que o usuário deve sempre conecte o cabo de rede no servidor para obter uma conexão de Internet.  
  
> [!NOTE]
>  Para obter mais informações sobre como configurar conexões de rede, consulte [pré-configuração de um roteador](Preconfiguring-a-Router.md).  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)