---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: "Considerações de topologia de implantação do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Usando o AD DS declarações com o AD FS
  
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
Você pode habilitar o controle de acesso mais avançado para aplicativos federados usando o usuário do Active Directory Domain Services \(AD DS\)\-issued e declarações de dispositivo, juntamente com os serviços de Federação do Active Directory \(AD FS\).  
  
## <a name="about-dynamic-access-control"></a>Sobre o controle de acesso dinâmico  
No Windows Server® 2012, o recurso de controle de acesso dinâmico permite que as organizações deve conceder acesso a arquivos com base em declarações do usuário \ (que é originado pelo attributes\ de conta de usuário) e declarações de dispositivo \ (que é originado pelo attributes\ de conta de computador) que são emitidos por \(AD DS\) serviços de domínio do Active Directory. AD DS emitidos requerimentos judiciais ou Extrajudiciais são integrados de autenticação integrada do Windows por meio do protocolo de autenticação Kerberos.  
  
Para obter mais informações sobre o controle de acesso dinâmico, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>O que há de novo no AD FS?  
Como uma extensão para o cenário de controle de acesso dinâmico, AD FS no Windows Server 2012 pode agora:  
  
-   Atributos de conta de computador de acesso além dos atributos da conta de usuário no AD DS. Em versões anteriores do AD FS, o serviço de Federação não foi possível acessar os atributos de conta de computador em todos do AD DS.  
  
-   Consuma o AD DS emitidos declarações do usuário ou dispositivo que residem em um tíquete de autenticação Kerberos. Em versões anteriores do AD FS, o mecanismo de requerimentos judiciais ou Extrajudiciais foi capaz de ler o usuário e grupo segurança IDs \(SIDs\) de Kerberos mas não foi capaz de ler qualquer declarações informações contidas em um tíquete Kerberos.  
  
-   Transforme AD DS que emitiu o usuário ou declarações de dispositivo no tokens SAML terceira aplicativos podem usar para executar o controle de acesso mais avançado.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Benefícios do uso do AD DS requerimentos judiciais ou Extrajudiciais com o AD FS  
Esses AD DS emitidos requerimentos judiciais ou Extrajudiciais pode ser inseridos em tíquetes de autenticação Kerberos e usados com o AD FS para fornecer os seguintes benefícios:  
  
-   As organizações que exigem políticas de controle de acesso mais avançadas podem habilitar o acesso com base em claims\ para aplicativos e recursos usando o AD DS emitidos declarações que se baseiam nos valores de atributo armazenados no AD DS para um determinado usuário ou uma conta de computador. Isso pode ajudar os administradores para reduzir a sobrecarga adicional associada a criação e gerenciamento:  
  
    -   AD DS grupos de segurança que outra forma seriam usados para controlar o acesso aos aplicativos e recursos que são acessíveis por meio de autenticação integrada do Windows.  
  
    -   Floresta relações de confiança que outra forma seriam usadas para controlar o acesso aos Business\-to\-Business \(B2B\) \ / aplicativos acessíveis da Internet e recursos.  
  
-   As organizações agora podem evitar acesso não autorizado aos recursos de rede de computadores cliente com base em se uma conta de computador específico atributo valor armazenado no AD DS \ (por exemplo, um computador DNS name\) corresponde a política de controle de acesso do recurso \ (por exemplo, um servidor de arquivos que foi ACLd com claims\) ou a política de terceiros terceira \ (por exemplo, um reconhecimento de claims\ application\ da Web). Isso pode ajudar os administradores definam políticas de controle mais refinadas de acesso para recursos ou aplicativos que são:  
  
    -   Só estão acessíveis por meio de autenticação integrada do Windows.  
  
    -   Internet acessível por meio de mecanismos de autenticação do AD FS. AD FS pode ser usado para transformar o AD DS emitidos declarações de dispositivo em declarações do AD FS que podem ser encapsuladas em tokens SAML que podem ser consumidos por um aplicativo de terceiros terceira ou um recurso acessível da Internet.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Diferenças entre o AD DS e do AD FS emitidos requerimentos judiciais ou Extrajudiciais  
Há dois fatores diferenciados que são importantes para entender sobre declarações que são emitidas do AD DS vs. AD FS. Essas diferenças incluem:  
  
-   AD DS somente pode emitir declarações que são encapsuladas em tíquetes Kerberos, não tokens SAML. Para saber mais sobre como o AD DS emite requerimentos judiciais ou Extrajudiciais, consulte [dinâmico roteiro de conteúdo de controle de acesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   AD FS só pode emitir declarações que são encapsuladas em tokens SAML, não tíquetes Kerberos. Para saber mais sobre como o AD FS emite requerimentos judiciais ou Extrajudiciais, consulte [a função do mecanismo requerimentos judiciais ou Extrajudiciais](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Como o AD DS emitidos requerimentos judiciais ou Extrajudiciais de trabalho com o AD FS  
AD DS emitidos requerimentos judiciais ou Extrajudiciais pode ser usado com o AD FS para acessar declarações de dispositivo e usuário diretamente a partir do usuário contexto de autenticação, em vez de fazer uma chamada LDAP separada para o Active Directory. A ilustração a seguir e as etapas correspondentes discute como esse processo funciona mais detalhadamente para habilitar o controle de acesso baseado em claims\ para o cenário de controle de acesso dinâmico.  
  
![usando declarações](media/UsingADDSClaimswithADFS.gif)  
  
1.  O administrador do AD DS usa o console do Centro Administrativo do Active Directory ou cmdlets do PowerShell para objetos de tipo de declaração específica permite no esquema do AD DS.  
  
2.  O administrador do AD FS usa o console de gerenciamento do AD FS para criar e configurar o provedor de declarações e o terceiro relações de confiança com qualquer uma pass\-through ou transformação reivindicar regras.  
  
3.  Um cliente Windows tenta acessar a rede. Como parte do processo de autenticação Kerberos, o cliente apresenta o usuário e computador ticket\ de concessão tíquete \(TGT\) que não ainda conter todas as alegações, o controlador de domínio. O controlador de domínio, em seguida, procura no AD DS tipos de declaração habilitados e inclui todas as alegações resultantes no tíquete Kerberos retornado.  
  
4.  Quando o user\/cliente tenta acessar um recurso de arquivo que está ACLd exigir que as declarações, eles podem acessar o recurso porque a ID composta que foi mostrada de Kerberos tem essas declarações.  
  
5.  Quando o mesmo cliente tenta acessar um site ou aplicativo que esteja configurado para autenticação do AD FS, o usuário é redirecionado para um servidor de Federação do AD FS está configurado para autenticação integrada do Windows. O cliente envia uma solicitação para o controlador de domínio usando Kerberos. O controlador de domínio emite um tíquete Kerberos contendo as declarações solicitadas que o cliente pode apresentar para o servidor de Federação.  
  
6.  Com base na maneira como as regras de requerimentos judiciais ou Extrajudiciais foram configuradas no provedor de declarações e contar relações de confiança de terceiros que o administrador configurado anteriormente, o AD FS lê as declarações do tíquete Kerberos e inclui-los em um token SAML que emitir para o cliente.  
  
7.  O cliente recebe o token SAML contendo as declarações corretas e, em seguida, for redirecionado para o site.  
  
Para obter mais informações sobre como criar as regras de declaração necessárias para declarações do AD DS emitido para trabalhar com o AD FS, consulte [criar uma regra para transformar uma entrada reivindicar](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
