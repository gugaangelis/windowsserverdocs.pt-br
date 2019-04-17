---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: "Requisitos de certificado para servidores de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para servidores de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Design de \(AD FS\) quaisquer serviços de Federação do Active Directory, vários certificados devem ser usados para comunicação segura e facilitar a autenticações de usuário entre clientes de Internet e servidores de Federação. Cada servidor de federação deve ter um certificado de comunicação do serviço e um certificado de assinatura de token\ antes de poder participar em comunicações do AD FS. A tabela a seguir descreve os tipos de certificado associados ao servidor de Federação.  
  
|Tipo de certificado|Descrição|  
|--------------------|---------------|  
|Certificado de assinatura de Token\|Um certificado de assinatura de token\ é um X509 certificado. Servidores de federação usam pares de chaves associados public\/privadas para assinar digitalmente todos os tokens de segurança que elas produzem. Isso inclui a assinatura de solicitações de resolução de metadados e artefato de Federação publicado.<br /><br />Você pode ter vários certificados de autenticação de token\ configurados no AD FS gerenciamento snap\-in para possibilitar a sobreposição de certificado quando um certificado está próximo expirar. Por padrão, todos os certificados na lista são publicados, mas apenas o certificado de assinatura de token\ principal é usado pelo AD FS realmente entre tokens. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [certificados de autenticação de Token](Token-Signing-Certificates.md) e [adicionar um certificado de assinatura de Token](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicação do serviço|Os servidores de federação usam um certificado de autenticação de servidor, também conhecido como uma comunicação de serviço para o Windows Communication Foundation \(WCF\) segurança de mensagem. Por padrão, esse é o mesmo certificado que usa um servidor de federação como o certificado Secure Sockets Layer \(SSL\) na Internet Information Services \(IIS\). **Observação:** o AD FS snap\-in Gerenciamento refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação do serviço.<br /><br />Para obter mais informações, consulte [certificados de comunicações de serviço](Service-Communications-Certificates.md) e [definir um certificado de comunicações de serviço](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Como o certificado de comunicação do serviço deve ser confiável pelos computadores cliente, recomendamos que você use um certificado assinado por uma autoridade de certificação confiável \(CA\). Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Secure Sockets Layer \(SSL\) certificado|Servidores de federação usam um certificado SSL para proteger o tráfego de serviços da Web para a comunicação SSL com clientes da Web e com proxies de servidor de Federação.<br /><br />Como o certificado SSL deve ser confiável pelos computadores cliente, recomendamos que você use um certificado assinado por uma autoridade de certificação confiável. Todos os certificados que você selecionar devem ter uma chave privada correspondente.|  
|Certificado de descriptografia Token\|Esse certificado é usado para descriptografar tokens que são recebidos por este servidor de Federação.<br /><br />Você pode ter vários certificados de descriptografia. Isso possibilita que um servidor de Federação do recurso ser capaz de descriptografar tokens são emitidos com um certificado mais antigo depois que um novo certificado é definido como o certificado de descriptografia principal. Todos os certificados podem ser usados para descriptografia, mas apenas o certificado de descriptografia de token\ principal, na verdade, é publicado nos metadados de Federação. Todos os certificados que você selecionar devem ter uma chave privada correspondente.<br /><br />Para obter mais informações, consulte [adicionar um certificado de descriptografia de Token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Você pode solicitar e instalar um certificado SSL ou um certificado de comunicação do serviço, solicitando um certificado de comunicação de serviço por meio do Console de gerenciamento Microsoft \(MMC\) snap\-para IIS. Para obter mais informações sobre o uso de certificados SSL, consulte [IIS 7.0: Configurando Secure Sockets Layer no IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) e [IIS 7.0: Configurando o servidor de certificados no IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> No AD FS, você pode alterar o nível de Secure Hash Algorithm \(SHA\) que é usado para assinaturas digitais para \(more secure\) SHA\-1 ou SHA\-256. AD FSdoes não suporta o uso de certificados com outros métodos de hash, como MD5 \ (o algoritmo de hash padrão que é usado com a tool\ de linha de command\ Makecert.exe). Como prática recomendada de segurança, recomendamos que você use SHA\-256 \ (que é definido pelo default\) para todas as assinaturas. SHA\-1 é recomendado para uso somente em cenários nos quais você deve interoperar com um produto que não oferece suporte a comunicações usando SHA\-256, como um produto Microsoft non\ ou AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinar sua estratégia de autoridade de certificação  
AD FS não exige que os certificados ser emitido por uma autoridade de certificação. No entanto, o certificado SSL \ (o certificado que também é usado por padrão como o certificate\ de comunicações de serviço) deve ser confiável pelos clientes do AD FS. Recomendamos que você não usar certificados assinados self\ para esses tipos de certificado.  
  
> [!IMPORTANT]  
> Uso de self\ assinado, certificados SSL em um ambiente de produção podem permitir que um usuário mal-intencionado em uma organização de parceiro de conta para assumir o controle de servidores de Federação em uma organização de parceiro de recurso. Esse risco de segurança existe porque certificados assinados self\ são certificados raiz. Eles devem ser adicionados ao repositório raiz confiável do outro servidor de Federação \ (por exemplo, o recurso federação servidor \), que pode deixar que o servidor vulnerável a ataques.  
  
Depois que você recebe um certificado de uma autoridade de certificação, certifique-se de que todos os certificados são importados para o repositório de certificados pessoal do computador local. Você pode importar certificados para o armazenamento pessoal com o MMC Certificados snap\-in.  
  
Como alternativa ao uso de certificados snap\-in, você também pode importar o certificado SSL com o Gerenciador do IIS snap\-in no momento em que você atribuir o certificado SSL ao site da Web padrão. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site padrão](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar o software do AD FS no computador que tornarão o servidor de federação, certifique-se de que ambos os certificados são no repositório de certificados pessoal do computador Local e que o certificado SSL é atribuído ao Site da Web padrão. Para saber mais sobre a ordem das tarefas que são necessárias para configurar um servidor de federação, consulte [lista de verificação: configuração de um servidor federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Dependendo de seus requisitos de segurança e orçamento, considere cuidadosamente quais dos seus certificados serão obtidas por um público CA ou uma autoridade de certificação corporativa. A figura a seguir mostra os emissores de CA recomendados para um determinado tipo de certificado. Essa recomendação reflete uma abordagem prática best\ em relação a segurança e custo.  
  
![requisitos de certificação](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de revogação de certificados  
Se qualquer certificado que você usa tiver CRLs, o servidor com o certificado configurado deve ser capaz de entrar em contato com o servidor que distribui as CRLs.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
