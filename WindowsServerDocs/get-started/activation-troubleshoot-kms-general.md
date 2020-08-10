---
title: Diretrizes para solucionar problemas do KMS
description: Fornece informações sobre o serviço KMS e sugere ferramentas e abordagens para solucionar problemas de ativação
ms.topic: troubleshooting
ms.date: 9/24/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: b31f357089ed54ed5f350657979740e7fd58651a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941804"
---
# <a name="guidelines-for-troubleshooting-the-key-management-service-kms"></a>Diretrizes para solucionar problemas do KMS (Serviço de Gerenciamento de Chaves)

Como parte do processo de implantação, muitos clientes empresariais configuram o KMS (Serviço de Gerenciamento de Chaves) para habilitar a ativação do Windows no ambiente deles. É um processo simples para configurar o host KMS, após o qual os clientes do KMS descobrem o host e tentam fazer a ativação por conta própria. Mas o que acontece se esse processo não funcionar? O que fazer em seguida? Este artigo oferece orientações pelos recursos de que você precisa para solucionar o problema. Para obter mais informações sobre entradas de log de eventos e o script Slmgr.vbs, confira [Referência técnica da ativação de volume](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502529(v=ws.11)).

## <a name="kms-overview"></a>Visão geral do KMS

Vamos começar com uma rápida atualização sobre a ativação do KMS. O KMS é um modelo de cliente/servidor. Conceitualmente, ele se assemelha ao DHCP. Em vez de distribuir endereços IP para clientes em sua solicitação, o KMS habilita a ativação do produto. O KMS também é um modelo de renovação, no qual os clientes tentam reativar em um intervalo regular. Há duas funções: o *host KMS* e o *cliente KMS*.

- O **host KMS** executa o serviço de ativação e habilita a ativação no ambiente. Para configurar um host KMS, você precisa instalar uma chave KMS do VLSC (Centro de Atendimento de Licença por Volume) e, em seguida, ativar o serviço.
- O **cliente KMS** é o sistema operacional Windows implantado no ambiente e precisa ser ativado. Os clientes KMS podem estar executando qualquer edição do Windows que usa a ativação de volume. Os clientes KMS são fornecidos com uma chave pré-instalada, chamada GVLK (Chave de Licença de Volume Genérico) ou chave de instalação de cliente KMS. A presença da GVLK é o que torna um sistema um cliente KMS. Os clientes KMS usam Registros SRV DNS (_vlmcs._tcp) para identificar o host KMS. Em seguida, os clientes tentam automaticamente descobrir e usar esse serviço para se ativarem. Durante o período de carência pronto para uso de 30 dias, eles tentarão ativar a cada duas horas. Após a ativação, os clientes KMS tentam renovar a ativação a cada sete dias.

Do ponto de vista da solução de problemas, talvez seja necessário examinar ambos os lados (host e cliente) para determinar o que está acontecendo.

## <a name="kms-host"></a>Host KMS

Há duas áreas a serem examinadas no host KMS. Primeiro, verifique o status do serviço de licença de software do host. Em segundo lugar, verifique o Visualizador de Eventos quanto a existência de eventos relacionados ao licenciamento ou à ativação.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs e o serviço de licenciamento de software

Para ver a saída detalhada do serviço de licenciamento de software, abra uma janela de prompt de comandos com privilégios elevados e insira **slmgr.vbs /dlv** no prompt de comando. A captura de tela a seguir mostra os resultados desse comando em um de nossos hosts KMS na Microsoft.

![Saída Slmgr do host KMS](./media/ee939272.kms_slmgr_output(en-us,technet.10).png)

Os campos mais importantes para solução de problemas são os seguintes. O que você está procurando pode ser diferente, dependendo do problema a ser resolvido.

