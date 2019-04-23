---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Referência técnica de serviço de tempo do Windows
description: O serviço W32Time fornece sincronização de relógio de rede para computadores sem a necessidade de uma ampla configuração. O serviço W32Time é essencial para a operação bem-sucedida de autenticação Kerberos V5 e, portanto, a autenticação do AD DS.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 904a53797d22d2e06e0cd2f7572b99fc386dae16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845117"
---
# <a name="windows-time-service-technical-reference"></a>Referência técnica de serviço de tempo do Windows
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

O serviço W32Time fornece sincronização de relógio de rede para computadores sem a necessidade de uma ampla configuração. O serviço W32Time é essencial para a operação bem-sucedida de autenticação Kerberos V5 e, portanto, a autenticação do AD DS. Qualquer aplicativo com reconhecimento de Kerberos, incluindo a maioria dos serviços de segurança, depende da sincronização de hora entre os computadores que estão participando na solicitação de autenticação. Controladores de domínio do AD DS também devem ser sincronizados relógios para ajudar a garantir que a replicação de dados precisos.

> [!NOTE]  
> No Windows Server 2003 e Microsoft Windows 2000 Server, o serviço de diretório é chamado serviço de diretório do Active Directory. No Windows Server 2008 R2 e Windows Server 2008, o serviço de diretório é chamado de serviços de domínio Active Directory (AD DS). O restante deste tópico refere-se ao AD DS, mas as informações também se aplicam aos serviços de domínio do Active Directory no Windows Server 2016.

O serviço W32Time é implementado em uma biblioteca de vínculo dinâmico chamada W32Time. dll, que é instalado por padrão no **%Systemroot%\System32**. W32Time. dll foi originalmente desenvolvido para o Windows 2000 Server dar suporte a uma especificação pelo protocolo de autenticação Kerberos V5 que necessários relógios em uma rede a serem sincronizados. Começando com o Windows Server 2003, W32time. dll fornecido aumento de precisão na sincronização do relógio de rede sobre o sistema operacional Windows Server 2000. Além disso, no Windows Server 2003, W32time. dll suporte para uma variedade de dispositivos de hardware e protocolos de rede de tempo usando provedores de tempo.

Embora originalmente projetado para fornecer sincronização do relógio para a autenticação Kerberos, muitos aplicativos atuais usam carimbos de hora para garantir a consistência transacional, registrar a hora de eventos importantes e outros negócios críticos, sensíveis ao tempo informações.  Esses aplicativos se beneficiar do tempo de sincronização entre computadores que são fornecidos pelo serviço de tempo do Windows.

## <a name="importance-of-time-protocols"></a>Importância dos protocolos de tempo
Protocolos de tempo a comunicação entre dois computadores para trocar informações de tempo e, em seguida, usar essas informações para sincronizar seus relógios. Com o protocolo de tempo de serviço de tempo do Windows, um cliente solicita informações de tempo de um servidor e sincroniza seu relógio com base nas informações que são recebidas.
  
