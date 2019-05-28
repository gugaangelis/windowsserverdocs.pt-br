---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Considerações sobre a topologia de implantação do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2af0950e52d800202235bf674545f6c47e9cd88
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190783"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Usando declarações do AD DS com o AD FS
  
  
Você pode habilitar o controle de acesso avançado para aplicativos federados usando o Active Directory Domain Services \(AD DS\)\-emissões de declarações de usuário e dispositivo junto com os serviços de Federação do Active Directory \(do AD FS \).  
  
## <a name="about-dynamic-access-control"></a>Sobre o controle de acesso dinâmico  
No Windows Server® 2012, o recurso de controle de acesso dinâmico permite que as organizações conceder acesso a arquivos com base em declarações de usuário \(que é originado pelos atributos de conta de usuário\) e declarações de dispositivo \(que é originado por atributos de conta de computador\) que são emitidos pelos serviços de domínio do Active Directory \(AD DS\). Emitida de declarações do AD DS são integrados a autenticação integrada do Windows por meio do protocolo de autenticação Kerberos.  
  
Para obter mais informações sobre o controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>O que há de novo no AD FS?  
Como uma extensão para o cenário de controle de acesso dinâmico, o AD FS no Windows Server 2012 pode agora:  
  
-   Atributos de conta de computador de acesso, além de atributos de conta de usuário no AD DS. Nas versões anteriores do AD FS, o serviço de Federação não pôde acessar os atributos de conta de computador em todos os do AD DS.  
  
-   Consuma o AD DS emitida de declarações de usuário ou dispositivo que residem em um tíquete de autenticação Kerberos. Nas versões anteriores do AD FS, o mecanismo de declarações não conseguiu ler as IDs de segurança de usuário e grupo \(SIDs\) do Kerberos, mas não foi capaz de ler qualquer informações contidas em um tíquete Kerberos de declarações.  
  
-   Transforme o AD DS emitido declarações do usuário ou dispositivo em tokens SAML que aplicativos de terceira parte confiável podem usar para executar o controle de acesso mais avançado.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Benefícios de usar o AD DS declarações com o AD FS  
Essas emissões de declarações do AD DS pode ser inseridos no tíquetes de autenticação Kerberos e usados com o AD FS para fornecer os seguintes benefícios:  
  
-   As organizações que exigem políticas de controle de acesso mais avançadas podem habilitar declarações\-acesso baseado em aplicativos e recursos usando o AD DS emissões de declarações com base nos valores de atributo armazenados no AD DS para uma determinada conta de usuário ou computador. Isso pode ajudar os administradores a reduzir a sobrecarga adicional associada à criação e gerenciamento:  
  
    -   AD DS grupos de segurança que, caso contrário, seriam usados para controlar o acesso a aplicativos e recursos que podem ser acessados por meio da autenticação integrada do Windows.  
  
    -   Relações de confiança de que, caso contrário, seriam usadas para controlar o acesso aos negócios de floresta\-à\-negócios \(B2B\) \/ aplicativos acessíveis da Internet e os recursos.  
  
-   As organizações agora podem impedir acesso não autorizado aos recursos da rede de computadores cliente com base em se armazenadas no AD DS do valor do atributo de uma conta de computador específico \(por exemplo, um nome de computador DNS\) coincide com o controle de acesso política do recurso \(por exemplo, um servidor de arquivos tiver sido ACLd com declarações\) ou a política de terceira parte confiável \(por exemplo, um declarações\-aplicativo Web com reconhecimento de\). Isso pode ajudar os administradores definam políticas de controle de acesso mais refinadas para recursos ou aplicativos que são:  
  
    -   Acessível somente por meio de autenticação integrada do Windows.  
  
    -   Acessível por meio de mecanismos de autenticação do AD FS na Internet. O AD FS pode ser usado para transformar as emissões de declarações de dispositivo em declarações do AD FS que podem ser encapsuladas em tokens SAML que podem ser consumidos por um recurso acessível da Internet ou um aplicativo de terceira parte confiável do AD DS.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Diferenças entre o AD DS e AD FS emissões de declarações  
Há dois fatores diferenciação que são importantes para entender sobre declarações que são emitidas do AD DS vs. O AD FS. Essas diferenças incluem:  
  
-   AD DS só pode emitir declarações que são encapsuladas em tíquetes Kerberos, não os tokens SAML. Para obter mais informações sobre como o AD DS emite declarações, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   AD FS pode emitir apenas declarações que são encapsuladas em tokens SAML, não os tíquetes Kerberos. Para obter mais informações sobre como o AD FS emite declarações, consulte [The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Como as declarações emitidas pelo AD DS funcionam com o AD FS  
Emitida de declarações do AD DS pode ser usado com o AD FS para acessar as declarações de usuário e dispositivo diretamente a partir contexto de autenticação, em do usuário, o vez de fazer uma chamada LDAP separada ao Active Directory. A ilustração a seguir e as etapas correspondentes discute como esse processo funciona em mais detalhes para habilitar as declarações\-com base em controle de acesso para o cenário de controle de acesso dinâmico.  
  
![usando declarações](media/UsingADDSClaimswithADFS.gif)  
  
1.  Um administrador do AD DS usa o console da Central Administrativa do Active Directory ou cmdlets do PowerShell para objetos de tipo de declaração específico permite no esquema do AD DS.  
  
2.  Um administrador do AD FS usa o console de gerenciamento do AD FS para criar e configurar o provedor de declarações e a terceira parte confiável relações de confiança com ou da pass\-por meio de ou regras de declaração de transformação.  
  
3.  Um cliente Windows tenta acessar a rede. Como parte do processo de autenticação Kerberos, o cliente apresenta seu respectivo tíquete de usuário e computador\-tíquete de concessão \(TGT\) que ainda não contém quaisquer reivindicações, ao controlador de domínio. O controlador de domínio, em seguida, procura os tipos de declaração habilitada no AD DS e inclui quaisquer declarações resultantes no tíquete Kerberos retornado.  
  
4.  Quando o usuário\/cliente tenta acessar um recurso de arquivo é acld, para exigir as declarações, eles poderão acessar o recurso porque a ID composta que apareceu do Kerberos tem essas declarações.  
  
5.  Quando o mesmo cliente tenta acessar um site da Web ou aplicativo Web que está configurado para autenticação do AD FS, o usuário é redirecionado para um servidor de Federação do AD FS está configurado para a autenticação integrada do Windows. O cliente envia uma solicitação para o controlador de domínio usando Kerberos. O controlador de domínio emite um tíquete Kerberos que contém as declarações solicitadas que o cliente, em seguida, pode apresentar ao servidor de Federação.  
  
6.  Com base na maneira como as regras de declarações foram configuradas no provedor de declarações e objetos de confiança que o administrador configurado anteriormente, o AD FS lê as declarações no tíquete Kerberos e inclui-los em um token SAML emitido para o cliente da terceira parte confiável.  
  
7.  O cliente recebe o token SAML que contém as declarações corretas e, em seguida, é redirecionado para o site.  
  
Para obter mais informações sobre como criar regras de declaração necessárias para declarações do AD DS emitido para trabalhar com o AD FS, consulte [criar uma regra para transformar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
