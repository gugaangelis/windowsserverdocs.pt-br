---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Atestado de chave de TPM
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: aa23d8df4391514d08ff1ef065af14275274dfa9
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939896"
---
# <a name="tpm-key-attestation"></a>Atestado de chave de TPM

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, engenheiro de escalonamento de suporte sênior com o grupo do Windows

> [!NOTE]
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.

## <a name="overview"></a>Visão geral
Embora o suporte para chaves protegidas por TPM tenha existido desde o Windows 8, não havia nenhum mecanismo para o CAs atestar criptograficamente que a chave privada do solicitante de certificado está realmente protegida por um Trusted Platform Module (TPM). Essa atualização permite que uma autoridade de certificação execute esse atestado e reflita Esse atestado no certificado emitido.

> [!NOTE]
> Este artigo pressupõe que o leitor esteja familiarizado com o conceito de modelo de certificado (para referência, consulte [modelos de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))). Ele também pressupõe que o leitor esteja familiarizado com a maneira de configurar CAs corporativas para emitir certificados com base em modelos de certificado (para referência, consulte [lista de verificação: configurar CAS para emitir e gerenciar certificados](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771533(v=ws.11))).

### <a name="terminology"></a>Terminologia

|Termo|Definição|
|--------|--------------|
|EK|Chave de endosso. Essa é uma chave assimétrica contida no TPM (injetada no tempo de fabricação). O EK é exclusivo para cada TPM e pode identificá-lo. O EK não pode ser alterado ou removido.|
|EKpub|Refere-se à chave pública do EK.|
|EKPriv|Refere-se à chave privada do EK.|
|EKCert|Certificado EK. Um certificado de TPM emitido pelo fabricante para EKPub. Nem todos os TPMs têm EKCert.|
|TPM|Trusted Platform Module. Um TPM é projetado para fornecer funções relacionadas à segurança com base em hardware. Um chip TPM é um processador de criptografia seguro projetado para desempenhar as operações de criptografia. O chip inclui vários mecanismos de segurança física para torná-lo resistente a adulterações nas funções de segurança do TPM por software mal-intencionado.|

### <a name="background"></a>Segundo plano
A partir do Windows 8, um Trusted Platform Module (TPM) pode ser usado para proteger a chave privada de um certificado. O KSP (provedor de armazenamento de chaves) do provedor Microsoft Platform crypto habilita esse recurso. Houve duas preocupações com a implementação:

-   Não havia nenhuma garantia de que uma chave seja realmente protegida por um TPM (alguém pode facilmente falsificar um KSP de software como um KSP de TPM com credenciais de administrador local).

-   Não foi possível limitar a lista de TPMs que têm permissão para proteger os certificados emitidos pela empresa (caso o administrador de PKI queira controlar os tipos de dispositivos que podem ser usados para obter certificados no ambiente).

### <a name="tpm-key-attestation"></a>Atestado de chaves do TPM
O atestado de chave do TPM é a capacidade da entidade que solicita um certificado para provar criptograficamente para uma AC que a chave RSA na solicitação de certificado é protegida por "a" ou "o" TPM no qual a autoridade de certificação confia. O modelo de confiança TPM é mais discutido na seção [visão geral da implantação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) mais adiante neste tópico.

### <a name="why-is-tpm-key-attestation-important"></a>Por que o atestado de chave TPM é importante?
Um certificado de usuário com uma chave atestada pelo TPM fornece maior garantia de segurança, backup por não exportável, antifalsificação e isolamento de chaves fornecidas pelo TPM.

Com o atestado de chave do TPM, agora é possível um novo paradigma de gerenciamento: um administrador pode definir o conjunto de dispositivos que os usuários podem usar para acessar recursos corporativos (por exemplo, VPN ou ponto de acesso sem fio) e ter **fortes** garantias de que nenhum outro dispositivo pode ser usado para acessá-los. Esse novo paradigma de controle de acesso é **forte** porque está vinculado a uma identidade de usuário *associada ao hardware* , que é mais forte do que uma credencial baseada em software.

### <a name="how-does-tpm-key-attestation-work"></a>Como o atestado de chave do TPM funciona?
Em geral, o atestado de chave do TPM baseia-se nos seguintes pilares:

