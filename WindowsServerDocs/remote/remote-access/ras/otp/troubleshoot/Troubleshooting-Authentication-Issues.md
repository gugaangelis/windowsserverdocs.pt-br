---
title: Solução de problemas de autenticação
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff1dd94db3e433235a87fd6809459283fc439d0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855737"
---
# <a name="troubleshooting-authentication-issues"></a>Solução de problemas de autenticação

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações sobre solução de problemas relacionados a problemas que os usuários podem ter ao tentar se conectar ao usando autenticação OTP do DirectAccess. OTP DirectAccerss relacionados a eventos são registrados no computador cliente no Visualizador de eventos sob **aplicativos e serviços Logs/Microsoft/Windows/OtpCredentialProvider**. Certifique-se de que esse log é habilitado ao solucionar problemas com OTP do DirectAccess.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>Falha ao acessar a autoridade de certificação que emite certificados OTP  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Registro de certificado OTP para usuário <username> falhou no servidor de autoridade de certificação < CA_name >, de solicitação com falha, possíveis motivos da falha: Nome do servidor de autoridade de certificação não pode ser resolvido, o servidor de autoridade de certificação não pode ser acessado por meio do túnel do DirectAccess primeiro ou não é possível estabelecer a conexão com o servidor de autoridade de certificação.  
  
**Causa**  
  
O usuário forneceu uma senha única válida e o servidor DirectAccess conectado a solicitação de certificado; No entanto, o computador cliente não pode contatar a autoridade de certificação que emite certificados OTP para concluir o processo de registro.  
  
**Solução**  
  
No servidor do DirectAccess, execute os seguintes comandos do Windows PowerShell:  
  
1.  Obter a lista de CAs de emissão de OTP configurado e verificar o valor de 'CAServer': `Get-DAOtpAuthentication`  
  
2.  Certifique-se de que as autoridades de certificação estão configurados como um servidor de gerenciamento: `Get-DAMgmtServer -Type All`  
  
3.  Certifique-se de que o computador cliente tenha estabelecido o túnel de infraestrutura: No Firewall do Windows com console de segurança avançada, expanda **associações de segurança/monitoramento**, clique em **modo principal**e certifique-se de que as associações de segurança IPsec aparecem com o remoto correto endereços para sua configuração do DirectAccess.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problemas de conectividade do servidor DirectAccess  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente)  
  
Um dos seguintes erros:  
  
-   Não é possível estabelecer uma conexão ao servidor de acesso remoto < DirectAccess_server_hostname > usando < OTP_authentication_path > do caminho base e a porta < OTP_authentication_port >. Código de erro: < internal_error_code >.  
  
-   As credenciais do usuário não podem ser enviadas ao servidor de acesso remoto < DirectAccess_server_hostname > usando < OTP_authentication_path > do caminho base e a porta < OTP_authentication_port >. Código de erro: < internal_error_code >.  
  
-   Uma resposta não foi recebida do servidor de acesso remoto < DirectAccess_server_hostname > usando < OTP_authentication_path > do caminho base e a porta < OTP_authentication_port >. Código de erro: < internal_error_code >.  
  
**Causa**  
  
O computador cliente não é possível acessar o servidor DirectAccess na Internet, devido a qualquer um dos problemas de rede ou para um servidor IIS mal configurado no servidor DirectAccess.  
  
**Solução**  
  
Certifique-se de que a conexão de Internet no computador cliente está funcionando e certifique-se de que o serviço de DirectAccess está em execução e acessível pela Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>Falha ao registrar o certificado de logon de OTP do DirectAccess  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Falha no registro de certificado de autoridade de certificação < CA_name >. A solicitação não estava assinada conforme o esperado, o certificado de autenticação de OTP ou o usuário não tem permissão para registrar.  
  
**Causa**  
  
