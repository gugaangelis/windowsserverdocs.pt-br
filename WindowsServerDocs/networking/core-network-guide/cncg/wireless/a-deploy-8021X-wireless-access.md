---
title: Implantar acesso sem fio autenticado 802.1X baseado em senha
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseado em senha 802.1 X acesso autenticado sem fio"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f34580f4a0fd92c6f059d19d09a6926fc2775960
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840007"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Implantar a senha\-com base em acesso sem fio autenticado 802.1 X

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este é um guia complementar para o Windows Server&reg; 2016 guia da rede principal. Guia da rede principal fornece instruções para planejar e implantar os componentes necessários para uma rede totalmente operacional e um novo domínio de Active Directory® em uma nova floresta.

Este guia explica como criar uma rede principal, fornecendo instruções sobre como implantar o Institute of Electrical and Electronics Engineers \(IEEE\) 802.1 X\-usando o acesso sem fio autenticado IEEE 802.11 Protected Extensible Authentication Protocol – Microsoft Challenge Handshake Authentication Protocol versão 2 \(PEAP\-MS\-CHAP v2\).

Como PEAP\-MS\-CHAP v2 requer que os usuários forneçam senha\-credenciais com base em vez de um certificado durante o processo de autenticação, ele é normalmente mais fácil e econômico de implantar que o EAP\-TLS ou PEAP\-TLS.

>[!NOTE]
>Neste guia, IEEE 802.1 X acesso autenticado sem fio com o PEAP\-MS\-CHAP v2 é abreviado como "acesso sem fio" e "Acesso de Wi-Fi".

## <a name="about-this-guide"></a>Sobre este guia
Neste guia, em combinação com os guias de pré-requisito descrita abaixo, fornece instruções sobre como implantar a seguinte infraestrutura de acesso Wi-Fi.  

- Um ou mais 802.1X\-compatíveis com pontos de acesso sem fio 802.11 \(APs\).

- Serviços de domínio do Active Directory \(AD DS\) usuários e computadores.

- Gerenciamento de Política de Grupo.

- Um ou mais servidores de diretiva de rede \(NPS\) servidores.

- Certificados de servidor para computadores que executam o NPS.

- Computadores cliente executando o Windows sem fio&reg; 10, Windows 8.1 ou Windows 8.

### <a name="dependencies-for-this-guide"></a>Dependências para este guia

Para implantar com êxito sem fio autenticado com este guia, você deve ter um ambiente de rede e domínio com todas as tecnologias necessárias implantadas. Você também deve ter certificados de servidor implantados em NPSs sua autenticação.

As seções a seguir fornecem links para documentação que mostra como implantar essas tecnologias.

#### <a name="network-and-domain-environment-dependencies"></a>Dependências do ambiente de rede e domínio

Este guia foi projetado para administradores de sistema e de rede que tenham seguido as instruções do Windows Server 2016 **guia da rede principal** para implantar uma rede principal, ou para aqueles que já implantou tecnologias principais incluído na rede principal, incluindo o AD DS, o sistema de nomes de domínio \(DNS\), Dynamic Host Configuration Protocol \(DHCP\), TCP\/IP, o NPS e o Windows Internet Name Service \(WINS\).  

Guia da rede principal está disponível nos seguintes locais:

- O Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponível na biblioteca técnica do Windows Server 2016. 

