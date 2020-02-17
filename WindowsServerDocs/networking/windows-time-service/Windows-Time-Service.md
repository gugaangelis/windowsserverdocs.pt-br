---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Referência Técnica do Serviço de Horário do Windows
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
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405222"
---
# <a name="windows-time-service"></a>Serviço de Tempo do Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior


**Neste guia**  
  
* Onde encontrar Informações sobre a Configuração do Serviço de Horário do Windows  
* O que é o Serviço de Horário do Windows?  
* Importância dos Protocolos de Tempo  
* Como funciona o serviço Horário do Windows   
* Ferramentas e configurações do Serviço de Tempo do Windows  
  
> [!NOTE]  
> No Windows Server 2003 e no Microsoft Windows 2000 Server, o serviço de diretório é denominado serviço de diretório Active Directory. No Windows Server 2008 R2 e no Windows Server 2008, o serviço de diretório é denominado AD DS (Active Directory Domain Services). O restante deste tópico se refere ao AD DS, mas as informações também se aplicam ao Active Directory Domain Services no Windows Server 2016.  
  
O Serviço de Horário do Windows, também conhecido como W32Time, sincroniza a data e a hora de todos os computadores em execução em um domínio AD DS. A sincronização de tempo é essencial para a operação adequada de muitos serviços do Windows e de aplicativos de linha de negócios. O Serviço de Horário do Windows usa o protocolo NTP para sincronizar os relógios dos computadores na rede para que um valor preciso do relógio ou um carimbo de data/hora possa ser atribuído à validação de rede e a solicitações de acesso aos recursos. O serviço integra o NTP e os provedores de horário, tornando-o um serviço de horário confiável e escalonável para administradores corporativos.
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não era criado para atender a necessidades de aplicativos sensíveis ao tempo.  Contudo, agora as atualizações do Windows Server 2016 permitem que você implemente uma solução para ter precisão de 1 ms em seu domínio.  Confira [Tempo Preciso do Windows 2016](accurate-time.md) e [Limite de suporte para configurar o Serviço de Horário do Windows para ambientes de alta precisão](support-boundary.md) para obter mais informações.  
  
## <a name="BKMK_Config"></a>Onde encontrar Informações sobre a Configuração do Serviço de Horário do Windows  
Este guia **não** discute a configuração do Serviço de Horário do Windows. Há vários tópicos diferentes no Microsoft TechNet e na Base de Dados de Conhecimento Microsoft que explicam os procedimentos para configurar o Serviço de Horário do Windows. Se você precisar de informações de configuração, os tópicos a seguir deverão ajudar a localizar as informações apropriadas.  
  
