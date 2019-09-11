---
title: Implantar acesso sem fio autenticado 802.1X baseado em senha
description: Este tópico faz parte do guia de rede do Windows Server 2016 "implantar o acesso sem fio autenticado 802.1 X com base em senha"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ee20e002aa8dad29eefcda87a7b949ffb0bb6e9b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868939"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Implantar o\-acesso sem fio autenticado 802.1 x baseado em senha

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este é um guia complementar para o guia de&reg; rede do Windows Server 2016 Core. O guia de rede principal fornece instruções para planejar e implantar os componentes necessários para uma rede totalmente funcional e um novo Active Directory® domínio em uma nova floresta.

Este guia explica como se basear em uma rede principal fornecendo instruções sobre como implantar o Instituto de engenheiros \(elétricos e eletrônicos do IEEE\) 802.1\-x autenticado com acesso sem fio IEEE 802,11 usando Protocolo de autenticação extensível protegida – protocolo de autenticação de handshake de \(desafio\-da Microsoft versão\)2 PEAP MS\-CHAP v2.

Como o\-PEAP\-MS CHAP v2 requer que os usuários\-forneçam credenciais baseadas em senha em vez de um certificado durante o processo de autenticação, normalmente é mais fácil e menos dispendioso implantar do que o EAP\-TLS ou PEAP\-TLS.

>[!NOTE]
>Neste guia, o acesso sem fio autenticado IEEE 802.1 x com\-PEAP\-MS CHAP v2 é abreviado como "acesso sem fio" e "acesso WiFi".

## <a name="about-this-guide"></a>Sobre este guia
Este guia, em combinação com os guias de pré-requisitos descritos abaixo, fornece instruções sobre como implantar a infraestrutura de acesso WiFi a seguir.  

- Um ou mais\- \(pontosdeacesso sem fio 802,11 com capacidade 802.1 x compatíveis.\)

- Active Directory Domain Services \(ADDS\) usuários e computadores.

- Gerenciamento de Política de Grupo.

- Um ou mais servidores NPS \(\) do servidor de políticas de rede.

- Certificados de servidor para computadores que executam o NPS.

- Computadores cliente sem fio que&reg; executam o windows 10, Windows 8.1 ou Windows 8.

### <a name="dependencies-for-this-guide"></a>Dependências deste guia

Para implantar com êxito o sem fio autenticado com este guia, você deve ter um ambiente de rede e de domínio com todas as tecnologias necessárias implantadas. Você também deve ter certificados de servidor implantados em seu NPSs de autenticação.

As seções a seguir fornecem links para a documentação do que mostra como implantar essas tecnologias.

#### <a name="network-and-domain-environment-dependencies"></a>Dependências de ambiente de rede e domínio

Este guia foi projetado para administradores de rede e de sistema que seguiram as instruções no guia de **rede** do Windows Server 2016 Core para implantar uma rede principal ou para aqueles que implantaram anteriormente as tecnologias principais incluídas no núcleo rede, incluindo AD DS \(, DNS\)do sistema de nome de domínio, protocolo \(DHCP\)de configuração\/de host dinâmico, IP TCP, NPS e \(serviço de cadastramento na Internet do Windows WINS\).  

O guia de rede principal está disponível nos seguintes locais:

- O [Guia de rede](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) do windows Server 2016 Core está disponível na biblioteca técnica do windows Server 2016. 

- O [Guia de rede principal](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) também está disponível no formato Word na galeria do TechNet [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683), em.

#### <a name="server-certificate-dependencies"></a>Dependências de certificado do servidor
Há duas opções disponíveis para registrar servidores de autenticação com certificados de servidor para uso com autenticação 802.1 x – implante sua própria infraestrutura de chave pública usando Active Directory serviços \(de certificados AD CS ou\) Use certificados de servidor registrados por uma AC \(\)de autoridade de certificação pública.

##### <a name="ad-cs"></a>CS DO AD
Os administradores de rede e de sistema que implantam o sem fio autenticado devem seguir as instruções no guia complementar de rede do Windows Server 2016 Core, **implantar certificados de servidor para implantações com e sem fio 802.1 x**. Este guia explica como implantar e usar o AD CS para registrar automaticamente certificados de servidor em computadores que executam o NPS.

