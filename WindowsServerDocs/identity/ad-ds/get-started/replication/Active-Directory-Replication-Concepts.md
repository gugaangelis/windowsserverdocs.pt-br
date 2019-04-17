---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: "Conceitos de replicação do Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 45753c048f9bb1cb174daade1d408b88b3686b4b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-replication-concepts"></a>Conceitos de replicação do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de criar a topologia de site, se familiarizar com alguns conceitos de replicação do Active Directory.  
  
-   [Objeto de Conexão](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Funcionalidade de failover](#BKMK_3)  
  
-   [Sub-rede](#BKMK_4)  
  
-   [Site](#BKMK_5)  
  
-   [Link de site](#BKMK_6)  
  
-   [Ponte do link de site](#BKMK_7)  
  
-   [Transitividade do link de site](#BKMK_8)  
  
-   [Servidor de catálogo global](#BKMK_9)  
  
-   [Cache de membros de grupos universais](#BKMK_10)  
  
## <a name="BKMK_1"></a>Objeto de Conexão  
Um objeto de conexão é um objeto do Active Directory que representa uma conexão de replicação de um controlador de domínio de origem para um controlador de domínio de destino. Um controlador de domínio é um membro de um único site e é representado por um objeto de servidor nos serviços de domínio do Active Directory (AD DS) no site. Cada objeto de servidor tem um filho configurações NTDS objeto que representa o controlador de domínio replicação no site.  
  
O objeto de conexão é um filho do objeto Configurações NTDS no servidor de destino. A ocorrência da replicação entre dois controladores de domínio, o objeto de servidor de um deve ter um objeto de conexão que representa a duplicação de entrada da outra. Todas as conexões de replicação de um controlador de domínio são armazenadas como objetos de conexão sob o objeto de configurações NTDS. O objeto de conexão identifica o servidor de origem de replicação, contém um agendamento de replicação e especifica um transporte de replicação.  
  
O verificador de consistência de Conhecimento (KCC) cria objetos de conexão automaticamente, mas também podem ser criados manualmente. Objetos de Conexão, criados pelo KCC aparecem em Sites do Active Directory e o snap-in Serviços como **<automatically generated>**e são consideradas adequadas em condições operacionais normais. Criado por um administrador de objetos de Conexão são criados manualmente objetos de conexão. Um objeto de conexão criada manualmente é identificado pelo nome atribuído pelo administrador quando ela foi criada. Quando você modifica uma **<automatically generated>**objeto de conexão, convertê-la em um objeto de conexão modificado de forma administrativa e o objeto aparece na forma de um GUID. O KCC não façam alterações em objetos de conexão manual ou modificados.  
  
## <a name="BKMK_2"></a>KCC  
O KCC é um processo interno que é executado em todos os controladores de domínio e gera a topologia de replicação de floresta do Active Directory. O KCC cria topologias de replicação separado dependendo se a replicação está ocorrendo em um site (entre sites) ou entre sites (entre sites). O KCC também ajusta dinamicamente a topologia para acomodar a adição de novos controladores de domínio, a remoção de controladores de domínio existentes, o movimento de controladores de domínio de e para sites, alterando os custos e agendas e controladores de domínio que não estão disponíveis temporariamente ou em um estado de erro.  
  
Em um site, as conexões entre controladores de domínio gravável sempre são organizadas em um anel bidirecional, com conexões de atalho adicionais para reduzir a latência em grandes sites. Por outro lado, a topologia entre sites é uma divisão em camadas de abrangência de árvores, que significa uma conexão entre sites existe entre dois sites para cada partição de diretório e geralmente não contém conexões de atalho. Para saber mais sobre a abrangência de árvores e topologia de replicação do Active Directory, consulte replicação topologia referência técnica Active Directory ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
Em cada controlador de domínio, o KCC cria rotas de replicação através da criação de objetos de conexão de entrada unidirecional que definem conexões a partir de outros controladores de domínio. Para controladores de domínio no mesmo site, o KCC cria objetos de conexão automaticamente sem intervenção administrativa. Quando você tiver mais de um site, configurar links de site entre sites e um único KCC em cada site cria automaticamente conexões entre os sites também.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Melhorias de KCC para Windows Server 2008 RODCs  

Há uma série de melhorias KCC para acomodar o controlador de domínio somente leitura recentemente disponibilizados (RODC) no Windows Server 2008. Um cenário típico de implantação para RODC é filial. A topologia de replicação do Active Directory mais comumente implantada nesse cenário é baseada em um projeto de hub e spoke, onde os controladores de domínio de ramificação em vários sites replicam com um pequeno número de servidores ponte em um site de hub.  
  
Um dos benefícios de implantar RODC nesse cenário é a replicação unidirecional. Servidores ponte não são necessários para replicar do RODC, o que reduz o uso de rede e administração.  
  
No entanto, um desafio administrativo realçado pela topologia de hub spoke em versões anteriores do sistema operacional Windows Server é que depois de adicionar um novo controlador de domínio de ponte no hub, existe um mecanismo automático de redistribuir as conexões de replicação entre os controladores de domínio ramificação e os controladores de domínio do hub para aproveitar o novo controlador de domínio do hub.  
  
Para controladores de domínio do Windows Server 2003, você pode rebalancear a carga de trabalho usando uma ferramenta como Adlb.exe do guia de implantação do Windows Server 2003 Branch Office ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Para Windows Server 2008 RODCs, funcionamento normal do KCC fornece algumas redistribuição, que elimina a necessidade de usar uma ferramenta adicional, como Adlb.exe. A nova funcionalidade é habilitada por padrão. Você pode desativá-lo, adicionando a seguinte chave do registro definido no RODC:  
  
**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters**  
  
**"Balanceamento de carga BH aleatório permitido"**  
**1 = habilitado (padrão), 0 = Disabled**  
  
Para obter mais informações sobre como essas melhorias KCC funcionam, consulte Planejando e implantando os serviços de domínio do Active Directory para filiais ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funcionalidade de failover  
Sites Certifique-se de que a replicação é roteada em torno de falhas de rede e controladores de domínio offline. ELE é executado em intervalos específicos para ajustar a topologia de replicação de mudanças que ocorrem no AD DS, como quando novos controladores de domínio são adicionados e novos sites são criados. O KCC examina o status de replicação de conexões existentes para determinar se todas as conexões não estão funcionando. Se uma conexão não está funcionando devido a um controlador de domínio que falhou, o KCC automaticamente compilações temporárias conexões com outros parceiros de replicação (se disponível) para garantir que a replicação ocorre. Se todos os controladores de domínio em um site não estiverem disponíveis, o KCC cria automaticamente conexões de replicação entre controladores de domínio de outro site.  
  
## <a name="BKMK_4"></a>Sub-rede  
Uma sub-rede é um segmento de uma rede TCP/IP para o qual um conjunto de endereços IP lógicos são atribuídas. Sub-redes agrupar computadores em uma forma que identifica sua proximidade física na rede. Os objetos de sub-rede no AD DS identificam os endereços de rede que são usados para mapear computadores aos sites.  
  
## <a name="BKMK_5"></a>Site  
Sites são objetos do Active Directory que representam uma ou mais sub-redes TCP/IP com conexões de rede altamente confiável e rápido. Informações do site permite que os administradores configurar o acesso ao Active Directory e replicação para otimizar o uso da rede física. Objetos de site são associados um conjunto de sub-redes, e cada controlador de domínio em uma floresta está associado um site do Active Directory de acordo com seu endereço IP. Sites podem hospedar controladores de domínio de mais de um domínio, e um domínio pode ser representado em mais de um site.  
  
## <a name="BKMK_6"></a>Link de site  
Links de site são objetos do Active Directory que representam os caminhos lógicos que ele usa para estabelecer uma conexão para replicação do Active Directory. Um objeto de link site representa um conjunto de sites que podem se comunicar a um custo uniforme por meio de um transporte entre sites especificado.  
  
Todos os sites contidos no link de site são considerados para se manter conectado por meio do mesmo tipo de rede. Sites devem ser vinculados manualmente a outros sites usando links de site para que os controladores de domínio em um site podem replicar as alterações no diretório de controladores de domínio em outro site. Como links de site não correspondem ao caminho real obtido com pacotes de rede na rede física durante a duplicação, você não precisa criar links de site redundante para melhorar a eficiência da replicação do Active Directory.  
  
Quando dois sites são conectados por um link de site, o sistema de replicação cria automaticamente conexões entre controladores de domínio específico em cada site que são chamados de servidores ponte. No Windows Server 2008, todos os controladores de domínio em um site que hospedam mesma partição de diretório são candidatos para sendo selecionado como servidores ponte. As conexões de replicação criadas pelo KCC aleatoriamente são distribuídas entre todos os servidores de ponte de candidato em um site para compartilhar a carga de trabalho de replicação. Por padrão, a seleção aleatório processo ocorre somente uma vez, quando os objetos de conexão pela primeira vez são adicionados ao site.  
  
## <a name="BKMK_7"></a>Ponte do link de site  
Uma ponte de link de site é um objeto do Active Directory que representa um conjunto de links de site, todos os sites podem se comunicar usando um transporte comum. Pontes habilitar controladores de domínio que não estão conectados diretamente por meio de um link de comunicação para replicar entre si. Normalmente, uma ponte de link de site corresponde a um roteador (ou um conjunto de roteadores) em uma rede IP.  
  
Por padrão, o KCC pode formar uma rota transitiva por meio de links de toda e qualquer site que têm em comum alguns sites. Se esse comportamento está desabilitado, cada link de site representa sua própria rede distinta e isolada. Conjuntos de links de site que pode ser tratado como uma única rota são expressos por meio de uma ponte de link de site. Cada ponte representa um ambiente de comunicação isolado para o tráfego de rede.  
  
Pontes são um mecanismo para representar logicamente transitiva conectividade física entre sites. Uma ponte de link de site permite que o KCC usar qualquer combinação dos links de sites incluídos para determinar a rota barata a interconexão de partições de diretório mantidas nesses sites. A ponte do link site não fornece conectividade real para os controladores de domínio. Se a ponte do link site for removida, replicação sobre os links de site combinados continuará até que o KCC remove os links.  
  
Pontes só são necessárias quando um site contém um controlador de domínio que hospeda uma partição de diretório que não está hospedada em um controlador de domínio em um site adjacente também, mas um controlador de domínio que hospeda a partição de diretório está localizado em um ou mais sites na floresta. Sites adjacentes são definidos como quaisquer dois ou mais sites incluídos em um único link de site.  
  
Uma ponte de link de site cria uma conexão entre dois links de site, fornecendo um caminho transitivo entre dois locais desconectados usando um site provisório lógico. Para fins do gerador de topologia entre sites (ISTG), a ponte implica conectividade física usando o site intermediário. A ponte não implica que um controlador de domínio no site do provisório fornecerá o caminho de replicação. No entanto, isso pode acontecer se o site provisório continha um controlador de domínio que hospedou a partição de diretório devem ser replicadas, caso em que uma ponte de link de site não é necessária.  
  
O custo de cada link de site é adicionado, criando um custo somado para o caminho resultante. A ponte do link site seria usada se o site intermediário não contém um controlador de domínio que hospeda a partição de diretório e um link de custo menor não existir. Se o site provisório continha um controlador de domínio que hospeda a partição de diretório, dois locais desconectados seriam configurar conexões de replicação ao controlador de domínio provisório e não usar a ponte.  
  
## <a name="BKMK_8"></a>Transitividade do link de site  
Por padrão todos os links de site são transitivas ou "transição". Quando o site links estão ligados pontes e as cronogramas sobreponham, o KCC cria conexões de replicação que determinam os parceiros de replicação de controlador de domínio entre sites, onde os sites não estão conectados diretamente por links de sites, mas estão conectados temporariamente por meio de um conjunto de sites comuns. Isso significa que você pode conectar qualquer site para qualquer outro site por meio de uma combinação de links do site.  
  
Em geral, para uma rede totalmente roteada, você não precisa criar qualquer site pontes de link, a menos que você quiser controlar o fluxo de alterações de replicação. Se sua rede não é totalmente roteada, pontes devem ser criados para evitar tentativas de replicação impossível. Todos os links de site para um transporte específico implicitamente pertencem a uma único site link ponte desse transporte. A ponte para obter links de site padrão ocorre automaticamente, e nenhum objeto do Active Directory representa essa ponte. O **ponte todos os links de sites** configuração, encontrada nas propriedades de contêineres de transporte entre sites IP tanto o SMTP Simple Mail Transfer Protocol (), que implementa ponte de link de site automática.  
  
> [!NOTE]  
> Replicação SMTP não terão suporte nas versões futuras do AD DS; Portanto, não é recomendável criar objetos de links de site no contêiner SMTP.  
  
## <a name="BKMK_9"></a>Servidor de catálogo global  
Um servidor de catálogo global é um controlador de domínio que armazena informações sobre todos os objetos na floresta, para que os aplicativos podem pesquisar o AD DS sem fazer referência a controladores de domínio específico que armazenam os dados solicitados. Como todos os controladores de domínio, um servidor de catálogo global armazena réplicas completas e graváveis as esquema e a configuração de partições de diretório e uma réplica completa, gravável da partição de diretório do domínio para o domínio que ela está hospedando. Além disso, um servidor de catálogo global armazena uma réplica parcial, somente leitura de todos os outros domínios na floresta. Réplicas domínio parcial, somente leitura contêm cada objeto no domínio, mas apenas um subconjunto dos atributos (os atributos que são mais comumente usados para pesquisar o objeto).  
  
## <a name="BKMK_10"></a>Cache de membros de grupos universais  
Cache de membros de grupos universais permite que o controlador de domínio em cache informações de associação de grupo universal para os usuários. Você pode habilitar controladores de domínio que executam o Windows Server 2008 para membros de grupos universais de cache, usando o snap-in Serviços e Sites do Active Directory.  
  
Habilitar o cache de membros de grupos universais elimina a necessidade de um servidor de catálogo global em cada site em um domínio, o que minimiza o uso de largura de banda de rede como um controlador de domínio não precisa replicar todos os objetos localizados na floresta. Ele também reduz horas de logon, porque os controladores de domínio de autenticação não sempre precisam acessar um catálogo global para obter informações de associação ao grupo universal. Para saber mais sobre quando usar o cache de membros de grupos universais, consulte [planejamento de posicionamento de servidor Catálogo Global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


