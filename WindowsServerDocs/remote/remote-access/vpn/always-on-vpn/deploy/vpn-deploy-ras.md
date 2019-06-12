---
title: Configurar o servidor de acesso remoto para VPN Always On
description: RRAS é projetado para executar bem como um roteador e um servidor de acesso remoto; Portanto, ele dá suporte a uma ampla gama de recursos.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 3920f7f075f4742a62577ade809cc0494b05bf1f
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749436"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Etapa 3. Configurar o servidor de acesso remoto para VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 2. Configurar a infraestrutura de servidor](vpn-deploy-server-infrastructure.md)
- [**Anterior:** Etapa 4. Instalar e configurar o servidor de diretivas de rede (NPS)](vpn-deploy-nps.md)

RRAS é projetado para executar bem como um roteador e um servidor de acesso remoto porque ele dá suporte a uma ampla gama de recursos. Para os fins desta implantação, você precisa apenas um pequeno subconjunto desses recursos: suporte para conexões VPN IKEv2 e roteamento de rede local.

O IKEv2 está descrito na solicitação de Internet Engineering Task Force para comentários 7296 de protocolo de túnel VPN. A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se a conexão for perdida temporariamente ou se um usuário move um computador cliente de uma rede para outra, IKEv2 restaura automaticamente a conexão VPN quando a conexão de rede for restabelecida — tudo sem a intervenção do usuário.

Configure o servidor RRAS para dar suporte a conexões IKEv2 enquanto desabilitando protocolos não utilizados, o que reduz o volume de segurança do servidor. Além disso, configure o servidor para atribuir endereços a clientes VPN de um pool de endereços estáticos. Você conseguiu pode atribuir endereços de um pool ou um servidor DHCP. No entanto, usando um servidor DHCP adiciona complexidade ao design e oferece os benefícios de mínimo.

>[!IMPORTANT]
>É importante:
>- Instale dois adaptadores de rede Ethernet em um servidor físico. Se você estiver instalando o servidor VPN em uma máquina virtual, você deve criar dois comutadores virtuais externos, uma para cada adaptador de rede física; e, em seguida, crie dois adaptadores de rede virtual para a VM, com cada adaptador de rede conectado a um comutador virtual.
>
>- Instale o servidor em sua rede de perímetro entre a borda e firewalls internos, com um adaptador de rede conectado à rede de perímetro externo e um adaptador de rede conectado à rede de perímetro interna.

>[!WARNING]
>Antes de começar, certifique-se de habilitar o IPv6 no servidor VPN. Caso contrário, não é possível estabelecer uma conexão e exibe uma mensagem de erro.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Instalar o acesso remoto como um servidor VPN de Gateway RAS