O serviço de tempo do Windows usa o NTP para ajudar a sincronizar a hora em uma rede. NTP é um protocolo de tempo de Internet que inclui os algoritmos de disciplina necessários para a sincronização de relógios. NTP é um protocolo de tempo mais preciso do que o simples SNTP Network Time Protocol () que é usado em algumas versões do Windows; No entanto, W32Time continua a dar suporte a SNTP para habilitar a compatibilidade com versões anteriores com computadores que executam serviços de tempo com base no SNTP como o Windows 2000.
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>Onde encontrar informações relacionadas à configuração do serviço do tempo do Windows  
Este guia faz **não** abordam a configuração do serviço de tempo do Windows. Existem vários tópicos diferentes no Microsoft TechNet e na Base de dados de Conhecimento da Microsoft que explicam os procedimentos para configurar o serviço de tempo do Windows. Se você precisar de informações de configuração, os tópicos a seguir devem ajudá-lo a localizar as informações apropriadas.  
<!-- should this be an if/then table -->
-   Para configurar o serviço de tempo do Windows para o emulador PDC (controlador) de domínio primário de raiz de floresta, consulte:  
  
    -   [Configurar o serviço de tempo do Windows no emulador PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurar uma fonte de tempo para a floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Artigo da Base de Conhecimento Microsoft 816042, [como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que descreve as definições de configuração para computadores que executam o Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 e o Windows Server 2003 R2.  
  
-   Para configurar o serviço de tempo do Windows em qualquer cliente membro do domínio ou servidor ou controladores de domínio, mesmo que não estão configurados como o emulador PDC de raiz de floresta, consulte [configurar um computador cliente para sincronização de hora automática de domínio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Alguns aplicativos podem exigir que seus computadores para que os serviços de tempo de alta precisão. Se esse for o caso, você pode optar por configurar uma fonte de horário manual, mas lembre-se de que o serviço de tempo do Windows não foi projetado para funcionar como uma fonte de tempo altamente precisos. Certifique-se de que você está ciente das limitações de suporte para ambientes de alta precisão de tempo, conforme descrito na Base de dados de Conhecimento Microsoft artigo 939322, [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md).  
  
-   Para configurar o serviço de tempo do Windows em qualquer Windows cliente ou servidor computadores que são configurados como veem os membros do grupo de trabalho, em vez de membros do domínio [configurar uma fonte de tempo manual para um computador cliente selecionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar o serviço de tempo do Windows em um computador host que executa um ambiente virtual, consulte o artigo da Base de Conhecimento Microsoft 816042, [como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se você estiver trabalhando com um produto de virtualização não Microsoft, certifique-se de consultar a documentação do fornecedor para o produto.  
  
-   Para configurar o serviço de tempo do Windows em um controlador de domínio que está em execução em uma máquina virtual, é recomendável que você desative parcialmente a sincronização de horário entre o sistema e o convidado sistema operacional do host que atua como um controlador de domínio. Isso permite que seu controlador de domínio convidado sincronizar a hora para a hierarquia de domínio, mas protege de distorção de um tempo se ele for restaurado de um estado salvo. Para obter mais informações, consulte o artigo da Base de Conhecimento Microsoft 976924, [recebem o serviço de tempo do Windows identificações de evento 24, 29 e 38 em um controlador de domínio virtualizado que está em execução em um servidor de host baseado no Windows Server 2008 com Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) e [considerações de implantação para controladores de domínio virtualizados](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar o serviço de tempo do Windows em um controlador de domínio atuando como o emulador PDC raiz de floresta que também está em execução em um computador virtual, siga as mesmas instruções para um computador físico, conforme descrito em [configurar o serviço de tempo do Windows em o emulador PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar o serviço de tempo do Windows em um servidor membro executando como um computador virtual, use a hierarquia de tempo de domínio, conforme descrito em [configurar um computador cliente para sincronização de hora automática domínio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).


> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não foi projetado para atender às necessidades do aplicativo sensível ao tempo.  No entanto, as atualizações para o Windows Server 2016 agora permitem que você implemente uma solução para 1ms precisão em seu domínio.  Para obter mais informações, consulte [tempo do Windows 2016 precisos](accurate-time.md) e [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) para obter mais informações.

## <a name="related-topics"></a>Tópicos relacionados
- [Hora precisa do Windows 2016](accurate-time.md)
- [Melhorias de precisão de tempo do Windows Server 2016](windows-server-2016-improvements.md)  
- [Como funciona o serviço de tempo do Windows](How-the-Windows-Time-Service-Works.md)  
- [Ferramentas de serviço de tempo do Windows e configurações](Windows-Time-Service-Tools-and-Settings.md)  
- [Limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md)
- [Artigo da Base de Conhecimento Microsoft 902229](https://go.microsoft.com/fwlink/?LinkId=186066)