- O [guia da rede principal](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) também está disponível no formato Word na Galeria do TechNet, em [ https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683 ](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dependências de certificado do servidor
Há duas opções disponíveis para o registro de servidores de autenticação com certificados de servidor para uso com a autenticação 802.1X - implantar sua própria infraestrutura de chave pública usando serviços de certificados do Active Directory \(AD CS\) ou usar certificados de servidor que são registrados por uma autoridade de certificação pública \(autoridade de certificação\).

##### <a name="ad-cs"></a>AD CS
Os administradores de rede e sistema de implantação sem fio autenticado devem seguir as instruções no Windows Server 2016 Core guia complementar da rede, **implantar certificados de servidor para 802.1 X com fio e implantações sem fio**. Este guia explica como implantar e usar o AD CS para registrar automaticamente certificados de servidor para computadores que executam o NPS.

Este guia está disponível no seguinte local.

- O Windows Server 2016 Core Network Companion Guide [implantar certificados de servidor para 802.1 X com fio e implantações sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) em formato HTML na biblioteca técnica. 

##### <a name="public-ca"></a>CA Pública
Você pode comprar certificados de servidor de uma CA pública, como a VeriSign, computadores cliente já confiam. 

Um computador cliente confia em uma autoridade de certificação quando o certificado de autoridade de certificação está instalado no repositório de certificados de autoridades de certificação raiz confiáveis. Por padrão, a computadores que executam o Windows tem vários certificados de autoridade de certificação públicos instalados em seus certificados de autoridades de certificação raiz confiáveis a armazenar.  

É recomendável que você examine os guias de design e implantação de cada uma das tecnologias usadas neste cenário de implantação. Esses guias podem ajudar a determinar se o cenário de implantação fornece os serviços e as configurações que você precisa para a rede de sua organização.

### <a name="requirements"></a>Requisitos
A seguir estão os requisitos para implantar uma infraestrutura de acesso sem fio usando o cenário documentado neste guia:

- Antes de implantar este cenário, você deve primeiro comprar 802.1 X\-pontos de acesso sem fio com capacidade de fornecer cobertura sem fio nos locais desejados em seu site. A seção de planejamento deste guia ajuda a determinar os recursos que devem dar suporte a seus PAS.

- Serviços de domínio do Active Directory \(AD DS\) estiver instalado, assim como outras tecnologias de rede necessária, acordo com as instruções no guia de rede do Windows Server 2016 Core.  

- AD CS é implantado e certificados de servidor são registrados em NPSs. Esses certificados são necessários quando você implanta o PEAP\-MS\-CHAP v2 certificado\-com base em método de autenticação que é usado neste guia.

- Um membro da sua organização é familiarizado com os padrões do IEEE 802.11 que são compatíveis com seus PAS sem fio e adaptadores de rede sem fio que estão instalados no computadores cliente e dispositivos na sua rede. Por exemplo, alguém em sua organização está familiarizado com os tipos de radiofrequência, autenticação sem fio 802.11 \(WPA2 ou WPA\)e as codificações \(AES ou TKIP\).

## <a name="what-this-guide-does-not-provide"></a>O que este guia não contém  
A seguir estão alguns itens que neste guia não fornece:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Orientações abrangentes para a seleção de 802.1 X\-pontos de acesso sem fio compatíveis com

Porque existem muitas diferenças entre marcas e modelos de 802.1 X\-APs sem fio com capacidade, este guia não fornece informações detalhadas sobre:  

- Determinar qual marca ou o modelo do AP sem fio é melhor adequada às suas necessidades.

- A implantação física de APs sem fio na sua rede.

- Avançadas de configuração de ponto de acesso sem fio, como para virtual redes locais sem fio \(VLANs\).

- Instruções sobre como configurar um fornecedor de AP sem fio\-atributos específicos no NPS.

Além disso, terminologia e nomes para as configurações variam entre modelos e marcas de AP sem fio e podem não coincidir com os nomes de configuração genérica que são usados neste guia. Para detalhes de configuração de ponto de acesso sem fio, você deve examinar a documentação do produto fornecida pelo fabricante do APs sem fio.

### <a name="instructions-for-deploying-nps-certificates"></a>Instruções para implantar certificados NPS
  
Há duas alternativas para a implantação de certificados do NPS. Este guia fornece orientações abrangentes para ajudá-lo a determinar qual alternativa melhor atenderá às suas necessidades. Em geral, no entanto, as opções que você enfrenta são:

- Comprar certificados de uma CA pública, como a VeriSign, que já são confiáveis pelo Windows\-com base em clientes. Normalmente, essa opção é recomendada para redes menores.

- Implantar uma infraestrutura de chave pública \(PKI\) em sua rede usando o AD CS. Isso é recomendado para a maioria das redes e as instruções de como implantar certificados de servidor com o AD CS estão disponíveis no guia de implantação mencionadas anteriormente.

### <a name="nps-network-policies-and-other-nps-settings"></a>Políticas de rede do NPS e outras configurações do NPS

Exceto para as definições de configuração feitas quando você executa o **configurar 802.1 X** assistente, conforme documentado neste guia, este guia não fornece informações detalhadas para configurar manualmente o NPS condições, restrições ou outro NPS Configurações.

### <a name="dhcp"></a>DHCP

Este guia de implantação não fornece informações sobre como criar ou implantar sub-redes DHCP para LANs sem fio.

## <a name="technology-overviews"></a>Visões gerais da tecnologia
A seguir estão as visões gerais de tecnologia para a implantação de acesso sem fio:

### <a name="ieee-8021x"></a>IEEE 802.1X
O padrão IEEE 802.1X define a porta\-com base em controle de acesso à rede que é usado para fornecer acesso autenticado à rede para redes Ethernet. Essa porta\-controle de acesso de rede com base em usa as características físicas da infraestrutura de LAN alternada para autenticar dispositivos conectados a uma porta LAN. O acesso à porta pode ser negado se o processo de autenticação falhar. Embora esse padrão foi projetado para redes Ethernet com fio, ele foi adaptado para uso em LANs sem fio 802.11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\-pontos de acesso sem fio compatíveis com \(APs\)
Esse cenário requer a implantação de um ou mais 802.1X\-APs sem fio com capacidade que são compatíveis com o Remote Authentication Dial\-In User Service \(RADIUS\) protocolo.

802.1X e RADIUS\-compatível com pontos de acesso, quando implantado em uma infraestrutura RADIUS, com um servidor RADIUS como um NPS, são chamados *clientes RADIUS*.

### <a name="wireless-clients"></a>Clientes sem fio
Este guia fornece detalhes de configuração abrangente para fornecer acesso autenticado para o domínio 802.1 X\-usuários de membro que se conectam à rede com computadores de cliente sem fio que executam o Windows 10, Windows 8.1 e Windows 8. Os computadores devem ser integrados ao domínio para estabelecer o acesso autenticado com êxito.

>[!NOTE]
>Você também pode usar computadores que executam o Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 como clientes sem fio.

### <a name="support-for-ieee-80211-standards"></a>Suporte para IEEE 802.11 padrões
Sistemas operacionais Windows e Windows Server com suporte que fornecem criado\-no suporte para 802.11 sem fio à rede. Nesses sistemas operacionais, um adaptador de rede sem fio 802.11 instalado aparece como uma conexão de rede sem fio na Central de rede e compartilhamento. 

Embora há baseia-se\-com suporte para redes sem fio 802.11, os componentes sem fio do Windows variam de acordo com o seguinte:

- Os recursos do adaptador de rede sem fio. O adaptador de rede sem fio instalado deve dar suporte a LAN sem fio ou padrões de segurança sem fio que você precisa. Por exemplo, se o adaptador de rede sem fio não dá suporte a Wi\-Fi Protected Access \(WPA\), você não pode habilitar ou configurar opções de segurança WPA.

- Os recursos do driver do adaptador de rede sem fio. Para permitir que você configure as opções de rede sem fio, o driver do adaptador de rede sem fio deve oferecer suporte a relatórios de todos os seus recursos para Windows. Verifique se que o driver de adaptador de rede sem fio é escrito para os recursos do seu sistema operacional. Certifique-se também de que o driver é a versão mais atual, verificando o Microsoft Update ou o site do fornecedor de adaptador de rede sem fio.

A tabela a seguir mostra as taxas de transmissão e as frequências de padrões comuns de sem fio IEEE 802.11.

|Padrões|Frequências|Transmissão de taxas de bits|Uso|
|-------------|---------------|--------------------------|---------|  
|802.11|S\-banda Industrial, científico e Medical \(ISM\) intervalo de frequência \(2.4 a 2,5 GHz\)|2 megabits por segundo \(Mbps\)|Obsoleto. Não são comumente usados.|  
|802.11b|S\-banda ISM|11 Mbps|Normalmente usado.|  
|802.11 a|C\-banda ISM \(5,725 a 5.875 GHz\)|54 Mbps|Não são usados normalmente devido à despesa e intervalo limitado.|  
|802.11g|S\-banda ISM|54 Mbps|Amplamente usado. dispositivos 802.11g são compatíveis com 802.11b dispositivos.|  
|802.11n \2.4 e 5.0 GHz|C\-S e banda\-banda ISM|250 Mbps|Dispositivos com base no pré\-ratificação IEEE 802.11 n padrão se tornou disponível em agosto de 2007. 802.11n muitos dispositivos são compatíveis com 802.11 a, b e dispositivos g.|
|802.11ac |5 GHz |6.93 Gbps |o 802.11ac, aprovado pelo IEEE em 2014, é mais escalonável e mais rápido do que 802.11n e é implantado onde APs e clientes sem fio oferecer suporte a ele.|

### <a name="wireless-network-security-methods"></a>Métodos de segurança de rede sem fio
**Sem fio de métodos de segurança de rede** é um agrupamento informal de autenticação sem fio \(às vezes chamado de segurança sem fio\) e criptografia de segurança sem fio. Criptografia e autenticação sem fio são usadas em pares para impedir que usuários não autorizados acessem a rede sem fio e para proteger as transmissões sem fio. 

Ao definir as configurações de segurança sem fio sem fio rede diretivas de diretiva de grupo, há várias combinações para sua escolha. No entanto, somente o WPA2\-Enterprise, WPA\-Enterprise e abrir com 802.1X padrões de autenticação têm suporte para implantações sem fio 802.1 X autenticado.

>[!NOTE]
>Ao configurar políticas de rede sem fio, você deve selecionar **WPA2\-Enterprise**, **WPA\-Enterprise**, ou **abrir com 802.1X** em a fim de acessar as configurações de EAP que são necessários para 802.1 X autenticado implantações sem fio.  

#### <a name="wireless-authentication"></a>Autenticação sem fio
Este guia recomenda que o uso dos seguintes padrões de autenticação sem fio para 802.1 X autenticado implantações sem fio.  

**Wi\-Fi Protected Access – Enterprise \(WPA\-Enterprise\)**  WPA é um padrão provisório desenvolvido pela Wi-Fi Alliance para estar em conformidade com o protocolo de segurança sem fio 802.11. O protocolo WPA foi desenvolvido em resposta a um número de falhas graves que foram descobertos no anterior Wired Equivalent Privacy \(WEP\) protocolo.

WPA\-Enterprise fornece segurança aprimorada pelo WEP por:  

1. Exigir autenticação que usa a estrutura X EAP 802.1 como parte da infraestrutura que garante que a autenticação mútua centralizada e gerenciamento de chaves dinâmico  

2. Aumentar o valor de verificação de integridade \(ICV\) com uma mensagem de verificação de integridade \(MIC\), para proteger o cabeçalho e a carga  

3. Implementando um contador de quadros para evitar ataques de reprodução  

**Wi\-Fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)**  como o WPA\-Enterprise, standard, WPA2\-Enterprise usa 802.1 X e o framework EAP. WPA2\-Enterprise fornece maior proteção de dados para vários usuários e grandes redes de gerenciadas. WPA2\-Enterprise é um protocolo robusto que foi desenvolvido para impedir o acesso à rede não autorizado, verificando os usuários da rede por meio de um servidor de autenticação.  

