---
title: Preparar o arquivo CAPolicy.inf
description: O arquivo CAPolicy. inf contém várias configurações que são usadas ao instalar o serviço de certificação do Active Directory (AD CS) ou ao renovar o certificado de autoridade de certificação.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19a87df7c4f165d3b0e6c5add4bc40ff97cc87cb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446465"
---
# <a name="capolicyinf-syntax"></a>Sintaxe de CAPolicy. inf
>   Aplica-se a: Windows Server (canal semestral), Windows Server 2016

O arquivo CAPolicy. inf é um arquivo de configuração que define as extensões, restrições e outras definições de configuração que são aplicadas a um certificado de autoridade de certificação raiz e todos os certificados emitidos pela autoridade de certificação raiz. O arquivo CAPolicy. inf deve ser instalado em um servidor de host antes da rotina de instalação para a raiz de que CA começa. Quando as restrições de segurança em uma autoridade de certificação raiz devem ser modificados, o certificado raiz deve ser renovado e um arquivo CAPolicy. inf atualizado deve ser instalado no servidor antes de começa o processo de renovação.

O arquivo CAPolicy. inf é:

-   Criado e definido manualmente por um administrador

-   Utilizados durante a criação de raiz e certificados de autoridade de certificação subordinada

-   Definido na CA de assinatura em que você entre e emite o certificado (não a autoridade de certificação em que a solicitação é concedida)

Depois de criar o arquivo CAPolicy. inf, você deve copiá-lo para o **% systemroot %** pasta do seu servidor antes de instalar o AD CS ou renovar o certificado de autoridade de certificação.

O arquivo CAPolicy. inf torna possível especificar e configurar uma ampla variedade de opções e os atributos de autoridade de certificação. A seção a seguir descreve todas as opções para criar um arquivo. inf sob medido para suas necessidades específicas.

## <a name="capolicyinf-file-structure"></a>Estrutura do arquivo CAPolicy. inf

Os seguintes termos são usados para descrever a estrutura do arquivo. inf:

-   _Seção_ – é uma área do arquivo que abrange um grupo lógico de chaves. Nomes de seção em arquivos. inf são identificados por que aparece entre colchetes. Muitas, mas nem todas as seções são usadas para configurar as extensões de certificado.

-   _Chave_ – é o nome de uma entrada e aparece à esquerda do sinal de igual.

-   _Valor_ – é o parâmetro e aparece à direita do sinal de igual.

No exemplo a seguir, **[versão]** é a seção **assinatura** é a chave, e **"\$Windows NT\$"** é o valor.

Exemplo:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

Identifica o arquivo como um arquivo. inf. Versão é a única necessária seção e deve estar no início do arquivo CAPolicy. inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Lista as políticas que foram definidas pela organização, e se eles são obrigatórios ou opcionais. Várias políticas são separadas por vírgulas. Os nomes têm significado no contexto de uma implantação específica ou em relação a aplicativos personalizados que verificam a presença dessas políticas.

Para cada política definida, deve haver uma seção que define as configurações dessa política específica. Para cada política, você precisa fornecer um identificador de objeto definido pelo usuário (OID) e o texto que deseja exibir como a declaração de política ou um ponteiro de URL para a declaração de política. A URL pode ser na forma de um HTTP, FTP ou URL LDAP.

Se você tiver um texto descritivo na declaração de política, as próximas três linhas de CAPolicy. inf pareceria com:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Se você pretende usar uma URL para hospedar a declaração de política de autoridade de certificação, próximas três linhas em vez disso, pareceria com:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Além disso:

-   Há suporte para várias chaves de URL e o aviso.

-   Há suporte para chaves de aviso e a URL na mesma seção de política.

-   URLs com espaços ou texto com espaços deve ser colocados entre aspas. Isso é verdadeiro para o **URL** chave, independentemente da seção na qual ele aparece.

Um exemplo de vários avisos e URLs em uma seção de política se pareceria com:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Você pode especificar pontos de distribuição da CRL (CDPs) para um certificado de autoridade de certificação raiz no CAPolicy. inf.  Depois de instalar a autoridade de certificação, você pode configurar as URLs de CPD que inclui o a autoridade de certificação em cada certificado emitido. O certificado de autoridade de certificação raiz mostra as URLs especificadas nesta seção do arquivo CAPolicy. inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Algumas informações adicionais sobre essa seção:
-   dá suporte a:
    - HTTP 
    - URLs de arquivos
    - URLs de LDAP 
    - Várias URLs
   
    >[!IMPORTANT]
    >Não oferece suporte a URLs HTTPS.

-   Aspas devem envolver as URLs com espaços.

-   Se nenhuma URL for especificado – ou seja, se o **[CRLDistributionPoint]** seção existe no arquivo, mas está vazia – a extensão de acesso a informações da autoridade é omitida do certificado raiz da autoridade de certificação. Isso geralmente é preferível ao configurar uma autoridade de certificação raiz. Windows não executam a verificação em um certificado de autoridade de certificação raiz, portanto, a extensão CPD é supérflua em um certificado de autoridade de certificação raiz de revogação.

