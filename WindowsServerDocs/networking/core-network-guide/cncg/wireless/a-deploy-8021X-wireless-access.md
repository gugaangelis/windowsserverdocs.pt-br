---
title: Implantar baseada em senha acesso autenticado 802.1 X sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseada em senha 802.1 X autenticado sem fio acesso"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ded8a273a9ad464b44fa7245db58d0fd05f06a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Implantar com base em Password\ acesso autenticado 802.1 X sem fio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este é um guia complementar para o Windows Server&reg; 2016 guia da rede principal. A guia da rede principal fornece instruções para planejar e implantar os componentes necessários para uma rede totalmente funcional e um novo domínio Active Directory® em uma nova floresta.

Este guia explica como criar mediante uma rede principal, fornecendo instruções sobre como implantar Institute of Electrical e engenheiros de aparelhos eletrônicos \(IEEE\) 802.1X\-acesso sem fio 802.11 IEEE usando Protected Extensible Authentication Protocol – Microsoft Challenge Handshake Authentication Protocol versão 2 autenticado \ (PEAP\-MS\-CHAP v2\).

Como PEAP\-MS\-CHAP v2 exige que os usuários forneçam credenciais baseadas em password\ em vez de um certificado durante o processo de autenticação, é geralmente mais fácil e mais econômica implantar que EAP\ TLS ou PEAP\-TLS.

>[!NOTE]
>Neste guia, IEEE 802.1 X acesso de acesso sem fio autenticado com PEAP\-MS\-CHAP v2 é abreviado para "acesso sem fio" e "Acesso Wi-Fi."

## <a name="about-this-guide"></a>Sobre este guia
Neste guia, em combinação com os guias de pré-requisito descrito abaixo, fornece instruções sobre como implantar a infraestrutura de acesso Wi-Fi a seguir.  

- Um ou mais 802.1X\-compatível com 802.11 acesso sem fio pontos \(APs\).

- Serviços de domínio do Active Directory \(AD DS\) usuários e computadores.

- Gerenciamento de política de grupo.

- Um ou mais servidores NPS \(NPS\).

- Certificados de servidor para computadores que executam o NPS.

- Sem fio computadores cliente que executam o Windows&reg; 10, Windows 8.1 ou Windows 8.

### <a name="dependencies-for-this-guide"></a>Dependências para este guia

Para implantar com êxito sem fio autenticado com este guia, você deve ter um ambiente de rede e domínio com todas as tecnologias necessárias implantadas. Você também deve ter implantados para seus servidores NPS autenticação de certificados do servidor.

As seções a seguir fornecem links para documentação que mostra como implantar essas tecnologias.

#### <a name="network-and-domain-environment-dependencies"></a>Dependências de ambiente de rede e domínio

Este guia foi projetado para os administradores de sistema e de rede que tem seguido as instruções no Windows Server 2016 **guia da rede principal** para implantar uma rede principal, ou para aqueles que já implantaram as principais tecnologias incluídas na rede principal, incluindo o AD DS, \(DNS\) Domain Name System, Dynamic Host Configuration Protocol \(DHCP\), TCP\/IP, NPS e nome do serviço de Internet do Windows \(WINS\).  

A guia da rede principal está disponível nos seguintes locais:

- O Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponível na biblioteca técnica do Windows Server 2016. 

