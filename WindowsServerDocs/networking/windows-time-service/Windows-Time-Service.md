---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: "Serviço de tempo do Windows"
description: 
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c3c53db3839b5f33a87fe3789f2f7958f0bc42c2
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
---
# <a name="windows-time-service"></a>Serviço de tempo do Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
 
  
## <a name="w2k3tr_times_intro"></a>Referência técnica de serviço de tempo do Windows  
**Neste guia**  
  
* Onde encontrar informações de configuração de serviço de tempo do Windows  
* Qual é o serviço de tempo do Windows?  
* Importância dos protocolos de tempo  
* Como funciona o serviço de tempo do Windows   
* Ferramentas de serviço de tempo do Windows e configurações  
  
> [!NOTE]  
> No Windows Server 2003 e Microsoft Windows 2000 Server, o serviço de diretório é nomeado serviço de diretório do Active Directory. No Windows Server 2008 R2 e Windows Server 2008, o serviço de diretório é nomeado os serviços de domínio do Active Directory (AD DS). O restante deste tópico refere-se no AD DS, mas as informações também são aplicáveis aos serviços de domínio do Active Directory no Windows Server 2016.  
  
O serviço de tempo do Windows, também conhecido como W32Time, sincroniza a data e hora para todos os computadores em execução em um domínio do AD DS. Sincronização de tempo é essencial para o bom funcionamento muitos serviços do Windows e aplicativos de linha de negócios. O serviço de tempo do Windows usa o protocolo de tempo de rede (NTP) para sincronizar os relógios do computador na rede para que um valor preciso de relógio, ou carimbo de data / hora, pode ser atribuído a solicitações de acesso de validação e recursos de rede. O serviço se integra NTP e provedores de tempo, tornando-a como um serviço de tempo escalável e confiável para os administradores corporativos.  
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não foi projetado para atender às necessidades do aplicativo de detecção de hora.  No entanto, as atualizações para Windows Server 2016 agora permitem que você implemente uma solução para 1 MS precisão em seu domínio.  Consulte [Windows 2016 precisão tempo](accurate-time.md) e [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](https://go.microsoft.com/fwlink/?LinkID=179459) para obter mais informações.  
  
## <a name="BKMK_Config"></a>Onde encontrar informações de configuração de serviço de tempo do Windows  
Este guia faz **não** discutir Configurando o serviço de tempo do Windows. Existem vários tópicos diferentes no Microsoft TechNet e na Base de dados de Conhecimento da Microsoft que explicam os procedimentos para configurar o serviço de tempo do Windows. Se você precisar de informações de configuração, os tópicos a seguir devem ajudá-lo a localizar as informações apropriadas.  
  
-   Para configurar o serviço de tempo do Windows para o emulador do floresta raiz domínio primário (PDC) do controlador, consulte:  
  
    -   [Configurar o serviço de tempo do Windows no emulador do PDC no domínio raiz da floresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurar uma fonte de tempo da floresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Artigo da Base de Conhecimento Microsoft 816042, [como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que descreve configurações para computadores que executam o Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 e Windows Server 2003 R2.  
  
-   Para configurar o serviço de tempo do Windows em qualquer cliente de membro do domínio ou servidor ou até mesmo controladores de domínio que não estão configuradas como o emulador do PDC de raiz floresta, consulte [configurar um computador cliente para sincronização de tempo de domínio automática](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Alguns aplicativos podem exigir seus computadores para ter os serviços de tempo de alta precisão. Se esse for o caso, você pode optar por configurar uma fonte de tempo manual, mas lembre-se de que o serviço de tempo do Windows não foi projetado para funcionar como uma fonte de tempo de alta precisão. Certifique-se de que você esteja ciente das limitações suporte para ambientes de tempo de alta precisão conforme descrito no artigo da Base de Conhecimento Microsoft 939322, [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](https://go.microsoft.com/fwlink/?LinkID=179459).  
  
-   Para configurar o serviço de tempo do Windows qualquer Windows cliente ou servidor em computadores baseados em que estão configurados como membros do grupo de trabalho em vez de membros do domínio vejam [configurar uma fonte de tempo manual para um computador cliente selecionado](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar o serviço de tempo do Windows em um computador host que executa um ambiente virtual, consulte o artigo da Base de Conhecimento Microsoft 816042, [como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se você estiver trabalhando com um produto de virtualização não são da Microsoft, certifique-se de consultar a documentação do fornecedor para esse produto.  
  
-   Para configurar o serviço de tempo do Windows em um controlador de domínio que está sendo executado em uma máquina virtual, é recomendável que você parcialmente desabilitar a sincronização de tempo entre o sistema e convidados sistema operacional host atua como um controlador de domínio. Isso permite que o controlador de domínio do convidado sincronizar o tempo para a hierarquia de domínio, mas protege contra tendo um tempo distorção se ele for restaurado de um estado salvo. Para obter mais informações, consulte o artigo da Base de Conhecimento Microsoft 976924, [você receber o serviço de tempo do Windows IDs de evento 24, 29 e 38 em um controlador de domínio virtualizado que está sendo executado em um servidor de host com base no Windows Server 2008 com Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) e [Considerações sobre a implantação em controladores de domínio virtualizado](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar o serviço de tempo do Windows em um controlador de domínio atuando como o emulador do PDC de raiz de floresta que também estiver sendo executado em uma máquina virtual, siga as mesmas instruções para um computador físico, conforme descrito em [configurar o serviço de tempo do Windows no PDC emulador no domínio raiz da floresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar o serviço de tempo do Windows em um servidor membro em execução como um computador virtual, use a hierarquia de tempo de domínio conforme descrito em ([configurar um computador cliente para sincronização de tempo de domínio automática](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="BKMK_WTS"></a>Qual é o serviço de tempo do Windows?  
O serviço de tempo do Windows (W32Time) fornece a sincronização do relógio de rede para computadores sem a necessidade de configuração ampla.  
  
O serviço de tempo do Windows é essencial para a operação bem-sucedida de autenticação Kerberos versão 5 e, portanto, a autenticação AD DS. Qualquer aplicativo com reconhecimento de Kerberos, incluindo a maioria dos serviços de segurança, depende de sincronização de tempo entre os computadores que estão participando a solicitação de autenticação. Controladores de domínio do AD DS também devem ser sincronizados relógios para ajudar a garantir a replicação de dados precisos.  
  
O serviço de tempo do Windows é implementado em uma biblioteca de vínculo dinâmico chamada W32Time.dll. W32Time.dll é instalado por padrão no **%Systemroot%\System32** pasta durante a instalação e configuração do sistema operacional.  
  
W32Time.dll foi desenvolvido originalmente para o Windows 2000 Server dar suporte a uma especificação pelo protocolo de autenticação Kerberos V5 que necessárias relógios em uma rede a serem sincronizadas. A partir do Windows Server 2003, W32Time.dll fornecido maior precisão na sincronização do relógio de rede sobre o sistema operacional Windows 2000 Server e, além disso, suporte para uma variedade de dispositivos de hardware e os protocolos de rede por meio de provedores de tempo. Embora originalmente projetado para fornecer a sincronização do relógio para autenticação Kerberos, muitos aplicativos atuais usam carimbos para garantir a consistência transacional, para registrar o tempo de eventos importantes e outras informações essenciais, detecção de hora. Esses aplicativos se beneficiar de tempo de sincronização entre computadores que é fornecida pelo serviço de tempo do Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importância dos protocolos de tempo  
Protocolos de tempo se comunicar entre dois computadores para trocar informações de tempo e, em seguida, usar essas informações para sincronizar seus relógios. Com o protocolo de tempo de serviço de tempo do Windows, um cliente solicita informações de tempo de um servidor e sincroniza o relógio com base nas informações que são recebidas.  
  
O serviço de tempo do Windows usa NTP para ajudar a sincronizar o tempo em uma rede. NTP é um protocolo de tempo de Internet que inclui os algoritmos de disciplina necessários para sincronizar relógios. NTP é um protocolo de tempo mais preciso do que a simples SNTP Network Time Protocol () que é usado em algumas versões do Windows; No entanto, W32Time continua a dar suporte a SNTP para habilitar a compatibilidade com computadores que executam serviços de tempo com base no SNTP como o Windows 2000.  
  
## <a name="see-also"></a>Consulte também  
[Como funciona o serviço de tempo do Windows](How-the-Windows-Time-Service-Works.md)  
[Ferramentas de serviço de tempo do Windows e configurações](Windows-Time-Service-Tools-and-Settings.md)  
[Artigo da Base de Conhecimento Microsoft 902229](https://go.microsoft.com/fwlink/?LinkId=186066)