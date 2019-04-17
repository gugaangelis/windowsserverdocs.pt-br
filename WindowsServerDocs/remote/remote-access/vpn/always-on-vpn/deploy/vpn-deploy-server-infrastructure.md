---
title: Configurar a infraestrutura de servidor
description: Nesta etapa, você instala e configurar os componentes de servidor necessários para dar suporte a VPN. Os componentes de servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 21b494bea1990fb8424537205db483d977331465
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066991"
---
# Etapa 2. Configurar a infraestrutura de servidor

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** etapa 1. Planejar a implantação de VPN sempre ativa](always-on-vpn-deploy-planning.md)<br>
& #187;  [ **Próximo:** etapa 3. Configurar o servidor de acesso remoto para sempre em VPN](vpn-deploy-ras.md)


Nesta etapa, você instala e configurar os componentes de servidor necessários para dar suporte a VPN. Os componentes de servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.  Você também pode configurar RRAS para dar suporte a conexões IKEv2 e o servidor NPS para executar a autorização para as conexões VPN.

## Configurar o registro automático de certificado na política de grupo
Este procedimento, você configura a política de grupo no controlador de domínio para que os membros do domínio automaticamente solicitam certificados de usuário e computador. Isso permite que os usuários de VPN para solicitar e recuperar certificados de usuário que autenticar conexões VPN automaticamente. Da mesma forma, esta configuração de política permite que os servidores NPS para solicitar o servidor certificados de autenticação automaticamente. 

Você registrar manualmente certificados em servidores VPN.

