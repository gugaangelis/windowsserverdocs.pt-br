---
title: Configurar o servidor de acesso remoto para VPN Always On
description: RRAS foi projetado para executar bem como um roteador e um servidor de acesso remoto; Portanto, ele dá suporte a uma ampla gama de recursos.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: a338ddfec1ed5cd0e9198f64dc4952eb591cdc1b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066986"
---
# Etapa 3. Configurar o servidor de acesso remoto para VPN Always On

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** etapa 2. Configurar a infraestrutura de servidor](vpn-deploy-server-infrastructure.md)<br>
& #187;  [ **Anterior:** etapa 4. Instalar e configurar o servidor de política de rede (NPS)](vpn-deploy-nps.md)


RRAS foi projetado para executar bem como um roteador e um servidor de acesso remoto porque ele dá suporte a uma ampla gama de recursos. Para os fins desta implantação, você precisa apenas um pequeno subconjunto desses recursos: suporte para conexões VPN IKEv2 e roteamento de rede local.

IKEv2 é uma VPN descrito na Internet Engineering Task Force solicitar para 7296 de comentários de protocolo de encapsulamento. A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se a conexão for temporariamente perdida ou se um usuário move um computador cliente de uma rede para outra, IKEv2 restaura automaticamente a conexão VPN quando a conexão de rede for restabelecida — tudo isso sem a intervenção do usuário.

Configure o servidor RRAS para dar suporte a conexões IKEv2 enquanto desabilitação protocolos não utilizados, o que reduz o volume de segurança do servidor. Além disso, configure o servidor para atribuir endereços para clientes VPN de um pool de endereço estático. Você pode atribuir conseguiu endereços de um pool ou um servidor DHCP; No entanto, usando um servidor DHCP adiciona complexidade ao design e oferece benefícios mínimo.


>[!Important]
>É importante:
>- Instale dois adaptadores de rede Ethernet no servidor físico. Se você estiver instalando o servidor VPN em uma VM, você deve criar dois externos comutadores virtuais, um para cada adaptador de rede física; e, em seguida, crie dois adaptadores de rede virtual para a VM, com cada adaptador de rede conectado a um comutador virtual.
>
>- Instale o servidor em sua rede de perímetro entre a borda e firewalls internos, com um adaptador de rede conectado à rede de perímetro externo e um adaptador de rede conectado à rede de perímetro interno.


>[!Warning]
>Antes de começar, certifique-se de habilitar IPv6 no servidor VPN. Caso contrário, não é possível estabelecer uma conexão e exibe uma mensagem de erro.

## Instalar o acesso remoto como um servidor VPN de Gateway RAS

Neste procedimento, você pode instalar a função de acesso remoto como um servidor de Gateway de RAS VPN único locatário. Para obter mais informações, consulte [Acesso Remoto](../../../Remote-Access.md).


### Instalar a função de acesso remoto usando o Windows PowerShell

1. Abra o Windows PowerShell como **administrador**.