- O [guia da rede principal](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) também está disponível no formato do Word na Galeria do TechNet, em [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dependências de certificado do servidor
Existem duas opções disponíveis para o registro de servidores de autenticação com certificados de servidor para uso com autenticação de 802.1 X - implantar sua própria infraestrutura de chave pública usando serviços de certificados do Active Directory \(AD CS\) ou usar certificados de servidor que estão registrados por uma autoridade de certificação pública \(CA\).

##### <a name="ad-cs"></a>AD CS
Os administradores de rede e do sistema implantação sem fio autenticado siga as instruções no guia do Windows Server 2016 Core rede complementar, **implantar certificados de servidor para 802.1 X com e sem fio implantações**. Este guia explica como implantar e usar o AD CS para registrar os certificados server para computadores que executam o NPS.

Este guia está disponível no seguinte local.

- O Windows Server 2016 Core guia complementar da rede [implantar certificados de servidor para 802.1 X com e sem fio implantações](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) em formato HTML na biblioteca técnica do. 

##### <a name="public-ca"></a>Autoridade de certificação pública
Você pode comprar certificados de servidor de uma autoridade de certificação pública, como VeriSign, computadores clientes já confiem. 

Um computador cliente confia em uma autoridade de certificação quando o certificado da CA é instalado no repositório de certificados de autoridades de certificação confiáveis. Por padrão, computadores que executam o Windows têm vários certificados de CA públicos instalados em seu certificado de autoridades de certificação confiáveis armazenar.  

É recomendável que você consulte os guias de design e implantação para cada uma das tecnologias que são usadas neste cenário de implantação. Estes guias podem ajudá-lo a determinar se esse cenário de implantação fornece os serviços e configuração que você precisa para rede da sua organização.

### <a name="requirements"></a>Requisitos
A seguir estão os requisitos para implantar uma infraestrutura de acesso sem fio usando o cenário documentado neste guia:

- Antes de implantar esse cenário, você deve primeiro adquirir 802.1X\-pontos de acesso sem fio com capacidade para fornecer uma cobertura sem fio nos locais desejados no seu site. Seção de planejamento deste guia ajuda a determinar os recursos que devem ter suporte para os pontos de acesso.

- Active Directory serviços de domínio \(AD DS\) é instalado, como são as outras tecnologias de rede necessários, acordo com as instruções no guia de rede do Windows Server 2016 Core.  

- AD CS é implantado e certificados de servidor são registrados nos servidores NPS. Esses certificados são necessários ao implantar o método de autenticação baseada em certificate\ v2 PEAP\-MS\-CHAP que é usado neste guia.

- Um membro da sua organização esteja familiarizado com os padrões IEEE 802.11 que são compatíveis com os pontos de acesso sem fio e os adaptadores de rede sem fio que estão instalados em computadores cliente e dispositivos em sua rede. Por exemplo, alguém em sua organização esteja familiarizado com os tipos de radiofrequência, autenticação sem fio 802.11 \ (WPA2 ou WPA\) e as codificações de \(AES or TKIP\).

## <a name="what-this-guide-does-not-provide"></a>O que este guia não fornece  
A seguir estão alguns itens que neste guia não fornece:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Diretrizes abrangentes para selecionar 802.1X\-pontos de acesso sem fio com capacidade

Como existem muitas diferenças entre modelos de 802.1X\ e marcas-compatível com pontos de acesso sem fio, este guia não fornece informações detalhadas sobre:  

- Determinar qual marca ou o modelo de ponto de acesso sem fio é melhor adequada às suas necessidades.

- A implantação física dos pontos de acesso sem fio em sua rede.

- Advanced configuration de ponto de acesso sem fio, como para sem fio \(VLANs\) redes locais virtuais.

- Instruções sobre como configurar os atributos de vendor\ específicos de ponto de acesso sem fio no NPS.

Além disso, terminologia e nomes para configurações variam entre os modelos e marcas de ponto de acesso sem fio e podem não coincidir com os nomes de configuração genérico que são usados neste guia. Para obter mais detalhes de configuração de ponto de acesso sem fio, você deve revisar a documentação do produto fornecida pelo fabricante dos pontos de acesso sem fio.

### <a name="instructions-for-deploying-nps-server-certificates"></a>Instruções para implantar certificados do servidor NPS
  
Há duas alternativas para implantar certificados do servidor NPS. Este guia não fornece diretrizes abrangentes para ajudá-lo a determinar quais alternativa melhor atendem às suas necessidades. Em geral, no entanto, as opções que você enfrenta são:

- Comprando certificados de autoridade de certificação pública, como VeriSign, que já são confiáveis para clientes baseados em Windows \. Normalmente, essa opção é recomendada para redes menores.

- Implantando \(PKI\) uma infraestrutura de chave pública em sua rede usando o AD CS. Isso é recomendado para a maioria das redes, e as instruções sobre como implantar certificados do servidor com o AD CS estão disponíveis no guia de implantação mencionado anteriormente.

### <a name="nps-network-policies-and-other-nps-settings"></a>Políticas de rede NPS e outras configurações de NPS

Exceto para as configurações feitas quando você executa o **configurar 802.1 X** assistente, conforme documentado neste guia, este guia não fornece informações detalhadas para configurar manualmente o NPS condições, restrições ou outras configurações de NPS.

### <a name="dhcp"></a>DHCP

Este guia de implantação não fornece informações sobre como projetar ou implantando sub-redes DHCP para redes sem fio.

## <a name="technology-overviews"></a>Visões gerais de tecnologia
A seguir estão visões gerais de tecnologia de implantação de acesso sem fio:

### <a name="ieee-8021x"></a>IEEE 802.1 X
O padrão IEEE 802.1 X define o controle de acesso de rede com base em port\ que é usado para fornecer acesso de rede autenticada para redes Ethernet. Esse controle de acesso de rede com base em port\ usa as características físicas da infraestrutura LAN alternada para autenticar dispositivos conectados a uma porta de LAN. Acesso à porta pode ser negado se o processo de autenticação falhar. Embora esse padrão foi projetado para redes Ethernet com fio, ele foi adaptado para uso em LANs sem fio 802.11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\-\(APs\) de pontos de acesso sem fio compatível com
Esse cenário requer a implantação de uma ou mais 802.1X\-compatível com pontos de acesso sem fio compatíveis com o protocolo \(RADIUS\) Remote Authentication Dial\-In User Service.

802.1 X e compatível com RADIUS\ PAS, quando implantado em uma infraestrutura de RAIO com um servidor RADIUS, como um servidor NPS, são chamadas *clientes RADIUS*.

### <a name="wireless-clients"></a>Clientes sem fio
Este guia fornece detalhes de configuração abrangente para fornecer acesso autenticado para que os usuários domain\ membro que se conectam à rede com computadores cliente sem fio que executam o Windows 10, Windows 8.1 e Windows 8 802.1 X. Computadores devem ter ingressado no domínio para estabelecer acesso autenticado com êxito.

>[!NOTE]
>Você também pode usar computadores que executam o Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016 como clientes sem fio.

### <a name="support-for-ieee-80211-standards"></a>Suporte para IEEE 802.11 padrões
Sistemas operacionais Windows e Windows Server compatíveis oferecem suporte à built\ em redes sem fio 802.11. Nesses sistemas operacionais, um adaptador de rede sem fio 802.11 instalado aparece como uma conexão de rede sem fio na Central de rede e compartilhamento. 

Embora não haja suporte para redes sem fio 802.11 built\-in, os componentes do Windows sem fio variam de acordo com o seguinte:

- Os recursos do adaptador de rede sem fio. O adaptador de rede sem fio instalado deve dar suporte a LAN sem fio ou padrões de segurança sem fio que você precisa. Por exemplo, se o adaptador de rede sem fio não dá suporte a \(WPA\) Wi\-Fi Protected Access, você não pode habilitar ou configurar as opções de segurança WPA.

- Os recursos do driver do adaptador de rede sem fio. Para permitir que você configurar as opções de rede sem fio, o driver do adaptador de rede sem fio deve suportar o relatório de todos os seus recursos para o Windows. Verifique se que o driver do adaptador de rede sem fio é escrito para os recursos do seu sistema operacional. Além disso, certifique-se de que o driver é a versão mais atual, verificando o Microsoft Update ou o site do fornecedor do adaptador de rede sem fio.

A tabela a seguir mostra as taxas de transmissão e frequências para padrões comuns de sem fio 802.11 IEEE.

|Padrões|Frequências|Transmissão taxas de bits|Uso|
|-------------|---------------|--------------------------|---------|  
|802.11|Intervalo de frequência de banda S\ industriais, científica e médicos \(ISM\) \ (2.4 para 2.5 GHz\)|2 megabits por segundo \(Mbps\)|Obsoleto. Não costuma ser usado.|  
|802.11 B|Banda S\ ISM|11 Mbps|Costuma ser usado.|  
|802.11 A|ISM de banda c \ \ (5.725 para 5.875 GHz\)|54 Mbps|Não costuma ser usado devido à despesa e intervalo limitado.|  
|802.11g|Banda S\ ISM|54 Mbps|Amplamente usado. 802.11g dispositivos são compatíveis com 802.11 b dispositivos.|  
|802.11 N \2.4 e 5.0 GHz|C \ banda e banda S\ ISM|250 Mbps|Dispositivos de acordo com o IEEE pre\ ratificação padrão 802.11 n se tornou disponíveis em agosto de 2007. 802.11 N muitos dispositivos são compatíveis com 802.11 a, b e dispositivos g.|
|802.11ac |5 GHz |6.93 Gbps |802.11ac, aprovados pela IEEE em 2014, é mais escalável e mais rápidas do que 802.11 n e é implantado onde PAS e clientes sem fio oferecer suporte a ele.|

### <a name="wireless-network-security-methods"></a>Métodos de segurança de rede sem fio
**Sem fio métodos de segurança de rede** é um agrupamento informal de autenticação sem fio \ (às vezes chamado de security\ sem fio) e a criptografia de segurança sem fio. Criptografia e autenticação sem fio são usados em pares para impedir que usuários não autorizados acessem a rede sem fio e para proteger a transmissão sem fio. 

Ao configurar as configurações de segurança sem fio sem fio rede políticas da política de grupo, há várias combinações de escolherem. No entanto, somente o Enterprise WPA2\, WPA\-Enterprise e abrir com 802.1 X padrões de autenticação são compatíveis para implantações sem fio 802.1 X autenticado.

>[!NOTE]
>Ao configurar as políticas de rede sem fio, você deve selecionar **WPA2\ Enterprise**, **WPA\ Enterprise**, ou **abrir com 802.1 X** para acessar as configurações de EAP que são necessárias para 802.1 X autenticado implantações sem fio.  

#### <a name="wireless-authentication"></a>Autenticação sem fio
Este guia recomenda que o uso dos seguintes padrões de autenticação sem fio 802.1 X autenticado implantações sem fio.  

**Wi\-Fi Protected Access – Enterprise \(WPA\-Enterprise\)** WPA é um padrão provisório desenvolvido pela rede Wi-Fi Alliance em conformidade com o protocolo de segurança sem fio 802.11. O protocolo WPA foi desenvolvido em resposta a um número de falhas graves descobertos no protocolo privacidade equivalente com fio \(WEP\) anterior.

Enterprise WPA\ oferece segurança aprimorada WEP ao:  

1. Exigir autenticação, que usa a estrutura de X EAP 802.1 como parte da infraestrutura que garante centralizada autenticação mútua e gerenciamento de chaves dinâmico  

2. Aprimorando \(ICV\) o valor de verificação de integridade com um \(MIC\) verificação de integridade de mensagem, para proteger o cabeçalho e a carga  

3. Implementando um contador de quadros para evitar ataques de repetição  

**Wi\-Fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)** como o WPA\-Enterprise padrão, WPA2\-Enterprise usa 802.1 X e EAP framework. WPA2\ Enterprise oferece maior proteção de dados para vários usuários e grandes redes gerenciadas. WPA2\ empresarial é um protocolo robusto que foi projetado para impedir o acesso não autorizado de rede, verificando os usuários da rede por meio de um servidor de autenticação.  

