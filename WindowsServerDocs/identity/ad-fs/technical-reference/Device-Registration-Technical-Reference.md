---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: Referência Técnica de Registro do Dispositivo
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0b921e4a88940477ec7d0d4b2fa165880bd41150
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860219"
---
# <a name="device-registration-technical-reference"></a>Referência Técnica de Registro do Dispositivo
O serviço de registro de dispositivo \(DRS\) é um novo serviço do Windows que está incluído com a função de Serviço de Federação Active Directory no Windows Server 2012 R2.  O DRS deve ser instalado e configurado em todos os servidores de federação no farm do AD FS.  Para obter informações sobre o DRS, consulte [Configurar um servidor de federação com o serviço de registro de dispositivo](https://technet.microsoft.com/library/dn486831.aspx).  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Objetos do Active Directory criados quando um dispositivo é registrado  
Os seguintes objetos do Active Directory são criados como parte do serviço de registro do dispositivo.  
  
### <a name="device-registration-configuration"></a>Configuração de registro do dispositivo  
A configuração de registro do dispositivo é armazenada no contexto de nomenclatura de configuração da floresta do Active Directory. \(por exemplo, **cn\=configuração de registro de dispositivo, cn\=Services, < Configuration\-nomenclatura\-Context** >\). Esse objeto é criado quando a floresta do Active Directory é iniciada para o registro do dispositivo.  
  
A configuração do registro de dispositivo inclui os seguintes elementos:  
  
-   **Chaves do emissor**  
  
    As chaves públicas e privadas usadas para emitir o certificado X.509 que é associado um dispositivo registrado.  As chaves privadas são protegidas por DKM.  
  
-   **Configuração do serviço de registro de dispositivo**  
  
    Políticas relativas ao serviço de registro do dispositivo.  
  
### <a name="registered-devices-container"></a>Contêiner de dispositivos registrados  
O contêiner do objeto de dispositivo é criado em um dos domínios na floresta do Active Directory.  Este contêiner de objeto contém todos os objetos de dispositivo para a floresta do Active Directory.  
  
Por padrão, o contêiner é criado no mesmo domínio do AD FS.  \(por exemplo, **CN\=RegisteredDevices, DC\=< padrão\-nomenclatura\-contexto** >\). Esse objeto é criado quando a floresta de Active Directory é inicializada para o registro do dispositivo.  
  
### <a name="registered-devices"></a>Dispositivos registrados  
Objetos de dispositivo são objetos novos e leves no Active Directory.  Eles são usados para representar a relação entre: um usuário, um dispositivo e a empresa.  Objetos de dispositivo usam um certificado assinado pelo AD FS para ancorar o dispositivo físico para o objeto de dispositivo lógico no Active Directory.  
  
Os dispositivos registrados incluem os seguintes elementos:  
  
-   **Nome de exibição**  
  
    Nome amigável do dispositivo.  Para dispositivos do Windows, esse é o nome do host do computador.  
  
-   **ID do dispositivo**  
  
    Uma GUID que é gerada pelo servidor de registro do dispositivo.  
  
-   **Impressão digital do certificado**  
  
    A impressão digital do certificado X.509 que é usada com o dispositivo registrado.  
  
-   **Tipo de so**  
  
    Tipo do sistema operacional do dispositivo.  
  
-   **Versão do so**  
  
    Versão do sistema operacional do dispositivo.  
  
-   **Está habilitado**  
  
    Um valor booliano que indica se o dispositivo está habilitado no Active Directory.  Somente os dispositivos habilitados têm permissão para acessar serviços.  
  
-   **Hora da última utilização aproximada**  
  
    O horário aproximado em que o dispositivo foi usado para acessar um recurso.  Para limitar o tráfego de replicação, isso só é atualizado uma vez a cada 14 dias.  
  
-   **Proprietário registrado**  
  
    A identidade de segurança \(\) SID do usuário que ingressou nesse dispositivo no local de trabalho.  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>AD FS a verificação de revogação de certificado SSL do\/DRS Server  
O cliente de ingresso do local de trabalho verifica a validade do certificado SSL do servidor do AD FS.  Se o certificado SSL do servidor de AD FS inclui uma lista de certificados revogados \(ponto de extremidade\) CRL, o cliente deve ser capaz de acessar o ponto de extremidade especificado para validar o certificado.  
  
Se você estiver usando um ambiente de teste e uma autoridade de certificação de teste \(\) da AC para emitir os certificados SSL do servidor, poderá optar por não incluir o ponto de extremidade da CRL nos certificados do servidor emitidos pela sua autoridade de certificação.  Isso permitirá que o cliente de ingresso do local de trabalho ignore a verificação da CRL.  
  
> [!CAUTION]  
> Isso nunca é recomendado para sistemas de produção  
  

