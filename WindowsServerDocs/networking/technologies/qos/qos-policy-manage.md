---
title: Gerenciar política de QoS
description: Este tópico fornece instruções sobre como criar e gerenciar a política de QoS (qualidade de serviço) no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8f10ab7b3da05fbefabb735ee2b8bb4ef1cb8a
ms.sourcegitcommit: effbc183bf4b370905d95c975626c1ccde057401
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781343"
---
# <a name="manage-qos-policy"></a>Gerenciar política de QoS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre como usar o assistente de política de QoS para criar, editar ou excluir uma política de QoS.

>[!NOTE]
>  Além deste tópico, a documentação de gerenciamento de política de QoS a seguir está disponível.
> 
>  - [Eventos e erros da política de QoS](qos-policy-errors.md)

Em sistemas operacionais Windows, a política de QoS combina a funcionalidade de QoS baseada em padrões com a capacidade de gerenciamento de Política de Grupo. A configuração dessa combinação torna o aplicativo fácil de políticas de QoS para Política de Grupo objetos. O Windows inclui um assistente de política de QoS para ajudá-lo a realizar as tarefas a seguir.

-  [Criar uma política de QoS](#bkmk_createpolicy)

-  [Exibir, editar ou excluir uma política de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Criar uma política de QoS

Antes de criar uma política de QoS, é importante entender os dois principais controles de QoS usados para gerenciar o tráfego de rede:

- Valor de DSCP

-   Taxa de limitação

### <a name="prioritizing-traffic-with-dscp"></a>Priorizando o tráfego com DSCP

Conforme observado no exemplo anterior de aplicativo de linha de negócios, você pode definir a prioridade do tráfego de rede de saída usando **especificar valor DSCP** para configurar uma política de QoS com um valor DSCP específico. 

Conforme descrito em RFC 2474, o DSCP permite que valores de 0 a 63 sejam especificados dentro do campo TOS de um pacote IPv4 e dentro do campo classe de tráfego no IPv6. Os roteadores de rede usam o valor DSCP para classificar os pacotes de rede e enfileirar-los adequadamente.
  
> [!NOTE]
>  Por padrão, o tráfego do Windows tem um valor DSCP de 0.
  
O número de filas e seu comportamento de priorização devem ser planejados como parte da estratégia de QoS de sua organização. Por exemplo, sua organização pode optar por ter cinco filas: tráfego sensível à latência, tráfego de controle, tráfego crítico para os negócios, tráfego de melhor esforço e tráfego de transferência de dados em massa.  
  
### <a name="throttling-traffic"></a>Acelerando o tráfego

Juntamente com valores DSCP, a limitação é outro controle de chave para gerenciar a largura de banda da rede. Conforme mencionado anteriormente, você pode usar a configuração **especificar taxa de restrição** para configurar uma política de QoS com uma taxa de limitação específica para o tráfego de saída. Usando a limitação, uma política de QoS limita o tráfego de rede de saída para uma taxa de limitação especificada. A aceleração e a marcação de DSCP podem ser usadas juntas para gerenciar o tráfego de maneira eficiente.

>[!NOTE]
>Por padrão, a caixa de seleção **Especificar Taxa de Aceleração** não está marcada.

Para criar uma política de QoS, edite as configurações de um objeto de Política de Grupo (GPO) de dentro da ferramenta Console de Gerenciamento de Política de Grupo (GPMC). Em seguida, o GPMC abre a Editor de Objeto de Política de Grupo.

Os nomes das políticas de QoS devem ser exclusivos. Como as políticas são aplicadas a servidores e usuários finais depende de onde a política de QoS é armazenada no Editor de Objeto de Política de Grupo:

- Uma política de QoS no computador \ Settings\QoS política se aplica a computadores, independentemente do usuário conectado no momento. Normalmente, você usa políticas de QoS baseadas no computador em servidores.

- Uma política de QoS no usuário \ Settings\QoS política se aplica aos usuários após o logon, independentemente do computador no qual eles fizeram logon.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Para criar uma nova política de QoS com o assistente de política de QoS

-   No Editor de Objeto de Política de Grupo, clique com o botão direito do mouse em um dos nós da **política de QoS** e clique em **criar uma nova política**.

### <a name="wizard-page-1---policy-profile"></a>Página 1 do assistente-perfil de política

Na primeira página do assistente de política de QoS, você pode especificar um nome de política e configurar como o QoS controla o tráfego de rede de saída.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Para configurar a página Perfil da Política do Assistente de QoS Baseado em Política

1. Em **Nome da política**, digite um nome para a política de QoS. O nome deve identificar a política de forma exclusiva.

2. Opcionalmente, use **especificar valor DSCP** para habilitar a marcação DSCP e, em seguida, configure um valor DSCP entre 0 e 63.

3. Como opção, use **Especificar Taxa de Aceleração** para habilitar a aceleração de tráfego e configurar a taxa de aceleração. O valor da taxa de limitação deve ser maior que 1 e você pode especificar unidades de kilobytes por segundo \(KBps\) ou megabytes por segundo \(MBps\).

4. Clique em **Avançar**.

### <a name="wizard-page-2---application-name"></a>Página 2 do assistente-nome do aplicativo

Na segunda página do assistente de política de QoS, você pode aplicar a política a todos os aplicativos, a um aplicativo específico, conforme identificado pelo nome do executável, para um caminho e nome de aplicativo, ou para os aplicativos de servidor HTTP que lidam com solicitações de uma URL específica.

- **Todos os aplicativos** especifica que as configurações de gerenciamento de tráfego na primeira página do assistente de política de QoS se aplicam a todos os aplicativos.

- **Somente aplicativos com esse nome executável** especificam que as configurações de gerenciamento de tráfego na primeira página do assistente de política de QoS são para um aplicativo específico. O nome do arquivo executável deve terminar com a extensão de nome de arquivo .exe.

- **Somente aplicativos de servidor http respondendo a solicitações para esta URL** especifica que as configurações de gerenciamento de tráfego na primeira página do assistente de política de QoS se aplicam apenas a determinados aplicativos de servidor http.

Como opção, você pode inserir o caminho do aplicativo. Para especial um caminho de aplicativo, inclua o caminho com o nome do aplicativo. O caminho pode incluir variáveis de ambiente. Por exemplo, %ProgramFiles%\Caminho do Meu Aplicativo\MeuAplicativo.exe ou c:\arquivos de programas\caminho do meu aplicativo\meuaplicativo.exe.

>[!NOTE]
>O caminho do aplicativo não pode incluir um caminho que resolva um link simbólico.

A URL deve estar em conformidade com a [RFC 1738](https://tools.ietf.org/html/rfc1738), na forma de `http[s]://<hostname\>:<port\>/<url-path>`. Você pode usar um caractere curinga, `‘*'`, para `<hostname>` e/ou `<port>`, por exemplo, `https://training.\*/, https://\*.\*`, mas o curinga não pode indicar uma subcadeia de `<hostname>` ou `<port>`.

Em outras palavras, nem `https://my\*site/` nem `https://\*training\*/` são válidos. 

Opcionalmente, você pode selecionar **incluir subdiretórios e arquivos** para executar correspondência em todos os subdiretórios e arquivos após uma URL. Por exemplo, se essa opção estiver marcada e a URL for `https://training`, a política de QoS considerará as solicitações de` https://training/video` uma boa correspondência.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Para configurar a página nome do aplicativo do assistente de política de QoS

1. Nesta **política de QoS aplica-se a**, selecione **todos os aplicativos** ou **somente aplicativos com esse nome executável**.

2. Se você selecionar **Somente aplicativos com este nome executável**, especifique o nome de um executável que termine com a extensão de nome de arquivo .exe.

3. Clique em **Avançar**.

### <a name="wizard-page-3---ip-addresses"></a>Página 3-endereços IP do assistente

Na terceira página do assistente de política de QoS, você pode especificar as condições de endereço IP para a política de QoS, incluindo o seguinte:

- Todos os endereços IPv4 ou IPv6 de origem ou endereços IPv4 ou IPv6 de origem específicos

- Todos os endereços IPv4 ou IPv6 de destino ou endereços IPv4 ou IPv6 de destino específicos

Se você selecionar **Somente para o seguinte endereço IP de origem** ou **Somente para o seguinte endereço IP de destino**, será necessário digitar uma das seguintes opções:

- Um endereço IPv4, como `192.168.1.1`

- Um prefixo de endereço IPv4 usando a notação de comprimento de prefixo de rede, como `192.168.1.0/24`

- Um endereço IPv6, como `3ffe:ffff::1`

- Um prefixo de endereço IPv6, como `3ffe:ffff::/48`

Se você selecionar ambos **apenas para o endereço IP de origem a seguir** e **apenas para o endereço IP de destino a seguir**, os endereços ou prefixos de endereço deverão ser baseados em IPv4 ou IPv6.

Se você especificou a URL para aplicativos de servidor HTTP na página anterior do assistente, observará que o endereço IP de origem da política de QoS nesta página do assistente está esmaecido. 

Isso é verdadeiro porque o endereço IP de origem é o endereço do servidor HTTP e não é configurável aqui. Por outro lado, você ainda pode personalizar a política especificando o endereço IP de destino. Isso possibilita que você crie políticas diferentes para clientes diferentes usando os mesmos aplicativos de servidor HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Para configurar a página endereços IP do assistente de política de QoS

1. Nesta **política de QoS aplica-se a** (origem), selecione **qualquer endereço IP de origem** ou **apenas para o endereço de origem IP a seguir**.

2. Se você selecionou **apenas o endereço de origem IP a seguir**, especifique um endereço ou prefixo IPv4 ou IPv6.

3. Nesta **política de QoS aplica-se a** (destino), selecione **qualquer endereço de destino** ou **apenas para o endereço IP de destino a seguir.**

4. Se você selecionou **apenas para o endereço IP de destino a seguir**, especifique um endereço IPv4 ou IPv6 ou prefixo que corresponda ao tipo de endereço ou prefixo especificado para o endereço de origem.

5.  Clique em **Avançar**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Página 4 do assistente-protocolos e portas

Na quarta página do assistente de política de QoS, você pode especificar os tipos de tráfego e as portas que são controladas pelas configurações na primeira página do assistente. Você pode especificar:  
-   Tráfego TCP, tráfego UDP ou os dois  

-   Todas as portas de origem, um intervalo de portas de origem ou uma porta de origem específica

-   Todas as portas de destino, um intervalo de portas de destino ou uma porta de destino específica  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Para configurar a página protocolos e portas do assistente de política de QoS

1. Em **Selecionar o protocolo ao qual esta política de QoS se aplica**, selecione **TCP**, **UDP** ou **TCP e UDP**.

2. Em **Especifique o número da porta de origem**, selecione **De qualquer porta de origem** ou **Deste número de porta de origem**.

3. Se você selecionou a **partir desse número de porta de origem**, digite um número de porta entre 1 e 65535.

     Opcionalmente, você pode especificar um intervalo de portas, no formato "*baixo*:*alto*", onde *baixo* e *alto* representam os limites inferiores e limites superiores do intervalo de portas, inclusive. *Baixo* e *alto* devem ser um número entre 1 e 65535. Não são permitidos espaços entre o caractere de dois pontos (:) e os números.

4. Em **Especifique o número da porta de destino**, selecione **Para qualquer porta de destino** ou **Para este número de porta de destino**.

5. Se você selecionou **Para este número de porta de destino** na etapa anterior, digite um número de porta entre 1 e 65535.

Para concluir a criação da nova política de QoS, clique em **concluir** na página **protocolos e portas** do assistente de política de QoS. Quando concluído, a nova política de QoS é listada no painel de detalhes do Editor de Objeto de Política de Grupo.  
  
Para aplicar as configurações de política de QoS a usuários ou computadores, vincule o GPO no qual as políticas de QoS estão localizadas em um contêiner Active Directory Domain Services, como um domínio, um site ou uma UO (unidade organizacional).  
  
##  <a name="bkmk_editpolicy"></a>Exibir, editar ou excluir uma política de QoS

As páginas do assistente de política de QoS descritas anteriormente correspondem às páginas de propriedades que são exibidas quando você exibe ou edita as propriedades de uma política.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Para exibir as propriedades de uma política de QoS  
  
-   Clique com o botão direito do mouse no nome da política no painel de detalhes da Editor de Objeto de Política de Grupo e clique em **Propriedades**.  
  
     O Editor de Objeto de Política de Grupo exibe a página Propriedades com as seguintes guias:  
  
    -   Perfil da Política  
  
    -   Nome do aplicativo  
  
    -   Endereços IP  
  
    -   Protocolos e portas  
  
### <a name="to-edit-a-qos-policy"></a>Para editar uma política de QoS  
  
-   Clique com o botão direito do mouse no nome da política no painel de detalhes da Editor de Objeto de Política de Grupo e clique em **Editar política existente**.  
  
     O Editor de Objeto de Política de Grupo exibe a caixa de diálogo **Editar uma política de QoS existente** .  
  
### <a name="to-delete-a-qos-policy"></a>Para excluir uma diretiva de QoS  
  
-   Clique com o botão direito do mouse no nome da política no painel de detalhes da Editor de Objeto de Política de Grupo e clique em **excluir política**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Relatórios GPMC de política de QoS 

Depois de ter aplicado uma série de políticas de QoS em sua organização, pode ser útil ou necessário examinar periodicamente como as políticas são aplicadas. Um resumo das políticas de QoS para um usuário ou computador específico pode ser exibido usando os relatórios do GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Para executar o assistente de resultados de Política de Grupo para um relatório de políticas de QoS  
  
-   No GPMC, clique com o botão direito do mouse no nó **política de grupo resultados** e selecione a opção de menu para **Assistente de política de grupo de resultados.**  
  
Depois que os resultados da Política de Grupo forem gerados, clique na guia **configurações** . Na guia **configurações** , as políticas de QoS podem ser encontradas nos nós "Computer \ Settings\QoS Policy" e "User \ Settings\QoS Policy".  
  
Na guia **configurações** , as políticas de QoS são listadas por seus nomes de política de QoS com o valor DSCP, a taxa de aceleração, as condições de política e o GPO vencedor listado na mesma linha.  
  
A exibição de resultados de Política de Grupo identifica exclusivamente o GPO vencedor. Quando vários GPOs têm políticas de QoS com o mesmo nome de política de QoS, o GPO com a maior precedência de GPO é aplicado. Esse é o GPO vencedor. As políticas de QoS conflitantes (identificadas pelo nome da política) que estão anexadas a um GPO de prioridade mais baixa não são aplicadas. Observe que as prioridades de GPO definem quais políticas de QoS são implantadas no site, no domínio ou na UO, conforme apropriado. Após a implantação, em um nível de usuário ou computador, as [regras de precedência de política de QoS](#BKMK_precedencerules) determinam qual tráfego é permitido e bloqueado.  
  
O valor DSCP da política de QoS, a taxa de limitação e as condições de política também são visíveis em Editor de Objeto de Política de Grupo (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Configurações avançadas para roaming e usuários remotos  
Com a política de QoS, o objetivo é gerenciar o tráfego na rede de uma empresa. Em cenários móveis, os usuários podem estar enviando tráfego para dentro ou para fora da rede corporativa. Como as políticas de QoS não são relevantes enquanto estão longe da rede da empresa, as políticas de QoS são habilitadas somente em interfaces de rede conectadas à empresa para Windows 8, Windows 7 ou Windows Vista.  
  
Por exemplo, um usuário pode conectar seu computador portátil à rede da sua empresa por meio da VPN (rede virtual privada) de uma cafeteria. Para VPN, a interface de rede física (como sem fio) não terá políticas de QoS aplicadas. No entanto, a interface VPN terá políticas de QoS aplicadas porque se conecta à empresa. Se o usuário mais tarde inserir outra rede da empresa que não tenha uma relação de confiança AD DS, as políticas de QoS não serão habilitadas.  
  
Observe que esses cenários móveis não se aplicam às cargas de trabalho do servidor. Por exemplo, um servidor com vários adaptadores de rede pode se sentar na borda da rede de uma empresa. O departamento de ti pode optar por ter políticas de QoS para limitar o tráfego que egresso a empresa; no entanto, esse adaptador de rede que envia esse tráfego de saída não necessariamente se conecta de volta à rede corporativa. Por esse motivo, as políticas de QoS sempre são habilitadas em todas as interfaces de rede de um computador que executa o Windows Server 2012.  
  
> [!NOTE]
>  A habilitação seletiva só se aplica a políticas de QoS e não às configurações de QoS avançadas discutidas a seguir neste documento.  
  
### <a name="advanced-qos-settings"></a>Configurações de QoS avançadas

As configurações de QoS avançadas fornecem controles adicionais para que os administradores de ti gerenciem o consumo de rede do computador e marcações DSCP. As configurações de QoS avançadas aplicam-se somente ao nível do computador, enquanto as políticas de QoS podem ser aplicadas nos níveis do computador e do usuário.

#### <a name="to-configure-advanced-qos-settings"></a>Para definir configurações de QoS avançadas

1.  Clique em **configuração do computador**e, em seguida, clique em **configurações do Windows em política de grupo**.
  
2.  Clique com o botão direito do mouse em **política de QoS**e clique em **configurações de QoS avançadas**.

     A figura a seguir mostra as duas guias configurações avançadas de QoS: **tráfego TCP de entrada** e **substituição de marcação DSCP**.
  
> [!NOTE]
>  As configurações de QoS avançadas são configurações de Política de Grupo no nível do computador.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Configurações de QoS avançadas: tráfego TCP de entrada

O **tráfego TCP de entrada** controla o consumo de largura de banda TCP no lado do destinatário, enquanto as políticas de QoS afetam o tráfego TCP e UDP de saída. 

Ao definir um nível de taxa de transferência menor na guia **tráfego TCP de entrada** , o TCP limitará o tamanho de sua janela de recebimento TCP anunciada. O efeito dessa configuração aumentará as taxas de taxa de transferência e a utilização de links para conexões TCP com larguras de banda ou latências maiores (produto de atraso de largura de banda). Por padrão, os computadores que executam o Windows Server 2012, o Windows 8, o Windows Server 2008 R2, o Windows Server 2008 e o Windows Vista são definidos como o nível máximo de taxa de transferência.
  
A janela de recepção TCP foi alterada no Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista de versões anteriores do Windows. As versões anteriores do Windows limitaram a janela do lado de recebimento TCP a um máximo de 64 kilobytes (KB), enquanto o Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista dimensionam dinamicamente a janela do lado de recebimento até 16 megabytes (MB ). No controle de tráfego TCP de entrada, você pode controlar o nível de taxa de transferência de entrada definindo o valor máximo para o qual a janela de recepção TCP pode crescer. Os níveis correspondem aos valores máximos a seguir. 
  
|Nível de taxa de transferência de entrada|Máximo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

O tamanho real da janela pode ser um valor igual ou menor que o máximo, dependendo das condições da rede.

###### <a name="to-set-the-tcp-receive-side-window"></a>Para definir a janela do lado de recebimento TCP

1. Em Editor de Objeto de Política de Grupo, clique em **política de computador local**, clique em **configurações do Windows**, clique com o botão direito do mouse em política de **QoS**e clique em **configurações de QoS avançadas**.
  
2. Em **TCP recebendo taxa de transferência**, selecione **Configurar taxa de transferência de recebimento de TCP**e, em seguida, selecione o nível de taxa de transferência desejado.

3.  Vincule o GPO à UO.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Configurações de QoS avançadas: substituição de marcação DSCP

A substituição de marcação DSCP restringe a capacidade dos aplicativos de especificar — ou "marcar" — valores de DSCP diferentes daqueles especificados nas políticas de QoS. Ao especificar que os aplicativos têm permissão para definir valores DSCP, os aplicativos podem definir valores DSCP diferentes de zero. 

Ao especificar **ignorar**, os aplicativos que usam APIs de QoS terão seus valores DSCP definidos como zero, e somente as políticas de QoS poderão definir valores DSCP. 

Por padrão, os computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista permitem que os aplicativos especifiquem valores DSCP; aplicativos e dispositivos que não usam as APIs de QoS não são substituídos.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valores de multimídia e de DSCP sem fio

A [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) estabeleceu uma certificação para\) de multimídia sem fio \(WMM que define quatro categorias de acesso \(WMM_AC\) para priorizar o tráfego de rede transmitido em uma rede sem fio Wi\-Fi. As categorias de acesso incluem \(na ordem de prioridade mais alta para a mais baixa\): voz, vídeo, melhor esforço e plano de fundo; abreviado, respectivamente, como VO, VI, is e BK. A especificação WMM define quais valores de DSCP correspondem a cada uma das quatro categorias de acesso:
  
|Valor de DSCP|Categoria de acesso do WMM|
|----------|-------------------|
|48-63|Voz (VO)|
|32-47|Vídeo (VI)|
|24-31, 0-7|Melhor esforço (ser)|
|8-23|Segundo plano (BK)|

Você pode criar políticas de QoS que usam esses valores DSCP para garantir que computadores portáteis com o Wi\-Fi CERTIFIED™ para adaptadores sem fio WMM recebam tratamento priorizado quando associados a Wi\-Fi certificados para pontos de acesso do WMM.
  
### <a name="BKMK_precedencerules"></a>Regras de precedência de política de QoS

Semelhante às prioridades do GPO, as políticas de QoS têm regras de precedência para resolver conflitos quando várias políticas de QoS se aplicam a um conjunto específico de tráfego. Para o tráfego TCP ou UDP de saída, somente uma política de QoS pode ser aplicada por vez, o que significa que as políticas de QoS não têm um efeito cumulativo, como o local em que as taxas de limitação seriam somadas.

Em geral, a política de QoS com as condições mais correspondentes vence. Quando várias políticas de QoS se aplicam, as regras se enquadram em três categorias: nível de usuário versus nível de computador; aplicativo versus a rede quintuple; e entre a rede quintuple.

Por *quintuple de rede*, queremos dizer o endereço IP de origem, o endereço IP de destino, a porta de origem, a porta de destino e o protocolo \(\)TCP/UDP.  

 **A política de QoS de nível de usuário tem precedência sobre a política de QoS no nível do computador**

Essa regra facilita muito o gerenciamento de GPOs de QoS pelos administradores de rede, especialmente para políticas baseadas em grupo de usuários. Por exemplo, se o administrador de rede quiser definir uma política de QoS para um grupo de usuários, ele poderá apenas criar e distribuir um GPO para esse grupo. Eles não precisam se preocupar com quais computadores esses usuários estão conectados e se esses computadores terão políticas de QoS em conflito definidas, porque, se houver um conflito, a política de nível de usuário sempre terá precedência.

> [!NOTE]
>  Uma política de QoS de nível de usuário só é aplicável ao tráfego gerado por esse usuário. Outros usuários de um computador específico e o próprio computador não estarão sujeitos a nenhuma política de QoS definida para esse usuário.

 **Especificidade do aplicativo e ter precedência sobre a rede quintuple**

Quando várias políticas de QoS correspondem ao tráfego específico, a política mais específica é aplicada. Entre as políticas que identificam aplicativos, uma política que inclui o caminho do arquivo do aplicativo de envio é considerada mais específica do que outra política que identifica apenas o nome do aplicativo (sem caminho). Se várias políticas com aplicativos ainda se aplicarem, as regras de precedência usarão a rede quintuple para encontrar a melhor correspondência.

Como alternativa, várias políticas de QoS podem se aplicar ao mesmo tráfego especificando condições não sobrepostas. Entre as condições de aplicativos e a rede quintuple, a política que especifica o aplicativo é considerada mais específica e é aplicada. 

Por exemplo, policy_A especifica apenas um nome de aplicativo (App. exe) e policy_B especifica o endereço IP de destino 192.168.1.0/24. Quando essas políticas de QoS entram em conflito \(app. exe envia o tráfego para um endereço IP dentro do intervalo de 192.168.4.0/24\), policy_A é aplicado.

 **Mais especificidades têm precedência na rede quintuple**

Para conflitos de política dentro da rede quintuple, a política com a maioria das condições de correspondência tem precedência. Por exemplo, suponha que policy_C especifica o endereço IP de origem "any", o endereço IP de destino 10.0.0.1, a porta de origem "any", a porta de destino "any" e o protocolo "TCP". 

Em seguida, suponha que policy_D especifica o endereço IP de origem "any", o endereço IP de destino 10.0.0.1, a porta de origem "any", a porta de destino 80 e o protocolo "TCP". Em seguida, policy_C e policy_D corresponder as conexões com o destino 10.0.0.1:80. Como a política de QoS aplica a política com as condições de correspondência mais específicas, policy_D tem precedência neste exemplo.  
  
No entanto, as políticas de QoS podem ter um número igual de condições. Por exemplo, várias políticas podem especificar apenas uma (mas não a mesma) parte da rede quintuple. Entre a rede quintuple, a ordem a seguir é de maior ou menor precedência:

- Endereço IP de origem

- Endereço IP de destino

- Porta de origem

- Porta de destino

- Protocolo (TCP ou UDP)

Dentro de uma condição específica, como endereço IP, um endereço IP mais específico é tratado com maior precedência; por exemplo, um endereço IP 192.168.4.1 é mais específico que 192.168.4.0/24.

Projete suas políticas de QoS o mais especificamente possível para simplificar a capacidade da sua organização de entender quais políticas estão em vigor.

Para o próximo tópico deste guia, consulte [eventos e erros de política de QoS](qos-policy-errors.md).

Para o primeiro tópico deste guia, consulte [política de QoS (qualidade de serviço)](qos-policy-top.md).
