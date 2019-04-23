---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Atestado de chave de TPM
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3cfc047fe1a66617abbda1de5f2c5842dcbdeb12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862877"
---
# <a name="tpm-key-attestation"></a>Atestado de chave de TPM

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Engenheiro de escalonamento de suporte sênior Justin Turner com o grupo do Windows  
  
> [!NOTE]  
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.  
  
## <a name="overview"></a>Visão geral  
Enquanto o suporte para chaves protegidas por TPM existe desde o Windows 8, não havia nenhum mecanismos para CAs a atestarem criptograficamente a que a chave privada do solicitante de certificado, na verdade, é protegida por um TPM (Trusted Platform Module). Essa atualização permite que uma autoridade de certificação para executar esse Atestado e para refletir essa Atestado no certificado emitido.  
  
> [!NOTE]  
> Este artigo pressupõe que o leitor esteja familiarizado com o conceito de modelo de certificado (para referência, consulte [modelos de certificado](https://technet.microsoft.com/library/cc730705.aspx)). Ele também pressupõe que o leitor esteja familiarizado com a configuração de autoridades de certificação corporativas para emitir certificados com base em modelos de certificado (para referência, consulte [lista de verificação: Configurar autoridades de certificação a ser emitido e gerenciar certificados](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminologia  
  
|Termo|Definição|  
|--------|--------------|  
|EK|Chave de endosso. Isso é uma chave assimétrica contida dentro do TPM (injetado no tempo de fabricação). A chave de endosso é exclusiva para cada TPM e possa identificá-la. A chave de endosso não pode ser alterada ou removida.|  
|EKpub|Refere-se a chave pública da chave de endosso.|  
|EKPriv|Refere-se a chave privada da chave de endosso.|  
|EKCert|Certificado EK. Um TPM fabricante certificado emitido para EKPub. Nem todos os TPMs têm EKCert.|  
|TPM|Trusted Platform Module. Um TPM foi projetado para fornecer funções de segurança baseada em hardware. Um chip TPM é um processador de criptografia seguro projetado para desempenhar as operações de criptografia. O chip inclui vários mecanismos de segurança física para torná-lo resistente a adulterações nas funções de segurança do TPM por software mal-intencionado.|  
  
### <a name="background"></a>Histórico  
Começando com o Windows 8, um Trusted Platform Module (TPM) pode ser usado para proteger a chave privada de um certificado. A Microsoft plataforma Crypto provedor de armazenamento KSP (provedor) habilita esse recurso. Havia duas preocupações com a implementação:  

-   Não havia nenhuma garantia de que uma chave, na verdade, está protegida por um TPM (alguém pode facilmente falsificar um KSP de software como um KSP do TPM com credenciais de administrador local).

-   Não foi possível limitar a lista de TPMs que têm permissão para proteger a empresa que emitiu os certificados (no caso em que o administrador de PKI deseja controlar os tipos de dispositivos que podem ser usados para obter certificados no ambiente).  

### <a name="tpm-key-attestation"></a>Atestado de chaves do TPM  
Atestado de chaves do TPM é a capacidade da entidade que está solicitando um certificado para comprovar criptograficamente para uma autoridade de certificação, que a chave RSA na solicitação de certificado é protegida por TPM de "um" ou "a" AC confia. O modelo de confiança do TPM é discutido em mais de [visão geral da implantação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) seção mais adiante neste tópico.  
  
### <a name="why-is-tpm-key-attestation-important"></a>Por que o atestado de chaves do TPM é importante?  
Um certificado de usuário com uma chave de Atestado por TPM oferece maior garantia de segurança, backup não exportabilidade, insistindo em anti e isolamento de chaves fornecidas pelo TPM.  
  
Atestado de chaves do TPM, um novo paradigma de gerenciamento agora é possível: Um administrador pode definir o conjunto de dispositivos que os usuários podem usar para acessar recursos corporativos (por exemplo, VPN ou ponto de acesso sem fio) e ter **forte** garante que nenhum outro dispositivo pode ser usado para acessá-los. Esse novo paradigma de controle de acesso é **forte** porque ele está associado a um *associadas ao hardware* identidade de usuário, que é mais forte do que uma credencial com base em software.
  
### <a name="how-does-tpm-key-attestation-work"></a>Como funciona o atestado de chaves do TPM?  
Em geral, o atestado de chaves do TPM é baseado no pilares a seguir:  
  
1.  Cada TPM é fornecido com uma chave assimétrica exclusiva, chamada a *chave de endosso* (EK), gravado pelo fabricante. A parte pública dessa chave como nos referimos *EKPub* e a chave privada associada como *EKPriv*. Alguns chips TPM também tem um certificado EK que é emitido pelo fabricante para o EKPub. Nós nos referimos a esse certificado como *EKCert*.  
  
2.  Uma autoridade de certificação estabelece a relação de confiança no TPM por meio de EKPub ou EKCert.  
  
3.  Um usuário de prova para a autoridade de certificação que a chave RSA para o qual o certificado está sendo solicitado criptograficamente está relacionada ao EKPub e que o usuário possui o EKpriv.  
  
4.  A autoridade de certificação emite um certificado com uma política de emissão especial OID para indicar que a chave é agora atestada para ser protegida por um TPM.  
  
## <a name="BKMK_DeploymentOverview"></a>Visão geral da implantação  
Nessa implantação, supõe-se que uma autoridade de certificação do Windows Server 2012 R2 corporativa está configurada. Além disso, os clientes (Windows 8.1) é configurados para registrar em relação a essa autoridade de certificação corporativa usando modelos de certificado. 

Há três etapas para implantar o atestado de chaves do TPM:  
  
1.  **Planeje o modelo de confiança do TPM:** A primeira etapa é decidir qual modelo de relação de confiança do TPM para usar. Há 3 modos com suporte para fazer isso:  
  
    -   **Relação de confiança com base em credenciais de usuário:** A autoridade de certificação corporativa confia o EKPub fornecido pelo usuário como parte da solicitação de certificado e nenhuma validação é executada diferentes de credenciais de domínio do usuário.  
  
    -   **Com base em EKCert de confiança:** A autoridade de certificação corporativa valida a cadeia de EKCert é fornecida como parte da solicitação de certificado em relação a uma lista de gerenciados pelo administrador do *aceitáveis cadeias de certificado EK*. Cadeias de aceitáveis são definidos por fabricante e são expressos por meio de dois repositórios de certificado personalizado na autoridade de certificação emissora (um repositório para intermediários) e outro para certificados de autoridade de certificação raiz. Significa que, neste modo de confiança **todos os** TPMs de um determinado fabricante são confiáveis. Observe que, nesse modo, os TPMs em uso no ambiente devem conter EKCerts.
  
    -   **Relação de confiança com base em EKPub:** A autoridade de certificação corporativa valida que o EKPub fornecido como parte da solicitação de certificado aparece em uma lista de gerenciados pelo administrador de permissão EKPubs. Essa lista é expressa como um diretório de arquivos onde o nome de cada arquivo nesse diretório é o hash SHA-2 a EKPub permitido. Essa opção oferece o mais alto nível de garantia, mas requer mais esforço administrativo, porque cada dispositivo individualmente é identificado. Nesse modelo de relação de confiança, somente os dispositivos que tiveram EKPub do seu TPM adicionado à lista de permissões de EKPubs têm permissão para registrar um certificado de atestado de TPM.  
  
    Dependendo de qual método é usado, a autoridade de certificação será aplicada a uma política diferente de emissão OID para o certificado emitido. Para obter mais detalhes sobre a política de emissão OIDs, consulte a tabela OIDs da política de emissão a [configurar um modelo de certificado](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) neste tópico.  
  
    Observe que é possível escolher uma combinação de modelos de relação de confiança do TPM. Nesse caso, a autoridade de certificação aceitará qualquer um dos métodos de Atestado e a política de emissão que OIDs refletirão todos os métodos de Atestado que tenha êxito.  
  
2.  **Configure o modelo de certificado:** Configurar o modelo de certificado é descrito o [detalhes da implantação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) neste tópico. Este artigo não abordar como esse modelo de certificado é atribuído para a autoridade de certificação corporativa ou como registrar acesso é concedido a um grupo de usuários. Para obter mais informações, consulte [lista de verificação: Configurar autoridades de certificação a ser emitido e gerenciar certificados](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurar a autoridade de certificação para o modelo de confiança do TPM**  
  
    1.  **Relação de confiança com base em credenciais de usuário:** Nenhuma configuração específica é necessária.  
  
    2.  **Com base em EKCert de confiança:** O administrador deve obter os certificados da cadeia de EKCert de fabricantes de TPM e importá-los para dois novos repositórios de certificados, criados pelo administrador, na autoridade de certificação que executam o atestado de chaves do TPM. Para obter mais informações, consulte o [configuração de autoridade de certificação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.  
  
    3.  **Relação de confiança com base em EKPub:** O administrador deve obter o EKPub para cada dispositivo que precisa de certificados de atestado de TPM e adicioná-los à lista de permitidos EKPubs. Para obter mais informações, consulte o [configuração de autoridade de certificação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.  
  
    > [!NOTE]  
    > -   Este recurso requer Windows 8.1 / Windows Server 2012 R2.  
    > -   Não há suporte para atestado de chaves do TPM para cartão inteligente de terceiros KSPs. Provedor de criptografia Microsoft plataforma KSP deve ser usado.  
    > -   Atestado de chaves do TPM funciona somente para chaves RSA.  
    > -   Não há suporte para atestado de chaves do TPM para uma autoridade de certificação autônoma.  
    > -   Não oferece suporte a atestado de chaves do TPM [processamento de certificado não persistente](https://technet.microsoft.com/library/ff934598).  
  
## <a name="BKMK_DeploymentDetails"></a>Detalhes da implantação  
  
### <a name="BKMK_ConfigCertTemplate"></a>Configurar um modelo de certificado  
Para configurar o modelo de certificado para atestado de chaves do TPM, siga as etapas de configuração a seguir:  
  
1.  **Compatibilidade** guia  
  
    No **configurações de compatibilidade** seção:  
  
    -   Certifique-se **Windows Server 2012 R2** está selecionado para o **autoridade de certificação**.  
  
    -   Certifique-se **Windows 8.1 / Windows Server 2012 R2** está selecionado para o **destinatário do certificado**.  
  
    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **Criptografia** guia  
  
    Certifique-se **Key Storage Provider** está selecionado para o **categoria de provedor** e **RSA** está selecionado para o **nome do algoritmo**. Certifique-se **as solicitações devem usar um dos seguintes provedores** está selecionado e o **provedor de criptografia de plataforma Microsoft** opção for selecionada na **provedores**.  
  
    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **Atestado de chaves** guia  
  
    Isso é uma nova guia do Windows Server 2012 R2:  
  
    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Escolha um modo de Atestado entre as três opções possíveis.  
  
    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **Nenhum:** Implica que não deve ser usado atestado de chaves  
  
    -   **É necessário, se o cliente é compatível com:** Permite que os usuários em um dispositivo que não oferece suporte a atestado de chaves do TPM para continuar a registrar para o certificado. Os usuários que podem executar Atestado serão ser diferenciados com uma política de emissão especial OID. Alguns dispositivos não podem ser capazes de executar o Atestado por causa de um TPM antigo que não dão suporte ao atestado de chave ou o dispositivo não ter que um TPM.
  
    -   **Necessário:** Cliente *deve* executar atestado de chaves do TPM, caso contrário, a solicitação de certificado falhará.  
  
    Em seguida, escolha o modelo de confiança do TPM. Novamente, há três opções:  
  
    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **Credenciais do usuário:** Permitir que um usuário de autenticação para um TPM válido garantir especificando suas credenciais de domínio.  
  
    -   **Certificado de endosso:** O EKCert do dispositivo deve validar por meio de gerenciado pelo administrador TPM certificados de AC intermediários para um certificado de autoridade de certificação raiz gerenciado pelo administrador. Se você escolher essa opção, você deve configurar EKCA e EKRoot repositórios na CA de emissão de certificado conforme descrito na [configuração de autoridade de certificação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.  
  
    -   **Chave de endosso:** O EKPub do dispositivo deve aparecer na lista de PKI gerenciado pelo administrador. Essa opção oferece o mais alto nível de garantia, mas requer mais esforço administrativo. Se você escolher essa opção, você deve configurar uma lista EKPub na AC emissora conforme descrito na [configuração de autoridade de certificação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) neste tópico.  
  
    Finalmente, decida qual política de emissão para mostrar no certificado emitido. Por padrão, cada tipo de imposição tem um identificador de objeto associado (OID) que será inserido no certificado se ele passa esse tipo de imposição, conforme descrito na tabela a seguir. Observe que é possível escolher uma combinação de métodos de imposição. Nesse caso, a autoridade de certificação aceitará qualquer um dos métodos de Atestado e a política de emissão OID refletirão todos os métodos de Atestado que foram bem-sucedidas.  
  
    **OIDs da política de emissão**  
  
    |OID|Tipo de atestado de chaves|Descrição|Nível de garantia|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|"EK Verified":   Para obter lista de gerenciados pelo administrador de EK|Alto|  
    |1.3.6.1.4.1.311.21.31|Certificado de endosso|"EK Certificate Verified": Quando a cadeia de certificado EK é validada|Médio|  
    |1.3.6.1.4.1.311.21.32|Credenciais de usuário|"EK confiável em uso": Para EK atestados por usuário|Baixo|  
  
    Os OIDs serão inseridos no certificado emitido se **incluir diretivas de emissão** é selecionada (a configuração padrão).  
  
    ![Atestado de chave de TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > Um uso potencial de ter o OID presente no certificado é limitar o acesso a VPN ou a determinados dispositivos de rede sem fio. Por exemplo, sua política de acesso pode permitir conexão (ou acesso a uma VLAN diferente), se OID 1.3.6.1.4.1.311.21.30 estiver presente no certificado. Isso permite que você limite o acesso a dispositivos EK cujo TPM está presente na lista EKPUB.  
  
### <a name="BKMK_CAConfig"></a>Configuração de autoridade de certificação  
  
1.  **Configurar EKCA e EKROOT repositórios de certificados em uma CA de emissão**  
  
    Se você escolheu **certificado de endosso** para as configurações de modelo, execute as seguintes etapas de configuração:  
  
    1.  Use o Windows PowerShell para criar dois novos repositórios de certificados no servidor de autoridade (CA) de certificação que executará o atestado de chaves do TPM.  
  
    2.  Obter intermediários e raiz do certificado de autoridade de certificação de fabricantes de que você deseja permitir em seu ambiente corporativo. Esses certificados devem ser importados em repositórios de certificado criado anteriormente (EKCA e EKROOT) conforme apropriado.  
  
    O seguinte script do Windows PowerShell executa as duas etapas. No exemplo a seguir, o fabricante do TPM Fabrikam forneceu um certificado raiz *FabrikamRoot.cer* e um certificado de autoridade de certificação emissora *Fabrikamca.cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Configurar lista EKPUB se usando o tipo de Atestado EK**  
  
    Se você escolheu **chave de endosso** nas configurações do modelo, as próximas etapas de configuração são criar e configurar uma pasta na AC emissora, que contém os arquivos de 0 byte, cada chamada para o hash SHA-2 de um EK permitido. Essa pasta serve como uma "lista de permissões" de dispositivos que têm permissão para obter certificados de atestado de chave TPM. Porque você deve adicionar manualmente o EKPUB para cada dispositivo que requer um certificado attested, ele fornece uma garantia de que os dispositivos que estão autorizados a obter certificados de atestado de TPM chave da empresa. Configurar uma autoridade de certificação para esse modo exige duas etapas:  
  
    1.  **Crie a entrada do registro EndorsementKeyListDirectories:** Use a ferramenta de linha de comando Certutil para configurar os locais de pasta onde EKpubs confiáveis são definidos como descrito na tabela a seguir.  
  
        |Operação|Sintaxe de comando|  
        |-------------|------------------|  
        |Adicionar locais de pasta|certutil.exe -setreg CA\EndorsementKeyListDirectories +"<folder>"|  
        |Remover locais de pasta|certutil.exe -setreg CA\EndorsementKeyListDirectories -"<folder>"|  
  
        O EndorsementKeyListDirectories no comando certutil é uma configuração de registro conforme descrito na tabela a seguir.  
  
        |Nome do valor|Tipo|Dados|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< caminho LOCAL ou UNC para EKPUB permitir lista (s) ><br /><br />Exemplo:<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* conterá uma lista de UNC ou caminhos de sistema de arquivos local, cada um apontando para uma pasta que a autoridade de certificação tem acesso de leitura. Cada pasta pode conter zero ou mais permitem entradas da lista, onde cada entrada é um arquivo com um nome que é o SHA-2 hash de um EKpub confiável, sem extensão de arquivo. 
        Criar ou editar esta configuração de chave do registro requer uma reinicialização da autoridade de certificação, assim como definições de configuração do registro da AC existentes. No entanto, edições para o parâmetro de configuração entrarão em vigor imediatamente e não exigirá a autoridade de certificação seja reiniciado.  
  
        > [!IMPORTANT]  
        > Proteja as pastas na lista contra violação e acesso não autorizado por meio da configuração de permissões para que somente administradores autorizados leu e acesso de gravação. A conta de computador da autoridade de certificação requer acesso somente leitura.  
  
    2.  **Preencha a lista EKPUB:** Use o seguinte cmdlet do Windows PowerShell para obter o hash da chave pública de EK o TPM usando o Windows PowerShell em cada dispositivo e, em seguida, enviar esse hash da chave pública para a autoridade de certificação e armazená-lo na pasta EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Solução de problemas  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Campos de atestado de chave não estão disponíveis em um modelo de certificado  
Os campos de atestado de chave não estão disponíveis se as configurações de modelo não satisfizerem os requisitos de Atestado. Motivos comuns são:  
  
1.  As configurações de compatibilidade não estão configuradas corretamente. Certifique-se de que eles são configurados da seguinte maneira:  
  
    1.  **Autoridade de Certificação**: **Windows Server 2012 R2**  
  
    2.  **Destinatário do certificado**: **Windows 8.1/Windows Server 2012 R2**  
  
2.  As configurações de criptografia não estão configuradas corretamente. Certifique-se de que eles são configurados da seguinte maneira:  
  
    1.  **Categoria de provedor**: **Provedor de armazenamento de chaves**  
  
    2.  **Nome do algoritmo**: **RSA**  
  
    3.  **Provedores de**: **Provedor de criptografia da plataforma Microsoft**  
  
3.  As configurações de tratamento de solicitação não estão configuradas corretamente. Certifique-se de que eles são configurados da seguinte maneira:  
  
    1.  O **permitir que a chave privada seja exportada** opção não deve ser selecionada.  
  
    2.  O **arquivar chave privada de criptografia de requerente** opção não deve ser selecionada.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Verificação de dispositivo do TPM para Atestado  
Use o cmdlet do Windows PowerShell **CAEndorsementKeyInfo confirmar**, para verificar se um dispositivo específico do TPM é confiável para Atestado por autoridades de certificação. Há duas opções: uma para verificar o EKCert e o outro para verificar um EKPub. O cmdlet é executar localmente em uma autoridade de certificação ou em CAs remoto por meio de comunicação remota do Windows PowerShell.  
  
1.  Para verificar a relação de confiança em um EKPub, siga as etapas a seguir:  
  
    1.  **Extraia o EKPub do computador cliente:** O EKPub pode ser extraído de um computador cliente por meio **Get-TpmEndorsementKeyInfo**. Em um prompt de comando com privilégios elevados, execute o seguinte:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Verificar a relação de confiança em um EKCert em um computador da autoridade de certificação:** Copie a cadeia de caracteres extraída (o hash SHA-2 do EKPub) para o servidor (por exemplo, via email) e passá-lo para o cmdlet Confirm CAEndorsementKeyInfo. Observe que esse parâmetro deve ser de 64 caracteres.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Para verificar um EKCert de confiança, siga as etapas a seguir:  
  
    1.  **Extraia o EKCert do computador cliente:** O EKCert pode ser extraído de um computador cliente por meio **Get-TpmEndorsementKeyInfo**. Em um prompt de comando com privilégios elevados, execute o seguinte:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **Verificar a relação de confiança em um EKCert em um computador da autoridade de certificação:** Copie o EKCert extraído (EkCert.cer) para a autoridade de certificação (por exemplo, via email ou xcopy). Por exemplo, se você copiar o arquivo de certificado na pasta "c:\diagnose" no servidor de autoridade de certificação, execute o seguinte para concluir a verificação:  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Consulte também  
[Visão geral da tecnologia Trusted Platform Module](https://technet.microsoft.com/library/jj131725.aspx)  
[Recursos externos: Trusted Platform Module](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
