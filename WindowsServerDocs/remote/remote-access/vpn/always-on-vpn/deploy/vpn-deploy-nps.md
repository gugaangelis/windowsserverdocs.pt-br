---
title: Instalar e configurar o servidor NPS
description: Processamento de servidor NPS de solicitações de conexão que são enviados pelo servidor VPN verifica se o usuário tem permissão para se conectar, a identidade do usuário e registra os aspectos da solicitação de conexão que você escolheu quando configurado estatísticas RADIUS no NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca53ef28497a78f264c60ac1132f721fb6e01c15
ms.sourcegitcommit: 9c142ad4d4321baf328707f29fa104247ff5149a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2019
ms.locfileid: "9035762"
---
# Etapa 4. Instalar e configurar o servidor de política de rede (NPS)

>   Aplicável a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Próximo:** etapa 3. Configurar o servidor de acesso remoto para sempre em VPN](vpn-deploy-ras.md)<br>
& #187;  [ **Próximo:** etapa 5. Definir as configurações de Firewall e DNS](vpn-deploy-dns-firewall.md)


Nesta etapa, você instalar o servidor de política de rede (NPS) para processamento de solicitações de conexão que são enviados pelo servidor VPN:

- Execute a autorização para verificar se o usuário tem permissão para se conectar.
- Executando a autenticação para verificar a identidade do usuário.
- Realizando contabilidade para registrar os aspectos da solicitação de conexão que você escolheu quando configurado estatísticas RADIUS no NPS.

As etapas nesta seção permitem que você conclua os seguintes itens:

1.  No computador ou VM que planejados para o servidor NPS e instalado em sua organização ou rede corporativa, você pode instalar o NPS.

   >[!TIP] 
   >Se você já tiver um ou mais servidores NPS em sua rede, você não precisa executar a instalação do servidor NPS - em vez disso, você pode usar este tópico para atualizar a configuração de um servidor NPS existente.

>[!NOTE]  
Você não pode instalar o serviço do servidor de políticas de rede no Windows Server Core.

2.  No servidor NPS organização/corporativa, você pode configurar o NPS para executar como um servidor RADIUS que processa as solicitações de conexão recebidas do servidor VPN.

## Instalar o NPS (Servidor de Políticas de Rede)

Este procedimento, você instala o NPS usando o Windows PowerShell ou o Server Manager adicionar funções e recursos do assistente. NPS é um serviço de função da função de servidor Serviços de acesso e diretiva de rede.

>[!TIP] 
>Por padrão, o NPS escuta o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Quando você instala o NPS e habilitar o Firewall do Windows com segurança avançada, exceções de firewall para essas portas são criadas automaticamente para o tráfego IPv4 e IPv6. Se os servidores de acesso de rede são configurados para enviar o tráfego RADIUS pelas portas que não sejam esses padrões, remova as exceções criadas no Firewall do Windows com segurança avançada durante a instalação do NPS e criar exceções para as portas de que você usa para Tráfego RADIUS.

**Procedimento para o Windows PowerShell:**

Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite o comando a seguir e pressione ENTER.

`Install-WindowsFeature NPAS -IncludeManagementTools`

**Procedimento para o Gerenciador do servidor:**

1.  No Gerenciador do servidor, clique em **Gerenciar**e clique em **Adicionar funções e recursos**. <p>Abre o assistente Adicionar funções e recursos.

2.  Em antes de começar, clique em **Avançar**.

    >[!NOTE] 
    >A página **Antes de começar** do assistente Adicionar funções e recursos não será exibida se anteriormente você tiver selecionado **Ignorar esta página, por padrão** quando executou o assistente Adicionar funções e recursos.

1.  Selecione o tipo de instalação, certifique-se de **instalação baseado em função ou recurso** é selecionado e clique em **Avançar**.

2.  Em Selecionar servidor de destino, certifique-se de **Selecionar um servidor de pool de servidores** é selecionado.

3.  No Pool de servidores, verifique se o computador local está selecionado e clique em **Avançar**.

4.  Em Selecionar funções de servidor em **funções**, selecione **Network Policy and Access Services**. Uma caixa de diálogo será aberto perguntando se ele deve adicionar os recursos necessários para serviços de acesso e diretiva de rede.

5.  **Adicionar recursos**e clique em **Avançar**

6.  Selecione recursos, clique em **Avançar**, em serviços de acesso e diretiva de rede, examine as informações fornecidas e clique em **Avançar**.

7.  Em serviços de função selecione, clique em **Servidor de políticas de rede**.

