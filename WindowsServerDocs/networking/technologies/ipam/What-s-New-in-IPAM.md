---
title: O que há de novo no IPAM
description: Este tópico descreve a funcionalidade de gerenciamento de endereço IP (IPAM) que é novo ou alterado no Windows Server 2016.
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
ms.openlocfilehash: b90cd1ab223e38cbf5933b58a594b32d5e3d4858
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ipam"></a>O que há de novo no IPAM

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico descreve a funcionalidade de gerenciamento de endereço IP (IPAM) que é novo ou alterado no Windows Server 2016.  
  
IPAM fornece recursos de monitoramento e administrativos altamente personalizáveis para o endereço IP e a infraestrutura DNS em uma rede Enterprise ou o provedor de serviços de nuvem (CSP). Você pode monitorar, auditar e gerenciar servidores que executam Dynamic Host Configuration Protocol (DHCP) e sistema de nome de domínio (DNS) usando IPAM.  
  
## <a name="BKMK_IPAM2012R2"></a>Atualizações no servidor IPAM  
A seguir estão os recursos novos e aprimorados para IPAM no Windows Server 2016.  
  
|Recursos/funcionalidades|Novos ou melhorados|Descrição|  
|--------------------------|-------------------|---------------|  
|[Gerenciamento avançado de endereço IP](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Aprimorado|Recursos IPAM aperfeiçoados para cenários como lidar com sub-redes /32 IPv4 e IPv6 /128 e localização de sub-redes gratuitos de endereço IP e intervalos de em um bloco de endereço IP.|  
|[Gerenciamento avançado de serviços DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Novo|IPAM dá suporte a registro de recurso DNS, o encaminhador condicional e o gerenciamento de zona DNS para os servidores DNS integrada ao Active Directory e feito o arquivo ingressado no domínio.|  
|[Integrado DNS, DHCP e o endereço IP gerenciamento (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Aprimorado|Várias novas experiências e gerenciamento de ciclo de vida integrado operações forem habilitadas, como visualizar todos os registros de recurso DNS que pertencem a um endereço IP, automatizada inventário dos endereços IP com base em registros de recurso DNS e gerenciamento de ciclo de vida de endereços IP para operações de DNS e DHCP.|  
|[Suporte a vários floresta do Active Directory](#bkmk_ad)|Novo|Você pode usar IPAM para gerenciar os servidores DHCP e DNS de várias florestas do Active Directory quando há uma relação de confiança bidirecional entre a floresta onde IPAM está instalado e cada uma das florestas remotas.|  
|[Limpar dados de utilização](#bkmk_purge)|Novo|Agora você pode reduzir o tamanho do banco de dados IPAM limpar os dados de utilização de endereço IP anterior a uma data que você especificar.|  
|[Suporte do Windows PowerShell para controle de acesso com base em função](#bkmk_ps)|Novo|Você pode usar o Windows PowerShell para definir os escopos de acesso em objetos IPAM.|  
  
### <a name="EIP"></a>Gerenciamento avançado de endereço IP  
Os seguintes recursos melhorar os recursos de gerenciamento de endereço IPAM.  
>[!NOTE]
>Para a referência de comando IPAM Windows PowerShell, consulte [Cmdlets de servidor de gerenciamento de endereço IP (IPAM) no Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Suporte para /31, /32 e /128 sub-redes  
IPAM no Windows Server 2016 agora dá suporte à /31, /32 e /128 sub-redes. Por exemplo, uma sub-rede dois endereços (/ 31 IPv4) podem ser necessárias para um link de ponto a ponto entre comutadores. Além disso, algumas opções podem exigir endereços de loopback único (:/ 32 para IPv4, /128 para o IPv6).  
  
#### **<a name="find-free-subnets-with-find-ipamfreesubnet"></a>Encontre sub-redes gratuitos com IpamFreeSubnet localizar**  
  
Esse comando retorna sub-redes que estão disponíveis para alocação, dado um bloco IP, o comprimento do prefixo e o número de sub-redes solicitadas.   
  
Se o número de sub-redes disponíveis for menor que o número de sub-redes solicitadas, as sub-redes disponíveis são retornadas com um aviso indicando que o número disponível é menor que o número solicitado.  
  
>[!NOTE]
>Essa função não alocar as sub-redes na verdade, ela só relata sua disponibilidade. No entanto, a saída do cmdlet pode ser passada para o **IpamSubnet adicionar** comando para criar a sub-rede.  
  
Para obter mais informações, consulte [localizar IpamFreeSubnet](https://technet.microsoft.com/library/mt712782.aspx).  
  
#### **<a name="find-free-address-ranges-with-find-ipamfreerange"></a>Encontre os intervalos de endereço gratuito com IpamFreeRange localizar**  
  
Esse novo comando retorna disponível intervalos de endereços IP dada uma sub-rede IP, o número de endereços que são necessárias no intervalo e o número de intervalos solicitado.   
  
O comando procura por uma série contínua de endereços IP não alocados que corresponde ao número de endereços solicitados. O processo é repetido até que o número de intervalos solicitado é encontrado, ou até que não estão disponíveis sem mais intervalos de endereços disponíveis.  
  
> [!NOTE]
> Essa função não aloca os intervalos na verdade, ela só relata sua disponibilidade. No entanto, a saída do cmdlet pode ser passada para o **IpamRange adicionar** comando para criar o intervalo.  
  
Para obter mais informações, consulte [localizar IpamFreeRange](https://technet.microsoft.com/library/mt712772.aspx).  
  
### <a name="EDNS"></a>Gerenciamento avançado de serviços DNS  
IPAM no Windows Server 2016 agora dá suporte a descoberta de servidores DNS baseados em arquivo, domínio em uma floresta do Active Directory no qual IPAM está em execução.  
  
Além disso, as seguintes funções DNS foram adicionadas:  
  
-   Zonas de DNS e recurso registra coleção (que não sejam aqueles pertencentes a DNSSEC) dos servidores DNS executando o Windows Server 2008 ou posterior.  
  
-   Configurar (criar, modificar e excluir) propriedades e operações em todos os tipos de registros de recurso (que não sejam aqueles pertencentes a DNSSEC).  
  
-   Configurar (criar, modificar, excluir) propriedades e operações em todos os tipos de zonas DNS incluindo principal secundário e as zonas de Stub).  
  
-   Disparada tarefas secundária e zonas de stub, independentemente se eles são para a frente ou zonas de pesquisa inversa. Por exemplo, tarefas como **transferir do mestre** ou **transferir nova cópia da zona de mestre**.  
  
-   Controle de acesso baseado na função para a configuração do DNS com suporte (registros de DNS e zonas DNS).  
  
-   Coleção de encaminhadores condicionais e configuração (criar, excluir, editar).  
  
### <a name="DDI"></a>Integrado DNS, DHCP e o endereço IP gerenciamento (DDI)  
Quando você exibe um endereço IP no inventário do endereço IP, você tem a opção na exibição de detalhes para ver todos os registros de recurso DNS associados ao endereço IP.  
  
Como parte DNS recurso registro coleção IPAM coleta os registros PTR para as zonas de pesquisa inversa DNS. Para todas as zonas de pesquisa reversa que são mapeadas para qualquer intervalo de endereços IP, IPAM cria os registros de endereço IP para todos os registros PTR pertencentes a essa região no intervalo de endereços IP mapeado correspondente. Se o endereço IP já existir, o registro de Ponteiro é simplesmente associado esse endereço IP. Os endereços IP não são criados automaticamente se a zona de pesquisa inversa não é mapeada para qualquer intervalo de endereços IP.  
  
Quando um registro de Ponteiro é criado em uma zona de pesquisa reversa por meio de IPAM, o inventário de endereço IP é atualizado da mesma forma, conforme descrito acima. Durante a coleta de subsequente, desde que o endereço IP já existirá no sistema, o registro de Ponteiro será simplesmente mapeado com esse endereço IP.  
  
### <a name="bkmk_ad"></a>Suporte a vários floresta do Active Directory  
No Windows Server 2012 R2, IPAM foi capaz de detectar e gerenciar servidores DNS e DHCP pertencentes à floresta do Active Directory como o servidor IPAM. Agora você pode gerenciar servidores DNS e DHCP pertencentes a uma floresta do AD diferente quando ela tem uma relação de confiança bidirecional com a floresta em que o servidor IPAM está instalado. Você pode ir para o **configurar servidor descoberta** caixa de diálogo caixa e adicionar domínios do outro trusted florestas que você deseja gerenciar. Depois que os servidores são detectados, a experiência de gerenciamento é o mesmo para os servidores que pertencem à mesma floresta onde IPAM está instalado.  
  
Para obter mais informações, consulte [gerenciar recursos em várias florestas do Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Limpar dados de utilização  
Dados de utilização da limpeza lhe permite reduzir o tamanho do banco de dados IPAM excluindo os dados de utilização de endereço IP antigos. Para executar a exclusão de dados, você pode especificar uma data e IPAM exclui todas as entradas de banco de dados que são mais antigas que ou igual à data que você fornecer.   
  
Para obter mais informações, consulte [limpar dados de utilização da](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Suporte do Windows PowerShell para controle de acesso com base em função  
Agora você pode usar o Windows PowerShell para configurar o controle de acesso com base em função. Você pode usar comandos do Windows PowerShell para recuperar os objetos DNS e DHCP IPAM e alterar seus escopos de acesso. Por isso, você pode gravar scripts do Windows PowerShell para atribuir os escopos de acesso para os seguintes objetos.  
  
-   Espaço de endereço IP  
  
-   Bloco de endereço IP  
  
-   Sub-redes de endereço IP  
  
-   Intervalos de endereço IP  
  
-   Servidores DNS  
  
-   Zonas DNS  
  
-   Encaminhadores condicionais DNS  
  
-   Registros de recurso do DNS  
  
-   Servidores DHCP  
  
-   Superescopos DHCP  
  
-   Escopos DHCP  
  
Para obter mais informações, consulte [gerenciar função com base em controle de acesso com o Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) e [Cmdlets de servidor de gerenciamento de endereço IP (IPAM) no Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  

