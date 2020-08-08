---
title: Visão geral da implantação de certificado do servidor
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 318bab675cc633034731e369b5da2bbb40d810b0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949410"
---
# <a name="server-certificate-deployment-overview"></a>Visão geral da implantação de certificado do servidor

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico inclui as seções a seguir.

-   [Componentes de implantação de certificado do servidor](#bkmk_components)

-   [Visão geral do processo de implantação de certificado do servidor](#bkmk_process)

## <a name="server-certificate-deployment-components"></a><a name="bkmk_components"></a>Componentes de implantação de certificado do servidor
Você pode usar este guia para instalar Active Directory serviços de certificados (AD CS) como uma AC (autoridade de certificação) raiz corporativa e registrar certificados de servidor em servidores que executam o NPS (servidor de diretivas de rede), o serviço de roteamento e acesso remoto (RRAS) ou NPS e RRAS.


Se você implantar o SDN com a autenticação baseada em certificado, os servidores serão obrigados a usar um certificado do servidor para provar suas identidades para outros servidores para que eles obtenham comunicações seguras.

A ilustração a seguir mostra os componentes necessários para implantar certificados de servidor em servidores em sua infraestrutura de SDN.

![Infraestrutura necessária para implantação de certificado do servidor](../../../media/Nps-Certs/Nps-Certs.jpg)

> [!NOTE]
> Na ilustração acima, vários servidores são representados: DC1, CA1, WEB1 e muitos servidores SDN. Este guia fornece instruções para implantar e configurar o CA1 e o WEB1, e para configurar o DC1, que este guia pressupõe que você já tenha instalado em sua rede. Se você ainda não tiver instalado seu domínio de Active Directory, poderá fazer isso usando o [Guia de rede principal](https://technet.microsoft.com/library/mt604042.aspx) do Windows Server 2016.

Para obter mais informações sobre cada item representado na ilustração acima, consulte o seguinte:

-   [CA1](#bkmk_ca1)

-   [WEB1](#bkmk_web1)

-   [DC1](#bkmk_dc1)

-   [NPS1](#bkmk_nps1)

### <a name="ca1-running-the-ad-cs-server-role"></a><a name="bkmk_ca1"></a>CA1 executando a função de servidor AD CS
Nesse cenário, a AC (autoridade de certificação) raiz corporativa também é uma CA emissora. A AC emite certificados para computadores de servidor que têm as permissões de segurança corretas para registrar um certificado. Os serviços de certificado Active Directory (AD CS) são instalados em CA1.

Para redes maiores ou em que as questões de segurança fornecem justificativa, você pode separar as funções da CA raiz e da CA emissora e implantar CAs subordinadas que estão emitindo CAs.

Nas implantações mais seguras, a autoridade de certificação raiz corporativa é colocado offline e fisicamente protegida.

#### <a name="capolicyinf"></a>Arquivo de CAPolicy. inf
Antes de instalar o AD CS, você configura o arquivo CAPolicy. inf com configurações específicas para sua implantação.

#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Cópia do modelo de certificado de **Servidores RAS e ias**
Ao implantar certificados de servidor, você faz uma cópia do modelo de certificado de **Servidores RAS e ias** e, em seguida, configura o modelo de acordo com seus requisitos e as instruções neste guia.

Você utiliza uma cópia do modelo em vez do modelo original para que a configuração do modelo original seja preservada para possível uso futuro. Você configura a cópia do modelo de **Servidores RAS e ias** para que a autoridade de certificação possa criar certificados de servidor que ele emite para os grupos em Active Directory usuários e computadores que você especificar.

#### <a name="additional-ca1-configuration"></a>Configuração de CA1 adicional
A CA publica uma CRL (lista de certificados revogados) que os computadores devem verificar para garantir que os certificados apresentados a eles como prova de identidade sejam certificados válidos e não tenham sido revogados. Você deve configurar sua autoridade de certificação com o local correto da CRL para que os computadores saibam onde procurar a CRL durante o processo de autenticação.

### <a name="web1-running-the-web-services-iis-server-role"></a><a name="bkmk_web1"></a>WEB1 executando a função de servidor de serviços Web (IIS)
No computador que está executando a função de servidor do servidor Web (IIS), WEB1, você deve criar uma pasta no Windows Explorer para uso como o local para a CRL e o AIA.

#### <a name="virtual-directory-for-the-crl-and-aia"></a>Diretório virtual para CRL e AIA
Depois de criar uma pasta no Windows Explorer, você deve configurar a pasta como um diretório virtual no Gerenciador Serviços de Informações da Internet (IIS), bem como configurar a lista de controle de acesso para o diretório virtual para permitir que os computadores acessem o AIA e a CRL após serem publicados lá.

### <a name="dc1-running-the-ad-ds-and-dns-server-roles"></a><a name="bkmk_dc1"></a>DC1 executando as funções de servidor AD DS e DNS
DC1 é o controlador de domínio e o servidor DNS em sua rede.

#### <a name="group-policy-default-domain-policy"></a>Política de Grupo Diretiva de domínio padrão
Depois de configurar o modelo de certificado na autoridade de certificação, você pode configurar a política de domínio padrão no Política de Grupo para que os certificados sejam registrados automaticamente nos servidores NPS e RAS. O Política de Grupo está configurado em AD DS no servidor DC1.

#### <a name="dns-alias-cname-resource-record"></a>Registro de recurso de alias DNS (CNAME)
Você deve criar um registro de recurso de alias (CNAME) para o servidor Web para garantir que outros computadores possam encontrar o servidor, bem como o AIA e a CRL que estão armazenados no servidor. Além disso, o uso de um registro de recurso CNAME de alias fornece flexibilidade para que você possa usar o servidor Web para outras finalidades, como hospedar sites Web e FTP.

### <a name="nps1-running-the-network-policy-server-role-service-of-the-network-policy-and-access-services-server-role"></a><a name="bkmk_nps1"></a>NPS1 executando o serviço de função do servidor de políticas de rede da função de servidor de serviços de acesso e política de rede
O NPS é instalado quando você executa as tarefas no guia de rede do Windows Server 2016 Core, portanto, antes de executar as tarefas neste guia, você já deve ter um ou mais NPSs instalados em sua rede.

#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Política de Grupo aplicado e certificado registrado nos servidores
Depois de configurar o modelo de certificado e o registro automático, você pode atualizar Política de Grupo em todos os servidores de destino. Neste momento, os servidores registram o certificado do servidor do CA1.

### <a name="server-certificate-deployment-process-overview"></a><a name="bkmk_process"></a>Visão geral do processo de implantação de certificado do servidor

> [!NOTE]
> Os detalhes de como executar essas etapas são fornecidos na seção implantação de [certificado do servidor](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).

O processo de configuração do registro de certificado do servidor ocorre nestes estágios:

1.  No WEB1, instale a função do servidor Web (IIS).

2.  No DC1, crie um registro de alias (CNAME) para seu servidor Web, WEB1.

3.  Configure o servidor Web para hospedar a CRL da autoridade de certificação e, em seguida, publique a CRL e copie o certificado de autoridade de certificação raiz corporativa para o novo diretório virtual.

4.  No computador em que você está planejando instalar o AD CS, atribua ao computador um endereço IP estático, renomeie o computador, ingresse o computador no domínio e faça logon no computador com uma conta de usuário que seja membro dos grupos admins. do domínio e administradores da empresa.

5.  No computador em que você pretende instalar o AD CS, configure o arquivo CAPolicy. inf com configurações específicas à sua implantação.

6.  Instale a função de servidor do AD CS e execute a configuração adicional da autoridade de certificação.

7.  Copie a CRL e o certificado de autoridade de certificação de CA1 para o compartilhamento no servidor Web WEB1.

8.  Na AC, configure uma cópia do modelo de certificado de servidores RAS e IAS. A CA emite certificados com base em um modelo de certificado, portanto, você deve configurar o modelo para o certificado do servidor antes que a autoridade de certificação possa emitir um certificado.

9.  Configure o registro automático do certificado do servidor no Política de Grupo. Quando você configura o registro automático, todos os servidores que você especificou com Active Directory associações de grupo recebem automaticamente um certificado de servidor quando Política de Grupo em cada servidor é atualizado. Se você adicionar mais servidores posteriormente, eles também receberão automaticamente um certificado de servidor.

10. Atualizar Política de Grupo em servidores. Quando Política de Grupo é atualizado, os servidores recebem o certificado do servidor, que é baseado no modelo que você configurou na etapa anterior. Esse certificado é usado pelo servidor para provar sua identidade para computadores cliente e outros servidores durante o processo de autenticação.

    > [!NOTE]
    > Todos os computadores membros do domínio recebem automaticamente o certificado da autoridade de certificação raiz da empresa sem a configuração do registro automático. Esse certificado é diferente do certificado do servidor que você configura e distribui usando o registro automático. O certificado da autoridade de certificação é instalado automaticamente no repositório de certificados das autoridades de certificação raiz confiáveis para todos os computadores membros do domínio para que eles confiem nos certificados emitidos por essa autoridade de certificação.

10. Verifique se todos os servidores registraram um certificado de servidor válido.



