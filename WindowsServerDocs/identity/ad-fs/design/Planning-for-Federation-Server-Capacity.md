---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: "Planejamento de capacidade do servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 618dc9419be965dedaaf7dc946da436a5001f121
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-capacity"></a>Planejamento de capacidade do servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Capacidade de planejamento para servidores de Federação ajuda você a calcular:  
  
-   Quais fatores aumentam o tamanho do banco de dados de configuração do AD FS.  
  
-   Os requisitos de hardware apropriado para cada servidor de Federação.  
  
-   O número de servidores de federação para colocar em cada organização.  
  
Servidores de Federação emitem tokens de segurança para os usuários. Esses tokens são apresentados a um terceiro para consumo. Servidores de Federação emitem tokens de segurança depois de autenticar um usuário ou depois de receber um token de segurança que anteriormente foi emitido por um serviço de Federação do parceiro. Um token de segurança é solicitado de um serviço de Federação quando os usuários inicialmente fazer logon aplicativos federados ou quando respectivos tokens de segurança expirarem enquanto eles estão acessando aplicativos federados.  
  
Servidores de Federação são projetados para acomodar as configurações de farm de servidor de disponibilidade high\ que usam a tecnologia de balanceamento de carga de rede Microsoft \(NLB\). Servidores de Federação em uma configuração de farm podem atender a solicitações de forma independente, sem acessar quaisquer componentes farm comuns para cada solicitação. Portanto, há pouca sobrecarga envolvida no dimensionamento de uma implantação de servidor de Federação.  
  
**Recomendações:**  
  
-   Para mission\ críticos ou implantações high\ disponibilidade, recomendamos que você criar um farm de servidores de Federação pequeno em cada organização de parceiro, com pelo menos dois servidores de federação por farm, para fornecer tolerância.  
  
-   Com a necessidade de alta disponibilidade e a facilidade de dimensionamento de servidores de federação, o dimensionamento é o método recomendado para lidar com grande número de solicitações por segundo para um determinado serviço de Federação. É improvável produzir capacidade significativa manipulando ganhos subindo além da configuração básica neste guia.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Crescimento e tamanho de banco de dados de configuração do AD FS  
O tamanho do banco de dados de configuração do AD FS geralmente é considerado pequeno e tamanho de banco de dados não tendem a ser importante em implantações do AD FS.  No tamanho exato do banco de dados de configuração do AD FS depende amplamente o número de relações de confiança e os metadados relacionados à trust\ associados — como requerimentos judiciais ou Extrajudiciais, reivindicar regras e configurações definidas para cada relação de confiança de monitoramento. À medida que aumenta o número de entradas de confiança no banco de dados de configuração, aumenta a necessidade de mais espaço em disco.  
  