#### <a name="wireless-security-encryption"></a>Criptografia de segurança sem fio
Criptografia de segurança sem fio é usada para proteger as transmissões sem fio que são enviadas entre o cliente sem fio e o ponto de acesso sem fio. Criptografia de segurança sem fio é usada em conjunto com o método de autenticação de segurança de rede selecionada. Por padrão, os computadores que executam o Windows 10, Windows 8.1 e Windows 8 dão suporte a dois padrões de criptografia:

1. **Protocolo de integridade de chave temporal** \(TKIP\) é um protocolo de criptografia mais antigo que foi projetado originalmente para fornecer uma criptografia mais segura sem fio que foi fornecido pelo protocolo inerentemente fracas \(WEP\) privacidade equivalente com fio. TKIP foi projetado por IEEE 802.11 i grupo e o Wi\-Fi Alliance para substituir o WEP sem exigir a substituição de hardware herdado de tarefas. TKIP é um conjunto de algoritmos que encapsula a carga WEP e permite que os usuários do equipamento de rede Wi-Fi Herdado atualizar para o TKIP sem substituição de hardware. Como WEP, TKIP usa o algoritmo de criptografia de fluxo RC4 como sua base. O novo protocolo, no entanto, criptografa cada pacote de dados com uma chave de criptografia exclusiva e as chaves são muito mais fortes que aqueles pelo WEP. Embora TKIP é útil para a atualização de segurança em dispositivos mais antigos que foram projetados para usar apenas WEP, ele não aborda todos os problemas de segurança voltado para redes sem fio e na maioria dos casos não é suficientemente robusto para proteger confidenciais governamentais ou transmissões de dados corporativos.  

