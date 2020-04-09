---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Quando criar um Farm de Proxy do Servidor de Federação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d4b2b889159dee9f3b93a54a2b1924be286792f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858489"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quando criar um Farm de Proxy do Servidor de Federação

Considere instalar proxies de servidor de Federação adicionais quando você tiver um grande Serviços de Federação do Active Directory (AD FS) \(AD FS\) implantação e desejar fornecer tolerância a falhas, carregar\-balanceamento e escalabilidade para sua implantação de proxy. O ato de criar dois ou mais proxies de servidor de Federação na mesma rede de perímetro e configurar cada um deles para proteger o mesmo AD FS Serviço de Federação cria um farm de proxy de servidor de Federação.  
  
Você pode criar um farm de proxy de servidor de Federação ou instalar proxies de servidor de Federação adicionais em um farm existente usando o assistente de configuração de proxy de servidor de Federação AD FS. Para obter mais informações, consulte [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md).  
  
Antes que todos os proxies do servidor de Federação possam funcionar juntos como um farm, você deve primeiro agrupá-los em um endereço IP e em um sistema de nome de domínio \(DNS\) nome de domínio totalmente qualificado \(FQDN\). Você pode agrupar os servidores implantando o balanceamento de carga de rede da Microsoft \(\) NLB dentro da rede de perímetro. As tarefas na tabela a seguir exigem que o NLB seja configurado adequadamente para clusterizar os proxies do servidor de Federação no farm.  
  
Para obter mais informações sobre como configurar um FQDN para um cluster usando a tecnologia Microsoft NLB, consulte [especificando os parâmetros de cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configurando proxies do servidor de federação para um farm  
A tabela a seguir descreve as tarefas que devem ser concluídas para que cada proxy de servidor de federação possa participar de um farm.  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|--------|---------------|  
|Aponte todos os proxies no farm para o mesmo nome de Serviço de Federação de AD FS|Ao criar os proxies de servidor de Federação, você deve digitar o mesmo nome de Serviço de Federação no assistente de configuração de proxy de servidor de Federação AD FS para todos os proxies de servidor de Federação que participarão do farm. O proxy do servidor de Federação usa a URL que compõe esse nome de host DNS para determinar qual AD FS Serviço de Federação instância ele contata.<p>Para obter mais informações, consulte [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obter e compartilhar certificados|Você pode obter um certificado de autenticação de servidor de uma autoridade de certificação pública \(\)de AC — por exemplo, VeriSign — e, em seguida, configurar o certificado para que todos os proxies de servidor de Federação compartilhem a mesma parte de chave privada do mesmo certificado no site padrão para cada proxy de servidor de Federação. Para compartilhar o certificado, você deve instalar o mesmo certificado de autenticação de servidor no site da Web padrão para cada proxy de servidor de Federação. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site da Web padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<p>Para obter mais informações, consulte [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Para obter mais informações sobre como adicionar novos proxies de servidor de Federação para criar um farm de proxy de servidor de Federação, consulte [lista de verificação: Configurando um proxy de servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
