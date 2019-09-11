---
title: Instalar e configurar o servidor NPS
description: O processamento de solicitações de conexão do servidor NPS enviadas pelo servidor VPN verifica se o usuário tem permissão para se conectar, a identidade do usuário e registra os aspectos da solicitação de conexão que você escolheu ao configurar a contabilização RADIUS no NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 553f3327e6252d2b03744b2e0fc88f340701f3a9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871326"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Etapa 4. Instalar e configurar o servidor de políticas de rede (NPS)

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Última** Etapa 3. Configurar o servidor de acesso remoto da VPN Always On](vpn-deploy-ras.md)
- [**Última** Etapa 5. Definir as configurações de DNS e de firewall](vpn-deploy-dns-firewall.md)

Nesta etapa, você instalará o NPS (servidor de políticas de rede) para o processamento de solicitações de conexão enviadas pelo servidor VPN:

- Execute a autorização para verificar se o usuário tem permissão para se conectar.
- Executando a autenticação para verificar a identidade do usuário.
- Executar a contabilidade para registrar em log os aspectos da solicitação de conexão que você escolheu quando configurou a contabilidade RADIUS no NPS.

As etapas nesta seção permitem que você conclua os seguintes itens:

1. No computador ou VM que planejou para o servidor NPS e instalado em sua organização ou rede corporativa, você pode instalar o NPS.

   >[!TIP]
   >Se você já tiver um ou mais servidores NPS em sua rede, não será necessário executar a instalação do servidor NPS-em vez disso, você poderá usar este tópico para atualizar a configuração de um servidor NPS existente.

> [!NOTE]
> Você não pode instalar o serviço servidor de políticas de rede no Windows Server Core.

2. Na organização/servidor NPS corporativo, você pode configurar o NPS para executar como um servidor RADIUS que processa as solicitações de conexão recebidas do servidor VPN.

## <a name="install-network-policy-server"></a>Instalar o NPS (Servidor de Políticas de Rede)

Neste procedimento, você instala o NPS usando o Windows PowerShell ou o assistente Gerenciador do Servidor adicionar funções e recursos. O NPS é um serviço de função da função de servidor Serviços de Acesso e Política de Rede.

>[!TIP]
>Por padrão, o NPS ouve o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Quando você instala o NPS e habilita o Firewall do Windows com segurança avançada, as exceções de firewall para essas portas são criadas automaticamente para o tráfego IPv4 e IPv6. Se os servidores de acesso à rede estiverem configurados para enviar tráfego RADIUS por portas diferentes desses padrões, remova as exceções criadas no firewall do Windows com segurança avançada durante a instalação do NPS e crie exceções para as portas que você usa para Tráfego RADIUS.

**Procedimento para o Windows PowerShell:**

Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, insira o seguinte cmdlet:

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**Procedimento para Gerenciador do Servidor:**

1.  Em Gerenciador do Servidor, selecione **gerenciar**e selecione **adicionar funções e recursos**.
        O Assistente para Adicionar Funções e Recursos é aberto.

2.  Em antes de começar, selecione **Avançar**.

    >[!NOTE] 
    >A página **antes de começar** do assistente para adicionar funções e recursos não será exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando o assistente para adicionar funções e recursos for executado.

3.  Em Selecionar tipo de instalação, verifique se a instalação baseada em **função ou em recurso** está selecionada e selecione **Avançar**.

4.  Em selecionar servidor de destino, verifique se **selecionar um servidor do pool de servidores** está selecionado.

5.  Em pool de servidores, verifique se o computador local está selecionado e selecione **Avançar**.

6.  Em selecionar funções de servidor, em **funções**, selecione **serviços de acesso e política de rede**. Uma caixa de diálogo será aberta perguntando se deve adicionar recursos necessários para serviços de acesso e política de rede.

7.  Selecione **Adicionar recursos**e, em seguida, selecione **Avançar**

8.  Em selecionar recursos, selecione **Avançar**e, em serviços de acesso e política de rede, examine as informações fornecidas e selecione **Avançar**.

9.  Em selecionar serviços de função, selecione **servidor de políticas de rede**.

10. Para obter os recursos necessários para o servidor de políticas de rede, selecione **Adicionar recursos**e, em seguida, selecione **Avançar**.

11. Em confirmar seleções de instalação, selecione **reiniciar o servidor de destino automaticamente, se necessário**.

