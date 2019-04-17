---
title: "Pré-configuração de um roteador"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7dc66c8a439552c2087d0348b0115adba04027ee
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="preconfiguring-a-router"></a>Pré-configuração de um roteador

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Normalmente, uma nova instalação do sistema operacional requer um roteador capaz de Internet e o firewall para se conectar a rede interna do cliente com a Internet. Se você fornecer um roteador como um valor adicional com um servidor pré-configurado, você pode executar etapas adicionais para pré-configurar o roteador para proporcionar uma melhor experiência de usuário.  
  
 O roteador deve ter DHCP ativado. O servidor deve ser atribuído a um endereço IP estático. Você pode fazer isso por reserva de DHCP de um endereço IP ou atribuindo um endereço IP que esteja fora do intervalo de endereços DHCP.  
  
 Você também deve pré-configurar a configurações no roteador para encaminhar portas específicas da interface externa do roteador para o endereço do servidor na rede interna de encaminhamento de porta. A tabela a seguir lista a configuração recomendada.  
  
|Configuração|Detalhes|  
|---------------------------|-------------|  
|DHCP|Em|  
|Encaminhamento de porta|Você deve encaminhar as seguintes portas para o endereço do servidor:<br /><br /> -80 (para configuração hospedada, use apenas 443)<br />-   443|  
|Suporte a UPnP|Você deve habilitar o suporte a ¢ UPnP fornecer a configuração de roteador mais fácil para o cliente e a melhor experiência do cliente durante a instalação.<br /><br /> **Aviso:** a arquitetura UPnP pode representar um risco de segurança se ele foi deixado habilitado.|  
  
 Além das configurações de pré-configuração do roteador básico, você pode concluir as seguintes tarefas para proporcionar uma experiência de usuário mais integrada para gerenciar o roteador:  
  
-   Estenda o painel fornecendo um suplemento no servidor que permite que os usuários a gerenciar o roteador por meio de uma interface do usuário personalizada.  
  
-   Estenda os alertas de integridade para que qualquer alerta do roteador pode ser vista no Visualizador de alerta.  
  
-   Se o roteador der suporte a várias sub-redes, o endereço IP do servidor deve ser distribuímos como um servidor DNS por meio de DHCP.  
  
-   Se o roteador tem um recurso de controle de acesso integrado para serviços de domínio do Active DirectoryÂ®, você pode automatizar a integração do Active Directory durante a configuração inicial do servidor. Você também deve expor esse recurso por meio do roteador gerenciamento suplemento no painel.  
  
> [!NOTE]
>  Para obter mais informações sobre como configurar conexões sem fio, consulte [configurar o suporte para uma rede sem fio](Configure-Support-for-a-Wireless-Network.md).  
  
## <a name="see-also"></a>Consulte também  
 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)