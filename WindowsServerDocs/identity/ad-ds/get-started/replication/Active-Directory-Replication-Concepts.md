---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Conceitos de replicação do Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ccc1801ba325fec4d7273b503ccb799966122b99
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591078"
---
# <a name="active-directory-replication-concepts"></a>Conceitos de replicação do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de criar a topologia do site, familiarize-se com alguns conceitos de replicação Active Directory.  
  
-   [Objeto de conexão](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Funcionalidade de failover](#BKMK_3)  
  
-   [Redes](#BKMK_4)  
  
-   [Locais](#BKMK_5)  
  
-   [Link do site](#BKMK_6)  
  
-   [Ponte de link de site](#BKMK_7)  
  
-   [Transitividade de link de site](#BKMK_8)  
  
-   [Servidor de catálogo global](#BKMK_9)  
  
-   [Cache de associação de grupo universal](#BKMK_10)  
  
## <a name="BKMK_1"></a>Objeto de conexão  
Um objeto de conexão é um objeto Active Directory que representa uma conexão de replicação de um controlador de domínio de origem para um controlador de domínio de destino. Um controlador de domínio é um membro de um único site e é representado no site por um objeto de servidor no Active Directory Domain Services (AD DS). Cada objeto de servidor tem um objeto de configurações NTDS filho que representa o controlador de domínio de replicação no site.  
  
O objeto de conexão é um filho do objeto de configurações NTDS no servidor de destino. Para que a replicação ocorra entre dois controladores de domínio, o objeto de servidor de um deve ter um objeto de conexão que representa a replicação de entrada do outro. Todas as conexões de replicação para um controlador de domínio são armazenadas como objetos de conexão no objeto de configurações NTDS. O objeto de conexão identifica o servidor de origem de replicação, contém um agendamento de replicação e especifica um transporte de replicação.  
  
O Knowledge Consistency Checker (KCC) cria objetos de conexão automaticamente, mas eles também podem ser criados manualmente. Os objetos de conexão criados pelo KCC aparecem no snap-in Active Directory sites e serviços como **<automatically generated>** e são considerados adequados em condições normais de operação. Os objetos de conexão criados por um administrador são objetos de conexão criados manualmente. Um objeto de conexão criado manualmente é identificado pelo nome atribuído pelo administrador quando ele foi criado. Quando você modifica um objeto de conexão **<automatically generated>** , você o converte em um objeto de conexão modificado administrativamente e o objeto é exibido na forma de um GUID. O KCC não faz alterações em objetos de conexão manuais ou modificados.  
  
## <a name="BKMK_2"></a>KCC  
O KCC é um processo interno que é executado em todos os controladores de domínio e gera a topologia de replicação para a floresta de Active Directory. O KCC cria topologias de replicação separadas dependendo se a replicação está ocorrendo dentro de um site (intra-site) ou entre sites (entre sites). O KCC também ajusta dinamicamente a topologia para acomodar a adição de novos controladores de domínio, a remoção de controladores de domínio existentes, a movimentação de controladores de domínio de e para sites, a alteração de custos e agendas e controladores de domínio que estão temporariamente indisponível ou em um estado de erro.  
  
Em um site, as conexões entre controladores de domínio graváveis são sempre organizadas em um anel bidirecional, com conexões de atalho adicionais para reduzir a latência em sites grandes. Por outro lado, a topologia entre sites é uma disposição em camadas de árvores de abrangência, o que significa que existe uma conexão entre sites entre dois sites para cada partição de diretório e geralmente não contém conexões de atalho. Para obter mais informações sobre árvores de abrangência e topologia de replicação de Active Directory, consulte Active Directory referência técnica da topologia de replicação ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
Em cada controlador de domínio, o KCC cria rotas de replicação criando objetos de conexão de entrada unidirecionais que definem conexões de outros controladores de domínio. Para controladores de domínio no mesmo site, o KCC cria objetos de conexão automaticamente sem intervenção administrativa. Quando você tem mais de um site, configura links de site entre sites, e um único KCC em cada site cria automaticamente conexões entre sites também.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Aprimoramentos do KCC para RODCs do Windows Server 2008  

Há várias melhorias do KCC para acomodar o RODC (controlador de domínio somente leitura) recentemente disponível no Windows Server 2008. Um cenário de implantação típico para o RODC é a filial. A topologia de replicação Active Directory mais comumente implantada nesse cenário é baseada em um design de Hub e spoke, em que os controladores de domínio de ramificação em vários sites replicam com um pequeno número de servidores bridgehead em um site de Hub.  
  
Um dos benefícios da implantação do RODC nesse cenário é a replicação unidirecional. Os servidores bridgehead não precisam ser replicados do RODC, o que reduz a administração e o uso da rede.  
  
No entanto, um desafio administrativo realçado pela topologia hub-spoke em versões anteriores do sistema operacional Windows Server é que, depois de adicionar um novo controlador de domínio bridgehead no Hub, não há um mecanismo automático para redistribuir o conexões de replicação entre os controladores de domínio da ramificação e os controladores de domínio do hub para aproveitar o novo controlador de domínio do Hub.  
  
Para controladores de domínio do Windows Server 2003, você pode reequilibrar a carga de trabalho usando uma ferramenta como o Adlb. exe do guia de implantação do Windows Server 2003 Branch Office ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Para RODCs do Windows Server 2008, o funcionamento normal do KCC fornece algum rebalanceamento, o que elimina a necessidade de usar uma ferramenta adicional, como o Adlb. exe. A nova funcionalidade é habilitada por padrão. Você pode desabilitá-lo adicionando o seguinte conjunto de chaves do registro no RODC:  
  
**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**"Balanceamento de carga de BH aleatório permitido"**  
**1 = habilitado (padrão), 0 = desabilitado**  
  
Para obter mais informações sobre como esses aprimoramentos do KCC funcionam, consulte Planejamento e implantação de Active Directory Domain Services para filiais ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funcionalidade de failover  
Os sites asseguram que a replicação seja roteada em relação a falhas de rede e controladores de domínio offline. O KCC é executado em intervalos especificados para ajustar a topologia de replicação para alterações que ocorrem em AD DS, como quando novos controladores de domínio são adicionados e novos sites são criados. O KCC revisa o status de replicação das conexões existentes para determinar se as conexões não estão funcionando. Se uma conexão não estiver funcionando devido a um controlador de domínio com falha, o KCC criará automaticamente conexões temporárias com outros parceiros de replicação (se disponíveis) para garantir que a replicação ocorra. Se todos os controladores de domínio em um site estiverem indisponíveis, o KCC criará automaticamente conexões de replicação entre os controladores de domínio de outro site.  
  
## <a name="BKMK_4"></a>Redes  
Uma sub-rede é um segmento de uma rede TCP/IP à qual um conjunto de endereços IP lógicos é atribuído. Sub-redes agrupam computadores de uma maneira que identifica sua proximidade física na rede. Os objetos de sub-rede no AD DS identificam os endereços de rede que são usados para mapear computadores para sites.  
  
## <a name="BKMK_5"></a>Locais  
Os sites são Active Directory objetos que representam uma ou mais sub-redes TCP/IP com conexões de rede altamente confiáveis e rápidas. As informações do site permitem que os administradores configurem Active Directory acesso e replicação para otimizar o uso da rede física. Os objetos do site são associados a um conjunto de sub-redes, e cada controlador de domínio em uma floresta é associado a um site Active Directory de acordo com seu endereço IP. Os sites podem hospedar controladores de domínio de mais de um domínio, e um domínio pode ser representado em mais de um site.  
  
## <a name="BKMK_6"></a>Link do site  
Links de site são Active Directory objetos que representam caminhos lógicos que o KCC usa para estabelecer uma conexão para Active Directory replicação. Um objeto de link de site representa um conjunto de sites que podem se comunicar com custo uniforme por meio de um transporte entre sites especificado.  
  
Todos os sites contidos no link do site são considerados conectados por meio do mesmo tipo de rede. Os sites devem ser vinculados manualmente a outros sites usando links de site para que os controladores de domínio em um site possam replicar alterações de diretório de controladores de domínio em outro site. Como os links de site não correspondem ao caminho real usado pelos pacotes de rede na rede física durante a replicação, você não precisa criar links de site redundantes para melhorar a eficiência de replicação Active Directory.  
  
Quando dois sites estão conectados por um link de site, o sistema de replicação cria automaticamente conexões entre controladores de domínio específicos em cada site que são chamados de servidores bridgehead. No Windows Server 2008, todos os controladores de domínio em um site que hospedam a mesma partição de diretório são candidatos para serem selecionados como servidores bridgehead. As conexões de replicação criadas pelo KCC são distribuídas aleatoriamente entre todos os servidores bridgehead candidatos em um site para compartilhar a carga de trabalho de replicação. Por padrão, o processo de seleção aleatório ocorre apenas uma vez, quando os objetos de conexão são adicionados primeiro ao site.  
  
## <a name="BKMK_7"></a>Ponte de link de site  
Uma ponte de link de site é um objeto Active Directory que representa um conjunto de links de site, todos aqueles cujos sites podem se comunicar usando um transporte comum. As pontes de link de site habilitam controladores de domínio que não estão conectados diretamente por meio de um link de comunicação para replicação entre si. Normalmente, uma ponte de link de site corresponde a um roteador (ou um conjunto de roteadores) em uma rede IP.  
  
Por padrão, o KCC pode formar uma rota transitiva por meio de qualquer e todos os links de site que têm alguns sites em comum. Se esse comportamento for desabilitado, cada link de site representa sua própria rede distinta e isolada. Conjuntos de links de site que podem ser tratados como uma única rota são expressos por meio de uma ponte de link de site. Cada ponte representa um ambiente de comunicação isolado para o tráfego de rede.  
  
As pontes de link de site são um mecanismo para representar logicamente a conectividade física transitiva entre sites. Uma ponte de link de site permite que o KCC use qualquer combinação dos links de site incluídos para determinar a rota menos dispendiosa para as partições de diretório de interconexão mantidas nesses sites. A ponte de link de site não fornece conectividade real com os controladores de domínio. Se a ponte de link de site for removida, a replicação nos links de site combinados continuará até que o KCC remova os links.  
  
As pontes de link de site só serão necessárias se um site contiver um controlador de domínio que hospede uma partição de diretório que não esteja também hospedada em um controlador de domínio em um site adjacente, mas um controlador de domínio que hospeda essa partição de diretório esteja localizado em um ou mais sites no a floresta. Os sites adjacentes são definidos como dois ou mais sites incluídos em um único link de site.  
  
Uma ponte de link de site cria uma conexão lógica entre dois links de site, fornecendo um caminho transitivo entre dois sites desconectados usando um site provisório. Para os fins do ISTG (gerador de topologia entre sites), a ponte implica conectividade física usando o site provisório. A ponte não significa que um controlador de domínio no site provisório fornecerá o caminho de replicação. No entanto, esse seria o caso se o site provisório contivesse um controlador de domínio que hospedasse a partição de diretório a ser replicada, caso em que uma ponte de link de site não é necessária.  
  
O custo de cada link de site é adicionado, criando um custo somado para o caminho resultante. A ponte de link de site seria usada se o site provisório não contiver um controlador de domínio que hospeda a partição de diretório e um link de custo mais baixo não existir. Se o site provisório contiver um controlador de domínio que hospeda a partição de diretório, dois sites desconectados configurariam as conexões de replicação com o controlador de domínio provisório e não usarão a ponte.  
  
## <a name="BKMK_8"></a>Transitividade de link de site  
Por padrão, todos os links de site são transitivos ou "com ponte". Quando os links de site são ponte e as agendas se sobrepõem, o KCC cria conexões de replicação que determinam parceiros de replicação de controlador de domínio entre sites, em que os sites não são diretamente conectados por links de site, mas são conectados transitivamente por meio de um conjunto de sites comuns. Isso significa que você pode conectar qualquer site a qualquer outro site por meio de uma combinação de links de site.  
  
Em geral, para uma rede totalmente roteada, você não precisa criar nenhuma ponte de link de site, a menos que queira controlar o fluxo de alterações de replicação. Se sua rede não for totalmente roteada, as pontes de link de site deverão ser criadas para evitar tentativas de replicação impossíveis. Todos os links de site para um transporte específico pertencem implicitamente a uma única ponte de link de site para esse transporte. A ponte padrão para links de site ocorre automaticamente e nenhum objeto de Active Directory representa essa ponte. A configuração **ponte para todos os links de site** , encontrada nas propriedades dos contêineres de transporte de IP e SMTP entre sites, implementa a ponte de link de site automática.  
  
> [!NOTE]  
> A replicação SMTP não terá suporte em versões futuras do AD DS; Portanto, a criação de objetos de links de site no contêiner SMTP não é recomendada.  
  
## <a name="BKMK_9"></a>Servidor de catálogo global  
Um servidor de catálogo global é um controlador de domínio que armazena informações sobre todos os objetos na floresta, para que os aplicativos possam Pesquisar AD DS sem se referir a controladores de domínio específicos que armazenam os dados solicitados. Assim como todos os controladores de domínio, um servidor de catálogo global armazena réplicas completas e graváveis das partições de diretório de esquema e configuração e uma réplica completa e gravável da partição de diretório de domínio para o domínio que está hospedando. Além disso, um servidor de catálogo global armazena uma réplica parcial somente leitura de todos os outros domínios na floresta. As réplicas de domínio somente leitura parciais contêm todos os objetos no domínio, mas apenas um subconjunto dos atributos (os atributos que são mais comumente usados para pesquisar o objeto).  
  
## <a name="BKMK_10"></a>Cache de associação de grupo universal  
O cache de associação de grupo universal permite que o controlador de domínio armazene em cache informações de associação de grupo universal para usuários. Você pode habilitar os controladores de domínio que executam o Windows Server 2008 para armazenar em cache associações de grupo universal usando o snap-in Active Directory sites e serviços.  
  
Habilitar o cache de associação de grupo universal elimina a necessidade de um servidor de catálogo global em cada site em um domínio, o que minimiza o uso da largura de banda da rede, pois um controlador de domínio não precisa replicar todos os objetos localizados na floresta. Ele também reduz os tempos de logon porque os controladores de domínio de autenticação nem sempre precisam acessar um catálogo global para obter informações de associação de grupo universal. Para obter mais informações sobre quando usar o cache de associação de grupo universal, consulte [planejando o posicionamento do servidor de catálogo global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


