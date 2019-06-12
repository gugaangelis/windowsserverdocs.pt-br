---
title: Gerenciar Transport Layer Security (TLS)
description: Segurança do Windows Server
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
ms.date: 05/16/2018
ms.openlocfilehash: 872647f09898bf8ae08ee69f28b717d28abf7c78
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447299"
---
# <a name="manage-transport-layer-security-tls"></a>Gerenciar Transport Layer Security (TLS)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configurar TLS Cipher Suite Order

Versões diferentes do Windows oferecem suporte a diferentes conjuntos de codificação TLS e ordem de prioridade. Ver [conjuntos de codificação no TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) para a ordem padrão com suporte pelo Microsoft Schannel Provider em diferentes versões do Windows.

> [!NOTE] 
> Você também pode modificar a lista de conjuntos de codificação por meio de funções CNG, consulte [priorizando pacotes de criptografia do Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) para obter detalhes.

As alterações a ordem do conjunto de codificação TLS entrarão em vigor na próxima inicialização. Até que a reinicialização ou desligamento, a ordem existente estará em vigor.

> [!WARNING] 
> Atualizando as configurações de registro para a ordem de prioridade padrão não tem suporte e pode ser redefinida com as atualizações de manutenção. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configurar TLS Cipher Suite Order usando a diretiva de grupo

Você pode usar as configurações de diretiva de grupo do SSL Cipher Suite Order para configurar a ordem de conjunto de codificação TLS padrão.

