---
title: Etapa 3 configurar o servidor de acesso remoto para OTP
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d588d9b8675dad8bffc9e020032bc66bebf503b0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313672"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Etapa 3 configurar o servidor de acesso remoto para OTP

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Depois que o servidor RADIUS tiver sido configurado com tokens de distribuição de software, as portas de comunicação estiverem abertas, um segredo compartilhado foi criado, as contas de usuário correspondentes a Active Directory foram criadas no servidor RADIUS e o servidor de acesso remoto tem sido configurado como um agente de autenticação RADIUS, o servidor de acesso remoto precisa ser configurado para dar suporte à OTP.  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|[3,1 usuários isentos da autenticação OTP (opcional)](#BKMK_Exempt)|Se usuários específicos forem isentos do DirectAccess com autenticação OTP, siga estas etapas preliminares.|  
|[3,2 configurar o servidor de acesso remoto para dar suporte à OTP](#BKMK_Config)|No servidor de acesso remoto, atualize a configuração de acesso remoto para dar suporte à autenticação de dois fatores de OTP.|  
|[cartões inteligentes 3,3 para autorização adicional](#BKMK_Smartcard)|Informações adicionais sobre o uso de cartões inteligentes.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets de exemplo do Windows PowerShell que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="31-exempt-users-from-otp-authentication-optional"></a><a name="BKMK_Exempt"></a>3,1 usuários isentos da autenticação OTP (opcional)  
Se usuários específicos forem isentos da autenticação OTP, essas etapas deverão ser executadas antes da configuração de acesso remoto:  
  
> [!NOTE]  
> Você deve aguardar a replicação entre os domínios para ser concluída ao configurar o grupo de isenção de OTP.  
  
#### <a name="create-user-exemption-security-group"></a>Criar grupo de segurança de isenção de usuário  
  
1.  Crie um grupo de segurança em Active Directory para a isenção de OTP de finalidade.  
  
2.  Adicione todos os usuários a serem isentos da autenticação OTP para o grupo de segurança.  
  
    > [!NOTE]  
    > Certifique-se de incluir apenas contas de usuário, e não contas de computador, no grupo de segurança de isenção de OTP.  
  
## <a name="32-configure-the-remote-access-server-to-support-otp"></a><a name="BKMK_Config"></a>3,2 configurar o servidor de acesso remoto para dar suporte à OTP  
Para configurar o acesso remoto para usar a autenticação de dois fatores e a OTP com o servidor RADIUS e a implantação de certificado das seções anteriores, use as seguintes etapas:  
  
#### <a name="configure-remote-access-for-otp"></a>Configurar o acesso remoto para OTP  
  
1.  Abra o **Gerenciamento de acesso remoto** e clique em **configuração**.  
  
2.  Na janela de **instalação do DirectAccess** , em **etapa 2 – servidor de acesso remoto**, clique em **Editar**.  
  
3.  Clique **em Avançar** três vezes e, na seção **autenticação** , selecione **autenticação de dois fatores** e **usar OTP**e verifique se a opção **usar certificados de computador** está marcada.  
  
    > [!NOTE]  
    > Depois que a OTP tiver sido habilitada no servidor de acesso remoto, se você desabilitar a OTP desmarcando **usar OTP**, as extensões ISAPI e CGI serão desinstaladas no servidor.  
  
4.  Se o suporte do Windows 7 for necessário, marque a caixa de seleção **habilitar computadores cliente do Windows 7 para se conectar via DirectAccess** . Observação: conforme discutido na seção de planejamento, os clientes do Windows 7 devem ter o DCA 2,0 instalado para dar suporte ao DirectAccess com OTP.  
  
5.  Clique em **Avançar**.  
  
6.  Na seção **servidor RADIUS de OTP** , clique duas vezes no campo **nome do servidor** em branco.  
  
7.  Na caixa de diálogo **Adicionar um servidor RADIUS** , digite o nome do servidor RADIUS no campo **nome do servidor** . Clique em **alterar** ao lado do campo **segredo compartilhado** e digite a mesma senha que você usou ao configurar o servidor RADIUS nos campos **novo segredo** e **confirmar novo segredo** . Clique em **OK** duas vezes e clique em **Avançar**.  
  
    > [!NOTE]  
    > Se o servidor RADIUS estiver em um domínio diferente do servidor de acesso remoto, o campo nome do **servidor** deverá especificar o FQDN do servidor RADIUS.  
  
8.  Na seção **servidores de AC de OTP** , selecione os servidores de AC a serem usados para o registro de certificados de autenticação de cliente de OTP e clique em **Adicionar**. Clique em **Avançar**.  
  
9. Na seção **modelos de certificado de OTP** , clique em **procurar** para selecionar o modelo de certificado usado para o registro de certificados emitidos para autenticação OTP.  
  
    > [!NOTE]  
    > O modelo de certificado para certificados OTP emitidos pela AC corporativa deve ser configurado sem a opção "não incluir informações de revogação em certificados emitidos". Se essa opção for selecionada durante a criação do modelo de certificado, os computadores cliente de OTP não conseguirão fazer logon corretamente.  
  
    Clique em **procurar** para selecionar um modelo de certificado usado para registrar o certificado usado pelo servidor de acesso remoto para assinar solicitações de registro de certificado OTP. Clique em **OK**. Clique em **Avançar**.  
  
10. Se a isenção de usuários específicos do DirectAccess com OTP for necessária, na seção **isenções de OTP** , selecione **não exigir que os usuários no grupo de segurança especificado se autentiquem usando a autenticação de dois fatores**. Clique em **grupo de segurança** e selecione o grupo de segurança que foi criado para isenções OTP.  
  
11. Na página **instalação do servidor de acesso remoto** , clique em **concluir**.  
  
12. Na janela de **instalação do DirectAccess** , em **etapa 3-servidores de infraestrutura**, clique em **Editar**.  
  
13. Clique em **Avançar** duas vezes e, na seção **Gerenciamento** , clique duas vezes no campo **servidores de gerenciamento** .  
  
14. Insira o **nome do computador** ou **endereço** do servidor de AC que está configurado para emitir certificados OTP e clique em **OK**.  
  
15. No Windows **instalação de acesso remoto** clique em **concluir**.  
  
16. Clique em **concluir** no **Assistente de especialista do DirectAccess**.  
  
17. Na caixa de diálogo **revisão de acesso remoto** , clique em **aplicar**, aguarde a atualização da política do DirectAccess e clique em **Fechar**.  
  
18. Na tela **Iniciar** , digite**PowerShell. exe**, clique com o botão direito do mouse em **PowerShell**, clique em **avançado**e clique em **Executar como administrador**. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
19. Na janela do Windows PowerShell, digite **gpupdate/force** e pressione Enter.  
  
Para configurar o acesso remoto para OTP usando comandos do PowerShell:  
  
![](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**comandos equivalentes do Windows** PowerShell  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
Para configurar o acesso remoto para usar a autenticação de dois fatores em uma implantação que atualmente usa a autenticação de certificado do computador:  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
Para configurar o acesso remoto para usar a autenticação OTP usando as seguintes configurações:  
  
-   Um servidor de OTP chamado OTP.corp.contoso.com.  
  
-   Um servidor de AC chamado APP1. Corp. contoso. com\corp-APP1-CA1.  
  
-   Um modelo de certificado chamado DAOTPLogon usado para o registro de certificados que são emitidos para autenticação OTP.  
  
-   Um modelo de certificado chamado DAOTPRA usado para registrar o certificado de autoridade de registro usado pelo servidor de acesso remoto para assinar solicitações de registro de certificado OTP.  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
Depois de executar os comandos do PowerShell, conclua as etapas 12-19 do procedimento anterior para configurar o servidor de acesso remoto para dar suporte à OTP.  
  
> [!NOTE]  
> Certifique-se de verificar se você aplicou as configurações de OTP no servidor de acesso remoto antes de adicionar um ponto de entrada.  
  
## <a name="33-smart-cards-for-additional-authorization"></a><a name="BKMK_Smartcard"></a>cartões inteligentes 3,3 para autorização adicional  
Na página autenticação da etapa 2 no assistente de instalação de acesso remoto, você pode exigir o uso de cartões inteligentes para acesso à rede interna. Quando essa opção é selecionada, o assistente de instalação de acesso remoto configura a regra de segurança de conexão IPsec para o túnel de intranet no servidor DirectAccess para exigir a autorização do modo de túnel com cartões inteligentes. A autorização do modo de túnel permite que você especifique que somente computadores ou usuários autorizados podem estabelecer um túnel de entrada.  
  
Para usar cartões inteligentes com a autorização do modo de túnel IPsec para o túnel de intranet, é necessário implantar uma PKI (Infraestrutura de Chave Pública) com a infraestrutura de cartões inteligentes.  
  
Como os clientes do DirectAccess estão usando cartões inteligentes para acesso à intranet, você também pode usar a garantia do mecanismo de autenticação, um recurso do Windows Server 2008 R2, para controlar o acesso a recursos, como arquivos, pastas e impressoras, com base em se o usuário conectado com um certificado baseado em cartão inteligente. A garantia de mecanismo de autenticação requer um nível funcional de domínio do Windows Server 2008 R2.  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Permitindo acesso para usuários com cartões inteligentes inutilizáveis  
Para permitir o acesso temporário para usuários com cartões inteligentes que são inutilizáveis, execute este procedimento:  
  
1.  Crie um grupo de segurança do Active Directory para conter as contas dos usuários que temporariamente não podem usar seus cartões inteligentes.  
  
2.  Para o objeto de Política de Grupo do servidor DirectAccess, defina as configurações de IPsec globais para a autorização do túnel IPsec e adicione o grupo de segurança Active Directory à lista de usuários autorizados.  
  
Para conceder acesso a um usuário que não pode usar o cartão inteligente, adicione temporariamente sua conta de usuário ao grupo de segurança do Active Directory. Remova a conta de usuário do grupo quando o cartão inteligente for utilizável.  
  
### <a name="under-the-covers-smart-card-authorization"></a>Nos bastidores: autorização de cartão inteligente  
A autorização de cartão inteligente funciona por meio da habilitação da autorização do modo de túnel na regra de segurança de conexão do túnel da intranet do servidor DirectAccess para uma SID (identificador de segurança) específica baseada no Kerberos. Para a autorização de cartão inteligente, essa é a SID conhecida (S-1-5-65), que é mapeada para logons baseados em cartão inteligente. Esse SID está presente em um token Kerberos do cliente DirectAccess e é conhecido como "este certificado de organização" quando configurado nas configurações de autorização do modo de túnel IPsec global.  
  
Quando você habilita a autorização de cartão inteligente na etapa 2 do assistente de instalação do DirectAccess, o assistente de instalação do DirectAccess configura a configuração de autorização do modo de túnel IPsec global com esse SID para o objeto de Política de Grupo do servidor DirectAccess. Para exibir essa configuração no snap-in Firewall do Windows com segurança avançada para o servidor DirectAccess Política de Grupo, faça o seguinte:  
  
1.  Clique com o botão direito do mouse em Firewall do Windows com Segurança Avançada e clique em Propriedades.  
  
2.  Na guia Configurações de IPsec, em autorização de túnel IPsec, clique em Personalizar.  
  
3.  Clique na guia usuários. Você deve ver o "certificado de organização do NT AUTHORITY\This" como um usuário autorizado.  
  


