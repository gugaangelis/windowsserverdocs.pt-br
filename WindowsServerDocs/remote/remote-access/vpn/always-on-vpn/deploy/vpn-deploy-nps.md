---
title: Instalar e configurar o servidor NPS
description: Processamento do servidor NPS de solicitações de conexão que são enviadas pelo servidor VPN verifica se o usuário tem permissão para conectar-se a identidade do usuário e registra os aspectos da solicitação de conexão que você escolheu quando configurou a contabilidade RADIUS no NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca52aeeed8c4872e4d9a433c55bddc65a74d9dc0
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749615"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Etapa 4. Instalar e configurar o servidor de diretivas de rede (NPS)

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Avançar:** Etapa 3. Configurar o servidor de acesso remoto da VPN Always On](vpn-deploy-ras.md)
- [**Avançar:** Etapa 5. Definir as configurações de DNS e de firewall](vpn-deploy-dns-firewall.md)

Nesta etapa, você instalará o servidor de diretivas de rede (NPS) para processamento de solicitações de conexão que são enviadas pelo servidor VPN:

- Execute a autorização para verificar se o usuário tem permissão para se conectar.
- Executar a autenticação para verificar a identidade do usuário.
- Execução de contabilidade para registrar em log os aspectos da solicitação de conexão que você escolheu quando configurou a contabilidade RADIUS no NPS.

As etapas nesta seção permitem que você concluir os seguintes itens:

1. No computador ou VM que planejado para o servidor NPS e instalado em sua organização ou a rede corporativa, você pode instalar o NPS.

   >[!TIP]
   >Se você já tiver um ou mais servidores NPS em sua rede, você não precisará executar a instalação do servidor NPS - em vez disso, você pode usar este tópico para atualizar a configuração de um servidor existente do NPS.

> [!NOTE]
> Você não pode instalar o serviço servidor de diretiva de rede no Windows Server Core.

2. No servidor NPS/corporativas de organização, você pode configurar o NPS como um servidor RADIUS que processa as solicitações de conexão recebidas do servidor VPN.

## <a name="install-network-policy-server"></a>Instalar o NPS (Servidor de Políticas de Rede)

Neste procedimento, você instala o NPS usando o Windows PowerShell ou o Server Manager adicionar funções e Assistente de recursos. O NPS é um serviço de função da função de servidor Serviços de Acesso e Política de Rede.

>[!TIP]
>Por padrão, o NPS ouve o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Quando você instalar o NPS e habilitar o Firewall do Windows com segurança avançada, exceções de firewall para essas portas são criadas automaticamente para o tráfego IPv4 e IPv6. Se seus servidores de acesso de rede estiverem configurados para enviar tráfego RADIUS em portas que não sejam essa padrão, remova as exceções criadas no Firewall do Windows com segurança avançada durante a instalação do NPS e crie exceções para as portas que você usa para Tráfego RADIUS.

**Procedimento para o Windows PowerShell:**

Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite o seguinte cmdlet:

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**Procedimento para o Gerenciador do servidor:**

1.  No Gerenciador do servidor, selecione **Manage**, em seguida, selecione **para adicionar funções e recursos**.
        O Assistente para Adicionar Funções e Recursos é aberto.

2.  Em antes de começar, selecione **próxima**.

    >[!NOTE] 
    >O **antes de começar** página do assistente Adicionar funções e recursos não será exibida se você selecionou anteriormente **pular esta página por padrão** quando o assistente Adicionar funções e recursos executou.

3.  Em Selecionar tipo de instalação, certifique-se de que **instalação baseada em função ou recurso** está selecionado e selecione **próxima**.

4.  Em Selecionar servidor de destino, certifique-se de que **selecione um servidor no pool de servidores** está selecionado.

5.  No Pool de servidores, certifique-se de que o computador local é selecionado e selecione **próxima**.

6.  Em Selecionar funções de servidor, na **funções**, selecione **serviços de acesso e diretiva de rede**. Uma caixa de diálogo é aberta, perguntando se ele deve adicionar recursos necessários para serviços de acesso e diretiva de rede.

7.  Selecione **adicionar recursos**, em seguida, selecione **Avançar**

8.  Em selecionados recursos, selecione **próxima**e em serviços de acesso e diretiva de rede, examine as informações fornecidas e selecione **próxima**.

9.  Nos serviços de função Select, selecione **Network Policy Server**.

10. Para os recursos necessários para o servidor de políticas de rede, selecione **adicionar recursos**, em seguida, selecione **próxima**.

