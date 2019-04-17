---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: "Integração do AD DS em uma infraestrutura DNS existente"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ab8d92a237a6d1fb623d9f4bb7dcc88561edf742
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integração do AD DS em uma infraestrutura DNS existente

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização já tiver um serviço de servidor de sistema de nome de domínio (DNS) existente, o proprietário de serviços de domínio do Active Directory (AD DS) no DNS deve funcionar com o proprietário do DNS para sua organização integrar AD DS a infraestrutura existente. Isso envolve a criação de um servidor DNS e configuração do cliente DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Criando uma configuração de servidor DNS  
Ao integrar o AD DS com um namespace DNS existente, recomendamos que você faça o seguinte:  
  
-   Instale o serviço de servidor DNS em todos os controladores de domínio na floresta. Isso proporciona tolerância se um dos servidores DNS não estiver disponível. Dessa forma, os controladores de domínio não precisa dependem de outros servidores DNS para resolução de nomes. Isso também simplifica o ambiente de gerenciamento porque todos os controladores de domínio tem uma configuração uniforme.  
  
-   Configure o controlador de domínio do Active Directory floresta raiz para hospedar a zona DNS de floresta do Active Directory.  
  
-   Configure os controladores de domínio para cada domínio regional hospedar as zonas DNS que correspondem aos seus domínios do Active Directory.  
  
-   Configurar a região que contém os registros de localizador de toda a floresta do Active Directory (ou seja, o msdcs.* NomeDaFloresta* zona) para replicar em cada servidor DNS na floresta usando a partição de diretório do aplicativo de DNS toda a floresta.  
  
    > [!NOTE]  
    > Quando o serviço de servidor DNS é instalado com o assistente domínio Active Directory serviços de instalação (recomendamos essa opção), todas as tarefas anteriores são executadas automaticamente. Para obter mais informações, consulte [Implantando um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > AD DS usa registros de localizador de toda a floresta para permitir que os parceiros de replicação para encontrar uns aos outros e para permitir que os clientes encontrar servidores de catálogo global. AD DS armazena os registros de localizador de toda a floresta em _ msdcs. *NomeDaFloresta* zona. Como as informações na região devem ser amplamente disponíveis, essa zona é replicada para todos os servidores DNS na floresta por meio de toda a floresta DNS partição.  
  
A estrutura DNS existente permanece intacta. Você não precisa mover servidores ou zonas. Você simplesmente precisa criar uma delegação para as zonas DNS integradas ao Active Directory do seu hierarquia DNS existente.  
  
## <a name="creating-the-dns-client-configuration"></a>Criando a configuração do cliente DNS  
Para configurar o DNS em computadores cliente, o DNS para proprietário do AD DS deve especificar o computador de nomenclatura esquema e como os clientes irão localizar servidores DNS. A tabela a seguir lista nossas configurações recomendadas para esses elementos de design.  
  
|Elemento de design|Configuração|  
|------------------|-----------------|  
|Nomenclatura do computador|Use o padrão de nomenclatura. Quando um Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista com base em computador ingressa em um domínio, o computador em si atribui um nome de domínio totalmente qualificado (FQDN) que compreende o nome do host do computador e o nome do domínio do Active Directory.|  
|Configuração de resolvedor do cliente|Configure computadores cliente para apontar para qualquer servidor DNS na rede.|  
  
> [!NOTE]  
> Clientes do Active Directory e controladores de domínio dinamicamente podem registrar seus nomes DNS mesmo se eles não estão apontando para o servidor DNS autoritativo para seus nomes.  
  
Um computador pode ter um nome diferente de DNS existente se a organização anteriormente, estaticamente registrado o computador em DNS ou se a organização implantou anteriormente uma solução Dynamic Host Configuration Protocol (DHCP) integrada. Se os computadores cliente já tiverem um nome DNS registrado, quando o domínio ao qual eles ingressam é atualizado para o Windows Server 2008 AD DS, ele terá dois nomes diferentes:  
  
-   O nome DNS existente  
  
-   O novo nome de domínio totalmente qualificado (FQDN)  
  
Os clientes ainda podem ser localizados por qualquer nome. Qualquer existente DNS, DHCP ou solução DHCP do DNS integrada permanece intacta. Os novos nomes principais são criados automaticamente e atualizados por meio de atualização dinâmica. Eles são limpos automaticamente por meio de eliminação.  
  
Se você quiser tirar proveito de autenticação Kerberos ao se conectar a um servidor que executa o Windows 2000, Windows Server 2003 ou Windows Server 2008, você deve garantir que o cliente se conecta ao servidor usando o nome principal.  
  