A senha de uso único fornecida pelo usuário estava correta, mas recusou-se a autoridade de certificação emissora (CA) emitir o certificado de logon OTP. A solicitação de certificado não pode ser assinada corretamente com o EKU correto (política de aplicativo de autoridade de registro OTP) ou o usuário não tem a permissão "Registrar" no modelo DA OTP.  
  
**Solução**  
  
Certifique-se de que os usuários de OTP do DirectAccess tem permissão para registrar o certificado de logon de OTP do DirectAccess e se a adequado "política de aplicativo" está incluída na autoridade de registro DA OTP modelo de assinatura. Além disso, certifique-se de que o certificado de autoridade de registro do DirectAccess no servidor de acesso remoto é válido. Consulte o modelo de certificado OTP de planejar de 3.2 e 3.3 planejar o certificado de autoridade de registro.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Certificado de conta de computador ausente ou inválido  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente).  Autenticação de OTP não pode ser concluída porque o certificado de computador necessário para a OTP não foi encontrado no repositório de certificados do computador local.  
  
**Causa**  
  
Autenticação de OTP do DirectAccess requer um certificado de computador cliente para estabelecer uma conexão SSL com o servidor DirectAccess; No entanto, o certificado do computador cliente não foi encontrado ou não é válido, por exemplo, se o certificado expirou.  
  
**Solução**  
  
Certifique-se de que o certificado de computador existe e é válido:  
  
1.  No computador cliente, no console de certificados do MMC, para a conta de computador Local, abra **Personal/Certificates**.  
  
2.  Certifique-se de que há um certificado emitido que corresponde ao nome do computador e clique duas vezes no certificado.  
  
3.  Sobre o **certificado** caixa de diálogo a **caminho do certificado** guia, em **status do certificado**, certifique-se de que ela diz "Este certificado é Okey".  
  
Se um certificado válido não for encontrado, exclua o certificado inválido (se houver) e registrar novamente para o certificado do computador executando qualquer um dos `gpupdate /Force` do prompt de comando elevado ou reiniciar o computador cliente.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>Ausente da autoridade de certificação que emite certificados OTP  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Autenticação de OTP não pode ser concluída porque o servidor DA não retornou um endereço de uma autoridade de certificação emissora.  
  
**Causa**  
  
Nenhuma autoridade de certificação que emitem certificados OTP configurados, ou todas as CAs configurado que emitem certificados OTP não estão respondendo.  
  
**Solução**  
  
1.  Use o comando a seguir para obter a lista de autoridades de certificação que emitem certificados OTP (o nome da autoridade de certificação é mostrado no CAServer): `Get-DAOtpAuthentication`.  
  
2.  Se nenhuma autoridade de certificação é configurados:  
  
    1.  Use o comando `Set-DAOtpAuthentication` ou o console de gerenciamento de acesso remoto para configurar as autoridades de certificação que emitem o OTP do DirectAccess certificado de logon.  
  
    2.  Aplicar a nova configuração e forçar os clientes a atualizar as configurações de GPO do DirectAccess executando `gpupdate /Force` do prompt de comando elevado ou reiniciar o computador cliente.  
  
3.  Se houver CAs configurado, verifique se que eles está online e respondendo a solicitações de registro.  
  
## <a name="misconfigured-directaccess-server-address"></a>Endereço do servidor DirectAccess configurados incorretamente  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Não é possível concluir a autenticação de OTP conforme o esperado. O nome ou endereço do servidor de acesso remoto não pode ser determinado.  Código de erro: < error_code >. As configurações do DirectAccess devem ser validadas pelo administrador do servidor.  
  
**Causa**  
  
O endereço do servidor DirectAccess não está configurado corretamente.  
  
**Solução**  
  
Verificação configurada usando de endereço do DirectAccess server `Get-DirectAccess` e corrija o endereço, se ele está configurado incorretamente.  
  
