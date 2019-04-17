---
title: Planejamento de implantação do certificado de servidor
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eacfa404da91d14a7a7328646c2320be8220000c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-planning"></a>Planejamento de implantação do certificado de servidor

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Antes de implantar certificados de servidor, você deve planejar os seguintes itens:  
  
-   [Planejar a configuração básica de servidor](#bkmk_basic)  
  
-   [Planeje o acesso ao domínio](#bkmk_domain)  
  
-   [Planeje o local e o nome do diretório virtual em seu servidor Web](#bkmk_virtual)  
  
-   [Planeje um registro de alias (CNAME) DNS para o servidor Web](#bkmk_cname)  
  
-   [Configuração de plano de CAPolicy](#bkmk_capolicy)  
  
-   [Planejar a configuração das extensões de CDP e AIA em CA1](#bkmk_cdp)  
  
-   [Planejar a operação de cópia entre a CA e o servidor Web](#bkmk_copy)  
  
-   [Planejar a configuração do modelo de certificado de servidor na autoridade de certificação](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Planejar a configuração básica de servidor  
Depois de instalar o Windows Server 2016 nos computadores que você pretende usar como sua autoridade de certificação e o servidor Web, você deve renomear o computador e atribuir e configurar um endereço IP estático para o computador local.  
  
Para obter mais informações, consulte o Windows Server 2016 [guia da rede principal](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_domain"></a>Planeje o acesso ao domínio  
Para fazer logon no domínio, o computador deve ser um computador membro do domínio e a conta de usuário deve ser criada no AD DS antes da tentativa de logon. Além disso, a maioria dos procedimentos neste guia exigir que a conta de usuário é um membro dos grupos de administradores corporativos ou administradores de domínio no Active Directory Users and Computers, portanto, você deve fazer logon para a autoridade de certificação com uma conta que tenha a associação ao grupo apropriado.  
  
Para obter mais informações, consulte o Windows Server 2016 [guia da rede principal](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_virtual"></a>Planeje o local e o nome do diretório virtual em seu servidor Web  
Para fornecer acesso a CRL e o certificado da CA para outros computadores, você deve armazenar esses itens em um diretório virtual em seu servidor Web. Neste guia, o diretório virtual está localizado no servidor Web WEB1. Esta pasta está na unidade "C" e é chamada "pki". Você pode localizar seu diretório virtual em seu servidor Web em qualquer local da pasta que seja adequado para a implantação.  
  
## <a name="bkmk_cname"></a>Planeje um registro de alias (CNAME) DNS para o servidor Web  
Registros de recurso de alias (CNAME) são também chamados de registros de recurso de nome canônico. Com esses registros, você pode usar mais de um nome para apontar para um único host, tornando mais fácil fazer coisas como host tanto um servidor de protocolo de transferência de arquivo (FTP) e um servidor Web no mesmo computador. Por exemplo, os nomes de servidor conhecido (ftp, www) são registrados usando registros de recurso de alias (CNAME) que mapeiam para o sistema de nome de domínio (DNS) nome do host, como WEB1, para o computador servidor que hospeda esses serviços.  
  
Este guia fornece instruções para configurar seu servidor Web para hospedar a lista de certificados revogados (CRL) para sua autoridade de certificação (CA). Também convém usar seu servidor Web para outras finalidades, como para hospedar um site ou FTP, é uma boa ideia para criar um registro de alias de recurso no DNS para seu servidor Web. Neste guia, o registro CNAME é denominado "pki", mas você pode escolher um nome que seja adequado para a implantação.  
  
## <a name="bkmk_capolicy"></a>Configuração de plano de CAPolicy  
Antes de instalar o AD CS, você deve configurar CAPolicy na autoridade de certificação com informações que estão corretas para a implantação. Um arquivo CAPolicy contém as seguintes informações:  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=http://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
Você deve planejar os seguintes itens para esse arquivo:  
  
-   **URL**. O exemplo de arquivo CAPolicy.inf tem um valor de URL de **http://pki.corp.contoso.com/pki/cps.txt**. Isso ocorre porque o servidor Web neste guia é denominado WEB1 e possui um registro de recurso DNS CNAME de pki. O servidor Web também é associado ao domínio corp.contoso.com. Além disso, há um diretório virtual no servidor Web chamado "pki" onde a lista de certificados revogados está armazenada. Certifique-se de que o valor que você fornecer para URL no seu arquivo CAPolicy aponta para um diretório virtual em seu servidor Web em seu domínio.  
  
-   **RenewalKeyLength**. O comprimento da chave padrão renovação do AD CS no Windows Server 2012 é 2048. O comprimento da chave que você selecionar deve ser tanto quanto possível enquanto ainda fornece compatibilidade com os aplicativos que você pretende usar.  
  
-   **RenewalValidityPeriodUnits**. O exemplo de arquivo CAPolicy tem um valor de RenewalValidityPeriodUnits de 5 anos. Isso ocorre porque o tempo de vida esperado da CA é cerca de dez anos. O valor de RenewalValidityPeriodUnits deve refletir o período de validade geral de CA ou o maior número de anos para o qual você deseja fornecer a inscrição.  
  
-   **CRLPeriodUnits**. O exemplo de arquivo CAPolicy tem um valor de CRLPeriodUnits 1. Isso ocorre porque o intervalo de atualização de exemplo para a lista de certificados revogados neste guia é de 1 semana. O valor de intervalo que você especificar com essa configuração, você deve publicar a CRL na CA para o diretório virtual do servidor da Web onde armazenar a CRL e fornecer acesso a ele para computadores que estão no processo de autenticação.  
  
-   **AlternateSignatureAlgorithm**. Este CAPolicy implementa um mecanismo de segurança aprimorada implementando formatos alternativos de assinatura. Você não deve implementar essa configuração se você ainda tem clientes do Windows XP que exigem certificados desta autoridade de certificação.  
  
Se você não planeja adicionar qualquer subordinadas a sua infraestrutura de chave pública mais tarde, e se você deseja impedir que a adição de qualquer subordinadas, você pode adicionar a chave PathLength ao seu arquivo CAPolicy com um valor de 0. Para adicionar essa chave, copie e cole o seguinte código no arquivo:  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> Não é recomendável que você alterar outras configurações no arquivo CAPolicy. inf, a menos que você tenha um motivo específico para fazê-lo.  
  
## <a name="bkmk_cdp"></a>Planejar a configuração das extensões de CDP e AIA em CA1  
Quando você configura o ponto de distribuição de lista de revogação de certificados (CRL) (CDP) e as configurações de acesso de informações da autoridade (AIA) em CA1, você precisa do nome do seu servidor Web e seu nome de domínio. Você também precisará do nome da pasta virtual que você cria no servidor Web, onde a lista de certificados revogados (CRL) e o certificado de autoridade de certificação são armazenados.  
  
O local de CDP que você deve inserir durante essa etapa de implantação tem o formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Por exemplo, se o seu servidor Web é chamado WEB1 e o registro CNAME alias para o servidor Web DNS é "pki", é de seu domínio corp.contoso.com e seu diretório é nomeado pki, a localização de CDP é:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
O local de AIA que você deve inserir tem o formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Por exemplo, se o seu servidor Web é chamado WEB1 e o registro CNAME alias para o servidor Web DNS é "pki", é de seu domínio corp.contoso.com e seu diretório é nomeado pki, a localização de AIA é:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Planejar a operação de cópia entre a CA e o servidor Web  
Para publicar o certificado CA e CRL da autoridade de certificação para o diretório de servidor da Web, você pode executar o comando de crl - certutil depois de definir os locais CDP e AIA na autoridade de certificação. Certifique-se de que você configure os caminhos corretos nas propriedades de CA **extensões** guia antes de executar esse comando usando as instruções neste guia. Além disso, para copiar o certificado de autoridade de certificação corporativa para o servidor Web, você deve ter já criou o diretório virtual no servidor Web e configurado a pasta como uma pasta compartilhada.  
  
## <a name="bkmk_template"></a>Planejar a configuração do modelo de certificado de servidor na autoridade de certificação  
Para implantar os certificados de servidor, você deve copiar o modelo de certificado denominado **RAS e o servidor IAS**. Por padrão, essa cópia é denominada **cópia do RAS e o servidor IAS**. Se você deseja renomear essa cópia do modelo, planeje o nome que você deseja usar durante essa etapa da implantação.  
  
> [!NOTE]  
> As seções de três últimos implantação neste guia - que permitem que você configurar o registro automático de certificado de servidor, atualize a política de grupo em servidores e verifique se que os servidores tem recebido um certificado de servidor válido da autoridade de certificação - não exigem etapas adicionais de planejamento.  
  


