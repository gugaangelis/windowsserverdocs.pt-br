---
title: Configurar a infraestrutura de servidor
description: Nesta etapa, você pode instala e configurar os componentes do lado do servidor necessários para oferecer suporte a VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 8a07d84427b770da465b8712d71eb846a7c9cf8d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749612"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Etapa 2. Configurar a infraestrutura de servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 1. Planejar a implantação da VPN Always On](always-on-vpn-deploy-planning.md)
- [**Avançar:** Etapa 3. Configurar o servidor de acesso remoto da VPN Always On](vpn-deploy-ras.md)

Nesta etapa, você vai instalar e configurar os componentes do lado do servidor necessários para oferecer suporte a VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.  Você também pode configurar o RRAS para dar suporte a conexões IKEv2 e o servidor NPS para executar a autorização para as conexões VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configurar o registro automático de certificado na diretiva de grupo
Neste procedimento, você configurar diretiva de grupo no controlador de domínio para que os membros do domínio automaticamente solicitar certificados de usuário e computador. Isso permite que os usuários de VPN para solicitar e recuperar certificados de usuário para autenticam conexões VPN automaticamente. Da mesma forma, essa política permite que os servidores NPS para solicitar o servidor de certificados de autenticação automaticamente. 

Você registra manualmente os certificados nos servidores VPN.

