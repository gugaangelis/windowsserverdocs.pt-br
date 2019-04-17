---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: "Planejar o posicionamento do controlador de domínio Regional"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c5254560e9b1a10030fa37ec0966868986212a4a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-regional-domain-controller-placement"></a>Planejar o posicionamento do controlador de domínio Regional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para garantir a eficiência de custo, planeje colocar como alguns controladores de domínio regionais quanto possível. Primeiro, lembre-se da planilha de (DSSTOPO_1.doc) "Localizações geográficas e comunicação Links" usada em [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) para determinar se um local é um hub.  
  
Planeje colocar os controladores de domínio regionais para cada domínio que é representado em cada local de hub. Depois que você coloca controladores de domínio regionais em todos os locais de hub, avalie a necessidade de colocar os controladores de domínio regionais em locais de satélite. Eliminar controladores de domínio regionais desnecessário de locais de satélite reduz os custos de suporte necessários para manter uma infraestrutura de servidor remoto.  
  
Além disso, certifique-se a segurança física dos controladores de domínio em locais de hub e satélite para que pessoas não autorizadas não podem acessá-los. Não coloque controladores de domínio gravável em hub e satélite locais em que você não pode garantir a segurança física do controlador de domínio. Uma pessoa que tenha acesso físico a um controlador de domínio gravável pode atacar o sistema por:  
  
-   Acessando discos físicos iniciando um sistema operacional alternativo em um controlador de domínio.  
  
-   Remoção (e possivelmente substituição) discos físicos em um controlador de domínio.  
  
-   Obtendo e manipulação de uma cópia de um backup de estado de sistema de controlador de domínio.  
  
Adicione controladores de domínio regionais graváveis apenas aos locais em que você pode garantir a segurança física.  
  
Em locais com segurança física inadequada, implantação de um controlador de domínio somente leitura (RODC) é a solução recomendada. Com exceção das senhas de conta, o RODC tem todos os objetos do Active Directory e atributos que mantém um controlador de domínio gravável. No entanto, não é fazer alterações no banco de dados que é armazenado no RODC. Alterações devem ser feitas em um controlador de domínio gravável e, em seguida, replicadas de volta para o RODC.  
  
Para autenticar o acesso aos servidores de arquivos local e logons de clientes, a maioria das organizações coloque controladores de domínio regionais para todos os domínios regionais que são representados em um determinado local. No entanto, você deve considerar muitas variáveis ao avaliar se um local de negócios exige que seus clientes tenham autenticação local ou os clientes possam confiar na autenticação e consulta ao longo de um link WAN (rede) de longa distância. A ilustração a seguir mostra como determinar se você quer colocar controladores de domínio em locais de satélite.  
  
