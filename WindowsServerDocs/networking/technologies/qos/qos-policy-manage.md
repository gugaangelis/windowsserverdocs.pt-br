---
title: Gerenciar diretiva de QoS
description: Este tópico fornece instruções sobre como criar e gerenciar a diretiva de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5baa83e41c79a558f5bd32f77a234367726592c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-qos-policy"></a>Gerenciar diretiva de QoS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como usar o Assistente de política de QoS para criar, editar ou excluir uma política de QoS.

>[!NOTE]
>  Além de neste tópico, a seguinte documentação de gerenciamento de política de QoS está disponível.
> 
>  - [Erros e eventos de política de QoS](qos-policy-errors.md)

Em sistemas operacionais Windows, política de QoS combina a funcionalidade de QoS baseadas em padrões com a capacidade de gerenciamento de política de grupo. Configuração dessa combinação torna fácil aplicação de políticas de QoS aos objetos de política de grupo. O Windows inclui um Assistente de política de QoS para ajudá-lo a fazer as tarefas a seguir.

-  [Criar uma política de QoS](#bkmk_createpolicy)

-  [Exibir, editar ou excluir uma diretiva de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Criar uma política de QoS

Antes de criar uma política de QoS, é importante que você compreenda os dois controles de QoS principais que são usados para gerenciar o tráfego de rede:

- Valor DSCP

-   Taxa de aceleração

### <a name="prioritizing-traffic-with-dscp"></a>Priorizando o tráfego com DSCP

Conforme observado no exemplo anterior do aplicativo de linha de negócios, você pode definir a prioridade de tráfego de rede de saída usando **especificar valor de DSCP** para configurar uma política de QoS com um valor DSCP específico. 

Conforme descrito em RFC 2474, DSCP permite que os valores de 0 a 63 seja especificado no campo instruções de um pacote IPv4 e dentro do campo de classe de tráfego no IPv6. Roteadores de rede usam o valor DSCP para classificar pacotes de rede e enfileirá-las adequadamente.
  
> [!NOTE]
>  Por padrão, o tráfego do Windows tem um valor de 0 DSCP.
  
O número de filas e seu comportamento de priorização deve ser designados como parte da estratégia de QoS da sua organização. Por exemplo, sua organização pode optar por ter cinco filas: dependentes de latência tráfego, o tráfego de controle, tráfego essenciais, tráfego de melhor esforço e tráfego de transferência de dados em massa.  
  
### <a name="throttling-traffic"></a>Limitação de tráfego

Juntamente com os valores DSCP, a aceleração é outro controle principais para o gerenciamento de largura de banda de rede. Conforme mencionado anteriormente, você pode usar o **especificar taxa de aceleração** configuração para configurar uma política de QoS com uma taxa de aceleração específica para o tráfego de saída. Usando a otimização, uma política de QoS limita o tráfego de rede de saída para uma taxa de aceleração especificado. A marcação e otimização DSCP pode ser usados juntos para gerenciar o tráfego efetivamente.

>[!NOTE]
>Por padrão, o **especificar taxa de aceleração** caixa de seleção não é selecionada.

Para criar uma política de QoS, edite as configurações de política de grupo objeto (GPO) de dentro da ferramenta Console de gerenciamento de política de grupo (GPMC). GPMC, em seguida, abre o Editor de objeto de política de grupo.

QoS nomes de política devem ser exclusivos. Como as políticas são aplicadas a usuários finais e servidores dependem de onde a política de QoS está armazenada no Editor de objeto de diretiva de grupo:

- Uma política de QoS do computador \ Settings\QoS política se aplica a computadores, independentemente do usuário está conectado no momento. Você normalmente usa políticas de QoS baseadas no computador para computadores de servidor.

- Uma política de QoS no usuário \ Settings\QoS política se aplica aos usuários depois que tiver entrado no, independentemente do computador no qual eles fizeram logon.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Para criar uma nova política de QoS com o Assistente de política de QoS

-   No Editor de objeto de diretiva de grupo, clique a **política QoS** nós e clique **criar uma nova política **.

### <a name="wizard-page-1---policy-profile"></a>Página 1 - política perfil do Assistente

Na primeira página do Assistente de política de QoS, você pode especificar um nome de política e configure como QoS controla o tráfego de rede de saída.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Para configurar a página de perfil de política do Assistente de política com base em QoS

1. Em **nome da política**, digite um nome para a política de QoS. O nome deve identificar exclusivamente a política.

2. Opcionalmente, usar **especificar valor de DSCP** para ativar a marcação DSCP e, em seguida, definir um valor DSCP entre 0 e 63.

3. Opcionalmente, usar **especificar taxa de aceleração** para habilitar a otimização do tráfego e configurar a taxa de aceleração. O valor de taxa de aceleração deve ser maior que 1 e você pode especificar unidades de quilobytes por segundo \(KBps\) ou megabytes por segundo \(MBps\).

4. Clique em **próxima**.

### <a name="wizard-page-2---application-name"></a>Página 2 - nome do aplicativo do Assistente

A segunda página do Assistente de política de QoS, você pode aplicar a política para todos os aplicativos, para um aplicativo específico conforme identificado por seu nome executável, como o caminho e nome do aplicativo ou para os aplicativos de servidor HTTP que manipulam solicitações para uma URL específica.

- **Todos os aplicativos** Especifica que as configurações de gerenciamento de tráfego na primeira página do Assistente de QoS política é aplicada a todos os aplicativos.

- **Somente aplicativos com esse nome executável** Especifica se as configurações de gerenciamento de tráfego na primeira página do Assistente de política de QoS estão para um aplicativo específico. O arquivo executável do nome e deve terminar com a extensão de nome de arquivo .exe.

- **Somente aplicativos do servidor HTTP responder às solicitações para essa URL** Especifica que as configurações de gerenciamento de tráfego na primeira página do Assistente de QoS política aplicam a somente determinados aplicativos de servidor HTTP.

Opcionalmente, você pode inserir o caminho do aplicativo. Para especificar um caminho de aplicativo, inclua o caminho com o nome do aplicativo. O caminho pode incluir as variáveis de ambiente. Por exemplo, %ProgramFiles%\My Path\MyApp.exe ou c:\program files\my application path\myapp.exe.

>[!NOTE]
>O caminho do aplicativo não pode incluir um caminho que é resolvido para um link simbólico.

A URL deve estar em conformidade com [RFC 1738](http://tools.ietf.org/html/rfc1738), na forma de `http[s]://<hostname\>:<port\>/<url-path>`. Você pode usar um curinga, `‘*’`, para `<hostname>`e/ou `<port>`, por exemplo, `http://training.\*/, https://\*.\*`, mas o curinga não pode indicar uma subcadeia de caracteres `<hostname>`ou `<port>`.

Em outras palavras, nenhum `http://my\*site/`nem `https://\*training\*/`é válido. 

Opcionalmente, você pode verificar **incluir arquivos e subdiretórios** para realizar a correspondência em todos os subdiretórios e arquivos que seguem uma URL. Por exemplo, se essa opção está marcada e a URL é `http://training`, política de QoS considerarão solicitações de` http://training/video` uma boa correspondência.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Para configurar a página Nome do aplicativo do Assistente de política de QoS

1. Em **QoS esta política se aplica ao**, selecione a opção **todos os aplicativos** ou **somente aplicativos com esse nome executável **.

2. Se você selecionar **somente aplicativos com esse nome executável**, especifique um nome do arquivo executável termina com a extensão de nome de arquivo .exe.

3. Clique em **próxima**.

### <a name="wizard-page-3---ip-addresses"></a>Página 3 - endereços IP do Assistente

Na terceira página do Assistente de política de QoS, você pode especificar as condições de endereço IP para a política de QoS, incluindo o seguinte:

- Todos os endereços IPv4 ou IPv6 ou endereços IPv4 ou IPv6 de origem específicos de origem

- Todos os endereços IPv4 ou IPv6 de destino ou endereços IPv4 ou IPv6 de destino específico

Se você selecionar **somente para o seguinte endereço IP de origem** ou **somente para o seguinte endereço IP de destino**, você deve digitar um destes procedimentos:

- Endereço de um IPv4, como `192.168.1.1`

- Um prefixo de endereço IPv4 usando a notação de comprimento de prefixo de rede, como `192.168.1.0/24`

- Um IPv6 tratam, como `3ffe:ffff::1`

- Endereço de um IPv6 prefixo, como `3ffe:ffff::/48`

Se você selecionar ambos **somente para o seguinte endereço IP de origem** e **somente para o seguinte endereço IP de destino**, endereços ou prefixos devem ser ambos IPv4 ou IPv6 são baseados.

Se você especificou a URL para aplicativos de servidor HTTP na página do assistente anterior, você notará que o endereço IP de origem para a política de QoS nesta página Assistente estiver esmaecido. 

Isso ocorre porque o endereço IP de origem é o endereço do servidor HTTP e não é configurável aqui. Por outro lado, você ainda pode personalizar a política, especificando o endereço IP de destino. Isso possibilita que você crie políticas diferentes para diferentes clientes usando os mesmos aplicativos de servidor HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Para configurar a página de endereços IP do Assistente de política de QoS

1. Em **QoS esta política se aplica ao** (origem), selecione **qualquer endereço IP de origem** ou **somente para o seguinte IP de origem de endereço **.

2. Se você selecionou **apenas o seguinte endereço de origem IP**, especificar um endereço IPv4 ou IPv6 ou prefixo.

3. Em **QoS esta política se aplica ao** (destino), selecione **qualquer endereço de destino** ou **somente para o seguinte endereço IP de destino.**

4. Se você selecionou **somente para o seguinte endereço IP de destino**, especifique um endereço IPv4 ou IPv6 ou o prefixo que corresponde ao tipo de endereço ou prefixo especificado para o endereço de origem.

5.  Clique em **próxima**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Página 4 - protocolos e portas do Assistente

Na quarta página do Assistente de política de QoS, você pode especificar os tipos de tráfego e as portas que são controladas pelas configurações na primeira página do assistente. Você pode especificar:  
-   O tráfego TCP, tráfego UDP ou ambos  

-   Todas as portas, um intervalo de portas de origem ou uma porta de origem específica de origem

-   Todas as portas de destino, um intervalo de portas de destino ou uma porta de destino específica  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Para configurar a página protocolos e portas do Assistente de política de QoS

1. Em **selecionar o protocolo essa política QoS se aplica a**, selecione **TCP**, **UDP**, ou **TCP e UDP **.

2. Em **especificar o número da porta de origem**, selecione **de qualquer porta de origem** ou **esse número de porta de origem **.

3. Se você selecionou **esse número de porta de origem**, digite um número de porta entre 1 e 65535.

     Opcionalmente, você pode especificar um intervalo de portas, no formato "*baixa*:*alto*," onde *baixa* e *alto* representam os limites inferior e superior do intervalo de porta, inclusive. *Baixo* e *alto* cada um deve ser um número entre 1 e 65535. Não são permitidos espaços entre o caractere de dois-pontos (:) e os números.

4. Em **especificar o número da porta de destino**, selecione **para qualquer porta de destino** ou **para este número da porta de destino**.

5. Se você selecionou **para este número da porta de destino** na etapa anterior, digite um número de porta entre 1 e 65535.

Para concluir a criação da nova diretiva de QoS, clique em **concluir** sobre o **protocolos e portas** página do Assistente de política de QoS. Quando concluir, a nova política de QoS é listada no painel de detalhes do Editor de objeto de política de grupo.  
  
Para aplicar as configurações de política de QoS para usuários ou computadores, vincule o GPO no qual as políticas de QoS estão localizadas para um contêiner de serviços de domínio do Active Directory, como um domínio, um site ou uma unidade organizacional (UO).  
  
##  <a name="bkmk_editpolicy"></a>Exibir, editar ou excluir uma diretiva de QoS

As páginas da política QoS Assistente descrito anteriormente correspondem às páginas de propriedades que são exibidas quando você exibe ou edita as propriedades de uma política.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Para exibir as propriedades de uma política de QoS  
  
-   Clique com botão direito o nome da política no painel de detalhes do Editor de objeto de política de grupo e, em seguida, clique em **propriedades**.  
  
     O Editor de objeto de política de grupo exibe a página de propriedades com as seguintes guias:  
  
    -   Perfil de política  
  
    -   Nome do aplicativo  
  
    -   Endereços IP  
  
    -   Protocolos e portas  
  
### <a name="to-edit-a-qos-policy"></a>Para editar uma política de QoS  
  
-   Clique com botão direito o nome da política no painel de detalhes do Editor de objeto de política de grupo e, em seguida, clique em **editar a política existente**.  
  
     O Editor de objeto de política de grupo exibe o **editar uma política existente de QoS** caixa de diálogo.  
  
### <a name="to-delete-a-qos-policy"></a>Para excluir uma diretiva de QoS  
  
-   Clique com botão direito o nome da política no painel de detalhes do Editor de objeto de política de grupo e, em seguida, clique em **Excluir política**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Política de QoS GPMC relatório 

Depois que você tenha aplicado a um número de políticas de QoS em sua organização, talvez seja útil ou necessário a consultar periodicamente em como as políticas são aplicadas. Um resumo das políticas de QoS para um determinado usuário ou computador pode ser exibido usando o GPMC relatório.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Para executar o Assistente de resultados de política de grupo para um relatório de políticas de QoS  
  
-   No GPMC, clique com botão direito do **Group Policy Results** nó e selecione a opção de menu para **Assistente de resultados de política de grupo.**  
  
Depois de resultados da política de grupo são gerados, clique no **configurações** guia. Sobre o **configurações** tab, as políticas de QoS podem ser encontradas em nós "Do computador \ Settings\QoS política" e "Política de Settings\QoS de configuração do usuário".  
  
Sobre o **configurações** tab, as políticas de QoS são listadas por seus nomes de política de QoS com valor DSCP, taxa de aceleração, condições de política e ganhar GPO listados na mesma linha.  
  
O modo de exibição de resultados de política de grupo identifica com exclusividade do GPO vencedor. Quando vários GPOs tem políticas de QoS com o mesmo nome de política de QoS, o GPO com a precedência mais alta do GPO é aplicado. Este é o GPO vencedor. QoS políticas conflitantes (identificadas pelo nome de política) que são anexadas a uma GPO não são aplicadas de prioridade mais baixa. Observe que as prioridades de GPO definem quais políticas de QoS são implantadas no site, domínio ou unidade Organizacional, conforme apropriado. Após a implantação em um nível de usuário ou computador, o [regras de precedência de política de QoS](#BKMK_precedencerules) determinar qual tráfego é permitido ou bloqueado.  
  
A política de QoS valor DSCP, taxa de aceleração e condições de política também ficam visíveis no Editor de objeto de política (GPOE) grupo  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Configurações avançadas para os usuários móveis e remotos  
Com a política de QoS, o objetivo é gerenciar o tráfego na rede da empresa. Em cenários móveis, os usuários podem ser enviando o tráfego ou desativar a rede corporativa. Como políticas de QoS não são relevantes fora de rede da empresa, políticas de QoS são habilitadas somente em interfaces de rede que estão conectados à empresa para Windows 8, Windows 7 ou Windows Vista.  
  
Por exemplo, um usuário pode conectar seu computador portátil a rede da sua empresa por meio de rede virtual privada (VPN) de um café. Para VPN, a interface de rede física (por exemplo, sem fio) não terão QoS políticas aplicadas. No entanto, a interface VPN terão QoS políticas aplicadas porque ele se conecta à empresa. Se o usuário insere mais tarde rede de outra empresa que não tem uma relação de confiança do AD DS, políticas de QoS não serão habilitadas.  
  
Observe que esses cenários móveis não se aplicam a cargas de trabalho do servidor. Por exemplo, um servidor com vários adaptadores de rede talvez sejam posicionados na borda da rede de uma empresa. O departamento de TI pode optar por ter QoS políticas Reduza o tráfego que egresses da empresa; No entanto, esse adaptador de rede que envia o tráfego de saída não necessariamente se conectar novamente à rede corporativa. Por esse motivo, as políticas de QoS são sempre habilitadas em todas as interfaces de rede de um computador executando o Windows Server 2012.  
  
> [!NOTE]
>  Habilitação seletiva só se aplica a políticas de QoS e não para as configurações avançadas de QoS abordadas a seguir neste documento.  
  
### <a name="advanced-qos-settings"></a>Configurações avançadas de QoS

Configurações avançadas de QoS fornecem controles adicionais para os administradores de TI gerenciar o consumo de rede do computador e marcações DSCP. Configurações avançadas de QoS se aplicam somente no nível do computador, enquanto as políticas de QoS podem ser aplicadas em níveis o computador e o usuário.

#### <a name="to-configure-advanced-qos-settings"></a>Para definir configurações avançadas de QoS

1.  Clique em **configuração do computador**e clique em **configurações do Windows na política de grupo**.
  
2.  Clique com botão direito **política QoS**e clique em **configurações avançadas de QoS**.

     A figura a seguir mostra os dois avançada guias de configurações de QoS: **tráfego TCP de entrada** e **substituição de marcação de DSCP**.
  
> [!NOTE]
>  Configurações avançadas de QoS são configurações de política de grupo do nível do computador.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Configurações de QoS avançadas: o tráfego TCP de entrada

**O tráfego de TCP de entrada** controla o consumo de largura de banda TCP no lado do receptor, enquanto as políticas de QoS afetam o tráfego de saída TCP e UDP. 

Definindo uma taxa de transferência de menor nível no **tráfego TCP de entrada** guia, TCP limitará o tamanho dele está anunciado a janela de recepção TCP. O efeito dessa configuração será taxas de transferência maior e utilização do link para conexões TCP com larguras ou latências (produto de atraso de largura de banda). Por padrão, os computadores que executam o Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista são definidos para o nível de taxa de transferência máxima.
  
O TCP receber janela mudou no Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista de versões anteriores do Windows. Versões anteriores do Windows limitado a janela do lado de recebimento TCP até um máximo de 64 KB (Quilobytes), enquanto o Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista dinamicamente dimensionar a janela do lado do recebimento até 16 megabytes (MB). No controle de tráfego de TCP de entrada, você pode controlar o nível de taxa de transferência de entrada, definindo o valor máximo para o qual a TCP receber janela pode aumentar. Os níveis correspondem aos seguintes valores máximos. 
  
|Nível de taxa de transferência de entrada|Máximo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

O tamanho real da janela pode ser um valor igual ou menor do que o máximo, dependendo das condições de rede.

###### <a name="to-set-the-tcp-receive-side-window"></a>Para definir a janela de lado receber TCP

1. No Editor de objeto de diretiva de grupo, clique em **política de computador Local**, clique em **configurações do Windows**, clique com botão direito **política QoS**e clique em **configurações avançadas de QoS**.
  
2. Em **taxa de transferência de recebimento de TCP**, selecione **configurar TCP recebendo Throughput**e, em seguida, selecione o nível de taxa de transferência que você deseja.

3.  Vincule o GPO à unidade Organizacional.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Configurações de QoS avançadas: substituição de marcação de DSCP

Substituição de marcação de DSCP restringe a capacidade de aplicativos para especificar — ou "marcar" — DSCP valores diferentes daqueles especificados nas políticas de QoS. Especificando que os aplicativos são permitidos para definir os valores DSCP, os aplicativos podem definir valores DSCP diferente de zero. 

Especificando **ignorar**, os aplicativos que usam APIs de QoS terão seus valores DSCP definidos como zero e apenas diretivas de QoS podem definir valores DSCP. 

Por padrão, computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista permitem que os aplicativos especificar valores DSCP; aplicativos e dispositivos que não use as APIs de QoS não são substituídos.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valores de multimídia sem fio e DSCP

O [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) estabeleceu uma certificação de multimídia sem fio \(WMM\) que define quatro categorias de acesso \(WMM_AC\) para priorizar o tráfego de rede transmitida em uma rede sem fio Wi\-Fi. As categorias de acesso incluem \ (em ordem de priority\ mais alto menor): voz, vídeo, melhor esforço e em segundo plano; respectivamente abreviada como VO, VI, BE e KB. A especificação WMM define quais DSCP valores correspondem com cada uma das categorias de acesso de quatro:
  
|Valor DSCP|Categoria de acesso WMM|
|----------|-------------------|
|48-63|Voz (VO)|
|32-47|Vídeo (VI)|
|24-31, 0-7|Melhor esforço (BE)|
|8-23|Em segundo plano (KB)|

Você pode criar políticas de QoS que usam esses valores DSCP para garantir que os computadores portáteis com Wi\-Fi Certified™ para adaptadores sem fio WMM recebem priorizada manipulação quando associado Wi\-Fi Certified WMM pontos de acesso.
  
### <a name="BKMK_precedencerules"></a>Regras de precedência de diretiva de QoS

Semelhante às prioridades do GPO, políticas de QoS têm regras de precedência para resolver conflitos quando várias políticas de QoS se aplicam a um conjunto específico de tráfego. Para TCP ou UDP o tráfego de saída, apenas uma política de QoS pode ser aplicada por vez, o que significa que as políticas de QoS não têm um efeito cumulativo, como onde as taxas de aceleração seriam somadas.

Em geral, a política de QoS com o máximo de correspondência condições vence. Quando várias políticas de QoS aplicam, as regras se enquadram em três categorias: nível de usuário versus nível do computador; aplicativo contra a cinco vezes rede; e na rede quintuple.

Por *rede quintuple*, nós significa que o endereço IP de origem, o endereço IP de destino, a porta de origem, a porta de destino e \(TCP/UDP\) de protocolo.  

 **Política de QoS de nível de usuário tem precedência sobre a política de QoS de nível de computador**

Essa regra consideravelmente facilita o gerenciamento de administradores de rede de QoS GPOs, especialmente para políticas do usuário baseados em grupo. Por exemplo, se quiser que o administrador de rede definir uma política de QoS para um grupo de usuários, eles só podem criar e distribuir um GPO para esse grupo. Eles não precisam mais se preocupar sobre quais computadores esses usuários estejam conectados e se esses computadores terão políticas conflitantes de QoS definidas, porque, se houver um conflito, a política de nível de usuário sempre tem precedência.

> [!NOTE]
>  Uma política de QoS de nível de usuário só é aplicável ao tráfego que é gerado pelo usuário. Outros usuários de um computador específico e o computador em si, não estará sujeito a quaisquer políticas de QoS que são definidas para esse usuário.

 **Especificidade de aplicativo e tirar precedência sobre cinco vezes de rede**

Quando várias políticas de QoS corresponderem o tráfego específico, a política mais específica é aplicada. Entre as políticas que identificam aplicativos, uma política que inclui o caminho do arquivo de envio do aplicativo é considerada mais específica do que outra política que identifica somente o nome do aplicativo (sem o caminho). Se várias políticas com aplicativos ainda se aplicam, as regras de precedência usam a rede quintuple para encontrar a melhor correspondência.

Como alternativa, várias políticas de QoS se aplicam ao mesmo tráfego especificando condições não sobrepostas. Entre as condições de aplicativos e os cinco vezes de rede, a política que especifica o aplicativo é considerada mais específica e é aplicada. 

Por exemplo, policy_A especifica somente um aplicativo nome (app.exe) e policy_B Especifica o 192.168.1.0 24 # destino IP endereço. Quando essas políticas de QoS conflito \ (app.exe envia o tráfego para um endereço IP dentro do intervalo de 192.168.4.0/24\), policy_A é aplicado.

 **Mais especificidade tem precedência dentro da rede quintuple**

Para conflitos de política dentro do cinco vezes de rede, a política com as condições mais coincidentes tem precedência. Por exemplo, suponha que policy_C Especifica o endereço de IP de origem "any", IP de destino resolver 10.0.0.1, porta de origem "qualquer" porta de destino "qualquer" e "TCP" de protocolo. 

Em seguida, pressupõem policy_D Especifica o endereço de IP de origem "any", endereço 10.0.0.1, porta de origem "qualquer" porta 80 de destino IP de destino e protocolo "TCP". Em seguida, policy_C e policy_D correspondem conexões com 10.0.0.1:80 de destino. Porque QoS política se aplica a política com as condições de correspondência mais específicas, policy_D tem precedência neste exemplo.  
  
No entanto, políticas de QoS podem ter um número igual de condições. Por exemplo, várias políticas podem cada especificar somente uma (mas não o mesmo) pedaço da cinco vezes de rede. Entre as cinco vezes rede, a ordem a seguir é do maior para diminuir a precedência:

- Endereço IP de origem

- Endereço IP de destino

- Porta de origem

- Porta de destino

- Protocolo (TCP ou UDP)

Dentro de uma condição específica, como endereço IP, um endereço IP mais específico é tratado com uma precedência maior; Por exemplo, um endereço IP 192.168.4.1 é mais específico que 192.168.4.0/24.

Projetar suas políticas de QoS especificamente possível para simplificar a capacidade da sua organização para entender quais políticas estão em vigor.

Para o próximo tópico neste guia, consulte [erros e eventos de política de QoS](qos-policy-errors.md).

Para o primeiro tópico neste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
