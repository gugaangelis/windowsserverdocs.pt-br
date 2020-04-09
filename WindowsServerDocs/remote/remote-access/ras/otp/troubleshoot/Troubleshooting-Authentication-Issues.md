---
title: Solução de problemas de autenticação
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e3aa3bf89a73f944b2922fcf06ac60df2874a221
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853649"
---
# <a name="troubleshooting-authentication-issues"></a>Solução de problemas de autenticação

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações de solução de problemas relacionados a problemas que os usuários podem ter ao tentar se conectar ao DirectAccess usando a autenticação de OTP. Os eventos relacionados à OTP DirectAccerss são registrados no computador cliente no Visualizador de Eventos em **logs de aplicativos e serviços/Microsoft/Windows/OtpCredentialProvider**. Verifique se esse log está habilitado ao solucionar problemas com a OTP do DirectAccess.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>Falha ao acessar a AC que emite certificados OTP  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Falha no registro do certificado OTP para o usuário <username> no servidor de CA < CA_name >, falha na solicitação, possíveis motivos para a falha: o nome do servidor de AC não pode ser resolvido, o servidor de AC não pode ser acessado pelo primeiro túnel do DirectAccess ou a conexão com o servidor de AC não pode ser estabelecida.  
  
**Causa**  
  
O usuário forneceu uma senha válida de uso único e o servidor DirectAccess assinou a solicitação de certificado; no entanto, o computador cliente não pode entrar em contato com a AC que emite certificados OTP para concluir o processo de registro.  
  
**Solução**  
  
No servidor DirectAccess, execute os seguintes comandos do Windows PowerShell:  
  
1.  Obtenha a lista de autoridades de certificação de emissão de OTP configuradas e verifique o valor de ' CAServer ': `Get-DAOtpAuthentication`  
  
2.  Verifique se as CAs estão configuradas como servidores de gerenciamento: `Get-DAMgmtServer -Type All`  
  
3.  Verifique se o computador cliente estabeleceu o túnel de infraestrutura: no console do Windows Firewall com segurança avançada, expanda **associações de monitoramento/segurança**, clique em **modo principal**e verifique se as associações de segurança IPSec aparecem com os endereços remotos corretos para a configuração do DirectAccess.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problemas de conectividade do servidor DirectAccess  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente)  
  
Um dos seguintes erros:  
  
-   Não é possível estabelecer uma conexão com o servidor de acesso remoto < DirectAccess_server_hostname > usando o caminho base < OTP_authentication_path > e a porta < OTP_authentication_port >. Código de erro: < internal_error_code >.  
  
-   As credenciais do usuário não podem ser enviadas ao servidor de acesso remoto < DirectAccess_server_hostname > usando o caminho base < OTP_authentication_path > e a porta < OTP_authentication_port >. Código de erro: < internal_error_code >.  
  
-   Uma resposta não foi recebida do servidor de acesso remoto < DirectAccess_server_hostname > usando o caminho base < OTP_authentication_path > e a porta < OTP_authentication_port >. Código de erro: < internal_error_code >.  
  
**Causa**  
  
O computador cliente não pode acessar o servidor DirectAccess pela Internet, devido a problemas de rede ou a um servidor IIS configurado incorretamente no servidor DirectAccess.  
  
**Solução**  
  
Verifique se a conexão com a Internet no computador cliente está funcionando e certifique-se de que o serviço DirectAccess está em execução e acessível pela Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>Falha ao registrar o certificado de logon de OTP do DirectAccess  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Falha no registro de certificado da CA < CA_name >. A solicitação não foi assinada conforme o esperado pelo certificado de assinatura de OTP ou o usuário não tem permissão para se registrar.  
  
**Causa**  
  
A senha de uso único fornecida pelo usuário estava correta, mas a AC (autoridade de certificação) emissora se recusou a emitir o certificado de logon de OTP. A solicitação de certificado pode não estar corretamente assinada com o EKU correto (política de aplicativo DA autoridade de registro de OTP) ou o usuário não tem a permissão "registrar" no modelo de OTP DA DA.  
  
**Solução**  
  
Verifique se os usuários de OTP do DirectAccess têm permissão para se registrar no certificado de logon de OTP do DirectAccess e se a "política de aplicativo" apropriada está incluída no modelo de assinatura DA autoridade de registro DA OTP. Verifique também se o certificado da autoridade de registro do DirectAccess no servidor de acesso remoto é válido. Consulte 3,2 planejar o modelo de certificado de OTP e 3,3 planejar o certificado de autoridade de registro.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Certificado de conta de computador ausente ou inválido  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente).  A autenticação OTP não pode ser concluída porque o certificado do computador necessário para OTP não pode ser encontrado no repositório de certificados do computador local.  
  
**Causa**  
  
A autenticação de OTP do DirectAccess requer um certificado de computador cliente para estabelecer uma conexão SSL com o servidor DirectAccess; no entanto, o certificado do computador cliente não foi encontrado ou não é válido, por exemplo, se o certificado expirou.  
  
**Solução**  
  
Verifique se o certificado do computador existe e é válido:  
  
