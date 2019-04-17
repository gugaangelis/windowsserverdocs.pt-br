---
title: Cenários de política de QoS
description: Este tópico fornece cenários de política de qualidade de serviço (QoS), que demonstram como usar a política de grupo para priorizar o tráfego de rede de aplicativos específicos e serviços no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b635f73cc25552f4c1a05f8db6303f636eda3c54
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-scenarios"></a>Cenários de política de QoS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para examinar hipotéticos cenários que demonstram como, quando e por que usar a política de QoS.

Os dois cenários neste tópico são:

1. Priorizar o tráfego de rede para um aplicativo de linha de negócios
2. Priorizar o tráfego de rede para um aplicativo de servidor HTTP

>[!NOTE]
>Algumas seções deste tópico contêm as etapas gerais que você pode tomar para executar as ações descritas. Para obter instruções sobre como gerenciar diretiva de QoS mais detalhadas, consulte [gerenciar diretiva de QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Cenário 1: Priorizar o tráfego de rede para um aplicativo de linha de negócios

Nesse cenário, um departamento de TI tem vários objetivos que pode ser feito usando a política de QoS:

- Fornece melhor desempenho de rede para aplicativos críticos de mission\.
- Fornece melhor desempenho de rede para um conjunto de chaves de usuários enquanto eles estiverem usando um aplicativo específico.
- Certifique-se de que os dados de company\ todo aplicativo de Backup não impedem o desempenho de rede usando muita largura de banda de uma só vez.

O departamento de TI decide configurar a política de QoS para priorizar aplicativos específicos, usando valores de ponto de código de serviço de diferenciação de \(DSCP\) para classificar o tráfego de rede e para configurar seus roteadores para fornecer tratamento preferencial para o tráfego de prioridade mais alto. 

>[!NOTE]
>Para obter mais informações sobre DSCP, consulte a seção **definir QoS prioridade por meio de um diferenciados serviços de ponto de código** no tópico [política de qualidade de serviço (QoS)](qos-policy-top.md).

Além dos valores DSCP, políticas de QoS podem especificar uma taxa de aceleração. Otimização tem o efeito de limitar todo o tráfego de saída que corresponde a política de QoS para uma determinada taxa de envio.

### <a name="qos-policy-configuration"></a>Configuração de política de QoS

Com três metas separadas para realizar, o administrador de TI decide criar três diferentes políticas de QoS.

#### <a name="qos-policy-for-lob-app-servers"></a>Política de QoS para servidores de aplicativos LOB

O primeiro aplicativo mission\ essenciais para os quais o departamento de TI cria uma política de QoS é um recurso de Enterprise company\ todo aplicativo \(ERP\) de planejamento. O aplicativo ERP é hospedado em vários computadores que executam Windows Server 2016. Nos serviços de domínio do Active Directory, esses computadores são membros de uma unidade organizacional \(OU\) que foi criado para servidores de aplicativos de \(LOB\) de linha de negócios. O componente client\ lado para o aplicativo ERP está instalado nos computadores que executam o Windows 10 e Windows 8.1.

Na política de grupo, um administrador de TI seleciona \(GPO\) o objeto de política de grupo no qual a política de QoS será aplicada. Usando o Assistente de política de QoS, o administrador de TI cria uma política de QoS chamada "servidor LOB política" que especifica um valor DSCP high\ prioridade de 44 para todos os aplicativos, qualquer endereço IP, TCP e UDP e número da porta.

A política de QoS é aplicada somente para os servidores LOB vinculando o GPO para a unidade Organizacional que contém somente esses servidores, por meio da ferramenta Console de gerenciamento de política de grupo \(GPMC\). Este servidor inicial LOB política aplica-se o valor DSCP high\ prioridade sempre que o computador envia o tráfego de rede. Esta configuração de política de QoS posterior pode ser editada \ (em tool\ o Editor de objeto de política de grupo) para incluir os números de porta do aplicativo ERP, que limita a política para aplicar somente quando o número da porta especificado é usado.

#### <a name="qos-policy-for-the-finance-group"></a>Política de QoS para o grupo de finanças

Enquanto vários grupos dentro da empresa acessem o aplicativo ERP, o grupo de finanças depende deste aplicativo ao lidar com os clientes, e o grupo requer consistentemente alto desempenho do aplicativo.

Para garantir que o grupo de Finanças pode dar suporte a seus clientes, a política de QoS deve classificar o tráfego desses usuários prioridade alta. No entanto, a política não deve ser aplicada ao membros do grupo Finanças usam aplicativos que não seja o aplicativo ERP. 

Por isso, o departamento de TI define uma política de QoS segunda chamada "cliente LOB política" na ferramenta do Editor de objeto de política de grupo que se aplica a um valor de 60 DSCP quando o grupo de usuários de finanças executa o aplicativo ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Política de QoS para um aplicativo de Backup

Um aplicativo separado de backup está em execução em todos os computadores. Para garantir que o tráfego do aplicativo de backup não usa todos os recursos de rede disponíveis, o departamento de TI cria uma política de dados de backup. Esta configuração de política backup Especifica um valor DSCP baseado no nome do executável do aplicativo de backup, que é de 1 **backup.exe **. 

Um terceiro GPO é criado e implantado para todos os computadores cliente no domínio. Sempre que o aplicativo de backup envia dados, o valor DSCP de baixa prioridade é aplicado, mesmo se ela se origina de computadores do departamento de finanças.
  
>[!NOTE]
>O tráfego de rede sem uma política de QoS envia com um valor DSCP de 0.

### <a name="scenario-policies"></a>Políticas de cenário

A tabela a seguir resume as políticas de QoS para esse cenário.
  
|Nome da política|Valor DSCP|Taxa de aceleração|Aplicada a unidades organizacionais|Descrição|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Nenhuma política]|0|None|[Nenhuma implantação]|Tratamento de melhor esforço (padrão) para o tráfego não classificado.|  
|Dados de backup|1|None|Todos os clientes|Aplica-se um valor DSCP de baixa prioridade para esses dados em massa.|  
|Servidor LOB|44|None|Computador OU para servidores ERP|Aplica-se DSCP de alta prioridade para o tráfego de servidor ERP|  
|Cliente LOB|60|None|Grupo de usuários de finanças|Aplica-se DSCP de alta prioridade para o tráfego de cliente ERP|  