2. **Advanced Encryption Standard** \(AES\) é o protocolo de criptografia preferencial para a criptografia de dados comerciais e governamentais. AES oferece um nível maior de segurança de transmissão sem fio que TKIP ou WEP. Ao contrário TKIP e WEP, AES exige hardware sem fio que suporta o padrão AES. AES é um padrão de criptografia de chave symmetric\ que usa três codificações de bloco, AES\-128, 192 AES\ e AES\-256.

No Windows Server 2016, os seguintes métodos de criptografia sem fio com base em AES\ estão disponíveis para configuração nas propriedades de perfil sem fio quando você seleciona um método de autenticação do WPA2\-Enterprise, o que é recomendado.

1. **CCMP AES\**. Contador modo codificação bloco encadeamento mensagem código protocolo de autenticação \(CCMP\) implementa o padrão 802.11 i e foi projetado para criptografia de segurança mais alta do que o fornecido pelo WEP e usa chaves de criptografia AES de 128 bits.
2. **AES\ GCMP**. Protocolo de modo de contador Galois \(GCMP\) é suportada pela 802.11ac, é mais eficiente do que AES\ CCMP e fornece melhor desempenho para os clientes sem fio. GCMP usa chaves de criptografia AES de 256 bits.

> [!IMPORTANT]
> Com fio equivalência privacidade \(WEP\) era o original sem fio padrão de segurança que foi usada para criptografar o tráfego de rede. Você não deve implantar WEP em sua rede porque há das vulnerabilidades conhecidas well\ neste formulário desatualizado de segurança.

