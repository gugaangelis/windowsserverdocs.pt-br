---
title: Implantação do certificado de servidor
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a10a9bafa6a8c9fddecac799ec8e837bf339d0e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment"></a>Implantação do certificado de servidor

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Siga estas etapas para instalar uma autoridade de certificação raiz corporativa (CA) e para implantar certificados de servidor para uso com o PEAP, EAP.  
  
> [!IMPORTANT]  
> Antes de instalar os serviços de certificados do Active Directory, você deve nomear o computador, configurar o computador com um endereço IP estático e ingressar o computador ao domínio. Depois de instalar o AD CS, você não pode alterar o nome do computador ou a participação no domínio do computador, no entanto, você pode alterar o endereço IP, se necessário. Para obter mais informações sobre como realizar essas tarefas, consulte o Windows Server&reg; 2016 [guia da rede principal](../../Core-Network-Guide.md).  

  
-   [Instalar o WEB1 de servidor da Web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Criar um registro de alias (CNAME) no DNS para WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurar WEB1 para distribuir listas de certificados revogados (CRLs)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Preparar o arquivo inf CAPolicy](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Instale uma autoridade de certificação](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurar as extensões CDP e AIA no CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copie o certificado da CA e CRL para o diretório virtual](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurar o modelo de certificado do servidor](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurar o registro automático de certificado de servidor](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Política de grupo de atualização](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Verifique se o servidor de registro de um certificado de servidor](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Os procedimentos neste guia não inclua instruções para casos em que o **User Account Control** abre a caixa de diálogo para solicitar sua permissão para continuar. Se essa caixa de diálogo Abrir enquanto você estiver executando os procedimentos neste guia e se a caixa de diálogo estava aberta em resposta a ações, clique em **continuar**.  
  


