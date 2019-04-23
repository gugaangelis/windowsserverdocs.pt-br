---
title: Visão geral da implantação de certificado do servidor
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0cafa4bdeb80b22d6bac4ad09bcae9436cda97c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877817"
---
# <a name="server-certificate-deployment-overview"></a>Visão geral da implantação de certificado do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico contém as seguintes seções.  
  
-   [Componentes de implantação de certificado do servidor](#bkmk_components)
  
-   [Visão geral do processo de implantação do servidor certificado](#bkmk_process)
  
## <a name="bkmk_components"></a>Componentes de implantação de certificado do servidor
Você pode usar este guia para instalar os serviços de certificados do Active Directory (AD CS) como uma autoridade de certificação raiz corporativa (CA) e para registrar certificados de servidor para servidores que executam o serviço servidor de diretivas de rede (NPS), roteamento e acesso remoto (RRAS) ou NPS e RRAS.


Se você implantar SDN com autenticação baseada em certificado, os servidores são necessários para usar um certificado de servidor para comprovar suas identidades para outros servidores, de modo que eles atingem comunicações seguras.
  
A ilustração a seguir mostra os componentes que são necessárias para implantar certificados de servidor para servidores na sua infraestrutura SDN.
  
![Infraestrutura necessária de implantação de certificados de servidor](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> Na ilustração acima, vários servidores são descritos: DC1, CA1, WEB1 e muitos servidores SDN. Este guia fornece instruções para implantar e configurar CA1 e WEB1 e para configurar o DC1, que este guia pressupõe que você já tiver instalado na sua rede. Se você ainda não instalou o domínio do Active Directory, você pode fazer isso usando o [guia da rede principal](https://technet.microsoft.com/library/mt604042.aspx) para Windows Server 2016.  
  
Para obter mais informações sobre cada item mostrado na ilustração acima, consulte o seguinte:  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 executando a função de servidor AD CS  
Nesse cenário, a CA (autoridade de certificação) raiz corporativa também é uma AC emissora. A autoridade de certificação emite certificados para computadores de servidor que tem as permissões de segurança corretas para registrar um certificado. Os serviços de certificados do Active Directory (AD CS) é instalado em CA1.  
  
Para onde as preocupações de segurança fornecem uma justificativa ou redes maiores, você pode separar as funções da raiz da autoridade de certificação e a autoridade de certificação emissora e implantar autoridades de certificação subordinadas são CAs de emissão.  
  
Nas implantações mais seguras, a autoridade de certificação raiz corporativa é colocada offline e fisicamente protegida.   
  
#### <a name="capolicyinf"></a>CAPolicy.inf  
Antes de instalar o AD CS, você configura o arquivo CAPolicy. inf com configurações específicas para sua implantação.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Cópia do **servidores RAS e IAS** modelo de certificado  
Quando você implantar certificados de servidor, é fazer uma cópia do **servidores RAS e IAS** modelo de certificado e, em seguida, configurar o modelo de acordo com seus requisitos e as instruções neste guia.   
  
Para que a configuração do modelo original é preservada para uso futuro possíveis, você utilizar uma cópia do modelo em vez do modelo original. A cópia de você configura o **servidores RAS e IAS** modelo para que a autoridade de certificação possa criar certificados de servidor que ele emite para os grupos de usuários do Active Directory e computadores que você especificar.  
  
#### <a name="additional-ca1-configuration"></a>Configuração adicional de CA1  
A autoridade de certificação publica uma lista de certificados revogados (CRL) que computadores devem verificar para garantir que os certificados que são apresentados a eles como prova de identidade são certificados válidos e não foi revogado. Você deve configurar sua autoridade de certificação com o local correto da CRL para que computadores sabe onde procurar a CRL durante o processo de autenticação.  
  
### <a name="bkmk_web1"></a>WEB1 executando a função de servidor de serviços Web (IIS)  
No computador que está executando a função de servidor servidor Web (IIS), WEB1, você deve criar uma pasta no Windows Explorer para uso como o local de CRL e AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Diretório virtual para a CRL e AIA  
Depois de criar uma pasta no Windows Explorer, você deve configurar a pasta como um diretório virtual no Gerenciador de serviços de informações da Internet (IIS), bem como configurar a lista de controle de acesso para o diretório virtual para permitir que computadores acessem o AIA e a CRL Depois que eles são publicados lá.  
  
### <a name="bkmk_dc1"></a>DC1 executando as funções de servidor AD DS e DNS  
DC1 é o controlador de domínio e servidor DNS na sua rede.  
  
#### <a name="group-policy-default-domain-policy"></a>Diretiva de domínio padrão de política de grupo  
Depois de configurar o modelo de certificado na autoridade de certificação, você pode configurar a política de domínio padrão na diretiva de grupo para que os certificados são inscritos automaticamente para servidores NPS e RAS. Diretiva de grupo é configurada no AD DS no servidor do DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS alias (CNAME) resource record  
Você deve criar um registro de recurso de alias (CNAME) para o servidor Web garantir que os outros computadores podem encontrar o servidor, bem como o AIA e a CRL que são armazenados no servidor. Além disso, usando um registro de recurso CNAME alias fornece flexibilidade para que você possa usar o servidor Web para outros fins, como hospedagem de sites da Web e FTP.  
  
### <a name="bkmk_nps1"></a>NPS1 executando o serviço de função de servidor de políticas de rede da função de servidor Serviços de acesso e diretiva de rede  
O NPS é instalado quando você executa as tarefas no guia de rede do Windows Server 2016 Core, portanto, antes de executar as tarefas neste guia, você já deve ter um ou mais NPSs instalados em sua rede.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Política de grupo aplicada e o certificado registrado para servidores  
Depois de configurar o modelo de certificado e registro automático, você pode atualizar a política de grupo em todos os servidores de destino. Neste momento, os servidores de registrar o certificado do servidor de CA1.  
  
### <a name="bkmk_process"></a>Visão geral do processo de implantação do servidor certificado  
  
> [!NOTE]  
> Os detalhes de como executar essas etapas são fornecidos na seção [implantação de certificados de servidor](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
O processo de configuração de registro de certificado do servidor ocorre nesses estágios:  
  
1.  No WEB1, instale a função de servidor Web (IIS).  
  
2.  No DC1, crie um registro de alias (CNAME) para seu servidor Web, WEB1.  
  
3.  Configure o servidor Web para hospedar a CRL da AC, em seguida, publicar a CRL e copie o certificado de autoridade de certificação raiz corporativa para o novo diretório virtual.  
  
4.  No computador em que você planeja instalar o AD CS, atribua o computador um endereço IP estático, renomear o computador, ingressar o computador no domínio e, em seguida, faça logon no computador com uma conta de usuário que seja membro dos grupos Admins. do domínio e administradores de empresa.  
  
5.  No computador em que você planeja instalar o AD CS, configure o arquivo CAPolicy. inf com configurações que são específicas para a implantação.  
  
6.  Instalar a função de servidor AD CS e executar uma configuração adicional da autoridade de certificação.  
  
7.  Copie o certificado de autoridade de certificação da CRL de CA1 para o compartilhamento no servidor Web WEB1.  
  
8.  Na AC, configure uma cópia do modelo de certificado servidores RAS e IAS. A autoridade de certificação emite certificados com base em um modelo de certificado, portanto, você deve configurar o modelo para o certificado do servidor antes que a CA pode emitir um certificado.  
  
9.  Configure o registro automático de certificado de servidor na diretiva de grupo. Quando você configura o registro automático, todos os servidores que você especificou com associações de grupo do Active Directory automaticamente recebem um certificado de servidor quando a política de grupo em cada servidor está atualizada. Se você adicionar mais servidores mais tarde, eles receberão automaticamente um certificado de servidor muito.  
  
10. Atualize a política de grupo em servidores. Quando a diretiva de grupo é atualizada, os servidores recebem o certificado do servidor, que se baseia no modelo que você configurou na etapa anterior. Esse certificado é usado pelo servidor para comprovar sua identidade para computadores cliente e outros servidores durante o processo de autenticação.  
  
    > [!NOTE]  
    > Todos os computadores de membro do domínio recebem automaticamente o certificado da autoridade de certificação raiz corporativa sem a configuração de registro automático. Esse certificado é diferente do que o certificado do servidor que você configure e distribuir usando o registro automático. O certificado da AC é instalado automaticamente no repositório de certificados de autoridades de certificação raiz confiáveis para todos os computadores de membro do domínio para que eles confiará os certificados emitidos por essa AC.   
  
10. Verifique se todos os servidores de tem registrado um certificado de servidor válido.  
  


