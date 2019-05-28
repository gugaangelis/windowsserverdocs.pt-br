---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: Planning for Federation Server Capacity (Planejando a capacidade do servidor de federação)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 569bea74fe7750eaf2b410a552876e0862b1e24b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191088"
---
# <a name="planning-for-federation-server-capacity"></a>Planning for Federation Server Capacity (Planejando a capacidade do servidor de federação)

Planejamento de capacidade para servidores de Federação ajuda a estimar:  
  
-   Quais fatores aumentar o tamanho do banco de dados de configuração do AD FS.  
  
-   Os requisitos de hardware apropriado para cada servidor de Federação.  
  
-   O número de servidores de federação para colocar em cada organização.  
  
Servidores de Federação emitem tokens de segurança para usuários. Esses tokens são apresentados para uma terceira parte confiável para consumo. Servidores de Federação emitem tokens de segurança depois de autenticar um usuário ou depois de receber um token de segurança emitido anteriormente por um serviço de Federação do parceiro. Um token de segurança é solicitado de um serviço de federação, quando os usuários inicialmente entram em aplicativos federados ou quando seus tokens de segurança expirarem enquanto eles estão acessando aplicativos federados.  
  
Servidores de Federação são criados para acomodar alta\-configurações de farm de servidores de disponibilidade que usam o balanceamento de carga de rede da Microsoft \(NLB\) tecnologia. Servidores de Federação em uma configuração de farm podem atender a solicitações de forma independente, sem acessar quaisquer componentes comuns do farm para cada solicitação. Portanto, há pouca sobrecarga envolvida na expansão de uma implantação de servidor de Federação.  
  
**Recomendações:**  
  
-   Para a missão\-alta ou crítica\-implantações de disponibilidade, recomendamos que você crie um farm de servidores de Federação pequenos em cada organização parceira, pelo menos dois servidores de federação por farm, para fornecer tolerância a falhas.  
  
-   Com a necessidade de alta disponibilidade e a facilidade de dimensionar os servidores de federação, a expansão é o método recomendado para manipular o grande número de solicitações por segundo para um determinado serviço de Federação. Escalar verticalmente, além da configuração de base neste guia é improvável que produzir ganhos de tratamento de capacidade significativa.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Crescimento e tamanho de banco de dados de configuração do AD FS  
O tamanho do banco de dados de configuração do AD FS é geralmente considerado ser pequeno, e o tamanho do banco de dados não tendem a ser uma consideração importante em implantações do AD FS.  O tamanho exato do banco de dados de configuração do AD FS pode depender muito o número de relações de confiança e a relação de confiança associada\-metadados relacionados — como declarações, regras de declaração, monitoramento e as configurações definidas para cada objeto de confiança. À medida que cresce o número de entradas de relação de confiança no banco de dados de configuração, aumenta a necessidade de mais espaço em disco.  
  
