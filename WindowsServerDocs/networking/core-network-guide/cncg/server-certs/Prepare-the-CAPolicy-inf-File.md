---
title: Preparar o arquivo CAPolicy.
description: O CAPolicy.inf contém várias configurações que são usadas ao instalar o serviço de certificação do Active Directory (AD CS) ou quando renovar a autoridade de certificação certificado.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9618d4abe512b487f4f22ffde85a052c1c52ef22
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/05/2018
---
# <a name="capolicyinf-syntax"></a>Sintaxe de CAPolicy.inf
>   Aplica-se a: Windows Server (anual por canal), Windows Server 2016

O CAPolicy.inf é um arquivo de configuração que define as extensões, restrições e outras configurações que são aplicadas a um certificado da CA raiz e todos os certificados emitidos pela CA raiz. O arquivo CAPolicy.inf deve ser instalado em um servidor host antes da rotina de instalação para a raiz que CA começa. Quando as restrições de segurança em uma CA raiz devem ser modificada, o certificado raiz deve ser renovado e um arquivo CAPolicy.inf atualizado deve ser instalado no servidor antes de começa o processo de renovação.

O CAPolicy.inf é:

-   Criamos e definimos manualmente por um administrador

-   Utilizada durante a criação de certificados raiz e subordinados CA

-   Definido na CA de assinatura, onde você entra e emite o certificado (não a CA onde a solicitação for atendida)

Depois que você criou seu arquivo CAPolicy.inf, você deve copiá-lo para o **% systemroot %** pasta do seu servidor antes de instalar ADCS ou renovar o certificado da CA.

O CAPolicy.inf torna possível especificar e configurar uma ampla variedade de opções e atributos de autoridade de certificação. A seção a seguir descreve todas as opções para criar um arquivo. inf adaptado às suas necessidades específicas.

## <a name="capolicyinf-file-structure"></a>Estrutura de arquivo CAPolicy.inf

Os termos a seguir são usados para descrever a estrutura de arquivos. inf:

-   _Seção_ – é uma área do arquivo que abrange um grupo lógico de chaves. Nomes de seção em arquivos. inf são identificados por aparece entre colchetes. Muitos, mas nem todas as seções são usadas para configurar extensões de certificado.

-   _Chave_ – é o nome de uma entrada e aparece à esquerda do sinal de igual.

-   _Valor_ – é o parâmetro e aparece à direita do sinal de igual.

No exemplo a seguir, **[versão]** é a seção **assinatura** é a chave, e **"\ $ Windows NT \ $"** é o valor.

Exemplo:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Versão

Identifica o arquivo como um arquivo. inf. Versão é a única necessária seção e deve estar no início do arquivo CAPolicy.inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Lista as políticas que foram definidas pela organização, e se eles são obrigatórios ou opcionais. Várias políticas são separadas por vírgulas. Os nomes tem significado no contexto de uma implantação específica, ou em relação a aplicativos personalizados que verificar a presença dessas políticas.

Para cada política definida, deve haver uma seção que define as configurações para essa política específica. Para cada política, você precisa fornecer um identificador de objeto definido pelo usuário (OIDs) e o texto que deseja exibir como a declaração de política ou um ponteiro de URL para a declaração de política. A URL pode ser na forma de uma URL de LDAP, HTTP ou FTP.

Se você pretende ter texto descritivo na declaração de política, as próximas três linhas do CAPolicy.inf seria semelhante:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Se você pretende usar uma URL para hospedar a declaração de política de autoridade de certificação, três linhas subsequentes em vez disso, seria semelhante:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Além disso:

-   Há suporte para várias chaves de URL e aviso.

-   Chaves de aviso e a URL na mesma seção política são suportadas.

-   URLs com espaços ou texto com espaços deve ser colocados entre aspas. Isso é verdadeiro para o **URL** chave, independentemente da seção em que ele apareça.

Um exemplo de vários avisos e URLs em uma seção de política seria semelhante:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Você pode especificar pontos de distribuição de CRL (Cpds) para um certificado da CA raiz no CAPolicy.inf. Depois que a autoridade de certificação tiver sido instalada, você pode configurar as URLs de CDP que inclua a CA em cada certificado emitido. As URLs especificadas nesta seção do arquivo CAPolicy.inf estão incluídas no certificado da CA raiz em si.

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Algumas informações adicionais sobre esta seção:

-   Há suporte para várias URLs.

