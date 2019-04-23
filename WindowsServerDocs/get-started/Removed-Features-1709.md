---
title: Recursos removidos ou com substituição planejada a partir do Windows Server (versão 1709)
description: Recursos e funcionalidades removidos ou planejados para remoção nas versões.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/05/2018
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 47d90c32f705157af60b1d8ca38122b3c6363c0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866777"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1709"></a>Recursos removidos ou com substituição planejada a partir do Windows Server, versão 1709

>Aplica-se a: Windows Server, versão 1709

Veja a seguir uma lista de recursos e funcionalidades do Windows Server, versão 1709, que foram removidos do produto nessa versão ou estão sendo considerados para remoção em potencial nas versões futuras. Essa lista é direcionada para profissionais de TI que estão atualizando sistemas operacionais em um ambiente comercial. **Esta lista está sujeita a alterações em versões futuras e pode não incluir todos os recursos afetados ou funcionalidade.** 

## <a name="features-removed-from-windows-server-version-1709"></a>Recursos removidos do Windows Server, versão 1709
O Windows Server, versão 1709 contém os mesmos recursos presentes no Windows Server 2016. Entretanto, essa versão oferece opções de instalação diferentes do Windows Server 2016:

- Como uma versão de Canal semestral, o Windows Server versão 1709 oferece somente a opção de instalação Server Core. Para obter mais detalhes, consulte [Visão geral do canal semestral do Windows Server](semi-annual-channel-overview.md).
- A partir dessa versão, o Nano Serve não está disponível como um sistema operacional de host instalável. Em vez disso, o Nano Server está disponível como um sistema operacional de contêiner. Consulte [Mudanças no Nano Server no Windows Server, versão 1709](nano-in-semi-annual-channel.md).
- A partir desta versão, bloco de mensagens de servidor (SMB) versão 1 não é mais instalado por padrão. Para obter detalhes, consulte [SMBv1 não está instalado por padrão no Windows 10 Fall Creators Update e Windows Server, versão 1709 e versões posteriores](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows).


## <a name="features-being-considered-for-replacement-starting-with-subsequent-releases"></a>Recursos sendo considerados para substituição a partir das versões posteriores

Os seguintes recursos e funcionalidades serão considerados para substituição a partir das versões após o Windows Server, versão 1709. Eventualmente, eles podem ser completamente removidos da imagem do produto instalado e substituídos por outros recursos ou funcionalidades (ou instaláveis por outras fontes), mas ainda estão disponíveis nesta versão, às vezes, com funcionalidades especificas removidas. Você deve começar planejar a implantação de métodos alternativos ou a substituição futura para qualquer aplicativo, código ou uso que dependa desses recursos.

Caso tenha comentários para compartilhar sobre a substituição proposta de qualquer um desses recursos, você pode usar o [aplicativo Hub de Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). Apesar do aplicativo ser executado no Windows 10, você também pode usá-lo para nos enviar comentários sobre o produto Windows Server (e a documentação).

### <a name="iis-6-management-compatibility"></a>Compatibilidade de gerenciamento do IIS 6
Os recursos específicos considerados para substituição são:

- Compatibilidade de metabase do IIS 6 (Metabase Web)
- Console de gerenciamento do IIS 6 (Console de gerenciamento Web herdado)
- Ferramentas de script do IIS 6 (scripts Web herdados)
- Compatibilidade de WMI do IIS 6 (WMI Web)

Em vez da Compatibilidade de metabase do IIS 6 (que atua como uma camada de emulação entre scripts de metabase do IIS 6 e a configuração com base em arquivos usada pelo IIS 7 ou pelas versões mais recentes), você deve começar a migração de scripts de gerenciamento para a configuração com base em arquivos do IIS usando ferramentas como o namespace Microsoft.Web.Administration.

Você também deve começar a migração do IIS 6.0 ou versões anteriores e mover para a versão mais recente do IIS, que sempre está disponível na versão mais recente do Windows Server.


### <a name="iis-digest-authentication"></a>Autenticação Digest do IIS
Esse método de autenticação será substituído. Em vez disso, você deve começar a usar outros métodos de autenticação, como o Mapeamento de certificado do cliente (consulte [Configurar mapeamentos de certificado de cliente um-para-um](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings)) ou a autenticação do Windows (consulte [Configurações do aplicativo](https://docs.microsoft.com/iis-administration/configuration/appsettings.json)).

### <a name="internet-storage-name-service-isns"></a>iSNS (Internet Storage Name Service)
O iSNS está sendo considerado para substituição. O recurso do protocolo SMB oferece essencialmente a mesma funcionalidade com recursos adicionais. Consulte a [Visão geral do protocolo SMB](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx) para obter mais informações sobre o recurso.

### <a name="rsaaes-encryption-for-iis"></a>Criptografia RSA/AES para IIS 
Esse método de criptografia está sendo considerado para substituição, pois a API de criptografia superior: Método de Next Generation (CNG) já está disponível. Para saber mais sobre a criptografia do CNG, consulte [Sobre o CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx).

### <a name="windows-powershell-20"></a>Windows PowerShell 2.0
Essa versão anterior do Windows PowerShell foi substituído por várias versões mais recentes. Para obter o melhor em desempenho e recursos, migre para o Windows PowerShell 5.0 ou posterior. Consulte a [Documentação do PowerShell](https://docs.microsoft.com/powershell/index?view=powershell-5.1) para obter mais informações.