-    Autoridade de certificação pode publicar arquivo UNC, por exemplo, para um compartilhamento que representa a pasta de um site em que um cliente recupera via HTTP.

-   Só use esta seção se você estiver configurando uma autoridade de certificação raiz ou renovando o certificado de autoridade de certificação raiz. A autoridade de certificação determina as extensões CPD de autoridade de certificação subordinadas.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Você pode especificar os pontos de acesso de informações da autoridade no CAPolicy. inf para o certificado de autoridade de certificação raiz.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Algumas observações adicionais sobre a seção de acesso de informações da autoridade:

-   Há suporte para várias URLs.

-   Há suporte para HTTP, FTP, LDAP e URLs do arquivo. Não há suporte para URLs HTTPS.

-   Esta seção é usada somente se você estiver configurando uma autoridade de certificação raiz ou renovando o certificado de autoridade de certificação raiz. As extensões AIA de autoridade de certificação subordinada são determinadas pela autoridade de certificação que emitiu o certificado da autoridade de certificação subordinada.

-   URLs com espaços devem ser colocados entre aspas.

-   Se nenhuma URL for especificado – ou seja, se o **[AuthorityInformationAccess]** seção existe no arquivo, mas está vazia – a extensão de ponto de distribuição da CRL é omitida do certificado raiz da autoridade de certificação. Novamente, isso seria a configuração preferencial no caso de um certificado de autoridade de certificação raiz que não haja nenhuma autoridade maior do que uma autoridade de certificação que precisa ser referenciado por um link para seu certificado raiz.

### <a name="certsrvserver"></a>certsrv_Server

Outra seção opcional de CAPolicy. inf é [servidor_certsrv], que é usado para especificar o comprimento de chave de renovação, o período de validade de renovação e o período de validade CRL (lista) de revogação de certificado para uma autoridade de certificação que está sendo renovada ou instalada. Nenhuma das chaves nesta seção são necessárias. Muitas dessas configurações têm valores padrão são suficientes para a maioria das necessidades e simplesmente podem ser omitidos do arquivo CAPolicy. inf. Como alternativa, muitas dessas configurações podem ser alteradas depois que a autoridade de certificação tiver sido instalada.

Um exemplo seria:

```
[certsrv_server]
RenewalKeyLength=2048
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=5
CRLPeriod=Days
CRLPeriodUnits=2
CRLDeltaPeriod=Hours
CRLDeltaPeriodUnits=4
ClockSkewMinutes=20
LoadDefaultTemplates=True
AlternateSignatureAlgorithm=0
ForceUTF8=0
EnableKeyCounting=0
```

**RenewalKeyLength** define o tamanho da chave somente para renovação. Isso só é usado quando um novo par de chaves é gerado durante a renovação de certificado de autoridade de certificação. O tamanho da chave do certificado de autoridade de certificação inicial é definido quando a autoridade de certificação está instalada.

Durante a renovação de um certificado de autoridade de certificação com um novo par de chaves, o comprimento da chave pode seja aumentado ou diminuído. Por exemplo, se você tiver definido uma raiz de tamanho da chave de autoridade de certificação de 4096 bytes ou superior e, em seguida, descobre que você tem aplicativos Java ou dispositivos de rede que só podem oferecer suporte a tamanhos de chave de 2048 bytes. Se você aumentar ou diminuir o tamanho, você deve emitir novamente todos os certificados emitidos pela AC.

**RenewalValidityPeriod** e **RenewalValidityPeriodUnits** estabelecer o tempo de vida do novo certificado de autoridade de certificação raiz ao renovar o certificado de autoridade de certificação raiz antigo. Ele só se aplica a uma autoridade de certificação raiz. O tempo de vida do certificado de uma autoridade de certificação subordinada é determinado pelo seu superior. RenewalValidityPeriod pode ter os seguintes valores: Horas, dias, semanas, meses e anos.

**CRLPeriod** e **CRLPeriodUnits** estabelecer o período de validade da CRL base. **CRLPeriod** pode ter os seguintes valores: Horas, dias, semanas, meses e anos.

**CRLDeltaPeriod** e **CRLDeltaPeriodUnits=0** estabelecer o período de validade da CRL delta. **CRLDeltaPeriod** pode ter os seguintes valores: Horas, dias, semanas, meses e anos.

Cada uma dessas configurações pode ser configurada após a instalação de autoridade de certificação:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Lembre-se de reiniciar o Active Directory Certificate Services para todas as alterações entrem em vigor.

**LoadDefaultTemplates=0** aplica-se somente durante a instalação de uma CA corporativa. Essa configuração, o verdadeiro ou falso (ou 1 ou 0), determina se a AC está configurada com qualquer um dos modelos padrão.