11. Em Confirmar seleções de instalação, selecione **reiniciar o servidor de destino automaticamente se necessário**.

12. Selecione **Yes** para confirmar o selecionado e, em seguida, selecione **instalar**.
    
    A página de progresso da instalação exibe o status durante o processo de instalação. Quando o processo for concluído, a mensagem "instalação bem-sucedida no *NomeDoComputador*" é exibida, onde *ComputerName* é o nome do computador no qual você instalou o servidor de políticas de rede.

13. Selecione **Fechar**.

## <a name="configure-nps"></a>Configurar o NPS

Depois de instalar o NPS, você configura o NPS para lidar com todos os autenticação, autorização e obrigações de estatísticas de conexão solicitação-la recebe do servidor VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Registrar o servidor NPS no Active Directory

Neste procedimento, você registra o servidor no Active Directory para que ele tenha permissão para acessar informações de conta de usuário durante o processamento de solicitações de conexão.

**Procedimento:**

1.  No Gerenciador do servidor, selecione **ferramentas**e, em seguida, selecione **Network Policy Server**. Abre o console do NPS.

2.  No console do NPS, clique com botão direito **NPS (Local)** , em seguida, selecione **registrar servidor no Active Directory**.
   
     Abre a caixa de diálogo do servidor de políticas de rede.

3.  Na caixa de diálogo servidor de políticas de rede, selecione **Okey** duas vezes.

