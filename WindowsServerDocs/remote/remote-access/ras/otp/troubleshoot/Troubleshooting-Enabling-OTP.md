---
title: Solução de problemas da habilitação de OTP
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a01a52be4ce755c57297ba825be249b9b6853c7d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958294"
---
# <a name="troubleshooting-enabling-otp"></a>Solução de problemas da habilitação de OTP

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico contém informações de solução de problemas relacionados à habilitação da autenticação de OTP do DirectAccess usando o cmdlet **Enable-DAOtpAuthentication** do PowerShell ou o console de gerenciamento de acesso remoto.

## <a name="failed-to-enroll-the-otp-signing-certificate"></a>Falha ao registrar o certificado de assinatura de OTP
**Erro recebido** (log de eventos do servidor). Um certificado de assinatura de OTP não pode ser registrado usando o modelo de certificado <OTP_signing_template_name>

**Causa**

Há três causas possíveis para esse erro:

-   O modelo não existe.

-   As permissões definidas no modelo não permitem que o servidor DirectAccess seja registrado.

-   Não há conectividade de rede com a CA (autoridade de certificação) emissora.

**Solução**

1.  Certifique-se de que o modelo de certificado de assinatura de OTP com o nome fornecido:

    1.  Existe e tem as permissões adequadas.

    2.  É definido para ser emitido por pelo menos uma autoridade de certificação que pode emitir certificados para o servidor DirectAccess.

2.  Se o modelo não existir, crie-o conforme descrito em 3,3 planejar o certificado de autoridade de registro ou se outro modelo de correspondência existir, reconfigure a OTP do DirectAccess com o novo nome de modelo.

## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>Falha ao habilitar a OTP do DirectAccess quando o WebDAV está instalado
**Cenário**. Ao tentar aplicar a configuração de OTP do DirectAccess no console de gerenciamento de acesso remoto ou usando o `Enable-DAOtpAuthentication` cmdlet do PowerShell, a operação falha.

**Erro recebido** (log de eventos do servidor). As configurações de OTP do DirectAccess não podem ser aplicadas porque a extensão do IIS do WebDAV está em execução no servidor. Remova o WebDAV e aplique as configurações novamente.

**Causa**

O serviço de OTP do DirectAccess é incompatível com o recurso de publicação WebDAV e não pode ser habilitado enquanto o WebDAV está instalado.

**Solução**

Desinstale a função WebDAV:

1.  No console do Gerenciador do Servidor, no painel esquerdo, clique em **IIS**.

2.  No painel principal, role até **funções e recursos**.

3.  Clique com o botão direito do mouse em **publicação WebDAV**e clique em **remover função ou recurso**.

4.  Conclua o assistente para remover funções e recursos.

5.  Aplique novamente a configuração de OTP do DirectAccess.

## <a name="no-templates-available-in-the-remote-access-management-console"></a>Nenhum modelo disponível no console de gerenciamento de acesso remoto
**Cenário**. Ao configurar os modelos de certificado de autoridade de registro ou OTP usando o console de gerenciamento de acesso remoto, alguns ou todos os modelos estão ausentes das janelas de seleção.

**Causa**

Há duas causas possíveis para esse erro:

-   O modelo não está configurado de acordo com os requisitos de OTP do DirectAccess e, portanto, não pode ser selecionado.

-   As CAs selecionadas em **servidores de AC OTP** não estão configuradas para emitir os modelos necessários.

**Solução**

1.  Verifique se o modelo de logon de OTP e o modelo de certificado de assinatura OTP estão configurados corretamente conforme descrito em 3,2 planejar o modelo de certificado de OTP e 3,3 planejar o certificado de autoridade de registro.

2.  Verifique se as CAs configuradas na lista **servidores de AC de OTP** estão configuradas para emitir os modelos relevantes:

    1.  No servidor de AC, abra o console da autoridade de certificação.

    2.  No painel esquerdo, expanda o servidor de AC escolhido.

    3.  Clique em **modelos de certificado** e verifique se os modelos necessários estão habilitados. Caso contrário, clique com o botão direito do mouse em **modelos de certificado**, clique em **novo**, clique em **modelo de certificado para emitir**e selecione os modelos que deseja habilitar.

## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>Não é possível definir o período de renovação do modelo de OTP como 1 hora
**Cenário**. Ao configurar o modelo de logon de OTP do DirectAccess usando a AC do Windows 2003, não é possível definir o período de renovação do modelo como 1 hora.

**Causa**

O snap-in MMC de modelos de certificado no Windows Server 2003 não permite que você defina o período de renovação de um modelo como 1 hora.

**Solução**

Instalar o snap-in de modelos de certificado em um servidor posterior ao Windows Server 2003 e usá-lo para configurar o modelo de logon de OTP, consulte [instalar o snap-in de modelos de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732445(v=ws.11)).