Este guia está disponível no local a seguir.

- O guia complementar de rede do Windows Server 2016 Core [implanta certificados de servidor para implantações com e sem fio 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) em formato HTML na biblioteca técnica. 

##### <a name="public-ca"></a>CA Pública
Você pode comprar certificados de servidor de uma CA pública, como a VeriSign, que os computadores cliente já confiam. 

Um computador cliente confia em uma AC quando o certificado de autoridade de certificação é instalado no repositório de certificados de autoridades de certificação raiz confiáveis. Por padrão, os computadores que executam o Windows têm vários certificados de AC pública instalados em seu repositório de certificados de autoridades de certificação raiz confiáveis.  

É recomendável que você examine os guias de design e implantação de cada uma das tecnologias usadas neste cenário de implantação. Esses guias podem ajudar a determinar se o cenário de implantação fornece os serviços e as configurações que você precisa para a rede de sua organização.

### <a name="requirements"></a>Requisitos
A seguir estão os requisitos para implantar uma infraestrutura de acesso sem fio usando o cenário documentado neste guia:

- Antes de implantar esse cenário, primeiro você deve adquirir pontos de\-acesso sem fio compatíveis com 802.1 x para fornecer cobertura sem fio nos locais desejados no seu site. A seção de planejamento deste guia ajuda a determinar os recursos aos quais seu APs deve dar suporte.

- Active Directory Domain Services \(ADDS\) está instalado, assim como as outras tecnologias de rede necessárias, de acordo com as instruções no guia de rede do Windows Server 2016 Core.  

- O AD CS é implantado e os certificados de servidor são registrados no NPSs. Esses certificados são necessários quando você implanta o método\-de\-autenticação baseado em\-certificado do MS CHAP v2 de PEAP que é usado neste guia.

- Um membro da sua organização está familiarizado com os padrões IEEE 802,11 que são suportados por seus APs sem fio e os adaptadores de rede sem fio instalados nos computadores cliente e dispositivos na sua rede. Por exemplo, alguém em sua organização está familiarizado com tipos de frequência de rádio, 802,11 \(autenticação sem fio\)WPA2 ou WPA e \(codificações AES\)ou TKIP.

## <a name="what-this-guide-does-not-provide"></a>O que este guia não contém  
A seguir estão alguns itens que este guia não fornece:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Diretrizes abrangentes para selecionar pontos\-de acesso sem fio compatíveis com 802.1 x

Como existem muitas diferenças entre marcas e modelos de APS sem\-fio compatíveis com 802.1 x, este guia não fornece informações detalhadas sobre:  

- Determinar qual marca ou modelo de AP sem fio é mais adequada para suas necessidades.

- A implantação física de APs sem fio em sua rede.

- Configuração avançada de AP sem fio, como para \(\)VLANs de redes locais virtuais sem fio.

- Instruções sobre como configurar atributos específicos de fornecedor\-de AP sem fio no NPS.

Além disso, a terminologia e os nomes das configurações variam entre marcas e modelos de AP sem fio e podem não corresponder aos nomes de configuração genérica que são usados neste guia. Para obter detalhes de configuração de AP sem fio, você deve examinar a documentação do produto fornecida pelo fabricante de seus APs sem fio.

### <a name="instructions-for-deploying-nps-certificates"></a>Instruções para implantar certificados do NPS
  
Há duas alternativas para a implantação de certificados NPS. Este guia não fornece diretrizes abrangentes para ajudá-lo a determinar qual alternativa atenderá melhor às suas necessidades. Em geral, no entanto, as opções que você enfrenta são:

- Comprar certificados de uma CA pública, como a Verisign, que já são confiáveis para clientes\-baseados no Windows. Essa opção geralmente é recomendada para redes menores.

- Implantar uma PKI \(\) de infraestrutura de chave pública em sua rede usando o AD CS. Isso é recomendado para a maioria das redes, e as instruções sobre como implantar certificados de servidor com o AD CS estão disponíveis no guia de implantação mencionado anteriormente.

### <a name="nps-network-policies-and-other-nps-settings"></a>Políticas de rede do NPS e outras configurações de NPS

Exceto para as definições de configuração feitas quando você executa o assistente para **Configurar 802.1 x** , conforme documentado neste guia, este guia não fornece informações detalhadas para configurar manualmente as condições do NPS, as restrições ou outras configurações de NPS.

