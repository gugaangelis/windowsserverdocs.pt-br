---
title: Etapa 3 configurar o servidor de acesso remoto para OTP
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 093877657f19006bba2b80c10b92db1fb3b40fde
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280861"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Etapa 3 configurar o servidor de acesso remoto para OTP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois que o servidor RADIUS foi configurado com tokens de distribuição de software, as portas de comunicação são abertas, um segredo compartilhado tiver sido criado, as contas de usuário que correspondem ao Active Directory foram criadas no servidor RADIUS e o servidor de acesso remoto tem foi configurado como um agente de autenticação de RADIUS, em seguida, o servidor de acesso remoto precisa ser configurado para dar suporte a OTP.  
  
|Tarefa|Descrição|  
|----|--------|  
|[3.1 isentar os usuários da autenticação de OTP (opcional)](#BKMK_Exempt)|Se usuários específicos serão isentos do DirectAccess com autenticação OTP, em seguida, siga essas etapas preliminares.|  
|[3.2 configurar o servidor de acesso remoto para dar suporte a OTP](#BKMK_Config)|No servidor de acesso remoto, atualize a configuração de acesso remoto para dar suporte à autenticação de dois fatores OTP.|  
|[3.3 os cartões inteligentes para autorização adicional](#BKMK_Smartcard)|Informações adicionais sobre o uso de cartões inteligentes.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Exempt"></a>3.1 isentar os usuários da autenticação de OTP (opcional)  
Se usuários específicos devem ser isentos da autenticação de OTP, essas etapas devem ser executadas antes da configuração do acesso remoto:  
  
> [!NOTE]  
> Você deve aguardar a replicação entre os domínios a serem executadas para configuração do grupo de isenção de OTP.  
  
#### <a name="create-user-exemption-security-group"></a>Criar grupo de segurança de isenção do usuário  
  
1.  Crie um grupo de segurança no Active Directory para a finalidade de isenção de OTP.  
  
2.  Adicione todos os usuários sejam isentos da autenticação de OTP para o grupo de segurança.  
  
    > [!NOTE]  
    > Certifique-se de incluir apenas contas de usuário e contas de computador, no grupo de segurança de isenção de OTP.  
  
## <a name="BKMK_Config"></a>3.2 configurar o servidor de acesso remoto para dar suporte a OTP  
Para configurar o acesso remoto para usar a autenticação de dois fatores e OTP com o servidor RADIUS e a implantação de certificado das seções anteriores, use as seguintes etapas:  
  
#### <a name="configure-remote-access-for-otp"></a>Configurar o acesso remoto para OTP  
  
1.  Abra **gerenciamento de acesso remoto** e clique em **configuração**.  
  
2.  No **instalação do DirectAccess** janela, em **etapa 2: servidor de acesso remoto**, clique em **editar**.  
  
3.  Clique em **próxima** três vezes e, nas **autenticação** seção selecionar ambas **autenticação de dois fatores** e **usar OTP**e certifique-se que **usar certificados de computador** é verificada.  
  
    > [!NOTE]  
    > Após ter sido habilitada OTP no servidor de acesso remoto, se você desabilitar a OTP, desmarcando **usar OTP**, serão desinstaladas as extensões ISAPI e CGI no servidor.  
  
4.  Se for necessário o suporte do Windows 7, selecione a **computadores cliente de habilitar o Windows 7 para se conectarem via DirectAccess** caixa de seleção. Observação: Conforme discutido na seção de planejamento, os clientes do Windows 7 devem ter DCA 2.0 instalados para dar suporte a DirectAccess com a OTP.  
  
5.  Clique em **Avançar**.  
  
6.  No **servidor do RADIUS OTP** seção, clique duas vezes o espaço em branco **nome do servidor** campo.  
  
7.  No **adicionar um servidor RADIUS** caixa de diálogo, digite o nome do servidor RADIUS na **nome do servidor** campo. Clique em **alteração** lado a **segredo compartilhado** campo e, em seguida, digite a mesma senha que você usou ao configurar o servidor RADIUS no **novo segredo** e  **Confirmar segredo novo** campos. Clique em **Okey** duas vezes e clique em **próxima**.  
  
    > [!NOTE]  
    > Se o servidor RADIUS estiver em um domínio diferente do servidor de acesso remoto, em seguida, a **nome do servidor** campo deve especificar o FQDN do servidor RADIUS.  
  
8.  No **servidores CA OTP** seção Selecione os servidores de autoridade de certificação a ser usado para o registro de certificados de autenticação de cliente OTP e, em seguida, clique em **Add**. Clique em **Avançar**.  
  
9. No **modelos de certificado de OTP** seção clique **procurar** para selecionar o modelo de certificado usado para registro de certificados emitidos para autenticação OTP.  
  
    > [!NOTE]  
    > O modelo de certificado para certificados OTP emitidos pela autoridade de certificação corporativa deve ser configurado sem a opção "Não incluem informações de revogação em certificados emitidos". Se essa opção for selecionada durante a criação do modelo de certificado, os computadores cliente OTP falhará ao fazer logon corretamente.  
  
    Clique em **procurar** para selecionar um modelo de certificado usado para registrar o certificado usado pelo servidor de acesso remoto para assinar as solicitações de registro de certificado OTP. Clique em **OK**. Clique em **Avançar**.  
  
10. Se a isenção de usuários específicos do DirectAccess com OTP for necessária, em seguida, no **isenções de OTP** seção Selecione **não exigem que os usuários no grupo de segurança especificado para se autenticar usando autenticação de dois fatores** . Clique em **grupo de segurança** e selecione o grupo de segurança que foi criado para isenções de OTP.  
  
11. Sobre o **configuração do servidor de acesso remoto** página, clique em **concluir**.  
  
12. No **instalação do DirectAccess** janela, em **etapa 3: servidores de infraestrutura**, clique em **editar**.  
  
13. Clique em **próxima** duas vezes e, na **Management** seção clique duas vezes no **servidores de gerenciamento** campo.  
  
14. Insira o **nome do computador** ou **endereço** do servidor da autoridade de certificação que está configurado para emitir certificados OTP e, em seguida, clique em **Okey**.  
  
15. No **configuração de acesso remoto** windows clique **concluir**.  
  
16. Clique em **concluir** sobre o **Assistente do DirectAccess especialista**.  
  
17. Sobre o **revisão de acesso remoto** clique da caixa de diálogo **aplicar**, aguarde até que a diretiva do DirectAccess a ser atualizado e clique em **fechar**.  
  
18. Sobre o **iniciar** tela, digite**powershell.exe**, clique com botão direito **powershell**, clique em **avançado**e clique em **executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
19. Na janela do Windows PowerShell, digite **gpupdate /force** e pressione ENTER.  
  
Para configurar o acesso remoto para OTP usando comandos do PowerShell:  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**comandos equivalentes do Windows PowerShell**  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para configurar o acesso remoto para usar a autenticação de dois fatores em uma implantação que atualmente usa a autenticação de certificado do computador:  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
Para configurar o acesso remoto para usar a autenticação de OTP usando as seguintes configurações:  
  
-   Um servidor de OTP chamado OTP.corp.contoso.com.  
  
-   A CA server named APP1.corp.contoso.com\corp-APP1-CA1.  
  
-   Um modelo de certificado denominado DAOTPLogon usado para registro de certificados emitidos para autenticação OTP.  
  
-   Um modelo de certificado denominado DAOTPRA usado para registrar o certificado de autoridade de registro usado pelo servidor de acesso remoto para assinar as solicitações de registro de certificado OTP.  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
Depois de executar os comandos do PowerShell conclua as etapas 12-19 do procedimento anterior para configurar o servidor de acesso remoto para dar suporte a OTP.  
  
> [!NOTE]  
> Certifique-se de verificar que você aplicou as configurações de OTP no servidor de acesso remoto antes de adicionar um ponto de entrada.  
  
## <a name="BKMK_Smartcard"></a>3.3 os cartões inteligentes para autorização adicional  
Na página de autenticação da etapa 2 no Assistente de configuração de acesso remoto, você pode exigir o uso de cartões inteligentes para acesso à rede interna. Quando essa opção é selecionada, o Assistente de configuração de acesso remoto configura a regra de segurança de conexão IPsec para o túnel de intranet no servidor do DirectAccess para exigir autorização do modo de túnel com cartões inteligentes. Autorização do modo de túnel permite que você especifique que somente computadores autorizados ou os usuários podem estabelecer um túnel de entrada.  
  
Para usar cartões inteligentes com a autorização do modo de túnel IPsec para o túnel de intranet, é necessário implantar uma PKI (Infraestrutura de Chave Pública) com a infraestrutura de cartões inteligentes.  
  
Como os clientes DirectAccess usando cartões inteligentes para o acesso à intranet, você também pode usar garantia do mecanismo de autenticação, um recurso do Windows Server 2008 R2, para controlar o acesso a recursos, como arquivos, pastas e impressoras, com base na possibilidade de usuário conectado com um certificado baseado em cartão inteligente. Garantia do mecanismo de autenticação requer um nível funcional de domínio do Windows Server 2008 R2.  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Permitindo acesso para usuários com cartões inteligentes inutilizáveis  
Para permitir o acesso temporário para usuários com cartões inteligentes que são inutilizáveis, execute este procedimento:  
  
1.  Crie um grupo de segurança do Active Directory para conter as contas dos usuários que temporariamente não podem usar seus cartões inteligentes.  
  
2.  Para o objeto de diretiva de grupo do servidor DirectAccess, definir as configurações de IPsec globais para autorização de túnel IPsec e adicione o grupo de segurança do Active Directory à lista de usuários autorizados.  
  
Para conceder acesso a um usuário que não pode usar o cartão inteligente, adicione temporariamente sua conta de usuário ao grupo de segurança do Active Directory. Remova a conta de usuário do grupo quando o cartão inteligente for utilizável.  
  
### <a name="under-the-covers-smart-card-authorization"></a>Nos bastidores: autorização de cartão inteligente  
A autorização de cartão inteligente funciona por meio da habilitação da autorização do modo de túnel na regra de segurança de conexão do túnel da intranet do servidor DirectAccess para uma SID (identificador de segurança) específica baseada no Kerberos. Para a autorização de cartão inteligente, essa é a SID conhecida (S-1-5-65), que é mapeada para logons baseados em cartão inteligente. Esse SID está presente no token Kerberos de um cliente DirectAccess e é conhecido como "Certificado desta organização" Quando configurada no IPsec global configurações de autorização do modo de túnel.  
  
Quando você habilita a autorização de cartão inteligente na etapa 2 do Assistente de instalação do DirectAccess, o Assistente de instalação do DirectAccess configura a configuração de autorização do modo de túnel IPsec global com essa SID para o objeto de diretiva de grupo do servidor DirectAccess. Para exibir essa configuração no Firewall do Windows com o snap-in segurança avançada para o objeto de diretiva de grupo do servidor DirectAccess, faça o seguinte:  
  
1.  Clique com botão direito no Firewall do Windows com segurança avançada e, em seguida, clique em Propriedades.  
  
2.  Na guia Configurações de IPsec, na autorização de túnel IPsec, clique em Personalizar.  
  
3.  Clique na guia usuários. Você deve ver o "NT Authority\certificado desta organização" como um usuário autorizado.  
  