Neste procedimento, você pode instalar a função acesso remoto como um servidor de VPN de Gateway RAS de único locatário. Para obter mais informações, consulte [Acesso Remoto](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Instalar a função acesso remoto usando o Windows PowerShell

1. Abra o Windows PowerShell como **administrador**.

2. Digite e execute o seguinte cmdlet:

   ```powershell
   Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools
   ```

   Após a conclusão da instalação, a seguinte mensagem será exibida no Windows PowerShell.

   ```powershell
   | Success | Restart Needed | Exit Code |               Feature Result               |
   |---------|----------------|-----------|--------------------------------------------|
   |  True   |       No       |  Success  | {RAS Connection Manager Administration Kit |
   ```

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Instalar a função acesso remoto usando o Gerenciador do servidor

Você pode usar o procedimento a seguir para instalar a função de acesso remoto usando o Gerenciador do servidor.

1. No servidor VPN, no Gerenciador do servidor, selecione **Manage** e selecione **para adicionar funções e recursos**.
   
   O Assistente para Adicionar Funções e Recursos é aberto.

2. No antes de você começar a página, selecione **próxima**.

3. Na página Selecionar tipo de instalação, selecione a **instalação baseada em função ou recurso** opção e selecione **próxima**.

4. Na página Selecionar destino do servidor, selecione a **selecione um servidor no pool de servidores** opção.

5. Em Pool de servidores, selecione o computador local e selecione **próxima**.

6. Em Selecionar funções de servidor página, na **funções**, selecione **acesso remoto**, em seguida, **próxima**.

7. Na página Selecionar recursos, selecione **próxima**.

8. Na página de acesso remoto, selecione **próxima**.

9.  Na página Selecionar função do serviço, na **serviços de função**, selecione **DirectAccess e VPN (RAS)** .

   O **assistente Adicionar funções e recursos** caixa de diálogo é aberta.

11. Na caixa de diálogo Adicionar funções e recursos, selecione **adicionar recursos** , em seguida, selecione **próxima**.

12. Na página função de servidor Web (IIS), selecione **próxima**.

13. Na página Selecionar função de serviços, selecione **próxima**.

14. Na página Confirmar seleções de instalação, reveja suas escolhas e selecione **instalar**.

15. Quando a instalação for concluída, selecione **fechar**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurar o acesso remoto como um servidor VPN

Nesta seção, você pode configurar o acesso remoto VPN para permitir conexões de VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para se conectar a clientes autorizados de VPN.

1. No servidor VPN, no Gerenciador do servidor, selecione a **notificações** sinalizador.

2. No **tarefas** menu, selecione **abrir o Assistente de Introdução**

   Abre o Assistente para configurar o acesso remoto.

   >[!NOTE]
   >O Assistente para configurar o acesso remoto pode abrir o Gerenciador de servidores por trás. Se você achar que o assistente está demorando muito para abrir, mover ou minimizar o Gerenciador do servidor para descobrir se o assistente está por trás dele. Caso contrário, aguarde até que o assistente inicializar.

3. Selecione **implantar VPN apenas**.

    O roteamento e acesso Microsoft Management Console (MMC) remoto é aberto.

4. Com o botão direito no servidor VPN e, em seguida, selecione **configurar e habilitar roteamento e acesso remoto**.

   O roteamento e Assistente de instalação do servidor de acesso remoto é aberto.

5. Em Bem-vindo ao Assistente de instalação do servidor de acesso remoto e roteamento, selecione **próxima**.

6. Na **Configuration**, selecione **configuração personalizada**e, em seguida, selecione **próxima**.

7. Na **configuração personalizada**, selecione **acesso VPN**e, em seguida, selecione **próxima**.

   Concluindo o Assistente de instalação do servidor de acesso remoto e de roteamento é aberto.

8. Selecione **terminar** para fechar o assistente, em seguida, selecione **Okey** para fechar a caixa de diálogo roteamento e acesso remoto.

9. Selecione **iniciar o serviço** para iniciar o acesso remoto.

10. No MMC do acesso remoto, clique com botão direito no servidor VPN e, em seguida, selecione **propriedades**.

11. Em propriedades, selecione a **segurança** guia e fazer:

    a. Selecione **provedor de autenticação** e selecione **autenticação RADIUS**.

    b. Selecione **configurar**.

       Abre a caixa de diálogo de autenticação RADIUS.

    c. Selecione **Adicionar**.

       Abre a caixa de diálogo Adicionar servidor RADIUS.

    d. Na **nome do servidor**, insira o nome de domínio totalmente qualificado (FQDN) do servidor NPS em sua rede corporativa/organização.
    
       Por exemplo, se o nome NetBIOS do servidor NPS é NPS1 e seu nome de domínio for corp.contoso.com, digite **NPS1.corp.contoso.com**.

    e. Na **segredo compartilhado**, selecione **alteração**.

       Abre a caixa de diálogo Alterar segredo.

    f. Na **novo segredo**, insira uma cadeia de caracteres de texto.

    g. Na **Confirmar novo segredo**, insira a mesma cadeia de caracteres de texto e selecione **Okey**.

    >[!IMPORTANT]
    >Salve essa cadeia de caracteres de texto. Quando você configura o servidor NPS em sua rede corporativa/organização, você irá adicionar este servidor de VPN como um cliente RADIUS. Durante a configuração, você usará esse mesmo segredo compartilhado para que o NPS e servidores VPN podem se comunicar.

12. Na **Adicionar servidor RADIUS**, examine as configurações padrão para:

    - **Tempo limite**

    - **Pontuação inicial**

    - **Porta**

13. Se necessário, altere os valores de acordo com os requisitos para o seu ambiente e selecione **Okey**.

    NAS é um dispositivo que fornece algum nível de acesso a uma rede maior. NAS usando uma infraestrutura RADIUS também é um cliente RADIUS, enviando solicitações de conexão e mensagens de estatísticas para um servidor RADIUS para autenticação, autorização e contabilidade.

14. Examine a configuração para **provedor de contabilização**:

    |                    Se você deseja que o...                     |                                                     Então…                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Atividade de acesso remota conectada no servidor de acesso remoto |                               Certifique-se de que **contabilidade Windows** está selecionado.                               |
    |        NPS para realizar serviços de contabilidade para VPN         | Alteração **provedor de contabilização** à **contabilidade RADIUS** e, em seguida, configurar o NPS como o provedor de estatísticas. |

15. Selecione o **IPv4** guia e fazer:

    a. Selecione **pool de endereços estáticos**.

    b. Selecione **adicionar** para configurar um pool de endereços IP.

       O pool de endereços estáticos deve conter endereços da rede de perímetro interna. Esses endereços são sobre a conexão de rede internos no servidor VPN, não a rede corporativa.

    c. Na **endereço IP inicial**, insira o endereço IP inicial do intervalo que você deseja atribuir a clientes VPN.

    d. Na **endereço IP final**, insira o endereço IP final no intervalo que você deseja atribuir a clientes VPN ou na **número de endereços**, insira o número do endereço que você deseja disponibilizar. Se você estiver usando DHCP para esta sub-rede, certifique-se de que você configure uma exclusão de endereço correspondente em seus servidores DHCP.

    e. (Opcional) Se você estiver usando DHCP, selecione **adaptador**e na lista de resultados, selecione o adaptador de Ethernet conectado à sua rede de perímetro interna.

16. (Opcional) *Se você estiver configurando o acesso condicional para conectividade VPN*, da **certificado** lista suspensa lista, em **associação de certificados SSL**, selecione o servidor VPN autenticação.

17. (Opcional) *Se você estiver configurando o acesso condicional para conectividade VPN*, no MMC do NPS, expanda **diretivas\\diretivas de rede** e fazer: 

    a. O direito **conexões para Microsoft Routing and Remote Access Server** política de rede e selecione **propriedades**.

    b. Selecione o **conceder acesso. Conceder acesso se a solicitação de conexão corresponde a essa política** opção.

    c. Em tipo de servidor de acesso de rede, selecione **servidor de acesso remoto (VPN-Dial-up)** na lista suspensa.

18. No MMC do acesso remoto e roteamento, clique com botão direito **portas** e, em seguida, selecione **propriedades**. 
    
    Abre a caixa de diálogo de propriedades de portas.

19. Selecione **miniporta de rede de longa distância (SSTP)** e selecione **configurar**. Configurar dispositivo - Miniporta de rede de longa distância (SSTP) é aberta.

    a. Desmarque a **conexões de acesso remoto (somente de entrada)** e **conexões de roteamento de discagem por demanda (entradas e saídas)** caixas de seleção.

    b. Selecione **OK**.

20. Selecione **miniporta de rede remota (L2TP)** e selecione **configurar**. Configurar dispositivo - Miniporta de rede remota (L2TP) é aberta.

    a. Na **número máximo de portas**, insira o número de portas para corresponder ao número máximo de conexões VPN simultâneas que você deseja dar suporte.

    b. Selecione **OK**.

21. Selecione **miniporta de rede (PPTP)** e selecione **configurar**. Configurar dispositivo - Miniporta de rede (PPTP) é aberta.

    a. Na **número máximo de portas**, insira o número de portas para corresponder ao número máximo de conexões VPN simultâneas que você deseja dar suporte.

    b. Selecione **OK**.

22. Selecione **miniporta de rede remota (IKEv2)** e selecione **configurar**. Configurar dispositivo - Miniporta de rede remota (IKEv2) é aberta.

     a. Na **número máximo de portas**, insira o número de portas para corresponder ao número máximo de conexões VPN simultâneas que você deseja dar suporte.

     b. Selecione **OK**.

23. Se solicitado, selecione **Yes** para confirmar a reinicialização do servidor e selecione **fechar** para reiniciar o servidor.

## <a name="next-step"></a>Próximas etapas

[Etapa 4. Instalar e configurar o servidor de diretivas de rede (NPS)](vpn-deploy-nps.md): Nesta etapa, instale o servidor de diretivas de rede (NPS) usando o Windows PowerShell ou o Server Manager adicionar funções e Assistente de recursos. Você também configurar o NPS para lidar com todos os autenticação, autorização e obrigações de estatísticas de conexão solicitações recebidas do servidor VPN.
