---
title: Cenários de política de QoS
description: Este tópico fornece cenários de política de QoS (qualidade de serviço), que demonstram como usar Política de Grupo para priorizar o tráfego de rede de aplicativos e serviços específicos no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ac5ab31db1b8c184fd179ecb3e6b87f7fffd2ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405234"
---
# <a name="qos-policy-scenarios"></a>Cenários de política de QoS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para examinar os cenários hipotéticos que demonstram como, quando e por que usar a política de QoS.

Os dois cenários neste tópico são:

1. Priorize o tráfego de rede para um aplicativo de linha de negócios
2. Priorizar o tráfego de rede para um aplicativo de servidor HTTP

>[!NOTE]
>Algumas seções deste tópico contêm etapas gerais que você pode seguir para executar as ações descritas. Para obter instruções mais detalhadas sobre como gerenciar a política de QoS, consulte [gerenciar política de QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Cenário 1: Priorize o tráfego de rede para um aplicativo de linha de negócios

Nesse cenário, um departamento de ti tem várias metas que podem realizar usando a política de QoS:

- Fornecer melhor desempenho de rede para\-aplicativos de missão crítica.
- Fornecer melhor desempenho de rede para um conjunto de chaves de usuários enquanto eles estão usando um aplicativo específico.
- Verifique se o aplicativo\-de backup de dados de toda a empresa não impede o desempenho da rede usando muita largura de banda ao mesmo tempo.

O departamento de ti decide configurar a política de QoS para priorizar aplicativos específicos usando valores DSCP \(\) de ponto de código de serviço de diferenciação para classificar o tráfego de rede e configurar seus roteadores para fornecer uma referência preferencial tratamento para tráfego de prioridade mais alta. 

>[!NOTE]
>Para obter mais informações sobre o DSCP, consulte a seção **definir a prioridade de QoS por meio de um ponto de serviços diferenciados de código** no tópico [política de QoS (qualidade de serviço)](qos-policy-top.md).

Além dos valores DSCP, as políticas de QoS podem especificar uma taxa de limitação. A limitação tem o efeito de limitar todo o tráfego de saída que corresponde à política de QoS a uma taxa de envio específica.

### <a name="qos-policy-configuration"></a>Configuração da política de QoS

Com três metas separadas a serem realizadas, o administrador de ti decide criar três políticas de QoS diferentes.

#### <a name="qos-policy-for-lob-app-servers"></a>Política de QoS para servidores de aplicativos LOB

O primeiro\-aplicativo essencial para o qual o departamento de ti cria uma política de QoS é\-um aplicativo ERP\) de \(planejamento de recursos empresariais em toda a empresa. O aplicativo ERP é hospedado em vários computadores que estão executando o Windows Server 2016. Em Active Directory Domain Services, esses computadores são \(membros de uma UO\) de unidade organizacional que foi criada para servidores de aplicativos LOB \(\) de linha de negócios. O componente\-do lado do cliente para o aplicativo ERP é instalado em computadores que executam o Windows 10 e o Windows 8.1.

No política de grupo, um administrador de ti seleciona o GPO \(\) do objeto política de grupo no qual a política de QoS será aplicada. Usando o assistente de política de QoS, o administrador de ti cria uma política de QoS chamada "política de LOB de servidor\-" que especifica um valor DSCP de alta prioridade de 44 para todos os aplicativos, qualquer endereço IP, TCP e UDP e número de porta.

A política de QoS é aplicada somente aos servidores LOB vinculando o GPO à UO que contém apenas esses servidores, por meio da ferramenta \(GPMC\) console de gerenciamento de política de grupo. Essa política de LOB de servidor inicial aplica\-o valor de DSCP de alta prioridade sempre que o computador envia o tráfego de rede. Essa política de QoS pode ser editada \(posteriormente na ferramenta\) de editor de objeto de política de grupo para incluir os números de porta do aplicativo ERP, o que limita a política a ser aplicada somente quando o número da porta especificada é usado.

#### <a name="qos-policy-for-the-finance-group"></a>Política de QoS para o grupo financeiro

Embora muitos grupos dentro da empresa acessem o aplicativo ERP, o grupo Finance depende desse aplicativo ao lidar com os clientes, e o grupo exige um alto desempenho consistente do aplicativo.

Para garantir que o grupo de finanças possa dar suporte a seus clientes, a política de QoS deve classificar o tráfego dos usuários como prioridade alta. No entanto, a política não deve se aplicar quando os membros do grupo Finanças usam aplicativos diferentes do aplicativo ERP. 

Por isso, o departamento de ti define uma segunda política de QoS chamada "política de LOB de cliente" na ferramenta de Editor de Objeto de Política de Grupo que aplica um valor de DSCP de 60 quando o grupo de usuários de finanças executa o aplicativo ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Política de QoS para um aplicativo de backup

Um aplicativo de backup separado está em execução em todos os computadores. Para garantir que o tráfego do aplicativo de backup não use todos os recursos de rede disponíveis, o departamento de ti cria uma política de dados de backup. Essa política de backup especifica um valor DSCP de 1 com base no nome do executável para o aplicativo de backup, que é **backup. exe**. 

Um terceiro GPO é criado e implantado para todos os computadores cliente no domínio. Sempre que o aplicativo de backup envia dados, o valor DSCP de baixa prioridade é aplicado, mesmo que seja originado de computadores no departamento financeiro.
  
>[!NOTE]
>O tráfego de rede sem uma política de QoS é enviado com um valor DSCP de 0.

### <a name="scenario-policies"></a>Políticas de cenário

A tabela a seguir resume as políticas de QoS para esse cenário.
  
|Nome da política|Valor de DSCP|Taxa de limitação|Aplicado a unidades organizacionais|Descrição|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Nenhuma política]|0|Nenhuma|[Sem implantação]|Tratamento de melhor esforço (padrão) para tráfego não classificado.|  
|Dados de backup|1|Nenhuma|Todos os clientes|Aplica um valor DSCP de baixa prioridade para esses dados em massa.|  
|LOB do servidor|44|Nenhuma|UO do computador para servidores ERP|Aplica DSCP de alta prioridade para o tráfego do servidor ERP|  
|LOB do cliente|60|Nenhuma|Grupo de usuários de finanças|Aplica DSCP de alta prioridade para o tráfego do cliente ERP|  