1.  Cada TPM é fornecido com uma chave assimétrica exclusiva, chamada de *chave de endosso* (EK), gravada pelo fabricante. Nós nos referimos à parte pública dessa chave como *EKPub* e à chave privada associada como *EKPriv*. Alguns chips do TPM também têm um certificado EK emitido pelo fabricante para o EKPub. Nós nos referimos a esse certificado como *EKCert*.

2.  Uma autoridade de certificação estabelece confiança no TPM por meio de EKPub ou EKCert.

3.  Um usuário comprova à AC que a chave RSA para a qual o certificado está sendo solicitado está criptograficamente relacionada ao EKPub e que o usuário possui o EKpriv.

4.  A CA emite um certificado com um OID de política de emissão especial para indicar que a chave agora está atestada para ser protegida por um TPM.

## <a name="deployment-overview"></a><a name="BKMK_DeploymentOverview"></a>Visão geral da implantação
Nessa implantação, supõe-se que uma autoridade de certificação Enterprise do Windows Server 2012 R2 está configurada. Além disso, os clientes (Windows 8.1) são configurados para se registrarem nessa AC corporativa usando modelos de certificado.

Há três etapas para implantar o atestado de chave do TPM:

1.  **Planejar o modelo de confiança do TPM:** A primeira etapa é decidir qual modelo de confiança do TPM usar. Há três maneiras suportadas para fazer isso:

    -   **Confiança baseada em credencial do usuário:** A autoridade de certificação corporativa confia no EKPub fornecido pelo usuário como parte da solicitação de certificado e nenhuma validação é executada além das credenciais de domínio do usuário.

    -   **Relação de confiança baseada em EKCert:** A autoridade de certificação corporativa valida a cadeia EKCert fornecida como parte da solicitação de certificado em uma lista gerenciada pelo administrador de *cadeias de certificados de EK aceitáveis*. As cadeias aceitáveis são definidas por fabricante e são expressas por meio de dois repositórios de certificados personalizados na AC emissora (uma loja para os certificados intermediários e um para certificado de CA raiz). Esse modo de confiança significa que **todos os** TPMS de um determinado fabricante são confiáveis. Observe que, nesse modo, o TPMs em uso no ambiente deve conter EKCerts.

    -   **Relação de confiança baseada em EKPub:** A autoridade de certificação corporativa valida que o EKPub fornecido como parte da solicitação de certificado aparece em uma lista gerenciada pelo administrador de EKPubs permitidos. Essa lista é expressa como um diretório de arquivos em que o nome de cada arquivo nesse diretório é o hash SHA-2 do EKPub permitido. Essa opção oferece o nível de garantia mais alto, mas requer mais esforço administrativo, pois cada dispositivo é identificado individualmente. Nesse modelo de confiança, somente os dispositivos que tiveram o EKPub do TPM adicionado à lista de permissões de EKPubs têm permissão para se registrar para um certificado atestado pelo TPM.

    Dependendo de qual método é usado, a autoridade de certificação aplicará um OID de política de emissão diferente ao certificado emitido. Para obter mais detalhes sobre OIDs de política de emissão, consulte a tabela OIDs de política de emissão na seção [configurar um modelo de certificado](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) neste tópico.

    Observe que é possível escolher uma combinação de modelos de confiança do TPM. Nesse caso, a autoridade de certificação aceitará qualquer um dos métodos de atestado, e os OIDs da política de emissão refletirão todos os métodos de atestado bem-sucedidos.

2.  **Configure o modelo de certificado:** A configuração do modelo de certificado é descrita na seção [detalhes da implantação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) neste tópico. Este artigo não aborda como esse modelo de certificado é atribuído à AC corporativa ou como o acesso de registro é fornecido a um grupo de usuários. Para obter mais informações, consulte [lista de verificação: configurar CAS para emitir e gerenciar certificados](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771533(v=ws.11)).

