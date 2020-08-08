---
title: Gerenciamento de registro de recursos de DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f648649ac4f874089c958578ff1291f0e3dc9c66
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966433"
---
# <a name="dns-resource-record-management"></a>Gerenciamento de registro de recursos de DNS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece informações sobre como gerenciar registros de recursos de DNS usando o IPAM.

> [!NOTE]
> Além deste tópico, os tópicos de gerenciamento de registros de recursos de DNS a seguir estão disponíveis nesta seção.
>
> -   [Adicionar um registro de recurso DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)
> -   [Excluir registros de recursos de DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)
> -   [Filtrar a exibição de registros de recursos de DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)
> -   [Exibir registros de recurso DNS para um endereço IP específico](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)

## <a name="resource-record-management-overview"></a>Visão geral do gerenciamento de registros de recursos
Ao implantar o IPAM no Windows Server 2016, você pode executar a descoberta de servidor para adicionar servidores DHCP e DNS ao console de gerenciamento do servidor IPAM. Em seguida, o servidor IPAM coleta dinamicamente os dados DNS a cada seis horas a partir dos servidores DNS que ele está configurado para gerenciar. O IPAM mantém um banco de dados local no qual ele armazena esses dados DNS. O IPAM fornece uma notificação do dia e da hora em que os dados do servidor foram coletados, além de informá-lo sobre o próximo dia e hora em que a coleta de dados de servidores DNS ocorrerá.

A barra de status amarela na ilustração a seguir mostra o local da interface do usuário das notificações do IPAM.

![Notificações do IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)

Os dados DNS coletados incluem informações sobre a zona DNS e o registro de recurso. Você pode configurar o IPAM para coletar informações de zona do seu servidor DNS preferencial.  O IPAM coleta zonas com base em arquivo e Active Directory.

> [!NOTE]
> O IPAM coleta dados somente de servidores DNS da Microsoft ingressados no domínio. Servidores DNS de terceiros e servidores não ingressados no domínio não têm suporte pelo IPAM.

A seguir está uma lista de tipos de registro de recurso de DNS que são coletados pelo IPAM.

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

-   Rotear por

-   Local do serviço

-   SOA

-   SRV

-   Texto

-   Serviços bem conhecidos

-   WINS

-   WINS-R

-   X. 25

No Windows Server 2016, o IPAM fornece integração entre o inventário de endereço IP, Zonas DNS e registros de recursos de DNS:

-   Você pode usar o IPAM para criar automaticamente um inventário de endereços IP de registros de recursos de DNS.

-   Você pode criar manualmente um inventário de endereços IP dos registros de recursos DNS A e AAAA.

-   Você pode exibir os registros de recurso DNS para uma zona DNS específica e filtrar os registros com base no tipo, endereço IP, dados de registro de recurso e outras opções de filtragem.

-   O IPAM cria automaticamente um mapeamento entre intervalos de endereços IP e zonas de pesquisa inversa de DNS.

-   O IPAM cria endereços IP para os registros PTR que estão presentes na zona de pesquisa inversa e que são incluídos nesse intervalo de endereços IP. Você também pode modificar manualmente esse mapeamento, se necessário.

O IPAM permite que você execute as seguintes operações em registros de recursos do console do IPAM.

-   Criar registros de recursos de DNS

-   Editar registros de recursos de DNS

-   Excluir registros de recurso DNS

-   Criar registros de recursos associados

O IPAM registra automaticamente todas as alterações de configuração de DNS feitas usando o console do IPAM.

## <a name="see-also"></a>Consulte Também
[Gerenciar IPAM](Manage-IPAM.md)



