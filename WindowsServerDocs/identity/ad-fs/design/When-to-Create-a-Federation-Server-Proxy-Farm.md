---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: "Quando criar um Farm de Proxy do servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8935760cad272d5b82edb675cda85caf0456565f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quando criar um Farm de Proxy do servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere Instalando proxies de servidor de Federação adicionais quando você tiver uma grande implantação \(AD FS\) serviços de Federação do Active Directory e você deseja fornecer tolerância, balanceamento load\ e escalabilidade para a implantação do proxy. O ato de criação proxies de servidor de Federação dois ou mais na mesma rede perímetro e configuração de cada um para proteger o mesmo serviço de Federação do AD FS cria um farm de proxy do servidor de Federação.  
  
Você pode criar um farm de proxy do servidor de Federação ou instalar proxies de servidor de Federação adicional a um farm existente usando o Assistente de configuração do AD FS federação servidor Proxy. Para obter mais informações, consulte [quando criar um Proxy de servidor de Federação](When-to-Create-a-Federation-Server-Proxy.md).  
  
Antes de todos os proxies de servidor de federação podem funcionar juntos como um farm, você deve primeiro clustê-los em um endereço IP e um domínio totalmente qualificado Domain Name System \(DNS\) nomear \(FQDN\). Você pode agrupar os servidores Implantando balanceamento de carga de rede Microsoft \(NLB\) dentro da rede do perímetro. As tarefas na tabela a seguir requerem NLB ser configurado adequadamente para os proxies de servidor de federação no farm do cluster.  
  
Para obter mais informações sobre como configurar um FQDN para um cluster usando a tecnologia Microsoft NLB, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configuração proxies de servidor de federação para um farm  
A tabela a seguir descreve as tarefas que devem ser concluídas para que cada proxy de servidor de Federação pode participar de um farm.  
  
|Tarefa|Descrição|  
|--------|---------------|  
|Aponte todos os proxies no farm para o mesmo nome de serviço de Federação do AD FS|Quando você cria a federação proxies de servidor, especifique o mesmo nome de serviço de Federação o Assistente de configuração do AD FS federação servidor Proxy para todos os proxies de servidor de Federação participarão sítio. O proxy do servidor de federação usa a URL que torna esse nome de host DNS para determinar qual instância do serviço de Federação do AD FS contatos.<br /><br />Para obter mais informações, consulte [configurar um computador para a função de Proxy do servidor de Federação](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obter e compartilhar certificados|Você pode obter o certificado de autenticação de um servidor de uma autoridade de certificação pública \ (CA\) — por exemplo, VeriSign — e, em seguida, configure o certificado para que todos os proxies de servidor de Federação compartilham o mesmo parte da chave privada do mesmo certificado no site do padrão para cada proxy de servidor de Federação. Para compartilhar o certificado, você deve instalar o mesmo certificado de autenticação de servidor no site do padrão para cada proxy de servidor de Federação. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Para obter mais informações, consulte [requisitos de certificado para Proxies de servidor de Federação](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Para obter mais informações sobre como adicionar novas proxies de servidor de federação para criar um farm de proxy do servidor de federação, consulte [lista de verificação: configuração de backup de Proxy de servidor de Federação um](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