>[!NOTE]
>Os valores de DSCP são representados em formato decimal.

Com as políticas de QoS definidas e aplicadas usando Política de Grupo, o tráfego de rede de saída recebe o valor DSCP especificado pela política. Em seguida, os roteadores fornecem tratamento diferencial com base nesses valores de DSCP usando o enfileiramento. Para esse departamento de ti, os roteadores são configurados com quatro filas: alta prioridade, prioridade intermediária, melhor esforço e baixa prioridade.

Quando o tráfego chega ao roteador com valores de DSCP de "política de LOB do servidor" e "política de LOB do cliente", os dados são colocados em filas de alta prioridade. O tráfego com um valor DSCP de 0 recebe um nível de serviço de melhor esforço. Os pacotes com um valor DSCP de 1 (do aplicativo de backup) recebem tratamento de baixa prioridade.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Pré-requisitos para priorizar um aplicativo de linha de negócios

Para concluir essa tarefa, verifique se você atende aos seguintes requisitos:

- Os computadores envolvidos estão executando sistemas\-operacionais compatíveis com QoS.

- Os computadores envolvidos são membros de um Active Directory Domain Services \(AD DS\) domínio para que possam ser configurados usando política de grupo.

- As redes TCP/IP são configuradas com roteadores configurados\)para DSCP \(RFC 2474. Para obter mais informações, consulte [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Os requisitos de credenciais administrativas são atendidos.

#### <a name="administrative-credentials"></a>Credenciais administrativas

Para concluir essa tarefa, você deve ser capaz de criar e implantar objetos Política de Grupo.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurando o ambiente de teste para priorizar um aplicativo de linha de negócios

Para configurar o ambiente de teste, conclua as tarefas a seguir.

- Crie um domínio AD DS com clientes e usuários agrupados em unidades organizacionais. Para obter instruções sobre como implantar AD DS, consulte o [Guia de rede principal](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configure os roteadores para filas diferenciais com base em valores DSCP. Por exemplo, o valor de DSCP 44 entra em uma fila "Platina" e todos os outros são ponderados-Fair-Queue.

>[!NOTE]
> Você pode exibir valores de DSCP usando capturas de rede com ferramentas como Monitor de Rede. Depois de executar uma captura de rede, você pode observar o campo TOS em dados capturados.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Etapas para priorizar um aplicativo de linha de negócios

Para priorizar um aplicativo de linha de negócios, conclua as seguintes tarefas:

1. Crie e vincule um GPO \(\) de objeto de política de grupo com uma política de QoS.

2. Configure os roteadores para tratar diferencialmente de um aplicativo de linha de negócios (usando o enfileiramento) com base nos valores DSCP selecionados. Os procedimentos dessa tarefa irão variar dependendo do tipo de roteadores que você tem.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Cenário 2: Priorizar o tráfego de rede para um aplicativo de servidor HTTP

No Windows Server 2016, a QoS baseada em políticas inclui as políticas baseadas em URL do recurso. As políticas de URL permitem que você gerencie a largura de banda para servidores HTTP.

Muitos aplicativos empresariais são desenvolvidos para o e hospedados \(em\) serviços de informações da Internet servidores Web do IIS, e os aplicativos Web são acessados de navegadores em computadores cliente.

Nesse cenário, suponha que você gerencie um conjunto de servidores IIS que hospeda vídeos de treinamento para todos os funcionários da sua organização. Seu objetivo é garantir que o tráfego desses servidores de vídeo não sobrecarregue sua rede e garanta que o tráfego de vídeo seja diferenciado da voz e do tráfego de dados na rede. 

A tarefa é semelhante à tarefa no cenário 1. Você criará e configurará as configurações de gerenciamento de tráfego, como o valor de DSCP para o tráfego de vídeo, e a taxa de limitação da mesma forma que faria para os aplicativos de linha de negócios. Mas ao especificar o tráfego, em vez de fornecer o nome do aplicativo, você só insere a URL para a qual seu aplicativo de servidor HTTP responderá https://hrweb/training: por exemplo,.
  
> [!NOTE]
>Você não pode usar políticas de QoS baseadas em URL para priorizar o tráfego de rede para computadores que executam sistemas operacionais Windows que foram lançados antes do Windows 7 e do Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Regras de precedência para políticas baseadas em URL

Todas as URLs a seguir são válidas e podem ser especificadas na política de QoS e aplicadas simultaneamente a um computador ou a um usuário:

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

Mas qual deles receberá precedência? As regras são simples. As políticas baseadas em URL são priorizadas em uma ordem de leitura da esquerda para a direita. Portanto, da prioridade mais alta para a prioridade mais baixa, os campos de URL são:
  
[1. Esquema de URL](#bkmk_QoS_UrlScheme)

[2. Host de URL](#bkmk_QoS_UrlHost)

[3. Porta da URL](#bkmk_QoS_UrlPort)

[4. Caminho da URL](#bkmk_QoS_UrlPath)

Os detalhes são os seguintes:

####  <a name="bkmk_QoS_UrlScheme"></a>uma. Esquema de URL

 `https://`tem uma prioridade mais alta `https://`do que.

####  <a name="bkmk_QoS_UrlHost"></a>2. Host de URL

 Da prioridade mais alta para a mais baixa, elas são:

1. nome_do_host

2. Endereço IPv6

3. Endereço IPv4

4. Curinga

No caso do nome do host, um nome de host com mais elementos pontilhados (mais profundidade) tem uma prioridade mais alta do que um nome de host com menos elementos pontilhados. Por exemplo, entre os nomes de host a seguir:

- video.internal.training.hr.mycompany.com (profundidade = 6)
  
- selfguide.training.mycompany.com (profundidade = 4)
  
- treinamento (profundidade = 1)
  
- biblioteca (profundidade = 1)
  
  **video.Internal.Training.hr.mycompany.com** tem a prioridade mais alta e **selfguide.Training.mycompany.com** tem a próxima prioridade mais alta. O **treinamento** e a **biblioteca** compartilham a mesma prioridade mais baixa.  
  
####  <a name="bkmk_QoS_UrlPort"></a>Beta. Porta da URL

Um número de porta específico ou implícito tem uma prioridade mais alta do que uma porta curinga.

####  <a name="bkmk_QoS_UrlPath"></a>quatro. Caminho da URL

Como um nome de host, um caminho de URL pode consistir em vários elementos. A única com mais elementos sempre tem uma prioridade mais alta do que aquela com menos. Por exemplo, os seguintes caminhos são listados por prioridade:  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

Se um usuário optar por incluir todos os subdiretórios e arquivos após um caminho de URL, esse caminho de URL terá uma prioridade mais baixa do que teria se a opção não fosse feita.

Um usuário também pode optar por especificar um endereço IP de destino em uma política baseada em URL. O endereço IP de destino tem uma prioridade mais baixa do que qualquer um dos quatro campos de URL descritos anteriormente.
  
### <a name="quintuple-policy"></a>Política de quintuple

Uma política de quintuple é especificada por ID de protocolo, endereço IP de origem, porta de origem, endereço IP de destino e porta de destino. Uma política quintuple sempre tem uma precedência mais alta do que qualquer política baseada em URL. 

Se uma política de quintuple já estiver aplicada a um usuário, uma nova política baseada em URL não causará conflitos em nenhum dos computadores cliente do usuário.

Para o próximo tópico deste guia, consulte [gerenciar política de QoS](qos-policy-manage.md).

Para o primeiro tópico deste guia, consulte [política de QoS (qualidade de serviço)](qos-policy-top.md).
