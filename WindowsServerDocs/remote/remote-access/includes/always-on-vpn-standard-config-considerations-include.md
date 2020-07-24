## <a name="standard-configuration-considerations"></a>Considerações de configuração padrão

Always On VPN tem muitas opções de configuração. No entanto, você escolhe a configuração de VPN, mas inclui as seguintes informações:

-   **Tipo de conexão.** A seleção de protocolo de conexão é importante e, em última análise, é disponibilizada com o tipo de autenticação que você usará. Para obter detalhes sobre os protocolos de túnel disponíveis, consulte [tipos de conexão VPN](/windows/security/identity-protection/vpn/vpn-connection-type/).

-   **Zona.** Nesse contexto, as regras de roteamento determinam se os usuários podem usar outras rotas de rede enquanto estiverem conectados à VPN.

    -   O _túnel dividido_ permite o acesso simultâneo a outras redes, como a Internet.

    -   O _túnel forçado_ requer que todo o tráfego passe exclusivamente pela VPN e não permita acesso simultâneo a outras redes.

-   **Disparar.** O _disparo_ determina como e quando uma conexão VPN é iniciada (por exemplo, quando um aplicativo é aberto, quando o dispositivo é ativado, manualmente pelo usuário). Para opções de disparo, consulte [Opções de perfil de VPN disparadas automaticamente](/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/).

-   **Autenticação do dispositivo ou do usuário.** Always On VPN usa certificados de dispositivo e conexão iniciada pelo dispositivo por meio de um recurso chamado [túnel de dispositivo](../vpn/vpn-device-tunnel-config.md). Essa conexão pode ser iniciada automaticamente e persistente, semelhante a uma conexão de túnel de infraestrutura do DirectAccess.

>[!TIP]
>Ao migrar do DirectAccess para Always On VPN, considere a possibilidade de começar com as opções de configuração que são comparáveis ao que você tem e, em seguida, expanda a partir daí.

Usando certificados de usuário, o cliente VPN Always On se conecta automaticamente, mas faz isso no nível de usuário (após a entrada do usuário) em vez de no nível do dispositivo (antes da entrada do usuário). A experiência ainda é direta para o usuário, mas dá suporte a mecanismos de autenticação mais avançados, como o Windows Hello para empresas.
