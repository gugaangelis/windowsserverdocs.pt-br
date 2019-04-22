---
title: Solução de problemas da habilitação de OTP
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
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d789671a0425974e560e5f4a43ebcba4c1741c76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819457"
---
# <a name="troubleshooting-enabling-otp"></a>Solução de problemas da habilitação de OTP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações sobre solução de problemas relacionados à habilitação da autenticação de OTP do DirectAccess usando o **Enable-DAOtpAuthentication** cmdlet do PowerShell ou o console de gerenciamento de acesso remoto.
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>Falha ao registrar o certificado de autenticação de OTP  
**Erro recebido** (log de eventos do servidor). Um certificado de autenticação de OTP não pode ser registrado usando o modelo de certificado < OTP_signing_template_name >  
  
**Causa**  
  
Há três possíveis causas para esse erro:  
  
-   O modelo não existe.  
  
-   As permissões definidas no modelo não permitem que o servidor DirectAccess registrar.  
  
-   Não há nenhuma conectividade de rede para a autoridade de certificação emissora (CA).  
  
**Solução**  
  
1.  Certifique-se de que a autenticação de OTP modelo de certificado com o nome fornecido:  
  
    1.  Existe e tem as permissões adequadas.  
  
    2.  É definido para ser emitido pelo menos uma autoridade de certificação que possa emitir certificados para o servidor DirectAccess.  
  
2.  Se o modelo não existe, criá-lo, conforme descrito no plano 3.3 o certificado de autoridade de registro ou se existe um outro modelo correspondente reconfigurar OTP do DirectAccess com o novo nome de modelo.  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>Falha ao habilitar a OTP do DirectAccess quando o WebDAV está instalado  
**Cenário**. Durante a tentativa de aplicar a configuração de OTP do DirectAccess no console de gerenciamento de acesso remoto ou usando o `Enable-DAOtpAuthentication` cmdlet do PowerShell, a operação falhará.  
  
**Erro recebido** (log de eventos do servidor). Configurações de OTP do DirectAccess não podem ser aplicadas porque a extensão WebDAV IIS está em execução no servidor. Remova o WebDAV e aplicar as configurações novamente.  
  
**Causa**  
  
O serviço de OTP do DirectAccess é incompatível com o recurso de publicação do WebDAV e não pode ser habilitado enquanto o WebDAV está instalado.  
  
**Solução**  
  
Desinstale a função de WebDAV:  
  
1.  No console do Gerenciador do servidor, no painel esquerdo, clique em **IIS**.  
  
2.  No painel principal, role até **funções e recursos**.  
  
3.  Clique com botão direito **a publicação WebDAV**e, em seguida, clique em **remover funções e recursos**.  
  
4.  Conclua o Assistente de recursos e remover funções.  
  
5.  Aplique novamente a configuração de OTP do DirectAccess.  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>Não há modelos disponíveis no console de gerenciamento de acesso remoto  
**Cenário**. Embora configurar OTP ou o registro de modelos de certificado de autoridade usando o console de gerenciamento de acesso remoto, alguns ou todos os modelos estão ausentes do windows de seleção.  
  
**Causa**  
  
Há duas causas possíveis para esse erro:  
  
-   O modelo não está configurado de acordo com os requisitos de OTP do DirectAccess e portanto não pode ser selecionado.  
  
-   As ACS selecionadas sob **servidores CA OTP** não estão configurados para emitir os modelos necessários.  
  
**Solução**  
  
1.  Certifique-se de que o modelo de OTP de logon e o modelo de certificado de autenticação de OTP estão configurados corretamente, conforme descrito no plano 3.2, o modelo de certificado OTP e 3.3 planejar o certificado de autoridade de registro.  
  
2.  Certifique-se de que autoridades de certificação configurado na **servidores CA OTP** lista são configurados para problemas de modelos de relevantes:  
  
    1.  No servidor de autoridade de certificação, abra o console da autoridade de certificação.  
  
    2.  No painel esquerdo, expanda o servidor de autoridade de certificação escolhido.  
  
    3.  Clique em **modelos de certificado** e verifique se os modelos necessários estão habilitados. Se não estiver, clique com botão direito **modelos de certificado**, clique em **New**, clique em **modelo de certificado para emitir**e, em seguida, selecione os modelos que você deseja habilitar.  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>Não é possível definir o período de renovação de modelo OTP para 1 hora  
**Cenário**. Ao configurar o modelo de logon de OTP do DirectAccess usando a autoridade de certificação do Windows 2003, não é possível definir o período de renovação do modelo para 1 hora.  
  
**Causa**  
  
O snap-in MMC de modelos de certificado no Windows Server 2003 não permite que você defina o período de renovação de um modelo para 1 hora.  
  
**Solução**  
  
Instalar o snap-in de modelos de certificado em um servidor do posterior ao Windows Server 2003 e usá-lo para configurar o modelo de logon OTP, consulte [instalar o Snap-In de modelos de certificado](https://technet.microsoft.com/library/cc732445.aspx).  
  


