---
title: Visão geral de implantação de certificado do servidor
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4650c99325def63fd0df25989c661230fada3dac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-overview"></a>Visão geral de implantação de certificado do servidor

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico contém as seções a seguir.  
  
-   [Componentes de implantação de certificado do servidor](#bkmk_components)
  
-   [Visão geral do processo de implantação do servidor certificado](#bkmk_process)
  
## <a name="bkmk_components"></a>Componentes de implantação de certificado do servidor
Você pode usar este guia para instalar os serviços de certificados do Active Directory (AD CS) como uma autoridade de certificação raiz corporativa (CA) e certificados de servidor para servidores que executam o serviço de servidor de política de rede (NPS), roteamento e acesso remoto (RRAS), ou NPS e RRAS.


Se você implantar SDN com autenticação baseada em certificado, servidores são necessárias para usar um certificado de servidor para provar sua identidade para outros servidores para que eles obter comunicações seguras.
  
A ilustração a seguir mostra os componentes que são necessárias para implantar certificados de servidor para servidores em sua infraestrutura SDN.
  
![Infraestrutura necessária de implantação do certificado de servidor](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> Na ilustração acima, vários servidores são retratados: DC1, CA1, WEB1 e SDN muitos servidores. Este guia fornece instruções para implantar e configurar CA1 e WEB1 e configurando DC1, que este guia pressupõe que você já tiver instalado em sua rede. Se você ainda não tiver instalado seu domínio do Active Directory, você pode fazer isso usando o [guia da rede principal](https://technet.microsoft.com/library/mt604042.aspx) para Windows Server 2016.  
  
Para obter mais informações sobre cada item mostrado na ilustração acima, consulte o seguinte:  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 executa a função de servidor do AD CS  
Nesse cenário, a autoridade de certificação (CA) raiz corporativa também é uma CA de emissão. A CA emite certificados em computadores de servidor que tem as permissões de segurança correto para registrar um certificado. Os serviços de certificados do Active Directory (AD CS) é instalado no CA1.  
  
Para redes maiores ou onde preocupações de segurança fornecem justificação, você pode separar as funções de CA raiz e CA emissora e implantar subordinadas que são a emissão de autoridades de certificação.  
  
Em implantações as mais seguras, a autoridade de certificação raiz corporativa é levada offline e fisicamente seguro.   
  
#### <a name="capolicyinf"></a>CAPolicy  
Antes de instalar o AD CS, configure o arquivo CAPolicy com configurações específicas para a implantação.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copie do **servidores RAS e IAS** modelo de certificado  
Quando você implanta certificados de servidor, fazer uma cópia da **servidores RAS e IAS** modelo de certificado e, em seguida, configure o modelo de acordo com seus requisitos e as instruções neste guia.   
  
Você utilizar uma cópia do modelo, em vez do modelo original, para que a configuração do modelo original é preservada para possível uso futuro. Configurar a cópia do **servidores RAS e IAS** modelo para que a autoridade de certificação pode criar certificados de servidor que emitir para os grupos no Active Directory usuários e computadores que você especificar.  
  
#### <a name="additional-ca1-configuration"></a>Configuração CA1 adicional  
A autoridade de certificação publica uma lista de certificados revogados (CRL) que computadores devem verificar para garantir que os certificados que são apresentados-los como prova de identidade são certificados válidos e não foi revogado. Você deve configurar sua autoridade de certificação com o local correto da CRL para que os computadores saibam onde procurar o CRL durante o processo de autenticação.  
  
### <a name="bkmk_web1"></a>WEB1 executa a função de servidor de serviços Web (IIS)  
No computador que executa a função de servidor de Web Server (IIS), WEB1, você deve criar uma pasta no Windows Explorer para uso como o local de CRL e AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Diretório de CRL e AIA  
Depois de criar uma pasta no Windows Explorer, você deve configurar a pasta como um diretório virtual no Gerenciador de serviços de informações da Internet (IIS), bem como configurar a lista de controle de acesso para o diretório virtual para permitir que os computadores para acessar o AIA e CRL depois de serem publicados lá.  
  
### <a name="bkmk_dc1"></a>DC1 executando as funções de servidor do AD DS e DNS  
DC1 é o controlador de domínio e o servidor DNS em sua rede.  
  
#### <a name="group-policy-default-domain-policy"></a>Política de domínio do padrão de política de grupo  
Depois de definir o modelo de certificado na autoridade de certificação, você pode configurar a política de domínio padrão na política de grupo para que os certificados são registrados automaticamente para servidores NPS e RAS. Política de grupo é configurada no AD DS no servidor DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS alias o registro de recurso (CNAME)  
Você deve criar um registro de recurso alias (CNAME) para o servidor Web garantir que outros computadores podem encontrar o servidor, bem como o AIA e a CRL que são armazenados no servidor. Além disso, usar um alias recurso CNAME fornece flexibilidade para que você pode usar o servidor Web para outras finalidades, como hospedagem de sites da Web e FTP.  
  
### <a name="bkmk_nps1"></a>NPS1 executando o serviço de função de servidor de política de rede da função de servidor Serviços de acesso e política de rede  
O servidor NPS é instalado quando você executar as tarefas no guia de rede do Windows Server 2016 Core, portanto, antes de executar as tarefas neste guia, você já deve ter um ou mais servidores NPS instalado em sua rede.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Política de grupo aplicado e certificado registrado para servidores  
Depois que você configurou o modelo de certificado e o registro automático, você pode atualizar a política de grupo em todos os servidores de destino. Neste momento, os servidores de registrar o certificado do servidor de CA1.  
  
### <a name="bkmk_process"></a>Visão geral do processo de implantação do servidor certificado  
  
> [!NOTE]  
> Os detalhes de como realizar essas etapas são fornecidos na seção [implantação de certificado de servidor](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
O processo de configuração de registro de certificado de servidor ocorre nesses estágios:  
  
1.  Em WEB1, instale a função Web Server (IIS).  
  
2.  Em DC1, crie um registro de alias (CNAME) para o servidor Web, WEB1.  
  
3.  Configure seu servidor Web para hospedar o CRL da autoridade de certificação, e depois publicar a CRL e copie o certificado da CA raiz da empresa para o novo diretório virtual.  
  
4.  No computador onde você pretende instalar o AD CS, atribua o computador de um endereço IP estático, renomear o computador, ingressar o computador ao domínio e, em seguida, fazer logon no computador com uma conta de usuário que seja um membro do grupo Admins. do domínio e administradores corporativos.  
  
5.  No computador onde você pretende instalar o AD CS, configure o arquivo CAPolicy com as configurações que são específicas para a implantação.  
  
6.  Instale a função de servidor do AD CS e execute a configuração adicional da autoridade de certificação.  
  
7.  Copie o certificado CA e CRL de CA1 para o compartilhamento no servidor Web WEB1.  
  
8.  A autoridade de certificação, configure uma cópia do modelo de certificado servidores RAS e IAS. A CA emite certificados com base em um modelo de certificado, portanto, você deve configurar o modelo para o certificado do servidor antes da CA pode emitir um certificado.  
  
9.  Configure o registro automático de certificado de servidor na política de grupo. Quando você configura o registro automático, todos os servidores que você especificou com associações de grupo do Active Directory automaticamente recebem um certificado de servidor quando a diretiva de grupo em cada servidor é atualizada. Se você adicionar mais servidores mais tarde, eles receberão automaticamente um certificado de servidor, também.  
  
10. Atualize a política de grupo em servidores. Quando a política de grupo for atualizada, os servidores de recebem o certificado do servidor, que é baseado no modelo que você configurou na etapa anterior. Esse certificado é usado pelo servidor para provar sua identidade para computadores cliente e outros servidores durante o processo de autenticação.  
  
    > [!NOTE]  
    > Todos os computadores membros do domínio recebem automaticamente o certificado da CA raiz corporativa sem a configuração de registro automático. Esse certificado é diferente do que o certificado do servidor que você configure e distribuir usando o registro automático. Certificado da CA é automaticamente instalado no repositório de certificados de autoridades de certificação confiáveis para todos os computadores membros do domínio para que eles confiará certificados são emitidos por essa CA.   
  
10. Verifique se que todos os servidores tiverem registrado um certificado de servidor válido.  
  


