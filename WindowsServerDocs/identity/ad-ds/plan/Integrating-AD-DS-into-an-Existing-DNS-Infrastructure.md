---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Integrando o AD DS a uma infraestrutura de DNS existente
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62405ea9ee38bb3fa457b7731e26fbffb2594797
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891037"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integrando o AD DS a uma infraestrutura de DNS existente

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização já tiver um serviço existente do servidor do sistema de nome de domínio (DNS), o DNS para o proprietário de serviços de domínio Active Directory (AD DS) deve trabalhar com o proprietário do DNS para sua organização a integrar o AD DS com a infra-estrutura existente. Isso envolve a criação de um servidor DNS e a configuração do cliente DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Criando uma configuração de servidor DNS  
Ao integrar o AD DS com um namespace DNS existente, recomendamos que você faça o seguinte:  
  
-   Instale o serviço servidor DNS em cada controlador de domínio na floresta. Isso fornece tolerância a falhas, se um dos servidores DNS não estiver disponível. Dessa forma, os controladores de domínio não é necessário confiar em outros servidores DNS para resolução de nome. Isso também simplifica o ambiente de gerenciamento porque todos os controladores de domínio têm uma configuração uniforme.  
  
-   Configure o controlador de domínio de raiz de floresta do Active Directory para hospedar a zona DNS de floresta do Active Directory.  
  
-   Configure controladores de domínio para cada domínio regional hospedar as zonas DNS que correspondem aos seus domínios do Active Directory.  
  
-   Configurar a zona que contém os registros de localizador de toda a floresta do Active Directory (ou seja, msdcs. *ForestName* zona) para replicar para todos os servidores DNS da floresta usando a partição de diretório de aplicativo de DNS de floresta.  
  
    > [!NOTE]  
    > Quando o serviço servidor DNS é instalado com o Active Directory domínio serviços de Assistente de instalação (recomendamos essa opção), todas as tarefas anteriores são executadas automaticamente. Para obter mais informações, consulte [implantação de um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > AD DS usa registros de localizador de toda a floresta para permitir que os parceiros de replicação para localizar uns aos outros e para permitir que os clientes localizar servidores de catálogo global. AD DS armazena os registros de localizador de toda a floresta na zona MSDCS. *forestname* zona. Como as informações na zona devem ser amplamente disponíveis, nesta zona é replicada para todos os servidores DNS na floresta por meio da partição de diretório de aplicativo de DNS de floresta.  
  
A estrutura DNS existente permanece intacta. Você não precisa mover quaisquer servidores ou zonas. Basta criar uma delegação para as zonas DNS integradas ao Active Directory de sua hierarquia DNS existente.  
  
## <a name="creating-the-dns-client-configuration"></a>Criando a configuração do cliente DNS  
Para configurar o DNS em computadores cliente, o DNS para o proprietário do AD DS deve especificar o esquema e como os clientes irão localizar os servidores DNS de nomeação do computador. A tabela a seguir lista as nossas configurações recomendadas para esses elementos de design.  
  
|Elemento de design|Configuração|  
|------------------|-----------------|  
|Nomenclatura de computador|Use o padrão de nomenclatura. Quando um Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou o computador baseado no Windows Vista ingressa em um domínio, o computador em si atribui um nome de domínio totalmente qualificado (FQDN) que inclui o nome do host do computador e o nome do ativo Domínio de diretório.|  
|Configuração do resolvedor de cliente|Configure computadores cliente para apontar para qualquer servidor DNS na rede.|  
  
> [!NOTE]  
> Clientes do Active Directory e controladores de domínio dinamicamente podem registrar seus nomes DNS, mesmo se eles não estão apontando para o servidor DNS autoritativo para seus nomes.  
  
Um computador pode ter um nome DNS existente diferente se a organização, estaticamente registrado anteriormente no computador no DNS ou se a organização implantou uma solução integrada de protocolo de configuração de Host dinâmico (DHCP). Se seus computadores cliente já tiverem um nome DNS registrado, quando o domínio ao qual elas são unidas é atualizado para o AD DS do Windows Server 2008, eles terá dois nomes diferentes:  
  
-   O nome DNS existente  
  
-   O novo nome de domínio totalmente qualificado (FQDN)  
  
Os clientes ainda podem ser localizados por qualquer nome. Qualquer existente de DNS, DHCP ou solução integrada de DHCP/DNS permanece intacta. Os novos nomes primários são criados automaticamente e atualizados por meio de atualização dinâmica. Elas são limpas automaticamente por meio de eliminação.  
  
Se você quiser aproveitar a autenticação Kerberos ao se conectar a um servidor que executa o Windows 2000, Windows Server 2003 ou Windows Server 2008, certifique-se de que o cliente se conecta ao servidor usando o nome primário.  
  


