---
title: Gerenciar recursos em várias florestas do Active Directory
description: Este tópico faz parte do guia de gerenciamento de gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b01680d4b35461ff85965781ebc60e7a613d1cb8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gerenciar recursos em várias florestas do Active Directory

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como usar IPAM para gerenciar controladores de domínio, servidores DHCP e DNS servidores em várias florestas do Active Directory.  
  
Para usar IPAM para gerenciar recursos em florestas do Active Directory remotos, a cada floresta que você deseja gerenciar deve ter um dois a maneira confiável com a floresta onde IPAM está instalado.  
  
Para iniciar o processo de descoberta de florestas do Active Directory diferentes, abra o Gerenciador do servidor e clique em IPAM. No console do cliente IPAM, clique em **configurar servidor descoberta**e clique em **obter florestas**. Isso inicia uma tarefa em segundo plano que detecta florestas confiáveis e seus domínios. Após concluir o processo de descoberta, clique em **configurar servidor descoberta**, que abre a caixa de diálogo a seguir.  
  
![Configurar a descoberta de servidor](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Com base em política de senha \ grupo provisionamento de um cenário de floresta do Active Directory cruzada, certifique-se de que você execute o seguinte cmdlet do Windows PowerShell no servidor IPAM e não no domínio confiante controladores de domínio. Por exemplo, se o servidor IPAM está associado à floresta corp.contoso.com e a floresta confiante é fabrikam.com, você pode executar o seguinte cmdlet do Windows PowerShell no servidor IPAM na corp.contoso.com para provisionamento baseado em política de senha \ grupo na floresta do fabrikam.com. Para executar este cmdlet, você deve ser um membro do grupo Admins. do domínio na floresta fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

No **configurar servidor descoberta** caixa de diálogo, clique em **selecione floresta**e escolha a que você deseja gerenciar com IPAM floresta. Selecione também os domínios que você deseja gerenciar e, em seguida, clique em **adicionar**.

Em **selecione as funções de servidor para descobrir**, para cada domínio que você deseja gerenciar, especifique o tipo de servidores para descobrir. As opções são **controlador de domínio**, **servidor DHCP**, e **servidor DNS**.

Por padrão, controladores de domínio, servidores DHCP e DNS servidores são descobertos - portanto se você não quiser encontrar um desses tipos de servidores, certifique-se de que você desmarca a caixa de seleção para essa opção.

Na ilustração exemplo acima, o servidor IPAM é instalado na floresta contoso.com e o domínio raiz da floresta fabrikam.com é adicionado para gerenciamento de IPAM. As funções de servidor selecionado permitem IPAM descobrir e gerenciar controladores de domínio, servidores DHCP e servidores DNS no domínio raiz fabrikam.com e domínio raiz contoso.com.

Depois que você especificou florestas, domínios e funções de servidor, clique em **Okey**. IPAM realiza descoberta, e quando descoberta é concluída, você pode gerenciar recursos da floresta local e remoto.
