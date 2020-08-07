---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: Referência Técnica de Registro do Dispositivo
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: fc42813829116cf3755d7807bec4e5fb00094c8e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937975"
---
# <a name="device-registration-technical-reference"></a>Referência Técnica de Registro do Dispositivo
O serviço de registro de dispositivo \( DRS \) é um novo serviço do Windows incluído com a Active Directory serviço de Federação função no Windows Server 2012 R2.  O DRS deve ser instalado e configurado em todos os servidores de federação no farm do AD FS.  Para obter informações sobre o DRS, consulte [Configurar um servidor de federação com o serviço de registro de dispositivo](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486831(v=ws.11)).

## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Objetos do Active Directory criados quando um dispositivo é registrado
Os seguintes objetos do Active Directory são criados como parte do serviço de registro do dispositivo.

### <a name="device-registration-configuration"></a>Configuração de registro do dispositivo
A configuração de registro do dispositivo é armazenada no contexto de nomenclatura de configuração da floresta do Active Directory. \(Por exemplo, ** \= configuração de registro de dispositivo CN, serviços cn \= , \- <\->contexto de nomenclatura de configuração ** \) . Esse objeto é criado quando a floresta do Active Directory é iniciada para o registro do dispositivo.

A configuração do registro de dispositivo inclui os seguintes elementos:

-   **Chaves do emissor**

    As chaves públicas e privadas usadas para emitir o certificado X.509 que é associado um dispositivo registrado.  As chaves privadas são protegidas por DKM.

-   **Configuração do serviço de registro do dispositivo**

    Políticas relativas ao serviço de registro do dispositivo.

### <a name="registered-devices-container"></a>Contêiner de dispositivos registrados
O contêiner do objeto de dispositivo é criado em um dos domínios na floresta do Active Directory.  Este contêiner de objeto contém todos os objetos de dispositivo para a floresta do Active Directory.

Por padrão, o contêiner é criado no mesmo domínio do AD FS.  \(Por exemplo, **CN \= REGISTEREDDEVICES, DC \=<\- contexto de nomenclatura padrão \->** \) . Esse objeto é criado quando a floresta de Active Directory é inicializada para o registro do dispositivo.

### <a name="registered-devices"></a>Dispositivos registrados
Objetos de dispositivo são objetos novos e leves no Active Directory.  Eles são usados para representar a relação entre: um usuário, um dispositivo e a empresa.  Objetos de dispositivo usam um certificado assinado pelo AD FS para ancorar o dispositivo físico para o objeto de dispositivo lógico no Active Directory.

Os dispositivos registrados incluem os seguintes elementos:

-   **Nome de exibição**

    Nome amigável do dispositivo.  Para dispositivos do Windows, esse é o nome do host do computador.

-   **ID do dispositivo**

    Uma GUID que é gerada pelo servidor de registro do dispositivo.

-   **Impressão digital do certificado**

    A impressão digital do certificado X.509 que é usada com o dispositivo registrado.

-   **Tipo de sistema operacional**

    Tipo do sistema operacional do dispositivo.

-   **Versão do sistema operacional**

    A versão do sistema operacional no dispositivo.

-   **Está habilitado**

    Um valor booliano que indica se o dispositivo está habilitado no Active Directory.  Somente os dispositivos habilitados têm permissão para acessar serviços.

-   **Último horário de uso aproximado**

    O horário aproximado em que o dispositivo foi usado para acessar um recurso.  Para limitar o tráfego de replicação, isso só é atualizado uma vez a cada 14 dias.

-   **Proprietário registrado**

    O \( Sid de identidade \) de segurança do usuário que ingressou neste dispositivo no local de trabalho.

## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>\/Verificação de revogação de certificado SSL do AD FS DRS Server
O cliente de ingresso do local de trabalho verifica a validade do certificado SSL do servidor do AD FS.  Se o certificado SSL do servidor do AD FS incluir um ponto de extremidade CRL da lista de certificados revogados \( \) , o cliente deverá ser capaz de alcançar o ponto de extremidade especificado para validar o certificado.

Se você estiver usando um ambiente de teste e uma AC de \( autoridade \) de certificação de teste para emitir os certificados SSL do servidor, poderá optar por não incluir o ponto de extremidade da CRL nos certificados do servidor emitidos pela sua autoridade de certificação.  Isso permitirá que o cliente de ingresso do local de trabalho ignore a verificação da CRL.

> [!CAUTION]
> Isso nunca é recomendado para sistemas de produção

