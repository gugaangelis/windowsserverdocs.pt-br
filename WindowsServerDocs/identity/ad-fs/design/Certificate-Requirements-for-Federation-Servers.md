---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisitos de certificado para servidores de federação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: fcf78fa6242c10d469320ce3803da94134576c8b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954362"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para servidores de federação

Em qualquer Serviços de Federação do Active Directory (AD FS) \( design de AD FS \) , vários certificados devem ser usados para proteger a comunicação e facilitar as autenticações de usuário entre os clientes da Internet e os servidores de Federação. Cada servidor de Federação deve ter um certificado de comunicação de serviço e um certificado de autenticação de token \- antes de poder participar de AD FS comunicações. A tabela a seguir descreve os tipos de certificado associados ao servidor de Federação.

|Tipo de certificado|Descrição|
|--------------------|---------------|
|Certificado de autenticação de tokens \-|Um certificado de autenticação de token \- é um certificado X509. Os servidores de Federação usam \/ pares de chave privada pública associados para assinar digitalmente todos os tokens de segurança que eles produzem. Isso inclui a assinatura de metadados de federação publicados e solicitações de resolução de artefato.<p>Você pode ter vários certificados de autenticação de tokens \- configurados no snap-in de gerenciamento de AD FS \- para permitir a substituição de certificado quando um certificado está perto de expirar. Por padrão, todos os certificados na lista são publicados, mas somente o certificado de autenticação de token primário \- é usado pelo AD FS para assinar tokens. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<p>Para mais informações, consulte [Certificados de autenticação de tokens](Token-Signing-Certificates.md) e [Adicionar um certificado de autenticação de token](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|
|Certificado de comunicação de serviço|Os servidores de Federação usam um certificado de autenticação de servidor, também conhecido como comunicação de serviço para Windows Communication Foundation \( segurança de mensagem do WCF \) . Por padrão, esse é o mesmo certificado que um servidor de Federação usa como o \( certificado protocolo SSL SSL \) no serviços de informações da Internet \( IIS \) . **Observação:** O snap in de gerenciamento de AD FS \- no refere-se aos certificados de autenticação de servidor para servidores de Federação como certificados de comunicação de serviço.<p>Para obter mais informações, consulte [certificados de comunicações de serviço](Service-Communications-Certificates.md) e [definir um certificado de comunicações de serviço](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<p>Como o certificado de comunicação do serviço deve ser confiável por computadores cliente, recomendamos que você use um certificado assinado por uma autoridade de certificação \( confiável \) . Todos os certificados que você selecionar devem ter uma chave privada correspondente.|
|Protocolo SSL \( \) certificado SSL|Os servidores de federação usam um certificado SSL para proteger o tráfego de serviços Web para comunicação SSL com os clientes Web e com proxies de servidor de federação.<p>Como o certificado SSL deve ser confiável para os computadores cliente, recomendamos que você use um certificado assinado por uma AC confiável. Todos os certificados que você selecionar devem ter uma chave privada correspondente.|
|\-Certificado de descriptografia de token|Esse certificado é usado para descriptografar tokens recebidos por esse servidor de Federação.<p>Você pode ter vários certificados de descriptografia. Isso possibilita que um servidor de Federação de recursos seja capaz de descriptografar tokens emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia primário. Todos os certificados podem ser usados para descriptografia, mas apenas o certificado de descriptografia de token primário \- é realmente publicado em metadados de Federação. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<p>Para obter mais informações, consulte [Adicionar um certificado de descriptografia de token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|

Você pode solicitar e instalar um certificado SSL ou um certificado de comunicação de serviço solicitando um certificado de comunicação de serviço por meio do snap-in MMC do console de gerenciamento Microsoft \( \) \- para IIS. Para obter mais informações gerais sobre como usar certificados SSL, consulte [iis 7,0: Configurando protocolo SSL no iis 7,0](https://go.microsoft.com/fwlink/?LinkID=108544) e [IIS 7,0: Configurando certificados de servidor no IIS 7,0](https://go.microsoft.com/fwlink/?LinkID=108545) .

> [!NOTE]
> No AD FS você pode alterar o nível de Sha do algoritmo de hash seguro \( \) que é usado para assinaturas digitais para o Sha \- 1 ou SHA \- 256 \( mais seguro \) . O AD FSdoes não dá suporte ao uso de certificados com outros métodos de hash, como MD5 \( o algoritmo de hash padrão usado com a ferramenta de linha de comando Makecert.exe \- \) . Como prática recomendada de segurança, recomendamos que você use \- o SHA 256 \( , que é definido por padrão \) para todas as assinaturas. O SHA \- 1 é recomendado para uso somente em cenários nos quais você deve interoperar com um produto que não ofereça suporte a comunicações usando o SHA \- 256, como um produto que não seja da \- Microsoft nem o AD FS 1. *x*.

## <a name="determining-your-ca-strategy"></a>Determinando sua estratégia de AC
AD FS não exige que os certificados sejam emitidos por uma autoridade de certificação. No entanto, o certificado SSL \( que também é usado por padrão como o certificado de comunicações do serviço \) deve ser confiável para os clientes do AD FS. Recomendamos que você não use \- certificados autoassinados para esses tipos de certificado.

> [!IMPORTANT]
> O uso de \- autoassinado, certificados SSL em um ambiente de produção pode permitir que um usuário mal-intencionado em uma organização parceira de conta assuma o controle dos servidores de Federação em uma organização de parceiro de recurso. Esse risco de segurança existe porque os \- certificados autoassinados são certificados raiz. Eles devem ser adicionados ao armazenamento de raiz confiável de outro servidor \( de Federação, por exemplo, o servidor de Federação de recursos \) , o que pode deixar esse servidor vulnerável a ataques.

Depois de receber um certificado de uma AC, certifique-se de que todos os certificados sejam importados para o repositório de certificados pessoais do computador local. Você pode importar certificados para o repositório pessoal com o snap-in do MMC de certificados \- .

Como alternativa ao uso do snap-in de certificados \- , você também pode importar o certificado SSL com o snap-in do Gerenciador do IIS \- no momento em que você atribui o certificado SSL ao site da Web padrão. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site da Web padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).

> [!NOTE]
> Antes de instalar o AD FS software no computador que se tornará o servidor de Federação, verifique se os dois certificados estão no repositório de certificados pessoal do computador local e se o certificado SSL está atribuído ao site padrão. Para obter mais informações sobre a ordem das tarefas necessárias para configurar um servidor de Federação, consulte lista de [verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).

Dependendo de suas necessidades de segurança e orçamento, considere cuidadosamente quais dos certificados serão obtidos por uma AC pública ou uma AC corporativa. A figura a seguir mostra os emissores de AC recomendados para um determinado tipo de certificado. Essa recomendação reflete uma \- abordagem de prática recomendada em relação à segurança e ao custo.

![requisitos de certificado](media/adfs2_fedserver_certstory_1.png)

## <a name="certificate-revocation-lists"></a>Listas de certificados revogados
Se qualquer certificado que você usa tiver CRLs, o servidor com o certificado configurado deve ser capaz de contatar o servidor que distribui CRLs.

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