12. Selecione **Sim** para confirmar a seleção e, em seguida, selecione **instalar**.
    
    A página progresso da instalação exibe o status durante o processo de instalação. Quando o processo for concluído, a mensagem "a instalação foi bem-sucedida em *ComputerName*" será exibida, em que *ComputerName* é o nome do computador no qual você instalou o servidor de políticas de rede.

13. Selecione **Fechar**.

## <a name="configure-nps"></a>Configurar o NPS

Depois de instalar o NPS, você configura o NPS para lidar com todas as tarefas de autenticação, autorização e contabilidade da solicitação de conexão que recebe do servidor VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Registrar o servidor NPS no Active Directory

Neste procedimento, você registra o servidor no Active Directory para que ele tenha permissão para acessar as informações da conta de usuário durante o processamento de solicitações de conexão.

**Procedure**

1.  Em Gerenciador do Servidor, selecione **ferramentas**e, em seguida, selecione **servidor de políticas de rede**. O console do NPS é aberto.

2.  No console do NPS, clique com o botão direito do mouse em **NPS (local)** e selecione **registrar servidor em Active Directory**.
   
     A caixa de diálogo servidor de diretivas de rede é aberta.

3.  Na caixa de diálogo servidor de diretivas de rede, selecione **OK** duas vezes.

