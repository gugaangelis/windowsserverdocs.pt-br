---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Criando um projeto de Site
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2bcbf30159721e1fc2e12af103ca6f3e79f3ec97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-design"></a>Criando um projeto de Site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criando um projeto de site envolve decidir quais locais se tornarão sites, criação de objetos de site, criação de objetos de sub-rede e associando as sub-redes a sites.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidindo quais locais se tornarão sites  
Decida quais locais para criar sites para da seguinte maneira:  
  
-   Crie sites para todos os locais em que você pretende colocar controladores de domínio. Consulte as informações documentadas na planilha (DSSTOPO_4.doc) "Posicionamento do controlador de domínio" para identificar os locais que incluem controladores de domínio.  
  
-   Crie sites para esses locais que incluam servidores que executam aplicativos que exigem um site será criado. Certos aplicativos, como o Namespaces do sistema de arquivos distribuído (DFSN), usá-los para localizar os servidores mais próximos aos clientes.  
  
    > [!NOTE]  
    > Se sua organização tiver várias redes próximos com conexões rápidas e confiáveis, você pode incluir todas as sub-redes dessas redes em um único site do Active Directory. Por exemplo, se o retorno de ida e volta de rede latência entre dois servidores em diferentes sub-redes é de 10 ms ou menos, você pode incluir ambas as sub-redes no mesmo site do Active Directory. Se a latência de rede entre os dois locais é maior do que 10 ms, você não deve incluir as sub-redes em um único site do Active Directory. Mesmo quando latência é de 10 ms ou menos, você pode optar por implantar sites separados, se você quiser segmentar o tráfego entre sites para aplicativos baseados no Active Directory.  
  
-   Se um site não é necessário para um local, adicione a sub-rede do local para um site para o qual a localização tem a velocidade WAN (rede) máximo longa distância e a largura de banda disponível.  
  
Locais de documentos que se tornarão sites e os endereços de rede e máscaras de sub-rede dentro de cada local. Para uma planilha para ajudá-lo a documentar sites, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e (DSSTOPO_6.doc) aberto "associar sub-redes com Sites".  
  
## <a name="creating-a-site-object-design"></a>Criando um projeto de objeto de site  
Para todos os locais onde você decidiu criar sites, planeje criar objetos de site nos serviços de domínio do Active Directory (AD DS). Locais de documentos que se tornarão sites na planilha "Associar sub-redes com Sites".  
  
Para obter mais informações sobre como criar objetos de site, consulte Criar um Site ([https://go.microsoft.com/fwlink/?LinkId=107067](https://go.microsoft.com/fwlink/?LinkId=107067)).  
  
## <a name="creating-a-subnet-object-design"></a>Criando um projeto de objeto de sub-rede  
Para cada sub-rede IP e máscara de sub-rede associada a cada local, planeje criar objetos de sub-rede no AD DS que representa todos os endereços IP do site.  
  
Ao criar um objeto de sub-rede do Active Directory, as informações sobre sub-rede IP e máscara de sub-rede são traduzidas automaticamente para o formato de notação de comprimento de prefixo de rede <IP address>/<prefix length>. Por exemplo, o rede IP versão 4 (IPv4) endereço 172.16.4.0 com uma máscara de sub-rede 255.255.252.0 aparece como 172.16.4.0/22. Além dos endereços IPv4, Windows Server 2008 também dá suporte a IP versão 6 (IPv6) sub-rede prefixos, por exemplo, 3FFE:FFFF:0:C000::/ / 64. Para obter mais informações sobre as sub-redes IP em cada local, consulte a planilha de (DSSTOPO_2.doc) "Locais e subredes" no [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) e [apêndice a: locais e prefixos de sub-rede ](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associar cada objeto de sub-rede com um objeto de site referindo-se à planilha (DSSTOPO_6.doc) "Associar sub-redes com Sites" na seção "Decidindo quais locais irá se tornar Sites" para determinar quais sub-rede é ser associado com o site. Documente o objeto de sub-rede do Active Directory associado a cada local na planilha (DSSTOPO_6.doc) "Associar sub-redes com Sites".  
  
Para obter mais informações sobre como criar objetos de sub-rede, consulte Criar uma sub-rede ([https://go.microsoft.com/fwlink/?LinkId=107068](https://go.microsoft.com/fwlink/?LinkId=107068)).  
  


