---
title: Um problema com a exclusão de um nó
description: Este artigo descreve os problemas encontrados ao remover nós da Associação de cluster de failover ativo.
ms.prod: windows-server
ms.technology: server-general
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 08f5e7ef2ddd0f11abee7d0f21b56c3d5a601d3d
ms.sourcegitcommit: 5fac756c2c9920757e33ef0a68528cda0c85dd04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84306515"
---
# <a name="having-a-problem-with-nodes-being-removed-from-active-failover-cluster-membership"></a>Tendo um problema com os nós sendo removidos da Associação de cluster de failover ativo

Este artigo apresenta como resolver os problemas em que os nós são removidos aleatoriamente da Associação de cluster de failover ativo.

## <a name="symptoms"></a>Sintomas

Quando o problema ocorrer, você verá eventos como este registrado no log de eventos do sistema:

![Evento 1135](media/problem-nodes-failover-cluster/1135-1.png)

Esse evento será registrado em todos os nós no cluster, exceto pelo nó removido. O motivo para esse evento é porque um dos nós do cluster marcou esse nó como inoperante. Em seguida, ele notifica todos os outros nós do evento. Quando os nós são notificados, eles descontinuam e subdividiram suas conexões de pulsação com o nó inoperante.

## <a name="what-caused-the-node-to-be-marked-down"></a>O que fez com que o nó fosse marcado como inativo

Todos os nós em um cluster de failover do Windows 2008 ou 2008 R2 se comunicam entre si nas redes que estão definidas para permitir a comunicação de rede de cluster nessa rede. Os nós enviarão pacotes de pulsação entre essas redes para todos os outros nós. Esses pacotes devem ser recebidos por outros nós e, em seguida, uma resposta é enviada de volta. Cada nó no cluster tem suas próprias pulsações que serão monitoradas para garantir que a rede esteja ativa e que os outros nós estejam ativos. O exemplo a seguir deve ajudar a esclarecer isso:

![Nós](media/problem-nodes-failover-cluster/Node2.png)

Se qualquer um desses pacotes não for retornado, a pulsação específica será considerada com falha. Por exemplo, W2K8-R2-NODE2 envia uma solicitação e recebe uma resposta de W2K8-R2-NODE1 para um pacote de pulsação para que ele determine a rede e o nó esteja ativo.  Se W2K8-R2-NODE1 enviar uma solicitação para W2K8-R2-NODE2 e W2K8-R2-NODE1 não obtiver a resposta, ela será considerada uma pulsação perdida e o W2K8-R2-NODE1 manterá o controle dela.  Essa resposta perdida pode ter W2K8-R2-NODE1 mostrar a rede como inoperante até que outra solicitação de pulsação seja recebida.

Por padrão, os nós de cluster têm um limite de cinco falhas em 5 segundos antes que a conexão seja marcada como inativa. Portanto, se W2K8-R2-NODE1 não receber a resposta cinco vezes no período de tempo, ele considerará que a rota específica para W2K8-R2-NODE2 seja inativa. Se outras rotas ainda forem consideradas como ativadas, W2K8-R2-NODE2 permanecerá como um membro ativo.

Se todas as rotas estiverem marcadas como inativas para W2K8-R2-NODE2, elas serão removidas da Associação de cluster de failover ativo e o evento 1135 que você vê na primeira seção será registrado. No W2K8-R2-NODE2, o serviço de cluster é encerrado e, em seguida, reiniciado para que ele possa tentar ingressar novamente no cluster.

Para obter mais informações sobre como tratamos rotas específicas ficando inativas com três ou mais nós, consulte o blog ["particionado" de redes de cluster](/archive/blogs/askcore/partitioned-cluster-networks) que foi escrito por Jeff Hughes.

## <a name="now-that-we-know-how-the-heartbeat-process-works-what-are-some-of-the-known-causes-for-the-process-to-fail"></a>Agora que sabemos como funciona o processo de pulsação, quais são algumas das causas conhecidas para o processo falhar

1. Falhas de hardware de rede reais. Se o pacote for perdido na conexão em algum lugar entre os nós, as pulsações falharão. Um rastreamento de rede de ambos os nós envolvidos revelará isso.