Verifique se as configurações mais recentes são implantadas no computador cliente executando `gpupdate /force` de um prompt de comando elevado ou reinicie o computador cliente.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>Falha ao gerar a solicitação de certificado de logon OTP  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). A solicitação de certificado para autenticação OTP não pode ser inicializada. Não é possível gerar uma chave privada, ou usuário <username> não é possível acessar o modelo de certificado < OTP_template_name > no controlador de domínio.  
  
**Causa**  
  
Há duas causas possíveis para esse erro:  
  
-   O usuário não tem permissão para ler o modelo de logon OTP.  
  
-   O computador do usuário não pode acessar o controlador de domínio devido a problemas de rede.  
  
**Solução**  
  
-   Examine as configurações no modelo de logon de OTP de permissões e certifique-se de que todos os usuários provisionados para OTP do DirectAccess 'leu' permissão.  
  
-   Certifique-se de que o controlador de domínio está configurado como um servidor de gerenciamento e que o computador cliente pode alcançar o controlador de domínio por meio do túnel de infraestrutura. Consulte o modelo de certificado OTP de plano 3.2.  
  
## <a name="no-connection-to-the-domain-controller"></a>Nenhuma conexão ao controlador de domínio  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Não é possível estabelecer uma conexão com o controlador de domínio para fins de autenticação de OTP. Código de erro: < error_code >.  
  
**Causa**  
  
Há duas causas possíveis para esse erro:  
  
-   O computador do usuário não tem nenhuma conectividade de rede.  
  
-   O controlador de domínio não está acessível por meio do túnel de infraestrutura.  
  
**Solução**  
  
-   Certifique-se de que o controlador de domínio está configurado como um servidor de gerenciamento executando o seguinte comando em um prompt do PowerShell: `Get-DAMgmtServer -Type All`.  
  
-   Certifique-se de que o computador cliente pode acessar o controlador de domínio por meio do túnel de infraestrutura.  
  
## <a name="otp-provider-requires-challengeresponse"></a>Provedor OTP exige o desafio/resposta  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Autenticação de OTP com o servidor de acesso remoto (< DirectAccess_server_name >) para o usuário (<username>) necessário um desafio do usuário.  
  
**Causa**  
  
O provedor OTP usado exige que o usuário fornecer credenciais adicionais na forma de uma troca de desafio/resposta RADIUS, que não há suporte para OTP do DirectAccess do Windows Server 2012.  
  
**Solução**  
  
Configure o provedor OTP para não exigir desafio/resposta em qualquer cenário.  
  
## <a name="incorrect-otp-logon-template-used"></a>Modelo de logon incorreto OTP usado  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). O modelo de autoridade de certificação do qual o usuário <username> solicitado um certificado não está configurado para emitir certificados OTP.  
  
**Causa**  
  
O modelo de logon de OTP do DirectAccess foi substituído e o computador cliente está tentando se autenticar usando um modelo mais antigo.  
  
**Solução**  
  
Verifique se que o computador cliente está usando a configuração de OTP mais recente, executando um dos seguintes:  
  
-   Forçar uma atualização de diretiva de grupo, executando o seguinte comando em um prompt de comando elevado: `gpupdate /Force`.  
  
-   Reinicie o computador cliente.  
  
## <a name="missing-otp-signing-certificate"></a>Falta certificado de autenticação de OTP  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente). Não foi encontrado um certificado de autenticação de OTP. A solicitação de registro de certificado OTP não pode ser assinada.  
  
**Causa**  
  
O certificado de autenticação de OTP do DirectAccess não pode ser encontrado no servidor de acesso remoto; Portanto, a solicitação de certificado de usuário não pode ser assinada pelo servidor de acesso remoto. Não há nenhum certificado de autenticação ou o certificado de autenticação expirou e não foi renovado.  
  
**Solução**  
  
Execute estas etapas no servidor de acesso remoto.  
  
1.  Verifique o nome de modelo de certificado configurado autenticação OTP, executando o cmdlet do PowerShell `Get-DAOtpAuthentication` e inspecionar o valor de `SigningCertificateTemplateName`.  
  
