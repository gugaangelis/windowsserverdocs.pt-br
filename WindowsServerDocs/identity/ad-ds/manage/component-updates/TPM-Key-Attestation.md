---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Atestado de chave do TPM
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d08efe825e10d526763a1654c7e090427719c51
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="tpm-key-attestation"></a>Atestado de chave do TPM

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: sênior Justin Turner engenheiro de escalonamento de suporte com o grupo do Windows  
  
> [!NOTE]  
> Este conteúdo foi criado por um engenheiro de suporte de cliente da Microsoft e se destina a arquitetos de sistemas que estão procurando mais profundamente explicações técnicas de recursos e soluções no Windows Server 2012 R2 de tópicos no TechNet geralmente fornecem e administradores experientes. No entanto, ele não passou os mesmos edição passos, para que a linguagem podem parecer que menos refinado que o que geralmente é encontrado no TechNet.  
  
## <a name="overview"></a>Visão geral  
Enquanto o suporte para chaves protegidas pelo TPM existiu desde o Windows 8, não havia nenhuma mecanismos para autoridades de certificação atestar criptograficamente que a chave privada do certificado solicitante está realmente protegida por um Trusted Platform Module (TPM). Esta atualização permite que uma autoridade de certificação para executar esse Atestado e refletir essa Atestado no certificado emitido.  
  