Para obter métodos alternativos de registrar o NPS, consulte [registrar um servidor NPS em um domínio do Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurar a contabilização do Servidor de Políticas de Rede

Neste procedimento, configure a política de servidor de contabilidade de rede usando um dos seguintes tipos de registro em log:

- **O log de eventos**. Usado principalmente para auditoria e solucionar problemas de tentativas de conexão. Você pode configurar eventos NPS em log, obtendo as propriedades do servidor NPS no console do NPS.

- **Registro em log solicitações de contabilização e autenticação de usuário em um arquivo local**. Usado principalmente para fins de análise e a cobrança de conexão. Também usado como uma ferramenta de investigação de segurança porque ele oferece um método de controle a atividade de um usuário mal-intencionado após um ataque. Você pode configurar o log de arquivo local usando o Assistente de configuração de contabilização.

- **Registro em log solicitações de contabilização e autenticação de usuário para um banco de dados compatível com o Microsoft SQL Server XML**. Usado para permitir que vários servidores executando o NPS para ter uma fonte de dados. Também fornece as vantagens de usar um banco de dados relacional. Você pode configurar o log do SQL Server usando o Assistente de configuração de contabilização.

Para configurar a contabilização de servidor de diretiva de rede, consulte [configurar política de servidor de contabilidade de rede](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Adicione o servidor VPN como um cliente RADIUS

No [configurar o servidor de acesso remoto VPN Always On](vpn-deploy-ras.md) seção, você instalou e configurou o servidor VPN. Durante a configuração do servidor VPN, você adicionou um segredo compartilhado RADIUS no servidor VPN.

Neste procedimento, você pode usar a mesma cadeia de caracteres de texto de segredo compartilhado para configurar o servidor VPN como um cliente RADIUS no NPS. Use a mesma cadeia de texto que você usou no servidor VPN ou falha de comunicação entre o servidor NPS e o servidor VPN.

>[!IMPORTANT] 
>Quando você adiciona um servidor de acesso de rede novo (servidor VPN, o ponto de acesso sem fio, comutador de autenticação ou servidor dial-up) à sua rede, você deve adicionar o servidor como um cliente RADIUS no NPS para que o NPS está ciente e podem se comunicar com o servidor de acesso de rede.

**Procedimento:**

1. No servidor NPS, no console do NPS, clique duas vezes em **clientes e servidores RADIUS**.

2. Clique com botão direito **clientes RADIUS** e selecione **New**. Abre a caixa de diálogo Novo cliente RADIUS.

3. Verifique se que o **habilitar este cliente RADIUS** caixa de seleção está selecionada.

4. Na **nome amigável**, insira um nome de exibição para o servidor VPN.

5. Na **endereço (IP ou DNS)** , digite o endereço IP NAS ou FQDN.
     
     Se você inserir o FQDN, selecione **Verify** se você deseja verificar se o nome está correto e é mapeado para um endereço IP válido.

6. Na **segredo compartilhado**, fazer:

    1. Certifique-se de que **Manual** está selecionado.

    2. Insira a cadeia de caracteres de texto de alta segurança que você inseriu também no servidor VPN.

    3. Digite novamente o segredo compartilhado em Confirmar segredo compartilhado.

7. Selecione **OK**. O servidor VPN é exibido na lista de clientes RADIUS configurado no servidor NPS.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configurar o NPS como um RADIUS para conexões VPN

Neste procedimento, você configura o NPS como servidor RADIUS na rede da sua organização. No NPS, você deve definir uma política que permite que somente usuários em um grupo específico para acessar a rede corporativa/organização por meio do servidor VPN - e, em seguida, somente quando usar um certificado de usuário válido em uma solicitação de autenticação do PEAP.

**Procedimento:**

1. No console do NPS, na configuração padrão, certifique-se de que **o servidor RADIUS para conexões de VPN ou Dial-Up** está selecionado.

2. Selecione **configurar VPN ou Dial-Up**.
        
    O Assistente para configurar VPN ou Dial-Up é aberta.

3. Selecione **conexões de rede Virtual privada (VPN)** e selecione **próxima**.

4. Em especificar Dial-Up ou servidor de VPN, em clientes RADIUS, selecione o nome do servidor VPN que você adicionou na etapa anterior. Por exemplo, se seu nome NetBIOS do servidor VPN é RAS1, selecione **RAS1**.

5. Selecione **Avançar**.

6. Configurar métodos de autenticação, conclua as seguintes etapas:

    1. Desmarque a **autenticação criptografada da Microsoft versão 2 (MS-CHAPv2)** caixa de seleção.

    2. Selecione o **Extensible Authentication Protocol** caixa de seleção para selecioná-lo.

    3. Em tipo (com base no método de acesso e configuração de rede), selecione **Microsoft: EAP protegido (PEAP)** , em seguida, selecione **configurar**.
      
        Abre a caixa de diálogo Editar propriedades de EAP protegidas.

    4. Selecione **remover** para remover o tipo EAP de senha segura (EAP-MSCHAP v2).

    5. Selecione **Adicionar**. Abre a caixa de diálogo Adicionar EAP.

    6. Selecione **do cartão inteligente ou outro certificado**, em seguida, selecione **Okey**.

    7. Selecione **Okey** para editar propriedades de EAP protegidas de fechar.

7. Selecione **Avançar**.

8. Em especificar grupos de usuários, conclua as seguintes etapas:

    1. Selecione **Adicionar**. Abre a caixa de diálogo Selecionar usuários, computadores, contas de serviço ou grupos.

    2. Insira **usuários de VPN**, em seguida, selecione **Okey**.

    3. Selecione **Avançar**.

9. Em especificar filtros de IP, selecione **próxima**.

10. Em especificar configurações de criptografia, selecione **próxima**. Não faça qualquer alteração.

    Essas configurações se aplicam somente a conexões de criptografia de ponto a ponto da Microsoft (MPPE), que não dá suporte a esse cenário.

11. Em Especifique um nome de Realm, selecione **próxima**.

12. Selecione **concluir** para fechar o assistente.

## <a name="autoenroll-the-nps-server-certificate"></a>Registro automático de certificado de servidor NPS

Neste procedimento, você atualizar política de grupo no servidor NPS local manualmente. Quando atualizações de diretiva de grupo, se o registro automático de certificado estiver configurado e funcionando corretamente, o computador local é autom-registro de um certificado pela autoridade de certificação (CA).

>[!NOTE]  
>Política de grupo é atualizada automaticamente quando você reiniciar o computador membro do domínio, ou quando um usuário faz logon em um computador membro de domínio. Além disso, diretiva de grupo atualiza periodicamente. Por padrão, essa atualização periódica ocorre a cada 90 minutos com um deslocamento aleatório de até 30 minutos.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

**Procedimento:**

1. No NPS, abra o Windows PowerShell.

2. No prompt do Windows PowerShell, digite **gpupdate**, e pressione ENTER.

## <a name="next-steps"></a>Próximas etapas

[Etapa 5. Definir configurações de DNS e o firewall para VPN Always On](vpn-deploy-dns-firewall.md): Nesta etapa, instale o servidor de diretivas de rede (NPS) usando o Windows PowerShell ou o Server Manager adicionar funções e Assistente de recursos. Você também configurar o NPS para lidar com todos os autenticação, autorização e obrigações de estatísticas de conexão solicitações recebidas do servidor VPN.