2.  Use o snap-in do MMC dos certificados para certificar-se de que um certificado válido registrado com base neste modelo existe no computador.  
  
3.  Se nenhum certificado existir, exclua o certificado expirado (se houver) e registrar um novo certificado com base neste modelo.  
  
Para criar a assinatura de OTP o modelo de certificado Consulte plano 3.3 o certificado de autoridade de registro.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>Ausente ou incorreta UPN/DN do usuário  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (log de eventos do cliente)  
  
Um dos seguintes erros:  
  
-   Usuário <username> não podem ser autenticados com OTP. Certifique-se de que um UPN está definido para o nome de usuário no Active Directory. Código de erro: < error_code >.  
  
-   Usuário <username> não podem ser autenticados com OTP. Certifique-se de que um DN é definido para o nome de usuário no Active Directory. Código de erro: < error_code >.  
  
**Erro recebido** (log de eventos do servidor)  
  
O nome de usuário <username> especificado para a autenticação OTP não existe.  
  
**Causa**  
  
O usuário não tem o nome Principal de usuário (UPN) ou atributos de nome diferenciado (DN) configurado corretamente na conta de usuário, essas propriedades são necessárias para o funcionamento adequado de OTP do DirectAccess.  
  
**Solução**  
  
Use o console de computadores e usuários do Active Directory no controlador de domínio para verificar se esses dois atributos estão definidas corretamente para o usuário da autenticação.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>Certificado OTP não é confiável para o logon  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Causa**  
  
A autoridade de certificação que emite certificados OTP não está no repositório NTAuth corporativo; Portanto, os certificados registrados não podem ser usados para logon. Isso pode ocorrer em ambientes de domínio e multifloresta de várias onde não é estabelecido entre domínios de confiança da autoridade de certificação.  
  
**Solução**  
  
Certifique-se de que o certificado de raiz da hierarquia de CA que emite certificados OTP está instalado na empresa repositório de certificados de NTAuth do domínio ao qual o usuário está tentando se autenticar.  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows não foi possível verificar as credenciais do usuário  
**Cenário**. Usuário não conseguir autenticar usando OTP com o erro: "Falha na autenticação devido a um erro interno"  
  
**Erro recebido** (computador cliente). Algo deu errado enquanto o Windows estavam Verificando suas credenciais. Tente novamente ou peça ao seu administrador para obter ajuda.  
  
**Causa**  
  
O protocolo de autenticação Kerberos não funciona quando o certificado de logon de OTP do DirectAccess não inclui uma CRL. O certificado de logon de OTP do DirectAccess não inclui uma lista de certificados Revogados, pois ambos:  
  
-   O modelo de logon de OTP do DirectAccess foi configurado com a opção **não incluem informações de revogação em certificados emitidos**.  
  
-   A autoridade de certificação está configurada para não publicar CRLs.  
  
**Solução**  
  
1.  Para confirmar a causa desse erro, no console de gerenciamento de acesso remoto, na **etapa 2 servidor de acesso remoto**, clique em **editar**e, em seguida, o **a configuração do servidor de acesso remoto** o assistente, clique em **modelos de certificado de OTP**. Anote o modelo de certificado usado para registro de certificados emitidos para autenticação OTP. Abra o console da autoridade de certificação, no painel esquerdo, clique em **modelos de certificado**, duas vezes no certificado de logon OTP para exibir as propriedades do modelo de certificado.  
  
    Para resolver esse problema, configure um certificado para o certificado de logon OTP e não selecionar o **não incluem informações de revogação em certificados emitidos** caixa de seleção a **Server** guia do modelo caixa de diálogo de propriedades.  
  
2.  No servidor de autoridade de certificação, abra o MMC de autoridade de certificação, clique com botão direito na AC emissora e clique em **propriedades**. Sobre o **extensões** guia Certifique-se de que a publicação da CRL esteja configurada corretamente.  
  


