---
title: Gerenciar recursos em várias florestas do Active Directory
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
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
ms.openlocfilehash: c6752d87be2e689e517b287092a2570e27943c18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815967"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gerenciar recursos em várias florestas do Active Directory

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a usar o IPAM para gerenciar controladores de domínio, servidores DHCP e servidores DNS em várias florestas do Active Directory.  
  
Para usar o IPAM para gerenciar recursos em florestas remotas do Active Directory, a cada floresta que você deseja gerenciar deve ter um de dois a confiança mútua com a floresta em que o IPAM é instalado.  
  
Para iniciar o processo de descoberta diferentes florestas do Active Directory, abra o Gerenciador do servidor e clique em IPAM. No console de cliente IPAM, clique em **configurar a descoberta de servidor**e, em seguida, clique em **obter florestas**. Isso inicia uma tarefa em segundo plano que descobre os seus domínios e florestas confiáveis. Depois de concluir o processo de descoberta, clique em **configurar a descoberta de servidor**, que abre a caixa de diálogo a seguir.  
  
![Configurar descoberta de servidor](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Política de grupo\-com a base de provisionamento para um cenário de diretório entre florestas do Active Directory, certifique-se de que você execute o seguinte cmdlet do Windows PowerShell no servidor IPAM e não no domínio confiante controladores de domínio. Por exemplo, se o servidor IPAM está associado ao corp.contoso.com floresta e a floresta confiante é fabrikam.com, você pode executar o seguinte cmdlet do Windows PowerShell no servidor IPAM no corp.contoso.com para diretiva de grupo\-com base em provisionamento no floresta de Fabrikam.com. Para executar este cmdlet, você deve ser um membro do grupo Admins. do domínio na floresta de fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

No **configurar a descoberta de servidor** caixa de diálogo, clique em **selecione a floresta**e, em seguida, escolha a floresta que você deseja gerenciar com o IPAM. Selecione também os domínios que você deseja gerenciar e, em seguida, clique em **adicionar**.

Na **selecione as funções de servidor para descobrir**, para cada domínio que você deseja gerenciar, especifique o tipo de servidores a serem descobertos. As opções são **controlador de domínio**, **servidor DHCP**, e **servidor DNS**.

Por padrão, controladores de domínio, servidores DHCP e servidores DNS são descobertos – portanto, se você não quiser descobrir um desses tipos de servidores, certifique-se de que você desmarque a caixa de seleção para essa opção.

A ilustração de exemplo acima, o servidor IPAM é instalado na floresta contoso.com e o domínio raiz da floresta fabrikam.com é adicionado para o gerenciamento do IPAM. As funções de servidor selecionado permitir que o IPAM descobrir e gerenciar controladores de domínio, servidores DHCP e servidores DNS no domínio fabrikam.com raiz e o domínio de raiz de contoso.com.

Após você ter especificado as florestas, domínios e funções de servidor, clique em **Okey**. O IPAM executa a descoberta e quando a descoberta estiver concluída, você pode gerenciar recursos na floresta local e remoto.
