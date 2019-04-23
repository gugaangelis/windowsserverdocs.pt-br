---
title: Cenários de política de QoS
description: Este tópico fornece cenários de política de qualidade de serviço (QoS), que demonstram como usar a diretiva de grupo para priorizar o tráfego de rede de aplicativos e serviços no Windows Server 2016 específicos.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a24ce3213b1aa1c66a438d278f7499b6d53bf6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834347"
---
# <a name="qos-policy-scenarios"></a>Cenários de política de QoS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para examinar cenários hipotéticos que demonstram como fazer isso, quando e por que usar a política de QoS.

Os dois cenários neste tópico são:

1. Priorizar o tráfego de rede para um aplicativo de linha de negócios
2. Priorizar o tráfego de rede para um aplicativo de servidor HTTP

>[!NOTE]
>Algumas seções deste tópico contêm etapas gerais que você pode executar para executar as ações descritas. Para obter mais instruções sobre como gerenciar a política de QoS, consulte [Gerenciar política de QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Cenário 1: Priorizar o tráfego de rede para um aplicativo de linha de negócios

Nesse cenário, um departamento de TI tem várias metas realizadas por meio da política de QoS:

- Fornecer um melhor desempenho de rede para a missão\-aplicativos críticos.
- Fornece um melhor desempenho de rede para um conjunto de chaves dos usuários enquanto eles estão usando um aplicativo específico.
- Certifique-se de que a empresa\-dados para todo o aplicativo de Backup não prejudicam o desempenho de rede usando muita largura de banda ao mesmo tempo.

O departamento de TI decide configurar política de QoS para priorizar aplicativos específicos usando o ponto de código do serviço de diferenciação \(DSCP\) valores para classificar o tráfego de rede e para configurar seus roteadores para fornecer preferencial tratamento para o tráfego de prioridade mais alta. 

>[!NOTE]
>Para obter mais informações sobre o DSCP, consulte a seção **definir QoS prioridade por meio de um diferenciados serviços de ponto de código** no tópico [política de qualidade de serviço (QoS)](qos-policy-top.md).

Além dos valores DSCP, políticas de QoS podem especificar uma taxa de aceleração. Limitação tem o efeito de limitar o tráfego de saída que corresponde a política de QoS para uma determinada taxa de envio.

### <a name="qos-policy-configuration"></a>Configuração de política de QoS

Com três objetivos separados para realizar, o administrador de TI decide criar três diferentes políticas de QoS.

#### <a name="qos-policy-for-lob-app-servers"></a>Política de QoS para servidores de aplicativo LOB

A primeira missão\-aplicativos críticos para o qual o departamento de TI cria uma política de QoS é uma empresa\-planejamento de recursos empresariais de grande \(ERP\) aplicativo. O aplicativo de ERP é hospedado em vários computadores que estiverem todas executando o Windows Server 2016. Nos serviços de domínio do Active Directory, esses computadores são membros de uma unidade organizacional \(UO\) que foi criado para a linha de negócios \(LOB\) servidores de aplicativos. O cliente\-componente do lado do aplicativo de ERP é instalado em computadores que executam o Windows 10 e Windows 8.1.

Na diretiva de grupo, um administrador de TI seleciona o objeto de diretiva de grupo \(GPO\) após a qual a política de QoS será aplicada. Usando o Assistente de política de QoS, o administrador de TI cria uma política de QoS chamada "servidor LOB política" que especifica uma alta\-prioridade valor DSCP de 44 para todos os aplicativos, qualquer endereço IP, TCP e UDP e número da porta.

A política de QoS é aplicada somente aos servidores LOB ao vincular o GPO à UO que contém apenas esses servidores, por meio do Console de gerenciamento de diretiva de grupo \(GPMC\) ferramenta. Este servidor inicial LOB política aplica-se a alta\-prioridade DSCP valor sempre que o computador envia o tráfego de rede. Essa política de QoS mais tarde pode ser editada \(na ferramenta do Editor de objeto de diretiva de grupo\) para incluir números de porta do aplicativo de ERP, que limita a política para aplicar somente quando o número da porta especificado é usado.

#### <a name="qos-policy-for-the-finance-group"></a>Política de QoS para o grupo de finanças

Enquanto muitos grupos dentro da empresa acessarem o aplicativo de ERP, o grupo de finanças depende deste aplicativo ao lidar com os clientes e consistentemente alto desempenho do aplicativo requer que o grupo.

Para garantir que o grupo de Finanças pode oferecer suporte a seus clientes, a política de QoS deve classificar o tráfego desses usuários como prioridade alta. No entanto, a política não deve ser aplicada quando os membros do grupo de Finanças de usam aplicativos que não seja o aplicativo de ERP. 

Por isso, o departamento de TI define uma segunda política de QoS chamada "cliente LOB política" na ferramenta do Editor de objeto de diretiva de grupo que se aplica a um valor DSCP de 60 quando o grupo de usuários de finanças executa o aplicativo de ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Política de QoS para um aplicativo de Backup

Um aplicativo de backup separado está em execução em todos os computadores. Para garantir que o tráfego de backup do aplicativo não usa todos os recursos de rede disponível, o departamento de TI cria uma política de backup de dados. Essa política de backup Especifica um valor DSCP igual a 1, com base no nome do executável para o aplicativo de backup, que é **backup.exe**. 

Um terceiro GPO é criado e implantado para todos os computadores cliente no domínio. Sempre que o aplicativo de backup envia dados, o valor DSCP de baixa prioridade é aplicado, mesmo se ele é proveniente de computadores do departamento financeiro.
  
>[!NOTE]
>Envia o tráfego de rede sem uma política de QoS com um valor DSCP igual a 0.

### <a name="scenario-policies"></a>Políticas de cenário

A tabela a seguir resume as políticas de QoS para esse cenário.
  
|Nome da política|Valor DSCP|Taxa de aceleração|Aplicado a unidades organizacionais|Descrição|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Nenhuma política]|0|Nenhuma|[Nenhuma implantação]|Tratamento de melhor esforço (padrão) para o tráfego não classificado.|  
|Dados de backup|1|Nenhuma|Todos os clientes|Aplica-se um valor DSCP de baixa prioridade para esses dados em massa.|  
|Servidor LOB|44|Nenhuma|UO do computador para servidores ERP|Aplica-se alta prioridade DSCP para tráfego do servidor de ERP|  
|Cliente LOB|60|Nenhuma|Grupo de usuários de finanças|Aplica-se alta prioridade DSCP para tráfego de cliente ERP|  

