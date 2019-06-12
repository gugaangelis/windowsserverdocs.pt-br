---
title: Implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseado em senha 802.1 X acesso autenticado sem fio"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b2e237cee6eac6be809add37a2ac29fdf1c92118
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446490"
---
# <a name="wireless-access-deployment"></a>Implantação de acesso sem fio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Siga estas etapas para implantar o acesso sem fio:

- [Implantar e configurar o APs sem fio](#bkmk_aps)

- [Criar um grupo de segurança de usuários sem fio](#bkmk_groups)

- [Configurar a rede sem fio \(IEEE 802.11\) políticas](#bkmk_policies)

- [Configurar NPSs](#bkmk_nps)

- [Novos computadores sem fio ingressar no domínio](#bkmk_domain)

## <a name="bkmk_aps"></a>Implantar e configurar o APs sem fio

Siga estas etapas para implantar e configurar seus PAS sem fio:

- [Especificar as frequências de canal de AP sem fio](#bkmk_channel)

- [Configurar APs sem fio](#bkmk_wirelessaps)

>[!NOTE]
>Os procedimentos deste guia não incluem instruções para os casos em que a caixa de diálogo **Controle de Conta de Usuário** é aberta para solicitar sua permissão para continuar. Caso essa caixa de diálogo seja aberta durante a execução dos procedimentos deste guia e em resposta às suas ações, clique em **Continuar**.

### <a name="bkmk_channel"></a>Especificar as frequências de canal de AP sem fio

Quando você implanta vários APs sem fio em um único local geográfico, você deve configurar APs sem fio que possuem sinais sobrepostos usar frequências de canal exclusivo para reduzir a interferência entre APs sem fio.

Você pode usar as diretrizes a seguir para ajudá-lo a escolher as frequências de canal que não entrem em conflito com outras redes sem fio no local geográfico da sua rede sem fio.

- Se houver outras organizações que têm escritórios em proximidade ou no mesmo prédio que sua organização, identificam se há redes sem fio pertencentes às organizações. Descubra o posicionamento e o canal atribuído frequências do seu PA sem fio, porque você precisa atribuir as frequências de canal diferente para seu ponto de acesso e você precisa determinar o melhor local para instalar o AP

- Identifique a sobreposição de sinais sem fio andares adjacentes dentro da sua organização. Depois que identificar áreas de cobertura fora e dentro de sua organização, sobreposição atribuir frequências de canal APs sem fio, garantindo que quaisquer dois APs sem fio com sobreposição de cobertura recebem frequências de canal diferente.

### <a name="bkmk_wirelessaps"></a>Configurar APs sem fio

Use as informações a seguir, juntamente com a documentação do produto fornecida pelo fabricante do AP sem fio para configurar seus PAS sem fio.

Esse procedimento enumera itens normalmente configurados em um AP sem fio. Os nomes de item podem variar de acordo com a marca e modelo e podem ser diferentes na lista a seguir. Para obter detalhes específicos, consulte a documentação de AP sem fio.

#### <a name="to-configure-your-wireless-aps"></a>Para configurar seus PAS sem fio  

- **SSID**. Especifique o nome da rede sem fio\(s\) \(por exemplo, ExampleWLAN\). Esse é o nome que é anunciado para os clientes sem fio.

- **Criptografia**. Especificar WPA2\-Enterprise \(preferencial\) ou WPA\-Enterprise e o AES \(preferencial\) ou codificação de criptografia TKIP, dependendo de quais versões têm suporte pelo seu adaptadores de rede do computador cliente sem fio.

- **Sem fio de endereço IP do PA \(estáticos\)** . Em cada ponto de acesso, configure um endereço IP estático exclusivo que esteja dentro do intervalo de exclusão do escopo DHCP para a sub-rede. Usando um endereço que é excluído da atribuição pelo DHCP impede que o servidor DHCP atribuindo o mesmo endereço IP para um computador ou outro dispositivo.

- **Máscara de sub-rede**. Configure isso para corresponder às configurações de máscara de sub-rede da LAN à qual você se conectou o AP sem fio.  

- **Nome DNS**. Alguns APs sem fio podem ser configurados com um nome DNS. O serviço DNS na rede pode resolver nomes DNS para um endereço IP. Em cada AP sem fio que dá suporte a esse recurso, insira um nome exclusivo para a resolução DNS.  

- **Serviço DHCP**. Se seu PA sem fio tem um built\-no serviço DHCP, desabilitá-lo.  

- **Segredo compartilhado RADIUS**. Use um segredo compartilhado RADIUS como exclusivo para cada PA sem fio, a menos que você estiver planejando configurar pontos de acesso como clientes RADIUS no NPS por grupo. Se você planeja configurar APs por grupo no NPS, o segredo compartilhado deve ser o mesmo para todos os membros do grupo. Além disso, cada segredo compartilhado que você usar deve ser uma sequência aleatória de pelo menos 22 caracteres que combine letras maiusculas e minúsculas, números e pontuação. Para garantir a aleatoriedade, você pode usar um gerador aleatório de caractere, como o gerador aleatório de caracteres encontrado no NPS **configurar 802.1 X** Assistente para criar os segredos compartilhados.

>[!TIP]
>Registre o segredo compartilhado para cada AP sem fio e armazená-lo em um local seguro, como um seguro do office. Você deve saber o segredo compartilhado para cada PA sem fio ao configurar clientes RADIUS no NPS.  

- **Endereço IP do servidor RADIUS**. Digite o endereço IP do servidor que executa o NPS.

- **A porta UDP\(s\)** . Por padrão, o NPS usa as portas UDP 1812 e 1645 para mensagens de autenticação e portas UDP 1813 e 1646 para mensagens de estatísticas. É recomendável que você use as mesmas portas UDP em seus PAS, mas se você tiver um motivo válido para usar portas diferentes, certifique-se de que você não apenas configurar os pontos de acesso com os novos números de porta, mas também reconfigurar todos os seus NPSs para usar os mesmos números de porta como pontos de acesso. Se os pontos de acesso e os NPSs não estão configurados com as mesmas portas UDP, o NPS não é possível receber ou processar solicitações de conexão dos pontos de acesso, e todas as tentativas de conexão sem fio na rede falhará.

- **VSAs**. Alguns APs sem fio exigem fornecedor\-atributos específicos \(VSAs\) para fornecer a funcionalidade de AP sem fio completo. VSAs são adicionados na diretiva de rede do NPS.

- **Filtragem de DHCP**. Configure APs sem fio para bloquear clientes sem fio de enviar pacotes IP da porta UDP 68 para a rede, conforme documentado pelo fabricante do AP sem fio.

- **Filtragem DNS**. Configure APs sem fio para bloquear clientes sem fio de enviar pacotes IP da porta TCP ou UDP 53 à rede, conforme documentado pelo fabricante do AP sem fio.

## <a name="create-security-groups-for-wireless-users"></a>Criar grupos de segurança para usuários sem fio

Siga estas etapas para criar grupos de segurança de um ou mais usuários sem fio e, em seguida, adicionar usuários ao grupo de segurança de usuários sem fio apropriados:

- [Criar um grupo de segurança de usuários sem fio](#bkmk_groups)

- [Adicionar usuários ao grupo de segurança sem fio](#bkmk_addusers)

### <a name="bkmk_groups"></a>Criar um grupo de segurança de usuários sem fio

Você pode usar este procedimento para criar um grupo de segurança sem fio no Console de gerenciamento Microsoft de computadores e usuários do Active Directory \(MMC\) encaixar\-no.  

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-create-a-wireless-users-security-group"></a>Para criar um grupo de segurança de usuários sem fio

1. Clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. O Active Directory Users and Computers snap\-no é aberta. Caso ainda não esteja selecionado, clique no nó do seu domínio. Por exemplo, se o seu domínio for exemplo.com, clique em **exemplo.com**.

2. No painel de detalhes, com o botão direito\-clique na pasta na qual você deseja adicionar um novo grupo \(por exemplo, com o botão direito\-clique em **usuários**\), aponte para **novo**, e, em seguida, clique em **grupo**.

3. Em **Novo Objeto – Grupo**, em **Nome do grupo**, digite o nome do novo grupo. Por exemplo, digite **grupo sem fio**.

4. Em **Escopo do grupo**, selecione uma das seguintes opções:

    - **Domínio local**

    - **Global**

    - **Universal**

5. Em **Tipo de grupo**, selecione **Segurança**.

6. Clique em **OK**.

Se você precisar de mais de um grupo de segurança para usuários sem fio, repita essas etapas para criar grupos de usuários sem fio adicionais. Posteriormente, você pode criar políticas de rede individuais no NPS para aplicar diferentes condições e restrições para cada grupo, fornecendo a eles permissões de acesso diferentes e regras de conectividade.

### <a name="bkmk_addusers"></a> Adicionar usuários ao grupo de segurança de usuários sem fio

Você pode usar este procedimento para adicionar um usuário, computador ou grupo ao seu grupo de segurança sem fio no Console de gerenciamento Microsoft de computadores e usuários do Active Directory \(MMC\) encaixar\-no.

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Para adicionar usuários ao grupo de segurança sem fio

1. Clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. O MMC Usuários e Computadores do Active Directory é aberto. Caso ainda não esteja selecionado, clique no nó do seu domínio. Por exemplo, se o seu domínio for exemplo.com, clique em **exemplo.com**.

2. No painel de detalhes, clique duas vezes\-clique na pasta que contém o grupo de segurança sem fio.

3. No painel de detalhes, com o botão direito\-clique no grupo de segurança sem fio e, em seguida, clique em **propriedades**. O **propriedades** abre a caixa de diálogo para o grupo de segurança.

4. Sobre o **membros** , clique em **Add**e, em seguida, conclua um dos procedimentos a seguir para adicionar um computador ou adicionar um usuário ou grupo.

##### <a name="to-add-a-user-or-group"></a>Para adicionar um usuário ou grupo

1. Na **digite os nomes de objeto para selecionar**, digite o nome do usuário ou grupo que você deseja adicionar e clique **Okey**.

2. Para atribuir a associação de grupo para outros usuários ou grupos, repita a etapa 1 deste procedimento.  

##### <a name="to-add-a-computer"></a>Para adicionar um computador

1. Clique em **Tipos de Objeto**. O **tipos de objeto** caixa de diálogo é aberta.

2. Na **tipos de objeto**, selecione **computadores**e, em seguida, clique em **Okey**.

3. Na **digite os nomes de objeto para selecionar**, digite o nome do computador que você deseja adicionar e, em seguida, clique em **Okey**.

4. Para atribuir a associação de grupo para outros computadores, repita as etapas 1\-3 deste procedimento.

## <a name="bkmk_policies"></a>Configurar a rede sem fio \(IEEE 802.11\) políticas

Siga estas etapas para configurar a rede sem fio \(IEEE 802.11\) extensão de diretiva de grupo:

- [Abrir ou adicionar e abrir um objeto de diretiva de grupo](#bkmk_opengpme)

- [Ativar o padrão de rede sem fio \(IEEE 802.11\) políticas](#bkmk_activate)

- [Configurar a nova política de rede sem fio](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Abrir ou adicionar e abrir um objeto de diretiva de grupo

Por padrão, o recurso Gerenciamento de diretiva de grupo é instalado em computadores que executam o Windows Server 2016 ao serviços de domínio do Active Directory \(AD DS\) função de servidor está instalada e o servidor está configurado como um controlador de domínio. O procedimento a seguir descreve como abrir o Console de gerenciamento de diretiva de grupo \(GPMC\) no controlador de domínio. O procedimento, em seguida, descreve como a abrir um domínio existente\-objeto de diretiva de grupo de nível \(GPO\) para edição, ou criar um novo GPO de domínio e abri-lo para edição.

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir ou adicionar e abrir um objeto de diretiva de grupo

1. No controlador de domínio, clique em **inicie**, clique em **ferramentas administrativas do Windows**e, em seguida, clique em **Group Policy Management**. Abre o Console de gerenciamento de diretiva de grupo.  

2. No painel esquerdo, clique duas vezes\-clique em sua floresta. Por exemplo, clique duas vezes\-clique em **floresta: exemplo.com**.  

3. No painel esquerdo, clique duas vezes\-clique em **domínios**e, em seguida, clique duas vezes\-clique no domínio para o qual você deseja gerenciar um objeto de diretiva de grupo. Por exemplo, clique duas vezes\-clique em **exemplo.com**.  

4. Siga um destes procedimentos:

    -   **Para abrir um domínio existente\-nível de GPO para edição**, clique duas vezes em um domínio que contém o objeto de diretiva de grupo que você deseja gerenciar, à direita\-clique na política de domínio que você deseja gerenciar, como a diretiva de domínio padrão e, em seguida, clique em **editar**. **Editor de gerenciamento de diretiva de grupo** é aberta.

    -   **Para criar um novo objeto de diretiva de grupo e abrir para edição**certo\-clique no domínio para o qual você deseja criar um novo objeto de diretiva de grupo e, em seguida, clique em **criar um GPO neste domínio e vinculá-lo aqui**.

        Na **novo GPO**, na **nome**, digite um nome para o novo objeto de diretiva de grupo e, em seguida, clique em **Okey**.

        À direita\-clique em seu novo objeto de diretiva de grupo e, em seguida, clique em **editar**. **Editor de gerenciamento de diretiva de grupo** é aberta.

Na próxima seção, você usará o Editor de gerenciamento de diretiva de grupo para criar a política sem fio.

### <a name="bkmk_activate"></a>Ativar o padrão de rede sem fio \(IEEE 802.11\) políticas

Este procedimento descreve como ativar o padrão de rede sem fio \(IEEE 802.11\) políticas usando o Editor de gerenciamento de diretiva de grupo \(GPME\).

>[!NOTE]
>Depois de ativar o **Windows Vista e versões posteriores** versão da rede sem fio \(IEEE 802.11\) políticas ou o **Windows XP** versão, a opção de versão é removido automaticamente da lista de opções quando você com o botão direito\-clique em **rede sem fio \(IEEE 802.11\) políticas**. Isso ocorre porque, depois de selecionar uma versão de política, a política é adicionada no painel de detalhes do GPME quando você seleciona os **rede sem fio \(IEEE 802.11\) diretivas** nó. Esse estado permanece, a menos que você excluir a política sem fio, momento em que a versão de política sem fio retorna para a direita\-clique no menu de **rede sem fio \(IEEE 802.11\) políticas** no GPME . Além disso, as políticas sem fio só serão listadas no painel de detalhes do GPME quando o **rede sem fio \(IEEE 802.11\) diretivas** nó é selecionado.

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Para ativar o padrão de rede sem fio \(IEEE 802.11\) políticas  

1. Siga o procedimento anterior, **para abrir ou adicionar e abrir um objeto de diretiva de grupo** para abrir o GPME.

2. No GPME, no painel esquerdo, clique duas vezes\-clique em **configuração do computador**, double\-clique em **políticas**, double\-clique **configurações do Windows** e, em seguida, clique duas vezes\-clique em **configurações de segurança**.

![Política de grupo de sem fio 802.1 X](../../../media/Wireless-GP/Wireless-GP.jpg)

3. Na **as configurações de segurança**certo\-clique em **rede sem fio \(IEEE 802.11\) políticas**e, em seguida, clique em **criar uma nova política sem fio para Windows Vista e versões posteriores**. 

![802.1 x política sem fio](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. O **novas propriedades de diretiva de rede sem fio** caixa de diálogo é aberta. Na **nome da política**, digite um novo nome para a política ou mantenha o nome padrão. Clique em **Okey** para salvar a política. A política padrão é ativada e listada no painel de detalhes do GPME com o novo nome que você forneceu ou com o nome padrão **nova política de rede sem fio**.

![Novas propriedades de diretiva de rede sem fio](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. No painel de detalhes, clique duas vezes\-clique em **nova diretiva de rede sem fio** para abri-lo.

Na próxima seção, você pode executar a configuração de política, ordem de preferência de processamento de política e permissões de rede.

### <a name="bkmk_policyconfig"></a>Configurar a nova política de rede sem fio

Você pode usar os procedimentos nesta seção para configurar a rede sem fio \(IEEE 802.11\) política. Essa política permite que você definir as configurações de segurança e autenticação, gerenciar os perfis sem fio e especificar permissões para redes sem fio que não estão configurados como redes preferenciais.

- [Configurar um perfil de Conexão sem fio para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Definir a ordem de preferência para perfis de Conexão sem fio](#bkmk_preferenceorder)  

- [Definir permissões de rede](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurar um perfil de Conexão sem fio para PEAP\-MS\-CHAP v2

Esse procedimento fornece as etapas necessárias para configurar um PEAP\-MS\-perfil sem fio do protocolo CHAP v2.  

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Para configurar um perfil de conexão sem fio para PEAP\-MS\-CHAP v2

1. No GPME, na caixa de diálogo de propriedades de rede sem fio, para a política que você acabou de criar na **gerais** guia e, na **descrição**, digite uma breve descrição para a política.

2. Para especificar que a configuração automática de WLAN é usada para definir as configurações de adaptador de rede sem fio, certifique-se de que **serviço de uso configuração automática de WLAN do Windows para clientes** está selecionado.  

3. Na **conectar a redes disponíveis na ordem dos perfis listados abaixo**, clique em **Add**e, em seguida, selecione **infraestrutura**. O **propriedades do novo perfil** caixa de diálogo é aberta.

4. No**propriedades do novo perfil** caixa de diálogo a **Conexão** guia o **nome do perfil** , digite um novo nome para o perfil. Por exemplo, digite **exemplo.com WLAN perfil para o Windows 10**.

5. Na **nome de rede\(s\) \(SSID\)** , digite o SSID que corresponde ao SSID configurado em seus PAS sem fio e, em seguida, clique em **adicionar**.

    Se sua implantação utiliza vários SSIDs e cada PA sem fio utiliza as mesmas configurações de segurança sem fio, repita essa etapa para adicionar o SSID de cada PA sem fio ao qual você deseja que esse perfil seja aplicado.

    Se sua implantação utiliza vários SSIDs e as configurações de segurança para cada SSID não são correspondentes, configure um perfil separado para cada grupo de SSIDs que usam as mesmas configurações de segurança. Por exemplo, se você tiver um grupo de PAS sem fio configurados para usar WPA2\-Enterprise e AES e outro grupo de PAS sem fio para usar WPA\-Enterprise e TKIP, configure um perfil para cada grupo de PAS sem fio.

6. Se o texto padrão **NEWSSID** estiver presente, selecione-o e, em seguida, clique em **remover**.

7. Se você implantou pontos de acesso sem fio configurados para suprimir o sinalizador de difusão, selecione **Conectar mesmo que não haja difusão na rede**.

    > [!NOTE]
    > A habilitação dessa opção pode criar um risco à segurança porque os clientes sem fio investigarão e tentar conexões a qualquer rede sem fio. Por padrão, essa configuração não está habilitada.  

8. Clique na guia **Segurança** , clique em **Avançado**e configure:

    1. Para definir as configurações 802.1X avançadas, em **IEEE 802.1X**, selecione **Aplicar configurações 802.1X avançadas**.

        Quando 802.1X avançadas são aplicadas as configurações, os valores padrão para **máx Eapol\-iniciar enfileiradas**, **período de retenção**, **período inicial**, e  **Período de autenticação** são suficientes para implantações sem fio típicas. Por isso, você não precisa alterar os padrões, a menos que você tenha um motivo específico para fazer isso.

    2. Para habilitar o Logon Único, selecione **Habilitar Logon Único para esta rede**.

    3. Os valores padrão restantes em **Logon único** são suficientes para implantações sem fio típicas.

    4. Na **Roaming rápido**, se seu PA sem fio estiver configurada para pré-\-autenticação, selecione **esta rede usa pré\-autenticação**.

9. Para especificar que comunicações sem fio atendem aos FIPS 140\-2 padrões, selecionados **executar criptografia no FIPS 140\-modo de certificado 2**.

10. Clique em **OK** para retornar à guia **Segurança**. Na **selecione os métodos de segurança para essa rede**, na **autenticação**, selecione **WPA2\-Enterprise** se ele for compatível com o AP sem fio e sem fio adaptadores de rede do cliente. Caso contrário, selecione **WPA\-Enterprise**.

11. Na **criptografia**, se compatível com o AP sem fio e adaptadores de rede de cliente sem fio, selecionadas **CCMP do AES**. Se você estiver usando pontos de acesso e adaptadores de rede sem fio compatíveis com 802.11ac, selecione **AES GCMP**. Caso contrário, selecione **TKIP**.

    > [!NOTE]  
    > As configurações para ambos **autenticação** e **criptografia** deve coincidir com as configurações definidas em seus PAS sem fio. As configurações padrão para **modo de autenticação**, **máximo de falhas de autenticação**, e **armazenam em Cache informações de usuário para conexões futuras a esta rede** são é suficiente para implantações sem fio típicas.  

12. Na **Selecionar método de autenticação de rede**, selecione **EAP protegido \(PEAP\)** e, em seguida, clique em **propriedades**. O **Propriedades EAP protegidas** caixa de diálogo é aberta.

13. Na **Propriedades EAP protegidas**, confirme se **verificar a identidade do servidor Validando o certificado** está selecionado.

14. Na **autoridades de certificação raiz confiáveis**, selecione a autoridade de certificação raiz confiável \(autoridade de certificação\) que emitiu o certificado de servidor para o NPS.

    > [!NOTE]  
    > Essa configuração limita as ACs raiz que os clientes confiam para as ACs selecionadas. Se nenhuma AC raiz confiável forem selecionada, em seguida, os clientes confiarão que todas as ACs raiz listadas em seu repositório de certificados de autoridades de certificação raiz confiáveis.  

15. No **Selecionar método de autenticação** , selecione **senha segura \(EAP\-MS\-CHAP v2\)** .

16. Clique em **Configurar**. No **propriedades de EAP MSCHAPv2** caixa de diálogo, verifique se **usar automaticamente o meu nome de logon do Windows e senha \(e o domínio, se houver\)**  está selecionado e, em seguida, clique em  **Okey**.

17. Para habilitar a reconexão rápida do PEAP, certifique-se de que **Habilitar reconexão rápida** está selecionado.

18. Para exigir um servidor tiver TLV com ligação nas tentativas de conexão, selecione **desconectar se o servidor não tiver TLV com ligação**.

19. Para especificar que a identidade do usuário sejam mascarada na fase um da autenticação, selecione **habilitar privacidade de identidade**e, na caixa de texto, digite um nome de identidade anônima, ou deixe a caixa de texto em branco.

    > [!NOTES]
    > - A política do NPS para 802.1X sem fio deve ser criada usando o NPS **política de solicitação de Conexão**. Se a diretiva NPS é criada usando o NPS **política de rede**, privacidade de identidade não funcionará.
    > - Privacidade de identidade EAP é fornecida por determinados métodos EAP onde vazia ou uma identidade anônima \(diferente do que a identidade real\) é enviado em resposta à solicitação de identidade EAP. PEAP envia a identidade duas vezes durante a autenticação. Na primeira fase, a identidade é enviada em texto sem formatação e essa identidade é usada para fins de roteamento, não para autenticação de cliente. A identidade real — usado para autenticação — é enviada durante a segunda fase de autenticação, dentro do túnel seguro que é estabelecida na primeira fase. Se **habilitar privacidade de identidade** caixa de seleção estiver marcada, o nome de usuário é substituída com a entrada especificada na caixa de texto. Por exemplo, suponha **habilitar privacidade de identidade** está selecionado e o alias de privacidade de identidade **anônimo** é especificado na caixa de texto. Para um usuário com um alias de identidade real <strong>jdoe@example.com</strong>, a identidade enviada na primeira fase de autenticação será alterada para <strong>anonymous@example.com</strong>. A parte da realm da identidade 1º fase não será modificada como ele é usado para fins de roteamento.  

20. Clique em **Okey** para fechar o **Propriedades EAP protegidas** caixa de diálogo.
21. Clique em **Okey** para fechar o **segurança** guia.
22. Se você quiser criar perfis adicionais, clique em **adicionar**e, em seguida, repita as etapas anteriores, tornando as diferentes opções para personalizar cada perfil para os clientes sem fio e a rede na qual você deseja que o perfil aplicado. Quando você terminar a adição de perfis, clique em **Okey** para fechar a caixa de diálogo Propriedades de diretiva de rede sem fio.

Na próxima seção, você pode ordenar os perfis de política de segurança ideal.

#### <a name="bkmk_preferenceorder"></a>Definir a ordem de preferência para perfis de Conexão sem fio
Você pode usar este procedimento se você criou vários perfis sem fio em sua política de rede sem fio e você quiser ordenar os perfis de segurança e eficiência ideal.

Para garantir que os clientes sem fio se conectar com o nível mais alto de segurança que podem dar suporte, coloque suas políticas mais restritivas na parte superior da lista.

Por exemplo, se você tiver dois perfis, um para os clientes que dão suporte a WPA2 e outro para clientes que dão suporte ao WPA, colocar o perfil do WPA2 mais alto na lista. Isso garante que os clientes que dão suporte a WPA2 usará esse método para a conexão em vez do WPA menos seguro.

Esse procedimento fornece as etapas para especificar a ordem em que os perfis de conexão sem fio são usados para conectar clientes sem fio de membros do domínio a redes sem fio.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Para definir a ordem de preferência para perfis de conexão sem fio

1. No GPME, na caixa de diálogo de propriedades de rede sem fio, para a política que você acabou de configurar, clique no **geral** guia.

2. Sobre o **gerais** guia de **conectar a redes disponíveis na ordem dos perfis listados abaixo**, selecione o perfil que você deseja mover na lista e, em seguida, clique em "seta para cima" botão "seta para baixo ou" botão para mover o perfil para o local desejado na lista.

3.  Repita a etapa 2 para cada perfil que você deseja mover na lista.  

4.  Clique em **Okey** para salvar as alterações.

Na seção a seguir, você pode definir permissões de rede para a política sem fio.

#### <a name="bkmk_permissions"></a>Definir permissões de rede
Você pode definir as configurações na **permissões de rede** guia para os membros do domínio ao qual rede sem fio \(IEEE 802.11\) políticas se aplicam.

Só é possível aplicar as seguintes configurações para redes sem fio que não estão configurados na **gerais** guia o **propriedades de diretiva de rede sem fio** página:

- Permitir ou negar conexões a redes sem fio específicas que você especifica por tipo de rede e o identificador do conjunto de serviço \(SSID\)

- Permitir ou negar conexões a redes ad-hoc

- Permitir ou negar conexões a redes de infra-estrutura

- Permitir ou negar que os usuários exibam os tipos de rede \(ad hoc ou infra-estrutura\) aos quais eles têm acesso negados

- Permitir ou negar que os usuários criem um perfil que se aplica a todos os usuários

- Os usuários só podem se conectar a redes permitidas usando perfis de diretiva de grupo

Associação na **Admins. do domínio**, ou equivalente, é o mínimo necessário para concluir esses procedimentos.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Para permitir ou negar conexões a redes sem fio específicas

1. No GPME, na caixa de diálogo Propriedades de rede sem fio, clique no **permissões de rede** guia.

2. Sobre o **permissões de rede** , clique em **Add**. O **entrada de novas permissões** caixa de diálogo é aberta.

3. No **nova entrada de permissão** na caixa a **nome de rede \(SSID\)**  campo, digite o SSID da rede de rede para o qual você deseja definir permissões.

4.  Na **tipo de rede**, selecione **infraestrutura** ou **Ad hoc**.

    > [!NOTE]  
    > Se você não tiver certeza se a rede de difusão é uma infraestrutura ou a rede ad hoc, você pode configurar duas entradas de permissão de rede, um para cada tipo de rede.

5. Na **permissão**, selecione **permitir** ou **Deny**.

6. Clique em **Okey**, para retornar para o **permissões de rede** guia.

##### <a name="to-specify-additional-network-permissions-optional"></a>Para especificar permissões de rede adicional \(opcional\)

1.  Sobre o **permissões de rede** guia, configure qualquer ou todos os seguintes:  

    -   Para negar acesso a redes ad hoc de seus membros do domínio, selecione **impedir conexões a anúncio\-redes hoc**.

    -   Para negar acesso a redes de infra-estrutura de seus membros do domínio, selecione **impedir conexões a redes de infraestrutura**.  

    -   Para permitir que os membros do domínio exibir os tipos de rede \(ad hoc ou infra-estrutura\) aos quais eles têm acesso negados, selecione **permitir que o usuário exiba redes negadas**.

    -   Para permitir que os usuários criem perfis que se aplicam a todos os usuários, selecione **permitir que todos criem todos os perfis de usuário**.

    -   Para especificar que os usuários só podem se conectar a redes permitidas usando perfis de diretiva de grupo, selecione **usar somente perfis de diretiva de grupo para redes permitidas**.

## <a name="bkmk_nps"></a>Configurar seu NPSs
Siga estas etapas para configurar NPSs para executar a autenticação 802.1 X para acesso sem fio:

- [Registrar o NPS nos serviços de domínio do Active Directory](#bkmk_npsreg)

- [Configurar um AP sem fio como um cliente RADIUS de NPS](#bkmk_radiusclient)

- [Criar políticas de NPS para 802.1X sem fio usando um assistente](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrar o NPS nos serviços de domínio do Active Directory
Você pode usar este procedimento para registrar um servidor que executa o servidor de políticas de rede \(NPS\) nos serviços de domínio do Active Directory \(AD DS\) no domínio em que o NPS é um membro. Para NPSs receber permissão para ler a discagem\-nas propriedades de contas de usuário durante o processo de autorização, cada NPS deve ser registrado no AD DS. Registrar um NPS adiciona o servidor para o **servidores RAS e IAS** grupo de segurança no AD DS.

>[!NOTE]
>Você pode instalar o NPS em um controlador de domínio ou em um servidor dedicado. Execute o seguinte comando do Windows PowerShell para instalar o NPS se ainda não tiver feito isso:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

#### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar um NPS em seu domínio padrão

1. Sobre o NPS, no **Gerenciador de servidores**, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Ajustar-se o NPS\-no é aberta.

2. À direita\-clique em **NPS \(Local\)** e, em seguida, clique em **registrar servidor no Active Directory**. A caixa de diálogo **Servidor de Políticas de Rede** é aberta.

3. Em **Servidor de Políticas de Rede**, clique em **OK** e em **OK** novamente.

### <a name="bkmk_radiusclient"></a>Configurar um AP sem fio como um cliente RADIUS de NPS
Você pode usar este procedimento para configurar um ponto de acesso, também conhecido como um *servidor de acesso de rede \(NAS\)* , como um Remote Authentication Dial\-In User Service \(RADIUS\) o cliente usando o snap NPS\-no. 

>[!IMPORTANT]
>Computadores cliente, como computadores portáteis sem fio e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede — como pontos de acesso sem fio 802.1 X\-comutadores com capacidade, a rede virtual privada \(VPN\) servidores e dial\-backup de servidores — porque eles usam o protocolo RADIUS para se comunicar com servidores RADIUS, como NPSs.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para adicionar um servidor de acesso de rede como um cliente RADIUS no NPS

1. Sobre o NPS, no **Gerenciador de servidores**, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Ajustar-se o NPS\-no é aberta.

2. No NPS snap\-, clique duas vezes\-clique em **clientes e servidores RADIUS**. À direita\-clique em **clientes RADIUS**e, em seguida, clique em **New**.

3. No **novo cliente RADIUS**, verifique se que o **habilitar este cliente RADIUS** caixa de seleção está selecionada.

4. Na **novo cliente RADIUS**, na **nome amigável**, digite um nome de exibição para o ponto de acesso sem fio.

    Por exemplo, se você quiser adicionar um ponto de acesso sem fio \(AP\) denominado AP\-01, digite **AP\-01**.

5. Na **endereço \(IP ou DNS\)** , digite o nome de domínio totalmente qualificado ou endereço IP \(FQDN\) para NAS.

    Se você inserir o FQDN, para verificar se o nome está correto e é mapeado para um endereço IP válido, clique em **Verify**e, em seguida, na **Verifique se o endereço**, no **endereço** , clique em  **Resolver**. Se o nome FQDN é mapeado para um endereço IP válido, o endereço IP do que NAS aparecerão automaticamente na **endereço IP**. Se o FQDN não resolver para um endereço IP, que você receberá uma mensagem indicando que nenhum host desse tipo é conhecido. Se isso ocorrer, verifique se que você tenha o nome correto do ponto de acesso e que o ponto de acesso está ligado e conectado à rede.  

    Clique em **Okey** para fechar **Verifique se o endereço**.  

6. Na **novo cliente RADIUS**, na **segredo compartilhado**, faça o seguinte:  

    -   Para configurar manualmente um segredo compartilhado RADIUS, selecione **Manual**e, em seguida, na **segredo compartilhado**, digite a senha forte que também é inserida NAS. Digite novamente o segredo compartilhado no **Confirmar segredo compartilhado**.  

    -   Para gerar automaticamente um segredo compartilhado, selecione a **Generate** caixa de seleção e, em seguida, clique no **gerar** botão. Salve o segredo compartilhado gerado e, em seguida, use esse valor para configurar o para que ele possa se comunicar com o NPS.  

        >[!IMPORTANT]
        >O segredo compartilhado RADIUS que você insira seu AP virtual no NPS deve coincidir exatamente com o segredo compartilhado RADIUS configurado no seu real PA sem fio Se você usar a opção de NPS para gerar um segredo compartilhado RADIUS, você deve configurar o AP sem fio real de correspondência com o segredo compartilhado RADIUS que foi gerado pelo NPS.

7. No **novo cliente RADIUS**diante o **avançado** guia **nome do fornecedor**, especifique o nome do fabricante NAS. Se você não tiver certeza do nome do fabricante NAS, selecione **padrão RADIUS**.

8. Na **opções adicionais**, se você estiver usando qualquer método de autenticação que não seja EAP e PEAP e se o seu NAS oferece suporte ao uso do atributo de autenticador de mensagem, selecione **mensagens de solicitação de acesso devem conter o Mensagem\-atributo autenticador**.

9. Clique em **OK**. Seu NAS é exibida na lista de clientes RADIUS configurado no NPS.

### <a name="bkmk_npspolicy"></a>Criar políticas de NPS para 802.1X sem fio usando um assistente
Você pode usar este procedimento para criar a conexão de diretivas de solicitação e necessárias para implantar qualquer 802.1 X de políticas de rede\-pontos de acesso sem fio compatíveis com como Remote Authentication Dial\-In User Service \(RADIUS\) clientes para o servidor RADIUS executando o servidor de políticas de rede \(NPS\).  
Depois de executar o assistente, as políticas a seguir são criadas:

- Diretiva de solicitação de uma conexão

- Uma diretiva de rede

>[!NOTE]
>Você pode executar o Assistente de novo IEEE 802.1 X com fio e seguras conexões sem fio toda vez que você precisa criar novas políticas de acesso autenticado 802.1 X.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Crie políticas para 802.1 X sem fio autenticado usando um assistente

1. Ajustar-se abrir o NPS\-no. Se ainda não estiver selecionado, clique em **NPS \(Local\)** . Se você estiver executando snap do MMC do NPS\-no e para criar políticas em um NPS remoto, selecione o servidor.

2. Na **guia de Introdução**, na **configuração padrão**, selecione **servidor RADIUS para conexões com fio ou de 802.1X sem fio**. O texto e os links abaixo para alterar o texto para refletir sua seleção.

3. Clique em **configurar 802.1 X**. Abre o Assistente Configurar 802.1 X.

4.  No **Selecionar tipo de conexões do 802.1 X** página de assistente, na **tipo de conexões do 802.1 X**, selecione **conexões seguras de sem fio**e, em **nome**, digite um nome para a política ou deixe o nome padrão **conexões de sem fio seguras**. Clique em **Avançar**.

5.  Sobre o **especificar opções de 802.1 X** página de assistente, na **clientes RADIUS**, todos os 802.1 X comutadores e pontos de acesso sem fio que você adicionou como clientes RADIUS no snap do NPS\-são mostrados. Siga qualquer um destes procedimentos:

    -   Para adicionar servidores de acesso de rede adicional \(NASs\), como o APs sem fio, na **clientes RADIUS**, clique em **Add**e, em seguida, no **novo cliente RADIUS**, insira as informações para: **Nome amigável**, **endereço \(IP ou DNS\)** , e **segredo compartilhado**.

    -   Para modificar as configurações para qualquer NAS, no **clientes RADIUS**, selecione o ponto de acesso para o qual você deseja modificar as configurações e, em seguida, clique em **editar**. Modifique as configurações conforme necessário.

    -   Para remover um na lista, na **clientes RADIUS**, selecione NAS e, em seguida, clique em **remover**.

        >[!WARNING]
        >Remoção de um cliente RADIUS de dentro de **configurar 802.1 X** assistente excluirá o cliente da configuração do NPS. Todas as adições, modificações e exclusões feitas dentro de **configurar 802.1 X** Assistente para os clientes RADIUS são refletidas no NPS snap\-no, no **clientes RADIUS** nó no  **NPS** \/ **clientes e servidores RADIUS**. Por exemplo, se você usar o Assistente para remover um comutador 802.1X, a opção também é removida do NPS snap\-no.

6. Clique em **Avançar**. Sobre o **configurar um método de autenticação** página de assistente, na **tipo \(com base no método de acesso e configuração de rede\)** , selecione **Microsoft: EAP protegido \(PEAP\)** e, em seguida, clique em **configurar**.

    >[!TIP]
    >Se você receber uma mensagem de erro indicando que não é possível encontrar um certificado para uso com o método de autenticação, e você tiver configurado o Active Directory Certificate Services para emitir automaticamente certificados para servidores RAS e IAS em sua rede pela primeira vez Certifique-se de que você tiver seguido as etapas para registrar o NPS nos serviços de domínio do Active Directory, use as seguintes etapas para atualizar a política de grupo: Clique em **inicie**, clique em **sistema do Windows**, clique em **executar**e, na **abrir**, tipo **gpupdate**e, em seguida, Pressione ENTER. Quando o comando retorna resultados que indica que o usuário e a política de grupo do computador foi atualizada com êxito, selecione **Microsoft: EAP protegido \(PEAP\)**  novamente e, em seguida, clique em **configurar**.
    >
    >Se depois de atualizar a política de grupo que você continue a receber a mensagem de erro indicando que não é possível encontrar um certificado para uso com o método de autenticação, o certificado não estiver sendo exibido porque ele não atende o certificado de servidor mínima requisitos conforme documentado no guia complementar da rede principal: [Implantar certificados de servidor para 802.1 X com fio e sem fio implantações](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Se isso acontecer, você deve interromper a configuração do NPS, revogar o certificado emitido para o NPS\(s\)e, em seguida, siga as instruções para configurar um novo certificado usando o guia de implantação de certificados do servidor.

7.  Sobre o **editar propriedades de EAP protegidas** página de assistente, na **certificado emitido**, certifique-se de que o certificado NPS correto está selecionado e, em seguida, faça o seguinte:

    >[!NOTE]
    >Verificar se o valor em **emissor** está correto para o certificado selecionado na **certificado emitido**. Por exemplo, o emissor esperado para um certificado emitido por uma autoridade de certificação executando serviços de certificados do Active Directory \(AD CS\) nomeada corp\DC1 no domínio contoso.com, é **corp\-DC1\-deautoridadedecertificação**.

    -   Para permitir que os usuários façam roaming com seus computadores sem fio entre pontos de acesso sem necessidade de se autenticar novamente cada vez que se associam a um novo ponto de acesso, selecione **Habilitar reconexão rápida**.

    -   Para especificar que conectam os clientes sem fio terminará o processo de autenticação de rede se o servidor RADIUS não tiver TLV tipo\-comprimento\-valor \(TLV\), selecione **desconectar Os clientes sem ligação de criptografia**.  

    -   Para modificar a política de configurações para o EAP digite, em **tipos de EAP**, clique em **editar**, na **propriedades de EAP MSCHAPv2**, modifique as configurações conforme necessário e, em seguida, clique em **Okey**.  

8.  Clique em **OK**. Fecha a caixa de diálogo Editar propriedades de EAP protegidas, e você volta para o **configurar 802.1 X** assistente. Clique em **Avançar**.

9. Na **especificar grupos de usuários**, clique em **Add**e, em seguida, digite o nome do grupo de segurança que você configurou para seus clientes sem fio em que os usuários do Active Directory e computadores encaixar\-no. Por exemplo, se você nomeou seu grupo de segurança sem fio grupo sem fio, digite **grupo sem fio**. Clique em **Avançar**.

10. Clique em **configurar** para configurar atributos RADIUS padrão e fornecedor\-os atributos específicos de LAN virtual \(VLAN\) conforme necessário e, como especificado na documentação fornecida pelo seu fornecedor de hardware de PA sem fio. Clique em **Avançar**.

11. Examine os detalhes de resumo de configuração e, em seguida, clique em **concluir**.

As políticas NPS agora são criadas, e você pode passar para ingressar computadores sem fio no domínio.

## <a name="bkmk_domain"></a>Novos computadores sem fio ingressar no domínio
O método mais fácil para novos computadores sem fio ingressar no domínio é conectar fisicamente o computador em um segmento de LAN com fio \(um segmento não controlado por um comutador 802.1X\) antes de ingressar o computador no domínio. Isso é mais fácil porque as configurações de diretiva de grupo sem fio são automaticamente e imediatamente aplicadas e, se você tiver implantado o seu próprio PKI, o computador recebe o certificado de autoridade de certificação e o coloca no repositório de certificados de autoridades de certificação raiz confiáveis permitindo que o cliente sem fio NPSs com certificados de servidor emitidos por sua autoridade de certificação de confiança.

Da mesma forma, depois que um novo computador sem fio é ingressado no domínio, o método preferencial para os usuários façam logon no domínio é executar o log em usando uma conexão com fio na rede.

### <a name="other-domain-join-methods"></a>Outros métodos de ingresso no domínio
Em casos onde não é prático ingressar computadores ao domínio usando uma conexão Ethernet com fio ou em casos em que o usuário não possa fazer logon no domínio pela primeira vez usando uma conexão com fio, você deve usar um método alternativo.

- **Configuração de computador de equipe de TI**. Um membro da equipe de TI ingressa um computador sem fio no domínio e configura um Single Sign On bootstrap perfil sem fio. Com esse método, o administrador de TI conecta o computador sem fio à rede Ethernet com fio e ingressa o computador ao domínio. Em seguida, o administrador distribui o computador para o usuário. Quando o usuário seja iniciado o computador sem usar uma conexão com fio, as credenciais de domínio especificadas manualmente para o logon do usuário são usadas para estabelece uma conexão para a rede sem fio e faça logon no domínio.

Para obter mais informações, consulte a seção [ingressar no domínio e o logon usando o método de configuração do computador de equipe de TI](#bkmk_itstaff)

-   **Inicializar a configuração do perfil sem fio por usuários**. Manualmente, o usuário configura o computador sem fio com um perfil sem fio de bootstrap e ingressa no domínio, com base nas instruções adquiridas de um administrador de TI. O perfil sem fio de bootstrap permite ao usuário estabelecer uma conexão sem fio e ingresse no domínio. Depois de ingressar o computador no domínio e reiniciar o computador, o usuário pode faça logon no domínio usando uma conexão sem fio e suas credenciais de conta de domínio.

Para obter mais informações, consulte a seção [ingressar no domínio e o logon usando a configuração de perfil de sem fio de Bootstrap por usuários](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Ingressar no domínio e o logon usando o método de configuração do computador de equipe de TI
Usuários de membro de domínio no domínio\-computadores cliente sem fio associados podem usar um perfil sem fio temporário para se conectar a um 802.1 X\-autenticado rede sem fio sem primeiro conectar-se à LAN com fio. Esse perfil sem fio temporário é chamado um *perfil sem fio de bootstrap*.

Um perfil sem fio de bootstrap, requer que o usuário especifique manualmente suas credenciais de conta de usuário de domínio e não valida o certificado do Remote Authentication Dial\-In User Service \(RADIUS\) server executando o servidor de políticas de rede \(NPS\).

Após o estabelecimento de conectividade sem fio, diretiva de grupo é aplicada no computador cliente sem fio e um novo perfil sem fio é emitido automaticamente. A nova política usa as credenciais de conta de computador e usuário para autenticação de cliente. 

Além disso, como parte do PEAP\-MS\-CHAP v2 a autenticação mútua usando o novo perfil, em vez do perfil de bootstrap, o cliente valida as credenciais do servidor RADIUS.

Depois de ingressar o computador ao domínio, use este procedimento para configurar um logon único sem fio perfil bootstrap, antes de distribuir o computador sem fio no domínio\-usuário membro.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Para configurar um logon único bootstrap perfil sem fio

1. Criar um perfil de bootstrap usando o procedimento neste guia denominada [configurar um perfil de Conexão sem fio para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)e use as seguintes configurações:

    - PEAP\-MS\-autenticação CHAP v2

    - Validar desabilitado de certificado do servidor RADIUS

    - O logon único habilitado

2. Nas propriedades da diretiva de rede sem fio dentro do qual você criou o novo perfil de inicialização na **gerais** guia, selecione o perfil de inicialização e, em seguida, clique em **exportar** para exportar o perfil para um compartilhamento de rede, unidade flash USB ou outro local de fácil acesso. O perfil é salvo como um arquivo *. XML para o local que você especificar.

3. Ingressar o novo computador sem fio no domínio \(por exemplo, por meio de uma conexão Ethernet que não exija IEEE 802.1 X autenticação\) e adicione o perfil sem fio de bootstrap ao computador usando o **netsh wlan Adicionar perfil** comando.

    >[!NOTE]
    >Para obter mais informações, consulte os comandos Netsh Wireless Local Area Network \(WLAN\) na [http:\/\/technet.microsoft.com\/biblioteca\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuir o novo computador sem fio para o usuário com o procedimento para "Fazer logon no domínio usando computadores que executam o Windows 10."

Quando o usuário inicia o computador, o Windows solicita que o usuário insira seu nome de conta de usuário de domínio e senha. Como o logon único está habilitado, o computador usa as credenciais de conta de usuário de domínio para primeiro estabelecer uma conexão com a rede sem fio e, em seguida, faça logon no domínio.

#### <a name="bkmk_w10"></a>Faça logon no domínio usando computadores que executam o Windows 10

1. Faça logoff do computador ou o reinicie.

2. Pressione qualquer tecla no teclado ou clique na área de trabalho. Aparece a tela de logon com um nome de conta de usuário local exibido e um campo de entrada de senha abaixo do nome. Não faça logon com a conta de usuário local.

3. No canto inferior esquerdo da tela, clique em **outro usuário**. A tela de logon do outro usuário aparece com dois campos, um nome de usuário e uma senha. Abaixo a senha do campo é o texto **faça logon em:** e, em seguida, o nome do domínio em que o computador está associado. Por exemplo, se seu domínio for denominado exemplo.com, o texto é lido **faça logon em: EXEMPLO**.

4. Na **nome de usuário**, digite seu nome de usuário de domínio.

5. Em **Senha**, digite sua senha do domínio e clique na seta ou pressione ENTER.

>[!NOTE]
>Se o **outro usuário** tela não inclui o texto **faça logon em:** e o nome de domínio, você deve inserir seu nome de usuário no formato *domínio\\usuário*. Por exemplo, para fazer logon no domínio example.com usando uma conta denominada **usuário\-01**, digite **exemplo\\usuário\-01**.

### <a name="bkmk_userbootstrap"></a>Ingressar no domínio e o logon usando a configuração de perfil de sem fio de Bootstrap pelos usuários
Com esse método, você concluir as etapas na seção etapas gerais e, em seguida, forneça o seu domínio\-usuários membros com as instruções sobre como configurar manualmente um computador sem fio com um perfil sem fio de bootstrap. O perfil sem fio de bootstrap permite ao usuário estabelecer uma conexão sem fio e ingresse no domínio. Depois que o computador está ingressado no domínio e reiniciado, o usuário pode fazer logon no domínio por meio de uma conexão sem fio.

#### <a name="general-steps"></a>Etapas gerais

1. Configurar uma conta de administrador do computador local, no **painel de controle**, para o usuário.

    >[!IMPORTANT]
    >Para ingressar um computador em um domínio, o usuário deve fazer logon no computador com a conta de administrador local. Como alternativa, o usuário deve fornecer as credenciais da conta de administrador local durante o processo de ingressar o computador ao domínio. Além disso, o usuário deve ter uma conta de usuário no domínio ao qual o usuário deseja ingressar o computador. Durante o processo de ingressar o computador ao domínio, o usuário será solicitado para credenciais de conta de domínio \(nome de usuário e senha\).

2. Fornecer aos usuários de domínio com as instruções para configurar um perfil sem fio de bootstrap, conforme documentado no procedimento a seguir **para configurar um perfil sem fio de bootstrap**.
3. Além disso, fornecer aos usuários as credenciais do computador local \(nome de usuário e senha\)e as credenciais de domínio \(domínio nome de conta de usuário e senha\) no formulário *DomainName \\Nome de usuário*, bem como os procedimentos para "Ingressar o computador no domínio" e "Fazer logon no domínio," como o Windows Server 2016 está documentado [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Para configurar um perfil sem fio de bootstrap

1. Usar as credenciais fornecidas pelo seu administrador de rede ou o suporte de TI profissionais para fazer logon no computador com a conta de administrador do computador local.

2. À direita\-clique no ícone de rede na área de trabalho e, em seguida, clique em **abrir Central de rede e compartilhamento**. A **Central de Rede e Compartilhamento** será aberta. Na **alterar suas configurações de rede**, clique em **configurar uma nova conexão ou rede**. O **definir uma Conexão ou rede** caixa de diálogo é aberta.

3. Clique em **se conectar manualmente a uma rede sem fio**e, em seguida, clique em **próxima**.

4. Na **se conectar manualmente a uma rede sem fio**, na **nome da rede**, digite o nome SSID do AP.  

5. Na **tipo de segurança**, selecione a configuração fornecida pelo seu administrador.

6. Na **tipo de criptografia** e **chave de segurança**, selecione ou digite as configurações fornecidas pelo seu administrador.

7. Selecione **iniciar essa conexão automaticamente**e, em seguida, clique em **próxima**.

8. Em **adicionado com êxito * * * seu SSID de rede*, clique em **alterar as configurações de conexão**.

9. Clique em **alterar configurações de conexão**. O *seu SSID de rede* caixa de diálogo de propriedades de rede sem fio é aberta.

10. Clique o **segurança** guia e, em seguida, na **escolher um método de autenticação de rede**, selecione **EAP protegido \(PEAP\)** .

11. Clique em **Configurações**. O **EAP protegidas \(PEAP\) propriedades** página será aberta.

12. No **EAP protegido \(PEAP\) propriedades** página, verifique se **Validar certificado do servidor** não é selecionado, clique em **Okey** duas vezes, e em seguida, clique em **fechar**.

13. Windows, em seguida, tenta se conectar à rede sem fio. As configurações do perfil sem fio de bootstrap especificam que você deve fornecer suas credenciais de domínio. Quando o Windows solicitar um nome de conta e senha, digite suas credenciais de conta de domínio da seguinte maneira:  *Nome de domínio\\nome de usuário*, *senha domínio*.

##### <a name="to-join-a-computer-to-the-domain"></a>Para ingressar um computador ao domínio

1. Faça logon no computador com a conta de Administrador local.

2. Na caixa de texto de pesquisa, digite **PowerShell**. Nos resultados da pesquisa, clique com botão direito **Windows PowerShell**e, em seguida, clique em **executar como administrador**. Windows PowerShell é aberta com um prompt com privilégios elevados.

3. No Windows PowerShell, digite o seguinte comando e pressione ENTER. Certifique-se de que você substitui o DomainName variável com o nome do domínio que você deseja ingressar.
    
    Add-Computer DomainName
    
4. Quando solicitado, digite seu nome de usuário de domínio e a senha e clique em **Okey**.
5. Reinicie o computador.
6. Siga as instruções na seção anterior [faça logon no domínio usando computadores que executam o Windows 10](#bkmk_w10).