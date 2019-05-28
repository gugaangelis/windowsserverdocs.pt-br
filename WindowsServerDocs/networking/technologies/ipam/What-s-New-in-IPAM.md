---
title: Novidades no IPAM
description: Este tópico descreve a funcionalidade de gerenciamento de endereço IP (IPAM) nova ou alterada no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04838cba63805d20ba31629ed9c8e95290046320
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624680"
---
# <a name="whats-new-in-ipam"></a>Novidades no IPAM

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade de gerenciamento de endereço IP (IPAM) nova ou alterada no Windows Server 2016.  
  
O IPAM fornece funcionalidades de administração e monitoramento altamente personalizáveis para o endereço IP e a infraestrutura DNS em uma rede Enterprise ou provedor de serviços de nuvem (CSP). Você pode monitorar, auditar e gerenciar servidores que executam o protocolo de configuração de Host dinâmico (DHCP) e o sistema de nome de domínio (DNS) usando o IPAM.  
  
## <a name="BKMK_IPAM2012R2"></a>Atualizações no servidor IPAM  
A seguir estão os recursos novos e aprimorados do IPAM no Windows Server 2016.  
  
|Recurso/funcionalidade|Novo ou melhorado|Descrição|  
|--------------------------|-------------------|---------------|  
|[Gerenciamento aprimorado de endereço IP](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Melhorado|Funcionalidades do IPAM são aprimoradas para cenários como o tratamento de sub-redes /32 IPv4 e IPv6 /128 e Localizando livres sub-redes de endereço IP e intervalos em um bloco de endereço IP.|  
|[Gerenciamento aprimorado de serviço DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Novo|O IPAM dá suporte a registro de recurso DNS encaminhador condicional e gerenciamento de zona DNS para os servidores DNS integrado ao Active Directory e o backup em arquivo ingressado no domínio.|  
|[Integrado DNS, DHCP e endereços IP gerenciamento (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Melhorado|Várias novas experiências e operações de gerenciamento do ciclo de vida integrada estão habilitadas, como visualizar todos os recursos DNS registros que pertencem a um IP endereço, automatizada de inventário de endereços IP com base em registros de recursos DNS e o gerenciamento de ciclo de vida do endereço IP para operações de DNS e DHCP.|  
|[Suporte a várias florestas do Active Directory](#bkmk_ad)|Novo|Você pode usar o IPAM para gerenciar os servidores DNS e DHCP de várias florestas do Active Directory quando há uma relação de confiança bidirecional entre a floresta em que o IPAM é instalado e cada uma das florestas remotas.|  
|[Limpar dados de utilização](#bkmk_purge)|Novo|Agora você pode reduzir o tamanho do banco de dados IPAM, limpando os dados de utilização de endereço IP com mais de uma data que você especificar.|  
|[Suporte do Windows PowerShell para controle de acesso baseado em função](#bkmk_ps)|Novo|Você pode usar o Windows PowerShell para definir escopos de acesso nos objetos do IPAM.|  
  
### <a name="EIP"></a>Gerenciamento aprimorado de endereço IP  
Os seguintes recursos melhoram os recursos de gerenciamento de endereço do IPAM.  
>[!NOTE]
>Para a referência de comandos do PowerShell do Windows de IPAM, consulte [Cmdlets do servidor de gerenciamento de endereço IP (IPAM) no Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/ipamserver/).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Suporte para /31, /32 e /128 sub-redes  
IPAM no Windows Server 2016 agora dá suporte a /31, /32 e /128 sub-redes. Por exemplo, uma sub-rede do endereço de dois (/ 31 IPv4) podem ser necessárias para uma conexão ponto a ponto entre comutadores. Além disso, algumas opções podem exigir endereços de loopback único (/ 32 para IPv4, / 128 para IPv6).  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Encontrar sub-redes gratuitas com IpamFreeSubnet Find**  
  
Esse comando retorna as sub-redes que estão disponíveis para alocação, dado um bloco IP, o comprimento do prefixo e o número de sub-redes solicitadas.   
  
Se o número de sub-redes disponíveis é menor que o número de sub-redes solicitados, as sub-redes disponíveis serão retornadas com um aviso indicando que o número disponível é menor que o número solicitado.  
  
>[!NOTE]
>Essa função não aloca as sub-redes na verdade, ele apenas relata sua disponibilidade. No entanto, a saída do cmdlet pode ser transferida para o **IpamSubnet adicionar** comando para criar a sub-rede.  
  
Para obter mais informações, consulte [Find-IpamFreeSubnet](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeSubnet).  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Localizar os intervalos de endereços livres com IpamFreeRange Find**  
  
Esse novo comando retorna disponíveis intervalos de endereços IP fornecido a uma sub-rede IP, o número de endereços que são necessários no intervalo e o número de intervalos solicitados.   
  
O comando procura uma série contínua de endereços IP não alocados que corresponde ao número de endereços solicitados. O processo é repetido até que o número solicitado de intervalos é encontrado ou até que há não mais disponíveis de intervalos de endereços disponíveis.  
  
> [!NOTE]
> Essa função não aloca, na verdade, os intervalos, ele apenas relata sua disponibilidade. No entanto, a saída do cmdlet pode ser transferida para o **Add-IpamRange** comando para criar o intervalo.  
  
Para obter mais informações, consulte [Find-IpamFreeRange](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeRange).  
  
### <a name="EDNS"></a>Gerenciamento aprimorado de serviço DNS  
O IPAM no Windows Server 2016 agora dá suporte a descoberta de servidores DNS baseados em arquivo, ingressado no domínio em uma floresta do Active Directory em que o IPAM está em execução.  
  
Além disso, as funções DNS a seguir foram adicionadas:  
  
-   As zonas DNS e o recurso registra a coleção (diferentes daqueles que pertencem a DNSSEC) dos servidores DNS executando o Windows Server 2008 ou posterior.  
  
-   Configurar (criar, modificar e excluir) propriedades e operações em todos os tipos de registros de recursos (diferentes daqueles que pertencem a DNSSEC).  
  
-   Configurar (criar, modificar, excluir) propriedades e operações em todos os tipos de zonas DNS, incluindo a primária secundária e zonas de Stub).  
  
-   Disparado tarefas no secundário e zonas de stub, independentemente se eles são para frente ou zonas de pesquisa inversa. Por exemplo, as tarefas, como **transferir do mestre** ou **transferir uma cópia nova da zona do mestre**.  
  
-   Controle de acesso baseado em função para a configuração de DNS com suporte (registros de DNS e zonas DNS).  
  
-   Coleção de encaminhadores condicionais e de configuração (criar, excluir, editar).  
  
### <a name="DDI"></a>Integrado DNS, DHCP e endereços IP gerenciamento (DDI)  
Quando você exibe um endereço IP no estoque de endereços IP, você tem a opção na exibição de detalhes para ver todos os registros de recurso DNS associados ao endereço IP.  
  
Como parte da coleção de registros de recursos DNS, o IPAM coleta os registros PTR para as zonas de pesquisa inversa de DNS. Para todas as zonas de pesquisa inversa que são mapeadas para qualquer intervalo de endereços IP, o IPAM cria os registros de endereço IP para todos os registros PTR que pertencem a essa região no intervalo de endereços IP mapeado correspondente. Se o endereço IP já existir, o registro PTR é simplesmente associado a esse endereço IP. Os endereços IP não serão criados automaticamente se a zona de pesquisa inversa não é mapeada para qualquer intervalo de endereços IP.  
  
Quando um registro PTR é criado em uma zona de pesquisa inversa por meio do IPAM, o inventário de endereço IP é atualizado da mesma forma, conforme descrito acima. Durante a coleta subsequente, uma vez que o endereço IP será já existe no sistema, o registro PTR será simplesmente mapeado com esse endereço IP.  
  
### <a name="bkmk_ad"></a>Suporte a várias florestas do Active Directory  
No Windows Server 2012 R2, o IPAM foi capaz de descobrir e gerenciar servidores DNS e DHCP que pertencem à mesma floresta do Active Directory como o servidor IPAM. Agora você pode gerenciar servidores DNS e DHCP que pertencem a uma floresta diferente do AD, quando ele tem uma relação de confiança bidirecional com a floresta em que o servidor IPAM está instalado. Você pode ir para o **configurar a descoberta de servidor** caixa de diálogo caixa e adicionar domínios da outra confiável florestas que você deseja gerenciar. Após a descoberta de servidores, a experiência de gerenciamento é da mesma maneira que os servidores que pertencem à mesma floresta em que o IPAM é instalado.  
  
Para obter mais informações, consulte [gerenciar recursos em várias florestas do Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Limpar dados de utilização  
Limpe os dados de utilização permite reduzir o tamanho de banco de dados do IPAM, excluindo dados de utilização de endereço IP antigos. Para executar a exclusão de dados, você pode especificar uma data e IPAM exclui todas as entradas de banco de dados mais antigos que ou igual à data que você fornecer.   
  
Para obter mais informações, consulte [limpar dados de utilização](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Suporte do Windows PowerShell para controle de acesso baseado em função  
Agora você pode usar o Windows PowerShell para configurar o controle de acesso baseado em função. Você pode usar comandos do Windows PowerShell para recuperar objetos DNS e DHCP no IPAM e altere seus escopos de acesso. Por isso, você pode escrever scripts do Windows PowerShell para atribuir escopos de acesso aos objetos a seguir.  
  
-   Espaço de endereço IP  
  
-   Bloco de endereço IP  
  
-   Sub-redes de endereço IP  
  
-   Intervalos de endereços IP  
  
-   Servidores DNS  
  
-   Zonas do DNS  
  
-   Encaminhadores condicionais DNS  
  
-   Registros de recurso DNS  
  
-   Servidores DHCP  
  
-   Superescopos DHCP  
  
-   Escopos do DHCP  
  
Para obter mais informações, consulte [gerenciar função com base em controle de acesso com o Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) e [Cmdlets do servidor de gerenciamento de endereço IP (IPAM) no Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/ipamserver/).  

