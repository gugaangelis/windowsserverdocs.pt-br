---
title: Planejamento da implantação de certificado do servidor
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2da57ab750cc556b521329f4096fb088e212a903
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318212"
---
# <a name="server-certificate-deployment-planning"></a>Planejamento da implantação de certificado do servidor

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Antes de implantar certificados de servidor, você deve planejar os seguintes itens:  
  
-   [Planejar a configuração básica do servidor](#bkmk_basic)  
  
-   [Planejar o acesso ao domínio](#bkmk_domain)  
  
-   [Planejar o local e o nome do diretório virtual no servidor Web](#bkmk_virtual)  
  
-   [Planejar um registro de alias DNS (CNAME) para seu servidor Web](#bkmk_cname)  
  
-   [Configuração de plano do CAPolicy. inf](#bkmk_capolicy)  
  
-   [Planejar a configuração das extensões CDP e AIA no CA1](#bkmk_cdp)  
  
-   [Planejar a operação de cópia entre a AC e o servidor Web](#bkmk_copy)  
  
-   [Planejar a configuração do modelo de certificado do servidor na autoridade de certificação](#bkmk_template)  
  
## <a name="plan-basic-server-configuration"></a><a name="bkmk_basic"></a>Planejar a configuração básica do servidor  
Depois de instalar o Windows Server 2016 nos computadores que você pretende usar como sua autoridade de certificação e servidor Web, você deve renomear o computador e atribuir e configurar um endereço IP estático para o computador local.  
  
Para obter mais informações, consulte o guia de [rede](../../../core-network-guide/Core-Network-Guide.md)do Windows Server 2016 Core.  
  
## <a name="plan-domain-access"></a><a name="bkmk_domain"></a>Planejar o acesso ao domínio  
Para fazer logon no domínio, o computador deve ser um computador membro do domínio e a conta de usuário deve ser criada no AD DS antes da tentativa de logon. Além disso, a maioria dos procedimentos neste guia exige que a conta de usuário seja membro dos grupos Administradores de empresa ou administradores de domínio em Active Directory usuários e computadores, portanto, você deve fazer logon na AC com uma conta que tenha a associação de grupo apropriada.  
  
Para obter mais informações, consulte o guia de [rede](../../../core-network-guide/Core-Network-Guide.md)do Windows Server 2016 Core.  
  
## <a name="plan-the-location-and-name-of-the-virtual-directory-on-your-web-server"></a><a name="bkmk_virtual"></a>Planejar o local e o nome do diretório virtual no servidor Web  
Para fornecer acesso à CRL e ao certificado de autoridade de certificação a outros computadores, você deve armazenar esses itens em um diretório virtual em seu servidor Web. Neste guia, o diretório virtual está localizado no servidor Web WEB1. Essa pasta está na unidade "C:" e é chamada de "PKI". Você pode localizar seu diretório virtual no servidor Web em qualquer local de pasta que seja apropriado para sua implantação.  
  
## <a name="plan-a-dns-alias-cname-record-for-your-web-server"></a><a name="bkmk_cname"></a>Planejar um registro de alias DNS (CNAME) para seu servidor Web  
Os registros de recurso de alias (CNAME) às vezes também são chamados de registros de recursos de nome canônico. Com esses registros, você pode usar mais de um nome para apontar para um único host, facilitando a realização de coisas como hospedar um servidor de protocolo FTP (FTP) e um servidor Web no mesmo computador. Por exemplo, os nomes de servidor conhecidos (FTP, www) são registrados usando registros de recurso de alias (CNAME) que mapeiam para o nome de host DNS (sistema de nomes de domínio), como WEB1, para o computador servidor que hospeda esses serviços.  
  
Este guia fornece instruções para configurar seu servidor Web para hospedar a CRL (lista de certificados revogados) para sua autoridade de certificação (CA). Como você também pode querer usar seu servidor Web para outras finalidades, como para hospedar um FTP ou site, é uma boa ideia criar um registro de recurso de alias no DNS para seu servidor Web. Neste guia, o registro CNAME é denominado "PKI", mas você pode escolher um nome que seja apropriado para sua implantação.  
  
## <a name="plan-configuration-of-capolicyinf"></a><a name="bkmk_capolicy"></a>Configuração de plano do CAPolicy. inf  
Antes de instalar o AD CS, você deve configurar o CAPolicy. inf na AC com informações que estão corretas para sua implantação. Um arquivo CAPolicy. inf contém as seguintes informações:  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=https://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
Você deve planejar os seguintes itens para este arquivo:  
  
-   **URL**. O arquivo CAPolicy. inf de exemplo tem um valor de URL de **https://pki.corp.contoso.com/pki/cps.txt** . Isso ocorre porque o servidor Web neste guia é denominado WEB1 e tem um registro de recurso DNS CNAME de PKI. O servidor Web também é ingressado no domínio corp.contoso.com. Além disso, há um diretório virtual no servidor Web chamado "PKI", no qual a lista de certificados revogados é armazenada. Verifique se o valor que você fornece para a URL em seu arquivo CAPolicy. inf aponta para um diretório virtual no seu servidor Web em seu domínio.  
  
-   **RenewalKeyLength**. O comprimento de chave de renovação padrão para o AD CS no Windows Server 2012 é 2048. O comprimento da chave que você selecionar deve ser o mais longo possível e, ao mesmo tempo, fornecer compatibilidade com os aplicativos que você pretende usar.  
  
-   **RenewalValidityPeriodUnits**. O arquivo CAPolicy. inf de exemplo tem um valor de RenewalValidityPeriodUnits de 5 anos. Isso ocorre porque o ciclo de vida esperado da autoridade de certificação é de cerca de dez anos. O valor de RenewalValidityPeriodUnits deve refletir o período de validade geral da autoridade de certificação ou o número mais alto de anos para o qual você deseja fornecer o registro.  
  
-   **CRLPeriodUnits**. O arquivo CAPolicy. inf de exemplo tem um valor de CRLPeriodUnits de 1. Isso ocorre porque o intervalo de atualização de exemplo para a lista de certificados revogados neste guia é de 1 semana. No valor de intervalo que você especificar com essa configuração, você deve publicar a CRL na autoridade de certificação no diretório virtual do servidor Web em que você armazena a CRL e fornecer acesso a ela para computadores que estão no processo de autenticação.  
  
-   **AlternateSignatureAlgorithm**. Esse arquivo CAPolicy. inf implementa um mecanismo de segurança aprimorado implementando formatos de assinatura alternativos. Você não deve implementar essa configuração se ainda tiver clientes do Windows XP que exigem certificados dessa autoridade de certificação.  
  
Se você não planeja adicionar qualquer CAs subordinada à sua infraestrutura de chave pública posteriormente e, se desejar evitar a adição de qualquer CAs subordinada, poderá adicionar a chave PathLength ao arquivo cafiler. inf com um valor de 0. Para adicionar essa chave, copie e cole o código a seguir em seu arquivo:  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> Não é recomendável alterar qualquer outra configuração no arquivo CAPolicy. inf, a menos que você tenha um motivo específico para fazer isso.  
  
## <a name="plan-configuration-of-the-cdp-and-aia-extensions-on-ca1"></a><a name="bkmk_cdp"></a>Planejar a configuração das extensões CDP e AIA no CA1  
Ao configurar o ponto de distribuição da CRL (lista de certificados revogados) e as configurações de AIA (acesso a informações de autoridade) em CA1, você precisará do nome do seu servidor Web e do seu nome de domínio. Você também precisa do nome do diretório virtual que você cria no servidor Web onde a CRL (lista de certificados revogados) e o certificado de autoridade de certificação são armazenados.  
  
O local de CDP que você deve inserir durante essa etapa de implantação tem o formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Por exemplo, se o seu servidor Web for denominado WEB1 e o registro CNAME do alias DNS para o servidor Web for "PKI", seu domínio for corp.contoso.com e seu diretório virtual for chamado PKI, o local de CDP será:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
O local do AIA que você deve inserir tem o formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Por exemplo, se o seu servidor Web for denominado WEB1 e o registro CNAME do alias DNS para o servidor Web for "PKI", seu domínio for corp.contoso.com e seu diretório virtual for chamado PKI, o local AIA será:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="plan-the-copy-operation-between-the-ca-and-the-web-server"></a><a name="bkmk_copy"></a>Planejar a operação de cópia entre a AC e o servidor Web  
Para publicar a CRL e o certificado de autoridade de certificação da CA para o diretório virtual do servidor Web, você pode executar o comando certutil-crl depois de configurar os locais de CDP e AIA na autoridade de certificação. Verifique se você configurou os caminhos corretos na guia **extensões** de propriedades de CA antes de executar esse comando usando as instruções neste guia. Além disso, para copiar o certificado de autoridade de certificação corporativa para o servidor Web, você já deve ter criado o diretório virtual no servidor Web e configurado a pasta como uma pasta compartilhada.  
  
## <a name="plan-the-configuration-of-the-server-certificate-template-on-the-ca"></a><a name="bkmk_template"></a>Planejar a configuração do modelo de certificado do servidor na autoridade de certificação  
Para implantar certificados de servidor autoinscritos, você deve copiar o modelo de certificado chamado **RAS e servidor IAS**. Por padrão, essa cópia é denominada **cópia do servidor RAS e ias**. Se você quiser renomear essa cópia de modelo, planeje o nome que deseja usar durante essa etapa de implantação.  
  
> [!NOTE]  
> As três últimas seções de implantação deste guia, que permitem configurar o registro automático do certificado do servidor, atualizar Política de Grupo em servidores e verificar se os servidores receberam um certificado de servidor válido da CA-não exigem planejamento adicional tarefas.  
  


