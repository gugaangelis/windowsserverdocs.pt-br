---
title: Planejamento da implantação de certificado do servidor
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0d14ed33c9bf389433f59774c04ff15e37256cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839527"
---
# <a name="server-certificate-deployment-planning"></a>Planejamento da implantação de certificado do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Antes de implantar certificados de servidor, você deve planejar os seguintes itens:  
  
-   [Planejar a configuração básica do servidor](#bkmk_basic)  
  
-   [Planejar o acesso de domínio](#bkmk_domain)  
  
-   [Planejar o local e o nome do diretório virtual em seu servidor Web](#bkmk_virtual)  
  
-   [Planejar um registro de alias (CNAME) de DNS para seu servidor Web](#bkmk_cname)  
  
-   [Planejar configuração de CAPolicy. inf](#bkmk_capolicy)  
  
-   [Planejar a configuração das extensões CPD e AIA CA1](#bkmk_cdp)  
  
-   [Planejar a operação de cópia entre a autoridade de certificação e o servidor Web](#bkmk_copy)  
  
-   [Planeje a configuração do modelo de certificado de servidor da autoridade de certificação](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Planejar a configuração básica do servidor  
Depois de instalar o Windows Server 2016 nos computadores que você planeja usar como sua autoridade de certificação e o servidor Web, você deve renomear o computador e atribuir e configurar um endereço IP estático para o computador local.  
  
Para obter mais informações, consulte o Windows Server 2016 [guia da rede principal](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_domain"></a>Planejar o acesso de domínio  
Para fazer logon no domínio, o computador deve ser um computador membro do domínio e a conta de usuário deve ser criada no AD DS antes da tentativa de logon. Além disso, a maioria dos procedimentos deste guia exigem que a conta de usuário é um membro dos grupos Administradores corporativos ou Admins. do domínio no Active Directory Users and Computers, portanto, você deve fazer logon para a autoridade de certificação com uma conta que tenha a associação de grupo apropriado.  
  
Para obter mais informações, consulte o Windows Server 2016 [guia da rede principal](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_virtual"></a>Planejar o local e o nome do diretório virtual em seu servidor Web  
Para fornecer acesso a CRL e o certificado de autoridade de certificação em outros computadores, você deve armazenar esses itens em um diretório virtual em seu servidor Web. Neste guia, o diretório virtual está localizado no servidor Web WEB1. Esta pasta está na unidade "C:" e é chamada "pki". Você pode localizar seu diretório virtual em seu servidor Web em qualquer local da pasta que é apropriado para sua implantação.  
  
## <a name="bkmk_cname"></a>Planejar um registro de alias (CNAME) de DNS para seu servidor Web  
Registros de recurso de alias (CNAME), às vezes, também são chamados de registros de recursos de nome canônico. Com esses registros, você pode usar mais de um nome para apontar para um único host, tornando mais fácil fazer coisas como host de um servidor de protocolo FTP (File Transfer) e um servidor Web no mesmo computador. Por exemplo, os nomes de servidores conhecidos (ftp, www) são registrados usando registros de recursos de alias (CNAME) que mapeiam para o sistema de nome de domínio (DNS) nome de host, como o WEB1, para o computador servidor que hospeda esses serviços.  
  
Este guia fornece instruções para configurar seu servidor Web para hospedar a lista de certificados revogados (CRL) para sua autoridade de certificação (CA). Você também poderá usar o seu servidor Web para outras finalidades, como para hospedar um FTP ou site da Web, é uma boa ideia criar um registro de recurso de alias no DNS para seu servidor Web. Neste guia, o registro CNAME é denominado "pki", mas você pode escolher um nome que seja apropriado para sua implantação.  
  
## <a name="bkmk_capolicy"></a>Planejar configuração de CAPolicy. inf  
Antes de instalar o AD CS, você deve configurar o CAPolicy. inf na autoridade de certificação com informações que estão corretas para sua implantação. Um arquivo CAPolicy. inf contém as seguintes informações:  
  
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
Você deve planejar os itens a seguir para este arquivo:  
  
-   **URL**. O arquivo CAPolicy. inf de exemplo tem um valor de URL **https://pki.corp.contoso.com/pki/cps.txt**. Isso ocorre porque o servidor Web neste guia chamado WEB1 e tem um registro de recurso DNS CNAME de pki. O servidor Web também é unido ao domínio corp.contoso.com. Além disso, há um diretório virtual no servidor Web denominado "pki" em que a lista de revogação de certificado é armazenada. Certifique-se de que o valor que você fornecer para a URL em seu arquivo de CAPolicy. inf aponta para um diretório virtual em seu servidor Web em seu domínio.  
  
-   **RenewalKeyLength**. O comprimento de chave de renovação de padrão para o AD CS no Windows Server 2012 é 2048. O comprimento da chave que você selecionar deve ser possível enquanto ainda fornecem compatibilidade com os aplicativos que você pretende usar.  
  
-   **RenewalValidityPeriodUnits**. O arquivo CAPolicy. inf de exemplo tem um valor de RenewalValidityPeriodUnits de 5 anos. Isso ocorre porque o tempo de vida esperado da autoridade de certificação é de cerca de dez anos. O valor de RenewalValidityPeriodUnits deve refletir o período de validade geral da autoridade de certificação ou o maior número de anos para o qual você deseja fornecer o registro.  
  
-   **CRLPeriodUnits**. O arquivo CAPolicy. inf de exemplo tem um valor de CRLPeriodUnits de 1. Isso ocorre porque o intervalo de atualização de exemplo para a lista de certificados revogados neste guia é de 1 semana. O valor de intervalo que você especificar com essa configuração, você deve publicar a CRL na autoridade de certificação para o diretório virtual do servidor Web onde você armazena a CRL e fornecer acesso a ele para computadores que estão no processo de autenticação.  
  
-   **AlternateSignatureAlgorithm**. Esse arquivo CAPolicy. inf implementa um mecanismo de segurança aprimorada com a implementação de formatos de assinatura alternativo. Você não deve implementar essa configuração se você ainda tiver clientes Windows XP que exigem certificados a partir da autoridade de certificação.  
  
Se você não planeja adicionar todas as CAs subordinadas à sua infraestrutura de chave pública em um momento posterior, e se você quiser impedir a adição de todas as CAs subordinadas, você pode adicionar a chave de PathLength em seu arquivo CAPolicy. inf com um valor de 0. Para adicionar essa chave, copie e cole o código a seguir em seu arquivo:  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> Não é recomendável que você altere outras configurações no arquivo CAPolicy. inf, a menos que você tenha um motivo específico para fazer isso.  
  
## <a name="bkmk_cdp"></a>Planejar a configuração das extensões CPD e AIA CA1  
Quando você configura o ponto de distribuição da lista de revogação de certificados (CRL) (CDP) e as configurações de acesso de informações da autoridade (AIA) em CA1, você precisa do nome do seu servidor Web e do nome de domínio. Você também precisa do nome do diretório virtual que você cria no seu servidor Web onde a lista de certificados revogados (CRL) e o certificado de autoridade de certificação estão armazenados.  
  
O local de CPD que você deve inserir durante essa etapa de implantação tem o formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Por exemplo, se seu servidor Web for denominado WEB1 e seu DNS alias registro CNAME para o servidor Web é "pki", seu domínio for corp.contoso.com e seu diretório virtual é denominado pki, o local de CPD é:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
O local de AIA que você precisa inserir tem o formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Por exemplo, se seu servidor Web for denominado WEB1 e seu DNS alias registro CNAME para o servidor Web é "pki", seu domínio for corp.contoso.com e seu diretório virtual é denominado pki, o local de AIA é:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Planejar a operação de cópia entre a autoridade de certificação e o servidor Web  
Para publicar o certificado CRL e a autoridade de certificação da autoridade de certificação para o diretório virtual do servidor Web, você pode executar o comando do certutil - crl depois de configurar os locais de CPD e AIA na autoridade de certificação. Certifique-se de que você configure os caminhos corretos nas propriedades de autoridade de certificação **extensões** guia antes de executar esse comando usando as instruções neste guia. Além disso, para copiar o certificado de autoridade de certificação corporativa para o servidor Web, você deve ter já criou o diretório virtual no servidor Web e configurou a pasta como uma pasta compartilhada.  
  
## <a name="bkmk_template"></a>Planeje a configuração do modelo de certificado de servidor da autoridade de certificação  
Para implantar os certificados de servidor, você deve copiar o modelo de certificado denominado **servidores RAS e IAS**. Por padrão, essa cópia é denominada **cópia dos servidores RAS e IAS**. Se você quiser renomear esta cópia do modelo, o nome que você deseja usar durante essa etapa de implantação do plano.  
  
> [!NOTE]  
> As seções de implantação de três últimos neste guia, que permitem que você configurar o registro automático de certificado do servidor, atualizar a política de grupo em servidores e verifique se os servidores receberam um certificado de servidor válido da AC - não exigem informações adicionais sobre planejamento etapas.  
  


