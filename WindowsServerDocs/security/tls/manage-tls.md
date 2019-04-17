---
title: Gerenciar Transport Layer Security (TLS)
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 06/05/2017
ms.openlocfilehash: e26d1f833dbb7219251947f1cb3cb09def958aea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-transport-layer-security-tls"></a>Gerenciar Transport Layer Security (TLS)

## <a name="configuring-tls-cipher-suite-order"></a>Configurando ordem do conjunto de codificação TLS

Versões diferentes do Windows dão suporte a diferentes conjuntos de codificação TLS e a ordem de prioridade. Consulte [pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) para a ordem padrão compatível com o Microsoft Schannel Provider em diferentes versões do Windows.

> [!NOTE] 
> Você também pode modificar a lista de pacotes de codificação usando funções CNG, consulte [priorizando pacotes de codificação Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) para obter detalhes.

Alterações à ordem de pacote de codificação TLS entrará em vigor na próxima inicialização. Até reiniciar ou desligar, a ordem existente entrará em vigor.

> [!WARNING] 
> Atualizar as configurações do registro para a ordem de prioridade padrão não tem suporte e pode ser redefinida com atualizações de manutenção. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configurando a ordem do conjunto de codificação TLS usando a política de grupo

Você pode usar as configurações de política de grupo ordem do pacote de codificação SSL para definir a ordem padrão TLS codificação suite.

1.  Usando o Console de gerenciamento de política de grupo, acesse **configuração do computador** > **modelos administrativos** > **redes** > **definições de configuração do SSL**.
2.  Clique duas vezes em **ordem do conjunto de codificação SSL**e, em seguida, clique no **Enabled** opção.
3.  Clique com botão direito **conjuntos de codificação SSL** caixa e selecione **Selecionar tudo** menu pop-up.

    ![Configuração de política de grupo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  Clique com botão direito o texto selecionado e selecione **cópia** menu pop-up.
5.  Cole o texto em um editor de texto, como notepad.exe e atualização com a nova lista de ordem de pacote de codificação.

    > [!NOTE]
    > A lista de ordem de pacote de codificação TLS deve estar no formato estrito delimitado por vírgulas. Cada cadeia de caracteres de pacote de codificação terminará com uma vírgula (,) para o lado direito dele. 

    > Além disso, a lista de pacotes de codificação é limitada a 1.023 caracteres.

6.  Substituir a lista no **conjuntos de codificação SSL** com a lista ordenada atualizada.
7.  Clique em **Okey** ou **aplicar**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configurando a ordem do conjunto de codificação TLS usando o MDM

CSP da política do Windows 10 dá suporte a configuração dos pacotes de codificação TLS. Consulte [criptografia/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) para obter mais informações.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurando ordem do conjunto de codificação TLS usando os Cmdlets do PowerShell TLS

O módulo PowerShell TLS dá suporte a obter a lista ordenada de pacotes de codificação TLS, desabilitando um conjunto de codificação e permitindo que um conjunto de codificação. Consulte [TLS módulo](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) para obter mais informações.

## <a name="configuring-tls-ecc-curve-order"></a>Configurando a ordem de curva ECC TLS 

A partir do Windows 10 e Windows Server 2016, ECC curva ordem pode ser configurada independente da ordem de pacote de codificação. Se a ordem do conjunto de lista tem sufixos de curva elíptica de codificação TLS, eles serão substituídos pela nova ordem de prioridade de curva elíptica, quando habilitado. Isso permitem que as organizações usar um objeto de política de grupo para configurar diferentes versões do Windows com a mesma ordem de pacotes de codificação.

> [!NOTE]
> Antes do Windows 10, as cadeias de caracteres de pacote de codificação foram junto com a curva elíptica para determinar a prioridade de curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Gerenciando curvas Windows ECC usando CertUtil

A partir do Windows 10 e Windows Server 2016, o Windows fornece gerenciamento de parâmetro de curva elíptica porém a linha de comando utilitário certuil.exe. Parâmetros de curva elíptica são armazenados na bcryptprimitives.dll. Usando certutil.exe, os administradores podem adicionar e remover os parâmetros de curva para e do Windows, respectivamente. Certutil.exe armazena os parâmetros de curva com segurança no registro. Windows pode começar a usar os parâmetros de curva pelo nome associado a curva.    

#### <a name="displaying-registered-curves"></a>Exibindo curvas registradas

Use o seguinte comando certutil.exe para exibir uma lista das curvas registrado para o computador atual.

```powershell
certutil.exe –displayEccCurve
```

![Curvas de exibição certutil](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1 Certutil.exe saída para exibir a lista das curvas registradas.*

#### <a name="adding-a-new-curve"></a>Adicionando uma nova curva

As organizações podem criar e usar os parâmetros de curva pesquisados por outras entidades confiáveis.  
Os administradores que desejam usar esses curvas de novo no Windows devem adicionar a curva.  
Use o seguinte comando certutil.exe para adicionar uma curva para o computador atual:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- O **curveName** argumento representa o nome da curva em que os parâmetros de curva foram adicionados.
- O **curveParameters** argumento representa o nome do arquivo de um certificado que contém os parâmetros das curvas você deseja adicionar.
- O **curveOid** argumento representa um nome de arquivo de um certificado que contém o OID dos parâmetros de curva que você deseja adicionar (opcional).
- O **curveType** argumento representa um valor decimal da curva nomeado do [EC denominado curva registro](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (opcional).

![Certutil adicionar curvas](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 adicionando uma curva usando certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Remover uma curva adicionada anteriormente

Os administradores podem remover uma curva adicionada anteriormente usando o seguinte comando certutil.exe:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows não pode usar uma curva nomeada depois que um administrador remove a curva do computador.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Gerenciando curvas ECC do Windows usando a política de grupo

As organizações podem distribuir parâmetros de curva para o enterprise, domínio, computador usando a política de grupo e a extensão do registro de preferências de política de grupo.  
O processo para distribuir uma curva é:

1.  No Windows 10 e Windows Server 2016, use **certutil.exe** para adicionar uma nova curva nomeada registrada para o Windows.
2.  Do mesmo computador, abra o Console de gerenciamento de política de grupo (GPMC), crie um novo objeto de política de grupo e editá-lo.
3.  Navegue até **configuração do computador | Preferências | Configurações do Windows | Registro**.  Clique com botão direito **registro**. Focalize **nova** e selecione **Item coleção**. Renomeie o item de coleção para corresponder ao nome da curva. Você criará um item de coleção de registro para cada chave do registro em *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurar a coleção de registro preferência de política recém-criada grupo, adicionando um novo **Item do registro** para cada valor do Registro listadas em *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\ [curveName]*.
5.  Implante o objeto de política de grupo que contém o item de coleção de registro de política de grupo para computadores Windows 10 e Windows Server 2016 que devem receber as curvas nomeadas novo.

    ![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 usando o Group Policy Preferences para distribuir curvas*

## <a name="managing-tls-ecc-order"></a>Gerenciando ordem TLS ECC

Política de grupo de ordem de curva ECC configurações podem ser usadas a partir do Windows 10 e Windows Server 2016, configure o TLS ECC curva ordem padrão. Usando ECC genérico e essa configuração, as organizações pode adicionar seus próprios denominado curvas (que são aprovadas para uso com TLS) para o sistema operacional confiável e, em seguida, adicioná-las denominado curvas para a configuração de política de grupo de prioridade de curva para garantir que eles sejam usados em futuras handshakes TLS. Novas listas de prioridade de curva se tornarão ativas na próxima reinicialização após receber as configurações de política.     

![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 Gerenciando TLS curva prioridade usando política de grupo*