#### <a name="wireless-security-encryption"></a>Criptografia de segurança sem fio
Criptografia de segurança sem fio é usada para proteger as transmissões sem fio que são enviadas entre o cliente sem fio e o AP sem fio. Criptografia de segurança sem fio é usada em conjunto com o método de autenticação de segurança de rede selecionada. Por padrão, os computadores que executam o Windows 8, Windows 8.1 e Windows 10 dão suporte a dois padrões de criptografia:

1. **Temporal Key Integrity Protocol** \(TKIP\) é um protocolo de criptografia mais antigo que foi originalmente projetado para fornecer uma criptografia mais segura sem fio que foi fornecido pelo inerentemente fraca Wired Equivalent Privacy \(WEP\) protocolo. TKIP foi criado pelo IEEE 802.11 i tarefas grupo e o Wi\-Fi Alliance para substituir o WEP sem a necessidade de substituição de hardware herdado. O TKIP é um pacote de algoritmos que encapsula a carga do WEP e permite que os usuários do equipamento legado de Wi-Fi atualizar para TKIP sem substituir o hardware. Como o WEP, TKIP usa o algoritmo de criptografia de fluxo RC4 como sua base. O novo protocolo, no entanto, criptografa cada pacote de dados com uma chave de criptografia exclusivo e as chaves são muito mais seguras do que aqueles pelo WEP. Embora TKIP é útil para a atualização de segurança em dispositivos mais antigos que foram projetados para usar somente o WEP, ele não aborda todos os problemas de segurança voltados para LANs sem fio e na maioria dos casos não é suficientemente robusto para proteger o governo confidencial ou dados corporativos transmissões.  

