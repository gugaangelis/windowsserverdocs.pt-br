---
title: Gerenciamento de registros de recurso do DNS
description: Este tópico faz parte do guia de gerenciamento de gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ed781ef37243b80ea9da8aad27a29046b8dc8c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-resource-record-management"></a>Gerenciamento de registros de recurso do DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece informações sobre gerenciamento de registros de recurso DNS usando IPAM.  
  
> [!NOTE]  
> Além neste tópico, os seguintes tópicos de gerenciamento de registros de recurso DNS estão disponíveis nesta seção.  
>   
> -   [Adicionar um registro de recurso DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Excluir registros de recurso do DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrar a exibição de registros de recurso do DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Registros de recurso DNS de modo de exibição para um endereço IP específico](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Visão geral do gerenciamento de registros de recurso  
Quando você implanta IPAM no Windows Server 2016, você pode executar descoberta de servidor para adicionar servidores DHCP e DNS para o console de gerenciamento de servidor IPAM. O servidor IPAM, em seguida, dinamicamente coleta dados DNS cada seis horas dos servidores DNS que ele está configurado para gerenciar. IPAM mantém um banco de dados local em que ele armazena esses dados DNS. IPAM fornece notificação do dia e hora em que os dados do servidor foram coletados, bem como informando o dia seguinte e tempo quando a coleta de dados dos servidores DNS ocorrerá.  
  
A barra de status amarelo na ilustração a seguir mostra a localização de interface do usuário de notificações de IPAM.  
  
![Notificações de IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Os dados DNS coletados incluem informações de registro zona e recurso DNS. Você pode configurar IPAM para coletar informações de zona do seu servidor DNS preferencial.  IPAM coleta zonas de diretório baseados em arquivo e o ativo.  
  
> [!NOTE]  
> IPAM coleta dados exclusivamente do domínio servidores DNS da Microsoft. Servidores DNS de terceiros e não do domínio associado não são suportados pelo IPAM.  
  
Esta é uma lista dos tipos de registro de recurso DNS que são coletadas pela IPAM.  
  
-   Banco de dados AFS  
  
-   Endereço ATM  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   Host A ou AAAA  
  
-   Informações de host  
  
-   ISDN  
  
-   MX  
  
-   Servidores de nomes  
  
-   (Ponteiro)  
  
-   Pessoa responsável  
  
-   Direcionar  
  
-   Local do serviço  
  
-   SOA  
  
-   SRV  
  
-   Texto  
  
-   Serviços conhecidos  
  
-   WINS  
  
-   WINS-R  
  
-   X. 25  
  
No Windows Server 2016, IPAM fornece integração entre o inventário de endereço IP, zonas de DNS e registros de recurso DNS:  
  
-   Você pode usar IPAM para criar automaticamente um inventário de endereço IP de registros de recurso DNS.  
  
-   Você pode criar manualmente um inventário de endereço IP do DNS A e registros de recurso AAAA.  
  
-   Você pode exibir registros de recurso DNS para uma zona DNS específica e filtrar os registros com base no tipo, endereço IP, dados de registro de recurso e outras opções de filtragem.  
  
-   IPAM cria automaticamente um mapeamento entre os intervalos de endereço IP e zonas de pesquisa inversa DNS.  
  
-   IPAM cria endereços IP para os registros PTR que estão presentes na zona de pesquisa inversa e que estão incluído nesse intervalo de endereços IP. Você também pode modificar manualmente esse mapeamento se necessário.  
  
IPAM permite que você executar as seguintes operações nos registros de recurso do console IPAM.  
  
-   Criar registros de recurso do DNS  
  
-   Editar os registros de recurso DNS  
  
-   Excluir registros de recurso do DNS  
  
-   Criar registros de recurso associado  
  
IPAM registra automaticamente todas as alterações de configuração de DNS que você faça usando o console IPAM.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar IPAM](Manage-IPAM.md)  
  