-   Para configurar o Serviço de Horário do Windows para o emulador PDC (controlador de domínio primário) raiz da floresta, confira:  
  
    -   [Configurar o Serviço de Horário do Windows no emulador PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurar uma fonte de horário para a floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   O artigo 816042 da Base de Dados de Conhecimento Microsoft, [Como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que descreve as configurações para computadores que executam o Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 e Windows Server 2003 R2.  
  
-   Para configurar o Serviço de Horário do Windows em qualquer cliente ou servidor membro do domínio ou até mesmo controladores de domínio que não estejam configurados como o emulador PDC raiz da floresta, confira [Configurar um computador cliente para sincronização automática de horário de domínio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Alguns aplicativos podem exigir que os computadores deles tenham serviços de tempo de alta precisão. Se esse for o caso, você poderá optar por configurar uma fonte de horário manual, mas saiba que o Serviço de Horário do Windows não foi criado para funcionar como uma fonte de horário altamente precisa. Verifique se você conhece as limitações de suporte para ambientes de tempo de alta precisão conforme descrito no artigo 939322 da Base de Dados de Conhecimento Microsoft, [Limite de suporte para configurar o Serviço de Horário do Windows para ambientes de alta precisão](support-boundary.md).  
  
-   Para configurar o Serviço de Horário do Windows em computadores cliente ou de servidor baseados em Windows que estão configurados como membros de um grupo de trabalho em vez de membros de domínio, confira [Configurar uma fonte de horário manual para um computador cliente selecionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar o Serviço de Horário do Windows em um computador host que executa um ambiente virtual, confira o artigo 816042 da Base de Dados de Conhecimento Microsoft, [Como configurar um servidor de horário autoritativo no Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se estiver trabalhando com um produto de virtualização que não seja da Microsoft, confira a documentação do fornecedor desse produto.  
  
-   Para configurar o Serviço de Horário do Windows em um controlador de domínio que está sendo executado em uma máquina virtual, é recomendável que você desabilite parcialmente a sincronização de tempo entre o sistema host e o sistema operacional convidado que funciona como um controlador de domínio. Isso permitirá que seu controlador de domínio convidado sincronize o tempo da hierarquia de domínio, mas a proteja contra uma distorção de tempo se ela for restaurada de um estado Salvo. Para obter mais informações, confira o artigo 976924 da Base de Dados de Conhecimento Microsoft, [Você recebe as IDs de evento 24, 29 e 38 do Serviço de Horário do Windows em um controlador de domínio virtualizado em execução em um host baseado em Windows Server 2008 com Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) e [Considerações de Implantação para Controladores de Domínio Virtualizados](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar o Serviço de Horário do Windows em um controlador de domínio que funciona como o emulador PDC raiz da floresta que também está em execução em um computador virtual, siga as mesmas instruções para um computador físico conforme descrito em [Configurar o Serviço de Horário do Windows no emulador PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar o Serviço de Horário do Windows em um servidor membro em execução como um computador virtual, use a hierarquia de tempo de domínio conforme descrito em ([Configurar um computador cliente para sincronização automática de tempo de domínio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="BKMK_WTS"></a>O que é o Serviço de Horário do Windows?  
O Serviço de Horário do Windows (W32Time) fornece sincronização de relógio de rede para computadores sem a necessidade de configuração extensiva.  
  
O Serviço de Horário do Windows é essencial para a operação bem-sucedida da autenticação do Kerberos versão 5 e, portanto, para a autenticação baseada em AD DS. Qualquer aplicativo com reconhecimento de Kerberos, incluindo a maioria dos serviços de segurança, depende da sincronização de tempo entre os computadores que participam da solicitação de autenticação. Os controladores de domínio do AD DS também devem ter relógios sincronizados para ajudar a garantir uma replicação de dados precisa.  
  
O Serviço de Horário do Windows é implementado em uma biblioteca de vínculo dinâmico chamada W32Time.dll. O W32Time.dll é instalado por padrão na pasta **%Systemroot%\System32** durante a configuração e instalação do sistema operacional.  
  
O W32Time.dll foi originalmente desenvolvido para o Windows 2000 Server dar suporte a uma especificação pelo protocolo de autenticação Kerberos V5 que exigia que os relógios estivessem em uma rede para serem sincronizados. Do Windows Server 2003 em diante, o W32Time.dll fornecia maior precisão na sincronização do relógio de rede no sistema operacional Windows 2000 Server e, além disso, dava suporte a uma variedade de dispositivos de hardware e protocolos NTP por meio de provedores de horário. Embora originalmente criado para fornecer sincronização de relógio para a autenticação do Kerberos, muitos aplicativos atuais usam carimbos de data/hora para garantir a consistência transacional, registrar o temo de importantes eventos e outras informações críticas para os negócios e sensíveis ao tempo. Esses aplicativos se beneficiam da sincronização de tempo entre computadores fornecidos pelo Serviço de Horário do Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importância dos Protocolos de Tempo  
Os protocolos de tempo se comunicam entre dois computadores para trocar informações sobre tempo e usá-las para sincronizar os relógios deles. Com o protocolo de tempo do Serviço de Horário do Windows, um cliente solicita informações sobre tempo de um servidor e sincroniza o relógio dele com base nas informações recebidas.  
  
O Serviço de Horário do Windows usa o NTP para ajudar a sincronizar o tempo em uma rede. O NTP é um protocolo de tempo da Internet que inclui os algoritmos de disciplina necessários para sincronizar relógios. O NTP é um protocolo de tempo mais preciso que o SNTP (protocolo NTP simples) usado em algumas versões do Windows; no entanto, o W32Time continua dando suporte ao SNTP para habilitar a compatibilidade com versões anteriores com computadores que executam serviços de tempo baseados em SNTP, como o Windows 2000.  
  
## <a name="see-also"></a>Consulte Também  
[Como o serviço de Horário do Windows funciona](How-the-Windows-Time-Service-Works.md)  
[Ferramentas e configurações do Serviço de Tempo do Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Artigo 902229 da Base de Dados de Conhecimento Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)