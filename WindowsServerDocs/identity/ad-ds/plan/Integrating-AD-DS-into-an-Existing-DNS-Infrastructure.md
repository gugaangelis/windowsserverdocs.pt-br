---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Integrando o AD DS a uma infraestrutura de DNS existente
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3aeac10b8c92c151fe57bcb935e685f7f1a2bfa2
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965518"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integrando o AD DS a uma infraestrutura de DNS existente

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização já tiver um serviço de servidor de DNS (sistema de nomes de domínio) existente, o proprietário do DNS para Active Directory Domain Services (AD DS) deverá funcionar com o proprietário do DNS da sua organização para integrar AD DS à infraestrutura existente. Isso envolve a criação de um servidor DNS e a configuração do cliente DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Criando uma configuração de servidor DNS  
Ao integrar AD DS com um namespace DNS existente, recomendamos que você faça o seguinte:  
  
-   Instale o serviço do servidor DNS em cada controlador de domínio na floresta. Isso fornece tolerância a falhas se um dos servidores DNS não estiver disponível. Dessa forma, os controladores de domínio não precisam confiar em outros servidores DNS para a resolução de nomes. Isso também simplifica o ambiente de gerenciamento, pois todos os controladores de domínio têm uma configuração uniforme.  
  
-   Configure o controlador de domínio raiz da floresta Active Directory para hospedar a zona DNS para a floresta Active Directory.  
  
-   Configure os controladores de domínio para cada domínio regional para hospedar as zonas DNS que correspondem aos seus domínios de Active Directory.  
  
-   Configure a zona que contém os Active Directory registros de localizador de toda a floresta (ou seja, o _msdcs.* nomedafloresta* Zone) a ser replicada em todos os servidores DNS na floresta usando a partição de diretório de aplicativo DNS de toda a floresta.  
  
    > [!NOTE]  
    > Quando o serviço do servidor DNS é instalado com o Assistente para Instalação do Active Directory Domain Services (recomendamos essa opção), todas as tarefas anteriores são executadas automaticamente. Para obter mais informações, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)).  
  
    > [!NOTE]  
    > AD DS usa registros de localizador em toda a floresta para permitir que os parceiros de replicação encontrem um ao outro e para permitir que os clientes encontrem servidores de catálogo global. AD DS armazena os registros de localizador de toda a floresta no _msdcs. zona *nomedafloresta* . Como as informações na zona devem estar amplamente disponíveis, essa zona é replicada para todos os servidores DNS na floresta por meio da partição de diretório de aplicativos DNS em toda a floresta.  
  
A estrutura DNS existente permanece intacta. Você não precisa mover nenhum servidor ou zona. Você simplesmente precisa criar uma delegação para suas zonas de DNS integradas ao Active Directory da sua hierarquia de DNS existente.  
  
## <a name="creating-the-dns-client-configuration"></a>Criando a configuração do cliente DNS  
Para configurar o DNS em computadores cliente, o DNS para AD DS proprietário deve especificar o esquema de nomenclatura do computador e como os clientes irão localizar servidores DNS. A tabela a seguir lista nossas configurações recomendadas para esses elementos de design.  
  
|Elemento de design|Configuração|  
|------------------|-----------------|  
|Nomeação do computador|Use a nomenclatura padrão. Quando um computador com Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista ingressa em um domínio, o computador atribui a ele mesmo um FQDN (nome de domínio totalmente qualificado) que compreende o nome do host do computador e o nome do domínio de Active Directory.|  
|Configuração do resolvedor de cliente|Configure os computadores cliente para apontarem para qualquer servidor DNS na rede.|  
  
> [!NOTE]  
> Active Directory clientes e controladores de domínio podem registrar dinamicamente seus nomes DNS mesmo se eles não estiverem apontando para o servidor DNS que é autoritativo para seus nomes.  
  
Um computador pode ter um nome DNS existente diferente se a organização tiver, estaticamente, registrado o computador no DNS ou se a organização tiver implantado anteriormente uma solução de DHCP (protocolo de configuração de host dinâmico) integrada. Se os computadores cliente já tiverem um nome DNS registrado, quando o domínio ao qual eles são ingressados for atualizado para o Windows Server 2008 AD DS, eles terão dois nomes diferentes:  
  
-   O nome DNS existente  
  
-   O novo FQDN (nome de domínio totalmente qualificado)  
  
Os clientes ainda podem ser localizados por um dos nomes. Qualquer solução de DNS/DHCP integrada ou DNS existente é deixada intacta. Os novos nomes primários são criados automaticamente e atualizados por meio da atualização dinâmica. Eles são limpos automaticamente por meio de eliminação.  
  
Se você quiser tirar proveito da autenticação Kerberos ao se conectar a um servidor que executa o Windows 2000, o Windows Server 2003 ou o Windows Server 2008, você deve garantir que o cliente se conecte ao servidor usando o nome primário.  
  