> [!NOTE]  
> Este artigo presume que o leitor esteja familiarizado com o conceito de modelo de certificado (para referência, consulte [modelos de certificado](https://technet.microsoft.com/library/cc730705.aspx)). Ele também presume que o leitor esteja familiarizado com a configuração do enterprise autoridades de certificação para emitir certificados baseados em modelos de certificado (para referência, consulte [lista de verificação: Configurar autoridades de certificação para emitir e gerenciar certificados](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminologia  
  
|Termo|Definição|  
|--------|--------------|  
|EK|Chave de endosso. Isso é uma chave assimétrica contida dentro do TPM (injetado no momento da fabricação). A chave de endosso é exclusivo para cada TPM e possa identificá-lo. A chave de endosso não pode ser alterado ou removido.|  
|EKpub|Faz referência à chave pública da chave de endosso.|  
|EKPriv|Faz referência à chave privada da chave de endosso.|  
|EKCert|Certificado EK. Um TPM fabricante certificado emitido para EKPub. Nem todos os TPMs têm EKCert.|  
|TPM|Trusted Platform Module. Um TPM é projetado para fornecer funções de relacionadas à segurança baseada em hardware. Um chip TPM é um processador de criptografia seguro projetado para realizar operações criptográficas. O chip inclui vários mecanismos de segurança física para torná-lo resistente a adulterações e software mal-intencionado é a adulterações com as funções de segurança do TPM.|  
  
### <a name="background"></a>Em segundo plano  
A partir do Windows 8, um Trusted Platform Module (TPM) pode ser usado para proteger a chave privada do certificado. A Microsoft plataforma criptográficas provedor de chave de armazenamento KSP (provedor) habilita esse recurso. Havia dois preocupações com a implementação:  

-   Não havia nenhuma garantia de que uma chave está realmente protegida por um TPM (alguém facilmente pode simular um software KSP como um KSP do TPM com credenciais de administrador local).

-   Não foi possível limitar a lista dos TPMs têm permissão para proteger enterprise certificados emitido (se o administrador de PKI deseja controlar os tipos de dispositivos que podem ser usados para obter certificados no ambiente).  

### <a name="tpm-key-attestation"></a>Atestado de chave do TPM  
Atestado de chave do TPM é a capacidade da entidade que solicitou um certificado de provar criptograficamente por uma CA que a chave RSA na solicitação de certificado é protegida por TPM "um" ou "a" que a autoridade de certificação confia. O modelo de confiança do TPM é abordado mais o [visão geral da implantação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) seção posteriormente neste tópico.  
  
### <a name="why-is-tpm-key-attestation-important"></a>Por que o Atestado da chave TPM é importante?  
Um certificado de usuário com uma chave atestada TPM fornece uma maior garantia de segurança, backup, sem a capacidade de exportação, anti-anti-hammering e isolamento das chaves fornecidas pelo TPM.  
  
Com atestado de chave do TPM, um novo paradigma de gerenciamento agora é possível: um administrador pode definir o conjunto de dispositivos que os usuários podem usar para acessar recursos corporativos (por exemplo, VPN ou ponto de acesso sem fio) e têm **forte** garante que outros dispositivos não podem ser usados para acessá-los. Este novo paradigma de controle de acesso é **forte** porque ele é vinculado a um *baseados em hardware* identidade do usuário, o que é mais forte que uma credencial baseada em software.
  
### <a name="how-does-tpm-key-attestation-work"></a>Como funciona o atestado de chave do TPM?  
Em geral, Atestado da chave TPM se baseia nos seguintes pilares:  
  
1.  Cada TPM é fornecido com uma chave assimétrica exclusiva, chamado o *chave de endosso* (EK) gravado pelo fabricante. Nos referimos a parte pública dessa chave como *EKPub* e a chave privada associada como *EKPriv*. Alguns chips TPM também têm um certificado de chave de endosso é emitido pelo fabricante para o EKPub. Nós nos referimos a esse certificado como *EKCert*.  
  
2.  Uma autoridade de certificação estabelece confiança no TPM seja via EKPub ou EKCert.  
  
3.  Um usuário prova para a autoridade de certificação que a chave RSA para o qual o certificado está sendo solicitado criptograficamente está relacionada ao EKPub e que o usuário possui o EKpriv.  
  
4.  A CA emite um certificado com uma política de emissão especial OID para indicar que a chave é agora atestada seja protegido por um TPM.  
  
## <a name="BKMK_DeploymentOverview"></a>Visão geral da implantação  
Essa implantação, presume-se uma autoridade de certificação do Windows Server 2012 R2 corporativa é configurada. Além disso, os clientes (Windows 8.1) está configurados para se inscrever em relação essa autoridade de certificação corporativa usando modelos de certificados. 

Há três etapas para implantar o atestado de chave do TPM:  
  
1.  **Planeje o modelo de confiança do TPM:** a primeira etapa é decidir qual modelo de confiança do TPM para usar. Há 3 maneiras com suporte para fazer isso:  
  
    -   **Relação de confiança com base em credenciais de usuário:** a autoridade de certificação corporativa confie o EKPub fornecido pelo usuário como parte da solicitação de certificado e nenhuma validação é realizada que não sejam as credenciais de domínio do usuário.  
  
    -   **Relação de confiança com base em EKCert:** a autoridade de certificação corporativa valida a cadeia de EKCert que é fornecida como parte da solicitação de certificado em relação a uma lista gerenciadas pelo administrador do *aceitáveis cadeias de certificado EK*. As cadeias de aceitáveis são definidos por fabricante e expresso por meio de dois repositórios de certificados personalizado na CA de emissão (uma loja para o intermediário) e outro para certificados de CA raiz. Esse modo de confiança significa que **todos os** TPMs de um determinado fabricante são confiáveis. Observe que, nesse modo, os TPMs em uso no ambiente devem conter EKCerts.
  
    -   **Relação de confiança com base em EKPub:** a autoridade de certificação corporativa valida se o EKPub fornecido como parte da solicitação de certificado é exibido em uma lista gerenciadas pelo administrador de permissão EKPubs. Essa lista é expresso como um diretório de arquivos onde o nome de cada arquivo nesse diretório é o hash SHA-2 do EKPub permitido. Essa opção oferece o mais alto nível de garantia, mas não requer mais esforço administrativo, porque cada dispositivo é identificado individualmente. Nesse modelo de confiança, somente os dispositivos que tiveram seus do TPM EKPub adicionado à lista de permissões do EKPubs têm permissão para se registrar para obter um certificado de atestado de TPM.  
  
    Dependendo de qual método é usado, a autoridade de certificação serão aplicadas a uma política de emissão diferentes OID para o certificado emitido. Para obter mais detalhes sobre a política de emissão OIDs, consulte a tabela OIDs de política de emissão a [configurar um modelo de certificado](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) seção deste tópico.  
  
    Observe que é possível escolher uma combinação de modelos de confiança do TPM. Nesse caso, a autoridade de certificação aceitará qualquer um dos métodos de Atestado e a política de emissão que OIDs refletirá todos os métodos de Atestado sucesso.  
  
2.  **Configurar o modelo de certificado:** configurar o modelo de certificado é descrito no [detalhes de implantação](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) seção deste tópico. Este artigo não abrange como esse modelo de certificado é atribuído à autoridade de certificação corporativa ou como registrar acesso é concedido a um grupo de usuários. Para obter mais informações, consulte [lista de verificação: Configurar autoridades de certificação para emitir e gerenciar certificados](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurar a CA para o modelo de confiança do TPM**  
  
    1.  **Relação de confiança com base em credenciais de usuário:** nenhuma configuração específica é necessária.  
  
    2.  **Relação de confiança com base em EKCert:** o administrador deve obter a cadeia de certificados EKCert por fabricantes de TPM e importá-los para dois repositórios de certificados novo, criados pelo administrador, na autoridade de certificação que executam o atestado de chave do TPM. Para obter mais informações, consulte o [configuração da CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) seção deste tópico.  
  
    3.  **Relação de confiança com base em EKPub:** o administrador deve obter o EKPub para cada dispositivo que será necessário atestados TPM certificados e adicioná-los à lista de permissões EKPubs. Para obter mais informações, consulte o [configuração da CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) seção deste tópico.  
  
    > [!NOTE]  
    > -   Esse recurso requer o Windows 8.1 / Windows Server 2012 R2.  
    > -   Atestado de chave do TPM para o cartão inteligente de terceiros KSPs não é permitido. Microsoft plataforma criptográficas provedor KSP deve ser usado.  
    > -   Atestado de chave do TPM só funciona para chaves RSA.  
    > -   Atestado de chave do TPM não é compatível com uma autoridade de certificação autônoma.  
    > -   Atestado de chave do TPM não suporta [processamento do certificado não persistentes](https://technet.microsoft.com/library/ff934598).  
  
## <a name="BKMK_DeploymentDetails"></a>Detalhes de implantação  
  
### <a name="BKMK_ConfigCertTemplate"></a>Configurar um modelo de certificado  
Para configurar o modelo de certificado de atestado de chave do TPM, execute as seguintes etapas de configuração:  
  
1.  **Compatibilidade** guia  
  
    No **configurações de compatibilidade** seção:  
  
    -   Certifique-se de **Windows Server 2012 R2** será selecionado para o **autoridade de certificação**.  
  
    -   Certifique-se de **Windows 8.1 / Windows Server 2012 R2** será selecionado para o **destinatário do certificado**.  
  
    ![Atestado de chave do TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **Criptografia** guia  
  
    Certifique-se de **Key Storage Provider** será selecionado para o **provedor categoria** e **RSA** será selecionado para o **nome do algoritmo**. Certifique-se de **solicitações devem usar um dos seguintes provedores** é selecionado e o **provedor de criptografia da plataforma Microsoft** opção é selecionada em **provedores**.  
  
    ![Atestado de chave do TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **Atestado de chave** guia  
  
    Esta é uma nova aba para Windows Server 2012 R2:  
  
    ![Atestado de chave do TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Escolha um modo de Atestado entre as três opções possíveis.  
  
    ![Atestado de chave do TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **NONE:** implica que atestado de chave não deve ser usado  
  
    -   **Necessária, se o cliente é compatível com:** permite que os usuários em um dispositivo que não oferece suporte a TPM tecla Atestado para continuar registrando-se para esse certificado. Os usuários que podem executar Atestado vai ser diferenciados com uma política de emissão especial OID. Alguns dispositivos não poderá executar Atestado por causa de um TPM antigo que não dá suporte atestado de chave ou o dispositivo sem um TPM em todos.
  
    -   **Obrigatório:** cliente *deve* executar atestado de chave do TPM, caso contrário, a solicitação de certificado falhará.  
  
    Em seguida, escolha o modelo de confiança do TPM. Novamente, há três opções:  
  
    ![Atestado de chave do TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **As credenciais do usuário:** permitir que um usuário autenticação atestar um TPM válido especificando suas credenciais de domínio.  
  
    -   **Certificado de endosso:** deve validar o EKCert do dispositivo por meio de gerenciadas pelo administrador TPM certificados de CA intermediários para um certificado de autoridade de certificação raiz gerenciadas pelo administrador. Se você escolher essa opção, você deve configurar EKCA e EKRoot repositórios na CA de emissão de certificados, conforme descrito no [configuração da CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) seção deste tópico.  
  
    -   **Chave de endosso:** EKPub do dispositivo deve aparecer na lista de PKI gerenciadas pelo administrador. Essa opção oferece o mais alto nível de garantia, mas não requer mais esforço administrativo. Se você escolher essa opção, você deve configurar uma lista de EKPub na CA de emissão conforme descrito no [configuração da CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) seção deste tópico.  
  
    Por fim, decida qual política de emissão para mostrar no certificado emitido. Por padrão, cada tipo de imposição tem um identificador de objeto associado (OIDs) que será inserido no certificado se ele for aprovado esse tipo de imposição, conforme descrito na tabela a seguir. Observe que é possível escolher uma combinação de métodos de imposição. Nesse caso, a autoridade de certificação aceitará qualquer um dos métodos Atestado e a política de emissão OID refletirá todos os métodos de Atestado foi bem-sucedida.  
  
    **OIDs de política de emissão**  
  
    |OID|Tipo de atestado de chave|Descrição|Nível de garantia|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|"EK Verified": para lista gerenciadas pelo administrador de EK|Alto|  
    |1.3.6.1.4.1.311.21.31|Certificado de endosso|"EK certificado Verified": quando a cadeia de certificados EK é validada|Médio|  
    |1.3.6.1.4.1.311.21.32|Credenciais do usuário|"EK confiável em uso": para EK atestados por usuário|Baixo|  
  
    Os OIDs serão inseridos no certificado emitido se **incluir políticas de emissão** é selecionada (a configuração padrão).  
  
    ![Atestado de chave do TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > Um possível uso de ter o OID presente no certificado é limitar o acesso a VPN ou rede para certos dispositivos sem fio. Por exemplo, sua política de acesso pode permitir conexão (ou acesso obtenham uma diferentes) se OID 1.3.6.1.4.1.311.21.30 estiver presente no certificado. Isso permite que você limite o acesso aos dispositivos cujos TPM EK está presente na lista EKPUB.  
  
### <a name="BKMK_CAConfig"></a>Configuração da CA  
  
1.  **Instalação EKCA e EKROOT repositórios de certificados em uma CA de emissão**  
  
    Se você optar por **certificado de endosso** para as configurações de modelo, execute as seguintes etapas de configuração:  
  
    1.  Use o Windows PowerShell para criar dois novos repositórios de certificados no servidor (autoridade de certificação) certificação que executarão o atestado de chave do TPM.  
  
    2.  Obtenha o intermediários e raiz certificados de CA de manufacturer(s) que você deseja permitir em seu ambiente empresarial. Esses certificados devem ser importados para as lojas de certificado criado anteriormente (EKCA e EKROOT) conforme apropriado.  
  
    O Windows PowerShell script a seguir realiza essas duas etapas. No exemplo a seguir, o fabricante do TPM Fabrikam forneceu um certificado raiz *FabrikamRoot.cer* e um certificado da CA emissora *Fabrikamca.cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Configurar a lista de EKPUB se usando o tipo de atestado de chave de endosso**  
  
    Se você optar por **chave de endosso** nas configurações do modelo, as próximas etapas de configuração são criar e configurar uma pasta na CA de emissão, que contém arquivos de 0 byte, cada chamada para o hash SHA-2 de um EK permitido. Esta pasta serve como uma "lista de permissões" de dispositivos que têm permissão para obter certificados de atestado de chave do TPM. Porque você deve adicionar manualmente o EKPUB para cada dispositivo que requer um certificado Atestado, ele fornece a empresa com a garantia de que os dispositivos que estão autorizados a obter certificados atestados chaves do TPM. Configurando uma autoridade de certificação para esse modo exige duas etapas:  
  
    1.  **Criar a entrada do registro EndorsementKeyListDirectories:** usar a ferramenta de linha de comando Certutil para configurar os locais da pasta onde EKpubs confiáveis são definidas conforme descrito na tabela a seguir.  
  
        |Operação|Sintaxe de comando|  
        |-------------|------------------|  
        |Adicionar locais de pasta|Certutil.exe - setreg CA\EndorsementKeyListDirectories + "<folder>"|  
        |Remover locais de pastas|Certutil.exe - setreg CA\EndorsementKeyListDirectories-"<folder>"|  
  
        EndorsementKeyListDirectories no comando certutil é uma configuração de registro conforme descrito na tabela a seguir.  
  
        |Nome do valor|Tipo|Dados|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< caminho UNC ou LOCAL EKPUB permitir listas ><br /><br />Exemplo:<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* conterá uma lista de UNC ou caminhos do sistema de arquivos local, cada apontando para uma pasta que a autoridade de certificação tem acesso de leitura. Cada pasta pode conter zero ou mais permitem entradas da lista, onde cada entrada é um arquivo com um nome que é o SHA-2 hash de um EKpub confiável, sem extensão de arquivo. 
        Criar ou editar essa configuração de chave do registro requer uma reinicialização da autoridade de certificação, assim como definições de configuração de registro de CA existentes. No entanto, edições para a configuração entrará em vigor imediatamente e não exige a autoridade de certificação seja reiniciado.  
  
        > [!IMPORTANT]  
        > Proteja as pastas na lista contra adulterações e acesso não autorizado ao configurar permissões para que somente administradores autorizados leu e acesso de gravação. A conta de computador da CA requer acesso somente leitura.  
  
    2.  **Preenche a lista EKPUB:** usar o seguinte cmdlet do Windows PowerShell para obter o hash da chave público do TPM EK usando o Windows PowerShell em cada dispositivo e, em seguida, enviar esse público tecla hash à autoridade de certificação e armazená-los na pasta EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublickKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Solução de problemas  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Campos de atestado de chave não estão disponíveis em um modelo de certificado  
Os campos de atestado de chave não estão disponíveis se as configurações de modelo não atender aos requisitos de Atestado. Motivos comuns são:  
  
1.  As configurações de compatibilidade não estão configuradas corretamente. Certifique-se de que eles são configurados da seguinte forma:  
  
    1.  **Autoridade de certificação**: **Windows Server 2012 R2**  
  
    2.  **Destinatário do certificado**: **Windows 8.1 / Windows Server 2012 R2**  
  
2.  As configurações de criptografia não estão configuradas corretamente. Certifique-se de que eles são configurados da seguinte forma:  
  
    1.  **Categoria de provedor**: **provedor de armazenamento de chave**  
  
    2.  **Nome do algoritmo**: **RSA**  
  
    3.  **Provedores**: **provedor de criptografia da plataforma Microsoft**  
  
3.  As configurações de tratamento de solicitação não estão configuradas corretamente. Certifique-se de que eles são configurados da seguinte forma:  
  
    1.  O **permitir que a chave privada seja exportada** opção não deve ser selecionada.  
  
    2.  O **arquivar chave privada de criptografia da entidade** opção não deve ser selecionada.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Verificação de dispositivo TPM Atestado  
Use o cmdlet do Windows PowerShell, **confirmar CAEndorsementKeyInfo**, para verificar se um dispositivo TPM específico é confiável para Atestado pelas autoridades de certificação. Há duas opções: um para verificar a EKCert e outro para verificar um EKPub. O cmdlet é executar localmente em uma CA ou em CAs remoto usando o Windows PowerShell remoto.  
  
1.  Para verificar a confiança em um EKPub, siga as etapas a seguir:  
  
    1.  **Extraia o EKPub do computador cliente:** EKPub o pode ser extraído de um computador cliente por meio de **Get-TpmEndorsementKeyInfo**. Em um prompt de comando com privilégios elevados, execute o seguinte:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Verifique se é confiável em um EKCert em um computador da CA:** copiar a cadeia de caracteres extraída (o hash SHA-2 do EKPub) para o servidor (por exemplo, via email) e passá-lo para o cmdlet CAEndorsementKeyInfo confirmar. Observe que este parâmetro deve ser 64 caracteres.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Para verificar a confiança em um EKCert, siga as etapas a seguir:  
  
    1.  **Extraia o EKCert do computador cliente:** EKCert o pode ser extraído de um computador cliente por meio de **Get-TpmEndorsementKeyInfo**. Em um prompt de comando com privilégios elevados, execute o seguinte:  
  
        ```  
        PS C:>\$a= Get- TpmEndorsementKeyInfo  
        PS C:>\$a.manufacturerCertificates|Export-Certificate c:\myEkcert.cer  
        ```  
  
    2.  **Verifique se é confiável em um KCert em um computador da CA:** copiar o EKCert extraído (EkCert.cer) para a autoridade de certificação (por exemplo, via email ou xcopy). Por exemplo, se você copiar o arquivo de certificado a pasta "c:\diagnose" no servidor CA, execute o seguinte para concluir a verificação:  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Consulte também  
[Visão geral da tecnologia Trusted Platform Module](https://technet.microsoft.com/library/jj131725.aspx)  
[Recurso externo: Módulo de plataforma confiável](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
  


