# Instalação do SAMBA

Pontuação: [30 pontos] 

Documente a instalação do Samba no Alpine Linux

Dica: 

1. Use o [aplicativo ChatGPT do Celular](https://play.google.com/store/apps/details?id=com.openai.chatgpt&hl=pt_BR)
2. Copie a resposta (Formato Markdown)
3. Cole em uma conversa do WhatsApp com seu colega de grupo
4. Abra o [WhatsApp Web](https://web.whatsapp.com/) em um PC/Notebook
5. Copie o conteúdo da conversa, que deve estar no formato Markdown, e cole em sua documentação.

!!! note "Dica de *prompt* para o [ChatGPT](https://chatgpt.com)" 

- Como instalar um servidor Samba como contraldor de domínio do ActiveDirectory. O sistema operacional é o Alpine Linux. O domínio "<estado>.lab".





Aqui está a documentação em Markdown da instalação do samba-tool no Alpine Linux com base nos comandos fornecidos:

# Instalação e Configuração do Samba como Controlador de Domínio no Alpine Linux

## 1. Configuração de Hostname e Hosts

### Verificar o arquivo /etc/hosts
```bash
cat /etc/hosts
```

### Verificar o arquivo /etc/hostname
```bash
cat /etc/hostname
```

### Alterar o hostname
```bash
echo goiania.goias.lab > /etc/hostname
```

### Verificar a alteração
```bash
cat /etc/hostname
hostname -F /etc/hostname
hostname
```

### Editar o arquivo /etc/hosts
```bash
nano /etc/hosts
```

### Adicione ou edite as seguintes entradas:
```bash
127.0.0.1       alpine.ifrn.local alpine localhost.localdomain localhost
::1             localhost localhost.localdomain
127.0.1.1       goiania.goias.lab goiania
```

### Testar a configuração de hostname
```bash
ping goiania
ping goiania.goias.lab
```

## 2. Sincronização de Hora

### Instalar e configurar o Chrony
```bash
apk add chrony
```

### Verificar serviços e portas
```bash
ls /etc/init.d
ss -tnl
```

### Iniciar e adicionar Chrony aos serviços
```bash
/etc/init.d/chronyd status
/etc/init.d/chronyd start
rc-update add chronyd
```

## 3. Instalação e Configuração do Samba

### Instalar pacotes necessários
```bash
apk add samba-dc krb5
```

### Verificar e configurar o arquivo smb.conf
```bash
cat /etc/samba/smb.conf
echo > /etc/samba/smb.conf
nano /etc/samba/smb.conf
```

### Adicione a seguinte configuração mínima para o Samba como Controlador de Domínio:
```bash
[global]
    server role = domain controller
    workgroup = GOIAS
    realm = goias.lab
    netbios name = GOIANIA
    passdb backend = samba4
    idmap_ldb:use rfc2307 = yes

[netlogon]
    path = /var/lib/samba/sysvol/goias.lab/scripts
    read only = No

[sysvol]
    path = /var/lib/samba/sysvol
    read only = No
```

### Criar diretórios necessários
```bash
mkdir -p -v /var/lib/samba/sysvol/goias.lab/scripts
```

### Provisionar o domínio
```bash
samba-tool domain provision --use-rfc2307 --interactive
```

## 4. Configuração do Serviço Samba

### Verificar o arquivo /etc/init.d/samba
```bash
cat /etc/init.d/samba
echo > /etc/init.d/samba
nano /etc/init.d/samba
```

### Adicione o seguinte conteúdo para gerenciar o serviço Samba:
```bash
#!/sbin/openrc-run

extra_started_commands="reload"

DAEMON=${SVCNAME#samba.}
SERVER_ROLE=samba-tool testparm --parameter-name="server role"  2>/dev/null | tail -1
if [ "$SERVER_ROLE" = "active directory domain controller" ]; then
    daemon_list="samba"
elif [ "$DAEMON" != "samba" ]; then
    daemon_list=$DAEMON
fi

depend() {
    need net
    after firewall
}

start_samba() {
    mkdir -p /var/run/samba
    start-stop-daemon --start --quiet --exec /usr/sbin/samba --
}

stop_samba() {
    start-stop-daemon --stop --quiet --pidfile /var/run/samba/samba.pid
}

start_smbd() {
    start-stop-daemon --start --quiet --exec /usr/sbin/smbd -- \
        ${smbd_options:-"-D"}
}

stop_smbd() {
    start-stop-daemon --stop --quiet --pidfile /var/run/samba/smbd.pid
}

start_nmbd() {
    start-stop-daemon --start --quiet --exec /usr/sbin/nmbd -- \
        ${nmbd_options:-"-D"}
}

stop_nmbd() {
    start-stop-daemon --stop --quiet --pidfile /var/run/samba/nmbd.pid
}

start_winbindd() {
    start-stop-daemon --start --quiet --exec /usr/sbin/winbindd -- \
        $winbindd_options
}

stop_winbindd() {
    start-stop-daemon --stop --quiet --pidfile /var/run/samba/winbindd.pid
}

start() {
    for i in $daemon_list; do
        ebegin "Starting $i"
        start_$i
        eend $?
    done
}

stop() {
    for i in $daemon_list; do
        ebegin "Stopping $i"
        stop_$i
        eend $?
    done
}

reload() {
    for i in $daemon_list; do
        ebegin "Reloading $i"
        killall -HUP $i
        eend $?
    done
}
```
### Adicionar o Samba ao OpenRC
```bash
rc-update add samba
```

### Iniciar o serviço Samba
```bash
rc-service samba start
```

### Verificar portas abertas
```bash
ss -tnl
```

## 5. Verificar o Nível do Domínio
```bash
samba-tool domain level show
``` 

### Essa documentação cobre os principais passos para instalar e configurar o Samba como controlador de domínio no Alpine Linux.