![Posicionamento do plano dc regionais](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilidade de conhecimento técnico no local  
Controladores de domínio precisam ser gerenciados continuamente por vários motivos. Coloque um controlador de domínio regionais apenas em locais que incluem autorizados que podem administrar o controlador de domínio ou certifique-se de que o controlador de domínio pode ser gerenciado remotamente.  
  
Em ambientes de filiais com segurança física normalmente ruim e pessoal com pouco conhecimento de tecnologia de informações, implantando um RODC geralmente é a solução recomendada. Permissões administrativas locais para um RODC podem ser delegadas a qualquer usuário do domínio sem que o usuário conceder a quaisquer direitos de usuário para o domínio ou outros controladores de domínio. Isso permite que um usuário ramificação local para fazer logon em um RODC e executar o trabalho de manutenção no servidor, como atualizar um driver. No entanto, o usuário ramificação não pode fazer logon para qualquer outro controlador de domínio ou executar qualquer outra tarefa administrativa no domínio. Dessa forma, o usuário ramificação pode ser recebido a autoridade adequada para gerenciar efetivamente o RODC na filial sem comprometer a segurança do resto do domínio ou floresta.  
  
## <a name="wan-link-availability"></a>Disponibilidade do link WAN  
Links WAN que sofrem frequentes pode causar perda de produtividade significativa para os usuários se a localização não incluir um controlador de domínio que possa autenticar os usuários. Se sua disponibilidade do link WAN não é 100% e sites remotos não podem tolerar uma queda de serviço, coloque um controlador de domínio regionais nos locais onde os usuários precisam ter a capacidade de fazer logon ou acesso ao exchange server quando o link WAN é pressionada.  
  
## <a name="authentication-availability"></a>Disponibilidade de autenticação  
Determinados organizações, como bancos, exigem que os usuários seja autenticado em todos os momentos. Coloque um controlador de domínio regionais em um local onde a disponibilidade do link WAN não é 100%, mas os usuários precisam de autenticação em todos os momentos.  
  
## <a name="logon-performance-over-wan-links"></a>Desempenho de logon por links WAN  
Se a sua disponibilidade do link WAN seja altamente confiável, colocar um controlador de domínio no local depende os requisitos de desempenho de logon através do link WAN. Fatores que influenciam o desempenho de logon na WAN incluem a velocidade do link e largura de banda disponível, número de usuários e perfis de uso e a quantidade de tráfego de rede de logon versus o tráfego de replicação.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Utilização de velocidade e a largura de banda do link WAN  
As atividades de um usuário único podem congestionar um link WAN lento. Coloque um controlador de domínio em um local se o desempenho de logon através do link WAN é inaceitável.  
  
O percentual de utilização de largura de banda médio indica como congestionado um link de rede. Se um link de rede tem utilização de largura de banda média que é maior do que um valor aceitável, coloque um controlador de domínio nesse local.  
  
### <a name="number-of-users-and-usage-profiles"></a>Número de usuários e perfis de uso  
O número de usuários e seus perfis de uso em um determinado local pode ajudar a determinar se você precisa colocar controladores de domínio regionais nesse local. Para evitar a perda de produtividade se uma conexão WAN falhar, coloque um controlador de domínio regionais em um local que tenha 100 ou mais usuários.  
  
Os perfis de uso indicam como os usuários usarem os recursos de rede. Você não precisa colocar um controlador de domínio em um local que contém apenas alguns usuários que não acessará com frequência recursos de rede.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Tráfego de rede de logon versus o tráfego de replicação  
Se um controlador de domínio não estiver disponível no mesmo local como o cliente do Active Directory, o cliente cria o tráfego de logon na rede. A quantidade de tráfego de rede de logon que é criado na rede física é influenciada por diversos fatores, incluindo associações de grupo; número e o tamanho dos objetos de política de grupo (GPOs); scripts de logon; e recursos, como pastas offline, redirecionamento de pasta e perfis móveis.  
  
Por outro lado, um controlador de domínio é colocado em um determinado local gera o tráfego de replicação na rede. A frequência e a quantidade de atualizações feitas em partições hospedadas nos controladores de domínio influenciam a quantidade de tráfego de replicação que é criado na rede. Os diferentes tipos de atualizações que podem ser feitas em partições hospedadas nos controladores de domínio incluem adicionar ou alterando atributos de usuário e os usuários, mudar a senha e adicionar ou alterar grupos globais, impressoras ou volumes.  
  
Para determinar se é necessário colocar um controlador de domínio regionais em um local, compare o custo de tráfego de logon criado por um local sem um controlador de domínio contra o custo de tráfego de replicação criado colocando um controlador de domínio no local.  
  
Por exemplo, considere uma rede que tenha filiais que são conectadas por meio de links lentos à sede e em quais controladores de domínio podem ser facilmente adicionados. Se o tráfego de pesquisa de logon e o diretório do diário de alguns usuários do site remoto faz com que o tráfego de rede mais do que a replicação de todos os dados da empresa para o branch, considere a adição de um controlador de domínio para a ramificação.  
  
Se reduzir o custo de manutenção de controladores de domínio é mais importante do que o tráfego de rede, centralizar os controladores de domínio no domínio e não coloque quaisquer controladores de domínio regional no local ou, considere colocar RODCs no local.  
  
Para uma planilha para ajudá-lo a documentar o posicionamento dos controladores de domínio regionais e o número de usuários para cada domínio que é representado em cada local, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixe < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and Abra (DSSTOPO_4.doc) "Posicionamento do controlador de domínio".  
  
Você precisará fazer referência às informações sobre locais em que você precisa colocar controladores de domínio regionais quando você implanta domínios regionais. Para obter mais informações sobre como implantar domínios regionais, consulte [implantação do Windows Server 2008 regionais domínios](https://technet.microsoft.com/library/cc755118.aspx).  
  