2. **Advanced Encryption Standard** \(AES\) é o protocolo de criptografia preferencial para a criptografia de dados comerciais e governamentais. AES oferece um nível mais alto de segurança de transmissão sem fio que TKIP ou WEP. Ao contrário de TKIP e WEP, AES requer hardware sem fio que suporta o padrão AES. AES é simétrica\-chave de criptografia padrão que usa três codificações de bloco, AES\-128, AES\-192 e AES\-256.

No Windows Server 2016, o seguinte AES\-métodos de criptografia sem fio com base em estão disponíveis para configuração nas propriedades de perfil sem fio quando você seleciona um método de autenticação do WPA2\-Enterprise, que é recomendado.

1. **AES\-CCMP**. Modo de codificação de bloco encadeamento mensagem código protocolo de autenticação de contador \(CCMP\) implementa o padrão 802.11 i e foi projetado para criptografia de segurança mais alto do que o fornecido pelo WEP e usa chaves de criptografia AES de 128 bits.
2. **AES\-GCMP**. Protocolo de modo de contador Galois \(GCMP\) é compatível com 802.11ac, é mais eficiente do que AES\-CCMP e oferece melhor desempenho para os clientes sem fio. GCMP usa chaves de criptografia AES de 256 bits.

> [!IMPORTANT]
> Com fio Equivalency Privacy \(WEP\) era o padrão de segurança sem fio original que foi usado para criptografar o tráfego de rede. Você não deve implantar WEP em sua rede porque há bem\-vulnerabilidades conhecidas neste formulário desatualizado de segurança.

