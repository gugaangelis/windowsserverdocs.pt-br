---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Conceitos de replicação do Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 087dee7c914b81d97c6cf3a2ea985d809cb57fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812347"
---
# <a name="active-directory-replication-concepts"></a>Conceitos de replicação do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de projetar a topologia de site, familiarize-se com alguns conceitos de replicação do Active Directory.  
  
-   [Objeto de Conexão](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Funcionalidade de failover](#BKMK_3)  
  
-   [Subrede](#BKMK_4)  
  
-   [Site](#BKMK_5)  
  
-   [Link de site](#BKMK_6)  
  
-   [Ponte de link de site](#BKMK_7)  
  
-   [Transitividade da relação de link de site](#BKMK_8)  
  
-   [Servidor de catálogo global](#BKMK_9)  
  
-   [Cache de associação de grupo universal](#BKMK_10)  
  
## <a name="BKMK_1"></a>Objeto de Conexão  
Um objeto de conexão é um objeto do Active Directory que representa uma conexão de replicação de um controlador de domínio de origem para um controlador de domínio de destino. Um controlador de domínio é um membro de um único site e é representado no site por um objeto de servidor nos serviços de domínio Active Directory (AD DS). Cada objeto de servidor tem um filho do objeto Configurações NTDS que representa o controlador de domínio de replicação no site.  
  
O objeto de conexão é um filho do objeto Configurações NTDS no servidor de destino. Para a replicação ocorrer entre os dois controladores de domínio, o objeto de servidor de um deve ter um objeto de conexão que representa a replicação de entrada da outra. Todas as conexões de replicação para um controlador de domínio são armazenadas como objetos de conexão no objeto de configurações NTDS. O objeto de conexão identifica o servidor de origem de replicação, contém um agendamento de replicação e especifica um transporte de replicação.  
  
O Knowledge Consistency Checker (KCC) cria os objetos de conexão automaticamente, mas também podem ser criados manualmente. Objetos de Conexão criados pelo KCC aparecem em Sites do Active Directory e o snap-in Serviços como **<automatically generated>** e são considerados adequados sob condições normais de operação. Objetos de Conexão criados por um administrador são criados manualmente os objetos de conexão. Um objeto de conexão criados manualmente é identificado pelo nome atribuído pelo administrador quando ela foi criada. Quando você modifica uma **<automatically generated>** conexão do objeto, você pode convertê-la em um objeto de conexão administrativamente modificado e o objeto é exibido na forma de um GUID. O KCC não fazer alterações em objetos de conexão manual ou modificados.  
  
## <a name="BKMK_2"></a>KCC  
O KCC é um processo interno que é executado em todos os controladores de domínio e gera a topologia de replicação para a floresta do Active Directory. O KCC cria topologias de replicação separadas, dependendo se a replicação está ocorrendo em um site (intrasites) ou entre sites (entre sites). O KCC também ajusta dinamicamente a topologia para acomodar a adição de novos controladores de domínio, a remoção de controladores de domínio existente, o movimento dos controladores de domínio para e de sites, alterando os custos e agendamentos e os controladores de domínio que são temporariamente indisponível ou em um estado de erro.  
  
Dentro de um site, as conexões entre os controladores de domínio gravável sempre são organizadas em um anel bidirecional, com conexões de atalho adicional para reduzir a latência em grandes sites. Por outro lado, a topologia entre sites é uma disposição em camadas de abrangência árvores, que significa que uma conexão entre sites existe entre dois sites para cada partição de diretório e geralmente não contém conexões de atalho. Para obter mais informações sobre a abrangência de árvores e topologia de replicação do Active Directory, consulte Active Directory replicação topologia referência técnica ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
Em cada controlador de domínio, o KCC cria rotas de replicação com a criação de objetos de conexão de entrada unidirecional que definem as conexões de outros controladores de domínio. Para controladores de domínio no mesmo site, o KCC cria objetos de conexão automaticamente sem intervenção administrativa. Quando você tiver mais de um site, você configura links de site entre sites e um único KCC em cada site cria automaticamente as conexões entre os sites também.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Melhorias do KCC para RODCs do Windows Server 2008  

Há uma série de aprimoramentos de KCC para acomodar o controlador de domínio recentemente disponível de somente leitura (RODC) no Windows Server 2008. Um cenário típico de implantação para o RODC é o escritório da filial. A topologia de replicação do Active Directory mais comumente implantada nesse cenário se baseia em um projeto de hub e spoke, onde os controladores de domínio de filial em vários sites replicam com um pequeno número de servidores bridgehead em um site de hub.  
  
Um dos benefícios da implantação RODC nesse cenário é que a replicação unidirecional. Servidores bridgehead não são necessários para replicar do RODC, que reduz a administração e o uso de rede.  
  
No entanto, um desafio administrativo realçado pela topologia hub-spoke em versões anteriores do sistema operacional Windows Server é que, depois de adicionar um novo controlador de domínio de ponte no hub, não há nenhum mecanismo automático para redistribuir o conexões de replicação entre os controladores de domínio de filial e os controladores de domínio de hub para tirar proveito do novo controlador de domínio de hub.  
  
Para controladores de domínio do Windows Server 2003, você pode redistribuir a carga de trabalho, usando uma ferramenta como Adlb.exe do guia de implantação do Windows Server 2003 Branch Office ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Windows Server 2008 nos RODCs, o funcionamento normal do KCC fornece alguns rebalanceamento, que elimina a necessidade de usar uma ferramenta adicional, como Adlb.exe. A nova funcionalidade é habilitada por padrão. Você pode desabilitá-lo adicionando a seguinte chave do registro definida no RODC:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**"BH aleatório o balanceamento de carga permitido"**  
**1 = habilitada (padrão), 0 = desabilitado**  
  
Para obter mais informações sobre como esses aprimoramentos do KCC funcionam, consulte Planejando e implantando os serviços de domínio do Active Directory para filiais ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funcionalidade de failover  
Sites garantem que a replicação seja roteada em torno de falhas de rede e controladores de domínio offline. O KCC é executado em intervalos especificados para ajustar a topologia de replicação para as alterações que ocorrem no AD DS, como quando novos controladores de domínio são adicionados e novos sites são criados. O KCC examina o status de replicação das conexões existentes para determinar se todas as conexões não estão funcionando. Se uma conexão não está funcionando devido a um controlador de domínio com falha, o KCC compila automaticamente conexões temporárias para outros parceiros de replicação (se disponível) para garantir que a replicação ocorra. Se todos os controladores de domínio em um site não estiverem disponíveis, o KCC cria automaticamente as conexões de replicação entre controladores de domínio de outro site.  
  
## <a name="BKMK_4"></a>Subrede  
Uma sub-rede é um segmento de uma rede TCP/IP para o qual um conjunto de endereços IP lógicos são atribuídos. Subredes agrupar computadores de forma que identifica sua proximidade física na rede. Objetos da sub-rede no AD DS identificam os endereços de rede que são usados para mapear os computadores para sites.  
  
## <a name="BKMK_5"></a>Site  
Os sites são objetos do Active Directory que representam uma ou mais sub-redes TCP/IP com conexões de rede altamente confiável e rápida. Informações do site permite que os administradores configurem o acesso ao Active Directory e a replicação para otimizar o uso da rede física. Objetos do site estão associados um conjunto de sub-redes, e cada controlador de domínio em uma floresta está associado um site do Active Directory de acordo com seu endereço IP. Sites podem hospedar controladores de domínio de mais de um domínio e um domínio pode ser representado em mais de um site.  
  
## <a name="BKMK_6"></a>Link de site  
Links de site são objetos do Active Directory que representam caminhos lógicos que o KCC usa para estabelecer uma conexão para replicação do Active Directory. Um objeto de link de site representa um conjunto de sites que podem se comunicar a um custo uniforme por meio de um transporte entre sites especificado.  
  
Todos os sites contidos no link de site são considerados estar conectados por meio do mesmo tipo de rede. Sites devem ser vinculados manualmente para outros sites usando links de site para que os controladores de domínio em um site podem replicar alterações de diretório de controladores de domínio em outro site. Como os links de site não correspondem ao caminho real feito pelos pacotes de rede na rede física durante a replicação, você não precisará criar links de site redundantes para melhorar a eficiência de replicação do Active Directory.  
  
Quando dois sites estão conectadas por um link de site, o sistema de replicação cria automaticamente as conexões entre controladores de domínio específicos em cada site que são chamados de servidores bridgehead. No Windows Server 2008, todos os controladores de domínio em um site que hospedam a mesma partição de diretório são candidatos à que está sendo selecionada como servidores bridgehead. As conexões de replicação criadas pelo KCC aleatoriamente são distribuídas entre todos os servidores bridgehead de candidato em um site para compartilhar a carga de trabalho de replicação. Por padrão, a seleção aleatório processo ocorre apenas uma vez, quando os objetos de conexão pela primeira vez são adicionados ao site.  
  
## <a name="BKMK_7"></a>Ponte de link de site  
Uma ponte de link de site é um objeto do Active Directory que representa um conjunto de links de site, de sites podem se comunicar por meio de um transporte comum. Pontes de link de site permitem que os controladores de domínio que não estão diretamente conectados por meio de um link de comunicação para replicar entre si. Normalmente, uma ponte de link de site corresponde a um roteador (ou um conjunto de roteadores) em uma rede IP.  
  
Por padrão, o KCC pode formar uma rota transitiva através de links de todos os sites que têm alguns sites em comum. Se esse comportamento estiver desabilitado, cada link de site representa sua própria rede distinta e isolada. Conjuntos de links de site que pode ser tratado como uma única rota são expressos por meio de uma ponte de link de site. Cada ponte representa um ambiente isolado de comunicação para o tráfego de rede.  
  
Pontes de link de site são um mecanismo para representar logicamente transitiva conectividade física entre sites. Uma ponte de link de site permite que o KCC usar qualquer combinação dos links de site incluídos para determinar a rota menos dispendiosa para interconectar partições de diretório mantidas nesses sites. A ponte de link de site não oferece conectividade real com os controladores de domínio. Se a ponte de link de site for removida, a replicação nos links de site combinado continuará até que o KCC remove os links.  
  
Pontes de link de site serão necessárias apenas se um site contém um controlador de domínio que hospeda uma partição de diretório que também não está hospedada em um controlador de domínio em um site adjacente, mas um controlador de domínio que hospeda a partição de diretório está localizado em um ou mais sites no a floresta. Sites adjacentes são definidos como quaisquer dois ou mais sites incluídos em um único link de site.  
  
Uma ponte de link de site cria uma conexão lógica entre dois links de site, fornecendo um caminho transitivo entre dois sites desconectados, usando um site provisório. Para fins de gerador de topologia entre sites (ISTG), a ponte implica conectividade física usando o site provisório. A ponte não implica que um controlador de domínio no site provisório fornecerá o caminho de replicação. No entanto, isso poderia ser o caso se o site provisório continha um controlador de domínio hospedado na partição de diretório a serem replicados, nesse caso, uma ponte de link de site não é necessária.  
  
O custo de cada link de site é adicionado, criando um custo somado para o caminho resultante. A ponte de link de site seria usada se o site provisório não contém um controlador de domínio que hospeda a partição de diretório e um link de custo mais baixo, não existe. Se o site provisório continha um controlador de domínio que hospeda a partição de diretório, dois sites desconectados seriam configurar conexões de replicação para o controlador de domínio provisório e não usar a ponte.  
  
## <a name="BKMK_8"></a>Transitividade da relação de link de site  
Por padrão todos os links de site são transitivas, ou "ponte". Quando os links de site são ligados com pontes e as agendas se sobrepõem, o KCC cria conexões de replicação que parceiros de replicação de controlador de domínio entre sites, em que os sites não estão diretamente conectados por links de site, mas estão conectados transitivamente por meio de determinar um conjunto de sites comuns. Isso significa que você pode conectar qualquer site para qualquer outro site por meio de uma combinação de links de site.  
  
Em geral, para uma rede totalmente roteada, não é necessário criar pontes de links de qualquer site, a menos que você deseja controlar o fluxo de alterações de replicação. Se sua rede não está totalmente roteada, pontes de link de site devem ser criados para evitar tentativas de replicação impossível. Todos os links de site para um transporte específico implicitamente pertencem a uma ponte de link de site único para esse transporte. A ponte de links de site padrão ocorre automaticamente, e nenhum objeto do Active Directory representa que a ponte. O **ponte para todos os links de site** configuração, encontrada nas propriedades de contêineres de transporte entre sites IP tanto o SMTP Simple Mail Transfer Protocol (), que implementa a ponte de link de site automática.  
  
> [!NOTE]  
> A replicação SMTP não terão suporte em versões futuras do AD DS; Portanto, não é recomendável criar objetos de links de site no contêiner de SMTP.  
  
## <a name="BKMK_9"></a>Servidor de catálogo global  
Um servidor de catálogo global é um controlador de domínio que armazena informações sobre todos os objetos na floresta, para que aplicativos possam pesquisar AD DS sem se referir a controladores de domínio específicos que armazenam os dados solicitados. Como todos os controladores de domínio, servidor de catálogo global armazena réplicas graváveis, completas das partições de diretório de configuração e de esquema e uma réplica completa e gravável da partição de diretório do domínio para o domínio que está hospedando. Além disso, o servidor de catálogo global armazena uma réplica parcial, somente leitura de todos os outros domínios na floresta. Réplicas de domínio parcial, somente leitura contém todos os objetos no domínio, mas apenas um subconjunto dos atributos (aqueles atributos que são mais comumente usados para pesquisar o objeto).  
  
## <a name="BKMK_10"></a>Cache de associação de grupo universal  
Cache de associação de grupo universal permite que o controlador de domínio em cache informações de associação de grupo universal para os usuários. Você pode habilitar os controladores de domínio que executam o Windows Server 2008 para associações ao grupo universal de cache usando o snap-in Serviços e Sites do Active Directory.  
  
Habilitação do cache de associação de grupo universal elimina a necessidade de um servidor de catálogo global em todos os sites em um domínio, o que minimiza o uso de largura de banda de rede como um controlador de domínio não precisa replicar todos os objetos localizados na floresta. Isso também reduz o tempo de logon porque os controladores de domínio de autenticação não é sempre necessário acessar um catálogo global para obter informações sobre associação de grupo universal. Para obter mais informações sobre quando usar o cache de associação de grupo universal, consulte [planejamento de posicionamento de servidores de Catálogo Global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