8.  Para os recursos necessários para o servidor de políticas de rede, clique em **Adicionar recursos** e clique em **Avançar**.

9.  Em Confirmar seleções de instalação, clique em **reiniciar o servidor de destino automaticamente se necessário**.

10. Clique em **Sim** para confirmar a selecionado e, em seguida, clique em **instalar**. <p>A página de progresso da instalação exibe o status durante o processo de instalação. Quando o processo for concluído, a mensagem "instalação bem-sucedida em *nome_do_computador"* é exibida, onde *ComputerName* é o nome do computador no qual você instalou o servidor de políticas de rede.

11. Clique em **Fechar**.

## Configurar o NPS

Depois de instalar o NPS, você configura o NPS para manipular todos os autenticação, autorização, e obrigações contabilidade para conexão solicitarem que ele recebe do servidor VPN.

### Registrar o servidor NPS no Active Directory

Neste procedimento, você deve registrar um servidor no Active Directory para que ele tenha permissão para acessar informações de conta de usuário durante o processamento de solicitações de conexão.

**Procedimento:**

1.  No Gerenciador do servidor, clique em **Ferramentas**e, em seguida, clique em **Servidor de políticas de rede**. Abre o console do NPS.

2.  No console do NPS, clique com botão direito **NPS (Local)** e clique em **Registrar servidor no Active Directory** para selecioná-lo.<p>Abre a caixa de diálogo do servidor de políticas de rede.

3.  Na caixa de diálogo servidor de políticas de rede, clique duas vezes em **Okey** .

