---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Criar um projeto de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d896213708f8c3ec5de44a1f85fb4ebd86b8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819067"
---
# <a name="creating-a-site-design"></a>Criar um projeto de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criando um projeto de site envolve Decidindo quais locais se tornarão sites, criação de objetos do site, criando os objetos da sub-rede e associando as sub-redes a sites.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidindo quais locais se tornarão sites

Decida quais locais para criar sites para da seguinte maneira:  
  
- Crie sites para todos os locais em que você planeja colocar controladores de domínio. Consulte as informações documentadas na planilha "Posicionamento de controlador de domínio" (DSSTOPO_4.doc) para identificar locais que incluem controladores de domínio.  
- Crie sites para esses locais que incluem servidores que executam aplicativos que exigem um site a ser criado. Determinados aplicativos, como o Distributed arquivo sistema DFSN (Namespaces), usam objetos de site para localizar os servidores mais próximos aos clientes.  

   > [!NOTE]  
   > Se sua organização tiver várias redes em estreita proximidade com conexões rápidas e confiáveis, você pode incluir todas as sub-redes para essas redes em um único site do Active Directory. Por exemplo, se o retorno de ida e volta latência de rede entre dois servidores em diferentes sub-redes é de 10 ms ou menos, você pode incluir ambas as sub-redes no mesmo site do Active Directory. Se a latência de rede entre os dois locais for maior que 10 ms, você não deve incluir as sub-redes em um único site do Active Directory. Mesmo quando a latência é de 10 ms ou menos, você pode optar por implantar sites separados se você quiser segmentar o tráfego entre sites para aplicativos baseados no Active Directory.  

- Se um site não é necessário para um local, adicione a sub-rede do local para um site para o qual o local tem a velocidade de rede de (longa distância WAN) de longa distância máxima e largura de banda disponível.  
  
Locais de documento que se tornarão sites e os endereços de rede e máscaras de sub-rede dentro de cada local. Para uma planilha ajudar a documentar os sites, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de baixar e abrir "associar sub-redes com Sites"(DSSTOPO_6.doc).  
  
## <a name="creating-a-site-object-design"></a>Criar um design do objeto de site

Para cada local em que você optou por criar sites, planeje criar objetos do site nos serviços de domínio Active Directory (AD DS). Locais de documento que se tornarão sites na planilha "Associar sub-redes com Sites".  
  
Para obter mais informações sobre como criar objetos do site, consulte o artigo [criar um Site](https://go.microsoft.com/fwlink/?LinkId=107067).  
  
## <a name="creating-a-subnet-object-design"></a>Criar um design do objeto de sub-rede

Para cada sub-rede IP e máscara de sub-rede associada a cada local, planeje criar objetos de sub-rede no AD DS que representa todos os endereços IP dentro do site.  
  
Ao criar um objeto de sub-rede do Active Directory, as informações sobre a sub-rede IP de rede e máscara de sub-rede são convertidas automaticamente no formato de notação de comprimento de prefixo de rede <IP address> / <prefix length>. Por exemplo, o rede IP versão 4 (IPv4) endereço 172.16.4.0 com uma máscara de sub-rede 255.255.252.0 é exibido como 172.16.4.0/22. Além dos endereços IPv4, Windows Server 2008 também dá suporte a IP versão 6 (IPv6) sub-rede prefixos, por exemplo, 3FFE:FFFF:0:C000::/ 64. Para obter mais informações sobre as sub-redes IP em cada local, consulte a planilha de "Locais e sub-redes" (DSSTOPO_2.doc) no [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) e [apêndice a: Locais e prefixos de sub-rede](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associar cada objeto de sub-rede com um objeto de site referindo-se a planilha "Associar sub-redes com Sites" (DSSTOPO_6.doc) na seção "Decidindo quais locais serão se tornar Sites" para determinar qual sub-rede deve ser associada com a qual site. O objeto de sub-rede do Active Directory associado a cada local na planilha "Associar sub-redes com Sites" (DSSTOPO_6.doc) de documento.  
  
Para obter mais informações sobre como criar objetos de sub-rede, consulte o artigo [criar uma sub-rede](https://go.microsoft.com/fwlink/?LinkId=107068).
