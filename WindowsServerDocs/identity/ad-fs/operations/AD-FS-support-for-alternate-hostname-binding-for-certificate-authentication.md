---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3fd459ac469d052b2c183520cbf434c1ac351c0c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962748"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado

Em muitas redes, as políticas de firewall local podem não permitir o tráfego por meio de portas não padrão, como 49443. Isso se tornou um problema ao tentar realizar a autenticação de certificado com AD FS antes de AD FS no Windows Server 2016. Isso ocorre porque você não pode ter associações diferentes para autenticação de dispositivo e autenticação de certificado de usuário no mesmo host. A porta padrão 443 está associada para receber certificados de dispositivo e não pode ser alterada para dar suporte a várias associações no mesmo canal. Os resultados eram que a autenticação de cartão inteligente não funcionaria e os usuários não conhecem o que aconteceu, pois não há nenhuma indicação do que realmente aconteceu.  
  
Com AD FS no Windows Server 2016, isso pode ser feito.
  
Em AD FS no Windows Server 2016, isso mudou. Agora, damos suporte a dois modos, o primeiro usa o mesmo host (ou seja, adfs.contoso.com) com portas diferentes (443, 49443). O segundo usado hosts diferentes (adfs.contoso.com e certauth.adfs.contoso.com) com a mesma porta (443). Isso exigirá um certificado SSL para dar suporte a "certauth. <ADFS-Service-Name>" como um nome de entidade alternativo. Isso pode ser feito no momento da criação do farm ou posteriormente por meio do PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Como configurar a associação de nome de host alternativo para autenticação de certificado  
Há duas maneiras de adicionar a associação de nome de host alternativo para autenticação de certificado. A primeira é ao configurar um novo farm de AD FS com AD FS para o Windows Server 2016, se o certificado contiver um SAN (nome alternativo da entidade), ele será automaticamente configurado para usar o segundo método mencionado acima. Ou seja, ele configurará automaticamente dois hosts diferentes (sts.contoso.com e certauth.sts.contoso.com com a mesma porta. Se o certificado não contiver uma SAN, você verá um aviso informando que os nomes alternativos da entidade do certificado não dão suporte a certauth. *. Consulte as capturas de tela abaixo. A primeira mostra uma instalação em que o certificado tinha uma SAN e a segunda mostra um certificado que não o fez.  
  
![Associação de nome de host alternativa](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![Associação de nome de host alternativa](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Da mesma forma, uma vez que AD FS no Windows Server 2016 foi implantado, você pode usar o cmdlet do PowerShell: Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Quando solicitado, clique em Sim para confirmar.  E isso deve ser.

![Associação de nome de host alternativa](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Referências adicionais

* [Gerenciamento de certificados SSL no AD FS e no Windows Server 2016](./manage-ssl-certificates-ad-fs-wap.md)
