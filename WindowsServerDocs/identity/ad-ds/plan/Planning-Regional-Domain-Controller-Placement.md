---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Planejando o posicionamento do controlador de domínio regional
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 79c6fd5f4b0cefdb746b37a3421ee28b385171d6
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938626"
---
# <a name="planning-regional-domain-controller-placement"></a>Planejando o posicionamento do controlador de domínio regional

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para garantir a eficiência de custos, planeje o posicionamento do máximo possível de controladores de domínio regionais. Primeiro, examine a planilha "locais geográficos e links de comunicação" (DSSTOPO_1.doc) usada na [coleta de informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) para determinar se um local é um Hub.

Planeje o posicionamento de controladores de domínio regionais para cada domínio representado em cada local do Hub. Depois de colocar os controladores de domínio regionais em todos os locais de Hub, avalie a necessidade de colocar controladores de domínio regionais em locais satélite. A eliminação de controladores de domínio regionais desnecessários de locais de satélite reduz os custos de suporte necessários para manter uma infraestrutura de servidor remoto.

Além disso, garanta a segurança física dos controladores de domínio em locais de Hub e satélite para que a equipe não autorizada não possa acessá-los. Não coloque controladores de domínio graváveis em locais de Hub e satélite nos quais você não pode garantir a segurança física do controlador de domínio. Uma pessoa que tem acesso físico a um controlador de domínio gravável pode atacar o sistema:

- Acessando discos físicos iniciando um sistema operacional alternativo em um controlador de domínio.
- Removendo (e possivelmente substituindo) discos físicos em um controlador de domínio.
- Obtenção e manipulação de uma cópia de um backup de estado do sistema do controlador de domínio.

Adicione controladores de domínio regionais graváveis somente a locais nos quais você pode garantir sua segurança física.

Em locais com segurança física inadequada, a implantação de um RODC (controlador de domínio somente leitura) é a solução recomendada. Exceto para senhas de contas, um RODC mantém todos os objetos Active Directory e atributos que um controlador de domínio gravável mantém. No entanto, as alterações não podem ser feitas no banco de dados armazenado no RODC. As alterações devem ser feitas em um controlador de domínio gravável e, em seguida, replicadas de volta para o RODC.

Para autenticar logons do cliente e o acesso a servidores de arquivos locais, a maioria das organizações coloca controladores de domínio regionais para todos os domínios regionais que são representados em um determinado local. No entanto, você deve considerar muitas variáveis ao avaliar se um local de negócios exige que seus clientes tenham autenticação local ou que os clientes possam contar com a autenticação e a consulta em um link de WAN (rede de longa distância). A ilustração a seguir mostra como determinar se os controladores de domínio devem ser colocados em locais satélite.

![planejar o posicionamento do DC regional](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)

## <a name="onsite-technical-expertise-availability"></a>Disponibilidade de experiência técnica no local

Os controladores de domínio precisam ser gerenciados continuamente por vários motivos. Coloque um controlador de domínio regional somente em locais que incluam pessoal que possa administrar o controlador de domínio ou verifique se o controlador de domínio pode ser gerenciado remotamente.

Em ambientes de filiais com pouca segurança física e pessoal com pouco conhecimento de tecnologia da informação, a implantação de um RODC é geralmente a solução recomendada. Permissões administrativas locais para um RODC podem ser delegadas a qualquer usuário de domínio sem conceder a esse usuário qualquer direito de usuário para o domínio ou outros controladores de domínio. Isso permite que um usuário de Branch local faça logon em um RODC e execute o trabalho de manutenção no servidor, como atualizar um driver. No entanto, o usuário da filial não pode fazer logon em nenhum outro controlador de domínio nem executar outra tarefa administrativa no domínio. Dessa forma, o usuário da ramificação pode ser delegado a capacidade de gerenciar efetivamente o RODC na filial sem comprometer a segurança do restante do domínio ou da floresta.

## <a name="wan-link-availability"></a>Disponibilidade do link de WAN

Links de WAN que experimentam interrupções frequentes podem causar perda de produtividade significativa para os usuários se o local não incluir um controlador de domínio que possa autenticar os usuários. Se a sua disponibilidade de link de WAN não for de 100% e seus sites remotos não puderem tolerar uma interrupção de serviço, coloque um controlador de domínio regional em locais onde os usuários precisem da capacidade de fazer logon ou do acesso ao Exchange Server quando o link de WAN estiver inativo.

