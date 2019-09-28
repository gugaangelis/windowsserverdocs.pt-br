---
title: Configurar o servidor de acesso remoto para VPN Always On
description: O RRAS foi projetado para funcionar bem como um roteador e um servidor de acesso remoto; Portanto, ele dá suporte a uma ampla gama de recursos.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: c04074338cf4ba0189eb1e9bc45a80b948fdbfbf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388037"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Etapa 3. Configurar o servidor de acesso remoto para VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior** Etapa 2. Configurar a infraestrutura do servidor](vpn-deploy-server-infrastructure.md)
- [**Anterior** Etapa 4. Instalar e configurar o servidor de políticas de rede (NPS)](vpn-deploy-nps.md)

O RRAS foi projetado para funcionar bem como um roteador e um servidor de acesso remoto, pois ele dá suporte a uma ampla gama de recursos. Para os fins desta implantação, você precisa apenas de um pequeno subconjunto desses recursos: suporte para conexões VPN IKEv2 e roteamento de LAN.

IKEv2 é um protocolo de encapsulamento de VPN descrito em solicitação de força de tarefa de engenharia da Internet para comentários 7296. A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se a conexão for temporariamente perdida ou se um usuário mover um computador cliente de uma rede para outra, o IKEv2 restaurará automaticamente a conexão VPN quando a conexão de rede for restabelecida, tudo isso sem a intervenção do usuário.

Configure o servidor RRAS para dar suporte a conexões IKEv2 ao desabilitar protocolos não utilizados, o que reduz a superfície de segurança do servidor. Além disso, configure o servidor para atribuir endereços a clientes VPN de um pool de endereços estáticos. Você pode conseguiu atribuir endereços de um pool ou de um servidor DHCP; no entanto, o uso de um servidor DHCP adiciona complexidade ao design e oferece benefícios mínimos.

>[!IMPORTANT]
>É importante:
>- Instale dois adaptadores de rede Ethernet no servidor físico. Se você estiver instalando o servidor VPN em uma VM, deverá criar dois comutadores virtuais externos, um para cada adaptador de rede física; em seguida, crie dois adaptadores de rede virtual para a VM, com cada adaptador de rede conectado a um comutador virtual.
>
>- Instale o servidor em sua rede de perímetro entre seus firewalls de borda e internos, com um adaptador de rede conectado à rede de perímetro externa e um adaptador de rede conectado à rede de perímetro interna.

>[!WARNING]
>Antes de começar, certifique-se de habilitar o IPv6 no servidor VPN. Caso contrário, uma conexão não pode ser estabelecida e uma mensagem de erro é exibida.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Instalar o acesso remoto como um servidor VPN de gateway de RAS

