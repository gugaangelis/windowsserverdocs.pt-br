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

Em qualquer Serviços de Federação do Active Directory (AD FS) projeto \(AD FS @ no__t-1, vários certificados devem ser usados para proteger a comunicação e facilitar as autenticações de usuário entre clientes da Internet e servidores de Federação. Cada servidor de Federação deve ter um certificado de comunicação de serviço e um certificado @ no__t-0signing de token antes de poder participar de AD FS comunicações. A tabela a seguir descreve os tipos de certificado associados ao servidor de Federação.  
  
|Tipo de certificado|Descrição|  
|--------------------|---------------|  
|Token @ no__t-0signing certificado|Um certificado de token @ no__t-0signing é um certificado X509. Os servidores de Federação usam pares de chaves públicas @ no__t-0private associados para assinar digitalmente todos os tokens de segurança que eles produzem. Isso inclui a assinatura de metadados de federação publicados e solicitações de resolução de artefato.<br /><br />Você pode ter vários certificados @ no__t-0signing de token configurados no snap-in AD FS Management @ no__t-1in para permitir a substituição do certificado quando um certificado está perto de expirar. Por padrão, todos os certificados na lista são publicados, mas somente o certificado @ no__t-0signing do token primário é usado pelo AD FS para assinar tokens de fato. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [Token-Signing Certificates](Token-Signing-Certificates.md) e [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicação de serviço|Os servidores de Federação usam um certificado de autenticação de servidor, também conhecido como comunicação de serviço para Windows Communication Foundation \(WCF @ no__t-1 Message Security. Por padrão, esse é o mesmo certificado que um servidor de Federação usa como o certificado protocolo SSL \(SSL @ no__t-1 no Serviços de Informações da Internet \(IIS @ no__t-3. **Observação:** O snap do AD FS Management @ no__t-0in refere-se aos certificados de autenticação de servidor para servidores de Federação como certificados de comunicação de serviço.<br /><br />Para obter mais informações, consulte [certificados de comunicações de serviço](Service-Communications-Certificates.md) e [definir um certificado de comunicações de serviço](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Como o certificado de comunicação do serviço deve ser confiável por computadores cliente, recomendamos que você use um certificado assinado por uma autoridade de certificação confiável \(CA @ no__t-1. Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Protocolo SSL \(SSL @ no__t-1 certificado|Os servidores de federação usam um certificado SSL para proteger o tráfego de serviços Web para comunicação SSL com os clientes Web e com proxies de servidor de federação.<br /><br />Como o certificado SSL deve ser confiável para os computadores cliente, recomendamos que você use um certificado assinado por uma AC confiável. Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Token @ no__t-0decryption certificado|Esse certificado é usado para descriptografar tokens recebidos por esse servidor de Federação.<br /><br />Você pode ter vários certificados de descriptografia. Isso possibilita que um servidor de Federação de recursos seja capaz de descriptografar tokens emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia primário. Todos os certificados podem ser usados para descriptografia, mas somente o certificado @ no__t-0decrypting do token primário é realmente publicado nos metadados da Federação. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [Adicionar um certificado de descriptografia de token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Você pode solicitar e instalar um certificado SSL ou um certificado de comunicação de serviço solicitando um certificado de comunicação de serviço por meio do console de gerenciamento Microsoft \(MMC @ no__t-1 snap @ no__t-2in para IIS. Para obter mais informações gerais sobre como usar certificados SSL, consulte [IIS 7,0: Configurando protocolo SSL no IIS 7.0 @ no__t-0 e [IIS 7,0: Configurando certificados de servidor no IIS 7.0 @ no__t-0.  
  
> [!NOTE]  
> No AD FS você pode alterar o nível de Sha \(\) do algoritmo de hash seguro que é usado para assinaturas digitais\-para o Sha\-1 \(ou SHA\)256 mais seguro. O AD FSdoes não dá suporte ao uso de certificados com outros métodos de hash, como o algoritmo de hash padrão MD5 \(the que é usado com o comando MakeCert. exe @ no__t-1line Tool @ no__t-2. Como prática recomendada de segurança, recomendamos que você use o\-Sha \(256, que é definido\) por padrão para todas as assinaturas. SHA @ no__t-01 é recomendado para uso somente em cenários nos quais você deve interoperar com um produto que não oferece suporte a comunicações usando SHA @ no__t-1256, como um produto que não seja @ no__t-2Microsoft ou AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinando sua estratégia de AC  
AD FS não exige que os certificados sejam emitidos por uma autoridade de certificação. No entanto, o certificado SSL \(the que também é usado por padrão, pois o certificado de comunicações do serviço @ no__t-1 deve ser confiável para os clientes do AD FS. Recomendamos que você não use certificados Self @ no__t-0signed para esses tipos de certificado.  
  
> [!IMPORTANT]  
> O uso de auto@ no__t-0signed, certificados SSL em um ambiente de produção, pode permitir que um usuário mal-intencionado em uma organização parceira de conta assuma o controle de servidores de Federação em uma organização de parceiro de recurso. Esse risco de segurança existe porque os certificados Self @ no__t-0signed são certificados raiz. Eles devem ser adicionados ao repositório de raiz confiável de outro servidor de Federação \(for exemplo, o servidor de Federação de recurso @ no__t-1, que pode deixar esse servidor vulnerável a ataques.  
  
Depois de receber um certificado de uma AC, certifique-se de que todos os certificados sejam importados para o repositório de certificados pessoais do computador local. Você pode importar certificados para o repositório pessoal com o snap\--in do MMC de certificados.  
  
Como alternativa ao uso do snap-in de certificados @ no__t-0in, também é possível importar o certificado SSL com o snap @ no__t-1in do Gerenciador do IIS no momento em que você atribui o certificado SSL ao site da Web padrão. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site da Web padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar o AD FS software no computador que se tornará o servidor de Federação, verifique se os dois certificados estão no repositório de certificados pessoal do computador local e se o certificado SSL está atribuído ao site padrão. Para obter mais informações sobre a ordem das tarefas necessárias para configurar um servidor de Federação, consulte [Checklist: Configurando um servidor de Federação @ no__t-0.  
  
Dependendo de suas necessidades de segurança e orçamento, considere cuidadosamente quais dos certificados serão obtidos por uma AC pública ou uma AC corporativa. A figura a seguir mostra os emissores de AC recomendados para um determinado tipo de certificado. Essa recomendação reflete uma abordagem melhor do @ no__t-0practice em relação à segurança e ao custo.  
  
![requisitos de certificado](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de certificados revogados  
Se qualquer certificado que você usa tiver CRLs, o servidor com o certificado configurado deve ser capaz de contatar o servidor que distribui CRLs.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