-   Há suporte para URLs LDAP, HTTP e FTP. URLs HTTPS não são suportados.

-   Esta seção é usada somente se você estiver configurando uma CA raiz ou renovando o certificado da CA raiz. Extensões CDP AC subordinada são determinadas pela autoridade de certificação que emite o certificado da AC subordinada.

-   URLs com espaços devem ser colocados entre aspas.

-   Se nenhuma URL for especificada – ou seja, se o **[CRLDistributionPoint]** seção existe no arquivo, mas estará vazia – a extensão de ponto de distribuição de CRL for omitida do certificado CA raiz. Isso geralmente é preferível quando estiver configurando uma CA raiz. Windows não executa a verificação em um certificado da CA raiz, então a extensão CDP é supérflua em um certificado de autoridade de certificação raiz de revogação.

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Você pode especificar os pontos de acesso de informações da autoridade no CAPolicy.inf para o certificado da CA raiz.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Algumas observações adicionais na seção de acesso de informações de autoridade:

-   Há suporte para várias URLs.

-   Há suporte para HTTP, FTP, LDAP e URLs de arquivos. URLs HTTPS não são suportados.

-   Esta seção é usada somente se você estiver configurando uma CA raiz ou renovando o certificado da CA raiz. Extensões AIA AC subordinada são determinadas pela autoridade de certificação que emitiu o certificado da AC subordinada.

-   URLs com espaços devem ser colocados entre aspas.

-   Se nenhuma URL for especificada – ou seja, se o **[AuthorityInformationAccess]** seção existe no arquivo, mas estará vazia – a extensão de ponto de distribuição de CRL for omitida do certificado CA raiz. Novamente, isso seria a configuração preferida no caso de um certificado da CA raiz que não haja nenhuma autoridade maior do que uma autoridade de certificação que precisa ser referenciado por um link para seu certificado raiz.

### <a name="certsrvserver"></a>certsrv_Server

Outra seção opcional do CAPolicy.inf é [certsrv_server], que é usado para especificar o comprimento da chave renovação, o período de validade de renovação e o período de validade do certificado revogados (CRL) de lista para uma autoridade de certificação é sendo renovada ou instalada. Nenhuma das teclas nesta seção são necessárias. Muitas dessas configurações têm valores padrão que são suficientes para a maioria das necessidades e podem simplesmente ser omitidos do arquivo CAPolicy.inf. Como alternativa, muitas dessas configurações podem ser alteradas depois que a autoridade de certificação tiver sido instalada.

Um exemplo seria semelhante:

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

**RenewalKeyLength** define o tamanho da chave somente para renovação. Isso só é usado quando um novo par de chaves é gerado durante a renovação de certificado de autoridade de certificação. O tamanho da chave para o certificado da CA inicial é definido quando a CA é instalada.

Quando renovar um certificado de autoridade de certificação com um novo par de chaves, o comprimento da chave pode ser aumentado ou diminuído. Por exemplo, se você tiver definido uma raiz de tamanho da chave da autoridade de certificação de bytes 4096 ou superior e depois descobrir que você tiver aplicativos de Java ou dispositivos de rede que somente suporta tamanhos de chave de 2048 bytes. Se você aumenta ou diminuir o tamanho, você deve relançar todos os certificados emitidos pela autoridade de certificação.

**RenewalValidityPeriod** e **RenewalValidityPeriodUnits** estabelecer o tempo de vida do novo certificado CA raiz ao renovar o certificado da CA raiz antigo. Ele só se aplica a uma CA raiz. O tempo de vida do certificado de uma AC subordinada é determinado pelo seu superior. RenewalValidityPeriod pode ter os seguintes valores: horas, dias, semanas, meses e anos.

**CRLPeriod** e **CRLPeriodUnits** estabelecer o período de validade da CRL base. **CRLPeriod** pode ter os seguintes valores: horas, dias, semanas, meses e anos.

**CRLDeltaPeriod** e **CRLDeltaPeriodUnits** estabelecer o período de validade da CRL delta. **CRLDeltaPeriod** pode ter os seguintes valores: horas, dias, semanas, meses e anos.

Cada uma dessas configurações pode ser configurada depois que a autoridade de certificação tiver sido instalada:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Lembre-se reiniciar os serviços de certificados do Active Directory para todas as alterações entrem em vigor.

