---
date: 2025-09-30T22:52:57-03:00
# description: ""
image: "/images/distrobox.svg"
lastmod: 2025-09-30
showTableOfContents: false
tags: ["Distrobox", "Linux", "Immutable Systems", "Archlinux", "Containers"]
title: "Distrobox, a melhor ferramenta para sistemas imutáveis"
type: "post"
---
![Distrobox](/images/distrobox.svg "Uma caixa de pandora")

# Distrobox 

Esse ano quero me familiarizar mais com 
Distrobox é uma forma simples de tornar distribuições linux imutáveis como o fedora silverblue mais utilizáveis. Ele basicamente roda um container docker e integra ele da melhor forma ao sistema. Para que as aplicacões possam ser rodadas diretamente através dessa camada de isolamento.
Algo positivo na experiência também é poder utilizar qualquer imagem, e consequentemente, qualquer gerenciador de pacotes que vier nelas como o aur do archlinux e não ter que lidar com rodar o archlinux diariamente, se quebrar, gera outra e vida que segue, o sistema raiz em si está intacto.

Comando distrobox create gera a receita do container. Através dele podemos passar os argumentos que farão a configuração inicial da imagem. 
* `--name` para nomear o container
* `--home` para configurar uma pasta diferente para a home da sua distrobox, isso é interessante porque qualquer programa que for instalado não bagunçará sua home raiz.
* `hostname` para aparecer do lado do seu usuário na emulação de terminal user@hostname.
* `--init` utiliza um systemd isolado somente para o container, é interessante para que aplicações que utilizam do systemd possam funcionar, apps que utilizam docker, apps em gui normalmente não precisam disso.
* `--bind` já que iremos utilizar uma home separada da home do host então vamos criar links simbólicos utilizando o argumento bind para que quando dê ls na home do container, apareça as mesmas pastas de usuário do host. Dessa forma, caso exclua o container, os arquivos pessoais estarão a salvo.
* `--image` a imagem selecionada foi a ublue-os/bazzite-arch. Essa imagem é pensada para jogos mas já é pré configurada para esse ambiente e pensada para esse ambiente de distrobox, criada pensando em sistemas como o do steamdeck.

In the end of the article I have a installer script. But a very basic setup would be:
DISTROBOX_HOME="$HOME/Machines/archbox"
IMUTABLE_HOME="/var$HOME"
mkdir -p $DISTROBOX_HOME

Para sistemas imutáveis (Ex. Fedora Silverblue 42):
```shell
distrobox-create \
  --name archbox \
  --home "/var$DISTROBOX_HOME" \
  --hostname archbox \
  --init \
  --image ghcr.io/ublue-os/arch-distrobox:latest \
  --volume "$IMUTABLE_HOME/Desktop:$DISTROBOX_HOME/Desktop:rw" \
  --volume "$IMUTABLE_HOME/Downloads:$DISTROBOX_HOME/Downloads:rw" \
  --volume "$IMUTABLE_HOME/Documents:$DISTROBOX_HOME/Documents:rw" \
  --volume "$IMUTABLE_HOME/Pictures:$DISTROBOX_HOME/Pictures:rw" \
```

```shell
distrobox enter archbox
```

```shell
cat > packages.txt <<EOF
systemd
podman
base-devel
git
vim
neovim
python
python-pip
nodejs
npm
yarn
rustup
docker
gcc
clang
go
fzf
calc
openssl
wget
aria2
cmake
gdb
make
tmux
htop
curl
unzip
zip
unrar
EOF
sudo pacman -Syu --needed - < packages.txt
rm packages.txt
```

## Example Script
Veja o código:
[devops-tools/distrobox/deploy_archbox_bazite.sh](https://github.com/rodhfr/devops-tools/blob/main/distrobox/deploy_archbox_bazite.sh)

E teste você mesmo! 
curl -sSL https://raw.githubusercontent.com/rodhfr/devops-tools/refs/heads/main/distrobox/deploy_archbox_bazite.sh | bash
