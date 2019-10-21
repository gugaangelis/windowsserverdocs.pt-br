---
title: Preparar o arquivo CAPolicy.inf
description: O arquivo CAPolicy. inf contém várias configurações que são usadas ao instalar o serviço de certificação Active Directory (AD CS) ou ao renovar o certificado de autoridade de certificação.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2af3a621991627addb94238e84cceb357fb47731
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588086"
---
# <a name="capolicyinf-syntax"></a>Sintaxe de CAPolicy. inf
>   Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O CAPolicy. inf é um arquivo de configuração que define as extensões, as restrições e outras definições de configuração que são aplicadas a um certificado de autoridade de certificação raiz e a todos os certificados emitidos pela autoridade de certificação raiz. O arquivo CAPolicy. inf deve ser instalado em um servidor host antes que a rotina de instalação para a autoridade de certificação raiz comece. Quando as restrições de segurança em uma autoridade de certificação raiz devem ser modificadas, o certificado raiz deve ser renovado e um arquivo. inf de capolicye atualizado deve ser instalado no servidor antes do início do processo de renovação.

O arquivo CAPolicy. inf é:

-   Criados e definidos manualmente por um administrador

-   Utilizado durante a criação de certificados de autoridade de certificação raiz e subordinada

-   Definido na CA de assinatura onde você assina e emite o certificado (não a AC em que a solicitação é concedida)

Depois de criar seu arquivo CAPolicy. inf, você deve copiá-lo na pasta **% systemroot%** do seu servidor antes de instalar o ADCS ou renovar o certificado de autoridade de certificação.

O arquivo CAPolicy. inf torna possível especificar e configurar uma ampla variedade de atributos e opções de CA. A seção a seguir descreve todas as opções para criar um arquivo. inf personalizado para suas necessidades específicas.

## <a name="capolicyinf-file-structure"></a>Estrutura do arquivo CAPolicy. inf

Os termos a seguir são usados para descrever a estrutura do arquivo. inf:

-   _Seção_ – é uma área do arquivo que abrange um grupo lógico de chaves. Os nomes de seção em arquivos. inf são identificados ao aparecer entre colchetes. Muitas seções, mas não todas, são usadas para configurar as extensões de certificado.

-   _Chave_ – o nome de uma entrada e é exibido à esquerda do sinal de igual.

-   _Valor_ – é o parâmetro e é exibido à direita do sinal de igual.

No exemplo a seguir, **[Version]** é a seção, **Signature** é a chave e **"\$Windows NT \$"** é o valor.

Exemplo:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Versão

Identifica o arquivo como um arquivo. inf. A versão é a única seção necessária e deve estar no início do arquivo CAPolicy. inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Lista as políticas que foram definidas pela organização e se são opcionais ou obrigatórios. Várias políticas são separadas por vírgulas. Os nomes têm significado no contexto de uma implantação específica ou em relação aos aplicativos personalizados que verificam a presença dessas políticas.

Para cada política definida, deve haver uma seção que defina as configurações para essa política específica. Para cada política, você precisa fornecer um OID (identificador de objeto) definido pelo usuário e o texto que deseja exibir como a declaração de política ou um ponteiro de URL para a declaração de política. A URL pode estar na forma de uma URL HTTP, FTP ou LDAP.

Se você for ter texto descritivo na declaração de política, as próximas três linhas do arquivo CAPolicy. inf seriam semelhantes a:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Se você pretende usar uma URL para hospedar a declaração de política de autoridade de certificação, as próximas três linhas, em vez disso, teriam a seguinte aparência:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Além disso:

-   Há suporte para várias chaves de URL e de aviso.

-   Há suporte para avisos e chaves de URL na mesma seção de política.

-   As URLs com espaços ou texto com espaços devem estar entre aspas. Isso é verdadeiro para a chave de **URL** , independentemente da seção na qual ela aparece.

Um exemplo de vários avisos e URLs em uma seção de política teria a seguinte aparência:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Você pode especificar os CDPs (pontos de distribuição) de CRL para um certificado de autoridade de certificação raiz no arquivo CAPolicy. inf.  Depois de instalar a AC, você pode configurar as URLs de CDP que a autoridade de certificação inclui em cada certificado emitido. O certificado de autoridade de certificação raiz mostra as URLs especificadas nesta seção do arquivo CAPolicy. inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Algumas informações adicionais sobre esta seção:
-   Suportar
    - HTTP 
    - URLs de arquivo
    - URLs de LDAP 
    - Várias URLs
   
    >[!IMPORTANT]
    >Não oferece suporte a URLs HTTPS.