### <a name="active-directory-doman-services-ad-ds"></a>\(AD DS\) de serviços de domínio do Active Directory
O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos de rede e dados específicos do application\ de aplicativos habilitados para Directory \. Os administradores podem usar o AD DS para organizar elementos de uma rede, como os usuários, computadores e outros dispositivos, uma estrutura hierárquica de contenção. A estrutura hierárquica contenção inclui floresta do Active Directory, domínios na floresta e unidades organizacionais \(OUs\) em cada domínio. Um servidor que está executando o AD DS é chamado uma *controlador de domínio*.  

AD DS contém as contas de usuário, contas de computador e propriedades da conta que são exigidas por IEEE 802.1 X e PEAP\-MS\-CHAP v2 para autenticar as credenciais do usuário e para avaliar a autorização para conexões sem fio.

### <a name="active-directory-users-and-computers"></a>Usuários e computadores active Directory
Diretório de usuários e computadores do Active é um componente do AD DS que contém as contas que representam as entidades físicas, como um computador, uma pessoa ou um grupo de segurança. A *grupo de segurança* é uma coleção de contas de usuário ou computador que os administradores podem gerenciar como uma única unidade. Contas de usuários e computadores que pertencem a um determinado grupo são chamadas de *Agrupar membros*.  

### <a name="group-policy-management"></a>Gerenciamento de política de grupo
Gerenciamento de política de grupo permite o gerenciamento de configuração e alteração baseada em Directory \ Configurações de usuário e computador, incluindo informações de segurança e do usuário. Usar política de grupo para definir configurações para grupos de usuários e computadores. Com a política de grupo, você pode especificar configurações para entradas do registro, segurança, instalação de software, scripts, redirecionamento de pasta, serviços de instalação remota e manutenção do Internet Explorer. A política de grupo configuração que você crie está contidas em uma política de grupo objeto \(GPO\). Associando um GPO selecionados contêineres de sistema do Active Directory — sites, domínios e OUs — você pode aplicar as configurações do GPO aos usuários e computadores nesses contêineres do Active Directory. Para gerenciar objetos de política de grupo em uma empresa, você pode usar o Editor de gerenciamento de política de grupo Microsoft Management Console \(MMC\).  

