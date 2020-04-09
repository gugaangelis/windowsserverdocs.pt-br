---
title: Configurar a infraestrutura do servidor
description: Nesta etapa, você instala e configura os componentes do lado do servidor necessários para dar suporte à VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 7c09ae7a792030152780ce4eb0029cea3ca234d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818919"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Etapa 2. Configurar a infraestrutura do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 1. Planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)
- [**Em seguida:** Etapa 3. Configurar o servidor de acesso remoto para VPN Always On](vpn-deploy-ras.md)

Nesta etapa, você instalará e configurará os componentes do lado do servidor necessários para dar suporte à VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.  Você também configura o RRAS para dar suporte a conexões IKEv2 e ao servidor NPS para executar a autorização para as conexões VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configurar o registro automático de certificado no Política de Grupo
Neste procedimento, você configura Política de Grupo no controlador de domínio para que os membros do domínio solicitem automaticamente certificados de usuário e computador. Isso permite que os usuários VPN solicitem e recuperem certificados de usuário que autenticam conexões VPN automaticamente. Da mesma forma, essa política permite que os servidores NPS solicitem certificados de autenticação de servidor automaticamente. 

Você registra certificados manualmente em servidores VPN.

>[!TIP]
>Para computadores associados que não são dofatontes, consulte [configuração de AC para computadores não ingressados no domínio](#ca-configuration-for-non-domain-joined-computers). Como o servidor RRAS não está ingressado no domínio, o registro automático não pode ser usado para registrar o certificado de gateway de VPN.  Portanto, use um procedimento de solicitação de certificado offline.

1. Em um controlador de domínio, abra o gerenciamento de Política de Grupo.

2. No painel de navegação, clique com o botão direito do mouse no domínio (por exemplo, corp.contoso.com), selecione **criar um GPO nesse domínio e vincule-o aqui**.

3. Na caixa de diálogo novo GPO, digite **política de registro automático**e, em seguida, selecione **OK**.

4. No painel de navegação, clique com o botão direito do mouse em **política de registro automático**e selecione **Editar**.

5. No Editor de Gerenciamento de Política de Grupo, conclua as seguintes etapas para configurar o registro automático do certificado do computador:

    1. No painel de navegação, vá para **configuração do computador** > **políticas** > **configurações do Windows** > **configurações de segurança** > **políticas de chave pública**.

    2. No painel de detalhes, clique com o botão direito do mouse em **cliente de serviços de certificados – registro automático**e selecione **Propriedades**.

    3. Na caixa de diálogo cliente de serviços de certificados – Propriedades de registro automático, em **modelo de configuração**, selecione **habilitado**.

    4. Selecione **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados** e **Atualizar certificados que usam modelos de certificados**.

    5. Selecione **OK**.

6. No Editor de Gerenciamento de Política de Grupo, conclua as seguintes etapas para configurar o registro automático de certificado de usuário:

    1. No painel de navegação, vá para **configuração do usuário** > **políticas** > **configurações do Windows** > **configurações de segurança** > **políticas de chave pública**.

    2. No painel de detalhes, clique com o botão direito do mouse em **Registro automático de cliente nos serviços de certificado** e selecione **Propriedades**.

    3. Na caixa de diálogo cliente de serviços de certificados – Propriedades de registro automático, em **modelo de configuração**, selecione **habilitado**.

    4. Selecione **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados** e **Atualizar certificados que usam modelos de certificados**.

    5. Selecione **OK**.

    6. Feche o Editor de Gerenciamento de Política de Grupo.

7. Feche o Gerenciamento de Política de Grupo.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configuração de AC para computadores não ingressados no domínio

Como o servidor RRAS não está ingressado no domínio, o registro automático não pode ser usado para registrar o certificado de gateway de VPN.  Portanto, use um procedimento de solicitação de certificado offline.

1. No servidor RRAS, gere um arquivo chamado **VPNGateway. inf** com base na solicitação de política de certificado de exemplo fornecida no apêndice a (seção 0) e personalize as seguintes entradas:

   - Na seção [NewRequest], substitua vpn.contoso.com usado para o nome da entidade com o FQDN do ponto de extremidade de VPN [_Customer_] escolhido.

   - Na seção [Extensions], substitua vpn.contoso.com usado para o nome alternativo da entidade com o FQDN do ponto de extremidade de VPN [_Customer_] escolhido.

2. Salve ou copie o arquivo **VPNGateway. inf** para um local escolhido.

3. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo **VPNGateway. inf** e digite:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copie o arquivo de saída **VPNGateway. req** criado recentemente para um servidor de autoridade de certificação ou Paw (estação de trabalho de acesso privilegiado).

5. Salve ou copie o arquivo **VPNGateway. req** em um local escolhido no servidor da autoridade de certificação ou na Paw (estação de trabalho de acesso privilegiado).

6. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo VPNGateway. req criado na etapa anterior e digite:

   ```
   certreq -attrib "CertificateTemplate:[Customer]VPNGateway" -submit VPNgateway.req VPNgateway.cer
   ```

7. Se solicitado pela janela de lista de autoridades de certificação, selecione a autoridade de certificação corporativa apropriada para atender à solicitação de certificado.

8. Copie o arquivo de saída **VPNGateway. cer** criado recentemente para o servidor RRAS. 

9. Salve ou copie o arquivo **VPNGateway. cer** para um local escolhido no servidor RRAS.

10. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo VPNGateway. cer criado na etapa anterior e digite:

    ```
    certreq -accept VPNGateway.cer
    ```

11. Execute o snap-in do MMC de certificados, conforme descrito [aqui](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) selecionando a opção de **conta de computador** .

12. Verifique se existe um certificado válido para o servidor RRAS com as seguintes propriedades:

    - **Finalidades pretendidas:** Autenticação de servidor, segurança de IP IKE intermediária 

    - **Modelo de certificado:** [_cliente_] servidor VPN

#### <a name="example-vpngatewayinf-script"></a>Exemplo: VPNGateway. inf script

Aqui você pode ver um script de exemplo de uma política de solicitação de certificado usada para solicitar um certificado de gateway de VPN usando um processo fora de banda.

>[!TIP]
>Você pode encontrar uma cópia do script VPNGateway. inf no kit de IP da oferta de VPN na pasta políticas de solicitação de certificado. Somente atualize os ' Subject ' e '\_continuar\_' com valores específicos do cliente.

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

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Criar grupos de usuários VPN, servidores VPN e servidores NPS

Neste procedimento, você pode adicionar um novo grupo de Active Directory (AD) que contém os usuários com permissão para usar a VPN para se conectar à rede da sua organização.

Esse grupo tem duas finalidades:

- Ele define quais usuários têm permissão para se registrar automaticamente para os certificados de usuário que a VPN requer.

- Ele define quais usuários o NPS autoriza para acesso VPN.

Usando um grupo personalizado, se você quiser revogar o acesso de VPN de um usuário, poderá remover esse usuário do grupo.

Você também adiciona um grupo que contém servidores VPN e outro grupo que contém servidores NPS. Você usa esses grupos para restringir as solicitações de certificado aos seus membros.

>[!NOTE]
>Recomendamos que os servidores VPN que residem no DMA/perímetro não sejam ingressados no domínio. No entanto, se você preferir ter os servidores VPN ingressados no domínio para melhor capacidade de gerenciamento (políticas de grupo, agente de backup/monitoramento, nenhum usuário local para gerenciar e assim por diante), adicione um grupo do AD ao modelo de certificado do servidor VPN.

### <a name="configure-the-vpn-users-group"></a>Configurar o grupo de usuários VPN

1. Em um controlador de domínio, abra Active Directory usuários e computadores.

2. Clique com o botão direito do mouse em um contêiner ou unidade organizacional, selecione **novo**e selecione **grupo**.

3. Em **nome do grupo**, insira **usuários VPN**e, em seguida, selecione **OK**.

4. Clique com o botão direito do mouse em **usuários VPN** e selecione **Propriedades**.

5. Na guia **Membros** da caixa de diálogo Propriedades de usuários VPN, selecione **Adicionar**.

6. Na caixa de diálogo Selecionar usuários, adicione todos os usuários que precisam de acesso VPN e selecione **OK**.

7. Feche Usuários e Computadores do Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configurar os grupos servidores VPN e servidores NPS

1. Em um controlador de domínio, abra Active Directory usuários e computadores.

2. Clique com o botão direito do mouse em um contêiner ou unidade organizacional, selecione **novo**e selecione **grupo**.

3. Em **nome do grupo**, insira **servidores VPN**e, em seguida, selecione **OK**.

4. Clique com o botão direito do mouse em **servidores VPN** e selecione **Propriedades**.

5. Na guia **Membros** da caixa de diálogo Propriedades de servidores VPN, selecione **Adicionar**.

6. Selecione **tipos de objeto**, marque a caixa de seleção **computadores** e, em seguida, selecione **OK**.

7. Em **Inserir os nomes de objeto a serem selecionados**, insira os nomes dos servidores VPN e, em seguida, selecione **OK**.

8. Selecione **OK** para fechar a caixa de diálogo Propriedades de servidores VPN.

9. Repita as etapas anteriores para o grupo de servidores NPS.

10. Feche Usuários e Computadores do Active Directory.

## <a name="create-the-user-authentication-template"></a>Criar o modelo de autenticação de usuário

Neste procedimento, você configura um modelo de autenticação cliente-servidor personalizado. Esse modelo é necessário porque você deseja melhorar a segurança geral do certificado selecionando os níveis de compatibilidade atualizados e escolhendo o provedor Microsoft Platform crypto. Essa última alteração permite que você use o TPM nos computadores cliente para proteger o certificado. Para obter uma visão geral do TPM, consulte [visão geral da tecnologia de Trusted Platform Module](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

>[!IMPORTANT] 
>Provedor de criptografia de plataforma da Microsoft "requer um chip TPM, caso você esteja executando uma VM e receba o seguinte erro:" não é possível encontrar um CSP válido no computador local "ao tentar registrar manualmente o certificado, você precisa verificar" armazenamento de chaves de software da Microsoft Provedor "e o têm em segundo lugar depois de" provedor Microsoft Platform crypto "na guia criptografia nas propriedades do certificado.

**Procedure**

1. Na AC, abra autoridade de certificação.

2. No painel de navegação, clique com o botão direito do mouse em **modelos de certificado** e selecione **gerenciar**.

3. No console modelos de certificado, clique com o botão direito do mouse em **usuário** e selecione **duplicar modelo**.

   >[!WARNING]
   >Não selecione **aplicar** ou **OK** a qualquer momento antes da etapa 10.  Se você selecionar esses botões antes de inserir todos os parâmetros, muitas escolhas se tornarão fixas e não serão mais editáveis. Por exemplo, na guia **criptografia** , se o _provedor de armazenamento criptográfico herdado_ aparecer no campo Categoria do provedor, ele se tornará desabilitado, impedindo qualquer alteração adicional. A única alternativa é excluir o modelo e recriá-lo.  

4. Na caixa de diálogo Propriedades do novo modelo, na guia **geral** , conclua as seguintes etapas:

   1. Em **nome de exibição do modelo**, digite **autenticação de usuário VPN**.

   2. Desmarque a caixa de seleção **publicar certificado no Active Directory** .

5. Na guia **segurança** , conclua as seguintes etapas:

   1. Selecione **Adicionar**.

   2. Na caixa de diálogo Selecionar usuários, computadores, contas de serviço ou grupos, insira **usuários VPN**e, em seguida, selecione **OK**.

   3. Em **nomes de grupo ou de usuário**, selecione **usuários VPN**.

   4. Em **permissões para usuários VPN**, marque as caixas de seleção **registrar** e **registrar automaticamente** na coluna **permitir** .

      >[!TIP]
      >Certifique-se de manter a caixa de seleção ler marcada. Em outras palavras, você precisará das permissões de leitura para o registro. 

   5. Em **nomes de grupo ou de usuário**, selecione **usuários de domínio**e, em seguida, selecione **remover**.

6. Na guia **compatibilidade** , conclua as seguintes etapas:

   1. Em **autoridade de certificação**, selecione **Windows Server 2012 R2**.

   2. Na caixa de diálogo **alterações resultantes** , selecione **OK**.

   3. Em **destinatário do certificado**, selecione **Windows 8.1/Windows Server 2012 R2**.

   4. Na caixa de diálogo **alterações resultantes** , selecione **OK**.

7. Na guia **tratamento de solicitação** , desmarque a caixa de seleção **permitir que a chave privada seja exportada** .

8. Na guia **criptografia** , conclua as seguintes etapas:

   1. Em **categoria do provedor**, selecione **provedor de armazenamento de chaves**.

   2. **As solicitações Select devem usar um dos provedores a seguir**.

   3. Marque a caixa de seleção **provedor Microsoft Platform crypto** .

9. Na guia **nome da entidade** , se você não tiver um endereço de email listado em todas as contas de usuário, desmarque as caixas de seleção **incluir nome de email no nome da entidade** e **nome de email** .

10. Selecione **OK** para salvar o modelo de certificado de autenticação de usuário VPN.

11. Feche o console de Modelos de Certificado.

12. No painel de navegação do snap-in autoridade de certificação, clique com o botão direito do mouse em **modelos de certificado**, selecione **novo** e **modelo de certificado a ser emitido**.

13. Selecione **autenticação de usuário VPN**e, em seguida, selecione **OK**.

14. Feche o snap-in de autoridade de certificação.

## <a name="create-the-vpn-server-authentication-template"></a>Criar o modelo de autenticação do servidor VPN

Neste procedimento, você pode configurar um novo modelo de autenticação de servidor para o servidor VPN. A adição da política de aplicativo IKE Intermediate de IP (IPsec) permite que o servidor filtre certificados se mais de um certificado estiver disponível com o uso estendido de chave de autenticação do servidor.

>[!IMPORTANT]
>Como os clientes VPN acessam esse servidor da Internet pública, os nomes de assunto e alternativos são diferentes do nome do servidor interno. Como resultado, você não pode registrar automaticamente esse certificado em servidores VPN.

**Pré-requisitos**

Servidores VPN ingressados no domínio

**Procedure**

1. Na AC, abra autoridade de certificação.

2. No painel de navegação, clique com o botão direito do mouse em **modelos de certificado** e selecione **gerenciar**.

3. No console modelos de certificado, clique com o botão direito do mouse em **servidor RAS e ias** e selecione **duplicar modelo**.

4. Na caixa de diálogo Propriedades do novo modelo, na guia **geral** , em **nome de exibição do modelo**, insira um nome descritivo para o servidor VPN, por exemplo, autenticação do **servidor VPN** ou **servidor RADIUS**.

5. Na guia **extensões** , conclua as seguintes etapas:

    1. Selecione **políticas de aplicativo**e, em seguida, selecione **Editar**.

    2. Na caixa de diálogo **Editar extensão de políticas de aplicativo** , selecione **Adicionar**.

    3. Na caixa de diálogo **Adicionar política de aplicativo** , selecione **segurança IP Ike intermediária**e, em seguida, selecione **OK**.
   
        A adição de segurança IP IKE intermediária ao EKU ajuda em cenários em que existe mais de um certificado de autenticação de servidor no servidor VPN. Quando a segurança IP IKE intermediária estiver presente, o IPSec usará apenas o certificado com ambas as opções EKU. Sem isso, a autenticação IKEv2 pode falhar com o erro 13801: as credenciais de autenticação IKE são inaceitáveis.

    4. Selecione **OK** para retornar à caixa **de diálogo Propriedades do novo modelo** .

6. Na guia **segurança** , conclua as seguintes etapas:

    1. Selecione **Adicionar**.

    2. Na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , insira **servidores VPN**e, em seguida, selecione **OK**.

    3. Em **nomes de grupo ou de usuário**, selecione **servidores VPN**.

    4. Em **permissões para servidores VPN**, marque a caixa de seleção **registrar** na coluna **permitir** .

    5. Em **nomes de grupo ou de usuário**, selecione **Servidores RAS e ias**e, em seguida, selecione **remover**.

7. Na guia **nome da entidade** , conclua as seguintes etapas:

    1. Selecione **fornecer na solicitação**.

    2. Na caixa de diálogo aviso de **modelos de certificado** , selecione **OK**.

8. Adicional Se você estiver configurando o acesso condicional para conectividade VPN, selecione a guia **tratamento de solicitação** e, em seguida, selecione **permitir que a chave privada seja exportada**.

9. Selecione **OK** para salvar o modelo de certificado do servidor VPN.

10. Feche o console de Modelos de Certificado.

11. No painel de navegação do snap-in autoridade de certificação, clique com o botão direito do mouse em **modelos de certificado**, clique em **novo** e em **modelo de certificado a ser emitido**.

12. Reinicie os serviços de autoridade de certificação. (*)

13. No painel de navegação do snap-in autoridade de certificação, clique com o botão direito do mouse em **modelos de certificado**, selecione **novo** e **modelo de certificado a ser emitido**.

14. Selecione o nome que você escolheu na etapa 4 acima e clique em **OK**.

15. Feche o snap-in de autoridade de certificação.

* **Você pode parar/iniciar o serviço de AC executando o seguinte comando no CMD:**

```
Net Stop "certsvc"
Net Start "certsvc"
```

## <a name="create-the-nps-server-authentication-template"></a>Criar o modelo de autenticação do servidor NPS

O terceiro e último modelo de certificado a ser criado é o modelo de autenticação do servidor NPS. O modelo de autenticação do servidor NPS é uma cópia simples do modelo de servidor RAS e IAS protegido para o grupo de servidores NPS que você criou anteriormente nesta seção.

Você configurará esse certificado para o registro automático.

**Procedure**

1. Na AC, abra autoridade de certificação.

2. No painel de navegação, clique com o botão direito do mouse em **modelos de certificado** e selecione **gerenciar**.

3. No console modelos de certificado, clique com o botão direito do mouse em **servidor RAS e ias**e selecione **duplicar modelo**.

4. Na caixa de diálogo Propriedades do novo modelo, na guia **geral** , em **nome de exibição do modelo**, digite **autenticação do servidor NPS**.

5. Na guia **segurança** , conclua as seguintes etapas:

    1. Selecione **Adicionar**.

    2. Na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , insira **servidores NPS**e selecione **OK**.

    3. Em **nomes de grupo ou de usuário**, selecione **servidores NPS**.

    4. Em **permissões para servidores NPS**, marque as caixas de seleção **registrar** e **registrar automaticamente** na coluna **permitir** .

    5. Em **nomes de grupo ou de usuário**, selecione **Servidores RAS e ias**e, em seguida, selecione **remover**.

6. Selecione **OK** para salvar o modelo de certificado do servidor NPS.

7. Feche o console de Modelos de Certificado.

8. No painel de navegação do snap-in autoridade de certificação, clique com o botão direito do mouse em **modelos de certificado**, selecione **novo** e **modelo de certificado a ser emitido**.

9. Selecione **autenticação do servidor NPS**e selecione **OK**.

10. Feche o snap-in de autoridade de certificação.

## <a name="enroll-and-validate-the-user-certificate"></a>Registrar e validar o certificado de usuário

Como você está usando Política de Grupo para registrar automaticamente os certificados de usuário, só precisa atualizar a política e o Windows 10 registrará a conta de usuário para o certificado correto. Em seguida, você pode validar o certificado no console de certificados.

**Procedure**

1. Entre em um computador cliente ingressado no domínio como um membro do grupo de **usuários VPN** .

2. Pressione Windows Key + R, digite **gpupdate/force**e pressione Enter.

3. No menu Iniciar, digite **certmgr. msc**e pressione Enter.

4. No snap-in certificados, em **pessoal**, selecione **certificados**. Os certificados são exibidos no painel de detalhes.

5. Clique com o botão direito do mouse no certificado que tem o nome de usuário do domínio atual e selecione **abrir**.

6. Na guia **geral** , confirme se a data listada em **válido de** é a data atual. Se não estiver, você pode ter selecionado o certificado errado.

7. Selecione **OK**e feche o snap-in de certificados.

## <a name="enroll-and-validate-the-server-certificates"></a>Registrar e validar os certificados do servidor

Ao contrário do certificado de usuário, você deve registrar manualmente o certificado do servidor VPN. Depois de tê-lo registrado, valide-o usando o mesmo processo usado para o certificado do usuário. Assim como o certificado de usuário, o servidor NPS registra automaticamente seu certificado de autenticação, portanto, tudo o que você precisa fazer é validá-lo.

>[!NOTE]
>Talvez seja necessário reiniciar os servidores VPN e NPS para permitir que eles atualizem suas associações de grupo antes que você possa concluir estas etapas.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Registrar e validar o certificado do servidor VPN

1. No menu Iniciar do servidor VPN, digite **certlm. msc**e pressione Enter.

2. Clique com o botão direito do mouse em **pessoal**, selecione **todas as tarefas** e, em seguida, selecione **solicitar novo certificado** para iniciar o assistente de registro de certificado.

3. Na página antes de começar, selecione **Avançar**.

4. Na página Selecionar política de registro de certificado, selecione **Avançar**.

5. Na página solicitar certificados, marque a caixa de seleção ao lado do servidor VPN para selecioná-lo.

6. Na caixa de seleção servidor VPN, selecione **mais informações são necessárias** para abrir a caixa de diálogo Propriedades do certificado e conclua as seguintes etapas:

    1. Selecione a guia **assunto** , selecione **nome comum** em **nome da entidade**, em **tipo**.

    2. Em **nome da entidade**, em **valor**, insira o nome dos clientes de domínio externo usados para se conectar à VPN, por exemplo, VPN.contoso.com, em seguida, selecione **Adicionar**.

    3. Em **nome alternativo**, em **tipo**, selecione **DNS**.

    4. Em **nome alternativo**, em **valor**, insira todos os nomes de servidor que os clientes usam para se conectar à VPN, por exemplo, VPN.contoso.com, VPN, 132.64.86.2.

    5. Selecione **Adicionar** depois de inserir cada nome.

    6. Selecione **OK** quando terminar.

7. Selecione **registrar**.

8. Selecione **Concluir**.

9. No snap-in certificados, em **pessoal**, selecione **certificados**.
    
    Os certificados listados aparecem no painel de detalhes.

10. Clique com o botão direito do mouse no certificado que tem o nome do servidor VPN e selecione **abrir**.

11. Na guia **geral** , confirme se a data listada em **válido de** é a data atual. Se não estiver, você pode ter selecionado o certificado incorreto.

12. Na guia **detalhes** , selecione **uso avançado de chave**e verifique se a **segurança de IP Ike intermediária** e a **autenticação de servidor** são exibidas na lista.

13. Selecione **OK** para fechar o certificado.

14. Abra o snap-in Certificados.

### <a name="validate-the-nps-server-certificate"></a>Validar o certificado do servidor NPS

1. Reinicie o servidor NPS.

2. No menu Iniciar do servidor NPS, digite **certlm. msc**e pressione Enter.

3. No snap-in certificados, em **pessoal**, selecione **certificados**.

    Os certificados listados aparecem no painel de detalhes.

4. Clique com o botão direito do mouse no certificado que tem o nome do servidor NPS e selecione **abrir**.

5. Na guia **geral** , confirme se a data listada em **válido de** é a data atual. Se não estiver, você pode ter selecionado o certificado incorreto.

6. Selecione **OK** para fechar o certificado.

7. Abra o snap-in Certificados.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

[Etapa 3. Configurar o servidor de acesso remoto para Always On VPN](vpn-deploy-ras.md): nesta etapa, você configura a VPN de acesso remoto para permitir conexões VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para emissão de endereços IP para conectar clientes VPN autorizados.
