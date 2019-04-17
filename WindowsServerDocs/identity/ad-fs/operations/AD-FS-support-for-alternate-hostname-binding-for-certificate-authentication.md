---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: "AD FS dão suporte para associação de nome de host alternativo para autenticação de certificado"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS dão suporte para associação de nome de host alternativo para autenticação de certificado

>Aplica-se a: Windows Server 2016

Em muitas redes as políticas de firewall local não podem permitir o tráfego por meio de portas não padrão como 49443. Isso tornou-se um problema ao tentar fazer a autenticação de certificado com o AD FS antes do AD FS no Windows Server 2016. Isso é porque você não pode ter associações diferentes para autenticação de dispositivo e autenticação de certificado de usuário no mesmo host. A porta padrão 443 está associada ao receber certificados de dispositivo e não pode ser alterada para dar suporte a vários associação no mesmo canal. Os resultados foram que não funcionaria autenticação com cartão inteligente e os usuários não sabem o que aconteceu como não há nenhuma indicação do que realmente aconteceu.  
  
Com o AD FS no Windows Server 2016 isso pode ser feito.
  
AD FS no Windows Server 2016 isso mudou. Agora oferecemos suporte a dois modos, o primeiro usa o mesmo host (ou seja, adfs.contoso.com) com portas diferentes (443, 49443). O segundo usado hosts diferentes (adfs.contoso.com e certauth.adfs.contoso.com) com a mesma porta (443). Isso exigirá um certificado SSL para dar suporte a "certauth. < nome do serviço adfs >" como um nome de entidade alternativo. Isso pode ser feito no momento da criação farm ou mais tarde por meio do PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Como configurar a associação de nome de host alternativo para autenticação de certificado  
Há duas maneiras que você pode adicionar a associação de nome de host alternativo para autenticação de certificado. A primeira é quando estiver configurando um novo farm AD FS com o AD FS para o Windows Server 2016, se o certificado contiver um nome de assunto alternativos (SAN), em seguida, ele será automaticamente configurado para usar o segundo método mencionado acima. Ou seja, ele irá configurar automaticamente dois hosts diferentes (sts.contoso.com e certauth.sts.contoso.com com a mesma porta. Se o certificado não contiver uma SAN, você verá um aviso informando que nomes de assunto alternativos certificado não suporta certauth.*. Veja as capturas de tela abaixo. Um dos primeiros mostra uma instalação em que o certificado tinha uma SAN e a segunda opção mostra um certificado que não.  
  
![associação de nome de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![associação de nome de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Da mesma forma, depois que o AD FS no Windows Server 2016 tenha sido implantado você pode usar o cmdlet do PowerShell: AdfsAlternateTlsClientBinding conjunto.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Quando solicitado, clique em Sim para confirmar.  E que devem sê-lo.

![associação de nome de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Referências adicionais

* [Gerenciamento de certificados SSL no AD FS e WAP no Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