3.  **Configurar a AC para o modelo de confiança do TPM**

    1.  **Confiança baseada em credencial do usuário:** Nenhuma configuração específica é necessária.

    2.  **Relação de confiança baseada em EKCert:** O administrador deve obter os certificados de cadeia de EKCert dos fabricantes de TPM e importá-los para dois novos repositórios de certificados, criados pelo administrador, na autoridade de certificação que executa o atestado de chave do TPM. Para obter mais informações, consulte a seção [configuração da AC](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.

    3.  **Relação de confiança baseada em EKPub:** O administrador deve obter o EKPub para cada dispositivo que precisará de certificados atestados pelo TPM e adicioná-los à lista de EKPubs permitidos. Para obter mais informações, consulte a seção [configuração da AC](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.

    > [!NOTE]
    > -   Este recurso requer o Windows 8.1/Windows Server 2012 R2.
    > -   Não há suporte para o atestado de chave do TPM para KSPs de cartão inteligente de terceiros. O KSP do provedor de criptografia da plataforma Microsoft deve ser usado.
    > -   O atestado de chave do TPM funciona somente para chaves RSA.
    > -   O atestado de chave do TPM não tem suporte para uma AC autônoma.
    > -   O atestado de chave do TPM não dá suporte ao [processamento de certificado não persistente](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff934598(v=ws.10)).

## <a name="deployment-details"></a><a name="BKMK_DeploymentDetails"></a>Detalhes de implantação

### <a name="configure-a-certificate-template"></a><a name="BKMK_ConfigCertTemplate"></a>Configurar um modelo de certificado
Para configurar o modelo de certificado para o atestado de chave do TPM, execute as seguintes etapas de configuração:

1.  Guia **Compatibilidade**

    Na seção **configurações de compatibilidade** :

    -   Verifique se o **Windows Server 2012 R2** está selecionado para a **autoridade de certificação**.

    -   Verifique se **Windows 8.1/Windows Server 2012 R2** está selecionado para o **destinatário do certificado**.

    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)

2.  Guia **Criptografia**

    Verifique se o **provedor de armazenamento de chaves** está selecionado para a categoria de **provedor** e **RSA** está selecionado para o **nome do algoritmo**. Verifique se as **solicitações devem usar um dos seguintes provedores** está selecionado e a opção **provedor de criptografia de plataforma da Microsoft** está selecionada em **provedores**.

    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)

