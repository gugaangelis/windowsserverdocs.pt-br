---
title: Novidades no IPAM
description: Este tópico descreve a funcionalidade de IPAM (gerenciamento de endereço IP) que é nova ou alterada no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d87c149bef3af0aa2b2b86aa5dfce58294b1634b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312302"
---
# <a name="whats-new-in-ipam"></a>Novidades no IPAM

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade de IPAM (gerenciamento de endereço IP) que é nova ou alterada no Windows Server 2016.  
  
O IPAM fornece recursos de monitoramento e administração altamente personalizáveis para o endereço IP e a infraestrutura de DNS em uma rede corporativa ou de provedor de serviços de nuvem (CSP). Você pode monitorar, auditar e gerenciar servidores que executam o protocolo DHCP e o DNS (sistema de nomes de domínio) usando o IPAM.  
  
## <a name="updates-in-ipam-server"></a><a name="BKMK_IPAM2012R2"></a>Atualizações no servidor IPAM  
A seguir estão os recursos novos e aprimorados para o IPAM no Windows Server 2016.  
  
|Recurso/funcionalidade|Novo ou melhorado|Descrição|  
|--------------------------|-------------------|---------------|  
|[Gerenciamento de endereço IP aprimorado](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Melhorado|Os recursos do IPAM são aprimorados para cenários como tratar sub-redes IPv4/32 e IPv6/128 e localizar sub-redes de endereço IP e intervalos livres em um bloco de endereço IP.|  
|[Gerenciamento avançado de serviços de DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Novo|O IPAM dá suporte ao registro de recurso DNS, encaminhador condicional e gerenciamento de zona DNS para servidores DNS integrados ao domínio Active Directory e com backup em arquivo.|  
|[DNS integrado, DHCP e gerenciamento de endereço IP (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Melhorado|Várias experiências novas e operações de gerenciamento de ciclo de vida integradas são habilitadas, como visualização de todos os registros de recursos DNS que pertencem a um endereço IP, inventário automatizado de endereços IP com base em registros de recursos DNS e gerenciamento de ciclo de vida de endereço IP para operações de DNS e DHCP.|  
|[Suporte a florestas múltiplas Active Directory](#bkmk_ad)|Novo|Você pode usar o IPAM para gerenciar os servidores DNS e DHCP de várias florestas do Active Directory quando há uma relação de confiança bidirecional entre a floresta em que o IPAM está instalado e cada uma das florestas remotas.|  
|[Limpar dados de utilização](#bkmk_purge)|Novo|Agora você pode reduzir o tamanho do banco de dados do IPAM, limpando o endereço IP de utilização que é mais antigo que uma data especificada.|  
|[Suporte do Windows PowerShell para controle de acesso baseado em função](#bkmk_ps)|Novo|Você pode usar o Windows PowerShell para definir escopos de acesso em objetos IPAM.|  
  
### <a name="enhanced-ip-address-management"></a><a name="EIP"></a>Gerenciamento de endereço IP aprimorado  
Os recursos a seguir melhoram os recursos de gerenciamento de endereço do IPAM.  
>[!NOTE]
>Para a referência de comando do IPAM do Windows PowerShell, consulte [cmdlets do servidor IPAM (gerenciamento de endereços IP) no Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Suporte para sub-redes/31,/32 e/128  
O IPAM no Windows Server 2016 agora dá suporte às sub-redes/31,/32 e/128. Por exemplo, uma sub-rede de dois endereços (/31 IPv4) pode ser necessária para um link ponto a ponto entre comutadores. Além disso, algumas opções podem exigir endereços de loopback único (/32 para IPv4,/128 para IPv6).  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Encontre sub-redes gratuitas com find-IpamFreeSubnet**  
  
Esse comando retorna sub-redes que estão disponíveis para alocação, dado um bloco de IP, um comprimento de prefixo e um número de sub-redes solicitadas.   
  
Se o número de sub-redes disponíveis for menor que o número de sub-redes solicitadas, as sub-redes disponíveis serão retornadas com um aviso indicando que o número disponível é menor que o número solicitado.  
  
>[!NOTE]
>Essa função não aloca, na verdade, as sub-redes, ela apenas relata sua disponibilidade. No entanto, a saída do cmdlet pode ser canalizada para o comando **Add-IpamSubnet** para criar a sub-rede.  
  
Para obter mais informações, consulte [Find-IpamFreeSubnet](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeSubnet).  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Localizar intervalos de endereços livres com find-IpamFreeRange**  
  
Esse novo comando retorna intervalos de endereços IP disponíveis, dado uma sub-rede IP, o número de endereços necessários no intervalo e o número de intervalos solicitados.   
  
O comando procura uma série contínua de endereços IP não alocados que correspondam ao número de endereços solicitados. O processo é repetido até que o número solicitado de intervalos seja encontrado ou até que não haja mais nenhum intervalo de endereços disponível disponível.  
  
> [!NOTE]
> Essa função não aloca, na verdade, os intervalos, apenas relata sua disponibilidade. No entanto, a saída do cmdlet pode ser canalizada para o comando **Add-IpamRange** para criar o intervalo.  
  
Para obter mais informações, consulte [Find-IpamFreeRange](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeRange).  
  
### <a name="enhanced-dns-service-management"></a><a name="EDNS"></a>Gerenciamento avançado de serviços de DNS  
O IPAM no Windows Server 2016 agora dá suporte à descoberta de servidores DNS associados ao domínio e baseados em arquivo em uma floresta Active Directory na qual o IPAM está em execução.  
  
Além disso, as seguintes funções DNS foram adicionadas:  
  
-   As zonas DNS e a coleção de registros de recursos (além daquelas pertencentes ao DNSSEC) de servidores DNS que executam o Windows Server 2008 ou posterior.  
  
-   Configurar (criar, modificar e excluir) Propriedades e operações em todos os tipos de registros de recursos (diferentes daqueles pertencentes ao DNSSEC).  
  
-   Configure (crie, modifique, exclua) as propriedades e operações em todos os tipos de zonas DNS, incluindo zonas secundárias primárias e de stub).  
  
-   Tarefas disparadas em zonas secundárias e de stub, independentemente de serem zonas de pesquisa direta ou inversa. Por exemplo, tarefas como **transferir do mestre** ou **transferir uma nova cópia da zona do mestre**.  
  
-   Controle de acesso baseado em função para a configuração de DNS com suporte (registros DNS e zonas DNS).  
  
-   Coleção e configuração de encaminhadores condicionais (criar, excluir, editar).  
  
### <a name="integrated-dns-dhcp-and-ip-address-ddi-management"></a><a name="DDI"></a>DNS integrado, DHCP e gerenciamento de endereço IP (DDI)  
Ao exibir um endereço IP no inventário de endereços IP, você tem a opção na exibição de detalhes para ver todos os registros de recursos de DNS associados ao endereço IP.  
  
Como parte da coleta de registro de recurso DNS, o IPAM coleta os registros PTR para as zonas de pesquisa inversa de DNS. Para todas as zonas de pesquisa inversa que são mapeadas para qualquer intervalo de endereços IP, o IPAM cria os registros de endereço IP para todos os registros PTR pertencentes a essa zona no intervalo de endereços IP mapeado correspondente. Se o endereço IP já existir, o registro PTR será simplesmente associado a esse endereço IP. Os endereços IP não serão criados automaticamente se a zona de pesquisa inversa não estiver mapeada para nenhum intervalo de endereços IP.  
  
Quando um registro PTR é criado em uma zona de pesquisa inversa por meio do IPAM, o inventário de endereço IP é atualizado da mesma maneira, conforme descrito acima. Durante a coleta subsequente, como o endereço IP já existirá no sistema, o registro PTR será simplesmente mapeado com esse endereço IP.  
  
### <a name="multiple-active-directory-forest-support"></a><a name="bkmk_ad"></a>Suporte a florestas múltiplas Active Directory  
No Windows Server 2012 R2, o IPAM conseguiu descobrir e gerenciar servidores DNS e DHCP pertencentes à mesma floresta Active Directory que o servidor IPAM. Agora você pode gerenciar servidores DNS e DHCP que pertencem a uma floresta diferente do AD quando ele tem uma relação de confiança bidirecional com a floresta em que o servidor IPAM está instalado. Você pode ir para a caixa de diálogo **Configurar descoberta de servidor** e adicionar domínios de outras florestas confiáveis que você deseja gerenciar. Depois que os servidores são descobertos, a experiência de gerenciamento é a mesma para os servidores que pertencem à mesma floresta em que o IPAM está instalado.  
  
Para obter mais informações, consulte [gerenciar recursos em várias florestas de Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="purge-utilization-data"></a><a name="bkmk_purge"></a>Limpar dados de utilização  
Limpar dados de utilização permite que você reduza o tamanho do banco do dados IPAM excluindo dados antigos de utilização de endereço IP. Para executar a exclusão de dados, você especifica uma data e o IPAM exclui todas as entradas de banco de dado que são mais antigas ou iguais à data fornecida.   
  
Para obter mais informações, consulte [limpar dados de utilização](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="windows-powershell-support-for-role-based-access-control"></a><a name="bkmk_ps"></a>Suporte do Windows PowerShell para controle de acesso baseado em função  
Agora você pode usar o Windows PowerShell para configurar o controle de acesso baseado em função. Você pode usar comandos do Windows PowerShell para recuperar objetos DNS e DHCP no IPAM e alterar seus escopos de acesso. Por isso, você pode escrever scripts do Windows PowerShell para atribuir escopos de acesso aos objetos a seguir.  
  
-   Espaço de endereço IP  
  
-   Bloco de endereço IP  
  
-   Sub-redes de endereço IP  
  
-   Intervalos de endereços IP  
  
-   Servidores DNS  
  
-   Zonas do DNS  
  
-   Encaminhadores condicionais de DNS  
  
-   Registros de recursos de DNS  
  
-   Servidores DHCP  
  
-   Superescopos DHCP  
  
-   Escopos DHCP  
  
Para obter mais informações, consulte [gerenciar o controle de acesso baseado em função com](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) os cmdlets do servidor do Windows PowerShell e do [IPAM (gerenciamento de endereço IP) no Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  