2. O perfil de suas conexões de rede pode estar saltando do domínio para o público e de volta para o domínio novamente. Durante a transição dessas alterações, a e/s de rede pode ser bloqueada. Você pode verificar se esse é o caso examinando o log operacional do perfil de rede. Você pode encontrar esse log abrindo o Visualizador de Eventos e navegando até: aplicativos e serviços Logs\Microsoft\Windows\NetworkProfile\Operational. Examine os eventos neste log no nó que foi mencionado na ID do evento: 1135 e veja se o perfil foi alterado no momento. Nesse caso, confira o artigo da base [de conhecimento o perfil de local de rede muda de "Domain" para "Public" no Windows 7 ou no Windows Server 2008 R2](https://support.microsoft.com/help/2524478/the-network-location-profile-changes-from-domain-to-public-in-windows).

3. Você tem o IPv6 habilitado nos servidores, mas tem as duas regras a seguir desabilitadas para entrada e saída no firewall do Windows:

    - Rede principal-anúncio de descoberta de vizinho
    - Rede principal-solicitação de descoberta de vizinho

4. O software antivírus também pode estar interferindo com esse processo. Se você suspeitar disso, teste desabilitando ou desinstalando o software. Faça isso por sua conta e risco porque você estará desprotegido contra vírus neste momento.

5. A latência na sua rede também poderia fazer com que isso aconteça. Os pacotes não podem ser perdidos entre os nós, mas eles podem não chegar aos nós rápido o suficiente antes que o período de tempo limite expire.

6. IPv6 é o protocolo padrão que o clustering de failover usará para suas pulsações. A pulsação em si é um pacote de rede de unicast UDP que se comunica pela porta 3343. Se houver opções, firewalls ou roteadores não configurados corretamente para permitir esse tráfego por meio do, você poderá experimentar problemas como esse.

7. As atualizações da diretiva de segurança IPsec também podem causar esse problema. O problema específico é que, durante uma atualização de diretiva de grupo IPSec, todas as SAs (associações de segurança) IPsec são divididas pelo firewall do Windows com segurança avançada (WFAS). Enquanto isso estiver acontecendo, toda a conectividade de rede será bloqueada. Ao renegociar as associações de segurança se houver atrasos na execução da autenticação com Active Directory, esses atrasos (onde todas as comunicações de rede são bloqueadas) também bloquearão a obtenção de pulsações de cluster e farão com que o monitoramento da integridade do cluster detecte nós como inativos se eles não responderem dentro do limite de 5 segundos.

8. Drivers de placa de rede antigos ou desatualizados e/ou firmware.  Às vezes, uma simples configuração incorreta da placa de rede ou do comutador também pode causar perda de pulsações.

9. Placas de rede modernas e placas de rede virtual podem estar sofrendo perda de pacotes.  Isso pode ser acompanhado abrindo o monitor de desempenho e adicionando o contador "rede Interface\Packets recebida descartada".  Esse contador é cumulativo e só aumenta até que o servidor seja reinicializado.  Ver um grande número de pacotes ignorados aqui pode ser um sinal de que os buffers de recebimento na placa de rede estão definidos como muito baixos ou que o servidor está executando lentamente e não pode manipular o tráfego de entrada.  Cada fabricante de placa de rede escolhe se deve expor essas configurações nas propriedades da placa de rede, portanto, você precisa consultar o site do fabricante para saber como aumentar esses valores e os valores recomendados devem ser usados.  Se você estiver executando no VMware, o blog a seguir falará sobre isso em um pouco mais de detalhes, incluindo como saber se esse é o problema, além de apontar para o artigo da VMWare sobre as configurações a serem alteradas.

    [Nós sendo removidos da Associação de cluster de failover no VMWare ESX](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx)

Esses são os motivos mais comuns pelos quais esses eventos são registrados, mas também pode haver outros motivos. O ponto deste blog foi fornecer informações sobre o processo e também dar ideias sobre o que procurar. Alguns geram os valores a seguir para seus valores máximos para tentar obter esse problema para parar.

|Parâmetro|Padrão|Intervalo|
|---|---|---|
|**SameSubnetDelay**|1000 milissegundos|250-2000 milissegundos|
|**CrossSubnetDelay**|1000 milissegundos|250-4000 milissegundos|
|**SameSubnetThreshold**|5|3-10|
|**CrossSubnetThreshold**|5|3-10|
||||

Aumentar esses valores para seu máximo pode fazer com que o evento e a remoção do nó desapareçam, apenas mascara o problema. Ele não corrige nada. A melhor coisa a fazer é descobrir a causa raiz das falhas de pulsação e obtê-la corrigida. A única necessidade real de aumentar esses valores é em um cenário multissite em que os nós residem em locais diferentes e a latência de rede não pode ser superada.
