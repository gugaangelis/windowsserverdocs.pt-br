---
title: Gerenciar segurança de camada de transporte (TLS)
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: a4ac1ea5b0648dbb80f103c146ad3df23fc04ab7
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322678"
---
# <a name="manage-transport-layer-security-tls"></a>Gerenciar segurança de camada de transporte (TLS)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configurando a ordem do conjunto de criptografia TLS

Versões diferentes do Windows dão suporte a diferentes conjuntos de criptografia TLS e ordem de prioridade. Consulte [conjuntos de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) para a ordem padrão com suporte pelo provedor do Microsoft Schannel em diferentes versões do Windows.

> [!NOTE] 
> Você também pode modificar a lista de conjuntos de codificação usando funções CNG, confira [priorizando pacotes de codificação Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) para obter detalhes.

As alterações na ordem do conjunto de criptografia TLS entrarão em vigor na próxima inicialização. Até a reinicialização ou desligamento, a ordem existente estará em vigor.

> [!WARNING] 
> Não há suporte para a atualização das configurações do registro para a ordem de prioridade padrão e elas podem ser redefinidas com atualizações de serviço. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configurando a ordem do conjunto de codificação TLS usando Política de Grupo

Você pode usar a ordem do conjunto de codificação SSL Política de Grupo configurações para configurar a ordem padrão do conjunto de codificação TLS.