>[!NOTE]
>Valores DSCP são representados em formato decimal.

Com as políticas de QoS definidas e aplicadas usando-se política de grupo, o tráfego de rede de saída recebe o valor DSCP especificado pela política. Roteadores fornecem tratamento diferencial com base nesses valores DSCP, usando o enfileiramento de mensagens. Para o departamento de TI, os roteadores estão configurados com quatro filas: alta prioridade, middle prioridade, melhor esforço e baixa prioridade.

Quando chega tráfego no roteador com valores DSCP de "servidor LOB política" e "cliente LOB política," os dados são colocados em filas de alta prioridade. O tráfego com um valor de 0 DSCP recebe um nível de melhor esforço de serviço. Pacotes com um valor de 1 (do aplicativo de backup) DSCP recebem tratamento de baixa prioridade.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Pré-requisitos para priorizar a um aplicativo de linha de negócios

Para concluir essa tarefa, certifique-se de que você atende aos seguintes requisitos:

- Os computadores envolvidos com QoS\ compatível com sistemas de operacionais.

- Os computadores envolvidos são membros de um domínio do Active Directory Domain Services \(AD DS\) para que eles podem ser configurados usando a política de grupo.

- Redes TCP/IP são configurados com roteadores configurados para DSCP \(RFC 2474\). Para obter mais informações, consulte [RFC 2474](http://www.ietf.org/rfc/rfc2474.txt).

- Requisitos de credenciais administrativas são atendidos.

#### <a name="administrative-credentials"></a>Credenciais administrativas

Para concluir essa tarefa, você deve ser capaz de criar e implantar objetos de política de grupo.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurando o ambiente de teste para priorizar a um aplicativo de linha de negócios

Para configurar o ambiente de teste, conclua as seguintes tarefas.

- Crie um domínio do AD DS com clientes e usuários agrupados em unidades organizacionais. Para obter instruções sobre como implantar o AD DS, consulte o [guia da rede principal ](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configure os roteadores differentially fila com base nos valores DSCP. Por exemplo, valor DSCP 44 insere uma fila "Platina" e todos os outros são ponderada-fair-na fila.

>[!NOTE]
> Você pode visualizar os valores DSCP usando capturas de rede com ferramentas como o Monitor de rede. Depois que você executar uma captura de rede, você pode observar o campo de instruções em dados capturados.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Etapas para a prioridade de um aplicativo de linha de negócios

Para priorizar a um aplicativo de linha de negócios, conclua as seguintes tarefas:

1. Crie e vincule \(GPO\) um objeto de política de grupo com uma política de QoS.

2. Configure os roteadores para tratar differentially um linha de negócios application (usando enfileiramento de mensagens) com base nos valores DSCP selecionados. Os procedimentos desta tarefa varia de acordo com o tipo dos roteadores que você tem.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Cenário 2: Priorizar o tráfego de rede para um aplicativo de servidor HTTP

No Windows Server 2016, com base em política QoS inclui o recurso políticas baseadas na URL. Políticas de URL permitem gerenciar largura de banda para servidores HTTP.

Muitos aplicativos corporativos são desenvolvidos para e hospedados em servidores de web do Internet Information Services \(IIS\), e os aplicativos da Web são acessados de navegadores em computadores cliente.

Nesse cenário, pressupomos gerenciar um conjunto de servidores do IIS que vídeos de treinamento de host para todos os seus funcionários. Seu objetivo é garantir que o tráfego desses servidores de vídeo não sobrecarregue sua rede e certifique-se de que o tráfego de vídeo é diferenciado de voz e dados de tráfego na rede. 

A tarefa é semelhante à tarefa no cenário 1. Você vai projetar e definir as configurações de gerenciamento de tráfego, como o valor DSCP para o tráfego de vídeo, e a otimização de classificar o mesmo que seguiria para os aplicativos de linha de negócios. Mas ao especificar o tráfego, em vez de fornecer o nome do aplicativo, você só pode inserir a URL para o qual seu aplicativo de servidor HTTP responderá: por exemplo,https://hrweb/training.
  
> [!NOTE]
>Você não pode usar políticas de QoS baseada na URL para priorizar o tráfego de rede para computadores que executam sistemas operacionais Windows que foram lançados antes do Windows 7 e Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Regras de precedência de políticas de URL

Todas as URLs a seguir são válidas e podem ser especificados na política de QoS e simultaneamente aplicados a um computador ou um usuário:

- http://video

- https://training.hr.mycompany.com

- http://10.1.10.249:8080/tech  

- https://*/ebooks

Mas qual deles receberá precedência? As regras são simples. Políticas de URL são priorizadas na ordem de leitura da esquerda para a direita. Assim, de maior prioridade para a prioridade mais baixa, os campos de URL são:
  
[1. Esquema de URL](#bkmk_QoS_UrlScheme)

[2. Host de URL](#bkmk_QoS_UrlHost)

[3. Porta de URL](#bkmk_QoS_UrlPort)

[4. Caminho da URL](#bkmk_QoS_UrlPath)

Detalhes são as seguintes:

####  <a name="bkmk_QoS_UrlScheme"></a>1. esquema de URL

 `https://` Tem uma prioridade maior do que `http://`.

####  <a name="bkmk_QoS_UrlHost"></a>2. host de URL

 Da prioridade mais alta para o menor, eles são:

1. Nome do host

2. Endereço IPv6

3. Endereço IPv4

4. Curinga

No caso de nome de host, um nome de host com elementos mais pontilhados (mais profundamente) tem uma prioridade maior do que um nome de host com menos elementos pontilhados. Por exemplo, entre o nome de host seguinte:

- video.internal.training.hr.mycompany.com (profundidade = 6)
  
- selfguide.training.mycompany.com (profundidade = 4)
  
- Treinamento (profundidade = 1)
  
- Biblioteca (profundidade = 1)
  
 **video.internal.training.hr.mycompany.com** tem a maior prioridade, e **selfguide.training.mycompany.com** tem a prioridade mais alta. **Treinamento** e **biblioteca** compartilham a mesma prioridade mais baixa.  
  
####  <a name="bkmk_QoS_UrlPort"></a>3. porta de URL

Uma versão específica ou um número de porta implícito tem uma prioridade maior do que uma porta curinga.

####  <a name="bkmk_QoS_UrlPath"></a>4. caminho da URL

Como um nome de host, um caminho de URL pode consistir em vários elementos. Aquele com mais elementos sempre tem uma prioridade maior do que aquele com menos. Por exemplo, os caminhos a seguir estão listados por prioridade:  

1.  /eBooks/Tech/Windows/Networking/QoS

2.  / livros eletrônicos/técnico/windows /

3.  /ebooks

4.  /

Se um usuário optar por incluir todos os subdiretórios e arquivos de um caminho de URL a seguir, esse caminho de URL terá uma prioridade mais baixa do que teria se a opção não foram feita.

Um usuário também pode optar por especificar um endereço IP de destino em uma política com base em URL. O endereço IP de destino tem uma prioridade mais baixa do que qualquer um dos quatro campos URL descritos anteriormente.
  
### <a name="quintuple-policy"></a>Política quintuple

Uma política Quintuple é especificada pelo ID do protocolo, endereço IP de origem, porta de origem, endereço IP de destino e porta de destino. Uma política Quintuple sempre tem uma precedência maior que qualquer política de URL. 

Se uma política Quintuple já é aplicada para um usuário, uma nova política com base em URL não causará conflitos em qualquer cliente do usuário computadores.

Para o próximo tópico neste guia, consulte [gerenciar diretiva de QoS](qos-policy-manage.md).

Para o primeiro tópico neste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