Para obter informações de implantação adicionais sobre o banco de dados de configuração do AD FS, consulte [considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Requisitos de espaço de memória, CPU e disco  
Felizmente, os requisitos de espaço de memória, CPU e disco para servidores de Federação são modestos, e eles não provavelmente serão um fator determinante em decisões de hardware. Para obter mais informações sobre requisitos de hardware, consulte [apêndice a: Revisar os requisitos do AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> Em testes que foram executados pela equipe de produto do AD FS usando um farm de servidores de Federação configurado com um SQL Server dedicado para armazenar o banco de dados de configuração do AD FS, a carga total no SQL Server tendia a ficar baixo. Em um teste usando um quatro\-federação\-farm de servidores que foi configurado para usar um único SQL Server, utilização da CPU não excedeu 10%, apesar de teste que trouxe os servidores de federação para utilização de destino.  
  
## <a name="bk_estimatefs"></a>Estimar o número de servidores de federação para sua organização  
Em um esforço para simplificar o processo para servidores de federação de planejamento de hardware, a equipe de produto do AD FS desenvolveu a planilha de dimensionamento para planejamento de capacidade do AD FS. Essa planilha do Excel inclui Calculadora\-com a funcionalidade que tomará os dados de uso esperado que você fornecer sobre os usuários em sua organização e retornar um número ideal recomendado de servidores de federação para o seu ambiente de produção do AD FS .  
  
> [!NOTE]  
> O número de servidores de federação que essa planilha recomendará baseia-se sobre as especificações de hardware e de rede que a equipe de produto do AD FS usada durante o teste. Portanto, o número de servidores de federação que a planilha será recomendável deve ser compreendido dentro desse contexto.  Para obter mais informações sobre as especificações usadas durante o teste, consulte o tópico intitulado [planejamento de capacidade do servidor do AD FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Usando a planilha de dimensionamento do planejamento de capacidade do AD FS  
Quando você usa essa planilha, você precisará selecionar um valor \(ambos **40%** , **60%** , ou **80%** \) que melhor representa o percentual de total de usuários que você espera que enviará solicitações de autenticação para seus servidores de Federação durante períodos de pico de uso.  
  
Em seguida, você precisará selecionar um valor \(ambos **1 minuto**, **15 minutos**, ou **1 hora** \) que melhor representa o período de tempo esperado o período de uso de pico para a última. Por exemplo, você pode estimar a 40% como o valor para o número total de usuários que farão logon em um período de 15 minutos, ou que 60% dos usuários farão logon em um período de 1 hora. Juntos, esses valores definem o perfil de carga de pico pelo qual sua recomendação de dimensionamento será calculada.  
  
Em seguida, você precisará especificar o número total de usuários que precisarão ter o logon único\-sobre o acesso às declarações de destino\-reconhecimento de aplicativo, dependendo se os usuários são:  
  
-   Fazer logon no Active Directory de um computador local que está fisicamente conectado à sua rede corporativa \(por meio da autenticação integrada do Windows\)  
  
-   Fazer logon no Active Directory remotamente a partir de um computador que não está fisicamente conectado à sua rede corporativa \(por meio do Windows integrado autenticação ou nome de usuário e senha\)  
  
-   Partir de outra organização e são a tentativa de acessar as declarações de destino\-aplicativo com reconhecimento de um parceiro confiável  
  
-   De um provedor de identidade SAML 2.0 e está tentando acessar as declarações de destino\-aplicativo com reconhecimento de  
  
#### <a name="how-to-use-this-spreadsheet"></a>Como usar essa planilha  
Você pode usar as etapas a seguir para cada instância de farm de servidor de federação que você planeja implantar para determinar o número recomendado de servidores de Federação.  
  
1.  Baixe e abra o [planilha de dimensionamento do planejamento do AD FS capacidade para o Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) ou o [planilha de dimensionamento do planejamento do AD FS capacidade para o Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Na célula à direita do **durante o período de uso do sistema de pico, espero que essa porcentagem dos usuários se autentiquem** de célula, clique na célula e, em seguida, use a\-para baixo as setas para selecionar a utilização de sistema estimados nível, tanto **40%** , **60%** ou **80%** para a implantação.  
  
3.  Na célula à direita do **dentro do período de tempo seguinte** de célula, clique na célula e, em seguida, use a operação de soltar\-para baixo as setas para selecionar **1 minuto**, **de15minutos**, ou **1 hora** para selecionar a duração da carga de pico.  
  
4.  Na célula à direita do **Enter número estimado de aplicativos internos \(como o SharePoint \(2007 ou 2010\) ou aplicativos web com reconhecimento de declarações\)**  de célula, digite o número aplicativos internos, você usará em sua organização.  
  
5.  Na célula à direita do **Enter número estimado de aplicativos online \(, como Office 365 Exchange Online, SharePoint Online ou o Lync Online\)**  de célula, digite o número de aplicativos online ou serviços que serão usados em sua organização.  
  
6.  Na célula intitulada **número de usuários**, digite um número em cada linha que se aplica a um cenário de aplicativo de exemplo, os usuários serão precisa de logon único\-no acesso a. Essa coluna deve conter o número de usuários definidos, não os usuários de pico por segundo. Se as tentativas de acesso feitas para o aplicativo devem primeiro ir por meio da página de descoberta de realm inicial, digite **Y**. Se você não tiver certeza de que essa seleção, digite **Y**.  
  
7.  Examine as seguintes recomendadas valores que são fornecidos:  
  
    1.  Para obter o número total de servidores de Federação recomendados, consulte a célula inferior direita que está realçada em cinza.  
  
    2.  Para o número de servidores recomendados para cada cenário de aplicativo de exemplo, consulte a célula na linha que está realçada em cinza.  
  
> [!NOTE]  
> O valor que será calculado automaticamente na célula à direita da célula intitulada **o número Total de servidores de Federação recomendado** na parte inferior da planilha contém uma fórmula que adicionará um buffer adicional de 20% para o soma total de todos os valores em cada uma das linhas individuais precede. A fórmula adicionada para o **Número Total de servidores de Federação recomendado** célula se baseia nesse buffer ao seu total número recomendado de servidores de Federação implantado para torná-lo muito improvável que nunca atingirá a carga total no farm seu ponto de saturação.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