- **Informações de versão**. Na parte superior da saída **slmgr.vbs /dlv** está a versão do serviço de licenciamento de software. Isso pode ser útil para determinar se a versão atual do serviço está instalada. Por exemplo, as atualizações no serviço KMS no Windows Server 2003 dão suporte a chaves de host KMS diferentes. Esses dados podem ser usados para avaliar se a versão é ou não atual e dá suporte à chave de host KMS que você está tentando instalar. Para obter mais informações sobre essas atualizações, confira [Uma atualização está disponível para o Windows Vista e Windows Server 2008 para estender o suporte à ativação do KMS para Windows 7 e Windows Server 2008 R2](https://support.microsoft.com/help/968912/an-update-is-available-for-windows-vista-and-for-windows-server-2008-t).
- **Nome**. Isso indica a edição do Windows instalada no sistema do host KMS. Isso poderá ser importante para a solução de problemas se você estiver tendo problemas para adicionar ou alterar a chave do host KMS (por exemplo, para verificar se a chave tem suporte na edição do sistema operacional).
- **Descrição**. Informa a chave que está instalada. Use este campo para verificar qual chave foi usada para ativar o serviço e se ela é ou não a correto para os clientes KMS que você implantou.
- **Status da licença**. Esse é o status do sistema do host KMS. O valor deve ser pelo menos **Licensed**. Qualquer outro valor significa que algo está errado e talvez você precise reativar o host.
- **Contagem atual**. A contagem exibida estará entre **0** e **50**. A contagem é cumulativa (entre sistemas operacionais) e indica o número de sistemas válidos que tentaram ser ativados em um período de 30 dias.

  Se a contagem é **0**, o serviço foi ativado recentemente ou nenhum cliente válido se conectou ao host KMS.

  A contagem não aumentará acima de **50**, independentemente de quantos sistemas válidos existirem no ambiente. Isso ocorre porque a contagem é definida para armazenar em cache apenas duas vezes a política de licença máxima que é retornada por um cliente KMS. A política máxima atualmente é definida pelo sistema operacional do cliente Windows, que requer uma contagem de **25** ou superior do host KMS para se ativar. Portanto, a contagem mais alta no host KMS é 2 x 25 ou 50. Observe que em ambientes que contêm apenas clientes KMS do Windows Server, a contagem máxima no host KMS será **10**. Isso ocorre porque o limite para as edições do Windows Server é **5** (2 x 5 ou 10).

  Um problema comum relacionado à contagem é se o ambiente tem um host KMS ativado e clientes suficientes, mas a contagem não aumenta além de um. O principal problema é que a imagem do cliente implantada não foi configurada corretamente (**sysprep /generalize**) e os sistemas não têm CMIDs (IDs do computador cliente) exclusivas. Para obter mais informações, confira [Cliente KMS](#kms-client) e [A contagem atual do KMS não aumenta quando você adiciona o novo Windows Vista ou computadores cliente baseados no Windows 7 à rede](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista). Um dos nossos engenheiros de escalonamento de suporte também publicou sobre esse problema, em [A contagem de cliente do host KMS não está aumentando devido a CMIDs duplicadas](/archive/blogs/askcore/kms-host-client-count-not-increasing-due-to-duplicate-cmids).

  Outro motivo pelo qual a contagem pode não estar aumentando é que há muitos hosts KMS no ambiente e a contagem é distribuída em todos eles.
- **Ouvindo na porta**. A comunicação com o KMS usa RPC anônimo. Por padrão, os clientes usam a porta TCP 1688 para se conectar ao host KMS. Verifique se essa porta está aberta entre seus clientes KMS e o host KMS. Você pode alterar ou configurar a porta no host KMS. Durante a comunicação, o host KMS envia a designação de porta para os clientes KMS. Se você alterar a porta em um cliente KMS, a designação de porta será substituída quando o cliente contatar o host.

Muitas vezes, recebemos uma pergunta sobre a seção "solicitações cumulativas" da saída **slmgr.vbs /dlv**. Geralmente, esses dados não são úteis para a solução de problemas. O host KMS mantém um registro contínuo do estado de cada cliente KMS que tenta ativar ou reativar. Solicitações com falha indicam clientes KMS para os quais o host KMS não dá suporte. Por exemplo, se um cliente KMS do Windows 7 tentar ativar em um host KMS que foi ativado usando uma chave KMS do Windows Vista, a ativação falhará. As linhas "Solicitações com status de licença" descrevem todos os estados de licença possíveis, passados e presentes. De uma perspectiva de solução de problemas, esses dados serão relevantes somente se a contagem não estiver aumentando conforme o esperado. Nesse caso, você deverá ver o número de solicitações com falha aumentando. Isso indica que você deve verificar a chave do produto (Product Key) que foi usada para ativar o sistema de host KMS. Além disso, observe que os valores de solicitação cumulativa serão redefinidos somente se você reinstalar o sistema de host KMS.

### <a name="useful-kms-host-events"></a>Eventos de host KMS úteis

#### <a name="event-id-12290"></a>ID do evento 12290

O host KMS registra a ID de evento 12290 quando um cliente KMS entra em contato com o host a fim de ativá-lo. A ID de evento 12290 fornece uma quantidade significativa de informações que você pode usar para descobrir que tipo de cliente contatou o host e por que ocorreu uma falha. O seguinte segmento de uma entrada de ID de evento 12290 é proveniente do log de eventos do Serviço de Gerenciamento de Chaves de nosso host KMS.

![Evento 12290 do KMS](./media/ee939272.kms_12290_event(en-us,technet.10).png)

Os detalhes do evento incluem as seguintes informações:

- **Contagem mínima necessária para ativar**. O cliente KMS está relatando que a contagem do host KMS deve ser **5** para ser ativada. Isso significa que se trata de um sistema operacional Windows Server, embora não indique uma edição específica. Se os clientes não estiverem sendo ativados, verifique se a contagem é suficiente no host.
- **CMID (ID do computador cliente**). Este é um valor exclusivo em cada sistema. Se esse valor não é exclusivo, isso ocorre porque uma imagem não foi preparada corretamente para distribuição (**sysprep /generalize**). Esse problema se manifesta no host KMS como uma contagem que não aumentará, mesmo que haja clientes suficientes no ambiente. Para obter mais informações, confira [A contagem atual do KMS não aumenta quando você adiciona o novo Windows Vista ou computadores cliente baseados no Windows 7 à rede](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista).
- **Estado da licença e tempo para expiração do estado**. Este é o estado atual da licença do cliente. Ele pode ajudar você a diferenciar um cliente que está tentando ativar pela primeira vez de um que está tentando reativar. A entrada de tempo informa quanto mais tempo o cliente permanecerá nesse estado, se nada mudar.

Se você estiver solucionando problemas de um cliente e não puder encontrar uma ID de evento 12290 correspondente no host KMS, esse cliente não se conectará ao host KMS. Alguns motivos pelos quais uma entrada de ID de evento 12290 pode não existir são os seguintes:

- Ocorreu uma interrupção de rede.
- O host não está resolvendo ou não está registrado no DNS.
- O firewall está bloqueando o TCP 1688.
   A porta pode estar bloqueada em vários locais no ambiente, incluindo no próprio sistema de host KMS. Por padrão, o host KMS tem uma exceção de firewall para o KMS, mas ela não é habilitada automaticamente. Você precisa ativar a exceção.
- O log de eventos está cheio.

Os clientes KMS registram dois eventos correspondentes, a ID de evento 12288 e a ID de evento 12289. Para obter informações sobre esses eventos, confira a seção [Cliente KMS](#kms-client).

#### <a name="event-id-12293"></a>ID do evento 12293

Outro evento relevante a ser pesquisado no host KMS é a ID de evento 12293. Esse evento indica que o host não publicou os registros necessários no DNS. Essa situação é conhecida por causar falhas e é algo que você deve verificar *depois* de configurar seu host e *antes* de implantar clientes. Para obter mais informações sobre problemas do DNS, confira [Procedimentos comuns de solução de problemas do KMS e do DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="kms-client"></a>Cliente KMS

Nos clientes, use as mesmas ferramentas (Slmgr e Visualizador de Eventos) para solucionar problemas de ativação.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs e o serviço de licenciamento de software

Para ver a saída detalhada do serviço de licenciamento de software, abra uma janela de prompt de comandos com privilégios elevados e insira **slmgr.vbs /dlv** no prompt de comando. A captura de tela a seguir mostra os resultados desse comando em um de nossos hosts KMS na Microsoft.

![Saída Slmgr do cliente KMS](./media/ee939272.kms_client_slmgr_output(en-us,technet.10).png)

A lista a seguir inclui os campos mais importantes para solução de problemas. O que você está procurando pode ser diferente, dependendo do problema a ser resolvido.

- **Nome**. Este valor é a edição do Windows instalada no sistema do cliente KMS. Use isso para verificar se a versão do Windows que você está tentando ativar pode usar o KMS. Por exemplo, nosso suporte técnico observou incidentes nos quais os clientes tentam instalar a chave de instalação do cliente KMS em uma edição do Windows que não usa a ativação de volume, como o Windows Vista Ultimate.
- **Descrição**. Este valor mostra a chave que está instalada. VOLUME_KMSCLIENT indica que a chave de instalação do cliente KMS (ou GVLK) está instalada (a configuração padrão para mídia de licenciamento por volume) e que esse sistema tenta automaticamente ativar usando um host KMS. Se você vir alguma outra coisa aqui, como MAK, precisará reinstalar a GVLK para configurar esse sistema como um cliente KMS. Você pode instalar manualmente a chave usando **slmgr.vbs /ipk &lt;*GVLK*&gt;** (conforme descrito em [Chaves de instalação de cliente KMS](kmsclientkeys.md)) ou usar a VAMT (Ferramenta de Gerenciamento de Ativação de Volume). Para obter mais informações sobre como obter e usar a VAMT, confira [Referência técnica da VAMT (Ferramenta de Gerenciamento de Ativação de Volume)](/windows/deployment/volume-activation/volume-activation-management-tool).
- **Chave do produto (Product Key) parcial**. Como o campo **Nome**, você pode usar essas informações para determinar se a chave de instalação de cliente KMS correta está instalada neste computador (em outras palavras, a chave corresponde ao sistema operacional instalado no cliente KMS). Por padrão, a chave correta está presente em sistemas criados com o uso de mídia do portal do VLSC (Centro de Atendimento de Licença por Volume). Em alguns casos, os clientes podem usar a ativação de MAK (chave de ativação múltipla) até que haja sistemas suficientes no ambiente para dar suporte à ativação do KMS. A chave de instalação de cliente KMS deve ser instalada nesses sistemas para fazer a transição da MAK para o KMS. Use a VAMT para instalar essa chave e verifique se a chave correta foi aplicada.
- **Status da licença**. Este valor mostra o status do sistema de cliente KMS. Para um sistema que foi ativado usando o KMS, esse valor deve ser **Licensed**. Qualquer outro valor pode indicar que há um problema. Por exemplo, se o host KMS estiver funcionando corretamente e o cliente KMS não for ativado (por exemplo, ele permanecerá em um estado **Grace**), algo poderá estar impedindo o cliente de acessar o sistema host (como um problema de firewall, interrupção de rede ou algo semelhante).
- **CMID (ID do computador cliente**). Cada cliente KMS deve ter uma CMID exclusiva. Conforme mencionado na seção [Host KMS](#kms-host), um problema comum relacionado à contagem é se o ambiente tiver um host KMS ativado e clientes suficientes, mas a contagem não aumentará além de **1**. Para obter mais informações, confira [A contagem atual do KMS não aumenta quando você adiciona o novo Windows Vista ou computadores cliente baseados no Windows 7 à rede](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista).
- **Nome do computador KMS do DNS**. Este valor mostra o FQDN do host KMS que o cliente usou com êxito para ativação e a porta TCP usada para a comunicação.
- **Armazenamento em cache do host KMS**. O valor final mostra se o armazenamento em cache está habilitado ou não. Por padrão, está habilitado. Isso significa que o cliente KMS armazena em cache o nome do host KMS que ele usou para ativação e se comunica diretamente com esse host (em vez de consultar o DNS) quando é hora de reativar. Se o cliente não puder contatar o host KMS armazenado em cache, ele consultará o DNS para descobrir um novo host KMS.

### <a name="useful-kms-client-events"></a>Eventos de cliente KMS úteis

#### <a name="event-id-12288-and-event-id-12289"></a>ID de evento 12288 e ID de evento 12289

Quando um cliente KMS é ativado ou reativado com êxito, o cliente registra dois eventos: ID de evento 12288 e ID de evento 12289. O seguinte segmento de uma entrada de ID de evento 12288 é proveniente do log de eventos do Serviço de Gerenciamento de Chaves de nosso cliente KMS.

![ID de evento 12288 do cliente KMS](./media/ee939272.client_12288(en-us,technet.10).png)

Caso veja apenas a ID de evento 12288 (sem uma ID de evento 12289 correspondente), isso significa que o cliente KMS não pôde acessar o host KMS, o host KMS não respondeu ou o cliente não recebeu a resposta. Nesse caso, verifique se o host KMS é detectável e se os clientes KMS podem contatá-lo.

As informações mais relevantes na ID de evento 12288 são os dados na seção de informações. Por exemplo, essa seção mostra o estado atual do cliente mais o FQDN e a porta TCP que o cliente usou quando ele tentou ativar. Você pode usar o FQDN para solucionar problemas de casos em que a contagem em um host KMS não está aumentando. Por exemplo, se houver muitos hosts KMS disponíveis para os clientes (sistemas legítimos ou não autorizados), a contagem poderá ser distribuída em todos eles.

Uma ativação malsucedida nem sempre significa que o cliente tem 12288 em vez de 12289. Uma ativação ou reativação com falha também pode ter ambos os eventos. Nesse caso, você precisa examinar o segundo evento para verificar o motivo da falha.

![ID de evento 12289 do cliente KMS](./media/ee939272.client_12289(en-us,technet.10).png)

A seção Informações da ID de evento 12289 fornece os seguintes dados:

- **Sinalizador de ativação**. Este valor indica se a ativação foi bem-sucedida (**1**) ou falhou (**0**).
- **Contagem atual no host KMS**. Este valor reflete o valor de contagem no host KMS quando o cliente tenta ativar. Se a ativação falhar, poderá ser porque a contagem é insuficiente para esse sistema operacional cliente ou porque não há sistemas suficientes no ambiente para criar a contagem.

## <a name="what-does-support-ask-for"></a>O que o suporte solicita?

Se você precisar chamar o suporte para solucionar problemas de ativação, o engenheiro de suporte normalmente solicitará as seguintes informações:

- Saída **Slmgr.vbs /dlv** do host KMS e dos sistemas de cliente KMS. Se você usar wscript ou cscript para executar o comando, poderá usar Ctrl + C para copiar a saída e, em seguida, colá-la no Bloco de notas para enviá-la ao contato de suporte.
- Logs de eventos do host KMS (log do Serviço de Gerenciamento de Chaves) e dos sistemas de cliente KMS (log do aplicativo)

## <a name="additional-references"></a>Referências adicionais
- [Pergunte à equipe principal: #Activation](/archive/blogs/askcore/kms-host-client-count-not-increasing-due-to-duplicate-cmids)
