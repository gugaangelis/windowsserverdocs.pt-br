---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory atualizações de esquema em todo o domínio
description: Atualizações de esquema em todo o domínio executadas pelo adprep/domainprep ao promover um controlador de domínio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.openlocfilehash: 606e48502587675b9a98e279f1b9047db8789021
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959094"
---
# <a name="domain-wide-schema-updates"></a>Atualizações de esquema em todo o domínio

>Aplica-se a: Windows Server

Você pode examinar o seguinte conjunto de alterações para ajudar a entender e preparar as atualizações de esquema executadas pelo adprep/domainprep no Windows Server.

A partir do Windows Server 2012, os comandos da Adprep são executados automaticamente conforme necessário durante a instalação do AD DS. Elas também podem ser executadas separadamente com antecedência da instalação do AD DS. Para obter mais informações, consulte [Executando Adprep.exe](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)).

Para obter mais informações sobre como interpretar as cadeias de caracteres de entrada de controle de acesso (ACE), consulte [seqüências de caracteres Ace](/windows/win32/secauthz/ace-strings). Para obter mais informações sobre como interpretar as cadeias de caracteres de ID de segurança (SID), consulte [cadeias de caracteres de Sid](/windows/win32/secauthz/sid-strings).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canal semestral): atualizações em todo o domínio

Depois que as operações executadas pelo **domainprep** no Windows Server 2016 (operação 89) forem concluídas, o atributo de **revisão** para o objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain será definido como **16**.

|Número de operações e GUID|Descrição|Permissões|
|------------------------------|---------------|--------------|---------------|
|**Operação 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Exclua a ACE concedendo controle total a administradores de chave corporativa e adicione uma ACE concedendo controle total aos administradores de chave corporativa apenas sobre o atributo msdsKeyCredentialLink.|Excluir (A; CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de chave corporativa) <br /> <br />Adicionar (OA; CIS RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Administradores de chave corporativa)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: atualizações em todo o domínio

Depois que as operações executadas pelo **domainprep** no Windows Server 2016 (operations 82-88) forem concluídas, o atributo de **revisão** para o objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain será definido como **15**.

|Número de operações e GUID|Descrição|Atributos|Permissões|
|------------------------------|---------------|--------------|---------------|
|**Operação 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Criar CN = Contêiner de chaves na raiz do domínio|-objectClass: contêiner<br />-Descrição: contêiner padrão para objetos de credencial de chave<br />-ShowInAdvancedViewOnly: TRUE|Um CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; EUM<br />Um CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;;D Um<br />Um CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Sy<br />Um CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;;D 3D<br />Um CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; COMANDOS|
|**Operação 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Adicionar controle total permitir ACEs para o contêiner CN = Keys para "domain\Key admins" e "rootdomain\Enterprise Key admins".|N/D|Um CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de chaves)<br />Um CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de chave corporativa)|
|**Operação 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifique o atributo otherWellKnownObjects para apontar para o contêiner CN = Keys.|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981: CN = Keys,% WS|N/D|
|**Operação 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modifique o NC de domínio para permitir que "domain\Key admins" e "rootdomain\Enterprise Key admins" modifiquem o atributo msDS-KeyCredentialLink. |N/D|OA CIS RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Administradores de chaves)<br />OA CIS RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Os administradores de chave corporativa no domínio raiz, mas em domínios não raiz resultaram em uma ACE falsa relativa ao domínio com um SID não resolvível-527)|
|**Operação 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Conceder o carro do DS-Validated-Write-Computer para o proprietário do criador e o próprio|N/D|OA CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11D0-a285-00aa003049e2; PS)<br />OA CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11D0-a285-00aa003049e2; CO)|
|**Operação 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Exclua a ACE concedendo controle total ao grupo de administradores de chave corporativa relativo ao domínio incorreto e adicione uma ACE concedendo controle total ao grupo de administradores de chave corporativa. |N/D|Excluir (A; CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de chave corporativa)<br /> <br />Adicionar (A; CIS RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administradores de chave corporativa)|
|**Operação 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Adicione "msDS-ExpirePasswordsOnSmartCardOnlyAccounts" no objeto NC de domínio e defina o valor padrão como FALSE|N/D|N/D|

Os grupos Administradores de chave corporativa e administradores de chave são criados somente depois que um controlador de domínio do Windows Server 2016 é promovido e assume a função FSMO do emulador de PDC.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: atualizações em todo o domínio

Embora nenhuma operação seja executada pelo **domainprep** no Windows Server 2012 R2, após a conclusão do comando, o atributo de **revisão** para o objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain é definido como **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: atualizações em todo o domínio

Depois que as operações executadas pelo **domainprep** no Windows Server 2012 (operations 78, 79, 80 e 81) forem concluídas, o atributo de **revisão** para o objeto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = ForestRootDomain será definido como **9**.

|Número de operações e GUID|Descrição|Atributos|Permissões|
|------------------------------|---------------|--------------|---------------|
|**Operação 78**: {c3c927a6-cc1d-47c0-966B-be8f9b63d991}|Crie um novo objeto CN = TPM Devices na partição de domínio.|Classe de objeto: msTPM-InformationObjectsContainer|N/D|
|**Operação 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Foi criada uma entrada de controle de acesso para o serviço TPM.|N/D|OA CIIO; WP; ea1b7b93-5e48-46D5-bc6c-4df4fda78a35; bf967a86-0de6-11D0-a285-00aa003049e2; PS)|
|**Operação 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Conceda "clone DC" estendido à direita para o grupo de **controladores de domínio clonable**|N/D|(OA;; CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;; *Sid de domínio*-522)|
|**Operação 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Conceder ms-DS-allowed-to-Act-on-be-of-Identity para a própria entidade de segurança em todos os objetos.|N/D|OA CIOI; RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;; PROFISSIONAIS|
