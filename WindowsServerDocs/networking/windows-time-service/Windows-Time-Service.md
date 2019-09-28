---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Referência técnica do serviço de tempo do Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 31c7c53a5dd28813076fcaa745093050808b5755
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405222"
---
# <a name="windows-time-service"></a>Serviço de Tempo do Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior


**Neste guia**  
  
* Onde encontrar informações de configuração de serviço de tempo do Windows  
* O que é o serviço de tempo do Windows?  
* Importância dos protocolos de tempo  
* Como funciona o serviço Horário do Windows   
* Ferramentas e configurações do Serviço de Tempo do Windows  
  
> [!NOTE]  
> No Windows Server 2003 e no Microsoft Windows 2000 Server, o serviço de diretório é nomeado Active Directory Directory Service. No Windows Server 2008 R2 e no Windows Server 2008, o serviço de diretório é denominado Active Directory Domain Services (AD DS). O restante deste tópico refere-se a AD DS, mas as informações também são aplicáveis a Active Directory Domain Services no Windows Server 2016.  
  
O serviço de tempo do Windows, também conhecido como W32Time, sincroniza a data e a hora de todos os computadores em execução em um domínio AD DS. A sincronização de tempo é essencial para a operação adequada de muitos serviços do Windows e de aplicativos de linha de negócios. O serviço de tempo do Windows usa o protocolo NTP (NTP) para sincronizar os relógios dos computadores na rede, de forma que um valor de relógio preciso ou carimbo de data/hora possa ser atribuído às solicitações de validação de rede e de acesso aos recursos. O serviço integra provedores de tempo e de NTP, tornando-o um serviço de tempo confiável e escalonável para administradores corporativos.
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não foi projetado para atender às necessidades de aplicativos sensíveis ao tempo.  No entanto, as atualizações para o Windows Server 2016 agora permitem que você implemente uma solução para precisão 1 ms em seu domínio.  Consulte limite de tempo e suporte [precisos do windows 2016](accurate-time.md) [para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) para obter mais informações.  
  
## <a name="BKMK_Config"></a>Onde encontrar informações de configuração de serviço de tempo do Windows  
Este guia não **aborda a** configuração do serviço de tempo do Windows. Há vários tópicos diferentes no Microsoft TechNet e na base de dados de conhecimento Microsoft que explicam os procedimentos para configurar o serviço de tempo do Windows. Se você precisar de informações de configuração, os tópicos a seguir devem ajudá-lo a localizar as informações apropriadas.  
  