1.  No computador cliente, no console de certificados do MMC, para a conta do computador local, abra **pessoal/certificados**.  
  
2.  Verifique se há um certificado emitido que corresponda ao nome do computador e clique duas vezes no certificado.  
  
3.  Na caixa de diálogo **certificado** , na guia **caminho do certificado** , em **status do certificado**, verifique se ele diz "este certificado está OK".  
  
Se um certificado válido não for encontrado, exclua o certificado inválido (se ele existir) e registre-se novamente para o certificado do computador executando `gpupdate /Force` em um prompt de comando elevado ou reiniciando o computador cliente.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>AC ausente que emite certificados OTP  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). A autenticação OTP não pode ser concluída porque o servidor DA não retornou um endereço de uma AC emissora.  
  
**Causa**  
  
Não há nenhuma CAs que emita certificados OTP configurados ou todas as CAs configuradas que emitem certificados OTP não respondem.  
  
**Solução**  
  
1.  Use o comando a seguir para obter a lista de CAs que emitem certificados OTP (o nome da autoridade de certificação é mostrado em CAServer): `Get-DAOtpAuthentication`.  
  
2.  Se nenhuma CAs estiver configurada:  
  
    1.  Use o comando `Set-DAOtpAuthentication` ou o console de gerenciamento de acesso remoto para configurar as CAs que emitem o certificado de logon de OTP do DirectAccess.  
  
    2.  Aplique a nova configuração e Force os clientes a atualizar as configurações do GPO do DirectAccess executando `gpupdate /Force` em um prompt de comando elevado ou reiniciando o computador cliente.  
  
3.  Se houver CAs configuradas, verifique se elas estão online e respondendo a solicitações de registro.  
  
## <a name="misconfigured-directaccess-server-address"></a>Endereço do servidor DirectAccess configurado incorretamente  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). A autenticação OTP não pode ser concluída conforme o esperado. O nome ou endereço do servidor de acesso remoto não pode ser determinado.  Código de erro: < error_code >. As configurações do DirectAccess devem ser validadas pelo administrador do servidor.  
  
**Causa**  
  
O endereço do servidor DirectAccess não está configurado corretamente.  
  
**Solução**  
  
Verifique o endereço do servidor DirectAccess configurado usando `Get-DirectAccess` e corrija o endereço se ele estiver configurado incorretamente.  
  
Verifique se as configurações mais recentes estão implantadas no computador cliente executando `gpupdate /force` em um prompt de comando elevado ou reinicie o computador cliente.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>Falha ao gerar a solicitação de certificado de logon de OTP  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). A solicitação de certificado para autenticação de OTP não pode ser inicializada. Não é possível gerar uma chave privada ou o usuário <username> não pode acessar o modelo de certificado < OTP_template_name > no controlador de domínio.  
  
**Causa**  
  
Há duas causas possíveis para esse erro:  
  
-   O usuário não tem permissão para ler o modelo de logon de OTP.  
  
-   O computador do usuário não pode acessar o controlador de domínio devido a problemas de rede.  
  
**Solução**  
  
-   Examine a configuração de permissões no modelo de logon de OTP e certifique-se de que todos os usuários provisionados para a OTP do DirectAccess tenham permissão de "leitura".  
  
-   Verifique se o controlador de domínio está configurado como um servidor de gerenciamento e se o computador cliente pode acessar o controlador de domínio pelo túnel de infraestrutura. Consulte 3,2 planejar o modelo de certificado de OTP.  
  
## <a name="no-connection-to-the-domain-controller"></a>Nenhuma conexão com o controlador de domínio  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Não é possível estabelecer uma conexão com o controlador de domínio para a finalidade da autenticação OTP. Código de erro: < error_code >.  
  
**Causa**  
  
Há duas causas possíveis para esse erro:  
  
-   O computador do usuário não tem conectividade de rede.  
  
-   O controlador de domínio não está acessível no túnel de infraestrutura.  
  
**Solução**  
  
-   Verifique se o controlador de domínio está configurado como um servidor de gerenciamento executando o seguinte comando em um prompt do PowerShell: `Get-DAMgmtServer -Type All`.  
  
-   Verifique se o computador cliente pode acessar o controlador de domínio pelo túnel de infraestrutura.  
  
## <a name="otp-provider-requires-challengeresponse"></a>O provedor de OTP requer desafio/resposta  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). A autenticação OTP com o servidor de acesso remoto (< DirectAccess_server_name >) para o usuário (<username>) exigiu um desafio do usuário.  
  
**Causa**  
  
O provedor de OTP usado exige que o usuário forneça credenciais adicionais na forma de uma troca de desafio/resposta RADIUS, que não tem suporte da OTP do DirectAccess do Windows Server 2012.  
  
**Solução**  
  
Configure o provedor de OTP para não exigir desafio/resposta em qualquer cenário.  
  
## <a name="incorrect-otp-logon-template-used"></a>Modelo de logon de OTP incorreto usado  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). O modelo de autoridade de certificação do qual o usuário <username> solicitou um certificado não está configurado para emitir certificados OTP.  
  
**Causa**  
  
