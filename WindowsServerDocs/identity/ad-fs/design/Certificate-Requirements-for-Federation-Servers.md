---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisitos de certificado para servidores de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7eeff82ef3311f18c8252c44c96310fcf3c18217
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359176"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para servidores de federação

Em qualquer Serviços de Federação do Active Directory (AD FS) \(AD FS\) design, vários certificados devem ser usados para proteger a comunicação e facilitar as autenticações de usuário entre os clientes da Internet e os servidores de Federação. Cada servidor de Federação deve ter um certificado de comunicação de serviço e um token\-certificado de assinatura antes que possa participar AD FS comunicações. A tabela a seguir descreve os tipos de certificado associados ao servidor de Federação.  
  
|Tipo de certificado|Descrição|  
|--------------------|---------------|  
|Certificado de assinatura de\-de token|Um token\-certificado de assinatura é um certificado X509. Os servidores de Federação usam pares de chaves privadas\/pública associados para assinar digitalmente todos os tokens de segurança que eles produzem. Isso inclui a assinatura de metadados de federação publicados e solicitações de resolução de artefato.<br /><br />Você pode ter vários tokens\-certificados de assinatura configurados no\-snap de gerenciamento de AD FS no para permitir a substituição do certificado quando um certificado estiver perto de expirar. Por padrão, todos os certificados na lista são publicados, mas somente o token primário\-certificado de autenticação é usado pelo AD FS para assinar tokens. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [Token-Signing Certificates](Token-Signing-Certificates.md) e [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicação de serviço|Os servidores de Federação usam um certificado de autenticação de servidor, também conhecido como comunicação de serviço para Windows Communication Foundation \(WCF\) segurança de mensagem. Por padrão, esse é o mesmo certificado que um servidor de Federação usa como o protocolo SSL \(certificado SSL\) no Serviços de Informações da Internet \(IIS\). **Observação:** O\-snap de gerenciamento de AD FS no refere-se aos certificados de autenticação de servidor para servidores de Federação como certificados de comunicação de serviço.<br /><br />Para obter mais informações, consulte [certificados de comunicações de serviço](Service-Communications-Certificates.md) e [definir um certificado de comunicações de serviço](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Como o certificado de comunicação do serviço deve ser confiável por computadores cliente, recomendamos que você use um certificado assinado por uma autoridade de certificação confiável \(\)da AC. Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Protocolo SSL \(SSL\) certificado|Os servidores de federação usam um certificado SSL para proteger o tráfego de serviços Web para comunicação SSL com os clientes Web e com proxies de servidor de federação.<br /><br />Como o certificado SSL deve ser confiável para os computadores cliente, recomendamos que você use um certificado assinado por uma AC confiável. Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Certificado de descriptografia de\-de token|Esse certificado é usado para descriptografar tokens recebidos por esse servidor de Federação.<br /><br />Você pode ter vários certificados de descriptografia. Isso possibilita que um servidor de Federação de recursos seja capaz de descriptografar tokens emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia primário. Todos os certificados podem ser usados para descriptografia, mas somente o token primário\-descriptografar o certificado é realmente publicado nos metadados da Federação. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [Adicionar um certificado de descriptografia de token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Você pode solicitar e instalar um certificado SSL ou um certificado de comunicação de serviço solicitando um certificado de comunicação de serviço por meio do console de gerenciamento Microsoft \(MMC\) snap\-no para IIS. Para obter mais informações gerais sobre como usar certificados SSL, consulte [iis 7,0: Configurando protocolo SSL no iis 7,0](https://go.microsoft.com/fwlink/?LinkID=108544) e [IIS 7,0: Configurando certificados de servidor no IIS 7,0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> No AD FS você pode alterar o algoritmo de hash seguro \(nível de\) do SHA que é usado para assinaturas digitais para o SHA\-1 ou SHA\-256 \(mais seguro\). O AD FSdoes não dá suporte ao uso de certificados com outros métodos de hash, como o MD5 \(o algoritmo de hash padrão usado com a ferramenta de linha de\-comando MakeCert. exe\). Como prática recomendada de segurança, recomendamos que você use o SHA\-256 \(que é definido por padrão\) para todas as assinaturas. O SHA\-1 é recomendado para uso somente em cenários nos quais você deve interoperar com um produto que não oferece suporte a comunicações usando o SHA\-256, como um produto não\-Microsoft ou um AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinando sua estratégia de AC  
AD FS não exige que os certificados sejam emitidos por uma autoridade de certificação. No entanto, o certificado SSL \(o certificado que também é usado por padrão, pois o certificado de comunicações do serviço\) deve ser confiável para os clientes do AD FS. Recomendamos que você não use auto\-certificados assinados para esses tipos de certificado.  
  
> [!IMPORTANT]  
> O uso de auto\-assinados, certificados SSL em um ambiente de produção pode permitir que um usuário mal-intencionado em uma organização de parceiro de conta assuma o controle dos servidores de Federação em uma organização de parceiro de recurso. Esse risco de segurança existe porque auto\-certificados assinados são certificados raiz. Eles devem ser adicionados ao armazenamento de raiz confiável de outro servidor de Federação \(por exemplo, o\)do servidor de Federação de recursos, o que pode deixar esse servidor vulnerável a ataques.  
  
Depois de receber um certificado de uma AC, certifique-se de que todos os certificados sejam importados para o repositório de certificados pessoais do computador local. Você pode importar certificados para o repositório pessoal com o snap\-do MMC de certificados no.  
  
Como alternativa ao uso do snap-in de certificados\-no, você também pode importar o certificado SSL com o snap\-do Gerenciador do IIS no no momento em que você atribui o certificado SSL ao site da Web padrão. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site da Web padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar o AD FS software no computador que se tornará o servidor de Federação, verifique se os dois certificados estão no repositório de certificados pessoal do computador local e se o certificado SSL está atribuído ao site padrão. Para obter mais informações sobre a ordem das tarefas necessárias para configurar um servidor de Federação, consulte lista de [verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Dependendo de suas necessidades de segurança e orçamento, considere cuidadosamente quais dos certificados serão obtidos por uma AC pública ou uma AC corporativa. A figura a seguir mostra os emissores de AC recomendados para um determinado tipo de certificado. Essa recomendação reflete uma abordagem de prática melhor\-em relação à segurança e ao custo.  
  
![requisitos de certificado](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de certificados revogados  
Se qualquer certificado que você usa tiver CRLs, o servidor com o certificado configurado deve ser capaz de contatar o servidor que distribui CRLs.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
