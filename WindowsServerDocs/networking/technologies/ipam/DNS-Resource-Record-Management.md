---
title: Gerenciamento de registro de recursos de DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2db802fa928a4051fbe409fc0ba60d774bb0428
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284049"
---
# <a name="dns-resource-record-management"></a>Gerenciamento de registro de recursos de DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico fornece informações sobre como gerenciar registros de recursos DNS usando o IPAM.  
  
> [!NOTE]  
> Além deste tópico, os seguintes tópicos de gerenciamento de registros de recursos DNS estão disponíveis nesta seção.  
>   
> -   [Adicionar um registro de recurso DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Excluir registros de recurso DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrar a exibição de registros de recurso DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Exibir registros de recurso DNS para um endereço IP específico](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Visão geral do gerenciamento de registros de recursos  
Quando você implanta o IPAM no Windows Server 2016, você pode executar a descoberta do servidor para adicionar servidores DNS e DHCP para o console de gerenciamento do servidor IPAM. O servidor IPAM, em seguida, dinamicamente coleta dados DNS a cada seis horas dos servidores DNS que ele está configurado para gerenciar. IPAM mantém um banco de dados local em que ele armazena esses dados DNS. O IPAM fornece notificação do dia e hora em que os dados do servidor foi coletados, bem como informando que o próximo dia e hora quando ocorrerá a coleta de dados de servidores DNS.  
  
A barra de status amarelo na ilustração a seguir mostra a localização da interface do usuário de notificações do IPAM.  
  
![Notificações do IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Os dados DNS que são coletados incluem informações de registro recursos e a zona DNS. Você pode configurar IPAM para coletar informações sobre a zona de seu servidor DNS preferencial.  O IPAM coleta zonas de diretório baseado em arquivo e o Active Directory.  
  
> [!NOTE]  
> O IPAM coleta dados apenas de servidores ingressados no domínio do DNS da Microsoft. Não há suporte para servidores DNS de terceiros e fora do domínio unidas pelo IPAM.  
  
A seguir está uma lista de tipos de registro de recurso DNS que são coletados pelo IPAM.  
  
-   Banco de dados AFS  
  
-   Endereço ATM  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   Host A ou AAAA  
  
-   Informações do host  
  
-   ISDN  
  
-   MX  
  
-   Servidores de Nome  
  
-   Ponteiro (PTR)  
  
-   Pessoa responsável  
  
-   Rotear por meio de  
  
-   Local do serviço  
  
-   SOA  
  
-   SRV  
  
-   Text  
  
-   Serviços conhecidos  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
No Windows Server 2016, o IPAM fornece a integração entre o inventário de endereço IP, as zonas DNS e registros de recurso DNS:  
  
-   Você pode usar o IPAM para criar automaticamente um inventário de endereço IP de registros de recursos DNS.  
  
-   Você pode criar manualmente um inventário de endereço IP de DNS A e registros de recurso AAAA.  
  
-   Você pode exibir os registros de recurso DNS para uma zona DNS específica e filtrar os registros com base no tipo, endereço IP, os dados de registro de recursos e outras opções de filtragem.  
  
-   IPAM automaticamente cria um mapeamento entre intervalos de endereços IP e zonas de pesquisa DNS inversa.  
  
-   IPAM cria endereços IP para os registros PTR que estão presentes na zona de pesquisa inversa e que estão incluídos nesse intervalo de endereço IP. Você pode modificar esse mapeamento também manualmente se necessário.  
  
O IPAM permite que você realize as seguintes operações em registros de recursos do console do IPAM.  
  
-   Criar registros de recurso DNS  
  
-   Editar os registros de recurso DNS  
  
-   Excluir registros de recurso DNS  
  
-   Criar registros de recursos associados  
  
IPAM registra automaticamente todas as alterações de configuração de DNS que você faça usando o console do IPAM.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar IPAM](Manage-IPAM.md)  
  