**LoadDefaultTemplates** se aplica somente durante a instalação de uma CA corporativa. Essa configuração, nenhuma True ou False (ou 1 ou 0), determina se a CA é configurada com qualquer um dos modelos padrão.

Em uma instalação padrão da autoridade de certificação, um subconjunto dos modelos de certificado padrão é adicionado à pasta modelos de certificado do snap-in Autoridade de certificação. Isso significa que assim que o serviço do AD CS é iniciado após a instalação a função de um usuário ou computador com permissões suficientes pode imediatamente se registrar para obter um certificado.

Não convém emitir nenhum certificado imediatamente após a instalação de uma autoridade de certificação, portanto, você pode usar a configuração LoadDefaultTemplates para impedir que os modelos padrão sendo adicionados para a autoridade de certificação corporativa. Se não existem modelos configurados na CA-lo não pode emitir nenhum certificado.

**AlternateSignatureAlgorithm** configura a autoridade de certificação para dar suporte o formato de assinatura PKCS\ #1 v 2.1 para o certificado da CA e solicitações de certificados. Quando definido como 1 em uma raiz CA o certificado da CA incluirá o formato de assinatura PKCS\ #1 v 2.1. Quando definido em um subordinado CA, autoridade de certificação subordinada criará uma solicitação de certificado que inclui o formato de assinatura PKCS\ #1 v 2.1.

**ForceUTF8** muda o padrão de codificação de nomes distintos relativos (RDNs) no assunto e emissor nomes UTF-8 distintos. Somente essas RDNs que dão suporte a UTF-8, como aqueles que são definidas como tipos de cadeia de caracteres de diretório por um RFC, são afetados. Por exemplo, o RDN para o componente de domínio (DC) dá suporte a codificação UTF-8, ou IA5 como enquanto o Country RDN (C) só dá suporte a codificação como uma cadeia de caracteres imprimível. A diretiva ForceUTF8, portanto, afeta RDN um controlador de domínio, mas não afetará um RDN C.

**EnableKeyCounting** configura a CA para incrementar um contador sempre que a chave de assinatura da autoridade de certificação é usada. Não habilite essa configuração, a menos que você tem um módulo de segurança de Hardware (HSM) e o provedor de serviço criptográfico associado (CSP) que dá suporte à contagem de chave. CSP de alta segurança da Microsoft nem a Microsoft provedor de armazenamento de chave de Software (KSP) suporte chave contagem.


## <a name="create-the-capolicyinf-file"></a>Criar o arquivo CAPolicy.inf

Antes de instalar o AD CS, configure o arquivo CAPolicy com configurações específicas para a implantação.

**Pré-requisito:** você deve ser um membro do grupo Administradores.

1.  No computador onde você pretende instalar o AD CS, abra o Windows PowerShell, digite **bloco de notas c:.inf** e pressione ENTER.

2.  Quando solicitado a criar um novo arquivo, clique em **Sim **.

3.  Digite o seguinte como o conteúdo do arquivo:
   ```
   [Version]  
    Signature="$Windows NT$"  
    [PolicyStatementExtension]  
    Policies=InternalPolicy  
    [InternalPolicy]  
    OID=1.2.3.4.1455.67.89.5  
    Notice="Legal Policy Statement"  
    URL=http://pki.corp.contoso.com/pki/cps.txt  
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
1.  Clique em **arquivo**e clique em **Salvar como **.

2.  Navegue até a pasta % systemroot %.

3.  Verifique o seguinte:

    -   **Nome do arquivo** é definido como **CAPolicy**

    -   **Salvar como tipo** é definido como **todos os arquivos**

    -   **Codificação** é **ANSI**

4.  Clique em **salvar**.

5.  Quando você for solicitado a substituir o arquivo, clique em **Sim**.

    ![Salvar como local para o arquivo CAPolicy.inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   Certifique-se de salvar a CAPolicy.inf com a extensão de inf. Se você não digitar especificamente **. inf** no final do nome do arquivo e selecione as opções conforme descrito, o arquivo será salvo como um arquivo de texto e não será usado durante a instalação da CA.

6.  Feche o bloco de notas.

>   [!IMPORTANT]  
>   No CAPolicy.inf, você pode ver que há uma linha especificando a URL http://pki.corp.contoso.com/pki/cps.txt. A seção política interna a CAPolicy.inf apenas é mostrada como um exemplo de como você deve especificar o local de uma instrução de prática de certificado (CPS). Neste guia, você não é instruído para criar a declaração de prática de certificado (CPS).
