---
title: Antes de instalar o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e7eb1b7bed780b41f1a87589add4ab015f41624a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="before-you-install-windows-server-essentials"></a>Antes de instalar o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a>Antes de começar a instalação do Windows Server Essentials, execute as seguintes tarefas:  

-   **Certifique-se de que seu computador atende aos requisitos mínimos de hardware**. Isso inclui determinar se você precisa de hardware adicional e verificar que os drivers para seu hardware são suportados pelo Windows Server Essentials. Para obter mais informações, consulte [requisitos de sistema para o Windows Server Essentials](../get-started/system-requirements.md).   

  
    > [!IMPORTANT]
    >  Antes de instalar o Windows Server Essentials em um computador preexistente, recomendamos que você totalmente Formatar e, em seguida, reparticionar os discos rígidos do computador preexistente. Formatar e reparticionar os discos rígidos, você remover a possibilidade de que permanecem ocultas partições nos discos rígidos.  
  
-   **Preparar sua rede** para preparar sua rede para instalar o Windows Server Essentials, faça o seguinte:  
    
  
    -   **Atualizar o sistema operacional em computadores cliente** Windows Server Essentials suporta os seguintes sistemas operacionais: Windows 8, Windows 7, Windows 10 e Macintosh OS X Lion ou superior. Esses sistemas operacionais fornecer os recursos de segurança necessário, a confiabilidade, o desempenho e a funcionalidade para a rede local.  
  
    -   **Configurar seu roteador** verificar se o roteador está configurado da seguinte maneira:  
  
        -   A estrutura UPnP está habilitada no seu roteador.  
  
        -   O serviço de Dynamic Host Configuration servidor DHCP (protocolo) da LAN pode ser ativado ou desabilitado.  Windows Server Essentials garante que o DHCP não está executando no servidor e o roteador? Quando DHCP está habilitado no roteador, DHCP não está habilitado no servidor durante a instalação.  
  
        -   Você tem um endereço IP para a interface externa do roteador, que é fornecido pelo seu provedor de serviços de Internet (ISP). O endereço IP pode ser atribuído dinamicamente pelo serviço de servidor DHCP no seu ISP ou configure manualmente um endereço IP estático usando o console de gerenciamento do roteador.  
  
        -   Se sua conexão de Internet requer um nome de usuário e senha, também chamada de protocolo ponto a ponto pela Ethernet (PPPoE), essas configurações são definidas no roteador, mesmo se o dispositivo dá suporte a estrutura UPnP.  
  
        -   O roteador está conectado à rede local e à Internet, ele é ativado e ele está funcionando corretamente.  
  
     Se o roteador não der suporte a estrutura UPnP, ou se o roteador não pode ser configurado durante a instalação, você deve configurá-la com as configurações manualmente para sua rede. Certifique-se de que as seguintes portas estejam abertas e direcionadas para o endereço IP do servidor de destino:  
  
    |Número da porta|Aplicativo|  
    |-----------------|-----------------|  
    |Porta 80|Tráfego HTTP Web|  
    |Porta 443|Tráfego da Web HTTPS|  
  

-   **Leia a documentação de versão do Windows Server Essentials**. A documentação da versão contém as informações mais recentes que podem ser essenciais para instalar e configurar o Windows Server Essentials corretamente. Para exibir ou imprimir a documentação da versão, consulte [liberar documentação para o Windows Server Essentials](../get-started/release-notes.md).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

