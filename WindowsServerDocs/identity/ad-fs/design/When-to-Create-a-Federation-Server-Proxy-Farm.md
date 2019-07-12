---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Quando criar um Farm de Proxy do Servidor de Federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c33475d7420383448439e2b769562e55127c7b0e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190631"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quando criar um Farm de Proxy do Servidor de Federação

Considere instalar proxies de servidor de Federação adicionais quando você tem um grande serviços de Federação do Active Directory \(do AD FS\) implantação e quiser fornecer tolerância a falhas, carga\-balanceamento e escalabilidade para sua implantação do proxy. O ato de criar federação dois ou mais proxies de servidor na mesma rede de perímetro e configurar cada um deles para proteger o mesmo serviço de Federação do AD FS cria um farm de proxy do servidor de Federação.  
  
Você pode criar um farm de proxy do servidor de Federação ou instalar proxies de servidor de Federação adicionais a um farm existente, usando o Assistente de configuração do AD FS Federation Server Proxy. Para obter mais informações, consulte [Quando criar um Farm de Proxy do Servidor de Federação](When-to-Create-a-Federation-Server-Proxy.md).  
  
Antes que todos os proxies de servidor de federação podem funcionar juntos como um farm, você deve primeiro clustê-los em um endereço IP e um sistema de nomes de domínio \(DNS\) nome de domínio totalmente qualificado \(FQDN\). Você pode agrupar os servidores com a implantação de balanceamento de carga de rede da Microsoft \(NLB\) dentro da rede de perímetro. As tarefas na tabela a seguir exigem que o NLB seja configurado corretamente para os proxies de servidor de federação no farm de cluster.  
  
Para obter mais informações sobre como configurar um FQDN para um cluster usando a tecnologia Microsoft NLB, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configurando proxies do servidor de federação para um farm  
A tabela a seguir descreve as tarefas que devem ser concluídas para que cada proxy do servidor de Federação pode participar de um farm.  
  
|Tarefa|Descrição|  
|--------|---------------|  
|Aponte todos os proxies no farm para o mesmo nome de serviço de Federação do AD FS|Quando você cria a federação, proxies de servidor, você deve digitar o mesmo nome de serviço de Federação em que o Assistente de configuração do AD FS Federation Server Proxy para todos os proxies de servidor de federação que farão parte do farm. O proxy do servidor de federação usa a URL que compõe esse nome de host DNS para determinar qual instância do serviço de Federação do AD FS-contatos.<br /><br />Para obter mais informações, consulte [configurar um computador para a função de Proxy do servidor de Federação](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obter e compartilhar certificados|Você pode obter o certificado de autenticação de um servidor de uma autoridade de certificação pública \(autoridade de certificação\)— por exemplo, VeriSign — e, em seguida, configure o certificado para que todos os proxies de servidor de Federação compartilham a mesma parte de chave privada das mesmo certificado no site da Web padrão para cada proxy do servidor de Federação. Para compartilhar o certificado, você deve instalar o mesmo certificado de autenticação de servidor no site da Web padrão para cada proxy do servidor de Federação. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o Site padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Para obter mais informações, consulte [requisitos de certificado para Proxies de servidor de Federação](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Para obter mais informações sobre como adicionar novos proxies de servidor de federação para criar um farm de proxy do servidor de federação, consulte [lista de verificação: Como configurar um Proxy do servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
