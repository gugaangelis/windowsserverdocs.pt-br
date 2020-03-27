---
title: Gerenciar recursos em várias florestas do Active Directory
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9a3e58a3ca8ca5334996776644bfb2c3b19cbc2e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309594"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gerenciar recursos em várias florestas do Active Directory

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a usar o IPAM para gerenciar controladores de domínio, servidores DHCP e servidores DNS em várias florestas de Active Directory.  
  
Para usar o IPAM para gerenciar recursos em florestas de Active Directory remota, cada floresta que você deseja gerenciar deve ter uma relação de confiança bidirecional com a floresta em que o IPAM está instalado.  
  
Para iniciar o processo de descoberta para diferentes Active Directory florestas, abra Gerenciador do Servidor e clique em IPAM. No console do cliente IPAM, clique em **Configurar descoberta de servidor**e em **obter florestas**. Isso inicia uma tarefa em segundo plano que descobre florestas confiáveis e seus domínios. Após a conclusão do processo de descoberta, clique em **Configurar descoberta de servidor**, que abre a caixa de diálogo a seguir.  
  
![Configurar descoberta de servidor](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Para o provisionamento baseado em Política de Grupo\-para um cenário de Active Directory entre florestas, certifique-se de executar o seguinte cmdlet do Windows PowerShell no servidor IPAM e não nos controladores de domínio confiantes. Por exemplo, se o servidor IPAM estiver ingressado na floresta corp.contoso.com e a floresta confiante for fabrikam.com, você poderá executar o seguinte cmdlet do Windows PowerShell no servidor IPAM no corp.contoso.com para o provisionamento baseado em Política de Grupo\-na floresta fabrikam.com. Para executar esse cmdlet, você deve ser membro do grupo Admins. do domínio na floresta fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

Na caixa de diálogo **Configurar descoberta de servidor** , clique em **selecionar a floresta**e escolha a floresta que você deseja gerenciar com o IPAM. Selecione também os domínios que você deseja gerenciar e clique em **Adicionar**.

Em **selecionar as funções de servidor para descobrir**, para cada domínio que você deseja gerenciar, especifique o tipo de servidores a serem descobertos. As opções são **controlador de domínio**, **servidor DHCP**e **servidor DNS**.

Por padrão, os controladores de domínio, servidores DHCP e servidores DNS são descobertos. portanto, se você não quiser descobrir um desses tipos de servidores, certifique-se de desmarcar a caixa de seleção dessa opção.

Na ilustração de exemplo acima, o servidor IPAM é instalado na floresta contoso.com e o domínio raiz da floresta fabrikam.com é adicionado para gerenciamento de IPAM. As funções de servidor selecionadas permitem que o IPAM descubra e gerencie controladores de domínio, servidores DHCP e servidores DNS no domínio raiz fabrikam.com e no domínio raiz contoso.com.

Depois de especificar florestas, domínios e funções de servidor, clique em **OK**. O IPAM executa a descoberta e, quando a descoberta é concluída, você pode gerenciar recursos na floresta local e remota.