-   As aspas devem envolver URLs com espaços.

-   Se nenhuma URL for especificada, ou seja, se a seção **[CRLDistributionPoint]** existir no arquivo, mas estiver vazia – a extensão do ponto de distribuição da CRL será omitida do certificado de autoridade de certificação raiz. Isso geralmente é preferível ao configurar uma AC raiz. O Windows não executa a verificação de revogação em um certificado de autoridade de certificação raiz, portanto, a extensão de CDP é supérflua em um certificado de autoridade de certificação raiz.

-    A CA pode publicar no arquivo UNC, por exemplo, para um compartilhamento que representa a pasta de um site em que um cliente recupera via HTTP.

-   Use esta seção somente se você estiver configurando uma AC raiz ou renovando o certificado de autoridade de certificação raiz. A AC determina as extensões de CDP de CA subordinadas.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Você pode especificar os pontos de acesso de informações de autoridade no arquivo CAPolicy. inf para o certificado de autoridade de certificação raiz.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Algumas observações adicionais sobre a seção acesso a informações da autoridade:

-   Há suporte para várias URLs.

-   Há suporte para HTTP, FTP, LDAP e URLs de arquivo. Não há suporte para URLs HTTPS.

-   Esta seção só será usada se você estiver configurando uma autoridade de certificação raiz ou renovando o certificado de autoridade de certificação raiz. As extensões AIA da AC subordinada são determinadas pela AC que emitiu o certificado da autoridade de certificação subordinada.

-   As URLs com espaços devem estar entre aspas.

-   Se nenhuma URL for especificada, ou seja, se a seção **[AuthorityInformationAccess]** existir no arquivo, mas estiver vazia – a extensão de acesso às informações da autoridade será omitida do certificado de autoridade de certificação raiz. Novamente, essa seria a configuração preferida no caso de um certificado de autoridade de certificação raiz, pois não há nenhuma autoridade mais alta do que uma autoridade de certificação raiz que precisaria ser referenciada por um link para seu certificado.

### <a name="certsrv_server"></a>certsrv_Server

Outra seção opcional do arquivo CAPolicy. inf é [certsrv_server], que é usada para especificar o comprimento da chave de renovação, o período de validade da renovação e o período de validade da CRL (lista de certificados revogados) para uma AC que está sendo renovada ou instalada. Nenhuma das chaves nesta seção é necessária. Muitas dessas configurações têm valores padrão que são suficientes para a maioria das necessidades e podem simplesmente ser omitidos do arquivo CAPolicy. inf. Como alternativa, muitas dessas configurações podem ser alteradas após a instalação da AC.

Um exemplo seria semelhante a:

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

**RenewalKeyLength** define o tamanho da chave somente para renovação. Isso só é usado quando um novo par de chaves é gerado durante a renovação do certificado de autoridade de certificação. O tamanho da chave para o certificado de autoridade de certificação inicial é definido quando a AC é instalada.

Ao renovar um certificado de autoridade de certificação com um novo par de chaves, o comprimento da chave pode ser aumentado ou diminuído. Por exemplo, se você tiver definido um tamanho de chave de autoridade de certificação raiz de 4096 bytes ou mais, descubra que você tem aplicativos Java ou dispositivos de rede que só podem dar suporte a tamanhos de chave de 2048 bytes. Se você aumentar ou diminuir o tamanho, deverá reemitir todos os certificados emitidos por essa autoridade de certificação.

**RenewalValidityPeriod** e **RenewalValidityPeriodUnits** estabelecem o tempo de vida do novo certificado de autoridade de certificação raiz ao renovar o certificado de autoridade de certificação raiz antigo. Ele se aplica somente a uma CA raiz. O tempo de vida do certificado de uma autoridade de certificação subordinada é determinado por seu superior. RenewalValidityPeriod pode ter os seguintes valores: horas, dias, semanas, meses e anos.

**CRLPeriod** e **CRLPeriodUnits** estabelecem o período de validade para a CRL base. **CRLPeriod** pode ter os seguintes valores: horas, dias, semanas, meses e anos.

**CRLDeltaPeriod** e **CRLDeltaPeriodUnits** estabelecem o período de validade da CRL delta. **CRLDeltaPeriod** pode ter os seguintes valores: horas, dias, semanas, meses e anos.

