---
title: Implantação de certificados de servidor
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 751c5c5958b3d06ae0f4b701e4d6e10a7fef19dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858487"
---
# <a name="server-certificate-deployment"></a>Implantação de certificados de servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Siga estas etapas para instalar uma autoridade de certificação raiz corporativa (CA) e implantar certificados de servidor para uso com o PEAP e EAP.  
  
> [!IMPORTANT]  
> Antes de instalar os serviços de certificados do Active Directory, nome do computador, configure o computador com um endereço IP estático e ingressar o computador no domínio. Depois de instalar o AD CS, você não pode alterar o nome do computador ou a associação de domínio do computador, no entanto, você pode alterar o endereço IP, se necessário. Para obter mais informações sobre como realizar essas tarefas, consulte o Windows Server&reg; 2016 [guia da rede principal](../../Core-Network-Guide.md).  

  
-   [Instalar o Web Server WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Criar um registro de alias (CNAME) no DNS para o WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurar o WEB1 para distribuir a listas de certificados revogados (CRLs)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Preparar o arquivo CAPolicy inf](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Instalar a autoridade de certificação](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurar as extensões AIA e CDP no CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copie o certificado de autoridade de certificação e a CRL para o diretório virtual](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurar o modelo de certificado do servidor](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurar o registro automático de certificado do servidor](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Atualizar diretiva de grupo](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Verificar o registro do servidor de um certificado de servidor](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Os procedimentos deste guia não incluem instruções para os casos em que a caixa de diálogo **Controle de Conta de Usuário** é aberta para solicitar sua permissão para continuar. Caso essa caixa de diálogo seja aberta durante a execução dos procedimentos deste guia e em resposta às suas ações, clique em **Continuar**.  
  