Para métodos alternativos de registro de NPS, consulte [registrar um servidor NPS em um domínio do Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### Configurar a contabilização do Servidor de Políticas de Rede

Neste procedimento, configure a política servidor contabilidade de rede usando um dos seguintes tipos de registro em log:

-   **Log de eventos**. Usado principalmente para auditoria e solucionar problemas de tentativas de conexão. Você pode configurar Obtendo as propriedades do servidor NPS no console do NPS de log de eventos do NPS.

-   **Registro em log solicitações de estatísticas para um arquivo local e autenticação do usuário**. Usado principalmente para fins de análise e faturamento de conexão. Também usado como uma ferramenta de investigação de segurança porque ele fornece um método de controle a atividade de um usuário mal-intencionado após um ataque. Você pode configurar o registro em log de arquivo local usando o Assistente de configuração de contabilização.

-   **Registro em log solicitações de estatísticas para um banco de dados compatível com XML do Microsoft SQL Server e autenticação do usuário**. Usado para permitir que vários servidores que executam NPS ter uma fonte de dados. Também fornece as vantagens de usar um banco de dados relacional. Você pode configurar o registro em log do SQL Server usando o Assistente de configuração de contabilização.

Para configurar o servidor de política de rede estatísticas, consulte [Configurar política servidor contabilidade de rede](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### Adicione o servidor VPN como um cliente RADIUS

Na seção de [Configurar o servidor de acesso remoto para VPN sempre ativa](vpn-deploy-ras.md) , você instalou e configurou o servidor VPN. Durante a configuração do servidor VPN, você adicionou um segredo compartilhado RADIUS no servidor VPN. 

Neste procedimento, você pode usar a mesma cadeia de caracteres de texto secreta compartilhada para configurar o servidor VPN como um cliente RADIUS no NPS. Use a cadeia de caracteres de texto mesmo que você usou no servidor VPN, ou falha de comunicação entre o servidor NPS e o servidor VPN.

>[!IMPORTANT] 
>Quando você adiciona um novo servidor acesso de rede (servidor VPN, ponto de acesso sem fio, comutador de autenticação ou servidor dial-up) para sua rede, você deve adicionar o servidor como um cliente RADIUS no NPS para que o NPS está ciente e podem se comunicar com o servidor de acesso de rede.

**Procedimento:**

1.  No servidor NPS, no console do NPS, clique duas vezes em **clientes e servidores RADIUS**.

2.  Clique com botão direito **Clientes RADIUS** e clique em **novo**. Abre a caixa de diálogo Novo cliente RADIUS.

3.  Verifique se a caixa de seleção **Habilitar este cliente RADIUS** é selecionada.

4.  **Nome amigável**, insira um nome de exibição para o servidor VPN.

5.  No **endereço (IP ou DNS)**, digite o endereço IP do NAS ou FQDN.<p>Se você inserir o FQDN, clique em **Verificar** se você deseja verificar se o nome está correto e é mapeado para um endereço IP válido.

6.  Em **segredo compartilhado**, fazer:

    1.  Certifique-se de que **Manual** é selecionado.

    2.  Insira a cadeia de caracteres de texto forte que você inseriu também no servidor VPN.

    3.  Digite novamente o segredo compartilhado em confirmar o segredo compartilhado.

7.  Clique em **OK**. O servidor VPN aparece na lista de clientes RADIUS configurados no servidor NPS.

## Configurar o NPS como um RAIO para conexões VPN

Este procedimento, você configura o NPS como um servidor RADIUS na rede da organização. No NPS, você deve definir uma política que permite que somente usuários em um grupo específico para acessar a rede corporativa/organização por meio do servidor VPN - e, em seguida, somente ao usar um certificado de usuário válidas em uma solicitação de autenticação PEAP.

**Procedimento:**

1.  No console do NPS, na configuração padrão, certifique-se de que o **servidor RADIUS para conexões VPN ou Dial-Up** está selecionado.

2.  Clique em **Configurar VPN ou Dial-Up**.<p>Abre o Assistente para configurar VPN ou Dial-Up.

3.  Clique em **conexões de rede Virtual privada (VPN)** e clique em **Avançar**.

4.  No Dial-Up especificar ou servidor VPN, em clientes RADIUS, selecione o nome do servidor VPN que você adicionou na etapa anterior.<p>Por exemplo, se seu nome de NetBIOS do servidor VPN é RAS1, clique em **RAS1**.

5.  Clique em **Avançar**.

6.  Configurar métodos de autenticação, conclua as seguintes etapas:

    1.  Desmarque a caixa de seleção de **Autenticação criptografada da Microsoft versão 2 (MS-CHAPv2)** .

    2.  Clique na caixa de seleção **Extensible Authentication Protocol** para selecioná-lo.

    3.  No tipo (com base no método de configuração de acesso e rede), clique em **Microsoft: EAP protegido (PEAP)** e clique em **Configurar**.<p>Abre a caixa de diálogo Editar propriedades de EAP protegido.

    4.  Clique em **Remover** para remover o tipo EAP senha segura (EAP-MSCHAP v2).

    5.  Clique em **Adicionar**. Abre a caixa de diálogo Adicionar EAP.

    6.  Clique em **cartão inteligente ou outro certificado**e clique em **Okey**.

    7.  Clique **Okey** para fechar editar propriedades de EAP protegido.

7.  Clique em **Avançar**.

8.  Em especificar grupos de usuários, conclua as seguintes etapas:

    1.  Clique em **Adicionar**. Abre a caixa de diálogo Selecionar usuários, computadores, contas de serviço ou grupos.

    2.  Digite **Os usuários da VPN** e clique em **Okey**.

    3.  Clique em **Avançar**.

9.  Em especificar filtros de IP, clique em **Avançar**.

10. Em especificar configurações de criptografia, clique em **Avançar**. Não faça as alterações.<p>Essas configurações se aplicam somente às conexões de criptografia de ponto a ponto da Microsoft (MPPE), que não dá suporte a esse cenário.

11. Em Especifique um nome de território, clique em **Avançar**.

12. Clique em **Concluir** para fechar o assistente.

## Registro automático de certificado de servidor NPS

Neste procedimento, você atualizar a política de grupo no servidor NPS local manualmente. Quando as atualizações de diretiva de grupo, se o registro automático de certificado está configurado e funcionando corretamente, o computador local é registrados automaticamente um certificado pela autoridade de certificação (CA).

>[!NOTE]  
>Política de grupo são atualizados automaticamente quando você reiniciar o computador membro do domínio, ou quando um usuário faz logon em um computador membro do domínio. Além disso, a política de grupo atualiza periodicamente. Por padrão, essa atualização periódica ocorre a cada 90 minutos com um deslocamento aleatório de até 30 minutos.

A associação ao grupo **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

**Procedimento:**

1.  No NPS, abra o Windows PowerShell.

2.  No prompt do Windows PowerShell, digite **gpupdate**e pressione ENTER.

## Próximas etapas
[Etapa 5. Definir as configurações de DNS e o firewall para VPN Always On](vpn-deploy-dns-firewall.md): nesta etapa, você instala o servidor de política de rede (NPS) usando o Windows PowerShell ou o Server Manager adicionar funções e recursos do assistente. Você também configurar o NPS para manipular todas as obrigações de estatísticas de conexão, autorização e autenticação solicitações que ele recebe do servidor VPN.



---