Este guia fornece instruções detalhadas sobre como especificar as configurações da rede sem fio \ (IEEE 802.11\) extensão de políticas de gerenciamento de política de grupo. A rede sem fio \ (IEEE 802.11\) políticas configurar computadores cliente sem fio de membro domain\ com a conectividade necessária e configurações sem fio 802.1 X autenticado acesso sem fio.

### <a name="server-certificates"></a>Certificados de servidor
Esse cenário de implantação exige certificados de servidor para cada servidor NPS que realiza autenticação 802.1 X.  

Um certificado de servidor é um documento digital que costuma ser usado para autenticação e proteção de informações em redes abertas. Um certificado com segurança associa uma chave pública para a entidade que contém a chave privada correspondente. Certificados são assinados digitalmente pela CA de emissão, e eles podem ser emitidos para um usuário, um computador ou um serviço.  

Uma autoridade de certificação \(CA\) é uma entidade responsável por estabelecer e comprovar a autenticidade de chaves públicas pertencentes a assuntos \ (geralmente os usuários ou computadores \) ou outras autoridades de certificação. Atividades de uma autoridade de certificação podem incluir a vinculação de chaves públicas para nomes diferenciados por meio de certificados assinados, o gerenciamento de números de série de certificado e revogação de certificados.  

Active Directory os serviços de certificado \(AD CS\) é uma função de servidor que emite certificados como uma autoridade de certificação de rede. Um AD CS certificado infraestrutura, também conhecido como um *infraestrutura de chave pública \(PKI\)*, fornecem serviços personalizáveis para emitir e gerenciar certificados para a empresa.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>PEAP, EAP e PEAP\-MS\-CHAP v2
Extensible Authentication Protocol \(EAP\) estende o protocolo de ponto de to\ Point\ \(PPP\), permitindo que os métodos de autenticação adicional que usam credenciais e informações de troca de tamanhos arbitrários. Com a autenticação de EAP, ambos os a acesso à rede cliente e o autenticador \ (por exemplo, o servidor \ NPS) deve ter suporte para o mesmo tipo EAP para autenticação bem-sucedida ocorra. Windows Server 2016 inclui uma infraestrutura EAP, dá suporte a dois tipos EAP e a capacidade de passar mensagens EAP para servidores NPS. Ao usar o EAP, você pode dar suporte a esquemas de autenticação adicionais, conhecidos como *tipos EAP*. Os tipos EAP que são suportados pelo Windows Server 2016 são:  

- Segurança de camada de transporte \(TLS\)

- Microsoft Challenge Handshake Authentication Protocol versão 2 \ (CHAP MS\ v2\)

>[!IMPORTANT]
>Os tipos EAP robustos \ (como aqueles que se baseiam certificates\) oferecem mais segurança contra ataques de força brute\, ataques de dicionário e ataques de protocolos de autenticação baseada em password\ de detecção de senha \ (por exemplo, CHAP ou CHAP MS\ 1 versão \).

Protegido \(PEAP\) EAP usa TLS para criar um canal criptografado entre um cliente PEAP de autenticação, como um computador sem fio e um autenticador PEAP, como um servidor NPS ou outros servidores RADIUS. PEAP não especifica um método de autenticação, mas ele oferece segurança adicional para outros protocolos de autenticação EAP \ (como v2\ EAP\-MS\-CHAP) que pode operar pelo canal criptografado TLS fornecido pelo PEAP. PEAP é usado como um método de autenticação para clientes de acesso que estejam conectados à rede da sua organização por meio dos seguintes tipos de rede acesso servidores \(NASs\):

- 802.1X\-pontos de acesso sem fio com capacidade

- 802.1X\-capaz de chaves de autenticação

- Computadores que executam o Windows Server 2016 e \(RAS\) o serviço de acesso remoto que estão configurados como particular virtual de rede \(VPN\) servidores, servidores do DirectAccess ou ambos

- Computadores que executam o Windows Server 2016 e serviços de área de trabalho remota

