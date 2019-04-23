---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887247"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado

>Aplica-se a: Windows Server 2016

Em muitas redes as políticas de firewall local talvez não permitam o tráfego por meio de portas não padrão, como 49443. Isso se tornou um problema ao tentar realizar a autenticação de certificado com o AD FS antes do AD FS no Windows Server 2016. Isso ocorre porque você não pode ter associações diferentes para autenticação de dispositivo e autenticação de certificado de usuário no mesmo host. A porta padrão 443 está associada a receber os certificados de dispositivo e não pode ser alterada para dar suporte a vários associação no mesmo canal. Os resultados foram que autenticação de cartão inteligente não funcionará e os usuários não estavam cientes do que aconteceu porque não há nenhuma indicação do que realmente aconteceu.  
  
Com o AD FS no Windows Server 2016 isso pode ser feito.
  
No AD FS no Windows Server 2016, isso foi alterado. Agora há suporte para dois modos, o primeiro usa o mesmo host (ou seja, adfs.contoso.com) com portas diferentes (443, 49443). A segunda usada hosts diferentes (adfs.contoso.com e certauth.adfs.contoso.com) com a mesma porta (443). Isso exigirá um certificado SSL para dar suporte a "certauth. < nome do serviço adfs >" como um nome alternativo da entidade. Isso pode ser feito no momento da criação do farm ou posteriormente por meio do PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Como configurar a associação de nome de host alternativo para autenticação de certificado  
Há duas maneiras que você pode adicionar a associação de nome de host alternativo para autenticação de certificado. A primeira é durante a configuração de um novo farm do AD FS com o AD FS do Windows Server 2016, se o certificado contém um nome alternativo da entidade (SAN), em seguida, ele será automaticamente configurado para usar o segundo método mencionado acima. Ou seja, ele automaticamente irá configurar dois hosts diferentes (sts.contoso.com e certauth.sts.contoso.com com a mesma porta. Se o certificado não contiver uma rede SAN, você verá um aviso informando que os nomes alternativos de entidade do certificado não dá suporte certauth.*. Consulte as capturas de tela abaixo. A primeira mostra uma instalação em que o certificado tivesse uma SAN e o segundo mostra um certificado que não o fez.  
  
![associação de nome de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![associação de nome de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Da mesma forma, após a implantação do AD FS no Windows Server 2016, você pode usar o cmdlet do PowerShell: Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Quando solicitado, clique em Sim para confirmar.  E que deve ser ele.

![associação de nome de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Referências adicionais

* [Gerenciamento de certificados SSL em AD FS e WAP no Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