### <a name="active-directorydoman-services-adds"></a>Serviços de domínio do Active Directory \(AD DS\)
O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos de rede e aplicativo\-dados específicos do diretório\-aplicativos habilitados. Os administradores podem usar AD DS para organizar elementos de uma rede, como usuários, computadores e outros dispositivos, em uma estrutura de confinamento hierárquica. A estrutura de confinamento hierárquica inclui a floresta do Active Directory, domínios na floresta e unidades organizacionais \(UOs\) em cada domínio. Um servidor que está executando o AD DS é chamado de um *controlador de domínio*.  

AD DS contém as contas de usuário, contas de computador e propriedades da conta que são exigidas pelo IEEE 802.1 X e PEAP\-MS\-CHAP v2 para autenticar as credenciais do usuário e para avaliar a autorização para conexões sem fio.

### <a name="active-directory-users-and-computers"></a>Usuários e computadores do Active Directory
Active Directory Users and Computers é um componente do AD DS que contém as contas que representam entidades físicas, como um computador, uma pessoa ou um grupo de segurança. Um *grupo de segurança* é uma coleção de contas de usuário ou computador que os administradores podem gerenciar como uma única unidade. Contas de usuário e computador que pertencem a um determinado grupo são denominadas *os membros do grupo*.  

### <a name="group-policy-management"></a>Gerenciamento de Política de Grupo
Gerenciamento de diretiva de grupo permite que o diretório\-com base em gerenciamento de configuração e alteração de configurações de usuário e computador, incluindo informações de segurança e do usuário. Usar política de grupo para definir configurações para grupos de usuários e computadores. Com a diretiva de grupo, você pode especificar configurações para as entradas do registro, segurança, instalação de software, scripts, redirecionamento de pasta, serviços de instalação remota e manutenção do Internet Explorer. As configurações de diretiva de grupo que você cria são contidas em um objeto de diretiva de grupo \(GPO\). Associando um GPO selecionados contêineres de sistema do Active Directory — sites, domínios e OUs — você pode aplicar as configurações do GPO aos usuários e computadores nesses contêineres do Active Directory. Para gerenciar objetos de diretiva de grupo em toda a empresa, você pode usar o Editor de gerenciamento de diretiva de grupo Microsoft Management Console \(MMC\).  

