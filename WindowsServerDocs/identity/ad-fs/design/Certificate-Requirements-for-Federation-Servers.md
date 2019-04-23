---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisitos de certificado para servidores de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827097"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para servidores de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em quaisquer serviços de Federação do Active Directory \(do AD FS\) design, vários certificados devem ser usados para proteger a comunicação e facilitar as autenticações de usuário entre servidores de Federação e os clientes da Internet. Cada servidor de federação deve ter um certificado de comunicação de serviço e um token\-o certificado de assinatura antes de poder participar de comunicações do AD FS. A tabela a seguir descreve os tipos de certificados que estão associados com o servidor de Federação.  
  
|Tipo de certificado|Descrição|  
|--------------------|---------------|  
|Token\-certificado de assinatura|Um token\-certificado de autenticação é um X509 certificado. Servidores de federação usam público associado\/pares de chave privadas para assinar digitalmente todos os tokens de segurança que eles produzem. Isso inclui a assinatura de metadados de federação publicados e solicitações de resolução de artefato.<br /><br />Você pode ter vários token\-certificados configurados no snap do gerenciamento do AD FS de autenticação\-para permitir a substituição do certificado quando ele estiver perto de expirar. Por padrão, todos os certificados na lista são publicados, mas apenas o token primário\-certificado de autenticação é usado pelo AD FS para realmente assinar tokens. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [Token-Signing Certificates](Token-Signing-Certificates.md) e [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicação de serviço|Servidores de federação usam um certificado de autenticação de servidor, também conhecido como uma comunicação de serviço para o Windows Communication Foundation \(WCF\) segurança de mensagem. Por padrão, esse é o mesmo certificado que um servidor de federação usa como o Secure Sockets Layer \(SSL\) certificado nos serviços de informações da Internet \(IIS\). **Observação:** O snap de gerenciamento do AD FS\-refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação de serviço.<br /><br />Para obter mais informações, consulte [certificados do serviço de comunicações](Service-Communications-Certificates.md) e [definir um certificado de comunicações de serviço](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Como o certificado de comunicação de serviço deve ser confiável por computadores cliente, é recomendável que você use um certificado assinado por uma autoridade de certificação confiáveis \(autoridade de certificação\). Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Secure Sockets Layer \(SSL\) certificado|Os servidores de federação usam um certificado SSL para proteger o tráfego de serviços Web para comunicação SSL com os clientes Web e com proxies de servidor de federação.<br /><br />Como o certificado SSL deve ser confiável para os computadores cliente, recomendamos que você use um certificado assinado por uma AC confiável. Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Token\-certificado de descriptografia|Esse certificado é usado para descriptografar tokens recebidos por este servidor de Federação.<br /><br />Você pode ter vários certificados de descriptografia. Isso torna possível para um servidor de federação de recurso ser capaz de descriptografar sinais emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia primário. Todos os certificados podem ser usados para descriptografia, mas apenas o token primário\-descriptografar o certificado é realmente publicado nos metadados de Federação. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [adicionar um certificado de descriptografia do Token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Você pode solicitar e instalar um certificado SSL ou o certificado de comunicação de serviço ao solicitar um certificado de comunicação de serviço por meio do Console de gerenciamento Microsoft \(MMC\) encaixar\-para o IIS. Para obter mais informações sobre como usar certificados SSL, consulte [IIS 7.0: Configurando o protocolo SSL no IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) e [IIS 7.0: Configurando certificados de servidor no IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> No AD FS, você pode alterar o algoritmo de Hash seguro \(SHA\) nível que é usado para assinaturas digitais para qualquer um dos SHA\-1 ou SHA\-256 \(mais seguro\). AD FSdoes não suporta o uso de certificados com outros métodos de hash, como MD5 \(o algoritmo de hash padrão que é usado com o comando Makecert.exe\-ferramenta de linha de\). Como prática recomendada de segurança, recomendamos que você use SHA\-256 \(que é definido por padrão\) para todas as assinaturas. SHA\-1 é recomendado para uso apenas em cenários em que você deve interoperar com um produto que não oferece suporte a comunicações usando SHA\-256, como um não\-produto da Microsoft ou do AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinando sua estratégia de AC  
O AD FS não exigem que certificados sejam emitidos por uma autoridade de certificação. No entanto, o certificado SSL \(o certificado que também é usado por padrão como o certificado de comunicações de serviço\) deve ser confiável para os clientes do AD FS. É recomendável que você não use auto\-certificados autoassinados para esses tipos de certificado.  
  
> [!IMPORTANT]  
> Uso de self\-assinado, certificados SSL em um ambiente de produção podem permitir que um usuário mal-intencionado em uma organização do parceiro de conta para assumir o controle dos servidores de Federação em uma organização do parceiro de recurso. Esse risco de segurança existe porque o próprio\-certificados autoassinados são os certificados raiz. Eles devem ser adicionados ao repositório raiz confiável de outro servidor de Federação \(por exemplo, o servidor de Federação do recurso\), que pode deixar o servidor vulnerável a ataques.  
  
Depois de receber um certificado de uma AC, certifique-se de que todos os certificados sejam importados para o repositório de certificados pessoais do computador local. Você pode importar certificados para o repositório pessoal com snap do MMC de certificados\-no.  
  
Como alternativa ao uso de certificados snap\-, você também pode importar o certificado SSL com o Gerenciador do IIS snap\-no momento em que você atribuir o SSL de certificado ao site da Web padrão. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o Site padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar o software AD FS no computador que se tornará o servidor de federação, certifique-se de que ambos os certificados estão no repositório de certificados pessoal do computador Local e que o certificado SSL está atribuído ao Site da Web padrão. Para obter mais informações sobre a ordem das tarefas que são necessárias para configurar um servidor de federação, consulte [lista de verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Dependendo de suas necessidades de segurança e orçamento, considere cuidadosamente quais dos certificados serão obtidos por uma AC pública ou uma AC corporativa. A figura a seguir mostra os emissores de AC recomendados para um determinado tipo de certificado. Essa recomendação reflete uma melhor\-praticar a abordagem em relação à segurança e custo.  
  
![requisitos do certificado](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de revogação de certificado  
Se qualquer certificado que você usa tiver CRLs, o servidor com o certificado configurado deve ser capaz de contatar o servidor que distribui CRLs.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