### <a name="dhcp"></a>DHCP

Este guia de implantação não fornece informações sobre como projetar ou implantar sub-redes DHCP para LANs sem fio.

## <a name="technology-overviews"></a>Visões gerais da tecnologia
Veja a seguir as visões gerais de tecnologia para a implantação de acesso sem fio:

### <a name="ieee-8021x"></a>IEEE 802.1X
O padrão IEEE 802.1 x define o controle\-de acesso à rede baseado em porta usado para fornecer acesso de rede autenticado a redes Ethernet. Esse controle\-de acesso à rede baseado em porta usa as características físicas da infraestrutura de LAN comutada para autenticar dispositivos conectados a uma porta de LAN. O acesso à porta pode ser negado se o processo de autenticação falhar. Embora esse padrão tenha sido projetado para redes Ethernet com fio, ele foi adaptado para uso em LANs sem fio 802,11.

### <a name="8021x-capable-wireless-access-points-aps"></a>\- pontos\(de acesso sem fio compatíveis com 802.1 x\)
Esse cenário requer a implantação de um ou mais APS sem\-fio com capacidade 802.1 x que são compatíveis com o protocolo\-RADIUS\) de autenticação \(remota no serviço do usuário.

Os APS em conformidade\-com 802.1 x e RADIUS, quando implantados em uma infraestrutura RADIUS com um servidor RADIUS, como um NPS, são chamados de *clientes RADIUS*.

### <a name="wireless-clients"></a>Clientes sem fio
Este guia fornece detalhes de configuração abrangentes para fornecer acesso autenticado 802.1 x\-para usuários membros do domínio que se conectam à rede com computadores cliente sem fio que executam o Windows 10, Windows 8.1 e Windows 8. Os computadores devem ser ingressados no domínio para estabelecer o acesso autenticado com êxito.

>[!NOTE]
>Você também pode usar computadores que executam o Windows Server 2016, o Windows Server 2012 R2 e o Windows Server 2012 como clientes sem fio.

### <a name="support-for-ieee-80211-standards"></a>Suporte para padrões IEEE 802,11
Os sistemas operacionais Windows e Windows Server com suporte\-oferecem suporte interno para a rede sem fio 802,11. Nesses sistemas operacionais, um adaptador de rede sem fio 802,11 instalado aparece como uma conexão de rede sem fio no centro de rede e compartilhamento. 

Embora haja suporte interno\-para a rede sem fio 802,11, os componentes sem fio do Windows dependem do seguinte:

- Os recursos do adaptador de rede sem fio. O adaptador de rede sem fio instalado deve dar suporte à LAN sem fio ou aos padrões de segurança sem fio que você precisa. Por exemplo, se o adaptador de rede sem fio não der\-suporte ao WPA \(\)Wi-Fi Protected Access, você não poderá habilitar ou configurar as opções de segurança WPA.

- Os recursos do driver do adaptador de rede sem fio. Para permitir que você configure opções de rede sem fio, o driver para o adaptador de rede sem fio deve oferecer suporte ao relatório de todos os seus recursos para o Windows. Verifique se o driver do adaptador de rede sem fio foi escrito para os recursos do seu sistema operacional. Além disso, verifique se o driver é a versão mais atual, verificando Microsoft Update ou o site do fornecedor do adaptador de rede sem fio.

A tabela a seguir mostra as taxas e as frequências de transmissão para os padrões sem fio IEEE 802,11 comuns.

|Padrões|Frequências|Taxas de transmissão de bits|Uso|
|-------------|---------------|--------------------------|---------|  
|802,11|\(\) Intervalo\- defrequênciade2,4doISMindustrial,científicoemédicodabandapara2,5GHz\(\)|2 megabits por segundo \(Mbps\)|Obsoleto. Comumente usado.|  
|padrões|ISM\-de banda S|11 Mbps|Normalmente usado.|  
|802.11 a|\- ISM\(de banda C 5,725 a 5,875 GHz\)|54 Mbps|Comumente usado devido à despesa e ao intervalo limitado.|  
|g|ISM\-de banda S|54 Mbps|Amplamente usado. dispositivos 802.11 g são compatíveis com dispositivos 802.11 b.|  
|802.11 n \ 2,4 e 5,0 GHz|C\-banda\-e o ISM de banda|250 Mbps|Os dispositivos com base no\-padrão Ratification IEEE 802.11 n foram disponibilizados em agosto de 2007. Muitos dispositivos 802.11 n são compatíveis com dispositivos 802.11 a, b e g.|
|802.11ac |5 GHz |6,93 Gbps |o 802.11 AC, aprovado pelo IEEE em 2014, é mais escalonável e mais rápido do que o 802.11 n e é implantado onde o APs e os clientes sem fio dão suporte a ele.|

### <a name="wireless-network-security-methods"></a>Métodos de segurança de rede sem fio
Os **métodos de segurança de rede sem fio** são um agrupamento \(informal de autenticação sem\) fio, às vezes chamado de segurança sem fio e criptografia de segurança sem fio. A autenticação e a criptografia sem fio são usadas em pares para impedir que usuários não autorizados acessem a rede sem fio e para proteger as transmissões sem fio. 

Ao definir as configurações de segurança sem fio nas políticas de rede sem fio do Política de Grupo, há várias combinações para escolher. No entanto, somente\-os padrões de\-Autenticação WPA2 Enterprise, WPA Enterprise e Open with 802.1 x têm suporte para implantações sem fio autenticadas 802.1 x.

>[!NOTE]
>Ao configurar as políticas de rede sem fio, você deve selecionar **WPA2\-Enterprise**, **WPA\-Enterprise**ou **abrir com 802.1 x** para obter acesso às configurações de EAP necessárias para 802.1 x autenticado implantações sem fio.  

#### <a name="wireless-authentication"></a>Autenticação sem fio
Este guia recomenda o uso dos seguintes padrões de autenticação sem fio para implantações sem fio autenticadas 802.1 X.  

**\(Wi\--Fi Protected Access – Enterprise\-WPA\) Enterprise** WPA é um padrão provisório desenvolvido pela Aliança WiFi para estar em conformidade com o protocolo de segurança sem fio 802,11. O protocolo WPA foi desenvolvido em resposta a várias falhas graves que foram descobertas no protocolo WEP \(\) de privacidade equivalente com fio anterior.

O\-WPA Enterprise fornece segurança aprimorada sobre o WEP:  

1. Exigir autenticação que usa a estrutura de EAP 802.1 X como parte da infraestrutura que garante a autenticação mútua centralizada e o gerenciamento dinâmico de chaves  

2. \(Aprimorando o valor de verificação de\) integridade ICV com um mic \(\)de verificação de integridade de mensagem, para proteger o cabeçalho e a carga  

3. Implementando um contador de quadros para desencorajar ataques de repetição  

**\-Wi\--Fi\) Protected Access 2 – Enterprise WPA2EnterprisecomooWPAEnterpriseStandard,oWPA2Enterpriseusaaestrutura802.1xeEAP.\(** \-\- O\-WPA2 Enterprise fornece proteção de dados mais forte para vários usuários e grandes redes gerenciadas. O\-WPA2 Enterprise é um protocolo robusto que foi projetado para impedir o acesso não autorizado à rede, verificando os usuários de rede por meio de um servidor de autenticação.  

#### <a name="wireless-security-encryption"></a>Criptografia de segurança sem fio
A criptografia de segurança sem fio é usada para proteger as transmissões sem fio que são enviadas entre o cliente sem fio e o AP sem fio. A criptografia de segurança sem fio é usada em conjunto com o método de autenticação de segurança de rede selecionado. Por padrão, os computadores que executam o Windows 10, Windows 8.1 e Windows 8 dão suporte a dois padrões de criptografia:

1. **Protocolo temporal de integridade de chave** O TKIP\) é um protocolo de criptografia mais antigo que foi originalmente projetado para fornecer criptografia sem fio mais segura do que o que foi fornecido pelo protocolo \(WEP\) de privacidade equivalente de Wired Equivalent inerentemente fraco. \( O TKIP foi projetado pelo grupo de tarefas IEEE 802.11 i e pela\-Wi-Fi Alliance para substituir o WEP sem exigir a substituição do hardware herdado. O TKIP é um conjunto de algoritmos que encapsula a carga WEP e permite que os usuários de equipamentos Wi-Fi herdados atualizem para TKIP sem substituir o hardware. Como o WEP, o TKIP usa o algoritmo de criptografia de fluxo RC4 como base. O novo protocolo, no entanto, criptografa cada pacote de dados com uma chave de criptografia exclusiva e as chaves são muito mais fortes do que aquelas pelo WEP. Embora o TKIP seja útil para atualizar a segurança em dispositivos mais antigos que foram projetados para usar apenas o WEP, ele não aborda todos os problemas de segurança que enfrentam LANs sem fio e, na maioria dos casos, não é suficientemente robusto para proteger dados governamentais ou corporativos confidenciais transmissões.  

2. **Criptografia AES** OAES\) é o protocolo de criptografia preferencial para a criptografia de dados comerciais e governamentais. \( O AES oferece um nível mais alto de segurança de transmissão sem fio do que o TKIP ou o WEP. Diferentemente do TKIP e do WEP, o AES requer hardware sem fio que ofereça suporte ao padrão AES. O AES é um\-padrão de criptografia de chave simétrica que usa três codificações\-de bloco,\-AES 128,\-AES 192 e AES 256.

No Windows Server 2016, os seguintes métodos\-de criptografia sem fio baseados em AES estão disponíveis para configuração em Propriedades de perfil sem fio quando você seleciona\-um método de autenticação de WPA2 Enterprise, que é recomendado.

1. **CCMP\-AES**. O protocolo \(CCMP\) de encadeamento de blocos de codificação do modo Counter Message Authentication Code implementa o padrão 802.11 i e é projetado para maior criptografia de segurança do que o fornecido pelo WEP e usa chaves de criptografia AES de 128 bits.
2. **GCMP\-AES**. O Galois Counter Mode \(Protocol\) GCMP tem suporte do 802.11 AC, é mais eficiente do\-que o AES CCMP e fornece melhor desempenho para clientes sem fio. GCMP usa chaves de criptografia AES de 256 bits.

> [!IMPORTANT]
> O WEP \(\) de privacidade de equivalência com fio era o padrão de segurança sem fio original que foi usado para criptografar o tráfego de rede. Você não deve implantar o WEP em sua rede porque há vulnerabilidades bem\-conhecidas nessa forma de segurança desatualizada.

### <a name="active-directorydoman-services-adds"></a>Active Directory serviços \(domínios AD DS\)
O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos\-de rede e dados\-específicos do aplicativo de aplicativos habilitados para diretório. Os administradores podem usar AD DS para organizar elementos de uma rede, como usuários, computadores e outros dispositivos, em uma estrutura de confinamento hierárquica. A estrutura de confinamento hierárquica inclui a floresta Active Directory, os domínios na floresta e as \(UOs\) das unidades organizacionais em cada domínio. Um servidor que está executando o AD DS é chamado de *controlador de domínio*.  

AD DS contém as contas de usuário, as contas de computador e as propriedades de conta exigidas pelo IEEE 802.1\-x\-e PEAP MS CHAP v2 para autenticar as credenciais do usuário e para avaliar a autorização de conexões sem fio.

### <a name="active-directory-users-and-computers"></a>Usuários e computadores do Active Directory
Active Directory usuários e computadores é um componente do AD DS que contém contas que representam entidades físicas, como um computador, uma pessoa ou um grupo de segurança. Um *grupo de segurança* é uma coleção de contas de usuário ou computador que os administradores podem gerenciar como uma única unidade. As contas de usuário e computador que pertencem a um grupo específico são chamadas de *membros do grupo*.  

### <a name="group-policy-management"></a>Gerenciamento de Política de Grupo
O gerenciamento de política de grupo\-permite a alteração baseada em diretório e o gerenciamento de configurações de usuário e computador, incluindo informações de segurança e de usuário. Você usa Política de Grupo para definir configurações para grupos de usuários e computadores. Com Política de Grupo, você pode especificar configurações para entradas de registro, segurança, instalação de software, scripts, redirecionamento de pasta, serviços de instalação remota e manutenção do Internet Explorer. As configurações de política de grupo que você cria estão contidas em um \(GPO\)de objeto de política de grupo. Ao associar um GPO a Active Directory contêineres de sistema selecionados — sites, domínios e UOs — você pode aplicar as configurações do GPO aos usuários e computadores nesses Active Directory contêineres. Para gerenciar objetos do política de grupo em uma empresa, você pode usar o console \(de gerenciamento do MMC\)do editor de gerenciamento de política de grupo Microsoft.  

Este guia fornece instruções detalhadas sobre como especificar configurações na extensão de políticas de \(rede sem\) fio IEEE 802,11 do gerenciamento de política de grupo. As políticas IEEE \(802,11\) de rede sem fio\-configuram computadores cliente sem fio do membro do domínio com a conectividade necessária e as configurações sem fio para acesso sem fio autenticado 802.1 x.

### <a name="server-certificates"></a>Certificados do servidor
Esse cenário de implantação requer certificados de servidor para cada NPS que executa a autenticação 802.1 X.  

Um certificado de servidor é um documento digital que é comumente usado para autenticação e para proteger informações em redes abertas. O certificado liga de forma segura uma chave pública à entidade que mantém a chave privada correspondente. Os certificados são assinados digitalmente pela AC emissora e podem ser emitidos para um usuário, um computador ou um serviço.  

\(Uma CA\) de autoridade de certificação é uma entidade responsável por estabelecer e comprovar a autenticidade de chaves públicas que pertencem \(a assuntos geralmente usuários\) ou computadores ou outras CAS. As atividades de uma autoridade de certificação podem incluir a associação de chaves públicas a nomes distintos por meio de certificados assinados, gerenciamento de números de série de certificado e revogação de certificados.  

Active Directory serviços \(de certificados ad\) cs é uma função de servidor que emite certificados como uma AC de rede. Uma infraestrutura de certificado do AD CS, também conhecida como *PKI \(\)de infraestrutura de chave pública*, fornece serviços personalizáveis para emitir e gerenciar certificados para a empresa.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP, PEAP e PEAP\-MS\-CHAP v2
O \(EAP\) \- \(\-do protocolo de autenticação extensível estende o protocolo PPP ponto a ponto permitindo métodos de autenticação adicionais que usam credenciais e informações\) trocas de comprimentos arbitrários. Com a autenticação EAP, o cliente de acesso à rede e \(o autenticador,\) como o NPS, devem dar suporte ao mesmo tipo de EAP para que a autenticação bem-sucedida ocorra. O Windows Server 2016 inclui uma infraestrutura EAP, dá suporte a dois tipos de EAP e a capacidade de passar mensagens EAP para NPSs. Usando o EAP, você pode dar suporte a esquemas de autenticação adicionais, conhecidos como *tipos de EAP*. Os tipos de EAP com suporte no Windows Server 2016 são:  

- TLS de segurança \(da camada de transporte\)

- Protocolo de autenticação de handshake de desafio da \(Microsoft\-versão 2 MS CHAP v2\)

>[!IMPORTANT]
>Tipos \(de EAP fortes, como aqueles baseados em certificados\) , oferecem melhor segurança contra ataques\-de força bruta, ataques de dicionário e ataques de adivinhação\-de senha do que com base em senha protocolos \(de autenticação como CHAP ou MS\-CHAP versão 1\).

O EAP \(protegido\) PEAP usa o TLS para criar um canal criptografado entre um cliente PEAP de autenticação, como um computador sem fio e um autenticador PEAP, como um NPS ou outros servidores RADIUS. O PEAP não especifica um método de autenticação, mas fornece segurança adicional para outros protocolos \(de autenticação EAP, como EAP\-\-MS CHAP\) v2 que podem operar por meio do canal criptografado TLS fornecido pelo PEAP. O PEAP é usado como um método de autenticação para clientes de acesso que estão se conectando à rede da sua organização por meio dos \(seguintes\)tipos de servidores de acesso à rede Nass:

- pontos de\-acesso sem fio compatíveis com 802.1 x

- comutadores\-de autenticação compatíveis com 802.1 x

- Computadores que executam o Windows Server 2016 e o \(RAS\) do serviço de acesso remoto que são \(configurados como servidores VPN\) de rede virtual privada, servidores DirectAccess ou ambos

- Computadores que executam o Windows Server 2016 e Serviços de Área de Trabalho Remota

O\-PEAP\-MS CHAP v2 é mais fácil de implantar\-do que o EAP TLS porque a autenticação do\-usuário é \(executada usando credenciais baseadas\)em senha nome de usuário e senha, em vez de certificados ou cartões inteligentes. Somente o NPS ou outros servidores RADIUS devem ter um certificado. O certificado NPS é usado pelo NPS durante o processo de autenticação para provar sua identidade para clientes PEAP.

Este guia fornece instruções para configurar seus clientes sem fio e seus\(s\) do NPS para\-usar\-o PEAP MS CHAP v2 para acesso autenticado 802.1 x.

### <a name="network-policy-server"></a>Servidor de Políticas de Rede
\(O NPS\- \(\) do servidor de políticas de rede permite que você configure e gerencie centralmente as políticas de rede usando a autenticação remota dial no servidor RADIUS do serviço do usuário e o proxy RADIUS.\) O NPS é necessário quando você implanta o acesso sem fio 802.1 X.

Quando você configura seus pontos de acesso sem fio 802.1 X como clientes RADIUS no NPS, o NPS processa as solicitações de conexão enviadas pelo APs. Durante o processamento da solicitação de conexão, o NPS executa autenticação e autorização. A autenticação determina se o cliente apresentou credenciais válidas. Se o NPS autenticar com êxito o cliente solicitante, o NPS determina se o cliente está autorizado a fazer a conexão solicitada e permite ou nega a conexão. Isso é explicado com mais detalhes da seguinte maneira:

#### <a name="authentication"></a>Autenticação

A autenticação de\-PEAP\-do MS CHAP V2 com êxito tem duas partes principais:

1.  O cliente autentica o NPS.  Durante essa fase de autenticação mútua, o NPS envia seu certificado de servidor ao computador cliente para que o cliente possa verificar a identidade do NPS com o certificado. Para autenticar o NPS com êxito, o computador cliente deve confiar na autoridade de certificação que emitiu o certificado NPS. O cliente confia nessa autoridade de certificação quando o certificado da autoridade de certificação está presente no repositório de certificados das autoridades de certificação raiz confiáveis no computador cliente.

    Se você implantar sua própria autoridade de certificação privada, o certificado de autoridade de certificação será instalado automaticamente no repositório de certificados das autoridades de certificação raiz confiáveis para o usuário atual e para o computador local quando Política de Grupo for atualizado no computador cliente membro do domínio. Se você decidir implantar certificados de servidor de uma AC pública, verifique se o certificado de autoridade de certificação pública já está no repositório de certificados de autoridades de certificação raiz confiáveis.  

2.  O NPS autentica o usuário. Depois que o cliente autenticar o NPS com êxito, o cliente envia as credenciais baseadas\-na senha do usuário para o NPS, que verifica as credenciais do usuário no banco de dados de contas de usuário \(no Active Directory serviços domínio AD DS\).

Se as credenciais forem válidas e a autenticação for bem sucedido, o NPS iniciará a fase de autorização do processamento da solicitação de conexão. Se as credenciais não forem válidas e a autenticação falhar, o NPS enviará uma mensagem de rejeição de acesso e a solicitação de conexão será negada.  

#### <a name="authorization"></a>Autorização

O servidor que executa o NPS executa a autorização da seguinte maneira:  

1. O NPS verifica as restrições nas propriedades de discagem\-da conta do usuário ou do computador em AD DS. Cada usuário e conta de computador em Active Directory usuários e computadores inclui várias propriedades, incluindo aquelas encontradas na **guia\-discar** . Nessa guia, em **permissão de acesso à rede**, se o valor for **permitir acesso**, o usuário ou o computador será autorizado a se conectar à rede. Se o valor for **negar acesso**, o usuário ou o computador não estará autorizado a se conectar à rede. Se o valor for **controlar o acesso por meio da política de rede do NPS**, o NPS avaliará as políticas de rede configuradas para determinar se o usuário ou o computador está autorizado a se conectar à rede. 

2. Em seguida, o NPS processa suas políticas de rede para encontrar uma política que corresponda à solicitação de conexão. Se uma política de correspondência for encontrada, o NPS concederá ou negará a conexão com base na configuração dessa política.  

Se a autenticação e a autorização forem bem-sucedidas e se a política de rede correspondente conceder acesso, o NPS concederá acesso à rede e o usuário e o computador poderão se conectar aos recursos de rede para os quais eles têm permissões.  

>[!NOTE]
>Para implantar o acesso sem fio, você deve configurar políticas de NPS. Este guia fornece instruções para usar o **Assistente para configurar 802.1 x** no NPS para criar políticas de NPS para acesso sem fio autenticado 802.1 x.  

### <a name="bootstrap-profiles"></a>Perfis de Bootstrap
Em redes sem\-fio autenticadas 802.1 x, os clientes sem fio devem fornecer credenciais de segurança que são autenticadas por um servidor RADIUS para se conectar à rede. Para \[EAP \[\-\]protegido Microsoft Challenge Handshake Authentication Protocol versão 2 MS CHAP v2, as credenciais de segurança são um nome de usuário e uma senha.\]\- Para TLS\-\] ou PEAP \[TLSdesegurançadacamadadetransporteEAP,ascredenciaisdesegurançasãocertificados,comousuáriosclienteecertificadosdecomputadoroucartõesinteligentes.\-

Ao se conectar a uma rede configurada para executar\-a\-autenticação PEAP MS CHAP\-v2, PEAP TLS\-ou EAP TLS, por padrão, os clientes sem fio do Windows também devem validar um certificado de computador que seja enviado pelo servidor RADIUS. O certificado do computador enviado pelo servidor RADIUS para cada sessão de autenticação é conhecido como um certificado do servidor.

Conforme mencionado anteriormente, você pode emitir seus servidores RADIUS com seu certificado de servidor de uma das duas maneiras: de uma \(AC comercial, como a Verisign,\)Inc., ou de uma AC privada que você implanta em sua rede. Se o servidor RADIUS envia um certificado de computador que foi emitido por uma AC comercial que já tem um certificado raiz instalado no repositório de certificados de autoridades de certificação raiz confiáveis do cliente, o cliente sem fio pode validar o servidor RADIUS certificado de computador, independentemente de o cliente sem fio ter ingressado no domínio de Active Directory. Nesse caso, o cliente sem fio pode se conectar à rede sem fio e, em seguida, você pode ingressar o computador no domínio.

>[!NOTE]
>O comportamento que exige que o cliente valide o certificado do servidor pode ser desabilitado, mas desabilitar a validação do certificado do servidor não é recomendado em ambientes de produção.

Os perfis de Bootstrap sem fio são perfis temporários configurados de forma a permitir que os usuários de cliente sem fio se\-conectem à rede sem fio autenticada 802.1 x antes que o computador\/seja ingressado no domínio e ou antes o usuário fez logon com êxito no domínio usando um determinado computador sem fio pela primeira vez.  Esta seção resume Qual problema é encontrado ao tentar ingressar um computador sem fio no domínio ou para que um usuário use um computador sem fio\-ingressado no domínio pela primeira vez para fazer logon no domínio.

Para implantações em que o usuário ou o administrador de ti não pode conectar fisicamente um computador à rede Ethernet com fio para ingressar o computador no domínio e o computador não tem o certificado de AC raiz de emissão necessário instalado em sua **raiz confiável** Repositório de certificados das autoridades de certificação, você pode configurar clientes sem fio com um perfil de conexão sem fio temporário, chamado *perfil de Bootstrap*, para se conectar à rede sem fio.

Um *perfil de inicialização* remove o requisito para validar o certificado de computador do servidor RADIUS. Essa configuração temporária permite que o usuário sem fio Ingresse o computador no domínio, quando as políticas IEEE 802,11 \(\) de rede sem fio são aplicadas e o certificado de autoridade de certificação raiz apropriado é instalado automaticamente no Ele.

Quando Política de Grupo é aplicado, um ou mais perfis de conexão sem fio que impõem o requisito de autenticação mútua são aplicados no computador; o perfil Bootstrap não é mais necessário e é removido. Depois de ingressar o computador no domínio e reiniciar o computador, o usuário poderá usar uma conexão sem fio para fazer logon no domínio.

Para obter uma visão geral do processo de implantação de acesso sem fio usando essas tecnologias, consulte [visão geral da implantação de acesso sem fio](b-wireless-access-deploy-overview.md).
