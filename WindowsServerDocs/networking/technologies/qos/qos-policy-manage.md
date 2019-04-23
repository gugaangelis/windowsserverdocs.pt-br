---
title: Gerenciar a política de QoS
description: Este tópico fornece instruções sobre como criar e gerenciar a política de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94e5a1832a6c1e160b9cc338d50636026a5eb751
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851667"
---
# <a name="manage-qos-policy"></a>Gerenciar a política de QoS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre como usar o Assistente de política de QoS para criar, editar ou excluir uma política de QoS.

>[!NOTE]
>  Além deste tópico, a seguinte documentação de gerenciamento de política de QoS está disponível.
> 
>  - [Erros e eventos de política de QoS](qos-policy-errors.md)

Em sistemas de operacionais do Windows, política de QoS combina a funcionalidade de QoS baseada em padrões com a capacidade de gerenciamento de diretiva de grupo. A configuração dessa combinação torna fácil aplicação das políticas de QoS a objetos de diretiva de grupo. Windows incluem um Assistente de política de QoS para ajudá-lo a realizar as seguintes tarefas.

-  [Criar uma política de QoS](#bkmk_createpolicy)

-  [Exibir, editar ou excluir uma política de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Criar uma política de QoS

Antes de criar uma política de QoS, é importante que você compreenda os principais e dois controles de QoS que são usados para gerenciar o tráfego de rede:

- Valor DSCP

-   Taxa de aceleração

### <a name="prioritizing-traffic-with-dscp"></a>Priorizando o tráfego com o DSCP

Conforme observado no exemplo anterior do aplicativo de linha de negócios, você pode definir a prioridade do tráfego de rede de saída usando **especificar valor de DSCP** para configurar uma política de QoS com um valor DSCP específico. 

Conforme descrito no RFC 2474, o DSCP permite que valores de 0 a 63 sejam especificados no campo TOS de um pacote IPv4 e no campo Traffic Class do IPv6. Roteadores de rede usam o valor DSCP para classificar pacotes de rede e para a fila-las adequadamente.
  
> [!NOTE]
>  Por padrão, o tráfego do Windows tem um valor DSCP igual a 0.
  
O número de filas e seu comportamento de priorização devem ser planejados como parte da estratégia de QoS de sua organização. Por exemplo, sua organização pode optar por ter cinco filas: tráfego sensível à latência, tráfego de controle, tráfego crítico para os negócios, tráfego de melhor esforço e tráfego de transferência de dados em massa.  
  
### <a name="throttling-traffic"></a>Acelerando o tráfego

Juntamente com os valores DSCP, a limitação é outro controle de tecla para o gerenciamento de largura de banda de rede. Como mencionado anteriormente, você pode usar o **especificar taxa de aceleração** configuração para configurar uma política de QoS com uma taxa de limitação específicas para o tráfego de saída. Ao usar limitação, uma política de QoS limita o tráfego de rede de saída para uma taxa de aceleração especificado. A aceleração e a marcação de DSCP podem ser usadas juntas para gerenciar o tráfego de maneira eficiente.

>[!NOTE]
>Por padrão, a caixa de seleção **Especificar Taxa de Aceleração** não está marcada.

Para criar uma política de QoS, edite as configurações de um grupo de política de GPO (objeto) de dentro da ferramenta do Console de gerenciamento de diretiva de grupo (GPMC). GPMC, em seguida, abre o Editor de objeto de diretiva de grupo.

Os nomes das políticas de QoS devem ser exclusivos. Como as políticas são aplicadas aos servidores e usuários finais depende de onde a política de QoS é armazenada no Editor de objeto de diretiva de grupo:

- Uma política de QoS na política de Settings\QoS de configuração do computador se aplica a computadores, independentemente do usuário que está conectado no momento. Normalmente, você usa políticas de QoS baseadas no computador em servidores.

- Uma política de QoS na política do usuário Configuration\Windows Settings\QoS aplica-se aos usuários depois que eles fizeram logon, independentemente do computador que eles fizeram logon.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Para criar uma nova política de QoS com o Assistente de política de QoS

-   No Editor de objeto de diretiva de grupo, clique em qualquer um dos **política de QoS** nós e clique **criar uma nova política**.

### <a name="wizard-page-1---policy-profile"></a>1 - perfil de diretiva de página do Assistente

Na primeira página do Assistente de política de QoS, você pode especificar um nome de política e configurar como QoS controla o tráfego de rede de saída.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Para configurar a página Perfil da Política do Assistente de QoS Baseado em Política

1. Em **Nome da política**, digite um nome para a política de QoS. O nome deve identificar exclusivamente a política.

2. Opcionalmente, use **especificar valor de DSCP** para habilitar a marcação de DSCP e, em seguida, configure um valor DSCP entre 0 e 63.

3. Como opção, use **Especificar Taxa de Aceleração** para habilitar a aceleração de tráfego e configurar a taxa de aceleração. O valor da taxa de limitação deve ser maior que 1 e você pode especificar unidades de quilobytes por segundo \(KBps\) megabytes por segundo \(MBps\).

4. Clique em **Avançar**.

### <a name="wizard-page-2---application-name"></a>2 - nome do aplicativo de página do Assistente

A segunda página do Assistente de política de QoS, você pode aplicar a política a todos os aplicativos, um aplicativo específico conforme identificado por seu nome de executável, para um caminho e o nome do aplicativo ou aos aplicativos de servidor HTTP que manipulam as solicitações para uma URL específica.

- **Todos os aplicativos** Especifica que as configurações de gerenciamento de tráfego na primeira página do Assistente de política de QoS se aplica a todos os aplicativos.

- **Somente aplicativos com este nome executável** Especifica que as configurações de gerenciamento de tráfego na primeira página do Assistente de política de QoS são para um aplicativo específico. O nome do arquivo executável deve terminar com a extensão de nome de arquivo .exe.

- **Somente aplicativos do servidor HTTP respondendo a solicitações para essa URL** Especifica que as configurações de gerenciamento de tráfego na primeira página do Assistente de política de QoS se aplicam a apenas certos aplicativos de servidor HTTP.

Como opção, você pode inserir o caminho do aplicativo. Para especial um caminho de aplicativo, inclua o caminho com o nome do aplicativo. O caminho pode incluir variáveis de ambiente. Por exemplo, %ProgramFiles%\Caminho do Meu Aplicativo\MeuAplicativo.exe ou c:\arquivos de programas\caminho do meu aplicativo\meuaplicativo.exe.

>[!NOTE]
>O caminho do aplicativo não pode incluir um caminho que aponta para um link simbólico.

A URL deve obedecer às [RFC 1738](https://tools.ietf.org/html/rfc1738), na forma de `http[s]://<hostname\>:<port\>/<url-path>`. Você pode usar um caractere curinga `‘*’`, para `<hostname>` e/ou `<port>`, por exemplo, `https://training.\*/, https://\*.\*`, mas o caractere curinga não pode indicar uma subcadeia de caracteres de `<hostname>` ou `<port>`.

Em outras palavras, nenhum dos dois `https://my\*site/` nem `https://\*training\*/` é válido. 

Opcionalmente, você pode verificar **incluir subdiretórios e arquivos** para fazer a correspondência em todos os subdiretórios e arquivos de uma URL a seguir. Por exemplo, se esta opção estiver marcada e a URL é `https://training`, política de QoS considerará as solicitações de` https://training/video` uma boa correspondência.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Para configurar a página de nome do aplicativo do Assistente de política de QoS

1. Na **esta política de QoS se aplica ao**, selecione **todos os aplicativos** ou **somente aplicativos com este nome executável**.

2. Se você selecionar **Somente aplicativos com este nome executável**, especifique o nome de um executável que termine com a extensão de nome de arquivo .exe.

3. Clique em **Avançar**.

### <a name="wizard-page-3---ip-addresses"></a>Página 3 - endereços IP do Assistente

Na terceira página do Assistente de política de QoS, você pode especificar as condições de endereço IP para a política de QoS, incluindo o seguinte:

- Todos os endereços IPv4 ou IPv6 de origem ou endereços IPv4 ou IPv6 de origem específicos

- Todos os endereços IPv4 ou IPv6 de destino ou endereços IPv4 ou IPv6 de destino específico

Se você selecionar **Somente para o seguinte endereço IP de origem** ou **Somente para o seguinte endereço IP de destino**, será necessário digitar uma das seguintes opções:

- Endereço IPv4, como `192.168.1.1`

- Um prefixo de endereço IPv4 usando a notação de comprimento de prefixo de rede, como `192.168.1.0/24`

- Um IPv6 endereço, como `3ffe:ffff::1`

- Um IPv6 endereço de prefixo, como `3ffe:ffff::/48`

Se você selecionar ambas **somente para o seguinte endereço IP de origem** e **somente para o seguinte endereço IP de destino**, endereços ou prefixos de endereço devem ser ambos IPv4 ou IPv6 são baseados.

Se você especificou a URL de HTTP para aplicativos de servidor na página anterior do assistente, você observará que o endereço IP de origem para a política de QoS nesta página do assistente está esmaecido. 

Isso é verdadeiro porque o endereço IP de origem é o endereço do servidor HTTP e não é configurável aqui. Por outro lado, você ainda pode personalizar a política, especificando o endereço IP de destino. Isso torna possível para você criar políticas diferentes para diferentes clientes usando os mesmos aplicativos de servidor HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Para configurar a página endereços IP do Assistente de política de QoS

1. Na **esta política de QoS se aplica ao** (origem), selecione **qualquer endereço IP de origem** ou **apenas para o IP seguinte endereço de origem**.

2. Se você selecionou **somente o seguinte endereço IP de origem**, especifique um endereço IPv4 ou IPv6 ou prefixo.

3. Na **esta política de QoS se aplica ao** (destino), selecione **qualquer endereço de destino** ou **somente para o seguinte endereço IP de destino.**

4. Se você selecionou **somente para o seguinte endereço IP de destino**, especifique um endereço IPv4 ou IPv6 ou o prefixo que corresponde ao tipo de endereço ou prefixo especificado para o endereço de origem.

5.  Clique em **Avançar**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Página 4 - protocolos e portas do Assistente

Na quarta página do Assistente de política de QoS, você pode especificar os tipos de tráfego e as portas que são controladas pelas configurações na primeira página do assistente. Você pode especificar:  
-   Tráfego TCP, tráfego UDP ou os dois  

-   Todas as portas de origem, um intervalo de portas de origem ou uma porta de origem específica

-   Todas as portas de destino, um intervalo de portas de destino ou uma porta de destino específico  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Para configurar a página protocolos e portas do Assistente de política de QoS

1. Em **Selecionar o protocolo ao qual esta política de QoS se aplica**, selecione **TCP**, **UDP** ou **TCP e UDP**.

2. Em **Especifique o número da porta de origem**, selecione **De qualquer porta de origem** ou **Deste número de porta de origem**.

3. Se você selecionou **deste número de porta de origem**, digite um número de porta entre 1 e 65535.

     Opcionalmente, você pode especificar um intervalo de portas no formato "*baixa*:*alta*," em que *baixo* e *alta* representam os limites inferiores e intervalo de limites superiores da porta, inclusive. *Baixa* e *alta* cada um deve ser um número entre 1 e 65535. Não são permitidos espaços entre o caractere de dois pontos (:) e os números.

4. Em **Especifique o número da porta de destino**, selecione **Para qualquer porta de destino** ou **Para este número de porta de destino**.

5. Se você selecionou **Para este número de porta de destino** na etapa anterior, digite um número de porta entre 1 e 65535.

Para concluir a criação da nova política de QoS, clique em **concluir** sobre o **protocolos e portas** página do Assistente de política de QoS. Quando concluído, a nova política de QoS é listada no painel de detalhes do Editor de objeto de diretiva de grupo.  
  
Para aplicar as configurações de política de QoS a usuários ou computadores, vincule o GPO em que as políticas de QoS estão localizadas a um contêiner de serviços de domínio do Active Directory, como um domínio, um site ou uma unidade organizacional (UO).  
  
##  <a name="bkmk_editpolicy"></a>Exibir, editar ou excluir uma política de QoS

As páginas da política de QoS Assistente descrito anteriormente correspondem às páginas de propriedades que são exibidas quando você exibir ou edita as propriedades de uma política.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Para exibir as propriedades de uma política de QoS  
  
-   O nome da política no painel de detalhes do Editor de objeto de diretiva de grupo com o botão direito e, em seguida, clique em **propriedades**.  
  
     O Editor de objeto de diretiva de grupo exibe a página de propriedades com as seguintes guias:  
  
    -   Perfil da Política  
  
    -   Nome do aplicativo  
  
    -   Endereços IP  
  
    -   Protocolos e portas  
  
### <a name="to-edit-a-qos-policy"></a>Para editar uma política de QoS  
  
-   O nome da política no painel de detalhes do Editor de objeto de diretiva de grupo com o botão direito e, em seguida, clique em **Editar política existente**.  
  
     O Editor de objeto de diretiva de grupo exibe os **editar uma política de QoS existente** caixa de diálogo.  
  
### <a name="to-delete-a-qos-policy"></a>Para excluir uma diretiva de QoS  
  
-   O nome da política no painel de detalhes do Editor de objeto de diretiva de grupo com o botão direito e, em seguida, clique em **Excluir política**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Relatórios do GPMC de política de QoS 

Depois de aplicar um número de políticas de QoS em sua organização, pode ser útil ou necessário revisar periodicamente como as políticas são aplicadas. Um resumo das políticas de QoS para um usuário ou computador específico pode ser exibido por meio de relatórios do GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Para executar o Assistente de resultados de política de grupo para um relatório das políticas de QoS  
  
-   No GPMC, clique com botão direito do **resultados da diretiva de grupo** nó e, em seguida, selecione a opção de menu para **Assistente de resultados de diretiva de grupo.**  
  
Após gerar resultados de diretiva de grupo, clique no **configurações** guia. Sobre o **configurações** guia, as políticas de QoS podem ser encontradas em nós "Política de Settings\QoS de configuração do computador" e "Política de Settings\QoS de configuração do usuário".  
  
Sobre o **configurações** guia, as políticas de QoS são listadas por seus nomes de política de QoS com valor de DSCP, taxa de aceleração, condições da política e vencedores de GPO listado na mesma linha...  
  
O modo de exibição de resultados de diretiva de grupo identifica exclusivamente o GPO vencedor. Quando vários GPOs têm políticas de QoS com o mesmo nome de política de QoS, o GPO com a precedência mais alta do GPO é aplicado. Isso é o GPO vencedor. Conflito de políticas de QoS (identificadas pelo nome da política) que estão anexadas a uma prioridade mais baixa GPO não são aplicadas. Observe que as prioridades de GPO definem quais políticas de QoS são implantadas no site, domínio ou UO, conforme apropriado. Após a implantação, em um nível de usuário ou computador, o [regras de precedência de política de QoS](#BKMK_precedencerules) determinar qual tráfego é permitido ou bloqueado.  
  
O valor DSCP a política de QoS, taxa de aceleração e condições da política também são visíveis no grupo de política de objeto GPOE (Editor)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Configurações avançadas para usuários móveis e remotos  
Com a política de QoS, a meta é gerenciar o tráfego na rede da empresa. Em cenários móveis, os usuários podem estar enviando o tráfego ou desativar a rede corporativa. Como as políticas de QoS não são relevantes mesmo longe de rede da empresa, políticas de QoS são habilitadas somente em interfaces de rede que estão conectados à empresa para Windows 8, Windows 7 ou Windows Vista.  
  
Por exemplo, um usuário pode conectar seu computador portátil à rede da sua empresa por meio de uma rede virtual privada (VPN) de um café. Para VPN, a interface de rede física (por exemplo, sem fio) não terá políticas de QoS aplicadas. No entanto, a interface VPN terá políticas de QoS aplicadas porque ele se conecta à empresa. Se o usuário inserir posteriormente a rede de outra empresa que não tem uma relação de confiança do AD DS, as políticas de QoS não serão habilitadas.  
  
Observe que esses cenários móveis não se aplicam às cargas de trabalho do servidor. Por exemplo, um servidor com vários adaptadores de rede poderão ficar na borda da rede de uma empresa. O departamento de TI pode escolher ter tráfego de acelerador de políticas de QoS que egresses da empresa; No entanto, esse adaptador de rede que envia o tráfego de saída não necessariamente conectam-se novamente à rede corporativa. Por esse motivo, as políticas de QoS estão sempre habilitadas em todas as interfaces de rede de um computador executando o Windows Server 2012.  
  
> [!NOTE]
>  Habilitação seletiva só se aplica às políticas de QoS e não para as configurações avançadas de QoS discutidas a seguir neste documento.  
  
### <a name="advanced-qos-settings"></a>Configurações avançadas de QoS

Configurações avançadas de QoS fornecem controles adicionais para os administradores de TI gerenciar o consumo de rede do computador e as marcações de DSCP. Configurações avançadas de QoS se aplica somente no nível do computador, enquanto as políticas de QoS podem ser aplicadas nos níveis do computador e o usuário.

#### <a name="to-configure-advanced-qos-settings"></a>Definir configurações avançadas de QoS

1.  Clique em **configuração do computador**e, em seguida, clique em **configurações do Windows na política de grupo**.
  
2.  Clique com botão direito **política de QoS**e, em seguida, clique em **configurações avançadas de QoS**.

     A figura a seguir mostra que os dois avançadas guias de configurações de QoS: **O tráfego TCP de entrada** e **substituição de marcação DSCP**.
  
> [!NOTE]
>  Configurações avançadas de QoS são configurações de diretiva de grupo de nível de computador.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Configurações de QoS avançadas: o tráfego TCP de entrada

**O tráfego TCP de entrada** controla o consumo de largura de banda TCP no lado do destinatário, ao passo que as políticas de QoS afetam o tráfego de TCP e UDP de saída. 

Definindo uma taxa de transferência menor nível na **tráfego TCP de entrada** guia, o TCP limitará o tamanho do seu TCP anunciado janela de recepção. O efeito dessa configuração serão as taxas de maior taxa de transferência e utilização do link para conexões TCP com larguras de banda maiores ou latências (produto de atraso da largura de banda). Por padrão, os computadores que executam o Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista são definidas para o nível de taxa de transferência máxima.
  
A recepção TCP janela foi alterada no Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista de versões anteriores do Windows. As versões anteriores do Windows limitada a janela do lado de recebimento do TCP a um máximo de 64 quilobytes (KB), enquanto o Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista dimensionar dinamicamente a janela do lado de recebimento até 16 MB (Megabytes ). No controle de tráfego de entrada TCP, você pode controlar o nível de taxa de transferência de entrada, definindo o valor máximo que pode atingir o-janela de recepção TCP. Os níveis de correspondem para os seguintes valores máximos. 
  
|Nível de taxa de transferência de entrada|Máximo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

O tamanho de janela real pode ser um valor igual ou menor do que o máximo, dependendo das condições da rede.

###### <a name="to-set-the-tcp-receive-side-window"></a>Para definir a janela do lado de recebimento de TCP

1. No Editor de objeto de diretiva de grupo, clique em **política do computador Local**, clique em **configurações do Windows**, clique com botão direito **política de QoS**e, em seguida, clique em **avançadas de QoS Configurações**.
  
2. Na **taxa de transferência de recebimento do TCP**, selecione **configurar produtividade de recebimento do TCP**e, em seguida, selecione o nível de taxa de transferência que você deseja.

3.  Vincule o GPO à UO.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Configurações avançadas de QoS: Substituição de marcação de DSCP

Substituição de marcação de DSCP restringe a capacidade de aplicativos especifiquem — ou "marcar" — DSCP valores diferentes daquelas especificadas na políticas de QoS. Especificando que os aplicativos têm permissão para configurar valores DSCP, os aplicativos podem definir valores DSCP diferente de zero. 

Especificando **ignorar**, aplicativos que usam APIs de QoS terão seus valores DSCP definidos como zero, e somente as políticas de QoS podem definir valores DSCP. 

Por padrão, computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista permitem que aplicativos especifiquem valores DSCP; aplicativos e dispositivos que não usam as APIs de QoS não serão substituídos.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valores DSCP e multimídia sem fio

O [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) estabeleceu uma certificação para multimídia sem fio \(WMM\) que define quatro categorias de acesso \(WMM_AC\) para priorizar o tráfego de rede transmitido em um Wi\-Fi de rede sem fio. As categorias de acesso incluem \(em ordem de prioridade mais alta menor\): voz, vídeo, melhor esforço e segundo plano; respectivamente abreviado como VO, VI, BE e BK. A especificação WMM define quais DSCP valores correspondem com cada uma das categorias de acesso de quatro:
  
|Valor DSCP|Categoria de acesso do WMM|
|----------|-------------------|
|48-63|Voz (VO)|
|32-47|Vídeo (VI)|
|24-31, 0-7|Melhor esforço (BE)|
|8-23|Em segundo plano (KB)|

Você pode criar políticas de QoS que usam esses valores DSCP para garantir que portátil computadores com Wi\-Fi Certified™ para adaptadores sem fio do WMM recebem priorizados tratamento quando associada Wi\-Fi certificados para o WMM pontos de acesso.
  
### <a name="BKMK_precedencerules"></a>Regras de precedência de política de QoS

Semelhante às prioridades do GPO, as políticas de QoS têm regras de precedência para resolver conflitos quando várias políticas de QoS se aplica a um conjunto específico de tráfego. Para o tráfego TCP ou UDP saído, apenas uma política de QoS pode ser aplicada por vez, o que significa que as políticas de QoS não tem um efeito cumulativo, como onde as taxas de limitação devem ser somadas.

Em geral, a política de QoS com condições de correspondência mais wins. Quando várias políticas de QoS se aplica, as regras se enquadram em três categorias: nível de usuário em comparação com o nível do computador; aplicativo versus quíntuplo a rede; e entre a rede quintuple.

Por *rede quintuple*, queremos dizer o endereço IP de origem, endereço IP de destino, porta de origem, porta de destino e protocolo \(TCP/UDP\).  

 **Política de QoS de nível de usuário tem precedência sobre a política de QoS de nível de computador**

Essa regra facilita bastante o gerenciamento de administradores de rede de GPOs de QoS, particularmente para as políticas com base no grupo de usuário. Por exemplo, se quiser que o administrador de rede definir uma política de QoS para um grupo de usuários, eles apenas podem criar e distribuir um GPO a esse grupo. Eles não precisam se preocupar sobre quais computadores desses usuários estão conectados e se esses computadores terão conflitantes políticas de QoS definidas, porque, se houver um conflito, a política de nível de usuário sempre terá precedência.

> [!NOTE]
>  Uma política de QoS de nível de usuário só é aplicável para o tráfego que é gerado por esse usuário. Outros usuários de um computador específico e o computador em si, não poderá ser sujeito a quaisquer políticas de QoS que são definidas para esse usuário.

 **Especificidade do aplicativo e tendo precedência sobre o failover quíntuplo de rede**

Quando várias políticas de QoS correspondem ao tráfego específico, a política mais específica é aplicada. Entre as políticas que identificam os aplicativos, uma política que inclui o caminho do arquivo do aplicativo remetente é considerada mais específica do que outra política que só identifica o nome do aplicativo (sem o caminho). Se várias políticas de aplicativos ainda se aplicam, as regras de precedência usam a rede quintuple para encontrar a melhor correspondência.

Como alternativa, várias diretivas de QoS podem se aplicar ao tráfego mesmo especificando condições não sobrepostos. Entre as condições de aplicativos e o quíntuplo de rede, a política que especifica o aplicativo é considerada mais específica e é aplicada. 

Por exemplo, policy_A especifica apenas um nome de aplicativo (app.exe) e policy_B Especifica o destino IP endereço 192.168.1.0/24. Quando essas políticas de QoS em conflito \(app.exe envia o tráfego para um endereço IP dentro do intervalo de 192.168.4.0/24\), policy_A é aplicado.

 **Mais de especificidade tem precedência dentro da rede quintuple**

Para os conflitos de política dentro de cinco vezes de rede, a política com as condições de correspondência mais terá precedência. Por exemplo, suponha que policy_C Especifica o endereço de IP de origem "qualquer", o endereço IP de destino 10.0.0.1, porta de origem "qualquer", "qualquer" da porta no destino e protocolo "TCP". 

Em seguida, suponha policy_D Especifica o endereço de IP de origem "qualquer", "qualquer" porta 80 do destino de 10.0.0.1, porta de origem de endereço IP de destino e "TCP" de protocolo. Em seguida, policy_C e policy_D correspondem conexões para 10.0.0.1: 80 de destino. Como diretiva de QoS se aplica a política com as condições de correspondência mais específicas, policy_D tem precedência neste exemplo.  
  
No entanto, as políticas de QoS podem ter um número igual de condições. Por exemplo, várias políticas podem especificam apenas um (mas não igual) pedaço de quíntuplo a rede. Entre quíntuplo rede, a ordem a seguir é do maior para o menor precedência:

- Endereço IP de origem

- Endereço IP de destino

- Porta de origem

- Porta de destino

- Protocolo (TCP ou UDP)

Dentro de uma condição específica, como o endereço IP, um endereço IP mais específico é tratado com precedência mais alta; Por exemplo, um endereço IP 192.168.4.1 é mais específico que 192.168.4.0/24.

Projetar suas políticas de QoS de maneira mais específica possível para simplificar a capacidade da sua organização para entender quais políticas estão em vigor.

Para o próximo tópico neste guia, consulte [erros e eventos de política de QoS](qos-policy-errors.md).

Para o primeiro tópico deste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