-   Para configurar o serviço de tempo do Windows para o emulador do controlador de domínio primário (PDC) da raiz da floresta, consulte:  
  
    -   [Configurar o serviço de tempo do Windows no emulador de PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurando uma fonte de tempo para a floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   O artigo 816042 da base de dados de conhecimento Microsoft, [como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que descreve as definições de configuração para computadores que executam o windows Server 2008 R2, o windows Server 2008, o windows Server 2003 e o Windows Server 2003 R2.  
  
-   Para configurar o serviço de tempo do Windows em qualquer cliente ou servidor membro do domínio, ou até mesmo controladores de domínio que não estejam configurados como o emulador PDC raiz da floresta, consulte [configurar um computador cliente para sincronização automática de horário de domínio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Alguns aplicativos podem exigir que seus computadores tenham serviços de tempo de alta precisão. Se esse for o caso, você pode optar por configurar uma fonte de tempo manual, mas lembre-se de que o serviço de tempo do Windows não foi projetado para funcionar como uma fonte de tempo altamente precisa. Verifique se você está ciente das limitações de suporte para ambientes de tempo de alta precisão, conforme descrito no artigo 939322 da base de dados de conhecimento Microsoft, [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md).  
  
-   Para configurar o serviço de tempo do Windows em qualquer computador cliente ou servidor baseado no Windows configurado como membros do grupo de trabalho, em vez de membros do domínio, consulte [Configurar uma fonte de tempo manual para um computador cliente selecionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar o serviço de tempo do Windows em um computador host que executa um ambiente virtual, consulte o artigo 816042 da base de dados de conhecimento Microsoft, [como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se você estiver trabalhando com um produto de virtualização que não seja da Microsoft, certifique-se de consultar a documentação do fornecedor do produto.  
  
-   Para configurar o serviço de tempo do Windows em um controlador de domínio que está sendo executado em uma máquina virtual, é recomendável que você desabilite parcialmente a sincronização de tempo entre o sistema host e o sistema operacional convidado atuando como um controlador de domínio. Isso permite que seu controlador de domínio de convidado sincronize a hora para a hierarquia de domínio, mas a protege de ter uma distorção de tempo se for restaurada a partir de um estado salvo. Para obter mais informações, consulte o artigo 976924 da base de dados de conhecimento Microsoft, [você recebe as IDs de evento 24, 29 e 38 de serviço de tempo do Windows em um controlador de domínio virtualizado que está sendo executado em um servidor de host baseado no Windows Server 2008 com o Hyper-V e a](https://go.microsoft.com/fwlink/?LinkID=192236) [implantação Considerações para controladores de domínio virtualizados](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar o serviço de tempo do Windows em um controlador de domínio que atua como o emulador PDC raiz da floresta que também está em execução em um computador virtual, siga as mesmas instruções para um computador físico, conforme descrito em [Configurar o serviço de tempo do Windows no PDC emulador no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar o serviço de tempo do Windows em um servidor membro em execução como um computador virtual, use a hierarquia de tempo de domínio conforme descrito em ([configurar um computador cliente para sincronização automática de horário de domínio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="BKMK_WTS"></a>O que é o serviço de tempo do Windows?  
O serviço de tempo do Windows (W32Time) fornece sincronização de relógio de rede para computadores sem a necessidade de configuração extensiva.  
  
O serviço de tempo do Windows é essencial para a operação bem-sucedida da autenticação do Kerberos versão 5 e, portanto, para a autenticação baseada em AD DS. Qualquer aplicativo com reconhecimento de Kerberos, incluindo a maioria dos serviços de segurança, depende da sincronização de hora entre os computadores que participam da solicitação de autenticação. AD DS controladores de domínio também devem ter relógios sincronizados para ajudar a garantir a replicação de dados precisa.  
  
O serviço de tempo do Windows é implementado em uma biblioteca de vínculo dinâmico chamada W32Time. dll. O W32Time. dll é instalado por padrão na pasta **%systemroot%\system32** durante a instalação e instalação do sistema operacional.  
  
O W32Time. dll foi originalmente desenvolvido para o Windows 2000 Server para dar suporte a uma especificação pelo protocolo de autenticação Kerberos V5 que exigia os relógios em uma rede para serem sincronizados. A partir do Windows Server 2003, o W32Time. dll forneceu maior precisão na sincronização do relógio de rede no sistema operacional Windows 2000 Server e, além disso, oferece suporte a uma variedade de dispositivos de hardware e protocolos de tempo de rede por meio de tempo fornecedor. Embora originalmente projetado para fornecer a sincronização de relógio para a autenticação Kerberos, muitos aplicativos atuais usam carimbos de data/hora para garantir a consistência transacional, registrar o tempo de eventos importantes e outros críticos para os negócios, que diferenciam o tempo divulgação. Esses aplicativos se beneficiam da sincronização de horário entre computadores fornecidos pelo serviço de tempo do Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importância dos protocolos de tempo  
Os protocolos de tempo se comunicam entre dois computadores para trocar informações de tempo e, em seguida, usam essas informações para sincronizar seus relógios. Com o protocolo de tempo do serviço de tempo do Windows, um cliente solicita informações de tempo de um servidor e sincroniza seu relógio com base nas informações recebidas.  
  
O serviço de tempo do Windows usa o NTP para ajudar a sincronizar o tempo em uma rede. O NTP é um protocolo de tempo da Internet que inclui os algoritmos de disciplina necessários para a sincronização de relógios. O NTP é um protocolo de tempo mais preciso do que o protocolo NTP simples (SNTP) usado em algumas versões do Windows; no entanto, o W32Time continua a dar suporte a SNTP para habilitar a compatibilidade com versões anteriores com computadores que executam serviços de tempo baseados em SNTP, como o Windows 2000.  
  
## <a name="see-also"></a>Consulte também  
[Como funciona o serviço de tempo do Windows](How-the-Windows-Time-Service-Works.md)  
[Ferramentas e configurações do Serviço de Tempo do Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Artigo 902229 da base de dados de conhecimento Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)