PEAP\-MS\-CHAP v2 é mais fácil de implantar que TLS EAP\ porque a autenticação do usuário é executada usando credenciais baseadas em password\ \ (nome de usuário e password\), em vez de certificados ou cartões inteligentes. Somente NPS ou outros servidores RADIUS são necessárias para ter um certificado. O certificado do servidor NPS é usado pelo servidor NPS durante o processo de autenticação para provar sua identidade para os clientes PEAP.

Este guia fornece instruções para configurar os clientes sem fio e seu server\(s\) NPS para usar PEAP\-MS\-CHAP v2 para acesso autenticado 802.1 X.

### <a name="network-policy-server"></a>Servidor de política de rede
Rede o servidor de política \(NPS\) permite configurar e gerenciar políticas de rede usando Remote Authentication Dial\-In User Service \(RADIUS\) servidor e proxy RADIUS centralmente. NPS é necessário quando você implanta acesso sem fio 802.1 X.

Quando você configura seu pontos de acesso sem fio 802.1 X como clientes RADIUS de NPS, o NPS processa as solicitações de conexão enviadas por pontos de acesso. Durante o processamento de solicitação de conexão, o NPS realiza autenticação e autorização. Autenticação determina se o cliente tenha apresentado credenciais válidas. Se o NPS autentica com êxito o cliente solicitante, NPS determina se o cliente está autorizado a fazer a conexão solicitada, e permite ou nega a conexão. Isso é explicado com mais detalhes da seguinte maneira:

#### <a name="authentication"></a>Autenticação

Autenticação de v2 PEAP\-MS\-CHAP mútua bem-sucedido tem duas partes principais:

1.  O cliente autentica o servidor NPS.  Durante essa fase de autenticação mútua, o servidor NPS envia seu certificado do servidor para o computador cliente para que o cliente pode verificar a identidade do servidor NPS com o certificado. Para autenticar o servidor NPS com êxito, o computador cliente deve confiar na CA que emitiu o certificado do servidor NPS. O cliente confiará essa CA ao certificado da CA está presente no repositório de certificados de autoridades de certificação confiáveis no computador cliente.

    Se você implantar sua própria CA particular, o certificado da CA é automaticamente instalado no repositório de certificados de autoridades de certificação confiáveis para o usuário atual e para o computador Local quando a diretiva de grupo é atualizada no computador de cliente de membro do domínio. Se você decidir implantar certificados de servidor de uma autoridade de certificação pública, certifique-se de que o certificado de autoridade de certificação pública já está no repositório de certificados de autoridades de certificação confiáveis.  

2.  O servidor NPS autentica o usuário. Depois que o cliente autentica com êxito o servidor NPS, o cliente envia as credenciais do usuário com base em password\ para o servidor NPS, que verifica as credenciais do usuário contra o banco de dados de contas de usuário nos serviços de domínio do Active Directory \(AD DS\).

Se as credenciais forem válidas e autenticação for bem-sucedida, o servidor NPS começa a fase de autorização do processamento da solicitação de conexão. Se as credenciais não são válidas e autenticação falhar, o NPS envia uma mensagem de rejeição de acesso e a solicitação de conexão será negada.  

#### <a name="authorization"></a>Autorização

O servidor NPS executa autorização da seguinte maneira:  

1. NPS verifica se há restrições em que o usuário ou computador dial\ propriedades da conta no AD DS. Cada conta de usuário e computador no Active Directory Users and Computers inclui várias propriedades, incluindo aqueles encontrados no **Dial\-in** guia. Nessa guia, em **permissão de acesso de rede**, se o valor for **permitir acesso**, o usuário ou computador está autorizado a se conectar à rede. Se o valor for **negar acesso**, o usuário ou o computador não está autorizado a se conectar à rede. Se o valor for **controlar o acesso por meio da política de rede NPS**, NPS avalia as políticas de rede configuradas para determinar se o usuário ou computador está autorizado a se conectar à rede. 

2. Em seguida, o NPS processa suas políticas de rede para encontrar uma política que corresponda à solicitação de conexão. Se uma política de correspondência for encontrada, o NPS concede ou nega a conexão com base na configuração da política.  