1. No Console de gerenciamento de diretiva de grupo, acesse **configuração do computador** > **modelos administrativos** > **redes**  >  **Definições de configuração de SSL**.
2. Clique duas vezes em **SSL Cipher Suite Order**e, em seguida, clique no **habilitado** opção.
3. Clique com botão direito **conjuntos de codificação SSL** caixa e selecione **Selecionar tudo** no menu pop-up.

   ![Configuração da Política de Grupo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. O texto selecionado com o botão direito e selecione **cópia** no menu pop-up.
5. Cole o texto em um editor de texto como notepad.exe e atualização com a nova lista de ordem do cipher suite.

   > [!NOTE]
   > Lista de ordem de TLS cipher suite deve ser no formato delimitado por vírgula de estrita. Cada cadeia de caracteres do cipher suite terminará com uma vírgula (,) à direita dele. 
   > 
   > Além disso, a lista de conjuntos de codificação é limitada a 1.023 caracteres.

6. Substituir a lista na **conjuntos de codificação SSL** com a lista ordenada atualizada.
7. Clique em **OK** ou **Aplicar**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configurar TLS Cipher Suite Order por meio do MDM

O CSP de política do Windows 10 dá suporte à configuração dos pacotes de codificação TLS. Ver [criptografia/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) para obter mais informações.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurar TLS Cipher Suite Order usando Cmdlets do PowerShell de TLS

O módulo do PowerShell de TLS dá suporte à obtenção da lista ordenada de conjuntos de codificação TLS, desabilitar um conjunto de codificação e habilitar um conjunto de codificação. Ver [TLS módulo](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) para obter mais informações.

## <a name="configuring-tls-ecc-curve-order"></a>Configurando a ordem de curva ECC TLS 

Começando com o Windows 10 e Windows Server 2016, ordem de curva ECC pode ser configurado independentemente da ordem do conjunto de codificação. Se a lista tiver sufixos de curva elíptica de ordem do conjunto de codificação de TLS, elas serão substituídas pela nova ordem de prioridade de curva elíptica, quando habilitado. Isso que as organizações a usar um objeto de diretiva de grupo para configurar diferentes versões do Windows com a mesma ordem de conjuntos de codificação.

> [!NOTE]
> Antes do Windows 10, cadeias de caracteres do cipher suite foram acrescentadas com a curva elíptica para determinar a prioridade da curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Gerenciando as curvas de ECC do Windows usando o CertUtil

Começando com o Windows 10 e Windows Server 2016, o Windows fornece gerenciamento de parâmetros de curva elíptica por meio de linha de comando utilitário certutil.exe. Parâmetros de curva elíptica são armazenados do bcryptprimitives.dll. Usando certutil.exe, os administradores podem adicionar e remover parâmetros de curva para e do Windows, respectivamente. Certutil.exe armazena os parâmetros de curva com segurança no registro. Windows podem começar a usar os parâmetros de curva, o nome associado a curva.    

#### <a name="displaying-registered-curves"></a>Exibindo curvas registradas

Use o seguinte comando para exibir uma lista das curvas certutil.exe registrado para o computador atual.

```powershell
certutil.exe –displayEccCurve
```

![Curvas de exibição de certutil](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Certutil.exe Figura 1 de saída para exibir a lista de curvas registradas.*

#### <a name="adding-a-new-curve"></a>Adicionando uma nova curva

As organizações podem criar e usar parâmetros de curva pesquisados por outras entidades confiáveis.  
Os administradores que desejam usar esses curvas de novo no Windows devem adicionar a curva.  
Use o seguinte comando certutil.exe para adicionar uma curva para o computador atual:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- O **curveName** argumento representa o nome da curva sob a qual os parâmetros de curva foram adicionados.
- O **curveParameters** argumento representa o nome do arquivo de um certificado que contém os parâmetros das curvas que você deseja adicionar.
- O **curveOid** argumento representa um nome de arquivo de um certificado que contém o OID dos parâmetros de curva que você deseja adicionar (opcional).
- O **curveType** argumento representa um valor decimal da curva nomeada da [registro de curva nomeada EC](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (opcional).

![Certutil adicione curvas](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 adicionando uma curva usando certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Removendo uma curva adicionada anteriormente

Os administradores podem remover uma curva adicionada anteriormente usando o seguinte comando certutil.exe:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows não é possível usar uma curva nomeada depois que um administrador remove a curva do computador.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Gerenciando as curvas de ECC do Windows usando a diretiva de grupo

As organizações podem distribuir os parâmetros de curva para enterprise, ingressado no domínio usando diretiva de grupo e a extensão do registro de preferências de diretiva de grupo do computador.  
O processo para a distribuição de uma curva é:

1.  No Windows 10 e Windows Server 2016, use **certutil.exe** para adicionar uma curva nomeada registrada de novo para o Windows.
2.  Do mesmo computador, abra o grupo de política de gerenciamento GPMC (Console), crie um novo objeto de diretiva de grupo e editá-lo.
3.  Navegue até **configuração do computador | Preferências | Configurações do Windows | Registro**.  Clique com botão direito **registro**. Passe o mouse sobre **New** e selecione **Item da coleção**. Renomeie o item de coleta para corresponder ao nome da curva. Você criará um item de coleta de registro para cada chave do registro sob *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurar a coleção de registro de preferência de diretiva recém-criado grupo adicionando um novo **Item do registro** para cada valor do registro listados sob *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ ECCParameters\[curveName]* .
5.  Implante o objeto de diretiva de grupo que contém o item de coleta de registro de diretiva de grupo para computadores Windows 10 e Windows Server 2016 que devem receber o novo curvas nomeadas.

    ![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 Using Group Policy Preferences para distribuir as curvas*

## <a name="managing-tls-ecc-order"></a>Gerenciando a ordem de ECC TLS

Diretiva de grupo de ordem de curva ECC configurações podem ser usadas a partir do Windows 10 e Windows Server 2016, configurar a ordem de curva ECC TLS padrão. Usando ECC genérico e essa configuração, as organizações pode adicionar seus próprios confiável chamado curvas (que são aprovadas para uso com TLS) para o sistema operacional e, em seguida, adicione esses curvas nomeadas para a configuração de diretiva de grupo de prioridade de curva para garantir que elas são usadas em futuras TLS Handshakes. Novas listas de prioridade de curva ficam ativas na próxima reinicialização depois de receber as configurações de política.     

![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 Gerenciando TLS curva prioridade usando a diretiva de grupo*