Este guia fornece instruções detalhadas sobre como especificar as configurações da rede sem fio \(IEEE 802.11\) extensão de políticas do gerenciamento de diretiva de grupo. A rede sem fio \(IEEE 802.11\) diretivas configurar domínio\-computadores cliente sem fio de membro com a conectividade necessária e configurações sem fio 802.1 X autenticado acesso sem fio.

### <a name="server-certificates"></a>Certificados do servidor
Neste cenário de implantação requer certificados de servidor para cada NPS que realiza a autenticação 802.1X.  

Um certificado de servidor é um documento digital geralmente usado para autenticação e proteção de informações em redes abertas. O certificado liga de forma segura uma chave pública à entidade que mantém a chave privada correspondente. Os certificados são assinados digitalmente pela autoridade de certificação emissora e podem ser emitidos para um usuário, um computador ou um serviço.  

Uma autoridade de certificação \(autoridade de certificação\) é uma entidade responsável por estabelecer e atestar a autenticidade das chaves públicas que pertencem a sujeitos \(geral usuários ou computadores\) ou outras autoridades de certificação. Atividades de uma autoridade de certificação podem incluir a vinculação de chaves públicas a nomes destacados através de certificados assinados, números de série do certificado de gerenciamento e revogação de certificados.  

Serviços de certificados do Active Directory \(AD CS\) é uma função de servidor que emite certificados como uma AC de rede. Um AD CS certificado de infra-estrutura, também conhecido como um *infraestrutura de chave pública \(PKI\)*, fornecem serviços personalizáveis para emissão e gerenciamento de certificados para a empresa.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP, PEAP e PEAP\-MS\-CHAP v2
Extensible Authentication Protocol \(EAP\) estende ponto\-ao\-protocolo ponto \(PPP\) , permitindo que os métodos de autenticação adicionais que usam credenciais e informações trocas de comprimentos arbitrários. Com a autenticação do EAP, os dois a acesso à rede cliente e o autenticador \(como o NPS\) deve oferecer suporte para o mesmo tipo EAP para autenticação bem-sucedida deve ocorrer. Windows Server 2016 inclui uma infra-estrutura do EAP, dá suporte a dois tipos de EAP e a capacidade de passar mensagens EAP para NPSs. Usando o EAP, você pode dar suporte a esquemas de autenticação adicionais, conhecidos como *tipos de EAP*. Os tipos EAP que são compatíveis com o Windows Server 2016 são:  

- Transport Layer Security \(TLS\)

- Microsoft Challenge Handshake Authentication Protocol versão 2 \(MS\-CHAP v2\)

>[!IMPORTANT]
>Os tipos EAP robustos \(, como aqueles que são baseadas em certificados\) oferecem mais segurança contra bruta\-forçar ataques, ataques de dicionário e ataques de senha de adivinhação de senha\-com base em protocolos de autenticação \(como o protocolo CHAP ou MS\-CHAP versão 1\).

EAP protegido \(PEAP\) usa TLS para criar um canal criptografado entre um cliente PEAP de autenticação, como um computador sem fio e um autenticador PEAP, como um NPS ou outros servidores RADIUS. PEAP não especifica um método de autenticação, mas ele fornece segurança adicional para outros protocolos de autenticação EAP \(, como o EAP\-MS\-CHAP v2\) que podem operar por meio do canal criptografado TLS fornecido pelo PEAP. PEAP é usado como um método de autenticação para clientes de acesso que estão se conectando à rede da sua organização por meio dos seguintes tipos de servidores de acesso de rede \(NASs\):

- 802.1 X\-pontos de acesso sem fio compatíveis com

- 802.1 X\-compatíveis com chaves de autenticação

- Computadores que executam o Windows Server 2016 e o serviço de acesso remoto \(RAS\) que são configurados como rede virtual privada \(VPN\) servidores, servidores do DirectAccess ou ambos

- Computadores que executam o Windows Server 2016 e os serviços de área de trabalho remota