## <a name="authentication-availability"></a>Disponibilidade de autenticação

Determinadas organizações, como bancos, exigem que os usuários sejam autenticados em todos os momentos. Coloque um controlador de domínio regional em um local em que a disponibilidade do link de WAN não seja de 100%, mas os usuários exigem autenticação o tempo todo.

## <a name="logon-performance-over-wan-links"></a>Desempenho de logon sobre links WAN

Se a sua disponibilidade de link de WAN for altamente confiável, colocar um controlador de domínio no local dependerá dos requisitos de desempenho de logon pelo link de WAN. Os fatores que influenciam o desempenho de logon pela WAN incluem velocidade de link e largura de banda disponível, número de usuários e perfis de uso e a quantidade de tráfego de rede de logon em comparação com o tráfego de replicação.

### <a name="wan-link-speed-and-bandwidth-utilization"></a>Velocidade da conexão WAN e utilização de largura de banda

As atividades de um único usuário podem congestionar um link WAN lento. Coloque um controlador de domínio em um local se o desempenho de logon pelo link de WAN for inaceitável.

A porcentagem média de utilização de largura de banda indica o quanto um link de rede está congestionado. Se um link de rede tiver uma utilização de largura de banda média maior que um valor aceitável, coloque um controlador de domínio nesse local.

### <a name="number-of-users-and-usage-profiles"></a>Número de usuários e perfis de uso

O número de usuários e seus perfis de uso em um determinado local pode ajudar a determinar se você precisa posicionar controladores de domínio regionais nesse local. Para evitar a perda de produtividade se um link de WAN falhar, coloque um controlador de domínio regional em um local que tenha 100 ou mais usuários.

Os perfis de uso indicam como os usuários usam os recursos de rede. Não é necessário posicionar um controlador de domínio em um local que contenha apenas alguns usuários que não acessam os recursos de rede com frequência.

### <a name="logon-network-traffic-vs-replication-traffic"></a>Tráfego de rede de logon versus tráfego de replicação

Se um controlador de domínio não estiver disponível no mesmo local que o cliente do Active Directory, o cliente criará o tráfego de logon na rede. A quantidade de tráfego de rede de logon que é criada na rede física é influenciada por vários fatores, incluindo associações de grupo; número e tamanho de objetos de Política de Grupo (GPOs); scripts de logon; e recursos como pastas offline, redirecionamento de pasta e perfis de roaming.

Por outro lado, um controlador de domínio que é colocado em um determinado local gera o tráfego de replicação na rede. A frequência e a quantidade de atualizações feitas nas partições hospedadas nos controladores de domínio influenciam a quantidade de tráfego de replicação que é criada na rede. Os diferentes tipos de atualizações que podem ser feitas nas partições hospedadas nos controladores de domínio incluem adicionar ou alterar usuários e atributos de usuário, alterar senhas e adicionar ou alterar grupos globais, impressoras ou volumes.

Para determinar se você precisa colocar um controlador de domínio regional em um local, compare o custo do tráfego de logon criado por um local sem um controlador de domínio versus o custo do tráfego de replicação criado colocando um controlador de domínio no local.

Por exemplo, considere uma rede que tem filiais conectadas por meio de links lentos para a sede e em que controladores de domínio podem ser facilmente adicionados. Se o logon diário e o tráfego de pesquisa de diretório de alguns usuários do site remoto fizerem mais tráfego de rede do que replicar todos os dados da empresa para a ramificação, considere adicionar um controlador de domínio à ramificação.

Se a redução do custo de manutenção de controladores de domínio for mais importante do que o tráfego de rede, centralize os controladores de domínio para esse domínio e não coloque nenhum controlador de domínio regional no local ou considere colocar os RODCs no local.

Para uma planilha para ajudá-lo a documentar o posicionamento dos controladores de domínio regionais e o número de usuários para cada domínio representado em cada local, consulte [auxílios de trabalho para o Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abrir o "posicionamento do controlador de domínio" (DSSTOPO_4.doc).

Você precisará consultar as informações sobre os locais nos quais você precisa inserir controladores de domínio regionais ao implantar domínios regionais. Para obter mais informações sobre a implantação de domínios regionais, consulte [implantando domínios regionais do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10)).
