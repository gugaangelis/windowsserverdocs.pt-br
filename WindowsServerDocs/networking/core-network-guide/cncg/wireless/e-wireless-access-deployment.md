---
title: Implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseada em senha 802.1 X autenticado sem fio acesso"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f78da14b91e5c1e9a2dc22f92ea95c89153befa8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment"></a>Implantação de acesso sem fio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Siga estas etapas para implantar o acesso sem fio:

- [Implantar e configurar pontos de acesso sem fio](#bkmk_aps)

- [Criar um grupo de segurança de usuários de acesso sem fio](#bkmk_groups)

- [Configurar a rede sem fio \ (IEEE 802.11\) políticas](#bkmk_policies)

- [Configure os servidores NPS](#bkmk_nps)

- [Adicione novos computadores sem fio ao domínio](#bkmk_domain)

## <a name="bkmk_aps"></a>Implantar e configurar pontos de acesso sem fio

Siga estas etapas para implantar e configurar os pontos de acesso sem fio:

- [Especificar frequências de canal de ponto de acesso sem fio](#bkmk_channel)

- [Configurar pontos de acesso sem fio](#bkmk_wirelessaps)

>[!NOTE]
>Os procedimentos neste guia não inclua instruções para casos em que o **User Account Control** abre a caixa de diálogo para solicitar sua permissão para continuar. Se essa caixa de diálogo Abrir enquanto você estiver executando os procedimentos neste guia e se a caixa de diálogo estava aberta em resposta a ações, clique em **continuar**.

### <a name="bkmk_channel"></a>Especificar frequências de canal de ponto de acesso sem fio

Ao implantar vários pontos de acesso sem fio em um único local geográfico, você deve configurar pontos de acesso sem fio que têm sinais sobrepostos usar frequências de canal exclusivo para reduzir a interferência entre pontos de acesso sem fio.

Você pode usar as seguintes diretrizes para ajudá-lo a escolher frequências de canal que não entrem em conflito com outras redes sem fio na localização geográfica da sua rede sem fio.

- Se houver outras organizações que têm escritórios em estreita proximidade ou no mesmo prédio como sua organização, identificam se há quaisquer redes sem fio pertencentes a essas empresas. Descubra o posicionamento e o canal atribuído frequências do seus PA sem fio, porque você precisa atribuir frequências de canal diferentes para seu ponto de acesso e você precisa determinar o melhor local para instalar o ponto de acesso

- Identifique a sobreposição de sinais sem fio em andares adjacentes em sua própria organização. Depois de identificação de sobreposição de áreas de cobertura fora e dentro da organização, atribuir frequências de canal para os pontos de acesso sem fio, assegurando que dois pontos de acesso sem fio com cobertura de sobreposição são atribuídos frequências de canal diferente.

### <a name="bkmk_wirelessaps"></a>Configurar pontos de acesso sem fio

Use as seguintes informações juntamente com a documentação do produto fornecida pelo fabricante do ponto de acesso sem fio para definir os pontos de acesso sem fio.

Este procedimento enumera itens comumente configurados em um ponto de acesso sem fio. Os nomes de item podem variar de acordo com a marca e modelo e podem ser diferentes na lista a seguir. Para obter detalhes específicos, consulte a documentação do ponto de acesso sem fio.

#### <a name="to-configure-your-wireless-aps"></a>Para definir os pontos de acesso sem fio  

- **SSID**. Especifique o nome do network\(s\) sem fio \ (por exemplo, ExampleWLAN\). Este é o nome que está anunciado para clientes sem fio.

- **Criptografia**. Especifica WPA2\ Enterprise \(preferred\) ou WPA\-Enterprise e AES \(preferred\) ou codificação de criptografia TKIP, dependendo de qual versões são compatíveis com os adaptadores de rede do computador cliente sem fio.

- **Sem fio PA IP endereço \(static\)**. Em cada ponto de acesso, configure um endereço IP estático exclusivo que esteja dentro do intervalo de exclusão do escopo para a sub-rede DHCP. Usando um endereço que é excluído de atribuição pelo DHCP impede que o servidor DHCP atribuindo o mesmo endereço IP em um computador ou outro dispositivo.

- **Máscara de sub-rede**. Configure isso para corresponder às configurações de máscara de sub-rede da LAN para o qual você se conectou o ponto de acesso sem fio.  

- **Nome DNS**. Alguns pontos de acesso sem fio podem ser configurados com um nome DNS. O serviço DNS na rede pode resolver nomes DNS para um endereço IP. Em cada ponto de acesso sem fio que dá suporte a esse recurso, insira um nome exclusivo para resolução DNS.  

- **Serviço DHCP**. Se o seu ponto de acesso sem fio tiver um serviço DHCP built\, desabilitá-la.  

- **Segredo compartilhado RADIUS**. Use um segredo compartilhado do RAIO exclusivo para cada ponto de acesso sem fio, a menos que você pretende configurar pontos de acesso como clientes RADIUS no NPS por grupo. Se você pretende configurar pontos de acesso pelo grupo no NPS, o segredo compartilhado deve ser o mesmo para todos os membros do grupo. Além disso, cada segredo compartilhado que você usa deve ser uma sequência aleatória de pelo menos 22 caracteres que combina maiusculas e minúsculas, números e pontuação. Para garantir aleatoriedade, você pode usar um gerador de caracteres aleatórios, como o gerador de caracteres aleatórios encontrado no NPS **configurar 802.1 X** Assistente para criar os segredos compartilhados.

>[!TIP]
>Registre o segredo compartilhado para cada ponto de acesso sem fio e armazená-la em um local seguro, como um escritório seguro. Você deve saber o segredo compartilhado para cada ponto de acesso sem fio quando você configurar clientes RADIUS no NPS.  

- **Endereço IP do servidor RADIUS**. Digite o endereço IP do servidor NPS.

- **UDP port\(s\)**. Por padrão, o NPS usa as portas UDP 1812 e 1645 para autenticação mensagens e portas UDP 1813 e 1646 para mensagens de contabilidade. É recomendável que você usa essas mesmas portas UDP no seu PAS, mas se você tiver um motivo válido para usar diferentes portas, certifique-se de que você não apenas configurar pontos de acesso com os novo números de porta, mas também reconfigura todos os servidores NPS para usar os mesmos números de porta como os pontos de acesso. Se os pontos de acesso e os servidores NPS não estão configurados com as mesmas portas UDP, NPS não pode receber ou processar solicitações de conexão dos pontos de acesso, e todas as tentativas de conexão sem fio na rede falhará.

- **VSAs**. Alguns pontos de acesso sem fio exigem \(VSAs\) atributos vendor\ específicos para fornecer a funcionalidade de ponto de acesso sem fio completo. VSAs são adicionados na política de rede NPS.

- **Filtragem de DHCP**. Configure pontos de acesso sem fio para bloquear clientes sem fio de enviar pacotes IP de porta UDP 68 à rede, conforme documentado pelo fabricante do ponto de acesso sem fio.

- **Filtragem de DNS**. Configure pontos de acesso sem fio para bloquear clientes sem fio de enviar pacotes IP de porta TCP ou UDP 53 à rede, conforme documentado pelo fabricante do ponto de acesso sem fio.

## <a name="create-security-groups-for-wireless-users"></a>Criar grupos de segurança para usuários sem fio

Siga estas etapas para criar um ou mais usuários sem fio grupos de segurança e, em seguida, adicionar usuários ao grupo de segurança aos usuários apropriados de sem fio:

- [Criar um grupo de segurança de usuários de acesso sem fio](#bkmk_groups)

- [Adicionar usuários ao grupo de segurança de acesso sem fio](#bkmk_addusers)

### <a name="bkmk_groups"></a>Criar um grupo de segurança de usuários de acesso sem fio

Você pode usar este procedimento para criar um grupo de segurança sem fio no Active Directory Users e Console de gerenciamento Microsoft computadores \(MMC\) snap\-no.  

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

#### <a name="to-create-a-wireless-users-security-group"></a>Para criar um grupo de segurança de usuários sem fio

1. Clique em **iniciar**, clique em **ferramentas administrativas**e clique em **usuários e Active Directory computadores**. O Active Directory usuários e computadores em snap\ abre. Se não ainda estiver selecionada, clique no nó do seu domínio. Por exemplo, se o domínio for example.com, clique em **example.com**.

2. No painel de detalhes, clique na pasta right\ no qual você deseja adicionar um novo grupo \ (por exemplo, clique em right\ **usuários**\), aponte para **nova**e clique em **grupo**.

3. Em **novo objeto – grupo**, na **nome do grupo**, digite o nome do novo grupo. Por exemplo, digite **grupo sem fio**.

4. Em **agrupar escopo**, selecione uma das seguintes opções:

    - **Domínio local**

    - **Global**

    - **Universal**

5. Em **agrupar tipo**, selecione **segurança**.

6. Clique em **Okey**.

Se você precisar de mais de um grupo de segurança para usuários sem fio, repita essas etapas para criar grupos de usuários adicionais sem fio. Mais tarde, você pode criar políticas de rede individuais no NPS para aplicar diferentes condições e restrições para cada grupo, fornecendo as permissões de acesso diferentes e regras de conectividade.

### <a name="bkmk_addusers"></a>Adicionar usuários para o grupo de segurança de usuários de acesso sem fio

Você pode usar este procedimento para adicionar um usuário, computador ou grupo ao seu grupo de segurança sem fio no Active Directory Users e Console de gerenciamento Microsoft computadores \(MMC\) snap\-no.

A associação ao grupo **Admins. do domínio**, ou equivalente é o requisito mínimo para executar este procedimento.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Para adicionar usuários ao grupo de segurança sem fio

1. Clique em **iniciar**, clique em **ferramentas administrativas**e clique em **usuários e Active Directory computadores**. Abre o Active Directory MMC Usuários e computadores. Se não ainda estiver selecionada, clique no nó do seu domínio. Por exemplo, se o domínio for example.com, clique em **example.com**.

2. No painel de detalhes, clique na pasta que contém o grupo de segurança sem fio double\.

3. No painel de detalhes, clique right\ o grupo de segurança sem fio e, em seguida, clique em **propriedades**. O **propriedades** abre a caixa de diálogo do grupo de segurança.

4. Sobre o **membros**, clique em **adicionar**e, em seguida, execute um dos procedimentos a seguir para adicionar um computador ou adicionar um usuário ou grupo.

##### <a name="to-add-a-user-or-group"></a>Para adicionar um usuário ou grupo

1. Em **digite os nomes de objeto para selecionar**, digite o nome do usuário ou grupo que você deseja adicionar e clique **Okey**.

2. Para atribuir a associação de grupo para outros usuários ou grupos, repita a etapa 1 deste procedimento.  

##### <a name="to-add-a-computer"></a>Para adicionar um computador

1. Clique em **tipos de objeto**. O **tipos de objeto** caixa de diálogo é aberta.

2. Em **tipos de objeto**, selecione **computadores**e clique em **Okey**.

3. Em **digite os nomes de objeto para selecionar**, digite o nome do computador ao qual você deseja adicionar e, em seguida, clique em **Okey**.

4. Para atribuir a associação de grupo para outros computadores, repita as etapas 3 1 \ deste procedimento.

## <a name="bkmk_policies"></a>Configurar a rede sem fio \ (IEEE 802.11\) políticas

Siga estas etapas para configurar a rede com fio \ (IEEE 802.11\) extensão de diretiva de grupo:

- [Abrir ou adicionar e abrir um objeto de política de grupo](#bkmk_opengpme)

- [Ativar a rede sem fio padrão \ (IEEE 802.11\) políticas](#bkmk_activate)

- [Configurar a nova política de rede sem fio](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Abrir ou adicionar e abrir um objeto de política de grupo

Por padrão, o recurso Gerenciamento de política de grupo é instalado em computadores que executam o Windows Server 2016 quando a função de servidor Serviços de domínio do Active Directory \(AD DS\) é instalada e o servidor está configurado como um controlador de domínio. O procedimento a seguir que descreve como abrir o \(GPMC\) Console de gerenciamento de política de grupo no controlador de domínio. O procedimento descreve como para abrir uma política de grupo existente do nível de domain\ objeto \(GPO\) para edição, ou criar um novo GPO de domínio e abra-o para edição.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir ou adicionar e abrir um objeto de política de grupo

1. No controlador de domínio, clique em **iniciar**, clique em **ferramentas administrativas do Windows**e clique em **Group Policy Management**. Abre o Console de gerenciamento de política de grupo.  

2. No painel esquerdo, clique em double\ floresta. Por exemplo, clique em double\ **floresta: example.com**.  

3. No painel esquerdo, clique em double\ **domínios**e double\ clique no domínio para que você deseja gerenciar um objeto de política de grupo. Por exemplo, clique em double\ **example.com**.  

4. Siga um destes procedimentos:

    -   **Para abrir um GPO de nível de domain\ existente para edição**, clique duas vezes no domínio que contém o objeto de política de grupo que você deseja gerenciar, clique right\ a política de domínio que você deseja gerenciar, como a política de domínio padrão e clique em **editar**. **Editor de gerenciamento de política de grupo** é aberta.

    -   **Criar um novo objeto de política de grupo e abrir para edição**, right\-click no domínio para o qual você deseja criar um novo objeto de política de grupo e, em seguida, clique em **criar um GPO neste domínio e vinculá-lo aqui**.

        Em **novo GPO**, na **nome**, digite um nome para o novo objeto de política de grupo e, em seguida, clique em **Okey**.

        Clique Right\ seu novo objeto de política de grupo e clique em **editar**. **Editor de gerenciamento de política de grupo** é aberta.

Na próxima seção, você usará um Editor de gerenciamento de política de grupo para criar a política sem fio.

### <a name="bkmk_activate"></a>Ativar a rede sem fio padrão \ (IEEE 802.11\) políticas

Este procedimento descreve como ativar o padrão de rede com fio \ (IEEE 802.11\) políticas usando o Editor de gerenciamento de política de grupo \(GPME\).

>[!NOTE]
>Depois de ativar o **Windows Vista e versões posteriores** versão da rede sem fio \ (IEEE 802.11\) políticas ou o **Windows XP** versão, a opção de versão é automaticamente removida da lista de opções ao right\ click **rede com fio \ (IEEE 802.11\) políticas**. Isso ocorre porque depois de selecionar uma versão de política, a política é adicionada no painel de detalhes do GPME quando você seleciona o **rede com fio \ (IEEE 802.11\) políticas** nó. Este estado permanece a menos que você exclua a política sem fio, no momento em que a versão de política sem fio retorna ao menu right\-click para **rede sem fio \ (IEEE 802.11\) políticas** no GPME. Além disso, as políticas sem fio só serão listadas no painel de detalhes GPME quando o **rede sem fio \ (IEEE 802.11\) políticas** nó é selecionado.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Para ativar o padrão de rede com fio \ (IEEE 802.11\) políticas  

1. Siga o procedimento anterior, **abrir ou adicionar e abrir um objeto de política de grupo** para abrir o GPME.

2. No GPME, no painel esquerdo, clique double\ **configuração do computador**, double\-click **políticas**, double\-click **configurações do Windows**e, em seguida, clique em double\ **configurações de segurança**.

![Política de grupo de acesso sem fio 802.1 X](../../../media/Wireless-GP/Wireless-GP.jpg)

3. Em **configurações de segurança**, clique right\ **rede com fio \ (IEEE 802.11\) políticas**e clique em **criar uma nova política de acesso sem fio para o Windows Vista e versões posteriores**. 

![802.1 X diretiva sem fio](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. O **novas propriedades de política de rede sem fio** caixa de diálogo é aberta. Em **nome da política**, digite um novo nome para a política ou mantenha o nome padrão. Clique em **Okey** para salvar a política. A política padrão é ativada e listada no painel de detalhes do GPME com o novo nome que você forneceu ou com o nome padrão **nova diretiva de rede sem fio**.

![Novas propriedades de política de rede sem fio](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. No painel de detalhes, clique em double\ **nova diretiva de rede sem fio** para abri-lo.

Na próxima seção, você pode executar configuração de política, ordem de preferência de processamento de política e as permissões de rede.

### <a name="bkmk_policyconfig"></a>Configurar a nova política de rede sem fio

Você pode usar os procedimentos desta seção para configurar a rede com fio \ (IEEE 802.11\) política. Esta configuração de política permite que você definir as configurações de autenticação e segurança, gerenciar perfis de sem fio e especifique as permissões para as redes sem fio que não estão configuradas como redes preferenciais.

- [Configurar um perfil de Conexão sem fio para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Definir a ordem de preferência de perfis de Conexão sem fio](#bkmk_preferenceorder)  

- [Definir permissões de rede](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurar um perfil de Conexão sem fio para PEAP\-MS\-CHAP v2

Esse procedimento fornece as etapas necessárias para configurar um perfil PEAP\-MS\-CHAP v2 sem fio.  

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Para configurar um perfil de conexão sem fio para PEAP\-MS\-CHAP v2

1. No GPME, na caixa de diálogo de propriedades de rede sem fio, para a política que você acabou de criar, no **geral** guia e em **descrição**, digite uma breve descrição para a política.

2. Para especificar que a configuração automática de WLAN é usada para definir as configurações de adaptador de rede sem fio, certifique-se de que **serviço de configuração automática de WLAN de Windows de uso para clientes** é selecionado.  

3. Em **conectar a redes disponíveis na ordem dos perfis listados abaixo**, clique em **adicionar**e, em seguida, selecione **infraestrutura**. O **propriedades do novo perfil** caixa de diálogo é aberta.

4. No**propriedades do novo perfil** caixa de diálogo, pela **Conexão** guia, o **nome do perfil**, digite um novo nome para o perfil. Por exemplo, digite **Example.com perfil de WLAN para o Windows 10**.

5. Em **rede Name\(s\) \(SSID\)**, digite o SSID que corresponde ao SSID configurado em seus pontos de acesso sem fio e, em seguida, clique em **adicionar**.

    Se sua implantação usa vários SSIDs e cada ponto de acesso sem fio usa as mesmas configurações de segurança sem fio, repita esta etapa para adicionar o SSID para cada ponto de acesso sem fio ao qual você deseja que esse perfil seja aplicado.

    Se sua implantação usa vários SSIDs e as configurações de segurança para cada SSID não corresponderem, configure um perfil separado para cada grupo de SSIDs que usam as mesmas configurações de segurança. Por exemplo, se você tiver um grupo de pontos de acesso sem fio configurado para usar WPA2\-Enterprise e AES e outro grupo dos pontos de acesso sem fio para usar WPA\-Enterprise e TKIP, configure um perfil para cada grupo de pontos de acesso sem fio.

6. Se o texto padrão **NEWSSID** é apresentar, selecioná-lo e, em seguida, clique em **remover**.

7. Se você implantou pontos de acesso sem fio que estão configurados para suprimir transmissões de beacon, selecione **se conectar mesmo se a rede não esteja transmitindo**.

    > [!NOTE]
    > Habilitar essa opção pode criar um risco à segurança, pois os clientes sem fio serão investigar e tentar conexões a qualquer rede sem fio. Por padrão, essa configuração não está habilitada.  

8. Clique no **segurança**, clique em **avançado**e, em seguida, configure o seguinte:

    1. Para definir configurações avançadas de 802.1 X, em **IEEE 802.1 X**, selecione **impor avançadas 802.1 configurações X**.

        Quando o avançadas de 802.1 X configurações são impostas, os valores padrão para **Max Eapol\-Start mensagens**, **período de retenção**, **iniciar período**, e **período de autenticação** são suficientes para implantações sem fio. Por isso, você não precisa alterar os padrões, a menos que você tenha um motivo específico para fazê-lo.

    2. Para habilitar o logon único, selecione **habilitar o logon único para essa rede**.

    3. Os demais valores padrão em **Sign-On único** são suficientes para implantações sem fio.

    4. Em **Mobilidade Rápida**, se o seu ponto de acesso sem fio está configurado para pre\-autenticação, selecione **esta rede usa autenticação pre\**.

9. Para especificar que as comunicações sem fio atender aos padrões de 140\-2 de FIPS, selecione **executar criptografia em modo de certificado FIPS 140\ 2**.

10. Clique em **Okey** para retornar para o **segurança** guia. Em **selecionar os métodos de segurança para essa rede**, na **autenticação**, selecione **WPA2\ Enterprise** se ele é compatível com seu ponto de acesso sem fio e adaptadores de rede sem fio do cliente. Caso contrário, selecione **WPA\ Enterprise**.

11. Em **criptografia**, se dá suporte ao seu ponto de acesso sem fio e adaptadores de rede sem fio do cliente, selecionados **AES-CCMP**. Se você estiver usando pontos de acesso e adaptadores de rede sem fio que oferecem suporte 802.11ac, selecione **AES-GCMP**. Caso contrário, selecione **TKIP**.

    > [!NOTE]  
    > As configurações de ambos **autenticação** e **criptografia** deve corresponder às configurações definidas em seus pontos de acesso sem fio. As configurações padrão de **modo de autenticação**, **máximo de falhas de autenticação**, e **armazena em Cache informações de usuário para conexões futuras a essa rede** são suficientes para implantações sem fio.  

12. Em **selecione um método de autenticação de rede**, selecione **EAP protegido \(PEAP\)**e clique em **propriedades**. O **Propriedades EAP protegidas** caixa de diálogo é aberta.

13. Em **Propriedades EAP protegidas**, confirme que **verificar a identidade do servidor ao validar o certificado** é selecionado.

14. Em **autoridades de certificação raiz confiáveis**, selecione a autoridade de certificação raiz confiável \(CA\) que emitiu o servidor de certificados para o servidor NPS.

    > [!NOTE]  
    > Essa configuração limita a raiz autoridades de certificação que os clientes confiam para as CAs selecionado. Se nenhuma autoridades de certificação raiz confiável é selecionado, em seguida, os clientes confiarão que todos raiz CAs listados em seu repositório de certificados de autoridades de certificação raiz confiáveis.  

15. No **Selecionar método de autenticação**, selecione **senha segura \ (EAP\-MS\-CHAP v2\)**.

16. Clique em **configurar**. No **propriedades de EAP MSCHAPv2** caixa de diálogo, verifique se **usar automaticamente meu nome de logon do Windows e a senha \ (e no domínio se any\)** está selecionado e clique em **Okey**.

17. Para habilitar PEAP reconexão rápida, certifique-se de que **Ativar reconexão rápida** é selecionado.

18. Para solicitar o servidor TLV em tentativas de conexão, selecione **desconectar se o servidor não tiver TLV com ligação**.

19. Para especificar que a identidade do usuário é mascarada na fase 1 de autenticação, selecione **habilitar privacidade de identidade**e na caixa de texto, digite um nome de identidade anônima ou deixar a caixa de texto em branco.

    >[! NOTAS]
    >- A política NPS para 802.1 X acesso sem fio deve ser criada usando NPS **diretiva de solicitação de Conexão **. Se a política NPS é criada usando o NPS **política de rede**, e a privacidade de identidade não funcionará.
    >- Privacidade de identidade EAP é fornecida pelo determinados métodos EAP onde uma vazia ou uma identidade anônima \ (diferente do identity\ real) é enviada em resposta à solicitação de identidade EAP. PEAP envia a identidade duas vezes durante a autenticação. Na primeira fase, a identidade é enviada em texto sem formatação e essa identidade é usada para fins de roteamento, não para autenticação de cliente. A identidade real — usado para autenticação — é enviado durante a segunda fase da autenticação, dentro do túnel seguro que é estabelecido na primeira fase. Se **habilitar privacidade de identidade** caixa de seleção estiver marcada, o nome de usuário é substituído com a entrada especificada na caixa de texto. Por exemplo, suponha que **habilitar privacidade de identidade** é selecionado e o alias de privacidade de identidade **anônimo** é especificado na caixa de texto. Para um usuário com um alias de identidade real **jdoe@example.com**, a identidade enviada na primeira fase da autenticação será alterada para **anonymous@example.com**. A parte da identidade de fase 1º território não é modificada conforme ele é usado para fins de roteamento.  

20. Clique em **Okey** para fechar o **Propriedades EAP protegidas** caixa de diálogo.
21. Clique em **Okey** para fechar o **segurança** guia.
22. Se você quiser criar perfis adicionais, clique em **adicionar**e, em seguida, repita as etapas anteriores, tornando opções diferentes para personalizar cada perfil para os clientes sem fio e a rede à qual deseja que o perfil aplicado. Quando você tiver concluído a adição de perfis, clique em **Okey** para fechar a caixa de diálogo Propriedades de política de rede sem fio.

Na próxima seção, você pode solicitar os perfis de política de segurança ideal.

#### <a name="bkmk_preferenceorder"></a>Definir a ordem de preferência de perfis de Conexão sem fio
Você pode usar esse procedimento se você criou vários perfis sem fio em sua política de rede sem fio e você deseja solicitar os perfis de ideal eficiência e segurança.

Para garantir que os clientes sem fio se conectar com o nível mais alto de segurança que eles podem oferecer suporte, coloque as políticas mais restritivas na parte superior da lista.

Por exemplo, se você tiver dois perfis, uma para clientes que dão suporte a WPA2 e outra para os clientes que dão suporte a WPA, coloque o perfil de WPA2 superior na lista. Isso garante que os clientes que dão suporte a WPA2 usará esse método para a conexão, em vez do WPA menos seguro.

Esse procedimento fornece as etapas para especificar a ordem em que os perfis de conexão sem fio são usados para conectar clientes sem fio de membros do domínio a redes sem fio.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Para definir a ordem de preferência para perfis de conexão sem fio

1. No GPME, na caixa de diálogo de propriedades de rede sem fio, para a política que você acabou de configurar, clique no **geral** guia.

2. Sobre o **geral** guia, **conectar a redes disponíveis na ordem dos perfis listados abaixo**, selecione o perfil que você deseja mover na lista e clique em ambos os "seta para cima" botão ou "seta para baixo" botão para mover o perfil para o local desejado na lista.

3.  Repita a etapa 2 para cada perfil que você deseja mover na lista.  

4.  Clique em **Okey** para salvar todas as alterações.

Na seção a seguir, você pode definir as permissões de rede para a diretiva sem fio.

#### <a name="bkmk_permissions"></a>Definir permissões de rede
Você pode definir configurações sobre o **permissões de rede** guia para os membros do domínio ao qual rede com fio \ (IEEE 802.11\) políticas se aplicam.

Só é possível aplicar as seguintes configurações em redes sem fio que não estão configuradas no **geral** guia a **propriedades de política de rede sem fio** página:

- Permitir ou negar conexões com redes sem fio específicas que você especificar por tipo de rede e o identificador do conjunto de serviço \(SSID\)

- Permitir ou negar conexões com redes ad-hoc

- Permitir ou negar conexões com redes de infraestrutura

- Permitir ou negar os usuários exibam tipos de rede \ (ad hoc ou infrastructure\) para que eles têm acesso negados

- Permitir ou negar aos usuários para criar um perfil que se aplica a todos os usuários

- Os usuários só podem se conectar a redes permitidos usando perfis de política de grupo

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir esses procedimentos.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Para permitir ou negar conexões com redes sem fio específicas

1. No GPME, na caixa de diálogo Propriedades de rede sem fio, clique no **permissões de rede** guia.

2. Sobre o **permissões de rede**, clique em **adicionar **. O **nova entrada de permissões** caixa de diálogo é aberta.

3. No **nova entrada de permissão** na caixa o **nome da rede \(SSID\)** campo, digite a rede SSID da rede para o qual você deseja definir permissões.

4.  Em **tipo de rede**, selecione **infraestrutura** ou **Ad-hoc **.

    > [!NOTE]  
    > Se você não tiver certeza se a rede de transmissão é uma infraestrutura ou a rede ad hoc, você pode configurar duas entradas de permissão de rede, um para cada tipo de rede.

5. Em **permissão**, selecione **permitir** ou **negar **.

6. Clique em **Okey**, para retornar para o **permissões de rede** guia.

##### <a name="to-specify-additional-network-permissions-optional"></a>Para especificar a rede adicionais permissões \(Optional\)

1.  Sobre o **permissões de rede** guia, configurar alguma ou todas as seguintes:  

    -   Para os membros do domínio negar acesso a redes ad-hoc, selecione **impedir conexões com redes ad\-hoc **.

    -   Para os membros do domínio negar acesso a redes de infraestrutura, selecione **impedir conexões com redes de infraestrutura **.  

    -   Para permitir que os membros do domínio exibir os tipos de rede \ (ad hoc ou infrastructure\) para que eles têm acesso negados, selecione **permitir que o usuário exiba as redes negadas **.

    -   Para permitir que os usuários criem perfis que se aplicam a todos os usuários, selecione **permitir que todos criem todos os perfis de usuário **.

    -   Para especificar que os usuários só podem se conectar a redes permitidas usando perfis de política de grupo, selecione **só usar perfis de política de grupo para redes permitidas **.

## <a name="bkmk_nps"></a>Configure os servidores NPS
Siga estas etapas para configurar os servidores NPS para realizar autenticação 802.1 X para acesso sem fio:

- [Registrar NPS nos serviços de domínio do Active Directory](#bkmk_npsreg)

- [Configurar um ponto de acesso sem fio como um cliente RADIUS de NPS](#bkmk_radiusclient)

- [Criar políticas de NPS para 802.1 X acesso sem fio usando um assistente](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrar NPS nos serviços de domínio do Active Directory
Você pode usar este procedimento para registrar um servidor que executa o servidor de política de rede \(NPS\) no Active Directory Domain Services \(AD DS\) no domínio em que o servidor NPS é um membro. Para servidores NPS ter permissão para ler as propriedades de dial\ de contas de usuário durante o processo de autorização, cada servidor NPS deve ser registrado no AD DS. Registrar um servidor NPS adiciona o servidor para o **servidores RAS e IAS** grupo de segurança no AD DS.

>[!NOTE]
>Você pode instalar NPS em um controlador de domínio ou em um servidor dedicado. Execute o seguinte comando do Windows PowerShell para instalar o NPS se você ainda não tiver feito isso:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

#### <a name="to-register-an-nps-server-in-its-default-domain"></a>Para registrar um servidor NPS no domínio padrão

1. No servidor NPS, em **Gerenciador do servidor**, clique em **ferramentas**e clique em **NPS **. Abre o NPS snap\-in.

2. Clique Right\ **NPS \(Local\)**e clique em **registrar servidor no Active Directory **. O **NPS** caixa de diálogo é aberta.

3. Em **NPS**, clique em **Okey**e clique em **Okey** novamente.

### <a name="bkmk_radiusclient"></a>Configurar um ponto de acesso sem fio como um cliente RADIUS de NPS
Você pode usar este procedimento para configurar um ponto de acesso, também conhecido como um *o servidor de acesso de rede \(NAS\)*, como um cliente remoto Authentication Dial\-In User Service \(RADIUS\) usando o NPS snap\-in. 

>[!IMPORTANT]
>Computadores cliente, como computadores portáteis sem fio e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, 802.1X\-comutadores compatíveis com, rede virtual privada \(VPN\) servidores e servidores dial\-up — porque eles usam o protocolo RADIUS para se comunicar com servidores RADIUS, como servidores NPS.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para adicionar um servidor de acesso de rede como um cliente RADIUS de NPS

1. No servidor NPS, em **Gerenciador do servidor**, clique em **ferramentas**e clique em **NPS **. Abre o NPS snap\-in.

2. No NPS snap\-in, clique em double\ **clientes e servidores RADIUS **. Clique em Right\ **clientes RADIUS**e clique em **nova **.

3. Em **novo cliente RADIUS**, verifique o **habilitar este cliente RADIUS** caixa de seleção está marcada.

4. Em **novo cliente RADIUS**, na **nome amigável**, digite um nome de exibição para o ponto de acesso sem fio.

    Por exemplo, se você quiser adicionar um acesso sem fio aponte \(AP\) chamado AP\-01, digite **AP\-01**.

5. Em **endereço \(IP or DNS\)**, digite o endereço IP ou totalmente qualificado \(FQDN\) de nome de domínio para NAS.

    Se você inserir o FQDN, para verificar se o nome está correto e é mapeado para um endereço IP válido, clique em **verificar**e, em seguida, em **Verificar endereço**, além do **endereço** campo, clique em **resolver**. Se o nome FQDN é mapeado para um endereço IP válido, o endereço IP desse NAS irá aparecer automaticamente na **endereço IP**. Se o FQDN não é resolvida em um endereço IP que você receberá uma mensagem indicando que não há tal host é conhecido. Se isso ocorrer, verifique se que você tenha o nome de ponto de acesso correto e que o ponto de acesso está ligado e conectado à rede.  

    Clique em **Okey** fechar **Verificar endereço**.  

6. Em **novo cliente RADIUS**, na **segredo compartilhado**, siga um destes procedimentos:  

    -   Para configurar manualmente um segredo compartilhado RADIUS, selecione **Manual**e, em seguida, em **segredo compartilhado**, digite a senha forte que também é inserida no NAS. Digite novamente o segredo compartilhado no **confirmar o segredo compartilhado**.  

    -   Para gerar automaticamente um segredo compartilhado, selecione o **gerar** caixa de seleção e, em seguida, clique no **gerar** botão. Salve o segredo compartilhado gerado e, em seguida, usar esse valor para configurar o para que ele possa se comunicar com o servidor NPS.  

        >[!IMPORTANT]
        >O segredo compartilhado RADIUS que você digita para o seu PA virtual no NPS deve corresponder exatamente o segredo compartilhado RADIUS configurado no seu real PA sem fio Se você usar a opção de NPS para gerar um segredo compartilhado RADIUS, configure o ponto de acesso sem fio real correspondente com o segredo compartilhado RADIUS que tenha sido gerado pelo NPS.

7. Em **novo cliente RADIUS**diante o **avançado** guia, **nome do fornecedor**, especifique o nome do fabricante NAS. Se você não tiver certeza do nome do fabricante NAS, selecione **padrão RADIUS**.

8. Em **opções adicionais**, se você estiver usando qualquer método de autenticação diferentes de EAP e PEAP e se o seu NAS dá suporte ao uso do atributo autenticador mensagem, selecione **mensagens de solicitação de acesso devem conter o atributo autenticador Message\**.

9. Clique em **Okey**. Seu NAS aparece na lista de clientes RADIUS configurados no servidor NPS.

### <a name="bkmk_npspolicy"></a>Criar políticas NPS para 802.1 X acesso sem fio usando um assistente
Você pode usar este procedimento para criar a conexão políticas de solicitação e de rede políticas necessárias para implantar qualquer 802.1X\-pontos de acesso sem fio compatível com como clientes Remote Authentication Dial\-In User Service \(RADIUS\) ao servidor RADIUS executando \(NPS\) servidor de política de rede.  
Depois de executar o assistente, as políticas a seguir são criadas:

- Política de solicitação de uma conexão

- Uma diretiva de rede

>[!NOTE]
>Você pode executar o Assistente de nova IEEE 802.1 X com fio e seguras conexões sem fio sempre que você precisa criar novas políticas de acesso autenticado 802.1 X.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Criar políticas para 802.1 X autenticado sem fio usando um assistente

1. Abra o NPS snap\-in. Se ainda não estiver selecionada, clique em **NPS \(Local\)**. Se você estiver executando o NPS MMC snap\-in e deseja criar políticas em um servidor NPS remoto, selecione o servidor.

2. Em **Introdução**, na **configuração padrão**, selecione **servidor RADIUS para 802.1 X acesso sem fio ou conexões de rede com fio**. O texto e os links abaixo do texto mudam para refletir sua seleção.

3. Clique em **configurar 802.1 X**. Abre o Assistente para configurar 802.1 X.

4.  No **selecione tipo de conexões 802.1 X** assistente página **tipo de 802.1 X conexões**, selecione **conexões sem fio seguras**e no **nome**, digite um nome para sua política ou deixar o nome padrão **conexões sem fio seguras**. Clique em **próxima**.

5.  No **especificar 802.1 X opções** assistente página **clientes RADIUS**, todos os 802.1 X comutadores e pontos de acesso sem fio que você adicionou conforme clientes RADIUS de NPS snap\-in. Siga um destes procedimentos:

    -   Adicionar rede adicionais acesso servidores \(NASs\), como pontos de acesso sem fio, em **clientes RADIUS**, clique em **adicionar**e, em seguida, em **cliente RADIUS novo**, insira as informações da: **nome amigável**, **endereço \(IP or DNS\)**, e **segredo compartilhado**.

    -   Para modificar as configurações para qualquer NAS, em **clientes RADIUS**, selecione o ponto de acesso para o qual você deseja modificar as configurações e, em seguida, clique em **editar**. Modifique as configurações conforme necessário.

    -   Para remover um da lista, em **clientes RADIUS**, selecione NAS e, em seguida, clique em **remover**.

        >[!WARNING]
        >Removendo um cliente RADIUS de dentro do **configurar 802.1 X** assistente exclui o cliente de configuração do servidor NPS. Todas as adições, modificações e exclusões que você faz dentro do **configurar 802.1 X** Assistente para os clientes RADIUS são refletidas no NPS snap\-in, no **clientes RADIUS** nó sob **NPS** \/ **clientes e servidores RADIUS**. Por exemplo, se você usar o Assistente para remover uma solução 802.1 X alternar, a opção também é removida do NPS snap\-in.

6. Clique em **próxima**. No **configurar um método de autenticação** assistente página **tipo \ (com base no método de configuração de rede e acesso)**, selecione **Microsoft: EAP protegido \(PEAP\)**e clique em **configurar**.

    >[!TIP]
    >Se você receber uma mensagem de erro indicando que não é possível encontrar um certificado para uso com o método de autenticação, e você tiver configurado o serviços de certificados do Active Directory para emitir certificados automaticamente para servidores RAS e IAS em sua rede, primeiro verifique se você tiver seguido as etapas para registrar NPS nos serviços de domínio do Active Directory e use as etapas a seguir para atualizar a política de grupo : Clique **iniciar**, clique em **sistema Windows**, clique em **executar**e em **abrir**, tipo **gpupdate**, e pressione ENTER. Quando o comando retorna resultados indicando que o usuário e o computador de política de grupo foram atualizados com sucesso, selecione **Microsoft: EAP protegido \(PEAP\)** novamente e clique em **configurar**.
    >
    >Se após a atualização de política de grupo, você continuar a receber a mensagem de erro indicando que não foi encontrado um certificado para uso com o método de autenticação, o certificado não estiver sendo exibido porque ele não atender aos requisitos de certificado do servidor mínimo como documentado na guia complementar da rede principal: [implantar certificados de servidor para 802.1 X com e sem fio implantações](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Se isso acontecer, descontinuar configuração NPS, revogar o certificado emitido para seu server\(s\) NPS e siga as instruções para configurar um novo certificado usando o guia de implantação de certificados do servidor.

7.  No **editar propriedades de EAP protegidas** assistente página **certificado emitido**, certifique-se de que o certificado do servidor NPS correto está selecionado e, em seguida, faça o seguinte:

    >[!NOTE]
    >Verifique o valor em **emissor** está correto para o certificado selecionado em **certificado emitido**. Por exemplo, é o emissor esperado para um certificado emitido por uma autoridade de certificação que executam serviços de certificados do Active Directory \(AD CS\) chamado corp\DC1 no domínio contoso.com, **corp\-DC1\-CA**.

    -   Para permitir que os usuários se movem com seus computadores sem fio entre pontos de acesso sem que eles precisem ser autenticado novamente cada vez que eles associam um novo ponto de acesso, selecione **Ativar reconexão rápida**.

    -   Para especificar que conectando clientes sem fio serão encerradas o processo de autenticação de rede se o servidor RADIUS não apresenta com ligação \(TLV\) Type\-Length\-valor, selecione **desconectar clientes sem com ligação**.  

    -   Para modificar a política de configurações para o EAP digitar, em **tipos EAP**, clique em **editar**, na **propriedades de EAP MSCHAPv2**, modifique as configurações conforme necessário e, em seguida, clique em **Okey**.  

8.  Clique em **Okey**. Fecha a caixa de diálogo Editar propriedades de EAP protegidas, retornando para a **configurar 802.1 X** assistente. Clique em **próxima**.

9. Em **especificar grupos de usuários**, clique em **adicionar**e, em seguida, digite o nome do grupo de segurança que você configurou para seus clientes sem fio no Active Directory usuários e computadores snap\-in. Por exemplo, se você nomeou seu grupo de segurança sem fio grupo sem fio, digite **grupo sem fio**. Clique em **próxima**.

10. Clique em **configurar** configurar atributos RADIUS padrão e atributos específicos vendor\ para virtual \(VLAN\) LAN conforme necessário e conforme especificado pela documentação fornecida pelo seu fornecedor de hardware do ponto de acesso sem fio. Clique em **próxima**.

11. Examine os detalhes de resumo de configuração e, em seguida, clique em **concluir**.

As políticas NPS agora são criadas, e você pode passar para ingressar no domínio em computadores sem fio.

## <a name="bkmk_domain"></a>Adicione novos computadores sem fio ao domínio
O método mais fácil para adicionar novos computadores sem fio ao domínio é anexar fisicamente o computador a um segmento da LAN com fio \ (um segmento não controlado por um X switch\ 802.1) antes de ingressar o computador ao domínio. Isso é mais fácil como configurações de política de grupo sem fio são aplicadas de imediatamente e automaticamente e, se tiver implantado o seu próprio PKI, o computador recebe o certificado da CA e coloca-o no repositório de certificados de autoridades de certificação confiáveis, permitindo que o cliente sem fio confiar em servidores NPS com certificados de servidor emitidos pela autoridade de certificação.

Da mesma forma, depois que um novo computador sem fio é ingressar no domínio, o método preferencial para os usuários façam logon no domínio é executar log em usando uma conexão com fio à rede.

### <a name="other-domain-join-methods"></a>Outros métodos de ingresso no domínio
Em casos onde não é prático adicionar computadores ao domínio usando uma conexão Ethernet com fio ou nos casos em que o usuário não pode fazer logon logon no domínio pela primeira vez usando uma conexão com fio, você deve usar um método alternativo.

- **Configuração de computador de equipe de TI**. Um membro da equipe de TI ingressa em um computador sem fio no domínio e configura um logon único bootstrap perfil sem fio. Com esse método, o administrador de TI conecta o computador sem fio à rede Ethernet com fio e o computador ingressa no domínio. Em seguida, o administrador distribui o computador para o usuário. Quando o usuário seja iniciado o computador sem o uso de uma conexão com fio, as credenciais de domínio que especificar manualmente para o logon do usuário são usadas para estabelece uma conexão à rede sem fio e para fazer logon no domínio.

Para obter mais informações, consulte a seção [participe de domínio e fazer logon usando o método de configuração do computador de equipe de TI](#bkmk_itstaff)

-   **Inicializar a configuração de perfil sem fio por usuários**. O usuário configura o computador sem fio com um perfil sem fio bootstrap e ingressa no domínio, com base nas instruções adquiridas de um administrador de TI manualmente. O perfil sem fio bootstrap permite ao usuário estabelecer uma conexão sem fio e, em seguida, ingressar no domínio. Depois de ingressar o computador ao domínio e reiniciar o computador, o usuário pode fazer logon no domínio usando uma conexão sem fio e suas credenciais de conta de domínio.

Para obter mais informações, consulte a seção [participe de domínio e logon por meio da configuração de perfil de acesso sem fio Bootstrap por usuários](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Participe de domínio e fazer logon usando o método de configuração do computador de equipe de TI
Usuários de membro do domínio com computadores cliente sem fio domain\ associado podem usar um perfil temporário sem fio para se conectar a um 802.1X\-autenticados rede sem fio sem precisar primeiro se conectar à rede local com fio. Esse perfil sem fio temporário é chamado uma *inicializar perfil sem fio*.

Um perfil sem fio bootstrap requer que o usuário especificar manualmente suas credenciais de conta de usuário de domínio e não valida o certificado do servidor remoto Authentication Dial\-In User Service \(RADIUS\) executando \(NPS\) servidor de política de rede.

Depois de conectividade sem fio é estabelecida, política de grupo é aplicada no computador cliente sem fio e um novo perfil sem fio é emitido automaticamente. A nova política usa as credenciais de conta de computador e do usuário para autenticação do cliente. 

Além disso, como parte da autenticação mútua de PEAP\-MS\-CHAP v2 usando o novo perfil em vez do perfil de inicialização, o cliente valida as credenciais do servidor RADIUS.

Depois de ingressar o computador ao domínio, use este procedimento para configurar um logon único bootstrap perfil sem fio, antes de distribuir o computador sem fio para o usuário domain\ membro.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Para configurar um logon único sem fio perfil bootstrap

1. Criar um perfil de inicialização usando o procedimento neste guia denominado [configurar um perfil de Conexão sem fio para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)e use as seguintes configurações:

    - Autenticação PEAP\-MS\-CHAP v2

    - Validar disabled de certificado de servidor RADIUS

    - Logon único habilitado

2. Nas propriedades da diretiva de rede sem fio no qual você criou o novo perfil de inicialização, no **geral** guia, selecione o perfil de inicialização e, em seguida, clique em **exportar** para exportar o perfil para um compartilhamento de rede, unidade flash USB, ou outro local de fácil acesso. O perfil é salvo como um arquivo XML para o local que você especificar.

3. Adicionar o novo computador sem fio ao domínio \ (por exemplo, por meio de uma conexão Ethernet que não exija IEEE 802.1 X authentication\) e adicione o perfil bootstrap sem fio ao computador usando o **netsh wlan adicionar perfil** comando.

    >[!NOTE]
    >Para obter mais informações, consulte comandos Netsh para \(WLAN\) LAN sem fio em [http:///\/technet.microsoft.com\/library\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuir o novo computador sem fio para o usuário com o procedimento para "Fazer logon no domínio usando computadores que executam o Windows 10".

Quando o usuário inicia o computador, o Windows solicita ao usuário para inserir seu nome de conta de usuário de domínio e a senha. Como o logon único é habilitado, o computador usa as credenciais de conta de usuário de domínio para primeiro estabelecer uma conexão com a rede sem fio e, em seguida, fazer logon no domínio.

#### <a name="bkmk_w10"></a>Faça logon no domínio usando computadores que executam o Windows 10

1. Faça logoff do computador ou reiniciá-lo.

2. Pressione qualquer tecla no teclado ou clique na área de trabalho. Aparece a tela de logon com um nome de conta de usuário local exibido e um campo de entrada de senha abaixo do nome. Não faça logon com a conta de usuário local.

3. No canto inferior esquerdo da tela, clique em **outro usuário**. O log de outro usuário na tela aparece com dois campos, um para o nome de usuário e uma senha. Abaixo a senha de campo é o texto **entrar em:** e, em seguida, o nome do domínio em que o computador está associado. Por exemplo, se o domínio for denominado example.com, o texto lê **entrar em: exemplo**.

4. Em **nome de usuário**, digite seu nome de usuário de domínio.

5. Em **senha**, digite sua senha de domínio e, em seguida, clique na seta ou pressione ENTER.

>[!NOTE]
>Se o **outro usuário** tela não inclua o texto **entrar em:** e seu nome de domínio, você deve inserir seu nome de usuário no formato *domínio \ \ usuário*. Por exemplo, para fazer logon no domínio example.com com uma conta chamada **User\-01**, tipo **example\\User\-01**.

### <a name="bkmk_userbootstrap"></a>Participe de domínio e logon por meio da configuração de perfil de acesso sem fio Bootstrap pelos usuários
Com esse método, você concluir as etapas na seção etapas gerais e fornecer aos usuários domain\ membro com as instruções sobre como configurar manualmente um computador sem fio com um perfil sem fio bootstrap. O perfil sem fio bootstrap permite ao usuário estabelecer uma conexão sem fio e, em seguida, ingressar no domínio. Depois que o computador está ingressado no domínio e reiniciado, o usuário pode fazer logon no domínio por meio de uma conexão sem fio.

#### <a name="general-steps"></a>Etapas gerais

1. Configurar uma conta de administrador do computador local, no **painel de controle**, para que o usuário.

    >[!IMPORTANT]
    >Para ingressar um computador em um domínio, o usuário deve estar conectado ao computador com a conta de administrador local. Como alternativa, o usuário deve fornecer as credenciais da conta de administrador local durante o processo de ingressar o computador ao domínio. Além disso, o usuário deve ter uma conta de usuário no domínio ao qual o usuário deseja ingressar o computador. Durante o processo de ingressar o computador ao domínio, o usuário será solicitado credenciais de conta de domínio \ (nome de usuário e password\).

2. Fornecer aos usuários de domínio conforme documentado no procedimento a seguir as instruções para configurar um perfil sem fio bootstrap, **para configurar um perfil sem fio bootstrap**.
3. Fornecer aos usuários Além disso, ambas as credenciais do computador local \ (nome de usuário e password\) e as credenciais de domínio \ (nome de conta de usuário de domínio e password\) na forma *nome_do_domínio \ \ nome_do_usuário*, bem como os procedimentos para "Ingressar o computador ao domínio," e "Fazer logon no domínio," como documentado no Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Para configurar um perfil sem fio bootstrap

1. Use as credenciais fornecidas por seu administrador de rede ou o suporte a TI profissional para fazer logon no computador com a conta de administrador do computador local.

2. O ícone rede na área de trabalho e clique em Right\-click **abrir Central de rede e compartilhamento**. **Central de rede e compartilhamento** é aberta. Em **alterar suas configurações de redes**, clique em **configurar uma nova conexão ou rede**. O **definir uma Conexão ou rede** caixa de diálogo é aberta.

3. Clique em **manualmente se conectar a uma rede sem fio**e clique em **próxima**.

4. Em **manualmente se conectar a uma rede sem fio**, na **nome da rede**, digite o nome SSID do ponto de acesso.  

5. Em **tipo de segurança**, selecione a configuração fornecida pelo seu administrador.

6. Em **tipo de criptografia** e **chave de segurança**, selecione ou digite as configurações fornecidas pelo seu administrador.

7. Selecione **iniciar esta conexão automaticamente**e clique em **próxima**.

8. No **adicionado com êxito * * * o SSID de rede*, clique em **alterar as configurações de conexão**.

9. Clique em **alterar as configurações de conexão**. O *sua rede SSID* caixa de diálogo de propriedades de rede com fio é aberta.

10. Clique no **segurança** guia e, em seguida, em **escolher um método de autenticação de rede**, selecione **EAP protegido \(PEAP\)**.

11. Clique em **configurações**. O **EAP protegido \(PEAP\) propriedades** página for aberta.

12. No **EAP protegido \(PEAP\) propriedades** página, certifique-se de que **Validar certificado do servidor** não é selecionado, clique em **Okey** duplicadas e, em seguida, clique em **fechar **.

13. Em seguida, o Windows tenta se conectar à rede sem fio. As configurações do perfil sem fio bootstrap especificam que você deve fornecer suas credenciais de domínio. Quando o Windows solicita que você forneça um nome de conta e senha, digite suas credenciais de conta de domínio da seguinte forma: *nome do domínio nome_de_domínio*, *senha de domínio *.

##### <a name="to-join-a-computer-to-the-domain"></a>Para participar de um computador ao domínio

1. Faça logon no computador com a conta de administrador local.

2. Na caixa de texto de pesquisa, digite **PowerShell **. Nos resultados da pesquisa, clique com botão direito **do Windows PowerShell**e clique em **executar como administrador **. Windows PowerShell é aberta com um prompt elevado.

3. No Windows PowerShell, digite o seguinte comando e pressione ENTER. Certifique-se de que você substitua DomainName variável com o nome do domínio que você deseja ingressar.
    
    Add-Computer DomainName
    
4. Quando solicitado, digite seu nome de usuário de domínio e a senha e clique em **Okey **.
5. Reinicie o computador.
6. Siga as instruções na seção anterior [logon no domínio usando computadores que executam o Windows 10](#bkmk_w10).