Para métodos alternativos de registro do NPS, consulte [registrar um servidor NPS em um domínio do Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurar a contabilização do Servidor de Políticas de Rede

Neste procedimento, configure a contabilização do servidor de políticas de rede usando um dos seguintes tipos de log:

- **Log de eventos**. Usado principalmente para auditoria e solução de problemas de tentativas de conexão. Você pode configurar o log de eventos do NPS obtendo as propriedades do servidor NPS no console do NPS.

- **Registrar em log a autenticação de usuário e as solicitações de contabilização em um arquivo local**. Usado principalmente para fins de cobrança e análise de conexão. Também é usado como uma ferramenta de investigação de segurança, pois fornece um método de acompanhar a atividade de um usuário mal-intencionado após um ataque. Você pode configurar o registro em log de arquivo local usando o assistente de configuração de contabilidade.

- **Registrar em log a autenticação de usuário e solicitações de contabilização em um Microsoft SQL Server banco de dados em conformidade com XML**. Usado para permitir que vários servidores executando o NPS tenham uma fonte de dados. Também fornece as vantagens de usar um banco de dados relacional. Você pode configurar o log de SQL Server usando o assistente de configuração de contabilidade.

Para configurar a contabilização do servidor de políticas de rede, consulte [Configurar a contabilidade do servidor de políticas de rede](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Adicionar o servidor VPN como um cliente RADIUS

Na seção [Configurar o servidor de acesso remoto para Always on VPN](vpn-deploy-ras.md) , você instalou e configurou o servidor VPN. Durante a configuração do servidor VPN, você adicionou um segredo compartilhado RADIUS no servidor VPN.

Neste procedimento, você usa a mesma cadeia de texto de segredo compartilhado para configurar o servidor VPN como um cliente RADIUS no NPS. Use a mesma cadeia de texto que você usou no servidor VPN ou a comunicação entre o servidor NPS e o servidor VPN falha.

>[!IMPORTANT] 
>Ao adicionar um novo servidor de acesso à rede (servidor VPN, ponto de acesso sem fio, comutador de autenticação ou servidor de conexão discada) à sua rede, você deve adicionar o servidor como um cliente RADIUS no NPS para que o NPS esteja ciente e possa se comunicar com o servidor de acesso à rede.

**Procedure**

1. No servidor NPS, no console do NPS, clique duas vezes em **clientes e servidores RADIUS**.

2. Clique com o botão direito do mouse em **clientes RADIUS** e selecione **novo**. A caixa de diálogo novo cliente RADIUS é aberta.

3. Verifique se a caixa de seleção **habilitar este cliente RADIUS** está marcada.

4. Em **nome amigável**, insira um nome de exibição para o servidor VPN.

5. Em **Endereço (IP ou DNS)** , insira o endereço IP do nas ou o FQDN.
     
     Se você inserir o FQDN, selecione **verificar** se deseja verificar se o nome está correto e se mapeia para um endereço IP válido.

6. Em **segredo compartilhado**, faça:

    1. Verifique se a seleção **manual** está selecionada.

    2. Insira a cadeia de texto forte que você também inseriu no servidor VPN.

    3. Insira novamente o segredo compartilhado em confirmar segredo compartilhado.

7. Selecione **OK**. O servidor VPN aparece na lista de clientes RADIUS configurados no servidor NPS.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configurar o NPS como um RADIUS para conexões VPN

Neste procedimento, você configura o NPS como um servidor RADIUS na rede da sua organização. No NPS, você deve definir uma política que permita que somente os usuários em um grupo específico acessem a rede corporativa/organizacional por meio do servidor VPN e, em seguida, somente ao usar um certificado de usuário válido em uma solicitação de autenticação PEAP.

**Procedure**

1. No console do NPS, em configuração padrão, verifique se **servidor RADIUS para conexões dial-up ou VPN** está selecionado.

2. Selecione **Configurar VPN ou dial-up**.
        
    O assistente para configurar VPN ou dial-up é aberto.

3. Selecione **conexões de VPN (rede virtual privada)** e selecione **Avançar**.

4. Em especificar servidor dial-up ou VPN, em clientes RADIUS, selecione o nome do servidor VPN que você adicionou na etapa anterior. Por exemplo, se o nome NetBIOS do servidor VPN for RAS1, selecione **RAS1**.

5. Selecione **Avançar**.

6. Em Configurar métodos de autenticação, conclua as seguintes etapas:

    1. Desmarque a caixa de seleção **autenticação criptografada da Microsoft versão 2 (MS-CHAPv2)** .

    2. Marque a caixa de seleção **protocolo de autenticação extensível** para selecioná-la.

    3. Em tipo (com base no método de acesso e configuração de rede), **selecione Microsoft: EAP protegido (PEAP)** e, em seguida, selecione **Configurar**.
      
        A caixa de diálogo Editar propriedades EAP protegidas é aberta.

    4. Selecione **remover** para remover o tipo de EAP de senha segura (EAP-MSCHAP v2).

    5. Selecione **Adicionar**. A caixa de diálogo Adicionar EAP é aberta.

    6. Selecione **cartão inteligente ou outro certificado**e, em seguida, selecione **OK**.

    7. Selecione **OK** para fechar a edição de propriedades EAP protegidas.

7. Selecione **Avançar**.

8. Em especificar grupos de usuários, conclua as seguintes etapas:

    1. Selecione **Adicionar**. A caixa de diálogo Selecionar usuários, computadores, contas de serviço ou grupos é aberta.

    2. Insira **usuários VPN**e, em seguida, selecione **OK**.

    3. Selecione **Avançar**.

9. Em especificar filtros IP, selecione **Avançar**.

10. Em especificar configurações de criptografia, selecione **Avançar**. Não faça nenhuma alteração.

    Essas configurações se aplicam somente a conexões MPPE (criptografia ponto a ponto) da Microsoft, às quais esse cenário não dá suporte.

11. Em especificar um nome de realm, selecione **Avançar**.

12. Selecione **concluir** para fechar o assistente.

## <a name="autoenroll-the-nps-server-certificate"></a>Registrar automaticamente o certificado do servidor NPS

Neste procedimento, você atualiza Política de Grupo no servidor NPS local manualmente. Quando o Política de Grupo for atualizado, se o registro automático de certificado estiver configurado e funcionando corretamente, o computador local será registrado automaticamente em um certificado pela autoridade de certificação (CA).

>[!NOTE]  
>Política de Grupo atualizado automaticamente quando você reinicia o computador membro do domínio ou quando um usuário faz logon em um computador membro do domínio. Além disso, Política de Grupo atualiza periodicamente. Por padrão, essa atualização periódica ocorre a cada 90 minutos com um deslocamento aleatório de até 30 minutos.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

**Procedure**

1. No NPS, abra o Windows PowerShell.

2. No prompt do Windows PowerShell, digite **gpupdate**e pressione Enter.

## <a name="next-steps"></a>Próximas etapas

[Etapa 5. Defina as configurações de DNS e firewall para](vpn-deploy-dns-firewall.md)Always on VPN: Nesta etapa, você instala o NPS (servidor de políticas de rede) usando o Windows PowerShell ou o assistente Gerenciador do Servidor adicionar funções e recursos. Você também configura o NPS para lidar com todas as tarefas de autenticação, autorização e contabilidade das solicitações de conexão que ele recebe do servidor VPN.