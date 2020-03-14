---
title: Implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "implantar o acesso sem fio autenticado 802.1 X com base em senha"
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f07520dcdefa04cb43760c5e5c66e28c0d1ce878
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322108"
---
# <a name="wireless-access-deployment"></a>Implantação de acesso sem fio

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Siga estas etapas para implantar o acesso sem fio:

- [Implantar e configurar APs sem fio](#bkmk_aps)

- [Criar um grupo de segurança de usuários sem fio](#bkmk_groups)

- [Configurar a rede sem fio \(as políticas de\) do IEEE 802,11](#bkmk_policies)

- [Configurar o NPSs](#bkmk_nps)

- [Unir novos computadores sem fio ao domínio](#bkmk_domain)

## <a name="bkmk_aps"></a>Implantar e configurar APs sem fio

Siga estas etapas para implantar e configurar seus APs sem fio:

- [Especificar frequências de canal AP sem fio](#bkmk_channel)

- [Configurar APs sem fio](#bkmk_wirelessaps)

>[!NOTE]
>Os procedimentos deste guia não incluem instruções para os casos em que a caixa de diálogo **Controle de Conta de Usuário** é aberta para solicitar sua permissão para continuar. Caso essa caixa de diálogo seja aberta durante a execução dos procedimentos deste guia e em resposta às suas ações, clique em **Continuar**.

### <a name="bkmk_channel"></a>Especificar frequências de canal AP sem fio

Ao implantar vários APs sem fio em um único site geográfico, você deve configurar APs sem fio que têm sinais sobrepostos para usar frequências de canal exclusivas para reduzir a interferência entre APs sem fio.

Você pode usar as diretrizes a seguir para ajudá-lo a escolher as frequências de canal que não entram em conflito com outras redes sem fio na localização geográfica da sua rede sem fio.

- Se houver outras organizações que têm escritórios em proximidade ou na mesma compilação de sua organização, identifique se há alguma rede sem fio pertencente a essas organizações. Descubra as frequências de posicionamento e de canal atribuídas de seus pontos de acesso sem fio, pois você precisa atribuir frequências de canal diferentes ao seu AP e você precisa determinar o melhor local para instalar o AP.

- Identifique sinais sem fio sobrepostos em andares adjacentes em sua própria organização. Depois de identificar áreas de cobertura sobrepostas fora e dentro de sua organização, atribua frequências de canal para seus APs sem fio, garantindo que quaisquer dois APs sem fio com cobertura sobreposta sejam atribuídos a frequências de canal diferentes.

### <a name="bkmk_wirelessaps"></a>Configurar APs sem fio

Use as informações a seguir junto com a documentação do produto fornecida pelo fabricante do AP sem fio para configurar seus APs sem fio.

Este procedimento enumera os itens normalmente configurados em um AP sem fio. Os nomes de item podem variar de acordo com a marca e o modelo e podem ser diferentes daqueles na lista a seguir. Para obter detalhes específicos, consulte a documentação do AP sem fio.

#### <a name="to-configure-your-wireless-aps"></a>Para configurar seus APs sem fio  

- **SSID**. Especifique o nome da rede sem fio\(s\) \(por exemplo, ExampleWLAN\). Esse é o nome anunciado para clientes sem fio.

- **Criptografia**. Especifique o WPA2\-Enterprise \(preferencial\) ou WPA\-Enterprise e a codificação de criptografia TKIP \(ou\) preferencial, dependendo de quais versões são suportadas por seus adaptadores de rede de computador cliente sem fio.

- O **endereço IP do AP sem fio \(\)estático** . Em cada ponto de acesso, configure um endereço IP estático exclusivo que esteja dentro do intervalo de exclusão do escopo do DHCP para a sub-rede. O uso de um endereço excluído da atribuição por DHCP impede que o servidor DHCP atribua o mesmo endereço IP a um computador ou outro dispositivo.

- **Máscara de sub-rede**. Configure isso para corresponder às configurações de máscara de sub-rede da LAN à qual você conectou o AP sem fio.  

- **Nome DNS**. Alguns APs sem fio podem ser configurados com um nome DNS. O serviço DNS na rede pode resolver nomes DNS para um endereço IP. Em cada AP sem fio que dê suporte a esse recurso, insira um nome exclusivo para a resolução DNS.  

- **Serviço DHCP**. Se o AP sem fio tiver um\-criado no serviço DHCP, desabilite-o.  

- **Segredo compartilhado RADIUS**. Use um segredo compartilhado RADIUS exclusivo para cada AP sem fio, a menos que você esteja planejando configurar APs como clientes RADIUS no NPS por grupo. Se você planeja configurar o APs por grupo no NPS, o segredo compartilhado deve ser o mesmo para todos os membros do grupo. Além disso, cada segredo compartilhado que você usa deve ser uma sequência aleatória de pelo menos 22 caracteres que combina letras maiúsculas e minúsculas, números e pontuação. Para garantir a aleatoriedade, você pode usar um gerador de caracteres aleatório, como o gerador de caracteres aleatórios encontrado no Assistente para **Configurar o 802.1 x** do NPS, para criar os segredos compartilhados.

>[!TIP]
>Registre o segredo compartilhado para cada AP sem fio e armazene-o em um local seguro, como um escritório seguro. Você deve saber o segredo compartilhado para cada AP sem fio ao configurar clientes RADIUS no NPS.  

- **Endereço IP do servidor RADIUS**. Digite o endereço IP do servidor que executa o NPS.

- A **porta UDP\(s\)** . Por padrão, o NPS usa as portas UDP 1812 e 1645 para mensagens de autenticação e portas UDP 1813 e 1646 para mensagens de contabilidade. É recomendável que você use essas mesmas portas UDP em seus APs, mas se você tiver um motivo válido para usar portas diferentes, certifique-se de não apenas configurar os APs com os novos números de porta, mas também reconfigurar todos os seus NPSs para usar os mesmos números de porta que os APs. Se os APs e o NPSs não estiverem configurados com as mesmas portas UDP, o NPS não poderá receber ou processar solicitações de conexão do APs, e todas as tentativas de conexão sem fio na rede falharão.

- **VSAs**. Alguns APs sem fio exigem atributos específicos do fornecedor\-\(VSAs\) para fornecer funcionalidade completa de AP sem fio. Os VSAs são adicionados à política de rede do NPS.

- **Filtragem de DHCP**. Configure APs sem fio para impedir que clientes sem fio enviem pacotes IP da porta UDP 68 para a rede, conforme documentado pelo fabricante do AP sem fio.

- **Filtragem de DNS**. Configure os APs sem fio para impedir que clientes sem fio enviem pacotes IP da porta TCP ou UDP 53 para a rede, conforme documentado pelo fabricante do AP sem fio.

## <a name="create-security-groups-for-wireless-users"></a>Criar grupos de segurança para usuários sem fio

Siga estas etapas para criar um ou mais grupos de segurança de usuários sem fio e, em seguida, adicione usuários ao grupo de segurança usuários sem fio apropriados:

- [Criar um grupo de segurança de usuários sem fio](#bkmk_groups)

- [Adicionar usuários ao grupo de segurança sem fio](#bkmk_addusers)

### <a name="bkmk_groups"></a>Criar um grupo de segurança de usuários sem fio

Você pode usar este procedimento para criar um grupo de segurança sem fio no console de gerenciamento do Active Directory usuários e computadores \(MMC\) snap\-in.  

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-create-a-wireless-users-security-group"></a>Para criar um grupo de segurança de usuários sem fio

1. Clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. O\-do snap Active Directory usuários e computadores no é aberto. Caso ainda não esteja selecionado, clique no nó do seu domínio. Por exemplo, se o seu domínio for exemplo.com, clique em **exemplo.com**.

2. No painel de detalhes, clique com o botão direito do\-mouse na pasta na qual você deseja adicionar um novo grupo \(, por exemplo,\-clique em **usuários**\), aponte para **novo**e clique em **grupo**.

3. Em **Novo Objeto – Grupo**, em **Nome do grupo**, digite o nome do novo grupo. Por exemplo, digite **grupo sem fio**.

4. Em **Escopo do grupo**, selecione uma das seguintes opções:

    - **Domínio local**

    - **Geral**

    - **Universal**

5. Em **Tipo de grupo**, selecione **Segurança**.

6. Clique em **OK**.

Se você precisar de mais de um grupo de segurança para usuários sem fio, repita essas etapas para criar grupos de usuários sem fio adicionais. Posteriormente, você pode criar políticas de rede individuais no NPS para aplicar diferentes condições e restrições a cada grupo, fornecendo a eles permissões de acesso e regras de conectividade diferentes.

### <a name="bkmk_addusers"></a>Adicionar usuários ao grupo de segurança usuários sem fio

Você pode usar este procedimento para adicionar um usuário, computador ou grupo ao grupo de segurança sem fio no console de gerenciamento do Active Directory usuários e computadores \(MMC\) snap\-no.

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Para adicionar usuários ao grupo de segurança sem fio

1. Clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. O MMC Usuários e Computadores do Active Directory é aberto. Caso ainda não esteja selecionado, clique no nó do seu domínio. Por exemplo, se o seu domínio for exemplo.com, clique em **exemplo.com**.

2. No painel de detalhes, clique duas vezes\-na pasta que contém o grupo de segurança sem fio.

3. No painel de detalhes, à direita\-clique no grupo segurança sem fio e, em seguida, clique em **Propriedades**. A caixa de diálogo **Propriedades** do grupo de segurança é aberta.

4. Na guia **Membros** , clique em **Adicionar**e, em seguida, conclua um dos procedimentos a seguir para adicionar um computador ou adicionar um usuário ou grupo.

##### <a name="to-add-a-user-or-group"></a>Para adicionar um usuário ou grupo

1. Em **Inserir os nomes de objeto a serem selecionados**, digite o nome do usuário ou grupo que você deseja adicionar e clique em **OK**.

2. Para atribuir a associação de grupo a outros usuários ou grupos, repita a etapa 1 deste procedimento.  

##### <a name="to-add-a-computer"></a>Para adicionar um computador

1. Clique em **Tipos de Objeto**. A caixa de diálogo **tipos de objeto** é aberta.

2. Em **tipos de objeto**, selecione **computadores**e clique em **OK**.

3. Em **Inserir os nomes de objeto a serem selecionados**, digite o nome do computador que você deseja adicionar e clique em **OK**.

4. Para atribuir a associação de grupo a outros computadores, repita as etapas 1\-3 deste procedimento.

## <a name="bkmk_policies"></a>Configurar a rede sem fio \(as políticas de\) do IEEE 802,11

Siga estas etapas para configurar a rede sem fio \(IEEE 802,11\) políticas Política de Grupo extensão:

- [Abrir ou adicionar e abrir um objeto Política de Grupo](#bkmk_opengpme)

- [Ativar a rede sem fio padrão \(as políticas de\) IEEE 802,11](#bkmk_activate)

- [Configurar a nova política de rede sem fio](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Abrir ou adicionar e abrir um objeto Política de Grupo

Por padrão, o recurso de gerenciamento de Política de Grupo é instalado em computadores que executam o Windows Server 2016 quando o Active Directory Domain Services \(AD DS função de servidor\) está instalado e o servidor é configurado como um controlador de domínio. O procedimento a seguir descreve como abrir o Console de Gerenciamento de Política de Grupo \(GPMC\) no controlador de domínio. Em seguida, o procedimento descreve como abrir um domínio existente\-nível Política de Grupo objeto \(GPO\) para edição ou criar um novo GPO de domínio e abri-lo para edição.

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir ou adicionar e abrir um objeto Política de Grupo

1. No controlador de domínio, clique em **Iniciar**, clique em **Ferramentas administrativas do Windows**e, em seguida, clique em gerenciamento de **política de grupo**. O Console de Gerenciamento de Política de Grupo é aberto.  

2. No painel esquerdo, clique duas vezes\-em sua floresta. Por exemplo, clique duas vezes\-clicar em **floresta: example.com**.  

3. No painel esquerdo, clique duas\-vezes em **domínios**e, em seguida,\-clique no domínio para o qual você deseja gerenciar um objeto de política de grupo. Por exemplo, clique duas vezes\-clicar em **example.com**.  

4. Siga um destes procedimentos:

    -   **Para abrir um GPO de nível de\-de domínio existente para edição**, clique duas vezes no domínio que contém o objeto de política de grupo que você deseja gerenciar, direito\-clique na diretiva de domínio que você deseja gerenciar, como a política de domínio padrão e clique em **Editar**. **Editor de gerenciamento de política de grupo** é aberto.

    -   **Para criar um novo objeto política de grupo e abrir para edição**, clique com o botão direito do mouse\-no domínio para o qual você deseja criar um novo objeto política de grupo e, em seguida, clique em **criar um GPO nesse domínio e vincule-o aqui**.

        Em **Novo GPO**, em **Nome**, digite um nome para o novo objeto de Política de Grupo e clique em **OK**.

        À direita\-clique no novo objeto Política de Grupo e clique em **Editar**. **Editor de gerenciamento de política de grupo** é aberto.

Na próxima seção, você usará Editor de Gerenciamento de Política de Grupo para criar a política sem fio.

### <a name="bkmk_activate"></a>Ativar a rede sem fio padrão \(as políticas de\) IEEE 802,11

Este procedimento descreve como ativar a rede sem fio padrão \(as políticas de\) do IEEE 802,11 usando o Editor de Gerenciamento de Política de Grupo \(GPME\).

>[!NOTE]
>Depois de ativar a versão do **Windows Vista e versões posteriores** da rede sem fio \(as políticas de\) do IEEE 802,11 ou a versão do **Windows XP** , a opção de versão será automaticamente removida da lista de opções quando você clicar com o botão direito\-clique em **rede sem fio \(IEEE 802,11\) políticas**. Isso ocorre porque, depois de selecionar uma versão de política, a política é adicionada no painel de detalhes do GPME quando você seleciona a **rede sem fio \(nó de políticas de\) IEEE 802,11** . Esse estado permanece a menos que você exclua a política sem fio, quando a versão da política sem fio retorna à direita\-menu de clique para as **políticas de\) de rede sem fio \(IEEE 802,11** no GPME. Além disso, as políticas sem fio são listadas apenas no painel detalhes do GPME quando o nó **rede sem fio \(IEEE 802,11\) políticas** está selecionado.

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Para ativar a rede sem fio padrão \(as políticas de\) IEEE 802,11  

1. Siga o procedimento anterior **para abrir ou adicionar e abrir um política de grupo objeto** para abrir o GPME.

2. No GPME, no painel esquerdo, clique duas\-vezes em **configuração do computador**, clique duas vezes\-clique em **políticas**, clique duas vezes\-clique em **configurações do Windows**e clique duas vezes\-clique em configurações de **segurança**.

![Política de Grupo sem fio 802.1 x](../../../media/Wireless-GP/Wireless-GP.jpg)

3. Em **configurações de segurança**, clique com o botão direito do mouse em **rede sem fio \(as políticas de\)\-do IEEE 802,11**e clique em **criar uma nova política sem fio para o Windows Vista e versões posteriores**. 

![Política sem fio 802.1 x](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. A caixa de diálogo **Propriedades da nova diretiva de rede sem fio** é aberta. Em **nome da política**, digite um novo nome para a política ou mantenha o nome padrão. Clique em **OK** para salvar a política. A política padrão é ativada e listada no painel de detalhes do GPME com o novo nome fornecido ou com o nome padrão **nova política de rede sem fio**.

![Novas propriedades da política de rede sem fio](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. No painel de detalhes, clique duas vezes\-clicar em **nova política de rede sem fio** para abri-la.

Na próxima seção, você pode executar a configuração de política, a ordem de preferência de processamento de política e as permissões de rede.

### <a name="bkmk_policyconfig"></a>Configurar a nova política de rede sem fio

Você pode usar os procedimentos nesta seção para configurar a rede sem fio \(a política de\) do IEEE 802,11. Essa política permite que você defina configurações de segurança e autenticação, gerencie perfis sem fio e especifique permissões para redes sem fio que não estão configuradas como redes preferenciais.

- [Configurar um perfil de conexão sem fio para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Definir a ordem de preferência para perfis de conexão sem fio](#bkmk_preferenceorder)  

- [Definir permissões de rede](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurar um perfil de conexão sem fio para PEAP\-MS\-CHAP v2

Este procedimento fornece as etapas necessárias para configurar um perfil sem fio PEAP\-MS\-CHAP v2.  

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Para configurar um perfil de conexão sem fio para PEAP\-MS\-CHAP v2

1. No GPME, na caixa de diálogo Propriedades da rede sem fio para a política que você acabou de criar, na guia **geral** e em **Descrição**, digite uma breve descrição para a política.

2. Para especificar que a configuração automática de WLAN é usada para definir as configurações do adaptador de rede sem fio, verifique se **usar o serviço de configuração automática de WLAN do Windows para clientes** está selecionado.  

3. Em **conectar a redes disponíveis na ordem dos perfis listados abaixo**, clique em **Adicionar**e selecione **infraestrutura**. A caixa de diálogo **Propriedades do novo perfil** é aberta.

4. Na caixa de diálogo**Propriedades do novo perfil** , na guia **conexão** , no campo **nome do perfil** , digite um novo nome para o perfil. Por exemplo, digite **perfil de WLAN example.com para Windows 10**.

5. Em **nome da rede\(s\) \(ssid\)** , digite o SSID que corresponde ao SSID configurado em seus APS sem fio e clique em **Adicionar**.

    Se sua implantação utiliza vários SSIDs e cada PA sem fio utiliza as mesmas configurações de segurança sem fio, repita essa etapa para adicionar o SSID de cada PA sem fio ao qual você deseja que esse perfil seja aplicado.

    Se sua implantação utiliza vários SSIDs e as configurações de segurança para cada SSID não são correspondentes, configure um perfil separado para cada grupo de SSIDs que usam as mesmas configurações de segurança. Por exemplo, se você tiver um grupo de APs sem fio configurado para usar o WPA2\-Enterprise e o AES e outro grupo de APs sem fio para usar WPA\-Enterprise e TKIP, configure um perfil para cada grupo de APs sem fio.

6. Se o texto padrão **NEWSSID** estiver presente, selecione-o e clique em **remover**.

7. Se você implantou pontos de acesso sem fio configurados para suprimir o sinalizador de difusão, selecione **Conectar mesmo que não haja difusão na rede**.

    > [!NOTE]
    > Habilitar essa opção pode criar um risco de segurança, pois os clientes sem fio investigarão e tentarão conexões com qualquer rede sem fio. Por padrão, essa configuração não está habilitada.  

8. Clique na guia **Segurança** , clique em **Avançado**e configure:

    1. Para definir as configurações 802.1X avançadas, em **IEEE 802.1X**, selecione **Aplicar configurações 802.1X avançadas**.

        Quando as configurações avançadas do 802.1 X são impostas, os valores padrão para **Max Eapol\-mensagens de início**, **período de retenção**, período de **início**e **período de autenticação** são suficientes para implantações sem fio típicas. Por isso, você não precisa alterar os padrões, a menos que tenha um motivo específico para fazer isso.

    2. Para habilitar o Logon Único, selecione **Habilitar Logon Único para esta rede**.

    3. Os valores padrão restantes em **Logon único** são suficientes para implantações sem fio típicas.

    4. Em **roaming rápido**, se o AP sem fio estiver configurado para autenticação\-, selecione **esta rede usa autenticação de pré\-** .

9. Para especificar que as comunicações sem fio atendam aos padrões FIPS 140\-2, selecione **executar criptografia no modo certificado do fips 140\-2**.

10. Clique em **OK** para retornar à guia **segurança** . Em **selecionar os métodos de segurança para esta rede**, em **autenticação**, selecione **WPA2\-Enterprise** se ele tiver suporte do AP sem fio e adaptadores de rede de cliente sem fio. Caso contrário, selecione **WPA\-Enterprise**.

11. Em **criptografia**, se houver suporte para o AP sem fio e adaptadores de rede de cliente sem fio, selecione **AES-CCMP**. Se você estiver usando pontos de acesso e adaptadores de rede sem fio que dão suporte a AC 802.11, selecione **AES-GCMP**. Caso contrário, selecione **TKIP**.

    > [!NOTE]  
    > As configurações para **autenticação** e **criptografia** devem corresponder às configurações definidas em seus APS sem fio. As configurações padrão para o **modo de autenticação**, as falhas de **autenticação máxima**e **as informações de usuário de cache para conexões subsequentes com essa rede** são suficientes para implantações sem fio típicas.  

12. Em **selecionar um método de autenticação de rede**, selecione **EAP protegidos \(PEAP\)** e clique em **Propriedades**. A caixa de diálogo **Propriedades EAP protegidas** é aberta.

13. Em **Propriedades EAP protegidas**, confirme que **Verifique se a identidade do servidor Validando o certificado** está selecionada.

14. Em **autoridades de certificação raiz confiáveis**, selecione a autoridade de certificação raiz confiável \(\) da AC que emitiu o certificado do servidor para o NPS.

    > [!NOTE]  
    > Essa configuração limita as ACs raiz que os clientes confiam para as ACs selecionadas. Se nenhuma AC raiz confiável for selecionada, os clientes confiarão em todas as CAs raiz listadas em seu repositório de certificados de autoridades de certificação raiz confiáveis.  

15. Na lista **selecionar método de autenticação** , selecione **senha segura \(EAP\-MS\-CHAP v2\)** .

16. Clique em **Configurar**. Na caixa de diálogo **Propriedades EAP MSCHAPv2** , verifique se **usar automaticamente o nome de logon e a senha do Windows \(e o domínio, se qualquer\)** estiver selecionado, e clique em **OK**.

17. Para habilitar a reconexão rápida de PEAP, verifique se **habilitar reconexão rápida** está selecionado.

18. Para exigir TLV de cryptobinding de servidor em tentativas de conexão, selecione **desconectar se o servidor não apresentar TLV de cryptobinding**.

19. Para especificar que a identidade do usuário está mascarada na fase um da autenticação, selecione **Habilitar Privacidade de identidade**e, na caixa de texto, digite um nome de identidade anônimo ou deixe a caixa de texto em branco.

    > [! REGISTRA
    > - A política do NPS para 802.1 X sem fio deve ser criada usando a **política de solicitação de conexão**do NPS. Se a política de NPS for criada usando a **política de rede**do NPS, a privacidade de identidade não funcionará.
    > - A privacidade de identidade EAP é fornecida por determinados métodos EAP em que uma identidade vazia ou anônima \(diferente da identidade real\) é enviada em resposta à solicitação de identidade EAP. O PEAP envia a identidade duas vezes durante a autenticação. Na primeira fase, a identidade é enviada em texto sem formatação e essa identidade é usada para fins de roteamento, não para autenticação de cliente. A identidade real — usada para autenticação — é enviada durante a segunda fase da autenticação, dentro do túnel seguro que é estabelecido na primeira fase. Se a caixa de seleção **Habilitar Privacidade de identidade** estiver marcada, o nome de usuário será substituído pela entrada especificada na caixa de texto. Por exemplo, suponha que **Habilitar Privacidade de identidade** esteja selecionado e que o alias de privacidade de identidade **anônimo** seja especificado na caixa de texto. Para um usuário com um alias de identidade real <strong>jdoe@example.com</strong>, a identidade enviada na primeira fase de autenticação será alterada para <strong>anonymous@example.com</strong>. A parte do Realm da identidade da 1ª fase não é modificada, pois é usada para fins de roteamento.  

20. Clique em **OK** para fechar a caixa de diálogo **Propriedades EAP protegidas** .
21. Clique em **OK** para fechar a guia **segurança** .
22. Se você quiser criar perfis adicionais, clique em **Adicionar**e repita as etapas anteriores, fazendo diferentes escolhas para personalizar cada perfil para os clientes sem fio e a rede para a qual você deseja aplicar o perfil. Quando terminar de adicionar perfis, clique em **OK** para fechar a caixa de diálogo Propriedades da diretiva de rede sem fio.

Na próxima seção, você pode ordenar os perfis de política para garantir a segurança ideal.

#### <a name="bkmk_preferenceorder"></a>Definir a ordem de preferência para perfis de conexão sem fio
Você pode usar este procedimento se tiver criado vários perfis sem fio em sua política de rede sem fio e desejar solicitar os perfis para obter a eficácia e a segurança ideais.

Para garantir que os clientes sem fio se conectem com o nível mais alto de segurança que eles podem dar suporte, coloque suas políticas mais restritivas na parte superior da lista.

Por exemplo, se você tiver dois perfis, um para clientes que dão suporte a WPA2 e outro para clientes que dão suporte a WPA, coloque o perfil WPA2 mais alto na lista. Isso garante que os clientes que dão suporte a WPA2 usarão esse método para a conexão em vez do WPA menos seguro.

Este procedimento fornece as etapas para especificar a ordem na qual os perfis de conexão sem fio são usados para conectar clientes sem fio membros do domínio a redes sem fio.

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Para definir a ordem de preferência para perfis de conexão sem fio

1. No GPME, na caixa de diálogo Propriedades da rede sem fio para a política que você acabou de configurar, clique na guia **geral** .

2. Na guia **geral** , em **conectar a redes disponíveis na ordem dos perfis listados abaixo**, selecione o perfil que você deseja mover na lista e clique no botão "seta para cima" ou no botão "seta para baixo" para mover o perfil para o local desejado na lista.

3.  Repita a etapa 2 para cada perfil que você deseja mover na lista.  

4.  Clique em **OK** para salvar todas as alterações.

Na seção a seguir, você pode definir permissões de rede para a política sem fio.

#### <a name="bkmk_permissions"></a>Definir permissões de rede
Você pode definir as configurações na guia **permissões de rede** para os membros do domínio aos quais a rede sem fio \(IEEE 802,11\) políticas se aplicam.

Você só pode aplicar as seguintes configurações para redes sem fio que não estão configuradas na guia **geral** da página **Propriedades da política de rede sem fio** :

- Permitir ou negar conexões a redes sem fio específicas que você especificar por tipo de rede e identificador de conjunto de serviços \(SSID\)

- Permitir ou negar conexões a redes ad hoc

- Permitir ou negar conexões a redes de infraestrutura

- Permitir ou negar que os usuários exibam tipos de rede \(\) de infraestrutura ou ad hoc aos quais eles têm acesso negado

- Permitir ou negar que os usuários criem um perfil que se aplica a todos os usuários

- Os usuários só podem se conectar a redes permitidas usando perfis de Política de Grupo

A associação em **Admins**. do domínio, ou equivalente, é o mínimo necessário para concluir esses procedimentos.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Para permitir ou negar conexões a redes sem fio específicas

1. No GPME, na caixa de diálogo Propriedades da rede sem fio, clique na guia **permissões de rede** .

2. Na guia **permissões de rede** , clique em **Adicionar**. A caixa de diálogo **nova entrada de permissões** é aberta.

3. Na caixa de diálogo **nova entrada de permissão** , no campo **nome da rede \(SSID\)** , digite o SSID da rede para o qual você deseja definir permissões.

4.  Em **tipo de rede**, selecione **infraestrutura** ou **ad hoc**.

    > [!NOTE]  
    > Se você não tiver certeza se a rede de difusão é uma infraestrutura ou rede ad hoc, você pode configurar duas entradas de permissão de rede, uma para cada tipo de rede.

5. Em **permissão**, selecione **permitir** ou **negar**.

6. Clique em **OK**para retornar à guia **permissões de rede** .

##### <a name="to-specify-additional-network-permissions-optional"></a>Para especificar permissões de rede adicionais \(\) opcional

1.  Na guia **permissões de rede** , configure um ou todos os itens a seguir:  

    -   Para negar acesso de membros de domínio a redes ad hoc, selecione **impedir conexões com redes ad\-hoc**.

    -   Para negar o acesso de membros de domínio a redes de infraestrutura, selecione **impedir conexões a redes de infraestrutura**.  

    -   Para permitir que os membros do domínio exibam tipos de rede \(ad hoc ou\) de infraestrutura aos quais eles têm acesso negado, selecione **permitir que o usuário exiba redes negadas**.

    -   Para permitir que os usuários criem perfis que se aplicam a todos os usuários, selecione **permitir que todos criem todos os perfis de usuário**.

    -   Para especificar que os usuários só podem se conectar a redes permitidas usando perfis de Política de Grupo, selecione **usar somente perfis de política de grupo para redes permitidas**.

## <a name="bkmk_nps"></a>Configurar seu NPSs
Siga estas etapas para configurar o NPSs para executar a autenticação 802.1 X para acesso sem fio:

- [Registrar o NPS no Active Directory Domain Services](#bkmk_npsreg)

- [Configurar um AP sem fio como um cliente RADIUS NPS](#bkmk_radiusclient)

- [Criar políticas de NPS para 802.1 X sem fio usando um assistente](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrar o NPS no Active Directory Domain Services
Você pode usar este procedimento para registrar um servidor que esteja executando o servidor de diretivas de rede \(NPS\) no Active Directory Domain Services \(AD DS no domínio em que o NPS é membro.\) Para que o NPSs receba permissão para ler a\-de discagem em Propriedades de contas de usuário durante o processo de autorização, cada NPS deve ser registrado em AD DS. O registro de um NPS adiciona o servidor ao grupo de segurança **Servidores RAS e ias** em AD DS.

>[!NOTE]
>Você pode instalar o NPS em um controlador de domínio ou em um servidor dedicado. Execute o seguinte comando do Windows PowerShell para instalar o NPS, se você ainda não tiver feito isso:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

#### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar um NPS em seu domínio padrão

1. No seu NPS, em **Gerenciador do servidor**, clique em **ferramentas**e em **servidor de políticas de rede**. O snap\-do NPS é aberto.

2. Direito\-clique em **NPS \(Local\)** e, em seguida, clique em **registrar servidor no Active Directory**. A caixa de diálogo **Servidor de Políticas de Rede** é aberta.

3. Em **Servidor de Políticas de Rede**, clique em **OK** e em **OK** novamente.

### <a name="bkmk_radiusclient"></a>Configurar um AP sem fio como um cliente RADIUS NPS
Você pode usar este procedimento para configurar um AP, também conhecido como *servidor de acesso à rede \(\)do nas* , como um\-de autenticação remota no serviço de usuário \(o cliente do RADIUS\) usando o snap\-do NPS no. 

>[!IMPORTANT]
>Computadores clientes, como computadores portáteis sem fio e outros computadores executando sistemas operacionais de cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede — como pontos de acesso sem fio, switches compatíveis com\-802.1 X, rede virtual privada \(servidores VPN\) e servidores dial\-up — porque usam o protocolo RADIUS para se comunicar com servidores RADIUS, como NPSs.

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para adicionar um servidor de acesso à rede como um cliente RADIUS no NPS

1. No seu NPS, em **Gerenciador do servidor**, clique em **ferramentas**e em **servidor de políticas de rede**. O snap\-do NPS é aberto.

2. No snap-in do NPS\-no, clique duas vezes\-clique em **clientes e servidores RADIUS**. Clique com o\-botão direito do mouse em **clientes RADIUS**e clique em **novo**.

3. Em **novo cliente RADIUS**, verifique se a caixa de seleção **habilitar este cliente RADIUS** está marcada.

4. Em **novo cliente RADIUS**, em **nome amigável**, digite um nome de exibição para o ponto de acesso sem fio.

    Por exemplo, se você quiser adicionar um ponto de acesso sem fio \(AP\) chamado AP\-01, digite **ap\-01**.

5. Em **endereço \(\)IP ou DNS** , digite o endereço IP ou o nome de domínio totalmente qualificado \(FQDN\) para o nas.

    Se você inserir o FQDN, para verificar se o nome está correto e é mapeado para um endereço IP válido, clique em **verificar**e, em **verificar endereço**, no campo **endereço** , clique em **resolver**. Se o nome FQDN for mapeado para um endereço IP válido, o endereço IP desse NAS será exibido automaticamente no **endereço IP**. Se o FQDN não for resolvido para um endereço IP, você receberá uma mensagem indicando que esse host não é conhecido. Se isso ocorrer, verifique se você tem o nome do AP correto e se o AP está ligado e conectado à rede.  

    Clique em **OK** para fechar **verificar endereço**.  

6. Em **novo cliente RADIUS**, em **segredo compartilhado**, siga um destes procedimentos:  

    -   Para configurar manualmente um segredo compartilhado RADIUS, selecione **manual**e, em **segredo compartilhado**, digite a senha forte que também é inserida no nas. Digite novamente o segredo compartilhado em **confirmar segredo compartilhado**.  

    -   Para gerar automaticamente um segredo compartilhado, marque a caixa de seleção **gerar** e, em seguida, clique no botão **gerar** . Salve o segredo compartilhado gerado e, em seguida, use esse valor para configurar o NAS para que ele possa se comunicar com o NPS.  

        >[!IMPORTANT]
        >O segredo compartilhado RADIUS que você insere para seus pontos de acesso virtuais no NPS deve corresponder exatamente ao segredo compartilhado RADIUS configurado em seu AP sem fio real. Se você usar a opção NPS para gerar um segredo compartilhado RADIUS, deverá configurar o AP sem fio real correspondente com o segredo compartilhado RADIUS gerado pelo NPS.

7. Em **novo cliente RADIUS**, na guia **avançado** , em **nome do fornecedor**, ESPECIFIQUE o nome do fabricante do nas. Se você não tiver certeza do nome do fabricante do NAS, selecione **RADIUS padrão**.

8. Em **Opções adicionais**, se você estiver usando qualquer método de autenticação diferente de EAP e PEAP, e se o seu nas der suporte ao uso do atributo autenticador de mensagem, selecione **as mensagens de solicitação de acesso devem conter a mensagem\-atributo autenticador**.

9. Clique em **OK**. Seu NAS aparece na lista de clientes RADIUS configurados no NPS.

### <a name="bkmk_npspolicy"></a>Criar políticas de NPS para 802.1 X sem fio usando um assistente
Você pode usar este procedimento para criar as políticas de solicitação de conexão e as políticas de rede necessárias para implantar o 802.1 X\-pontos de acesso sem fio com capacidade de autenticação remota\-no serviço de usuário \(RADIUS\) clientes para o servidor RADIUS que está executando o servidor de políticas de rede \(NPS\).  
Depois de executar o assistente, as seguintes políticas são criadas:

- Uma política de solicitação de conexão

- Uma política de rede

>[!NOTE]
>Você pode executar o novo assistente de conexões com fio e sem fio IEEE 802.1 X seguras toda vez que precisar criar novas políticas para acesso autenticado 802.1 X.

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Criar políticas para 802.1 X autenticado sem fio usando um assistente

1. Abra o\-de snap do NPS no. Se ainda não estiver selecionado, clique em **NPS \(Local\)** . Se você estiver executando o snap do MMC do NPS\-no e quiser criar políticas em um NPS remoto, selecione o servidor.

2. Em **introdução**, em **configuração padrão**, selecione **servidor RADIUS para conexões 802.1 x sem fio ou com fio**. O texto e os links abaixo do texto são alterados para refletir sua seleção.

3. Clique em **Configurar 802.1 x**. O assistente para configurar 802.1 X é aberto.

4.  Na página **selecionar assistente de tipo de conexões 802.1 x** , em **tipo de conexões 802.1 x**, selecione **conexões sem fio seguras**e, em **nome**, digite um nome para a política ou deixe o nome padrão **conexões sem fio seguras**. Clique em **Avançar**.

5.  Na página especificar o assistente de **comutadores 802.1 x** , em **clientes RADIUS**, todos os comutadores 802.1 x e pontos de acesso sem fio adicionados como clientes RADIUS no snap\-do NPS no são mostrados. Siga qualquer um destes procedimentos:

    -   Para adicionar servidores de acesso à rede adicionais \(NASs\), como APs sem fio, em **clientes RADIUS**, clique em **Adicionar**e, em seguida, em **novo cliente RADIUS**, insira as informações para: **nome amigável**, **endereço \(IP ou DNS\)** e **segredo compartilhado**.

    -   Para modificar as configurações de qualquer NAS, em **clientes RADIUS**, selecione o AP para o qual você deseja modificar as configurações e clique em **Editar**. Modifique as configurações conforme necessário.

    -   Para remover um NAS da lista, em **clientes RADIUS**, selecione o nas e clique em **remover**.

        >[!WARNING]
        >A remoção de um cliente RADIUS de dentro do assistente para **Configurar 802.1 x** exclui o cliente da configuração do NPS. Todas as adições, modificações e exclusões feitas dentro do assistente para **Configurar 802.1 x** para clientes RADIUS são refletidas no snap\-do NPS no, no nó **clientes RADIUS** em **clientes e servidores RADIUS**do **NPS** \/. Por exemplo, se você usar o assistente para remover uma opção 802.1 X, a opção também será removida do snap\-do NPS no.

6. Clique em **Avançar**. Na página Assistente para **configurar um método de autenticação** , **no tipo \(com base no método de\)de configuração de acesso e rede** , selecione **Microsoft: EAP protegido \(PEAP\)** e clique em **Configurar**.

    >[!TIP]
    >Se você receber uma mensagem de erro indicando que um certificado não pode ser encontrado para uso com o método de autenticação, e você tiver configurado Active Directory serviços de certificados para emitir certificados automaticamente para servidores RAS e IAS em sua rede, primeiro verifique se você seguiu as etapas para registrar o NPS no Active Directory Domain Services, em seguida, use as seguintes etapas para atualizar Política de Grupo: clique em **Iniciar**, clique em **sistema Windows**, clique em **executar**e, em **abrir**, digite **gpupdate** e pressione ENTER. Quando o comando retornar resultados indicando que o usuário e o computador Política de Grupo foram atualizados com êxito, selecione **Microsoft: EAP protegido \(PEAP\)** novamente e clique em **Configurar**.
    >
    >Se depois de atualizar Política de Grupo você continuar a receber a mensagem de erro indicando que um certificado não pode ser encontrado para uso com o método de autenticação, o certificado não será exibido porque não atende aos requisitos mínimos de certificado do servidor, conforme documentado no guia complementar da rede principal: [implantar certificados de servidor para implantações com e sem fio 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Se isso acontecer, você deverá descontinuar a configuração do NPS, revogar o certificado emitido para o\)do NPS\(s e, em seguida, seguir as instruções para configurar um novo certificado usando o guia de implantação de certificados do servidor.

7.  Na página **Editar propriedades de EAP protegidas** , em **certificado emitido**, verifique se o certificado NPS correto está selecionado e, em seguida, faça o seguinte:

    >[!NOTE]
    >Verifique se o valor no **emissor** está correto para o certificado selecionado em **certificado emitido**. Por exemplo, o emissor esperado para um certificado emitido por uma autoridade de certificação que executa Active Directory serviços de certificados \(o AD CS\) chamado corp\DC1, no domínio contoso.com, é **corp\-DC1\-AC**.

    -   Para permitir que os usuários façam roaming com seus computadores sem fio entre pontos de acesso sem precisar reautenticar cada vez que eles se associarem a um novo AP, selecione **habilitar reconexão rápida**.

    -   Para especificar que a conexão de clientes sem fio encerrará o processo de autenticação de rede se o servidor RADIUS não apresentar o tipo cryptobinding\-comprimento\-valor \(TLV\), selecione **Desconectar clientes sem cryptobinding**.  

    -   Para modificar as configurações de política para o tipo de EAP, em **tipos de EAP**, clique em **Editar**, em **Propriedades EAP MSCHAPv2**, modifique as configurações conforme necessário e clique em **OK**.  

8.  Clique em **OK**. A caixa de diálogo Editar propriedades EAP protegidas é fechada, retornando você ao Assistente para **Configurar 802.1 x** . Clique em **Avançar**.

9. Em **especificar grupos de usuários**, clique em **Adicionar**e digite o nome do grupo de segurança que você configurou para seus clientes sem fio no snap Active Directory usuários e computadores\-no. Por exemplo, se você tiver nomeado seu grupo sem fio do grupo de segurança sem fio, digite **grupo sem fio**. Clique em **Avançar**.

10. Clique em **Configurar** para configurar atributos padrão RADIUS e fornecedor\-atributos específicos para a LAN virtual \(VLAN\) conforme necessário e, conforme especificado pela documentação fornecida pelo seu fornecedor de hardware de AP sem fio. Clique em **Avançar**.

11. Examine os detalhes do resumo da configuração e clique em **concluir**.

Suas políticas de NPS agora são criadas e você pode passar para ingressar computadores sem fio no domínio.

## <a name="bkmk_domain"></a>Unir novos computadores sem fio ao domínio
O método mais fácil de unir novos computadores sem fio ao domínio é anexar fisicamente o computador a um segmento da LAN com fio \(um segmento não controlado por um comutador 802.1 X\) antes de ingressar o computador no domínio. Isso é mais fácil porque as configurações da diretiva de grupo sem fio são aplicadas automaticamente e imediatamente e, se você tiver implantado sua própria PKI, o computador receberá o certificado de autoridade de certificação e o colocará no repositório de certificados de autoridades de certificação raiz confiáveis, permitir que o cliente sem fio confie no NPSs com certificados de servidor emitidos pela sua autoridade de certificação.

Da mesma forma, depois que um novo computador sem fio é ingressado no domínio, o método preferencial para que os usuários façam logon no domínio é fazer logon usando uma conexão com fio com a rede.

### <a name="other-domain-join-methods"></a>Outros métodos de ingresso no domínio
Nos casos em que não é prático unir computadores ao domínio usando uma conexão Ethernet com fio ou, em casos em que o usuário não pode fazer logon no domínio pela primeira vez usando uma conexão com fio, você deve usar um método alternativo.

- **Configuração do computador da equipe de ti**. Um membro da equipe de ti une um computador sem fio ao domínio e configura um perfil sem fio de Bootstrap de logon único. Com esse método, o administrador de ti conecta o computador sem fio à rede Ethernet com fio e une o computador ao domínio. Em seguida, o administrador distribui o computador ao usuário. Quando o usuário inicia o computador sem usar uma conexão com fio, as credenciais de domínio que eles especificam manualmente para o logon do usuário são usadas para estabelecer uma conexão com a rede sem fio e fazer logon no domínio.

Para obter mais informações, consulte a seção [ingressar no domínio e fazer logon usando o método de configuração do computador da equipe de ti](#bkmk_itstaff)

-   **Configuração de perfil sem fio de Bootstrap por usuários**. O usuário configura manualmente o computador sem fio com um perfil sem fio de Bootstrap e ingressa no domínio, com base nas instruções adquiridas de um administrador de ti. O perfil sem fio de Bootstrap permite que o usuário estabeleça uma conexão sem fio e, em seguida, ingresse no domínio. Depois de ingressar o computador no domínio e reiniciar o computador, o usuário poderá fazer logon no domínio usando uma conexão sem fio e suas credenciais de conta de domínio.

Para obter mais informações, consulte a seção [ingressar no domínio e fazer logon usando a configuração de perfil sem fio de Bootstrap por usuários](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Ingressar no domínio e fazer logon usando o método de configuração do computador da equipe de ti
Os usuários membros do domínio com computadores cliente sem fio ingressados no domínio\-podem usar um perfil sem fio temporário para se conectar a uma rede sem fio autenticada 802.1 X\-sem primeiro se conectar à LAN com fio. Esse perfil sem fio temporário é chamado de *perfil sem fio de Bootstrap*.

Um perfil sem fio de Bootstrap exige que o usuário especifique manualmente suas credenciais de conta de usuário de domínio e não valida o certificado do\-de discagem de autenticação remota no serviço de usuário \(servidor de\) RADIUS que executa o servidor de diretivas de rede \(\)do NPS.

Depois que a conectividade sem fio é estabelecida, o Política de Grupo é aplicado no computador cliente sem fio e um novo perfil sem fio é emitido automaticamente. A nova política usa as credenciais de conta de usuário e computador para autenticação de cliente. 

Além disso, como parte da autenticação mútua do protocolo PEAP\-MS\-CHAP v2 usando o novo perfil em vez do perfil Bootstrap, o cliente valida as credenciais do servidor RADIUS.

Depois de ingressar o computador no domínio, use este procedimento para configurar um perfil sem fio de Bootstrap de logon único, antes de distribuir o computador sem fio para o domínio\-usuário membro.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Para configurar um perfil sem fio de Bootstrap de logon único

1. Crie um perfil de inicialização usando o procedimento neste guia chamado [configurar um perfil de conexão sem fio para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)e use as seguintes configurações:

    - Autenticação do protocolo PEAP\-MS\-CHAP v2

    - Validar certificado do servidor RADIUS desabilitado

    - Logon único habilitado

2. Nas propriedades da política de rede sem fio na qual você criou o novo perfil de inicialização, na guia **geral** , selecione o perfil Bootstrap e clique em **Exportar** para exportar o perfil para um compartilhamento de rede, unidade flash USB ou outro local facilmente acessível. O perfil é salvo como um arquivo *. xml no local que você especificar.

3. Ingresse o novo computador sem fio no domínio \(por exemplo, por meio de uma conexão Ethernet que não exija a autenticação IEEE 802.1 X\) e adicione o perfil sem fio de Bootstrap ao computador usando o comando **netsh wlan Add Profile** .

    >[!NOTE]
    >Para obter mais informações, consulte comandos netsh para rede local sem fio \(WLAN\) em [http:\/\/technet.microsoft.com\/library\/dd744890. aspx](https://technet.microsoft.com/library/dd744890).

4. Distribua o novo computador sem fio para o usuário com o procedimento para "fazer logon no domínio usando computadores que executam o Windows 10".

Quando o usuário inicia o computador, o Windows solicita que o usuário insira seu nome de conta de usuário de domínio e senha. Como o logon único está habilitado, o computador usa as credenciais da conta de usuário de domínio para primeiro estabelecer uma conexão com a rede sem fio e, em seguida, fazer logon no domínio.

#### <a name="bkmk_w10"></a>Faça logon no domínio usando computadores que executam o Windows 10

1. Faça logoff do computador ou o reinicie.

2. Pressione qualquer tecla no teclado ou clique na área de trabalho. A tela de logon aparece com um nome de conta de usuário local exibido e um campo de entrada de senha abaixo do nome. Não faça logon com a conta de usuário local.

3. No canto inferior esquerdo da tela, clique em **outro usuário**. A tela logon de outro usuário é exibida com dois campos, um para nome de usuário e outro para senha. Abaixo do campo senha está o texto de **entrada para:** e o nome do domínio no qual o computador está ingressado. Por exemplo, se o seu domínio for denominado example.com, o texto lerá a **entrada para: example**.

4. Em **nome de usuário**, digite seu nome de usuário de domínio.

5. Em **Senha**, digite sua senha do domínio e clique na seta ou pressione ENTER.

>[!NOTE]
>Se a tela **outro usuário** não incluir o logon de texto em **:** e seu nome de domínio, você deverá inserir seu nome de usuário no formato *domínio\\usuário*. Por exemplo, para fazer logon no domínio example.com com uma conta denominada **usuário\-01**, digite o **exemplo\\usuário\-01**.

### <a name="bkmk_userbootstrap"></a>Ingressar no domínio e fazer logon usando a configuração de perfil sem fio de Bootstrap por usuários
Com esse método, você conclui as etapas na seção etapas gerais e, em seguida, fornece ao seu domínio\-usuários Membros com as instruções sobre como configurar manualmente um computador sem fio com um perfil sem fio de bootstrap. O perfil sem fio de Bootstrap permite que o usuário estabeleça uma conexão sem fio e, em seguida, ingresse no domínio. Depois que o computador é ingressado no domínio e reiniciado, o usuário pode fazer logon no domínio por meio de uma conexão sem fio.

#### <a name="general-steps"></a>Etapas gerais

1. Configure uma conta de administrador do computador local, no **painel de controle**, para o usuário.

    >[!IMPORTANT]
    >Para ingressar um computador em um domínio, o usuário deve estar conectado ao computador com a conta de administrador local. Como alternativa, o usuário deve fornecer as credenciais para a conta de administrador local durante o processo de ingresso do computador no domínio. Além disso, o usuário deve ter uma conta de usuário no domínio no qual o usuário deseja ingressar no computador. Durante o processo de ingressar o computador no domínio, o usuário será solicitado a fornecer credenciais de conta de domínio \(nome de usuário e senha\).

2. Forneça aos usuários do domínio as instruções para configurar um perfil sem fio de Bootstrap, conforme documentado no procedimento **a seguir para configurar um perfil sem fio de Bootstrap**.
3. Além disso, forneça aos usuários as credenciais do computador local \(nome de usuário e senha\)e as credenciais de domínio \(nome da conta de usuário de domínio e senha\) no formato *domainname\\username*, bem como os procedimentos para "ingressar o computador no domínio" e para "fazer logon no domínio", conforme documentado no [Guia de rede](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)do Windows Server 2016 Core.

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Para configurar um perfil sem fio de Bootstrap

1. Use as credenciais fornecidas pelo administrador de rede ou pelo profissional de suporte de ti para fazer logon no computador com a conta de administrador do computador local.

2. À direita\-clique no ícone de rede na área de trabalho e clique em **Abrir Central de rede e compartilhamento**. A **Central de Rede e Compartilhamento** será aberta. Em **alterar as configurações de rede**, clique em **Configurar uma nova conexão ou rede**. A caixa de diálogo **Configurar uma conexão ou rede** é aberta.

3. Clique em **conectar-se manualmente a uma rede sem fio**e clique em **Avançar**.

4. Em **conectar-se manualmente a uma rede sem fio**, em **nome da rede**, digite o nome SSID do AP.  

5. Em **tipo de segurança**, selecione a configuração fornecida pelo administrador.

6. Em **tipo de criptografia** e **chave de segurança**, selecione ou digite as configurações fornecidas pelo administrador.

7. Selecione **iniciar esta conexão automaticamente**e clique em **Avançar**.

8. Em **adicionado com êxito * * * seu SSID de rede*, clique em **alterar configurações de conexão**.

9. Clique em **alterar configurações de conexão**. A caixa de diálogo propriedade de rede sem fio Network *SSID* é aberta.

10. Clique na guia **segurança** e, em **escolha um método de autenticação de rede**, selecione **EAP protegido \(PEAP\)** .

11. Clique em **Configurações**. A página **Propriedades do EAP protegido \(PEAP\)** é aberta.

12. Na página **Propriedades do EAP protegido \(PEAP\)** , verifique se **validar certificado do servidor** não está selecionado, clique em **OK** duas vezes e, em seguida, clique em **fechar**.

13. Em seguida, o Windows tenta se conectar à rede sem fio. As configurações do perfil sem fio de Bootstrap especificam que você deve fornecer suas credenciais de domínio. Quando o Windows solicitar um nome de conta e uma senha, digite suas credenciais de conta de domínio da seguinte maneira: *nome de domínio\\nome de usuário*, *senha de domínio*.

##### <a name="to-join-a-computer-to-the-domain"></a>Para ingressar um computador no domínio

1. Faça logon no computador com a conta de Administrador local.

2. Na caixa de texto Pesquisar, digite **PowerShell**. Nos resultados da pesquisa, clique com o botão direito do mouse em **Windows PowerShell**e clique em **Executar como administrador**. O Windows PowerShell é aberto com um prompt com privilégios elevados.

3. No Windows PowerShell, digite o comando a seguir e pressione ENTER. Certifique-se de substituir a variável nomedodomínio pelo nome do domínio que você deseja unir.
    
    Add-computador nome_do_domínio
    
4. Quando solicitado, digite o nome de usuário e a senha do domínio e clique em **OK**.
5. Reinicie o computador.
6. Siga as instruções na seção anterior [fazer logon no domínio usando computadores que executam o Windows 10](#bkmk_w10).