Cada uma dessas configurações pode ser configurada depois que a AC tiver sido instalada:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Lembre-se de reiniciar Active Directory serviços de certificados para que as alterações entrem em vigor.

O **LoadDefaultTemplates** só se aplica durante a instalação de uma autoridade de certificação corporativa. Essa configuração, true ou false (ou 1 ou 0), determina se a AC está configurada com qualquer um dos modelos padrão.

Em uma instalação padrão da autoridade de certificação, um subconjunto dos modelos de certificado padrão é adicionado à pasta modelos de certificado no snap-in autoridade de certificação. Isso significa que assim que o serviço do AD CS for iniciado depois que a função tiver sido instalada, um usuário ou computador com permissões suficientes poderá se registrar imediatamente em um certificado.

Talvez você não queira emitir certificados imediatamente após a instalação de uma CA, para que você possa usar a configuração LoadDefaultTemplates para impedir que os modelos padrão sejam adicionados à AC corporativa. Se não houver modelos configurados na autoridade de certificação, ele poderá emitir nenhum certificado.

**AlternateSignatureAlgorithm** configura a AC para dar suporte ao formato de assinatura PKCS \#1 v 2.1 para as solicitações de certificado e certificado de autoridade de certificação. Quando definido como 1 em uma AC raiz, o certificado de autoridade de certificação incluirá o formato de assinatura PKCS \#1 V 2.1. Quando definido em uma autoridade de certificação subordinada, a autoridade de certificação subordinada criará uma solicitação de certificado que inclui o formato de assinatura PKCS \#1 V 2.1.

**ForceUTF8** altera a codificação padrão de nomes diferenciados relativos (RDNS) em nomes diferenciados de assunto e emissor para UTF-8. Somente os RDNs que dão suporte a UTF-8, como os que são definidos como tipos de cadeia de caracteres de diretório por uma RFC, são afetados. Por exemplo, o RDN para o controlador de domínio (DC) dá suporte à codificação como IA5 ou UTF-8, enquanto o RDN de país (C) dá suporte apenas à codificação como uma cadeia de caracteres imprimível. Portanto, a diretiva ForceUTF8 afetará um RDN DC, mas não afetará um RDN C.

**EnableKeyCounting** configura a autoridade de certificação para incrementar um contador sempre que a chave de assinatura da autoridade de certificação é usada. Não habilite essa configuração a menos que você tenha um HSM (módulo de segurança de hardware) e um CSP (provedor de serviços de criptografia) associado que ofereça suporte à contagem de chaves. Nem o CSP forte da Microsoft nem o KSP (provedor de armazenamento de chaves de software) da Microsoft oferece suporte à contagem de chaves.

## <a name="create-the-capolicyinf-file"></a>Criar o arquivo CAPolicy. inf

Antes de instalar o AD CS, você configura o arquivo CAPolicy. inf com configurações específicas para sua implantação.

**Pré-requisito:** Você deve ser um membro do grupo Administradores.

1. No computador em que você está planejando instalar o AD CS, abra o Windows PowerShell, digite **notepad c:\CAPolicy.inf** e pressione Enter.

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
4. Clique em **arquivo**e, em seguida, clique em **salvar como**.

5. Navegue até a pasta% systemroot%.

6. Verifique o seguinte:

   -   **Nome do arquivo** é definido como **CAPolicy. inf**

   -   **Salvar como tipo** está definido como **Todos os Arquivos**

   -   **Codificação** é **ANSI**

7. Clique em **Salvar**.

8. Quando o programa perguntar se você deseja substituir o arquivo, clique em **Sim**.

   ![Salvar como local do arquivo CAPolicy. inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   Verifique se você salvou o CAPolicy.inf com a extensão inf. Se você não digitar **.inf** especificamente no final do nome de arquivo e selecionar as opções conforme descrito, o arquivo será salvo como um arquivo de texto e não será usado durante a instalação da AC.

9. Feche o Bloco de Notas.

> [!IMPORTANT]
>   No arquivo CAPolicy. inf, você pode ver que há uma linha especificando a URL https://pki.corp.contoso.com/pki/cps.txt. A seção Política Interna do CAPolicy.inf é mostrada apenas como um exemplo de como você poderia especificar o local de uma CPS (declaração de prática de certificação). Neste guia, você não é instruído a criar a declaração de prática de certificado (CPS).