PEAP\-MS\-CHAP v2 é mais fácil de implantar que EAP\-TLS porque a autenticação de usuário é realizada por meio de senha\-com base em credenciais \(nome de usuário e senha\), em vez de os certificados ou cartões inteligentes. Somente o NPS ou outros servidores RADIUS devem ter um certificado. O certificado do NPS é usado pelo NPS durante o processo de autenticação para provar sua identidade para os clientes PEAP.

Este guia fornece instruções para configurar seus clientes sem fio e o NPS\(s\) usar PEAP\-MS\-CHAP v2 para 802.1 X autenticado acesso.

### <a name="network-policy-server"></a>Servidor de Políticas de Rede
Servidor de diretivas de rede \(NPS\) lhe permite configurar e gerenciar políticas de rede usando o Remote Authentication Dial centralmente\-In User Service \(RADIUS\) servidor e proxy RADIUS. O NPS é necessário quando você implanta o acesso sem fio 802.1 X.

Quando você configura seus pontos de acesso sem fio 802.1 X como clientes RADIUS no NPS, o NPS processa as solicitações de conexão enviadas pelos pontos de acesso. Durante o processamento de solicitação de conexão, o NPS executa autenticação e autorização. Autenticação determina se o cliente tem apresentado credenciais válidas. Se o NPS autentica com êxito o cliente solicitante, o NPS determina se o cliente está autorizado para fazer a conexão solicitada, e permite ou nega a conexão. Isso é explicado em mais detalhes da seguinte maneira:

#### <a name="authentication"></a>Autenticação

PEAP mútuo bem-sucedida\-MS\-autenticação CHAP v2 tem duas partes principais:

1.  O cliente autentica o NPS.  Durante essa fase de autenticação mútua, o NPS envia seu certificado de servidor para o computador cliente para que o cliente possa verificar a identidade do NPS com o certificado. Para autenticar com êxito o NPS, o computador cliente deve confiar na AC que emitiu o certificado do NPS. O cliente confia essa autoridade de certificação quando o certificado da AC está presente no repositório de certificados de autoridades de certificação raiz confiáveis no computador cliente.

    Se você implantar sua própria autoridade de certificação privada, o certificado de autoridade de certificação é instalado automaticamente no repositório de certificados de autoridades de certificação raiz confiáveis para o usuário atual e para o computador Local quando a diretiva de grupo é atualizada no computador do cliente de membro de domínio. Se você optar por implantar certificados de servidor de uma CA pública, certifique-se de que o certificado de autoridade de certificação pública já está no repositório de certificados de autoridades de certificação raiz confiáveis.  

2.  O NPS autentica o usuário. Depois que o cliente autentica com êxito o NPS, o cliente envia a senha do usuário\-com base em credenciais para o NPS, que verifica as credenciais do usuário contra o banco de dados de contas de usuário nos serviços de domínio do Active Directory \(AD DS\).

Se as credenciais são válidas e a autenticação for bem-sucedida, o NPS começa a fase de autorização do processamento da solicitação de conexão. Se as credenciais não são válidas e a autenticação falhar, o NPS envia uma mensagem de acesso rejeitar e a solicitação de conexão será negada.  

#### <a name="authorization"></a>Autorização

O servidor que executa o NPS executa a autorização da seguinte maneira:  

1. O NPS compara para restrições de discagem de conta de usuário ou computador\-nas propriedades no AD DS. Cada conta de usuário e computador no Active Directory Users and Computers inclui várias propriedades, incluindo aqueles encontrados na **Dial\-em** guia. Nessa guia, na **permissão de acesso de rede**, se o valor estiver **permitir o acesso**, o computador ou usuário está autorizado a se conectar à rede. Se o valor for **negar acesso**, o usuário ou computador não está autorizado a se conectar à rede. Se o valor for **controlar o acesso por meio da diretiva de rede do NPS**, NPS avalia as políticas de rede configuradas para determinar se o computador ou usuário está autorizado a se conectar à rede. 

2. Em seguida, o NPS processa as diretivas de rede para localizar uma diretiva que corresponda à solicitação de conexão. Se uma política de correspondência for encontrada, o NPS concede ou nega a conexão com base na configuração da diretiva.  

