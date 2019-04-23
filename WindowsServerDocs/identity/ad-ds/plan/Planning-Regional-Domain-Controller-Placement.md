---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Planejar o posicionamento de controlador de domínio Regional
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bec8595ab6eae8eb6cedaf9307ab97ac9c8316b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880437"
---
# <a name="planning-regional-domain-controller-placement"></a>Planejar o posicionamento de controlador de domínio Regional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para garantir a eficiência de custos, planeje colocar o menor número de controladores de domínio regional quanto possível. Primeiro, examine a planilha "Geográficas locais e Links de comunicação" (DSSTOPO_1.doc) usada na [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) para determinar se um local é um hub.  
  
Você planeja colocar controladores de domínio regional para cada domínio que é representado em cada local de hub. Depois de colocar os controladores de domínio regional em todos os locais de hub, avalie a necessidade de colocar os controladores de domínio regional em locais de satélite. A eliminação de controladores de domínio regional desnecessárias locais satélite reduz os custos de suporte necessários para manter uma infra-estrutura de servidor remoto.  
  
Além disso, verifique se a segurança física dos controladores de domínio em locais de hub e satélite para que pessoas não autorizadas não podem acessá-los. Não coloque os controladores de domínio graváveis em locais de hub e satélite em que você não pode garantir a segurança física do controlador de domínio. Uma pessoa que possui acesso físico a um controlador de domínio gravável pode atacar o sistema por:  
  
- Acessar discos físicos iniciando um sistema operacional alternativo em um controlador de domínio.  
- Remoção (e, possivelmente, substituição) os discos físicos em um controlador de domínio.  
- Obtendo e manipulando uma cópia de um backup de estado do sistema de controlador de domínio.  
  
Adicione controladores de domínio graváveis de regionais somente para locais em que você pode garantir a segurança física.  
  
Em locais com segurança física inadequada, implantação de um controlador de domínio somente leitura (RODC) é a solução recomendada. Com exceção das senhas de conta, o RODC tem todos os objetos do Active Directory e os atributos de um controlador de domínio gravável. No entanto, não é possível fazer alterações no banco de dados que é armazenada no RODC. Alterações devem ser feitas em um controlador de domínio gravável e, em seguida, replicadas para o RODC.  
  
Para autenticar o acesso aos servidores de arquivos local e logons de clientes, a maioria das organizações coloque os controladores de domínio regional para todos os domínios regionais que são representados em um determinado local. No entanto, você deve considerar muitas variáveis ao avaliar se um local de negócios exige que seus clientes tenham a autenticação local ou os clientes podem contar com autenticação e a consulta em um link WAN (rede) de longa distância. A ilustração a seguir mostra como determinar se é necessário colocar os controladores de domínio em locais de satélite.  
  
