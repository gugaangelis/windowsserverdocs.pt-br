---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Criar um projeto de site
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0464510b498296570bdef5edb122c6c26e1fe18f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822769"
---
# <a name="creating-a-site-design"></a>Criar um projeto de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criar um design de site envolve decidir quais locais se tornarão sites, criando objetos de site, criando objetos de sub-rede e associando as sub-redes a sites.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidindo quais locais se tornarão sites

Decida quais locais criar sites do da seguinte maneira:  
  
- Crie sites para todos os locais em que você planeja posicionar controladores de domínio. Consulte as informações documentadas na planilha "posicionamento do controlador de domínio" (DSSTOPO_4. doc) para identificar os locais que incluem controladores de domínio.  
- Crie sites para esses locais que incluem servidores que executam aplicativos que exigem um site a ser criado. Determinados aplicativos, como DFSN (namespaces do Sistema de Arquivos Distribuído), usam objetos do site para localizar os servidores mais próximos aos clientes.  

   > [!NOTE]  
   > Se sua organização tiver várias redes em proximidade com conexões rápidas e confiáveis, você poderá incluir todas as sub-redes para essas redes em um único site de Active Directory. Por exemplo, se a viagem de ida e volta da latência de rede entre dois servidores em sub-redes diferentes for de 10 ms ou menos, você poderá incluir ambas as sub-redes no mesmo site Active Directory. Se a latência de rede entre os dois locais for maior que 10 ms, você não deverá incluir as sub-redes em um único site de Active Directory. Mesmo quando a latência for de 10 ms ou menos, você poderá optar por implantar sites separados se desejar segmentar o tráfego entre sites para aplicativos baseados em Active Directory.  

- Se um site não for necessário para um local, adicione a sub-rede do local a um site para o qual o local tem a velocidade máxima da WAN (rede de longa distância) e a largura de banda disponível.  
  
Locais de documentos que se tornarão sites e os endereços de rede e máscaras de sub-rede dentro de cada local. Para uma planilha para ajudá-lo a documentar sites, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e abrir "associando sub-redes com sites" (DSSTOPO_6. doc).  
  
## <a name="creating-a-site-object-design"></a>Criando um design de objeto do site

Para cada local em que você decidiu criar sites, planeje criar objetos de site no Active Directory Domain Services (AD DS). Locais de documentos que se tornarão sites na planilha "associando sub-redes com sites".  
  
Para obter mais informações sobre como criar objetos de site, consulte o artigo [criar um site](https://go.microsoft.com/fwlink/?LinkId=107067).  
  
## <a name="creating-a-subnet-object-design"></a>Criando um design de objeto de sub-rede

Para cada sub-rede IP e máscara de sub-rede associada a cada local, planeje criar objetos de sub-rede no AD DS representando todos os endereços IP no site.  
  
Ao criar um objeto de sub-rede Active Directory, as informações sobre a sub-rede IP de rede e a máscara de sub-rede são automaticamente convertidas no formato de notação de comprimento de prefixo de rede <IP address>/<prefix length>. Por exemplo, o endereço IP de rede versão 4 (IPv4) 172.16.4.0 com uma máscara de sub-rede 255.255.252.0 aparece como 172.16.4.0/22. Além dos endereços IPv4, o Windows Server 2008 também dá suporte a prefixos de sub-rede IP versão 6 (IPv6), por exemplo, 3FFE: FFFF: 0: C000::/64. Para obter mais informações sobre sub-redes IP em cada local, consulte a planilha "locais e sub-redes" (DSSTOPO_2. doc) em [coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) e [Apêndice A: locais e prefixos de sub-rede](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associe cada objeto de sub-rede a um objeto de site fazendo referência à planilha "associando sub-redes com sites" (DSSTOPO_6. doc) na seção "decidindo quais locais se tornarão sites" para determinar qual sub-rede deve ser associada a qual site. Documente a Active Directory objeto de sub-rede associado a cada local na planilha "associando sub-redes com sites" (DSSTOPO_6. doc).  
  
Para obter mais informações sobre como criar objetos de sub-rede, consulte o artigo [criar uma sub-rede](https://go.microsoft.com/fwlink/?LinkId=107068).
