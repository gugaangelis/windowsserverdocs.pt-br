---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: "Atualizações de esquema de todo o domínio do Active Directory"
description: "Atualização de esquema de todo o domínio realizada pelo adprep /domainprep ao promover um controlador de domínio"
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 07/14/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59efb7c854b7f3693776bf9a1a371f98c2206d60
ms.sourcegitcommit: 79c1359232bece2e5c3ee5507f1e4bf19b44f487
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2018
---
# <a name="domain-wide-schema-updates"></a>Atualizações de esquema de todo o domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode revisar o seguinte conjunto de alterações para ajudar a entender e preparar para as atualizações de esquema são executadas por adprep /domainprep no Windows Server. 

A partir do Windows Server 2012, Adprep comandos são executados automaticamente conforme necessário durante a instalação do AD DS. Eles também podem ser executados separadamente antes da instalação do AD DS. Para obter mais informações, consulte [executando Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Para obter mais informações sobre como interpretar as cadeias de caracteres de entrada (ACE) de controle de acesso, consulte [cadeias de caracteres ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Para obter mais informações sobre como interpretar as cadeias de caracteres de ID (SID) de segurança, consulte [cadeias de caracteres de SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (anual por canal): Atualizações de todo o domínio

Após as operações que são executadas por **domainprep** no Windows Server 2016 (operação 89) completa, o **revisão** atributo para CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain objeto é definido como **16**.

|Número de operações e o GUID|Descrição|Permissões|
|------------------------------|---------------|--------------|---------------|
|**Operação 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Exclua a ACE conceder controle total a administradores de chave corporativos e adicionar uma ACE conceder controle total do Enterprise chave administradores sobre apenas o atributo msdsKeyCredentialLink.|Excluir (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de chave corporativos) <br /> <br />Adicionar (OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de chave corporativos)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: Atualizações de todo o domínio

Após as operações que são executadas por **domainprep** no Windows Server 2016 (operações 82 88) completa, o **revisão** atributo para CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain objeto é definido como **15**.

|Número de operações e o GUID|Descrição|Atributos|Permissões|
|------------------------------|---------------|--------------|---------------|
|**Operação 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Criar CN = contêiner de chaves na raiz do domínio|-Classe: contêiner<br />-Descrição: contêiner padrão para objetos de chaves de credenciais<br />-ShowInAdvancedViewOnly: TRUE|(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; EA)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; D A)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; D D)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; ED)|
|**Operação 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Adicionar o controle total permitir aces para CN = contêiner de chaves para "domain\Key administradores" e "rootdomain\Enterprise administradores chave".|N/D|(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de chaves)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de chave corporativos)|
|**Operação de 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifique o atributo otherWellKnownObjects para apontar para o CN = contêiner de chaves.|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN = chaves, %ws|N/D|
|**Operação 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modificar o domínio NC para permitir "domain\Key administradores" e "rootdomain\Enterprise administradores chave" para modificar o atributo msds-KeyCredentialLink. |N/D|(OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de chaves)<br />(OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de chave corporativos no domínio raiz, mas em domínios não raiz resultaram em uma ACE relativo ao domínio falsa com um SID-527 não puder ser resolvido)|
|**Operação 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Conceder o CARRO DS-validado-gravação-computador proprietário criador e self|N/D|(OA; CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA; CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;co)|
|**Operação 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Exclua a ACE conceder controle total a incorreto relativo ao domínio chave grupo Administradores de empresa e adicionar uma ACE conceder controle total ao grupo Admins da chave. |N/D|Excluir (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de chave corporativos)<br /> <br />Adicionar (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de chave corporativos)|
|**Operação 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Adicione "msDS-ExpirePasswordsOnSmartCardOnlyAccounts" no objeto de NC de domínio e defina o valor padrão como FALSE|N/D|N/D|

Os grupos de administradores de chave de empresa e administradores de chave são criados somente depois que um controlador de domínio do Windows Server 2016 é promovido e ele assume a função FSMO de emulador do PDC.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: Atualizações de todo o domínio

Embora não operações serão executadas por **domainprep** no Windows Server 2012 R2, depois que o comando é concluído, o **revisão** atributo para CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain objeto é definido como **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: Atualizações de todo o domínio

Após as operações que são executadas por **domainprep** no Windows Server 2012 (operações 78, 79, 80 e 81) completa, o **revisão** atributo para CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain objeto é definido como **9**.

|Número de operações e o GUID|Descrição|Atributos|Permissões|
|------------------------------|---------------|--------------|---------------|
|**Operação 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Crie um novo objeto CN = TPM dispositivos na partição do domínio.|Classe de objeto: msTPM-InformationObjectsContainer|N/D|
|**Operação 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Criado uma entrada de controle de acesso para o serviço TPM.|N/D|(OA; CIIO; WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**Operação 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Conceder "Clone DC" direito estendido para **Cloneable controladores de domínio** grupo|N/D|(OA; CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e; *SID do domínio*-522)|
|**Operação 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Conceder ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity a entidade de segurança Self em todos os objetos.|N/D|(OA; CIOI; RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79; PS)|
