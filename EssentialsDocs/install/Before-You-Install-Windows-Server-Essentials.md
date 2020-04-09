---
title: Antes de instalar o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 08f39acccef390cbc5cfa10d0743b05f6914327c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817349"
---
# <a name="before-you-install-windows-server-essentials"></a>Antes de instalar o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="before-you-begin-your-installation-of--windows-server-essentials-perform-the-following-tasks"></a><a name="BKMK_BeforeYouBegin"></a>Antes de começar a instalação do Windows Server Essentials, execute as seguintes tarefas:  

-   **Garanta que seu computador cumpra os requisitos mínimos de hardware**. Isso inclui determinar se você precisa de hardware adicional e verificar se os drivers para seu hardware têm suporte do Windows Server Essentials. Para obter mais informações, consulte [requisitos do sistema para o Windows Server Essentials](../get-started/system-requirements.md).   

> [!IMPORTANT]
> Antes de instalar o Windows Server Essentials em um computador pré-existente, recomendamos que você formate totalmente e reparticione os discos rígidos do computador já existente. Formatando e reparticionando os discos rígidos, você elimina a possibilidade de que partições ocultas permaneçam nos discos rígidos.  

- **Preparar sua rede** Para preparar sua rede para instalar o Windows Server Essentials, faça o seguinte:  


  - **Atualizar o sistema operacional em seus computadores cliente**  O Windows Server Essentials dá suporte aos seguintes sistemas operacionais: Windows 8, Windows 7, Windows 10 e Macintosh OS X Lion ou superior. Esses sistemas operacionais fornecem os recursos de segurança, confiabilidade, desempenho e funcionalidade necessários para a rede local.  

  - **Configurar o roteador** Verifique se o roteador está configurado da seguinte maneira:  

    -   A estrutura de UPnP está habilitada no seu roteador.  

    -   O serviço de Servidor do Protocolo DHCP para LAN pode ser habilitado ou desabilitado.  O Windows Server Essentials garante que o DHCP não esteja em execução no servidor e no roteador? Quando o DHCP está habilitado no roteador, o DHCP não é habilitado no servidor durante a instalação.  

    -   Você tem um endereço IP para a interface externa do seu roteador, que é fornecido pelo seu provedor de serviços de Internet (ISP). O endereço IP pode ser dinamicamente atribuído pelo serviço do Servidor DHCP no seu ISP, ou você deve configurar manualmente um endereço IP estático usando o console de gerenciamento de roteador.  

    -   Se sua conexão de Internet exigir um nome de usuário e senha, também chamado de Protocolo ponto a ponto na Ethernet (PPPoE), essas configurações são configuradas no seu roteador, mesmo se o dispositivo tiver suporte para a estrutura UPnP.  

    -   O roteador está conectado à LAN e à Internet, ligado e funcionando adequadamente.  

    Se seu roteador não tiver suporte para a estrutura UPnP, ou se o roteador não puder ser configurado durante a instalação, você deve configurá-lo manualmente com as configurações para a sua rede. Verifique se as seguintes portas estão abertas e direcionadas para o endereço IP do Servidor de Destino:  

  |Número da Porta|Aplicativo|  
  |-----------------|-----------------|  
  |Porta 80|Tráfego da Web HTTP|  
  |Porta 443|Tráfego da Web HTTPS|  


- **Leia a documentação da versão do Windows Server Essentials**. A documentação da versão contém as informações mais recentes que podem ser críticas para instalar e configurar corretamente o Windows Server Essentials. Para exibir ou imprimir a documentação da versão, consulte [a documentação da versão para o Windows Server Essentials](../get-started/release-notes.md).  

## <a name="see-also"></a>Consulte também  

-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