Neste procedimento, você instala a função de acesso remoto como um servidor VPN de gateway de RAS de locatário único. Para obter mais informações, consulte [Acesso Remoto](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Instalar a função de acesso remoto usando o Windows PowerShell

1. Abra o Windows PowerShell como **administrador**.

2. Insira e execute o seguinte cmdlet:

   ```powershell
   Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools
   ```

   Após a conclusão da instalação, a seguinte mensagem aparecerá no Windows PowerShell.

   ```powershell
   | Success | Restart Needed | Exit Code |               Feature Result               |
   |---------|----------------|-----------|--------------------------------------------|
   |  True   |       No       |  Success  | {RAS Connection Manager Administration Kit |
   ```

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Instalar a função de acesso remoto usando Gerenciador do Servidor

Você pode usar o procedimento a seguir para instalar a função de acesso remoto usando Gerenciador do Servidor.

1. No servidor VPN, em Gerenciador do Servidor, selecione **gerenciar** e selecione **adicionar funções e recursos**.
   
   O Assistente para Adicionar Funções e Recursos é aberto.

2. Na página antes de começar, selecione **Avançar**.

3. Na página Selecionar tipo de instalação, selecione a opção de instalação baseada em **função ou recurso** e selecione **Avançar**.

4. Na página Selecionar servidor de destino, selecione a opção **selecionar um servidor do pool de servidores** .

5. Em pool de servidores, selecione o computador local e selecione **Avançar**.

6. Na página Selecionar funções de servidor, em **funções**, selecione **acesso remoto**e **Avançar**.

7. Na página Selecionar recursos, selecione **Avançar**.

8. Na página acesso remoto, selecione **Avançar**.

9.  Na página Selecionar Serviço de função, em **serviços de função**, selecione **DirectAccess e VPN (RAS)** .

   A caixa de diálogo **Assistente de adição de funções e recursos** é aberta.

11. Na caixa de diálogo Adicionar funções e recursos, selecione **Adicionar recursos** e selecione **Avançar**.

12. Na página função de servidor Web (IIS), selecione **Avançar**.

13. Na página Selecionar Serviços de função, selecione **Avançar**.

14. Na página confirmar seleções de instalação, examine suas escolhas e selecione **instalar**.

15. Quando a instalação for concluída, selecione **fechar**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurar o acesso remoto como um servidor VPN

Nesta seção, você pode configurar a VPN de acesso remoto para permitir conexões VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para conectar clientes VPN autorizados.

1. No servidor VPN, em Gerenciador do Servidor, selecione o sinalizador **notificações** .

2. No menu **tarefas** , selecione **abrir o assistente de introdução**

   O assistente para configurar acesso remoto é aberto.

   >[!NOTE]
   >O assistente para configurar acesso remoto pode abrir por trás do Gerenciador do Servidor. Se você achar que o assistente está demorando muito para abrir, mova ou minimize Gerenciador do Servidor para descobrir se o assistente está por trás dele. Caso contrário, aguarde até que o assistente seja inicializado.

3. Selecione **implantar somente VPN**.

    O MMC (console de gerenciamento Microsoft) de roteamento e acesso remoto é aberto.

4. Clique com o botão direito do mouse no servidor VPN e selecione **configurar e habilitar roteamento e acesso remoto**.

   O assistente de instalação do servidor de roteamento e acesso remoto é aberto.

5. Na página Bem-vindo ao Assistente para instalação do servidor de roteamento e acesso remoto, selecione **Avançar**.

6. Em **configuração**, selecione **configuração personalizada**e, em seguida, selecione **Avançar**.

7. Em **configuração personalizada**, selecione **acesso VPN**e, em seguida, selecione **Avançar**.

   O assistente para concluir o roteamento e a instalação do servidor de acesso remoto é aberto.

8. Selecione **concluir** para fechar o assistente e, em seguida, selecione **OK** para fechar a caixa de diálogo roteamento e acesso remoto.

9. Selecione **Iniciar serviço** para iniciar o acesso remoto.

10. No MMC de acesso remoto, clique com o botão direito do mouse no servidor VPN e selecione **Propriedades**.

11. Em Propriedades, selecione a guia **segurança** e:

    a. Selecione **provedor de autenticação** e selecione **autenticação RADIUS**.

    b. Selecione **Configurar**.

       A caixa de diálogo autenticação RADIUS é aberta.

    c. Selecione **Adicionar**.

       A caixa de diálogo Adicionar servidor RADIUS é aberta.

    d. Em **nome do servidor**, insira o nome de domínio totalmente qualificado (FQDN) do servidor NPS em sua organização/rede corporativa.
    
       Por exemplo, se o nome NetBIOS do servidor NPS for NPS1 e o nome de domínio for corp.contoso.com, digite **NPS1.Corp.contoso.com**.

    e. Em **segredo compartilhado**, selecione **alterar**.

       A caixa de diálogo Alterar segredo é aberta.

    f. Em **novo segredo**, insira uma cadeia de texto.

    g. Em **confirmar novo segredo**, insira a mesma cadeia de texto e, em seguida, selecione **OK**.

    >[!IMPORTANT]
    >Salve esta cadeia de texto. Ao configurar o servidor NPS em sua organização/rede corporativa, você adicionará esse servidor VPN como um cliente RADIUS. Durante essa configuração, você usará esse mesmo segredo compartilhado para que os servidores NPS e VPN possam se comunicar.

12. Em **Adicionar servidor RADIUS**, examine as configurações padrão para:

    - **Tempo limite**

    - **Pontuação inicial**

    - **Porto**

13. Se necessário, altere os valores para corresponder aos requisitos do seu ambiente e selecione **OK**.

    Um NAS é um dispositivo que fornece algum nível de acesso a uma rede maior. Um NAS que usa uma infraestrutura RADIUS também é um cliente RADIUS, enviando solicitações de conexão e mensagens de contabilização para um servidor RADIUS para autenticação, autorização e contabilidade.

14. Examine a configuração do **provedor de contabilidade**:

    |                    Se você quiser...                     |                                                     Então…                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Atividade de acesso remoto registrada no servidor de acesso remoto |                               Verifique se a **contabilidade do Windows** está selecionada.                               |
    |        NPS para executar serviços de contabilidade para VPN         | Altere o **provedor de contabilização** para **contabilização RADIUS** e configure o NPS como o provedor de contabilidade. |

15. Selecione a guia **IPv4** e faça:

    a. Selecione **pool de endereços estáticos**.

    b. Selecione **Adicionar** para configurar um pool de endereços IP.

       O pool de endereços estáticos deve conter endereços da rede de perímetro interna. Esses endereços estão na conexão de rede interna no servidor VPN, não na rede corporativa.

    c. Em **endereço IP inicial**, insira o endereço IP inicial no intervalo que você deseja atribuir a clientes VPN.

    d. Em **endereço IP final**, insira o endereço IP final no intervalo que você deseja atribuir a clientes VPN, ou em **número de endereços**, insira o número do endereço que você deseja disponibilizar. Se você estiver usando o DHCP para essa sub-rede, certifique-se de configurar uma exclusão de endereço correspondente em seus servidores DHCP.

    e. Adicional Se você estiver usando DHCP, selecione **adaptador**e, na lista de resultados, selecione o adaptador Ethernet conectado à sua rede de perímetro interna.

16. Adicional *Se você estiver configurando o acesso condicional para conectividade VPN*, na lista suspensa **certificado** , em **Associação de certificado SSL**, selecione a autenticação do servidor VPN.

17. Adicional *Se você estiver configurando o acesso condicional para conectividade VPN*, no MMC do NPS, expanda **políticas\\políticas de rede** e faça: 

    a. Direita – as **conexões com a política de rede do servidor de roteamento e acesso remoto da Microsoft** e selecione **Propriedades**.

    b. **Selecione conceder acesso. Conceder acesso se a solicitação de conexão corresponder a** essa opção de política.

    c. Em tipo de servidor de acesso à rede, selecione **servidor de acesso remoto (VPN-Dial up)** na lista suspensa.

18. No MMC roteamento e acesso remoto, clique com o botão direito do mouse em **portas** e selecione **Propriedades**. 
    
    A caixa de diálogo Propriedades de portas é aberta.

19. Selecione **SSTP (WAN Miniport)** e selecione **Configurar**. A caixa de diálogo Configurar o dispositivo-Miniport WAN (SSTP) é aberta.

    a. Desmarque as caixas de seleção **conexões de acesso remoto (somente entrada)** e **conexões de roteamento de discagem por demanda (entrada e saída)** .

    b. Selecione **OK**.

20. Selecione **Wan Miniport (L2TP)** e selecione **Configurar**. A caixa de diálogo Configurar dispositivo-WAN Miniport (L2TP) é aberta.

    a. Em **máximo de portas**, insira o número de portas para corresponder ao número máximo de conexões VPN simultâneas às quais você deseja dar suporte.

    b. Selecione **OK**.

21. Selecione **Wan Miniport (PPTP)** e selecione **Configurar**. A caixa de diálogo Configurar dispositivo – Miniporta WAN (PPTP) é aberta.

    a. Em **máximo de portas**, insira o número de portas para corresponder ao número máximo de conexões VPN simultâneas às quais você deseja dar suporte.

    b. Selecione **OK**.

22. Selecione **Wan Miniport (IKEv2)** e selecione **Configurar**. A caixa de diálogo Configurar dispositivo – Miniporta de WAN (IKEv2) é aberta.

     a. Em **máximo de portas**, insira o número de portas para corresponder ao número máximo de conexões VPN simultâneas às quais você deseja dar suporte.

     b. Selecione **OK**.

23. Se solicitado, selecione **Sim** para confirmar a reinicialização do servidor e selecione **fechar** para reiniciar o servidor.

## <a name="next-step"></a>Próximas etapas

[Etapa 4. Instalar e configurar o servidor de políticas de rede (](vpn-deploy-nps.md)NPS): Nesta etapa, você instala o NPS (servidor de políticas de rede) usando o Windows PowerShell ou o assistente Gerenciador do Servidor adicionar funções e recursos. Você também configura o NPS para lidar com todas as tarefas de autenticação, autorização e contabilidade das solicitações de conexão que ele recebe do servidor VPN.
