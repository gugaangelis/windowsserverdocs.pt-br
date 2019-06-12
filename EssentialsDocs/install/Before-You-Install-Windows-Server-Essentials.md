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
ms.openlocfilehash: 32b2b37e0d0109b8ad2a991b9f7693139103734d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433713"
---
# <a name="before-you-install-windows-server-essentials"></a>Antes de instalar o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a> Antes de começar a instalação do Windows Server Essentials, execute as seguintes tarefas:  

-   **Garanta que seu computador cumpra os requisitos mínimos de hardware**. Isso inclui determinar se você precisa de hardware adicional e verificar que os drivers para o seu hardware têm suporte pelo Windows Server Essentials. Para obter mais informações, consulte [requisitos de sistema para o Windows Server Essentials](../get-started/system-requirements.md).   


~~~
> [!IMPORTANT]
>  Before you install  Windows Server Essentials on a pre-existing computer, we recommend that you fully format and then repartition the hard disks of the pre-existing computer. By formatting and repartitioning the hard disks, you remove the possibility that hidden partitions remain on the hard disks.  
~~~

- **Prepare a sua rede** para preparar sua rede para instalar o Windows Server Essentials, faça o seguinte:  


  - **Atualizar o sistema operacional nos seus computadores cliente** Windows Server Essentials oferece suporte a sistemas operacionais a seguir:  Windows 8, Windows 7, Windows 10 e Macintosh OS X Lion ou superior. Esses sistemas operacionais fornecem os recursos de segurança, confiabilidade, desempenho e funcionalidade necessários para a rede local.  

  - **Configurar o roteador** Verifique se o roteador está configurado da seguinte maneira:  

    -   A estrutura de UPnP está habilitada no seu roteador.  

    -   O serviço de Servidor do Protocolo DHCP para LAN pode ser habilitado ou desabilitado.  Windows Server Essentials garante que o DHCP não está executando no servidor e o roteador? Quando o DHCP está habilitado no roteador, DHCP não está habilitado no servidor durante a instalação.  

    -   Você tem um endereço IP para a interface externa do seu roteador, que é fornecido pelo seu provedor de serviços de Internet (ISP). O endereço IP pode ser dinamicamente atribuído pelo serviço do Servidor DHCP no seu ISP, ou você deve configurar manualmente um endereço IP estático usando o console de gerenciamento de roteador.  

    -   Se sua conexão de Internet exigir um nome de usuário e senha, também chamado de Protocolo ponto a ponto na Ethernet (PPPoE), essas configurações são configuradas no seu roteador, mesmo se o dispositivo tiver suporte para a estrutura UPnP.  

    -   O roteador está conectado à LAN e à Internet, ligado e funcionando adequadamente.  

    Se seu roteador não tiver suporte para a estrutura UPnP, ou se o roteador não puder ser configurado durante a instalação, você deve configurá-lo manualmente com as configurações para a sua rede. Verifique se as seguintes portas estão abertas e direcionadas para o endereço IP do Servidor de Destino:  

  |Número da Porta|Aplicativo|  
  |-----------------|-----------------|  
  |Porta 80|Tráfego da Web HTTP|  
  |Porta 443|Tráfego da Web HTTPS|  


- **Leia a documentação de versão do Windows Server Essentials**. Documentação de versão contém as informações mais recentes que podem ser essenciais para instalar e configurar o Windows Server Essentials de corretamente. Para exibir ou imprimir a documentação de versão, consulte [Release Documentation for Windows Server Essentials](../get-started/release-notes.md).  

## <a name="see-also"></a>Consulte também  

-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