O modelo de logon de OTP do DirectAccess foi substituído e o computador cliente está tentando se autenticar usando um modelo mais antigo.  
  
**Solução**  
  
Verifique se o computador cliente está usando a configuração de OTP mais recente executando uma das seguintes opções:  
  
-   Force uma atualização de Política de Grupo executando o seguinte comando em um prompt de comando com privilégios elevados: `gpupdate /Force`.  
  
-   Reinicie o computador cliente.  
  
## <a name="missing-otp-signing-certificate"></a>Certificado de assinatura de OTP ausente  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Não é possível encontrar um certificado de assinatura de OTP. Não é possível assinar a solicitação de registro de certificado OTP.  
  
**Causa**  
  
O certificado de assinatura de OTP do DirectAccess não pode ser encontrado no servidor de acesso remoto; Portanto, a solicitação de certificado de usuário não pode ser assinada pelo servidor de acesso remoto. Não há um certificado de autenticação ou o certificado de autenticação expirou e não foi renovado.  
  
**Solução**  
  
Execute estas etapas no servidor de acesso remoto.  
  
1.  Verifique o nome do modelo de certificado de assinatura de OTP configurado executando o cmdlet do PowerShell `Get-DAOtpAuthentication` e inspecione o valor de `SigningCertificateTemplateName`.  
  
2.  Use o snap-in do MMC de certificados para garantir que um certificado válido registrado por meio desse modelo exista no computador.  
  
3.  Se esse certificado não existir, exclua o certificado expirado (se houver) e registre-se para obter um novo certificado com base neste modelo.  
  
Para criar o modelo de certificado de assinatura de OTP, consulte 3,3 planejar o certificado de autoridade de registro.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>UPN/DN ausente ou incorreto para o usuário  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente)  
  
Um dos seguintes erros:  
  
-   O <username> de usuário não pode ser autenticado com OTP. Verifique se um UPN está definido para o nome de usuário em Active Directory. Código de erro: < error_code >.  
  
-   O <username> de usuário não pode ser autenticado com OTP. Verifique se um DN está definido para o nome de usuário em Active Directory. Código de erro: < error_code >.  
  
**Erro recebido** (log de eventos do servidor)  
  
O nome de usuário <username> especificado para autenticação OTP não existe.  
  
**Causa**  
  
O usuário não tem os atributos nome principal do usuário (UPN) ou nome distinto (DN) definidos corretamente na conta de usuário; essas propriedades são necessárias para o funcionamento adequado da OTP do DirectAccess.  
  
**Solução**  
  
Use o console Active Directory usuários e computadores no controlador de domínio para verificar se ambos os atributos estão definidos corretamente para o usuário que está Autenticando.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>O certificado OTP não é confiável para logon  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Causa**  
  
A AC que emite certificados OTP não está no repositório NTAuth corporativo; Portanto, os certificados registrados não podem ser usados para logon. Isso pode ocorrer em ambientes de vários domínios e multiflorestais em que a confiança de autoridade de certificação entre domínios não é estabelecida.  
  
**Solução**  
  
Certifique-se de que o certificado da raiz da hierarquia de autoridade de certificação que emite certificados OTP esteja instalado no repositório de certificados NTAuth corporativo do domínio no qual o usuário está tentando se autenticar.  
  
## <a name="windows-could-not-verify-user-credentials"></a>O Windows não pôde verificar as credenciais do usuário  
**Cenário**. O usuário não consegue autenticar usando OTP com o erro: "falha na autenticação devido a um erro interno"  
  
**Erro recebido** (computador cliente). Algo deu errado enquanto o Windows estava verificando suas credenciais. Tente novamente ou peça ajuda ao administrador.  
  
**Causa**  
  
O protocolo de autenticação Kerberos não funciona quando o certificado de logon de OTP do DirectAccess não inclui uma CRL. O certificado de logon de OTP do DirectAccess não inclui uma CRL porque:  
  
-   O modelo de logon de OTP do DirectAccess foi configurado com a opção **não incluir informações de revogação em certificados emitidos**.  
  
-   A autoridade de certificação está configurada para não publicar CRLs.  
  
**Solução**  
  
1.  Para confirmar a causa desse erro, no console de gerenciamento de acesso remoto, na **etapa 2 servidor de acesso remoto**, clique em **Editar**e, em seguida, no assistente de **instalação do servidor de acesso remoto** , clique em **modelos de certificado OTP**. Anote o modelo de certificado usado para o registro de certificados emitidos para autenticação OTP. Abra o console da autoridade de certificação, no painel esquerdo, clique em **modelos de certificado**, clique duas vezes no certificado de logon de OTP para exibir as propriedades do modelo de certificado.  
  
    Para resolver esse problema, configure um certificado para o certificado de logon de OTP e não marque a caixa de seleção não **incluir informações de revogação em certificados emitidos** na guia **servidor** da caixa de diálogo Propriedades do modelo.  
  
2.  No servidor de AC, abra o MMC da autoridade de certificação, clique com o botão direito do mouse na AC emissora e clique em **Propriedades**. Na guia **extensões** , verifique se a publicação da CRL está configurada corretamente.  
  