Se a autenticação e autorização forem bem-sucedidas, e se a diretiva de rede correspondente concede acesso, NPS concede acesso à rede e o usuário e o computador podem se conectar aos recursos da rede para os quais eles têm permissões.  

>[!NOTE]
>Para implantar o acesso sem fio, você deve configurar as políticas de NPS. Este guia fornece instruções para usar o **configurar 802.1 X wizard** no NPS para criar políticas NPS para acesso sem fio do 802.1 X autenticado.  

### <a name="bootstrap-profiles"></a>Perfis de inicialização
No 802.1 X\-autenticado redes sem fio, os clientes sem fio devem fornecer credenciais de segurança que são autenticadas por um servidor RADIUS para se conectar à rede. Para EAP protegido \[PEAP\]\-Microsoft Challenge Handshake Authentication Protocol versão 2 \[MS\-CHAP v2\], as credenciais de segurança são um nome de usuário e senha. Para o EAP\-Transport Layer Security \[TLS\] ou PEAP\-TLS, as credenciais de segurança são certificados, como certificados de usuário e computador cliente ou cartões inteligentes.

Ao se conectar a uma rede que está configurada para executar o PEAP\-MS\-CHAP v2, PEAP\-TLS ou EAP\-autenticação TLS, por padrão, os clientes sem fio também devem validar um certificado de computador do Windows enviado pelo servidor RADIUS. O certificado do computador que é enviado pelo servidor RADIUS para cada sessão de autenticação é conhecido como um certificado de servidor.

Conforme mencionado anteriormente, você pode emitir os servidores RADIUS seu certificado de servidor em uma das duas maneiras: de uma CA comercial \(, como VeriSign, Inc.,\), ou de uma autoridade de certificação privada que são implantados na sua rede. Se o servidor RADIUS enviará um certificado de computador que foi emitido por uma CA comercial que já tenha um certificado raiz instalado no repositório de certificados de autoridades de certificação raiz confiáveis do cliente, o cliente sem fio pode validar o servidor RADIUS certificado de computador, independentemente se o cliente sem fio tenha ingressado no domínio do Active Directory. Nesse caso, o cliente sem fio pode se conectar à rede sem fio e, em seguida, você pode ingressar o computador ao domínio.

>[!NOTE]
>O comportamento que o cliente validar o certificado do servidor pode ser desabilitado, mas não é recomendável desabilitar a validação de certificado do servidor em ambientes de produção.

Os perfis sem fio de bootstrap são perfis temporários que são configurados de forma para permitir que os usuários do cliente sem fio para se conectar ao 802.1X\-autenticados rede sem fio, antes do computador tiver ingressado no domínio, e\/ou antes o usuário tem conectado com êxito domínio por meio de um determinado computador sem fio pela primeira vez.  Esta seção resume o problema é encontrado durante a tentativa de ingressar um computador sem fio no domínio ou um usuário poderá usar um domínio\-ingressado no computador sem fio pela primeira vez fazer logon no domínio.

Para implantações em que o usuário ou administrador de TI não é possível conectar fisicamente um computador à rede Ethernet com fio para ingressar o computador ao domínio e o computador não tem emissora necessário raiz do certificado de autoridade de certificação instalado no seu  **Autoridades de certificação raiz confiáveis** repositório de certificados, você pode configurar os clientes sem fio com um perfil de conexão sem fio temporário, chamado de um *inicializar perfil*, para se conectar à rede sem fio.

Um *inicializar perfil* remove a necessidade de validar o certificado de computador do servidor RADIUS. Essa configuração temporária permite que o usuário sem fio ingressar o computador ao domínio, momento em que de rede sem fio \(IEEE 802.11\) as políticas são aplicadas e o certificado de autoridade de certificação de raiz apropriado é instalado automaticamente no computador.

Quando a política de grupo é aplicada, um ou mais perfis de conexão sem fio que impõem o requisito para autenticação mútua são aplicados no computador. o perfil de inicialização não é mais necessário e é removido. Depois de ingressar o computador no domínio e reiniciar o computador, o usuário pode usar uma conexão sem fio para fazer logon no domínio.

Para uma visão geral do processo de implantação de acesso sem fio usando essas tecnologias, consulte [visão geral da implantação de acesso sem fio](b-wireless-access-deploy-overview.md).