>[!TIP]
>Para computadores associados não impedirem, consulte [configuração de autoridade de certificação para fora do domínio de computadores ingressados no](#ca-configuration-for-non-domain-joined-computers). Uma vez que o servidor RRAS não estiver ingressado no domínio, o registro automático não pode ser usado para registrar o certificado de gateway VPN.  Portanto, use um procedimento de solicitação de certificado off-line.

1. Em um controlador de domínio, abra o gerenciamento de diretiva de grupo.

2. No painel de navegação, clique com botão direito seu domínio (por exemplo, corp.contoso.com) e selecione **criar um GPO neste domínio e vinculá-lo aqui**.

3. Na caixa de diálogo Novo GPO, insira **política de registro automático**, em seguida, selecione **Okey**.

4. No painel de navegação, clique com botão direito **política de registro automático**, em seguida, selecione **editar**.

5. No Editor de gerenciamento de diretiva de grupo, conclua as seguintes etapas para configurar o registro automático de certificados do computador:

    1. No painel de navegação, acesse **configuração do computador** > **políticas** > **configurações do Windows**  >   **Configurações de segurança** > **diretivas de chave pública**.

    2. No painel de detalhes, clique com botão direito **cliente de serviços de certificado – registro automático**, em seguida, selecione **propriedades**.

    3. No cliente serviços certificado – registro automático de caixa de diálogo Propriedades, na **modelo de configuração**, selecione **habilitado**.

    4. Selecione **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados** e **Atualizar certificados que usam modelos de certificados**.

    5. Selecione **OK**.

6. No Editor de gerenciamento de diretiva de grupo, conclua as etapas a seguir para configurar registro automático de certificados do usuário:

    1. No painel de navegação, acesse **configuração do usuário** > **políticas** > **configurações do Windows**  >   **Configurações de segurança** > **diretivas de chave pública**.

    2. No painel de detalhes, clique com o botão direito do mouse em **Registro automático de cliente nos serviços de certificado** e selecione **Propriedades**.

    3. No cliente serviços certificado – registro automático de caixa de diálogo Propriedades, na **modelo de configuração**, selecione **habilitado**.

    4. Selecione **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados** e **Atualizar certificados que usam modelos de certificados**.

    5. Selecione **OK**.

    6. Feche o Editor de Gerenciamento de Política de Grupo.

7. Feche o Gerenciamento de Política de Grupo.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configuração de autoridade de certificação para computadores associados fora do domínio

Uma vez que o servidor RRAS não estiver ingressado no domínio, o registro automático não pode ser usado para registrar o certificado de gateway VPN.  Portanto, use um procedimento de solicitação de certificado off-line.

1. No servidor RRAS, gerar um arquivo chamado **VPNGateway.inf** com base na solicitação de política de certificado de exemplo fornecida no Apêndice uma (seção 0) e personalizar as seguintes entradas:

   - Na seção [NewRequest], substitua vpn.contoso.com usado para o nome da entidade com a escolhida [_cliente_] FQDN do ponto de extremidade VPN.

   - Na seção [Extensions], substitua vpn.contoso.com usado para nome alternativo da entidade com a escolhida [_cliente_] FQDN do ponto de extremidade VPN.

2. Salvar ou copiar o **VPNGateway.inf** para um local escolhido.

3. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o **VPNGateway.inf** arquivo e digite:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copie a recém-criada **VPNGateway.req** arquivo de saída para um servidor de autoridade de certificação ou estação de trabalho de acesso privilegiado (PAW).

5. Salvar ou copiar o **VPNGateway.req** arquivo para um local escolhido no servidor de autoridade de certificação ou estação de trabalho de acesso privilegiado (PAW).

6. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo VPNGateway.req criado na etapa anterior e digite:

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Se solicitado pela janela lista de autoridade de certificação, selecione a autoridade de certificação corporativa apropriada para atender à solicitação de certificado.

8. Copie a recém-criada **VPNGateway.cer** arquivo de saída para o servidor RRAS. 

9. Salvar ou copiar o **VPNGateway.cer** arquivo em um determinado local no servidor RRAS.

10. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo VPNGateway.cer criado na etapa anterior e digite:

    ```
    certreq -accept VPNGateway.cer
    ```

11. Execute o snap-in MMC de certificados, conforme descrito [aqui](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) selecionando o **conta de computador** opção.

12. Certifique-se de que um certificado válido existe para o servidor RRAS com as seguintes propriedades:

    - **Finalidades:** Autenticação de servidor, segurança de IP IKE intermediária 

    - **Modelo de certificado:** [_cliente_] servidor VPN

#### <a name="example-vpngatewayinf-script"></a>Exemplo: Script VPNGateway.inf

Aqui você pode ver um exemplo de script de uma diretiva de solicitação de certificado usado para solicitar um certificado de gateway VPN usando um processo fora de banda.

>[!TIP]
>Você pode encontrar uma cópia do script VPNGateway.inf VPN oferecendo IP Kit sob a pasta de diretivas de solicitação de certificado. Apenas atualizar 'Assunto' e '\_continuar\_' com valores específicos do cliente.

```
[Version] 

Signature="$Windows NT$"

[NewRequest]
Subject = "CN=vpn.contoso.com"
Exportable = FALSE   
KeyLength = 2048     
KeySpec = 1          
KeyUsage = 0xA0      
MachineKeySet = True
ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
RequestType = PKCS10 

[Extensions]
2.5.29.17 = "{text}"
_continue_ = "dns=vpn.contoso.com&"
```

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Criar os usuários VPN, servidores VPN e os grupos de servidores NPS

Neste procedimento, você pode adicionar um novo grupo do Active Directory (AD) que contém os usuários que podem usar VPN para se conectar à rede da organização.

Esse grupo tem duas finalidades:

- Ele define quais usuários têm permissão para registrar automaticamente os certificados de usuário que requer que a VPN.

- Ele define quais usuários o NPS autoriza o acesso à VPN.

Usando um grupo personalizado, se você nunca deseja revogar o acesso VPN do usuário, você pode remover esse usuário a partir do grupo.

Você também adicionar um grupo que contém servidores VPN e outro grupo que contém os servidores NPS. Você pode usar esses grupos para restringir as solicitações de certificado aos seus membros.

>[!NOTE]
>É recomendável VPN servidores que residem no DMA/perímetro não ingressar no domínio. No entanto, se você preferir que os servidores VPN ingressado no domínio para melhor capacidade de gerenciamento (diretivas de grupo, o agente de Backup/monitoramento, não há usuários locais para gerenciar e assim por diante), em seguida, adicionar um grupo do AD ao modelo de certificado do servidor VPN.

### <a name="configure-the-vpn-users-group"></a>Configurar o grupo de usuários da VPN

1. Em um controlador de domínio, abra o Active Directory Users and Computers.

2. Clique com botão direito um contêiner ou unidade organizacional, selecione **New**, em seguida, selecione **grupo**.

3. Na **nome do grupo**, insira **usuários da VPN**, em seguida, selecione **Okey**.

4. Clique com botão direito **usuários de VPN** e selecione **propriedades**.

5. Sobre o **membros** guia da caixa de diálogo Propriedades de usuários de VPN, selecione **Add**.

6. Na caixa de diálogo Selecionar usuários, adicionar todos os usuários que precisam de acesso VPN e selecione **Okey**.

7. Feche Usuários e Computadores do Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configure os grupos de servidores VPN e servidores NPS

1. Em um controlador de domínio, abra o Active Directory Users and Computers.

2. Clique com botão direito um contêiner ou unidade organizacional, selecione **New**, em seguida, selecione **grupo**.

3. Na **nome do grupo**, insira **servidores VPN**, em seguida, selecione **Okey**.

4. Clique com botão direito **servidores VPN** e selecione **propriedades**.

5. Sobre o **membros** guia da caixa de diálogo Propriedades de servidores VPN, selecione **Add**.

6. Selecione **tipos de objetos**, selecione o **computadores** caixa de seleção e, em seguida, selecione **Okey**.

7. Na **digite os nomes de objeto para selecionar**, digite os nomes dos seus servidores VPN e selecione **Okey**.

8. Selecione **Okey** para fechar a caixa de diálogo Propriedades de servidores VPN.

9. Repita as etapas anteriores para o grupo de servidores NPS.

10. Feche Usuários e Computadores do Active Directory.

## <a name="create-the-user-authentication-template"></a>Criar o modelo de autenticação de usuário

Neste procedimento, você deve configurar um modelo de autenticação de cliente-servidor personalizado. Esse modelo é necessário porque você deseja melhorar a segurança geral do certificado, selecionando os níveis de compatibilidade atualizado e escolhendo o provedor de criptografia de plataforma da Microsoft. Esta última alteração permite o uso do TPM nos computadores cliente para proteger o certificado. Para obter uma visão geral do TPM, consulte [Trusted Platform Module visão geral da tecnologia](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procedimento:**

1. Na autoridade de certificação, abra a autoridade de certificação.

2. No painel de navegação, clique com botão direito **modelos de certificado** e selecione **gerenciar**.

3. No console de modelos de certificado, clique com botão direito **usuário** e selecione **Duplicar modelo**.

   >[!WARNING]
   >Não marque **Apply** ou **Okey** a qualquer momento antes da etapa 10.  Se você selecionar esses botões antes de entrar em todos os parâmetros, muitas opções tornam-se fixos e não é mais editável. Por exemplo, na **Cryptography** guia, se _provedor de armazenamento criptográfico herdado_ mostra no campo de categoria de provedor, torna-se desativado, impedindo que qualquer outra alteração. A única alternativa é excluir o modelo e recriá-lo.  

4. Na caixa de diálogo Propriedades do novo modelo, sobre o **geral** guia, conclua as seguintes etapas:

   1. Na **nome de exibição do modelo**, digite **autenticação do usuário VPN**.

   2. Desmarque a **publicar o certificado no Active Directory** caixa de seleção.

5. Sobre o **segurança** guia, conclua as seguintes etapas:

   1. Selecione **Adicionar**.

   2. Em Selecionar usuários, computadores, contas de serviço ou caixa de diálogo grupos, insira **usuários de VPN**, em seguida, selecione **Okey**.

   3. Na **nomes de usuário ou grupo**, selecione **usuários da VPN**.

   4. Na **permissões para usuários de VPN**, selecione o **registrar** e **registrar automaticamente** caixas de seleção a **permitir** coluna.

      >[!TIP]
      >Certifique-se de manter a leitura de caixa de seleção selecionada. Em outras palavras, você precisará das permissões de leitura para o registro. 

   5. Na **nomes de usuário ou grupo**, selecione **os usuários do domínio**, em seguida, selecione **remover**.

6. Sobre o **compatibilidade** guia, conclua as seguintes etapas:

   1. Na **autoridade de certificação**, selecione **Windows Server 2012 R2**.

   2. Sobre o **alterações resultantes** caixa de diálogo, selecione **Okey**.

   3. Na **destinatário do certificado**, selecione **do Windows 8.1/Windows Server 2012 R2**.

   4. Sobre o **alterações resultantes** caixa de diálogo, selecione **Okey**.

7. Sobre o **tratamento de solicitação** guia, desmarque as **permitir que a chave privada seja exportada** caixa de seleção.

8. Sobre o **Cryptography** guia, conclua as seguintes etapas:

   1. Na **categoria de provedor**, selecione **Key Storage Provider**.

   2. Selecione **as solicitações devem usar um dos seguintes provedores**.

   3. Selecione o **provedor de criptografia de plataforma Microsoft** caixa de seleção.

9. Sobre o **nome da entidade** guia, se você não tiver um endereço de email listado em todas as contas de usuário, desmarque a **incluir nome de email no nome da entidade** e **nome de email** caixas de seleção.

10. Selecione **Okey** para salvar o modelo de certificado de autenticação de usuário de VPN.

11. Feche o console de Modelos de Certificado.

12. No painel de navegação do snap-in Autoridade de certificação, clique com botão direito **modelos de certificado**, selecione **New** e, em seguida, selecione **modelo de certificado a ser emitido**.

13. Selecione **autenticação de usuário de VPN**, em seguida, selecione **Okey**.

14. Feche o snap-in de autoridade de certificação.

## <a name="create-the-vpn-server-authentication-template"></a>Criar o modelo de autenticação do servidor VPN

Neste procedimento, você pode configurar um novo modelo de autenticação de servidor para o servidor VPN. Adicionando a política de aplicativo (IPsec) de segurança de IP IKE Intermediate permite que o servidor para certificados de filtro se mais de um certificado está disponível com a autenticação do servidor uso estendido de chave.

>[!IMPORTANT]
>Como os clientes VPN acessem este servidor da Internet pública, o assunto e os nomes alternativos são diferentes do nome de servidor interno. Como resultado, você não pode registrar automaticamente esse certificado em servidores VPN.

**Pré-requisitos:**

Servidores VPN ingressado no domínio

**Procedimento:**

1. Na autoridade de certificação, abra a autoridade de certificação.

2. No painel de navegação, clique com botão direito **modelos de certificado** e selecione **gerenciar**.

3. No console de modelos de certificado, clique com botão direito **servidores RAS e IAS** e selecione **Duplicar modelo**.

4. Na caixa de diálogo Propriedades do novo modelo na **gerais** guia **nome de exibição do modelo**, insira um nome descritivo para o servidor VPN, por exemplo, **autenticação do servidor VPN** ou **servidor RADIUS**.

5. Sobre o **extensões** guia, conclua as seguintes etapas:

    1. Selecione **políticas de aplicativo**, em seguida, selecione **editar**.

    2. No **Editar extensão de políticas de aplicativo** caixa de diálogo, selecione **Add**.

    3. Sobre o **adicionar política de aplicativo** caixa de diálogo, selecione **segurança de IP IKE intermediária**, em seguida, selecione **Okey**.
   
        Adicionando o IP IKE intermediária para o EKU de segurança ajuda em cenários onde mais de um certificado de autenticação de servidor existe no servidor VPN. Quando a segurança IP IKE intermediária estiver presente, o IPSec usa apenas o certificado com ambas as opções de EKU. Sem isso, a autenticação IKEv2 poderia falhar com o erro 13801: Credenciais de autenticação IKE são inaceitáveis.

    4. Selecione **Okey** para retornar para o **propriedades do novo modelo** caixa de diálogo.

6. Sobre o **segurança** guia, conclua as seguintes etapas:

    1. Selecione **Adicionar**.

    2. Sobre o **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo, digite **servidores VPN**, em seguida, selecione **Okey**.

    3. Na **nomes de usuário ou grupo**, selecione **servidores VPN**.

    4. Na **permissões para servidores VPN**, selecione o **registrar** caixa de seleção a **permitir** coluna.

    5. Na **nomes de usuário ou grupo**, selecione **servidores RAS e IAS**, em seguida, selecione **remover**.

7. Sobre o **nome da entidade** guia, conclua as seguintes etapas:

    1. Selecione **fornecer na solicitação**.

    2. Sobre o **modelos de certificado** caixa de diálogo de aviso, selecione **Okey**.

8. (Opcional) Se você estiver configurando o acesso condicional para conectividade VPN, selecione a **tratamento de solicitação** guia e, em seguida, selecione **permitir que a chave privada seja exportada**.

9. Selecione **Okey** para salvar o modelo de certificado do servidor VPN.

10. Feche o console de Modelos de Certificado.

11. No painel de navegação do snap-in Autoridade de certificação, clique com botão direito **modelos de certificado**, selecione **New** e, em seguida, selecione **modelo de certificado a ser emitido**.

12. Selecione o nome que você escolheu na etapa 4 acima e, em seguida, clique em **Okey**.

13. Feche o snap-in de autoridade de certificação.

## <a name="create-the-nps-server-authentication-template"></a>Criar o modelo de autenticação do servidor NPS

O modelo de certificado de terceiro e último criar é o modelo de autenticação do servidor NPS. O modelo de autenticação do servidor NPS é um simples copiar do modelo de servidores RAS e IAS protegido para o grupo de servidor NPS que você criou anteriormente nesta seção.

Você irá configurar este certificado para o registro automático.

**Procedimento:**

1. Na autoridade de certificação, abra a autoridade de certificação.

2. No painel de navegação, clique com botão direito **modelos de certificado** e selecione **gerenciar**.

3. No console de modelos de certificado, clique com botão direito **servidores RAS e IAS**e selecione **Duplicar modelo**.

4. Na caixa de diálogo Propriedades do novo modelo na **gerais** guia **nome de exibição do modelo**, tipo **autenticação do servidor NPS**.

5. Sobre o **segurança** guia, conclua as seguintes etapas:

    1. Selecione **Adicionar**.

    2. Sobre o **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo, digite **servidores NPS**, em seguida, selecione **Okey**.

    3. Na **nomes de usuário ou grupo**, selecione **servidores NPS**.

    4. Na **permissões para servidores NPS**, selecione o **registrar** e **registrar automaticamente** caixas de seleção a **permitir** coluna.

    5. Na **nomes de usuário ou grupo**, selecione **servidores RAS e IAS**, em seguida, selecione **remover**.

6. Selecione **Okey** para salvar o modelo de certificado do servidor NPS.

7. Feche o console de Modelos de Certificado.

8. No painel de navegação do snap-in Autoridade de certificação, clique com botão direito **modelos de certificado**, selecione **New** e, em seguida, selecione **modelo de certificado a ser emitido**.

9. Selecione **autenticação do servidor NPS**e selecione **Okey**.

10. Feche o snap-in de autoridade de certificação.

## <a name="enroll-and-validate-the-user-certificate"></a>Registrar e validar o certificado de usuário

Porque você está usando a diretiva de grupo para registrar automaticamente certificados de usuário, você só precisa atualizar a política e Windows 10 serão registrados automaticamente a conta de usuário para o certificado correto. Em seguida, você pode validar o certificado no console de certificados.

**Procedimento:**

1. Entre em um computador cliente ingressado no domínio como um membro do **usuários da VPN** grupo.

2. Pressione a tecla Windows + R, digite **gpupdate /force**, e pressione Enter.

3. No menu Iniciar, digite **certmgr. msc**, e pressione Enter.

4. No snap-in de certificados, sob **pessoais**, selecione **certificados**. Os certificados são exibidos no painel de detalhes.

5. O certificado que tem o nome de usuário do domínio atual e, em seguida, selecione **aberto**.

6. Sobre o **geral** guia, confirme se a data listada sob **válido de** é a data de hoje. Caso contrário, o certificado incorreto pode ter selecionado.

7. Selecione **Okey**e feche o snap-in de certificados.

## <a name="enroll-and-validate-the-server-certificates"></a>Registrar e validar os certificados do servidor

Diferentemente o certificado de usuário, você deve registrar manualmente o certificado do servidor VPN. Depois que você tiver registrado a ele, validá-lo usando o mesmo processo usado para o certificado de usuário. Como o certificado de usuário, o servidor NPS inscreve automaticamente seu certificado de autenticação, portanto, tudo o que você precisa fazer é validá-la.

>[!NOTE]
>Talvez você precise reiniciar os servidores VPN e o NPS para que eles atualizem suas associações de grupo antes de concluir estas etapas.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Registrar e validar o certificado do servidor VPN

1. No menu de início do servidor VPN, digite **certlm**, e pressione Enter.

2. Clique com botão direito **pessoais**, selecione **todas as tarefas** e, em seguida, selecione **Solicitar novo certificado** para iniciar o Assistente de registro de certificado.

3. Na página antes de começar, selecione **próxima**.

4. Na página Selecionar política de registro de certificado, selecione **próxima**.

5. Na página solicitar certificados, selecione a caixa de seleção ao lado do servidor VPN para selecioná-lo.

6. Na caixa de seleção do servidor VPN, selecione **mais informações são necessárias** para abrir a caixa de diálogo Propriedades do certificado e conclua as seguintes etapas:

    1. Selecione o **assunto** guia, selecione **nome comum** sob **nome da entidade**, no **tipo**.

    2. Sob **nome da entidade**, na **valor**, insira o nome dos clientes do domínio externo usado para se conectar à VPN, por exemplo, vpn.contoso.com, em seguida, selecione **adicionar**.

    3. Sob **nome alternativo**, na **tipo**, selecione **DNS**.

    4. Sob **nome alternativo**, na **valor**, insira todos os nomes de servidor, os clientes usam para se conectar à VPN, por exemplo, vpn.contoso.com, vpn, 132.64.86.2.

    5. Selecione **adicionar** depois de inserir cada nome.

    6. Selecione **Okey** quando terminar.

7. Selecione **registrar**.

8. Selecione **concluir**.

9. No snap-in de certificados, sob **pessoais**, selecione **certificados**.
    
    Os certificados listados são exibidos no painel de detalhes.

10. Clique com botão direito no certificado que tem o servidor VPN de nome e, em seguida, selecione **aberto**.

11. Sobre o **geral** guia, confirme se a data listada sob **válido de** é a data de hoje. Caso contrário, o certificado incorreto pode ter selecionado.

12. Sobre o **detalhes** guia, selecione **uso avançado de chave**e verifique **segurança de IP IKE intermediária** e **autenticação de servidor** Exibir na lista.

13. Selecione **Okey** para fechar o certificado.

14. Feche o snap-in de certificados.

### <a name="validate-the-nps-server-certificate"></a>Validar o certificado do servidor NPS

1. Reinicie o servidor NPS.

2. No menu de início do servidor NPS, digite **certlm**, e pressione Enter.

3. No snap-in de certificados, sob **pessoais**, selecione **certificados**.

    Os certificados listados são exibidos no painel de detalhes.

4. Clique com botão direito no certificado que tem o servidor NPS nome e, em seguida, selecione **aberto**.

5. Sobre o **geral** guia, confirme se a data listada sob **válido de** é a data de hoje. Caso contrário, o certificado incorreto pode ter selecionado.

6. Selecione **Okey** para fechar o certificado.

7. Feche o snap-in de certificados.

## <a name="next-steps"></a>Próximas etapas

[Etapa 3. Configurar o servidor de acesso remoto para sempre na VPN](vpn-deploy-ras.md): Nesta etapa, você deve configurar VPN de acesso remoto para permitir conexões de VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para se conectar a clientes autorizados de VPN.