Se forem bem sucedidas autenticação e autorização, e a política de rede correspondentes concede acesso, NPS concede acesso à rede e o usuário e o computador podem se conectar aos recursos de rede para os quais tenham permissões.  

>[!NOTE]
>Para implantar o acesso sem fio, você deve configurar políticas NPS. Este guia fornece instruções para usar o **configurar 802.1 X wizard** no NPS para criar políticas NPS para 802.1 X autenticado acesso sem fio.  

### <a name="bootstrap-profiles"></a>Perfis de inicialização
Em 802.1X\-autenticados redes sem fio, os clientes sem fio devem fornecer as credenciais de segurança que são autenticadas por um servidor RADIUS para se conectar à rede. Para EAP protegido \[PEAP\]\-Microsoft Challenge Handshake Authentication Protocol versão 2 \[MS\-CHAP v2\], as credenciais de segurança são um nome de usuário e senha. Para \[TLS\ EAP\-Transport Layer Security] ou PEAP\-TLS, as credenciais de segurança são certificados, como certificados de usuário e computador cliente ou cartões inteligentes.

Ao se conectar a uma rede que está configurada para executar PEAP\-MS\-CHAP v2, PEAP\ TLS ou autenticação EAP\ TLS, por padrão, os clientes do Windows sem fio também deverá validar um certificado de computador que é enviado pelo servidor RADIUS. O certificado de computador que é enviado pelo servidor RADIUS para cada sessão de autenticação é conhecido como um certificado de servidor.

Como mencionado anteriormente, você pode emitir os servidores RADIUS o certificado de servidor de duas maneiras: de uma CA comercial \ (como VeriSign, Inc., \), ou de uma CA particular que você implantar em sua rede. Se o servidor RADIUS enviará um certificado de computador que foi emitido por uma autoridade de certificação comercial que já tenha um certificado raiz instalado no repositório de certificados de autoridades de certificação confiáveis do cliente, o cliente sem fio pode validar o certificado de computador do servidor RADIUS, independentemente se o cliente sem fio já ingressou no domínio do Active Directory. Nesse caso o cliente sem fio pode se conectar à rede sem fio e, em seguida, você pode ingressar o computador ao domínio.

>[!NOTE]
>O comportamento de exigir que o cliente para validar o certificado do servidor pode ser desabilitado, mas desabilitando a validação do certificado do servidor não é recomendado em ambientes de produção.

Perfis de inicialização sem fio são temporários perfis que estão configurados de forma como para permitir que os usuários do cliente sem fio conectar-se para o 802.1X\-rede sem fio autenticada antes que o computador tiver ingressado no domínio, e \ / ou antes que o usuário faz logon com êxito domínio usando um computador sem fio fornecido pela primeira vez.  Esta seção resume qual problema for encontrado quando tentar ingressar em um computador sem fio ao domínio, ou para um usuário use um computador sem fio associado domain\ pela primeira vez para fazer logon no domínio.

Para implantações em que o usuário ou o administrador de TI não é possível conectar fisicamente um computador à rede Ethernet com fio para ingressar o computador ao domínio e o computador não tiver a emissão de necessário raiz certificado da CA instalado no seu **autoridades de certificação confiáveis** repositório de certificados, você pode configurar clientes sem fio com um perfil temporário de conexão sem fio, chamado um *inicializar perfil* , para se conectar à rede sem fio.

A *inicializar perfil* elimina a necessidade de validar o certificado de computador do servidor RADIUS. Essa configuração temporária permite que o usuário ingressar o computador no domínio, no momento em que a rede sem fio de rede sem fio \ (IEEE 802.11\) políticas são aplicadas e o certificado da CA raiz apropriado é automaticamente instalado no computador.

Quando a política de grupo é aplicada, um ou mais perfis de conexão sem fio que impõem o requisito de autenticação mútua são aplicados no computador. o perfil de inicialização não for mais necessário e é removido. Depois de ingressar o computador ao domínio e reiniciar o computador, o usuário pode usar uma conexão sem fio para fazer logon no domínio.

Para obter uma visão geral do processo de implantação de acesso sem fio usando essas tecnologias, consulte [visão geral da implantação acesso sem fio](b-wireless-access-deploy-overview.md).