>[!NOTE]
>Valores DSCP são representados no formato decimal.

Com as políticas de QoS definidas e aplicadas usando a diretiva de grupo, o tráfego de rede de saída recebe o valor DSCP política especificada. Roteadores, em seguida, fornecem tratamento diferencial com base nesses valores DSCP, usando o enfileiramento de mensagens. Para esse departamento de TI, os roteadores são configurados com quatro filas: alta prioridade, prioridade intermediária, melhor esforço e baixa prioridade.

Quando o tráfego chega ao roteador com valores DSCP de "servidor LOB política" e "cliente LOB política," os dados são colocados em filas de alta prioridade. O tráfego com um valor DSCP igual a 0 recebe um nível de esforço de serviço. Pacotes com um valor DSCP igual a 1 (do aplicativo de backup) recebem o tratamento de baixa prioridade.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Pré-requisitos para a prioridade de um aplicativo de linha de negócios

Para concluir essa tarefa, certifique-se de que você atenda aos seguintes requisitos:

- Os computadores envolvidos estão executando o QoS\-sistemas operacionais compatíveis.

- Os computadores envolvidos são membros de um Active Directory Domain Services \(AD DS\) domínio para que eles podem ser configurados usando diretiva de grupo.

- Redes TCP/IP estão definidos com roteadores configurados para DSCP \(RFC 2474\). Para obter mais informações, consulte [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Requisitos de credenciais administrativas são atendidos.

#### <a name="administrative-credentials"></a>Credenciais administrativas

Para concluir essa tarefa, você deve ser capaz de criar e implantar objetos de diretiva de grupo.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurando o ambiente de teste para a prioridade de um aplicativo de linha de negócios

Para configurar o ambiente de teste, conclua as seguintes tarefas.

- Crie um domínio AD DS com clientes e usuários agrupados em unidades organizacionais. Para obter instruções sobre como implantar o AD DS, consulte a [guia da rede principal](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configure os roteadores para correção diferencial fila com base nos valores DSCP. Por exemplo, o valor DSCP 44 entra em uma fila de "Platinum" e todas as outras são ponderados fair enfileirados no.

>[!NOTE]
> Você pode exibir os valores DSCP usando capturas de rede com ferramentas como o Monitor de rede. Depois de executar uma captura de rede, você pode observar o campo TOS em dados capturados.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Etapas para a prioridade de um aplicativo de linha de negócios

Para priorizar um aplicativo de linha de negócios, conclua as seguintes tarefas:

1. Criar e vincular um objeto de diretiva de grupo \(GPO\) com uma política de QoS.

2. Configure os roteadores para correção diferencial tratar um line-of-business application (usando o enfileiramento de mensagens) com base nos valores DSCP selecionados. Os procedimentos desta tarefa variam de acordo com o tipo de roteadores que você tem.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Cenário 2: Priorizar o tráfego de rede para um aplicativo de servidor HTTP

No Windows Server 2016, a QoS baseada em política inclui o recurso políticas com base em URL. Diretivas de URL permitem gerenciar largura de banda para servidores HTTP.

Muitos aplicativos corporativos são desenvolvidos para e hospedados nos serviços de informações da Internet \(IIS\) servidores web e os aplicativos Web são acessados por navegadores em computadores cliente.

Nesse cenário, suponha que gerenciar um conjunto de servidores do IIS que vídeos de treinamento do host para todos os funcionários da organização. Seu objetivo é garantir que o tráfego desses servidores de vídeo não sobrecarregar sua rede e certifique-se de que o tráfego de vídeo é diferenciado de tráfego de voz e dados na rede. 

A tarefa é semelhante à tarefa no cenário 1. Você irá criar e configurar as configurações de gerenciamento de tráfego, como o valor DSCP para o tráfego de vídeo, e a limitação de taxa a mesma como você faria para os aplicativos de linha de negócios. Mas, ao especificar o tráfego, em vez de fornecer o nome do aplicativo, você só pode inserir a URL para a qual seu aplicativo para servidores HTTP responderá: por exemplo, https://hrweb/training.
  
> [!NOTE]
>É possível usar as políticas de QoS baseado em URL para priorizar o tráfego de rede para computadores que executam sistemas operacionais Windows que foram lançados antes do Windows 7 e Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Regras de precedência para políticas com base em URL

Todas as URLs a seguir são válidas e podem ser especificadas na política de QoS e aplicadas simultaneamente em um computador ou usuário:

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

Mas qual delas receberá precedência? As regras são simples. Políticas com base em URL são priorizadas em uma ordem de leitura da esquerda para direita. Portanto, da prioridade mais alta para a prioridade mais baixa, os campos de URL são:
  
[1. Esquema de URL](#bkmk_QoS_UrlScheme)

[2. Host da URL](#bkmk_QoS_UrlHost)

[3. Porta da URL](#bkmk_QoS_UrlPort)

[4. Caminho da URL](#bkmk_QoS_UrlPath)

Detalhes são da seguinte maneira:

####  <a name="bkmk_QoS_UrlScheme"></a> 1. Esquema de URL

 `https://` tem uma prioridade maior que `https://`.

####  <a name="bkmk_QoS_UrlHost"></a> 2. Host da URL

 De que a prioridade mais alta para a mais baixa, elas são:

1. nome_do_host

2. Endereço IPv6

3. Endereço IPv4

4. Curinga

No caso de nome de host, um nome de host com elementos mais pontilhados (mais detalhes) tem uma prioridade maior que um nome de host com menos elementos pontilhados. Por exemplo, entre os seguintes nomes de host:

- Video.Internal.training.hr.mycompany.com (profundidade = 6)
  
- selfguide.training.mycompany.com (profundidade = 4)
  
- treinamento (profundidade = 1)
  
- biblioteca (profundidade = 1)
  
 **Video.Internal.training.hr.mycompany.com** tem a prioridade mais alta, e **selfguide.training.mycompany.com** tem a próxima prioridade mais alta. **Treinamento** e **biblioteca** compartilham a mesma prioridade mais baixa.  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3. Porta da URL

Um específico ou um número de porta implícita tem uma prioridade maior que uma porta de curinga.

####  <a name="bkmk_QoS_UrlPath"></a> 4. Caminho da URL

Como um nome de host, um caminho de URL pode conter vários elementos. Aquele com mais elementos sempre tem uma prioridade maior que aquele com o menor. Por exemplo, os caminhos a seguir são listados por prioridade:  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

Se um usuário optar por incluir todos os subdiretórios e arquivos de um caminho de URL a seguir, esse caminho de URL terá uma prioridade mais baixa do que teria se a opção não foram feita.

Um usuário também pode optar por especificar um endereço IP de destino em uma política baseada em URL. Endereço IP de destino tem uma prioridade menor que qualquer um dos quatro campos de URL descritos anteriormente.
  
### <a name="quintuple-policy"></a>Diretiva quintuple

Uma política de Quintuple é especificada pelo ID de protocolo, endereço IP de origem, porta de origem, endereço IP de destino e porta de destino. Uma política Quintuple sempre tem uma precedência maior do que qualquer diretiva baseada em URL. 

Se uma política Quintuple for aplicada para um usuário, uma nova política baseada em URL não irá causar conflitos em qualquer um dos cliente desse usuário computadores.

Para o próximo tópico neste guia, consulte [Gerenciar política de QoS](qos-policy-manage.md).

Para o primeiro tópico deste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