1. No Console de Gerenciamento de Política de Grupo, vá para **configuração do computador** > **Modelos Administrativos** > **redes** > **definições de configuração de SSL**.
2. Clique duas vezes em **ordem do pacote de criptografia SSL**e, em seguida, clique na opção **habilitado** .
3. Clique com o botão direito do mouse na caixa **conjuntos de criptografia SSL** e selecione **selecionar tudo** no menu pop-up.

   ![Definição de Política de Grupo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. Clique com o botão direito do mouse no texto selecionado e selecione **copiar** no menu pop-up.
5. Cole o texto em um editor de texto como Notepad. exe e Update com a nova lista de pedidos do pacote de codificação.

   > [!NOTE]
   > A lista de pedidos do conjunto de criptografia TLS deve estar no formato delimitado por vírgula estrito. Cada cadeia de caracteres do pacote de codificação terminará com uma vírgula (,) no lado direito dela. 
   > 
   > Além disso, a lista de conjuntos de codificação é limitada a 1.023 caracteres.

6. Substitua a lista nos **conjuntos de codificação SSL** pela lista ordenada atualizada.
7. Clique em **OK** ou **Aplicar**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configurando a ordem do conjunto de codificação TLS usando MDM

O CSP de política do Windows 10 dá suporte à configuração dos conjuntos de criptografia TLS. Consulte [Cryptography/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) para obter mais informações.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurando a ordem do conjunto de codificação TLS usando cmdlets do PowerShell do TLS

O módulo do PowerShell do TLS dá suporte à obtenção da lista ordenada de conjuntos de criptografia TLS, à desabilitação de um pacote de codificação e à habilitação de um pacote de codificação. Consulte o [módulo TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) para obter mais informações.

## <a name="configuring-tls-ecc-curve-order"></a>Configurando a ordem de curva do TLS ECC 

A partir do Windows 10 & Windows Server 2016, a ordem de curva ECC pode ser configurada independentemente da ordem do conjunto de codificação. Se a lista de pedidos do conjunto de criptografia TLS tiver sufixos de curva elíptica, elas serão substituídas pela nova ordem de prioridade de curva elíptica quando habilitada. Isso permite que as organizações usem um objeto Política de Grupo para configurar versões diferentes do Windows com a mesma ordem de pacotes de codificação.

> [!NOTE]
> Antes do Windows 10, as cadeias de caracteres do pacote de codificação foram anexadas com a curva elíptica para determinar a prioridade da curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Gerenciando as curvas do ECC do Windows usando o CertUtil

A partir do Windows 10 e do Windows Server 2016, o Windows fornece o gerenciamento de parâmetros de curva elíptica por meio do utilitário de linha de comando certutil. exe. Os parâmetros de curva elíptica são armazenados no bcryptprimitives. dll. Usando o Certutil. exe, os administradores podem adicionar e remover parâmetros de curva de e para o Windows, respectivamente. O Certutil. exe armazena os parâmetros de curva com segurança no registro. O Windows pode começar a usar os parâmetros de curva pelo nome associado à curva.    

#### <a name="displaying-registered-curves"></a>Exibindo curvas registradas

Use o comando certutil. exe a seguir para exibir uma lista de curvas registradas para o computador atual.

```powershell
certutil.exe –displayEccCurve
```

![Curvas de exibição de certutil](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1 saída de certutil. exe para exibir a lista de curvas registradas.*

#### <a name="adding-a-new-curve"></a>Adicionando uma nova curva

As organizações podem criar e usar parâmetros de curva pesquisados por outras entidades confiáveis.  
Os administradores que desejam usar essas novas curvas no Windows devem adicionar a curva.  
Use o comando certutil. exe a seguir para adicionar uma curva ao computador atual:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- O argumento **curvename** representa o nome da curva sob a qual os parâmetros de curva foram adicionados.
- O argumento **curveparameters** representa o nome de arquivo de um certificado que contém os parâmetros das curvas que você deseja adicionar.
- O argumento **curveOid** representa um nome de arquivo de um certificado que contém o OID dos parâmetros de curva que você deseja adicionar (opcional).
- O argumento **curvetype** representa um valor decimal da curva nomeada do registro de [curva nomeado do EC](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (opcional).

![Certutil adicionar curvas](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 adicionando uma curva usando o Certutil. exe.*

#### <a name="removing-a-previously-added-curve"></a>Removendo uma curva adicionada anteriormente

Os administradores podem remover uma curva adicionada anteriormente usando o seguinte comando certutil. exe:

```powershell
Certutil.exe –deleteEccCurve curveName
```

O Windows não pode usar uma curva nomeada depois que um administrador remove a curva do computador.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Gerenciando as curvas do ECC do Windows usando Política de Grupo

As organizações podem distribuir parâmetros de curva para o computador corporativo, ingressado no domínio usando Política de Grupo e a extensão de registro de preferências Política de Grupo.  
O processo de distribuição de uma curva é:

1.  No Windows 10 e no Windows Server 2016, use o **certutil. exe** para adicionar uma nova curva nomeada registrada ao Windows.
2.  No mesmo computador, abra o Console de Gerenciamento de Política de Grupo (GPMC), crie um novo objeto de Política de Grupo e edite-o.
3.  Navegar para **configuração do computador | Preferências | Configurações do Windows | Registro**.  Clique com o botão direito do mouse em **registro**. Passe o mouse sobre **novo** e selecione **item de coleta**. Renomeie o item de coleta para corresponder ao nome da curva. Você criará um item de coleção de registro para cada chave do registro em *HKEY_LOCAL_MACHINE \currentcontrolset\control\cryptography\eccparameters*.
4.  Configure a coleção de registro de preferência de Política de Grupo recém-criada adicionando um novo **item de registro** para cada valor de registro listado em *HKEY_LOCAL_MACHINE \currentcontrolset\control\cryptography\eccparameters\[curvename]* .
5.  Implante o objeto de Política de Grupo que contém Política de Grupo item de coleção do registro em computadores com Windows 10 e Windows Server 2016 que devem receber as novas curvas nomeadas.

    ![Distribuir curvas de GPP](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 usando preferências de Política de Grupo para distribuir curvas*

## <a name="managing-tls-ecc-order"></a>Gerenciando a ordem de ECC TLS

A partir do Windows 10 e do Windows Server 2016, as configurações da política de grupo da ordem de curva do ECC podem ser usadas para configurar a ordem de curva de ECC TLS padrão. Usando o ECC genérico e essa configuração, as organizações podem adicionar suas próprias curvas nomeadas confiáveis (que são aprovadas para uso com TLS) para o sistema operacional e, em seguida, adicionar essas curvas nomeadas à prioridade de curva Política de Grupo configuração para garantir que elas sejam usadas no futuro TLS Handshakes. Novas listas de prioridades de curva se tornam ativas na próxima reinicialização depois de receber as configurações de política.     

![Distribuir curvas de GPP](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 Gerenciando a prioridade da curva TLS usando Política de Grupo*