>[!TIP]
>Para computadores que ingressaram não domained, consulte a [configuração de autoridade de certificação para fora do domínio associado computadores](#ca-configuration-for-non-domain-joined-computers) abaixo. Como o servidor RRAS não é ingressado no domínio, o registro automático não pode ser usado para registrar o certificado de gateway VPN.  Portanto, use um procedimento de solicitação de certificado off-line. 


1.  Em um controlador de domínio, abra o gerenciamento de política de grupo.

2.  No painel de navegação, clique com botão direito seu domínio (por exemplo, corp.contoso.com) e clique em**criar um GPO neste domínio e vinculá-lo aqui**.

3.  Na caixa de diálogo Novo GPO, digite **Diretiva de registro automático**e clique em **Okey**.

4.  No painel de navegação, clique com botão direito **Diretiva de registro automático**e clique em **Editar**.

5.  No Editor de gerenciamento de política de grupo, conclua as seguintes etapas para configurar o registro automático de certificado de computador:

    1.  No painel de navegação, clique em **Políticas de chave do computador Configuration\\Policies\\Windows configurações Settings\\Public**.

    2.  No painel de detalhes, clique com botão direito**Serviços cliente – registro automático de certificado**e clique em **Propriedades**.

    3.  No cliente de serviços de certificado – caixa de diálogo Propriedades de registro automático no **Modelo de configuração**, clique em **ativado**.

    4.  Selecione**Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados**e**Atualizar certificados que usam modelos de certificado**.

    5.  Clique em**Okey**.

6.  No Editor de gerenciamento de política de grupo, conclua as seguintes etapas para configurar registro automático de certificado do usuário:

    1.  No painel de navegação, clique em **Políticas de chave do usuário Configuration\\Policies\\Windows configurações Settings\\Public**.

    2.  No painel de detalhes, clique com botão direito**Serviços cliente – registro automático de certificado**e clique em **Propriedades**.

    3.  No cliente de serviços de certificado – caixa de diálogo Propriedades de registro automático no **Modelo de configuração**, clique em **ativado**.

    4.  Selecione**Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados**e**Atualizar certificados que usam modelos de certificado**.

    5.  Clique em**Okey**.

    6.  Feche o Editor de Gerenciamento de Política de Grupo.

7.  Gerenciamento de política de grupo de fechar.

### Configuração de autoridade de certificação para computadores que ingressaram fora do domínio

Como o servidor RRAS não é ingressado no domínio, o registro automático não pode ser usado para registrar o certificado de gateway VPN.  Portanto, use um procedimento de solicitação de certificado off-line. 

1. No servidor RRAS, gere um arquivo chamado **VPNGateway.inf** com base na política de certificado de exemplo solicitar fornecida no Apêndice uma (seção 0) e personalizar as seguintes entradas: 

   - Na seção [NewRequest], substitua vpn.contoso.com usado para o nome do assunto com o ponto de extremidade do [_cliente_] VPN FQDN escolhido.

   - Na seção [Extensions], substitua vpn.contoso.com usado para o nome alternativo do assunto com o ponto de extremidade do [_cliente_] VPN FQDN escolhido.

2. Salvar ou copiar o arquivo **VPNGateway.inf** para um local escolhido.

3. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo **VPNGateway.inf** e digite:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copie o arquivo de saída **VPNGateway.req** recém-criado para um servidor de autoridade de certificação ou em uma estação de trabalho de acesso privilegiado (PATA). 

5. Salvar ou copiar o arquivo **VPNGateway.req** para um local escolhido no servidor de autoridade de certificação, ou uma estação de trabalho de acesso privilegiado (PATA).

6. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo VPNGateway.req criado na etapa anterior e digite: 

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Se solicitado pela janela lista de autoridade de certificação, selecione a autoridade de certificação corporativa apropriado para atender à solicitação de certificado.

8. Copie o arquivo de saída **VPNGateway.cer** recém-criado para o servidor RRAS. 

9. Salvar ou copiar o arquivo **VPNGateway.cer** para um local escolhido no servidor RRAS.

10. Em um prompt de comando com privilégios elevados, navegue até a pasta que contém o arquivo VPNGateway.cer criado na etapa anterior e digite:
   
   ```
   certreq -accept VPNGateway.cer
   ```

11. Execute o snap-in do MMC certificados como descrito [aqui](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) selecionando a opção de **conta de computador** .

12. Certifique-se de que existe um certificado válido para o servidor RRAS com as seguintes propriedades:

   - **Finalidades:** Autenticação de servidor, segurança IP IKE intermediária 

   - **Modelo de certificado:** [_Cliente_] Servidor VPN

#### Exemplo: Script de VPNGateway.inf

Aqui você pode ver um script de exemplo de uma diretiva de solicitação de certificado usado para solicitar um certificado de gateway VPN usando um processo fora de banda.

>[!TIP]
>Você pode encontrar uma cópia do script VPNGateway.inf no VPN oferecendo IP Kit sob a pasta diretivas de solicitação de certificado. Só atualize o 'Assunto' e '\_continue\_' com valores específicos do cliente.

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

## Criar os usuários VPN, servidores VPN e grupos de servidores NPS

Neste procedimento, você pode adicionar um novo grupo do Active Directory (AD) que contém os usuários que podem usar a VPN para se conectar à rede da organização.

Esse grupo tem duas finalidades:

-   Ele define quais usuários têm permissão para registrar automaticamente para os certificados de usuário que requer a VPN.

-   Ele define quais usuários autoriza o NPS para acesso VPN.

Ao usar um grupo personalizado, se você quiser revogar o acesso VPN do usuário, você pode remover esse usuário a partir do grupo.

Você também adicionar um grupo que contém servidores VPN e outro grupo que contém servidores NPS. Você pode usar esses grupos para restringir solicitações de certificado aos seus membros.

>[!NOTE] 
>É recomendável VPN servidores que residem em DMA/perímetro não ingressar no domínio. No entanto, se você preferir que os servidores VPN que ingressaram no domínio para melhor capacidade de gerenciamento (políticas de grupo, o agente de Backup/Monitoring, nenhum usuário local para gerenciar e assim por diante), em seguida, adicionar um grupo do AD para o modelo de certificado do servidor VPN.

### Configurar o grupo de usuários de VPN

1.  Em um controlador de domínio, abra o Active Directory Users and Computers.

2.  Clique com botão direito a um contêiner ou unidade organizacional, clique em **novo** e clique em **grupo**.

3.  Em **nome do grupo**, digite **Os usuários da VPN**e clique em **Okey**.

4.  Clique com botão direito **Usuários VPN**e clique em **Propriedades**.

5.  Na guia **membros** da caixa de diálogo de propriedades de usuários de VPN, clique em **Adicionar**.

6.  Na caixa de diálogo Selecionar usuários, adicionar todos os usuários que precisam de VPN de acesso e clique em **Okey**.

7.  Feche os computadores e usuários do Active Directory.

### Configure os grupos de servidores VPN e servidores NPS

1.  Em um controlador de domínio, abra o Active Directory Users and Computers.

2.  Clique com botão direito a um contêiner ou unidade organizacional, clique em **novo** e clique em **grupo**.

3.  Em **nome do grupo**, digite **Servidores VPN**e clique em **Okey**.

4.  Clique com botão direito **Servidores VPN**e clique em **Propriedades**.

5.  Na guia **membros** da caixa de diálogo Propriedades de servidores VPN, clique em **Adicionar**.

6.  Clique em **Tipos de objeto**, marque a caixa de seleção de **computadores** e clique em **Okey**.

7.  Em **Inserir nomes do objeto para selecionar**, digite os nomes dos seus servidores VPN e clique em **Okey**.

8.  Clique em **Okey** para fechar a caixa de diálogo de propriedades de servidores VPN.

9.  Repita as etapas anteriores para o grupo de servidores NPS.

10. Feche os computadores e usuários do Active Directory.

## Criar o modelo de autenticação do usuário

Este procedimento, você configura um modelo de autenticação de cliente-servidor personalizado. Esse modelo é necessário porque você deseja melhorar a segurança geral do certificado, selecionando os níveis de compatibilidade atualizado e escolhendo o provedor de criptografia de plataforma da Microsoft. Essa última alteração permite que você use o TPM nos computadores cliente para proteger o certificado. Para obter uma visão geral do TPM, consulte [Trusted Platform Module visão geral da tecnologia](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procedimento:**

1.  Na autoridade de certificação, abra a autoridade de certificação.

2.  No painel de navegação, clique com botão direito**Modelos de certificado**e clique em**Gerenciar**.

3.  No console Modelos de certificado, clique com botão direito de**usuário**e clique em**Duplicar modelo**.

    >[!WARNING]
    >Não clique **Aplicar** ou **Okey** a qualquer momento antes da etapa 10.  Se você clicar esses botões antes de entrar em todos os parâmetros, muitas opções ficam fixo e não editável. Por exemplo, na guia **criptografia** , se o _Provedor de armazenamento criptográfico herdado_ mostra no campo de categoria de provedor, ele fica desabilitado, impedindo que qualquer outra alteração. A única alternativa é excluir o modelo e recriá-lo.  

4.  Na caixa de diálogo de propriedades do novo modelo, em**Geral**guia, conclua as seguintes etapas:

    1.  Em **nome para exibição do modelo**, digite **VPN autenticação do usuário**.

    2.  Desmarque a caixa de seleção **publicar o certificado no Active Directory** .

5.  Sobre a**segurança**guia, conclua as seguintes etapas:

    1.  Clique em **Adicionar**.

    2.  Em Selecionar usuários, computadores, contas de serviço ou caixa de diálogo grupos, digite **Os usuários da VPN**e clique em **Okey**.

    3.  Em **nomes de grupo ou usuário**, clique em **Usuários de VPN**.

    4.  Em **permissões para usuários de VPN**, marque as caixas de seleção de **registrar** e **registrar automaticamente** na coluna **Permitir** .

       >[!TIP]
       >Certifique-se de manter a caixa de seleção de leitura selecionada. Em outras palavras, você precisa de permissões de leitura para registro. 

    5.  Em **nomes de grupo ou usuário**, clique em **Usuários de domínio**e clique em **Remover**.

6.  Na guia **compatibilidade** , conclua as seguintes etapas:

    1.  Na **Autoridade de certificação**, clique em **Windows Server2012R2**.

    2.  Na caixa de diálogo **resultante alterações** , clique em **Okey**.

    3.  No **destinatário do certificado**, clique em **Windows 8.1/Windows Server2012R2**.

    4.  Na caixa de diálogo **resultante alterações** , clique em **Okey**.

7.  Na guia **Tratamento de solicitação** , desmarque a caixa de seleção **Permitir que a chave privada seja exportada** .

8.  Na guia **criptografia** , conclua as seguintes etapas:

    1.  Na **Categoria de provedor**, clique no **Provedor de armazenamento de chave**.

    2.  Clique em **solicitações devem usar um dos seguintes provedores**.

    3.  Marque a caixa de seleção de **Provedor de criptografia de plataforma Microsoft** .

9.  Na guia **Nome do assunto** , se você não tiver um endereço de email listado em todas as contas de usuário, desmarque as caixas de seleção de **incluir o nome de email no nome do assunto** e o **nome de email** .

10. Clique em**Okey** para salvar o modelo de certificado de autenticação de usuário de VPN.

11. Console de modelos de certificado fechar.

12. No painel de navegação do theCertification Authoritysnap-in, clique com botão direito**Modelos de certificado**, clique em **novo** e, em seguida, clique em**Modelo de certificado a ser emitido**.

13. Clique em**Autenticação de usuário de VPN**e clique em**Okey**.

14. Feche o snap-in Autoridade theCertification.

## Criar o modelo de autenticação de servidor VPN

Neste procedimento, você pode definir um novo modelo de autenticação de servidor para o servidor VPN. Adicionando a política de aplicativo de segurança de IP (IPsec) IKE Intermediate permite que o servidor certificados de filtro se mais de um certificado está disponível com a autenticação de servidor uso estendido de chave.

>[!IMPORTANT] 
>Como os clientes VPN acessar este servidor pela Internet pública, o assunto e nomes alternativos são diferentes do nome do servidor interno. Como resultado, você não pode registrar automaticamente esse certificado em servidores VPN.

**Pré-requisitos:**<p>
Servidores VPN ingressado no domínio

**Procedimento:** 

1.  Na autoridade de certificação, abra a autoridade de certificação.

2.  No painel de navegação, clique com botão direito **Modelos de certificado**e clique em**Gerenciar**.

3.  No console Modelos de certificado, clique com botão direito**RAS e servidor IAS**e clique em**Duplicar modelo**.

4.  Na caixa de diálogo de propriedades do novo modelo, em**Geral**guia, no **nome para exibição do modelo**, insira um nome descritivo para o servidor VPN, por exemplo, **Autenticação de servidor VPN** ou **Servidor RADIUS**.

5.  As**extensões**guia, conclua as seguintes etapas:

    1.  Clique em**Diretivas de aplicativo**e clique em**Editar**.

    2.  Na caixa de diálogo **Editar extensão de diretivas de aplicativo** , clique em**Adicionar**.

    3.  Na caixa de diálogo **Adicionar diretiva de aplicativo** , clique em **segurança IP IKE intermediária**e clique em **Okey**.<p>Adicionar IP segurança IKE intermediária para o EKU Ajuda em cenários onde mais de um certificado de autenticação de servidor existe no servidor VPN. Quando a segurança IP IKE intermediária estiver presente, o IPSec usa apenas o certificado com ambas as opções de EKU. Sem isso, autenticação IKEv2 pode falhar com o erro 13801: credenciais de autenticação IKE são inaceitáveis.

    4.  Clique em **Okey** para retornar ao**Propriedades do novo modelo**caixa de diálogo.

6.  Sobre a**segurança**guia, conclua as seguintes etapas:

    1.  Clique em **Adicionar**.

    2.  Na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , digite **Servidores VPN**e clique em **Okey**.

    3.  Em **nomes de grupo ou usuário**, clique em **Servidores VPN**.

    4.  Em **permissões para servidores VPN**, marque a caixa de seleção de **registro** na coluna **Permitir** .

    5.  Em **nomes de grupo ou usuário**, clique em **servidores RAS e IAS**e clique em **Remover**.

7.  Na guia **Nome do assunto** , conclua as seguintes etapas:

    1.  Clique em **fornecer na solicitação**.

    2.  Na caixa de diálogo de aviso **Modelos de certificado** , clique em **Okey**.

8.  (Opcional) *Se você estiver configurando o acesso condicional para conectividade VPN*, clique na guia **Tratamento de solicitação** e clique em **Permitir que a chave privada seja exportada** para selecioná-lo.

9.  Clique em**Okey** para salvar o modelo de certificado do servidor VPN.

10. Console de modelos de certificado fechar.

11. No painel de navegação da certificação Authoritysnap-in, clique com botão direito**Modelos de certificado**, clique em **novo** e, em seguida, clique em**Modelo de certificado a ser emitido**.

12. Reinicie os serviços de autoridade de certificação.

13. Selecione o nome que você escolheu na etapa 4 acima e clique em**Okey**.

14. Feche o snap-in Autoridade theCertification.

## Criar o modelo de autenticação de servidor NPS

O modelo de certificado de terceiro e último criar é o modelo de autenticação do servidor NPS. O modelo de autenticação de servidor NPS é uma cópia simple do modelo RAS e servidor IAS protegido ao grupo de servidores NPS que você criou anteriormente nesta seção. 

Você irá configurar esse certificado para registro automático.

**Procedimento:**

1.  Na autoridade de certificação, abra a autoridade de certificação.

2.  No painel de navegação, clique com botão direito **Modelos de certificado**e clique em**Gerenciar**.

3.  No console Modelos de certificado, clique com botão direito**RAS e servidor IAS**e selecione o **Modelo duplicado**.

4.  Na caixa de diálogo de propriedades do novo modelo, em**Geral**guia, em **nome de exibição do modelo**, digite a **Autenticação do servidor NPS**.

5.  Sobre a**segurança**guia, conclua as seguintes etapas:

    1.  Clique em **Adicionar**.

    2.  Na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , digite **Servidores NPS**e clique em **Okey**.

    3.  Em **nomes de grupo ou usuário**, clique em **Servidores NPS**.

    4.  Em **permissões para servidores NPS**, marque as caixas de seleção de **registrar** e **registrar automaticamente** na coluna **Permitir** .

    5.  Em **nomes de grupo ou usuário**, clique em **servidores RAS e IAS**e clique em **Remover**.

6.  Clique em**Okey** para salvar o modelo de certificado do servidor NPS.

7.  Console de modelos de certificado fechar.

8.  No painel de navegação da certificação Authoritysnap-in, clique com botão direito**Modelos de certificado**, clique em **novo** e, em seguida, clique em**Modelo de certificado a ser emitido**.

9.  Clique em**Autenticação de servidor NPS**e clique em**Okey**.

10. Feche o snap-in Autoridade theCertification.

## Registrar e validar o certificado do usuário

Como você está usando a política de grupo para registrar os certificados de usuário, você só precisa atualizar a política e Windows 10 será automaticamente inscrito a conta de usuário para o certificado correto. Em seguida, você pode validar o certificado no console certificados.

**Procedimento:**

1.  Entre a um computador cliente ingressado no domínio como um membro do grupo de **Usuários de VPN** .

2.  Pressione a tecla Windows + R, digite **gpupdate /force**e pressione Enter.

3.  No menu Iniciar, digite **certmgr. msc**e pressione Enter.

4.  No snap-in certificados, em **particular**, clique em **certificados**. Os certificados aparecem no painel de detalhes.

5.  Clique com botão direito no certificado que tem seu nome de usuário de domínio atual e, em seguida, clique em **Abrir**.

6.  Na guia **Geral** , confirme que a data listada em **válida de** é de hoje. Se não for, talvez você tenha selecionado o certificado errado.

7.  Clique em **Okey**e, em seguida, feche o snap-in de certificados.

## Inscrever-se e validar os certificados do servidor

Ao contrário do certificado do usuário, você deve registrar manualmente o certificado do servidor VPN. Depois que você tiver registrado-lo, validá-lo usando o mesmo processo que você usou para o certificado do usuário. Como o certificado de usuário, o servidor NPS registra automaticamente seu certificado de autenticação, portanto, tudo que você precisa fazer é validá-lo.

>[!NOTE] 
>Talvez seja necessário reiniciar os servidores VPN e NPS para permitir que eles atualizem suas associações de grupo antes de concluir essas etapas.

### Inscrever-se e validar o certificado do servidor VPN

1.  No menu de início do servidor VPN, digite **certlm**e pressione Enter.

2.  Clique com botão direito **pessoal**, clique em **Todas as tarefas** e, em seguida, clique em **Solicitar novo certificado** para iniciar o Assistente de registro de certificado.

3.  Na página Antes de Começar, clique em **Avançar**.

4.  Na página Selecionar política de registro de certificado, clique em **Avançar**.

5.  Na página solicitar certificados, clique na caixa de seleção ao lado do servidor VPN para selecioná-lo.

6.  Sob a caixa de seleção do servidor VPN, clique em **que obter mais informações são necessárias** para abrir a caixa de diálogo de propriedades do certificado e conclua as seguintes etapas:

    1.  Clique na guia **assunto** , clique em **Nome comum** em **nome do assunto**, no **tipo**.

    2.  Em **nome do assunto**, no **valor**, insira o nome dos clientes do domínio externo usado para se conectar à VPN, por exemplo, vpn.contoso.com e clique em **Adicionar**.

    3.  Em **Nome alternativo**, **tipo**, clique em **DNS**.

    4.  Em **Nome alternativo**, em **valor**, insira todas do servidor nomes clientes usam para se conectar à VPN, por exemplo, vpn.contoso.com, vpn, 132.64.86.2.

    5.  Depois de inserir cada nome, clique em **Adicionar** .

    6.  Quando terminar, clique em **OK**.

7.  Clique em **Registrar**.

8.  Clique em **concluir**.

9.  No snap-in certificados, em **particular**, clique em **certificados**.<p>Os certificados listados aparecem no painel de detalhes.

10. Clique com botão direito no certificado que tem o nome do seu servidor VPN e, em seguida, clique em **Abrir**.

11. Na guia **Geral** , confirme que a data listada em **válida de** é de hoje. Se não for, talvez você tenha selecionado o certificado incorreto.

12. Na guia **detalhes** , clique em **Uso avançado de chave**e verificar se a **Autenticação de servidor** e de **segurança IP IKE intermediária** exibam na lista.

13. Clique **Okey** para fechar o certificado.

14. Feche o snap-in de certificados.

### Validar o certificado do servidor NPS

1.  Reinicie o servidor NPS.

2.  No menu de início do servidor NPS, digite **certlm**e pressione Enter.

3.  No snap-in certificados, em **particular**, clique em **certificados**.<p>Os certificados listados aparecem no painel de detalhes.

4.  Clique com botão direito no certificado que tem o nome do seu servidor NPS e, em seguida, clique em **Abrir**.

5.  Na guia **Geral** , confirme que a data listada em **válida de** é de hoje. Se não for, talvez você tenha selecionado o certificado incorreto.

6.  Clique **Okey** para fechar o certificado.

7.  Feche o snap-in de certificados.

## Próximas etapas
[Etapa 3. Configurar o servidor de acesso remoto para VPN Always On](vpn-deploy-ras.md): nesta etapa, você configura acesso remoto VPN para permitir conexões VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para emissão de endereços IP para se conectar clientes VPN autorizados.







---