![plan regional dc placement](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilidade de conhecimentos técnicos no local

Controladores de domínio precisam ser gerenciados continuamente por vários motivos. Coloque um controlador de domínio regional apenas em locais que incluem funcionários que podem administrar o controlador de domínio, ou certifique-se de que o controlador de domínio pode ser gerenciado remotamente.  
  
Em ambientes de filiais com segurança física geralmente ruim e pessoal com pouco conhecimento de tecnologia da informação, implantar um RODC geralmente é a solução recomendada. Permissões administrativas locais para um RODC podem ser delegadas para qualquer usuário de domínio sem conceder nenhum direito de usuário para o domínio ou outros controladores de domínio. Isso permite que um usuário local da filial para fazer logon em um RODC e executar o trabalho de manutenção no servidor, como atualizar um driver. No entanto, o usuário de ramificação não pode fazer logon no outro controlador de domínio ou executar todas as outras tarefas administrativas no domínio. Dessa forma, o usuário de ramificação pode ser delegada a capacidade de gerenciar com eficiência o RODC na filial, sem comprometer a segurança do resto do domínio ou floresta.  
  
## <a name="wan-link-availability"></a>Disponibilidade de link de WAN

Links WAN que enfrentar interrupções frequentes que pode causar a perda de produtividade significativa para os usuários se o local não inclui um controlador de domínio que pode autenticar os usuários. Se a disponibilidade do seu link de WAN não é 100 por cento e seus locais remotos não podem tolerar uma interrupção de serviço, coloque um controlador de domínio regional em locais onde os usuários precisam ter a capacidade de fazer logon ou o acesso ao exchange server quando o link WAN está inativo.  
  
## <a name="authentication-availability"></a>Disponibilidade de autenticação

Determinadas organizações, como bancos, exigem que os usuários sejam autenticadas em todos os momentos. Colocar um controlador de domínio regional em um local onde a disponibilidade de link de WAN não é 100 por cento, mas os usuários precisam de autenticação em todos os momentos.  
  
## <a name="logon-performance-over-wan-links"></a>Desempenho de logon em links de WAN

Se a disponibilidade do seu link de WAN for altamente confiável, colocar um controlador de domínio no local depende dos requisitos de desempenho de logon pelo link WAN. Fatores que influenciam o desempenho de logon na WAN incluem a velocidade do link e largura de banda disponível, número de usuários e perfis de uso e a quantidade de tráfego de rede de logon em comparação com o tráfego de replicação.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Utilização de largura de banda e a velocidade do link WAN

As atividades de um único usuário podem congestionar um link WAN lento. Coloque um controlador de domínio em um local se o desempenho de logon pelo link WAN é inaceitável.  
  
A porcentagem média de utilização de largura de banda indica congestionada como um link de rede. Se um link de rede tem a utilização de largura de banda média maior que um valor aceitável, coloque um controlador de domínio nesse local.  
  
### <a name="number-of-users-and-usage-profiles"></a>Número de usuários e perfis de uso

O número de usuários e seus perfis de uso em um determinado local pode ajudar a determinar se é necessário colocar controladores de domínio regional nesse local. Para evitar a perda de produtividade se um link WAN falhar, coloque um controlador de domínio regional em um local que tem 100 ou mais usuários.  
  
Os perfis de uso indicam como os usuários usam os recursos de rede. Você não precisa colocar um controlador de domínio em um local que contém apenas alguns usuários que não acessam recursos de rede com frequência.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Tráfego de rede de logon versus o tráfego de replicação

Se um controlador de domínio não está disponível no mesmo local que o cliente do Active Directory, o cliente cria o tráfego de logon na rede. A quantidade de tráfego de rede de logon é criado na rede física é influenciada por vários fatores, incluindo associações de grupo; número e tamanho dos objetos de diretiva de grupo (GPOs); scripts de logon; e recursos como pastas offline, redirecionamento de pasta e perfis móveis.  
  
Por outro lado, um controlador de domínio que é colocado em um determinado local gera o tráfego de replicação na rede. A frequência e a quantidade de atualizações feitas nas partições hospedadas nos controladores de domínio influenciam a quantidade de tráfego de replicação que é criado na rede. Os diferentes tipos de atualizações que podem ser feitas sobre as partições hospedadas nos controladores de domínio incluem adicionando ou alterando os usuários e atributos de usuário, alteração de senhas e adicionando ou alterando os grupos globais, impressoras ou volumes.  
  
Para determinar se você precisa colocar um controlador de domínio regional em um local, compare o custo de tráfego de logon criado por um local sem um controlador de domínio e o custo de tráfego de replicação criado colocando um controlador de domínio no local.  
  
Por exemplo, considere uma rede que tenha filiais conectados por meio de links lentos à sede e em quais controladores de domínio podem ser facilmente adicionados. Se o tráfego diário de pesquisa de logon e o diretório de alguns usuários do site remoto faz com que mais tráfego de rede que a replicação de todos os dados da empresa para a ramificação, considere adicionar um controlador de domínio para a ramificação.  
  
Se for mais importante do que o tráfego de rede reduzindo o custo de manter os controladores de domínio, centralizar os controladores de domínio para esse domínio e não coloque os controladores de domínio regional no local ou considere a colocação de RODCs no local.  
  
Para uma planilha ajudar a documentar o posicionamento de controladores de domínio regional e o número de usuários para cada domínio que é representado em cada local, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abra "controlador de posicionamento de domínio" (DSSTOPO_4.doc).  
  
Você precisará consultar as informações sobre os locais em que você precisa colocar os controladores de domínio regional quando você implantar domínios regionais. Para obter mais informações sobre como implantar domínios regionais, consulte [implantar o Windows Server 2008 domínios regionais](https://technet.microsoft.com/library/cc755118.aspx).  