Para obter informações de implantação adicionais sobre o banco de dados de configuração do AD FS, consulte [considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Requisitos de espaço de memória, CPU e disco  
Felizmente, requisitos de espaço de memória, CPU e disco para servidores de Federação são moderados, e eles não têm probabilidade de ser um fator de automóvel de decisões de hardware. Para obter mais informações sobre requisitos de hardware, consulte [apêndice a: examinando o AD FS requisitos](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> Em testes que foram realizados pela equipe de produto do AD FS usando um farm de servidores de Federação configurado com um SQL Server dedicado para armazenar o banco de dados de configuração do AD FS, a carga total no SQL Server tendia a ser baixa. Em um teste usando um farm de four\-federation\-servidor que foi configurado para usar um único SQL Server, a utilização da CPU não ultrapassou 10% apesar dos testes que consiga acessar os servidores de Federação utilização de destino.  
  
## <a name="bk_estimatefs"></a>Estimar o número de servidores de federação para sua organização  
Em um esforço para simplificar o processo para servidores de federação de planejamento de hardware, a equipe de produto do AD FS desenvolveu a planilha do AD FS capacidade planejamento de dimensionamento. Essa planilha do Excel inclui calculator\ funcionalidade que levará dados de uso esperado que você fornece sobre os usuários em sua organização e retorna um número recomendado ideal de servidores de federação para seu ambiente de produção do AD FS.  
  
> [!NOTE]  
> O número de servidores de federação que essa planilha recomendará se baseia as especificações de hardware e rede que a equipe de produto do AD FS usada durante o teste. Portanto, o número de servidores de federação que a planilha recomendará deve ser compreendido nesse contexto.  Para obter mais informações sobre as especificações usado durante o teste, consulte o tópico [planejamento de capacidade de servidor do AD FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Usando a planilha de dimensionamento de planejamento da capacidade de FS AD  
Quando você usa essa planilha, você precisará selecionar um valor \ (qualquer **40%**, **60%**, ou **80%**\) que melhor representa o percentual do total de usuários que você espera envia solicitações de autenticação para os servidores de Federação durante os períodos de uso de pico.  
  
Em seguida, você precisará selecionar um valor \ (qualquer **1 minuto**, **15 minutos**, ou **1 hora**\) que melhor representa o período de tempo que você espera que o período de uso de pico para durar. Por exemplo, você pode estimar a 40% como o valor para o número total de usuários que vai fazer logon em um período de 15 minutos, ou que 60% dos usuários irá fazer logon em um período de 1 hora. Juntos, esses valores definem o perfil de carga de pico pelo qual seu recomendação de dimensionamento é calculada.  
  
Em seguida, você precisará especificar o número total de usuários que exigem acesso de sign\-on único para o aplicativo com reconhecimento de claims\ destino, com base em se os usuários são:  
  
-   Registro em log no Active Directory de um computador local que esteja fisicamente conectado à rede corporativa \ (por meio de authentication\ integrado do Windows)  
  
-   Registro em log no Active Directory remotamente em um computador que não esteja fisicamente conectado à rede corporativa \ (por meio do Windows integrada autenticação ou nome de usuário e password\)  
  
-   De outra organização e estão tentando acessar o aplicativo com reconhecimento de claims\ destino de um parceiro confiável  
  
-   De um provedor de identidade SAML 2.0 e está tentando acessar o aplicativo com reconhecimento de claims\ destino  
  
#### <a name="how-to-use-this-spreadsheet"></a>Como usar essa planilha  
Você pode usar as etapas a seguir para cada instância de farm do servidor de federação que você planeja implantar para determinar o número recomendado de servidores de Federação.  
  
1.  Baixe e, em seguida, abra o [planilha de dimensionamento do planejamento do AD FS capacidade para o Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) ou o [planilha de dimensionamento de planejamento do AD FS capacidade para o Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Na célula à direita do **durante o período de uso do sistema de pico, posso esperar essa porcentagem de meus usuários se autentiquem** da célula, clique na célula e, em seguida, use as setas de drop\ para baixo para selecionar o nível de utilização estimada de sistema, ou **40%**, **60%** ou **80%** para a implantação.  
  
3.  Na célula à direita do **dentro do período de tempo seguir** da célula, clique na célula e, em seguida, use as setas de drop\ para baixo para selecionar **1 minuto**, **15 minutos**, ou **1 hora** para selecionar a duração da carga de pico.  
  
4.  Na célula à direita do **Enter número estimado de aplicativos internos \ (como SharePoint \(2007 or 2010\) ou declarações web ciente applications\)** da célula, digite o número de aplicativos internos que você usará em sua organização.  
  
5.  Na célula à direita do **Enter número estimado de aplicativos on-line \ (como o Office 365 Exchange Online, SharePoint Online ou Lync Online\)** da célula, digite o número de aplicativos on-line ou serviços que você irá usados em sua organização.  
  
6.  Sob a célula intitulada **número de usuários**, um número em cada linha que se aplica a um cenário de aplicativo de exemplo, os usuários serão necessário único sign\ acesso de tipo. Essa coluna deve conter o número de usuários definidos, não os usuários de pico por segundo. Se tentativas de acesso para o aplicativo primeiro devem passar por meio da página de descoberta de território primário, digite **Y**. Se você não tiver certeza de que essa seleção, digite **Y**.  
  
7.  Examine as seguintes valores que são fornecidas recomendadas:  
  
    1.  Para o número total de servidores de Federação recomendada, consulte a célula inferior direita que está realçada em cinza.  
  
    2.  Para o número de servidores recomendado para cada cenário de aplicativo de exemplo, consulte a célula na linha que está realçada em cinza.  
  
> [!NOTE]  
> O valor que será calculado automaticamente na célula à direita da célula intitulada **Número Total de servidores de Federação recomendado** na parte inferior da planilha contém uma fórmula que adicionará um buffer de 20% adicionais sobre o total de soma de todos os valores em cada uma das linhas individuais anterior. A fórmula adicionada para o **Número Total de servidores de Federação recomendado** célula compilações nesse buffer para seu total recomendado número de servidores de Federação implantados para torná-lo muito improvável que a carga total no farm nunca atingirá seu ponto de saturação.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