3.  Guia **atestado de chave**

    Esta é uma nova guia para o Windows Server 2012 R2:

    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)

    Escolha um modo de atestado dentre as três opções possíveis.

    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)

    -   **Nenhum:** Implica que o atestado de chave não deve ser usado

    -   **Necessário, se o cliente for compatível:** Permite que os usuários em um dispositivo que não dão suporte ao atestado de chave do TPM continuem a registrar-se nesse certificado. Os usuários que podem executar o atestado serão diferenciados por um OID de política de emissão especial. Alguns dispositivos podem não ser capazes de realizar o atestado devido a um TPM antigo que não dá suporte ao atestado de chave ou ao dispositivo que não tem um TPM.

    -   **Necessário:** O cliente *deve* executar o atestado de chave do TPM, caso contrário, a solicitação de certificado falhará.

    Em seguida, escolha o modelo de confiança do TPM. Há novamente três opções:

    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)

    -   **Credenciais do usuário:** Permitir que um usuário de autenticação comprovando um TPM válido especificando suas credenciais de domínio.

    -   **Certificado de endosso:** O EKCert do dispositivo deve validar por meio de certificados de AC intermediárias do TPM gerenciados pelo administrador para um certificado de autoridade de certificação raiz gerenciado pelo administrador. Se você escolher essa opção, deverá configurar repositórios de certificados EKCA e EKRoot na AC emissora conforme descrito na seção  [configuração da autoridade de certificação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.

    -   **Chave de endosso:** O EKPub do dispositivo deve aparecer na lista gerenciada pelo administrador PKI. Essa opção oferece o nível de garantia mais alto, mas requer mais esforço administrativo. Se você escolher essa opção, deverá configurar uma lista EKPub na AC emissora, conforme descrito na seção [configuração da autoridade de certificação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.

    Por fim, decida qual política de emissão será mostrada no certificado emitido. Por padrão, cada tipo de imposição tem um identificador de objeto associado (OID) que será inserido no certificado se ele passar esse tipo de imposição, conforme descrito na tabela a seguir. Observe que é possível escolher uma combinação de métodos de imposição. Nesse caso, a AC aceitará qualquer um dos métodos de atestado, e o OID da política de emissão refletirá todos os métodos de atestado que tiveram êxito.

    **OIDs da política de emissão**

    |OID|Tipo de atestado de chave|DESCRIÇÃO|Nível de garantia|
    |-------|------------------------|---------------|-------------------|
    |1.3.6.1.4.1.311.21.30|EK|"EK verificado": para lista de EK gerenciada pelo administrador|Alto|
    |1.3.6.1.4.1.311.21.31|Certificado de endosso|"Certificado EK verificado": quando a cadeia de certificados EK é validada|Médio|
    |1.3.6.1.4.1.311.21.32|Credenciais de usuário|"EK confiável em uso": para EK atestado pelo usuário|Baixo|

    Os OIDs serão inseridos no certificado emitido se **incluir políticas de emissão** estiver selecionada (a configuração padrão).

    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)

    > [!TIP]
    > Um uso potencial de ter a OID presente no certificado é limitar o acesso a redes VPN ou sem fio a determinados dispositivos. Por exemplo, sua política de acesso pode permitir conexão (ou acesso a uma VLAN diferente) se o OID 1.3.6.1.4.1.311.21.30 estiver presente no certificado. Isso permite que você limite o acesso a dispositivos cuja EK do TPM está presente na lista EKPUB.

### <a name="ca-configuration"></a><a name="BKMK_CAConfig"></a>Configuração da AC

1.  **Configurar repositórios de certificados EKCA e EKROOT em uma AC emissora**

    Se você escolheu **certificado de endosso** para as configurações de modelo, execute as seguintes etapas de configuração:

    1.  Use o Windows PowerShell para criar dois novos repositórios de certificados no servidor de autoridade de certificação (CA) que executará o atestado de chave do TPM.

    2.  Obtenha os certificados de autoridade de certificação intermediários e raiz do (s) fabricante (es) que você deseja permitir em seu ambiente corporativo. Esses certificados devem ser importados para os repositórios de certificados criados anteriormente (EKCA e EKROOT) conforme apropriado.

    O script do Windows PowerShell a seguir executa essas duas etapas. No exemplo a seguir, o fabricante do TPM da Fabrikam forneceu um certificado raiz *FabrikamRoot. cer* e um certificado de AC emissora *Fabrikamca. cer*.

    ```powershell
    PS C:>\cd cert:
    PS Cert:\>cd .\\LocalMachine
    PS Cert:\LocalMachine> new-item EKROOT
    PS Cert:\ LocalMachine> new-item EKCA
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT
    ```

2.  **Configurar lista de EKPUB se estiver usando o tipo de atestado de EK**

    Se você escolher **chave de endosso** nas configurações do modelo, as próximas etapas de configuração serão criar e configurar uma pasta na AC emissora, contendo arquivos de 0 byte, cada um com o nome do hash SHA-2 de uma EK permitida. Essa pasta serve como uma "lista de permissões" de dispositivos que têm permissão para obter certificados de atestado de chave do TPM. Como você deve adicionar manualmente o EKPUB para cada dispositivo que requer um certificado atestado, ele fornece à empresa uma garantia dos dispositivos que estão autorizados a obter certificados de atestado de chave do TPM. A configuração de uma AC para esse modo requer duas etapas:

    1.  **Crie a entrada de registro EndorsementKeyListDirectories:** Use a ferramenta de linha de comando certutil para configurar os locais de pasta onde EKpubs confiáveis são definidos conforme descrito na tabela a seguir.

        |Operação|Sintaxe do comando|
        |-------------|------------------|
        |Adicionar locais de pasta|certutil.exe-setreg CA\EndorsementKeyListDirectories + " <folder> "|
        |Remover locais de pasta|certutil.exe-setreg CA\EndorsementKeyListDirectories-" <folder> "|

        O EndorsementKeyListDirectories no comando certutil é uma configuração do registro, conforme descrito na tabela a seguir.

        |Nome do valor|Type|Dados|
        |--------------|--------|--------|
        |EndorsementKeyListDirectories|REG_MULTI_SZ|<caminho LOCAL ou UNC para EKPUB permitir lista (s) ><p>Exemplo:<p>*\\\blueCA.contoso.com\ekpub*<p>*\\\bluecluster1.contoso.com\ekpub*<p>D:\ekpub|

        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>

        O *EndorsementKeyListDirectories* conterá uma lista de caminhos de sistema de arquivos locais ou UNC, cada um apontando para uma pasta à qual a autoridade de certificação tem acesso de leitura. Cada pasta pode conter zero ou mais entradas de lista de permissões, em que cada entrada é um arquivo com um nome que é o hash SHA-2 de um EKpub confiável, sem extensão de arquivo.
        Criar ou editar essa configuração de chave do registro requer uma reinicialização da autoridade de certificação, assim como as definições de configuração de registro de autoridade de certificação existentes. No entanto, as edições na definição de configuração entrarão em vigor imediatamente e não exigirão que a AC seja reiniciada.

        > [!IMPORTANT]
        > Proteja as pastas na lista contra violação e acesso não autorizado Configurando permissões para que somente administradores autorizados tenham acesso de leitura e gravação. A conta de computador da autoridade de certificação requer somente acesso de leitura.

    2.  **Preencha a lista EKPUB:** Use o seguinte cmdlet do Windows PowerShell para obter o hash de chave pública do endosso do TPM usando o Windows PowerShell em cada dispositivo e, em seguida, envie esse hash de chave pública para a AC e armazene-o na pasta EKPubList

        ```powershell
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file
        ```

## <a name="troubleshooting"></a>Solução de problemas

### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Os campos de atestado de chave não estão disponíveis em um modelo de certificado
Os campos de atestado de chave não estarão disponíveis se as configurações do modelo não atenderem aos requisitos de atestado. Os motivos comuns são:

1.  As configurações de compatibilidade não estão configuradas corretamente. Verifique se eles estão configurados da seguinte maneira:

    1.  **Autoridade de certificação**: **Windows Server 2012 R2**

    2.  **Destinatário do certificado**: **Windows 8.1/Windows Server 2012 R2**

2.  As configurações de criptografia não estão configuradas corretamente. Verifique se eles estão configurados da seguinte maneira:

    1.  **Categoria do provedor**: **provedor de armazenamento de chaves**

    2.  **Nome do algoritmo**: **RSA**

    3.  **Provedores**: **provedor de criptografia de plataforma Microsoft**

3.  As configurações de tratamento de solicitação não estão configuradas corretamente. Verifique se eles estão configurados da seguinte maneira:

    1.  A opção **permitir que a chave privada seja exportada** não deve ser selecionada.

    2.  A opção **arquivar chave privada de criptografia da entidade** não deve ser selecionada.

### <a name="verification-of-tpm-device-for-attestation"></a>Verificação do dispositivo TPM para atestado
Use o cmdlet do Windows PowerShell, **Confirm-CAEndorsementKeyInfo**, para verificar se um dispositivo TPM específico é confiável para atestado por CAS. Há duas opções: uma para verificar o EKCert e a outra para verificar um EKPub. O cmdlet é executado localmente em uma autoridade de certificação ou em CAs remotas usando a comunicação remota do Windows PowerShell.

1.  Para verificar a confiança em um EKPub, execute as duas etapas a seguir:

    1.  **Extraia o EKPub do computador cliente:** O EKPub pode ser extraído de um computador cliente por meio de **Get-TpmEndorsementKeyInfo**. Em um prompt de comando com privilégios elevados, execute o seguinte:

        ```
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256
        ```

    2.  **Verifique a relação de confiança em um EKCert em um computador de autoridade de certificação:** Copie a cadeia de caracteres extraída (o hash SHA-2 do EKPub) para o servidor (por exemplo, por email) e passe-o para o cmdlet Confirm-CAEndorsementKeyInfo. Observe que esse parâmetro deve ter 64 caracteres.

        ```
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>
        ```

2.  Para verificar a confiança em um EKCert, execute as duas etapas a seguir:

    1.  **Extraia o EKCert do computador cliente:** O EKCert pode ser extraído de um computador cliente por meio de **Get-TpmEndorsementKeyInfo**. Em um prompt de comando com privilégios elevados, execute o seguinte:

        ```
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```

    2.  **Verifique a relação de confiança em um EKCert em um computador de autoridade de certificação:** Copie o EKCert extraído (EkCert. cer) para a CA (por exemplo, por email ou xcopy). Por exemplo, se você copiar o arquivo de certificado da pasta "c:\diagnose" no servidor de AC, execute o seguinte para concluir a verificação:

        ```
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo
        ```

## <a name="see-also"></a>Consulte Também
[Visão geral](/previous-versions/windows/it-pro/windows-8.1-and-8/jj131725(v=ws.11)) 
 da tecnologia Trusted Platform Module [Recurso externo: Trusted Platform Module](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)