2. Digite o comando a seguir e pressione **ENTER**:

   `Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools`

   Após a conclusão da instalação, a seguinte mensagem aparece no Windows PowerShell.
    
   | Sucesso | Reinicialização necessária | Código de saída | Resultado de recurso                             |
   |---------|----------------|-----------|--------------------------------------------|
   | True    | Não             | Sucesso   | {Kit de administração do Gerenciador de Conexão de RAS |
   ---

### Instalar a função de acesso remoto usando o Gerenciador do servidor

Você pode usar o procedimento a seguir para instalar a função de acesso remoto usando o Gerenciador do servidor.

1.  No servidor VPN, no Gerenciador do servidor, clique em **Gerenciar** e clique em **Adicionar funções e recursos**. <p>O assistente Adicionar funções e recursos abre.

2.  Em antes você começar a página, clique em**Avançar**.

3.  Na página Selecionar o tipo de instalação, selecione a opção de **instalação baseado em função ou recurso** e clique em **Avançar**.

4.  Na página Select destination server, selecione a opção de **Selecionar um servidor de pool de servidores** .

5.  Em Pool de servidores, selecione o computador local e clique em **Avançar**.

6.  Na página Select server funções, em **funções**, clique em **Acesso remoto**e clique em seguida **Avançar**.

7.  Na página Selecionar recursos, clique em **Avançar**.

8.  Na página acesso remoto, clique em **Avançar**.

9.  Na página do serviço de função selecione, nos **Serviços de função**, clique em**DirectAccess e VPN (RAS)**.<p>Abre a caixa de diálogo do **Assistente Adicionar funções e recursos** .

10. Na caixa de diálogo de recursos e adicionar funções, clique em **Adicionar recursos** e clique em **Avançar**.

11. Na página função de servidor Web (IIS), clique em **Avançar**.

12. Na página de serviços de função selecione, clique em **Avançar**.

13. Na página Confirmar instalação seleções, examine suas opções e clique em **instalar**.

14. Quando a instalação for concluída, clique em **Fechar**.

## Configurar o acesso remoto como um servidor VPN

Nesta seção, você pode configurar o acesso remoto VPN para permitir conexões VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para clientes VPN autorizados conectados.

1.  No servidor VPN, no Gerenciador do servidor, clique no sinalizador de **notificações** .

2.  No menu de **tarefas** , clique em **Abrir o Assistente de Introdução**.<p>O Assistente para configurar acesso remoto é aberto. 

    >[!NOTE] 
    >O Assistente para configurar o acesso remoto pode abrir por trás do Gerenciador de servidores. Se você acha que o assistente é levando muito tempo para abrir, mover ou minimizar o Gerenciador do servidor para descobrir se o assistente é atrás dela. Caso contrário, aguarde o assistente inicializar.

3.  Clique em **implantar apenas VPN**.<p>O roteamento e acesso Microsoft Management Console (MMC) remoto abre.

4.  Clique com botão direito no servidor VPN e clique em **Configurar e ativar o roteamento e acesso remoto**.<p>O Assistente de instalação de servidor de acesso remoto e de roteamento abre.

5.  No Welcome to o roteamento e o Assistente de instalação de servidor de acesso remoto, clique em **Avançar**.

6.  Na **configuração**, clique em **Configuração personalizada**e, em seguida, clique em **Avançar**.

7.  **Configuração personalizada**, clique em **acesso VPN**e, em seguida, clique em **Avançar**.<p>Concluindo o Assistente de instalação de servidor de acesso remoto e de roteamento abre.

8.  Clique em **Concluir** para fechar o assistente e clique em **Okey** para fechar a caixa de diálogo roteamento e acesso remoto.

9.  Clique em **Iniciar o serviço** para iniciar o acesso remoto.

10. No MMC de acesso remoto, clique com botão direito no servidor VPN e clique em **Propriedades**.

11. Nas propriedades, clique na guia de **segurança** e fazer:

    a. Clique em **provedor de autenticação** e **Autenticação RADIUS**.
    
    b. Clique em **Configurar**.<p>Abre a caixa de diálogo de autenticação RADIUS.
    
    c. Clique em **Adicionar**.<p>Abre a caixa de diálogo Adicionar servidor RADIUS.
    
    d. Em **nome do servidor**, digite o nome de domínio totalmente qualificado (FQDN) do servidor NPS em sua rede corporativa/organização.<p>Por exemplo, se o nome de NetBIOS do servidor NPS é NPS1 e o nome de domínio é corp.contoso.com, digite **NPS1.corp.contoso.com**.
    
    e. Em **segredo compartilhado**, clique em **Alterar**.<p>Abre a caixa de diálogo Alterar segredo.
    
    f. No **novo segredo**, digite uma cadeia de caracteres de texto.
    
    g. **Confirmar novo segredo**, digite a mesma cadeia de caracteres de texto e clique em **Okey**.

    >[!IMPORTANT] 
    >Salve essa cadeia de caracteres de texto. Quando você configura o servidor NPS em sua rede corporativa/organização, você adicionará esse servidor VPN como um cliente RADIUS. Durante essa configuração, você usará esse mesmo segredo compartilhado para que o NPS e servidores VPN podem se comunicar.

12. Ao **Adicionar um servidor RADIUS**, revise as configurações padrão para:

    - **Tempo limite**
    
    - **Pontuação inicial**
    
    - **Port**

13. Se necessário, altere os valores para corresponder aos requisitos para seu ambiente e clique em **Okey**.<p>NAS é um dispositivo que fornece algum nível de acesso a uma rede maior. Um com infraestrutura RADIUS também é um cliente RADIUS, enviar solicitações de conexão e mensagens de estatísticas para um servidor RADIUS para autenticação, autorização e estatísticas.

14. Revise a configuração do **provedor de estatísticas**:

    | Se quiser que o...  | Então…             |
    |---------------------|-------------------|
    | Atividade de acesso remota conectada, o servidor de acesso remoto | Certifique-se de **Estatísticas do Windows** é selecionado.      |
    | NPS para realizar serviços de contabilidade para VPN   | Altere o **provedor de estatísticas** para **Estatísticas RADIUS** e, em seguida, configurar o NPS como o provedor de estatísticas. |
    ---

15. Clique na guia **IPv4** e fazer:

    a. Clique em **pool de endereço estático**.
    
    b. Clique em **Adicionar** para configurar um pool de endereços IP.<p>O pool de endereço estático deve conter endereços de rede de perímetro interno. Esses endereços são sobre a conexão de rede de face interna no servidor VPN, não a rede corporativa.
    
    c. No **endereço IP inicial**, digite o endereço IP inicial do intervalo que você deseja atribuir aos clientes VPN.
    
    d. No **endereço IP final**, digite o endereço IP final no intervalo que você deseja atribuir aos clientes VPN ou no **número de endereços**, digite o número do endereço que você deseja disponibilizar. Se você estiver usando DHCP para esta sub-rede, certifique-se de que você configure uma exclusão de endereço correspondente em seus servidores DHCP.
    
    e. (Opcional) Se você estiver usando o DHCP, clique em **adaptador**e na lista de resultados, clique no adaptador de Ethernet conectado à sua rede de perímetro interno.

16. (Opcional) *Se você estiver configurando o acesso condicional para conectividade VPN*, na lista suspensa **certificado** , em **Associação de certificado SSL**, selecione a autenticação do servidor VPN.

17. (Opcional) *Se você estiver configurando o acesso condicional para conectividade VPN*, no NPS MMC, expanda **Policies\\Network políticas** e fazer: 

    a. Direita-a política de rede de **conexões de roteamento da Microsoft e o servidor de acesso remoto** e selecione **Propriedades**.
    
    b. Selecione o **conceder acesso. Conceder acesso se a solicitação de conexão corresponde a essa política** opção.
    
    c. Em tipo de servidor de acesso de rede, selecione o **Servidor de acesso remoto (VPN-Dial-up)** na lista suspensa.

3.  De roteamento e o MMC de acesso remoto, clique com botão direito **portas** e, em seguida, clique em **Propriedades**. <p>Abre a caixa de diálogo de propriedades de portas.

4.  Clique em **Miniporta WAN (SSTP)** e clique em **Configurar**. Configurar dispositivo - Miniporta WAN (SSTP) é aberta.

    a. Desmarque as caixas de seleção **conexões de acesso remoto (somente de entrada)** e **conexões de roteamento de discagem por demanda (entradas e saídas)** .
    
    b. Clique em **OK**.

5.  Clique em **Miniporta de rede remota (L2TP)** e clique em **Configurar**. Configurar dispositivo – Miniporta de rede remota (L2TP) é aberta.

    a. No **número máximo de portas**, digite o número de portas para corresponder ao número máximo de conexões VPN simultâneas que você deseja dar suporte.
    
    b. Clique em **OK**.

6.  Clique em **Miniporta de rede (PPTP)** e clique em **Configurar**. Configurar dispositivo – Miniporta de rede (PPTP) é aberta.

    a. No **número máximo de portas**, digite o número de portas para corresponder ao número máximo de conexões VPN simultâneas que você deseja dar suporte.
    
    b. Clique em **OK**.
    
7. Clique em **Miniporta WAN (IKEv2)** e clique em **Configurar**. Configurar dispositivo - Miniporta WAN (IKEv2) é aberta.

    a. No **número máximo de portas**, digite o número de portas para corresponder ao número máximo de conexões VPN simultâneas que você deseja dar suporte.
    
    b. Clique em **OK**.

7.  Se solicitado, clique em **Sim** para confirmar a reinicialização do servidor e clique em **Fechar** para reiniciar o servidor.

## Próximas etapas
[Etapa 4. Instalar e configurar o servidor de política de rede (NPS)](vpn-deploy-nps.md): nesta etapa, você instala o servidor de política de rede (NPS) usando o Windows PowerShell ou o Server Manager adicionar funções e recursos do assistente. Você também configura o NPS para manipular todas as obrigações de estatísticas de conexão, autorização e autenticação solicitações que ele recebe do servidor VPN.





---
