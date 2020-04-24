---
title: Recursos removidos ou com substituição planejada do Windows Server versão 1709 em diante
description: Recursos e funcionalidades removidos ou planejados para remoção nas versões.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.date: 08/22/2019
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: a74f3c6ec629df7d1cc40199091e84989606a50e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826679"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1709"></a>Recursos removidos ou com substituição planejada do Windows Server versão 1709 em diante

>Aplica-se a: Windows Server, versão 1709

Veja a seguir uma lista de recursos e funcionalidades do Windows Server versão 1709, que foram removidos do produto nessa versão ou estão sendo considerados para possível substituição em versões futuras. Essa lista é direcionada para profissionais de TI que estão atualizando sistemas operacionais em um ambiente comercial. **Esta lista está sujeita a alteração nas versões subsequentes e pode não incluir todos os recursos ou as funcionalidades afetados.** 

> [!TIP]
> - Você pode obter acesso antecipado a builds do Windows Server ao ingressar no [Programa Windows Insider](https://insider.windows.com). Essa é uma ótima maneira de testar as alterações de recurso.
> - Tem dúvidas sobre outras versões? Confira [Recursos removidos ou com substituição planejada no Windows Server](../get-started-19/removed-features.md).

## <a name="features-removed-from-windows-server-version-1709"></a>Recursos removidos do Windows Server versão 1709

O Windows Server versão 1709 contém os mesmos recursos presentes no Windows Server 2016. Entretanto, essa versão oferece opções de instalação diferentes do Windows Server 2016:

- Como uma versão de Canal semestral, o Windows Server versão 1709 oferece somente a opção de instalação Server Core. Para ver detalhes, confira [Comparação dos canais de manutenção](../get-started-19/servicing-channels-19.md).
- Dessa versão em diante, o Nano Server não está disponível como um sistema operacional de host instalável. Em vez disso, o Nano Server está disponível como um sistema operacional de contêiner. Consulte [Alterações no Nano Server no Windows Server, versão 1709](nano-in-semi-annual-channel.md).
- Desta versão em diante, o protocolo SMB versão 1 não é mais instalado por padrão. Para ver detalhes, confira [O SMBv1 não é instalado por padrão no Windows 10 Fall Creators Update e no Windows Server versão 1709 e versões posteriores](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows).


## <a name="features-being-considered-for-replacement-starting-with-subsequent-releases"></a>Recursos sendo considerados para substituição começando com as versões posteriores

Os recursos e funcionalidades a seguir estão sendo considerados para substituição começando pelas versões subsequentes ao Windows Server versão 1709. Eventualmente, eles podem ser completamente removidos da imagem do produto instalado e substituídos por outros recursos ou funcionalidades (ou instaláveis de outras fontes), mas ainda estão disponíveis nesta versão, às vezes, com funcionalidades específicas removidas. Você deve começar planejar a implantação de métodos alternativos ou a substituição futura para qualquer aplicativo, código ou uso que dependa desses recursos.

Caso tenha comentários para compartilhar sobre a substituição proposta de qualquer um desses recursos, você pode usar o [aplicativo Hub de Comentários](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). Apesar do aplicativo ser executado no Windows 10, você também pode usá-lo para nos enviar comentários sobre o produto Windows Server (e a documentação).

### <a name="iis-6-management-compatibility"></a>Compatibilidade de gerenciamento do IIS 6
Os recursos específicos sendo considerados para substituição são:

- Compatibilidade de metabase do IIS 6 (Metabase Web)
- Console de gerenciamento do IIS 6 (Console de gerenciamento Web herdado)
- Ferramentas de script do IIS 6 (scripts Web herdados)
- Compatibilidade de WMI do IIS 6 (WMI Web)

Em vez da Compatibilidade de metabase do IIS 6 (que atua como uma camada de emulação entre scripts de metabase do IIS 6 e a configuração com base em arquivos usada pelo IIS 7 ou pelas versões mais recentes), você deve começar a migração de scripts de gerenciamento para a configuração com base em arquivos do IIS usando ferramentas como o namespace Microsoft.Web.Administration.

Você também deve começar a migração do IIS 6.0 ou versões anteriores e mover para a versão mais recente do IIS, que está sempre disponível na versão mais recente do Windows Server.


### <a name="iis-digest-authentication"></a>Autenticação Digest do IIS
Esse método de autenticação será substituído. Em vez disso, você deve começar a usar outros métodos de autenticação, como o Mapeamento de certificado do cliente (consulte [Configurar mapeamentos de certificado de cliente um-para-um](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings)) ou a Autenticação do Windows (consulte [Configurações do aplicativo](https://docs.microsoft.com/iis-administration/configuration/appsettings.json)).

### <a name="internet-storage-name-service-isns"></a>iSNS (Internet Storage Name Service)
O iSNS está sendo considerado para substituição. O recurso do protocolo SMB oferece essencialmente a mesma funcionalidade, com recursos adicionais. Consulte a [Visão geral do protocolo SMB](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx) para obter mais informações sobre esse recurso.

### <a name="rsaaes-encryption-for-iis"></a>Criptografia RSA/AES para IIS 
Esse método de criptografia está sendo considerado para substituição, pois o método superior de Cryptography API: Next Generation (CNG) já está disponível. Para saber mais sobre a criptografia do CNG, confira [Sobre o CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx).

### <a name="windows-powershell-20"></a>Windows PowerShell 2.0
Essa versão anterior do Windows PowerShell foi substituída por várias versões mais recentes. Para obter o melhor em desempenho e recursos, migre para o Windows PowerShell 5.0 ou posterior. Consulte a [Documentação do PowerShell](https://docs.microsoft.com/powershell/index?view=powershell-5.1) para obter mais informações.