Em uma instalação padrão da autoridade de certificação, um subconjunto dos modelos de certificado padrão é adicionado à pasta de modelos de certificado no snap-in Autoridade de certificação. Isso significa que assim que o serviço de AD CS é iniciado após a instalação de função de um usuário ou computador com permissões suficientes pode imediatamente registrar um certificado.

Talvez você não queira emitir certificados imediatamente depois de uma autoridade de certificação tiver sido instalada, então você pode usar a configuração LoadDefaultTemplates=0 para impedir que os modelos padrão que está sendo adicionado à autoridade de certificação corporativa. Se não houver nenhum modelo configurado na autoridade de certificação, em seguida, ele pode emitir nenhum certificado.

**Alternatesignaturealgorithm=1** configura a autoridade de certificação para dar suporte a PKCS\#formato de assinatura V2.1 1 para o certificado de autoridade de certificação e solicitações de certificado. Quando definido como 1 em uma autoridade de certificação raiz da CA incluirá o PKCS\#formato de assinatura 1 v 2.1. Quando definido em uma autoridade de certificação subordinada, a autoridade de certificação subordinada criará uma solicitação de certificado que inclui o PKCS\#formato de assinatura 1 v 2.1.

**ForceUTF8** altera o padrão de codificação de nomes distintos relativos (RDNs) no assunto e emissor nomes para UTF-8 distintos. Apenas esses RDNs que dão suporte a UTF-8, como aqueles que são definidos como tipos de cadeia de caracteres de diretório por um RFC são afetados. Por exemplo, o RDN para o componente de domínio (DC) dá suporte a codificação como IA5 ou UTF-8, enquanto o Country RDN (C) dá suporte apenas a codificação como uma cadeia de caracteres imprimíveis. A diretiva ForceUTF8 afetará, portanto, um RDN de controlador de domínio, mas não afetarão um RDN de C.

**EnableKeyCounting** configura a autoridade de certificação para incrementar um contador sempre que a chave de assinatura da autoridade de certificação é usada. Não habilite essa configuração, a menos que você tiver um módulo de segurança de Hardware (HSM) e o provedor de serviços de criptografia associados (CSP) que dá suporte à contagem de chaves. O CSP de alta segurança da Microsoft nem a Microsoft Software Key Storage KSP (provedor) suporte a chaves de contagem.


## <a name="create-the-capolicyinf-file"></a>Criar o arquivo CAPolicy. inf

Antes de instalar o AD CS, você configura o arquivo CAPolicy. inf com configurações específicas para sua implantação.

**Pré-requisito:** Você deve ser um membro do grupo Administradores.

1. No computador em que você planeja instalar o AD CS, abra o Windows PowerShell, digite **bloco de notas c:\CAPolicy.inf** e pressione ENTER.

2. Quando ele perguntar se você deseja criar um novo arquivo, clique em **Sim**.

3. Insira o seguinte como o conteúdo do arquivo:
   ```
   [Version]  
   Signature="$Windows NT$"  
   [PolicyStatementExtension]  
   Policies=InternalPolicy  
   [InternalPolicy]  
   OID=1.2.3.4.1455.67.89.5  
   Notice="Legal Policy Statement"  
   URL=https://pki.corp.contoso.com/pki/cps.txt  
   [Certsrv_Server]  
   RenewalKeyLength=2048  
   RenewalValidityPeriod=Years  
   RenewalValidityPeriodUnits=5  
   CRLPeriod=weeks  
   CRLPeriodUnits=1  
   LoadDefaultTemplates=0  
   AlternateSignatureAlgorithm=1  
   [CRLDistributionPoint]  
   [AuthorityInformationAccess]
   ```
4. Clique em **arquivo**e, em seguida, clique em **Salvar como**.

5. Navegue até a pasta % systemroot %.

6. Verifique o seguinte:

   -   **Nome do arquivo** é definido como **CAPolicy. inf**

   -   **Salvar como tipo** está definido como **Todos os Arquivos**

   -   **Codificação** é **ANSI**

7. Clique em **Salvar**.

8. Quando o programa perguntar se você deseja substituir o arquivo, clique em **Sim**.

   ![Salvar como local para o arquivo CAPolicy. inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   Verifique se você salvou o CAPolicy.inf com a extensão inf. Se você não digitar **.inf** especificamente no final do nome de arquivo e selecionar as opções conforme descrito, o arquivo será salvo como um arquivo de texto e não será usado durante a instalação da AC.

9. Feche o Bloco de Notas.

> [!IMPORTANT]
>   No CAPolicy. inf, você pode ver que há uma linha especificando o URL https://pki.corp.contoso.com/pki/cps.txt. A seção Política Interna do CAPolicy.inf é mostrada apenas como um exemplo de como você poderia especificar o local de uma CPS (declaração de prática de certificação). Neste guia, você não é instruído para criar a instrução de prática de certificado (CPS).
