# Instalando uma distro linux no android


![IMG_20220809_012527](https://user-images.githubusercontent.com/81770118/183819951-15025d0e-e4f3-411a-b03e-0641c75f7783.jpg)




Neste tutorial você vai aprender a baixar uma distro linux no seu celular, no caso, o archlinux, porém o processo é o mesmo para outras distros

Você vai poder usar seu celular basucamente como um computador, porém se ele for muito fraco, recomendo não tentar usar uma interface grafica



# Instalando o termux

Primeiro de tudo, nós precisaremos de um app chamado termux, e alguns apps secundários dele

O termux é um emulador de terminal, e você pode baixar ele junto com seus apps secundários através dos links abaixo

https://f-droid.org/repo/com.termux_118.apk
https://f-droid.org/repo/com.termux.styling_29.apk
https://f-droid.org/repo/com.termux.api_51.apk


Após instalar o termux e seus plugins, somente o ícone do termux irá aparecer na tela inicial

Abra o termux e a primeira coisa a se fazer é mudar a fonte, para uma que suporte mais símbolos

Para mudar a fonte, aperte e segure na tela, então siga por esses botões

more... > style > choose font

Você pode escolher a fonte que mais gostar, eu particularmente uso a FiraCode



# Instalando a distro
O processo de instalação é bem simples, mas configurar é um pouco difícil, otermux sozinho já é extremamente poderoso, e você pode muito bem usar ele sem uma distro linux, caso  você seja iniciante, recomendo que use o termux puro por um tempo até se acostumar

Mas, se quiser se aventurar, então pode continuar  seguindo o tutorial

##Atualizar o termux
```
apt update --fix-missing
apt upgrade
```

O termux provavelmente irá te pedir confirmações e configurações, é só dar enter todas as vezes

## Instalando o arch

```
apt install termux-api proot-distro
```

Com o proot-distro instalado, você pode ver as distros disponíveis com `proot-distro list`

Você pode baixar a distro que quiser, porém o debian não tá fucionando direito, e o ubuntu é muito dependente do snap, que só fuciona se seu celular for rooteado

Ao menos pela minha experiência, o archlinux é a melhor opção, é uma distro poderosa e que tem uma boa perfomance

Se seu celular for muito fraco, talvez seja bom considerar o alpine, que é a distro mais leve disponível


```
proot-distro install archlinux
```

Isso demora em média 2 minutos



### Instalando pulseaudio (opcional)

Você provavelmente vai querer ter áudio no seu computador, e para isso é necessário baixar um pacote específico

Mas, caso não vá usar uma  interface gráfica ou caso não se importe com áudio, pode pular essa parte

```
apt install pulseaudio
```

# Logando na distro

Com a distro instalada, você pode logar nela com o seguinte comando:
```
proot-distro login archlinux
```

Mas é bem chato escrever isso tudo, então vamos criar um alias pra isso e aproveitar pra configurar o pulseaudio

Abra o arquivo _.bashrc_ com seu editor de código favorito, aqui vamos usar o nano por ser mais simples e já vir instalado no  termux

```
nano ~/.bashrc
```

Este arquivo deve conter o seguinte código

```
#!/bin/bash
alias arch='proot-distro login archlinux'
pulseaudio --start --exit-idle-time=-1
```

Caso não vá usar o pulseaudio, apague a ultima linha

Altere o alias de acordo com a distro que você escolheu, ou não mexa em nada pra deixar como arch 

salve o arquivo com __CTRL s__ e saia do nano com __CTRL x__

Carregue as configurações do _.bashrc_ na sessão atual

```
source .bashrc
```

Agora, logue no archlinux digitando o alias que definimos pra aquele código grandão

```
arch
```

Pronto, você já está usando um archlinux, agora só faltam algumas configurações específicas dele, e aprender o básico sobre como usar ele

# Archlinux

## Pacman

O pacman é o __gerenciador de pacotes do arch__, tal como o apt é no debian

vamos ver alguns comandos básicos 

1- `pacman -Syu pacote`
		Equivalente ao `apt install pacote`
		Instala um ou mais pacotes, e atualiza os pacotes já instalados

2- `pacman -Ss palavra`
		Equivalente ao `apt search pacote`
		Pesquisa por um pacote que tenha a _palavra_ pesquisada em seu nome ou descrição

3- `pacman -Rs pacote`
		__Parecido__ com o  `apt remove pacote`
		Remove um pacote junto de todas suas dependências que não são usadas por outro pacote


### Cores
Por algum motivo, o pacman por padrão não mostra cores diferentes, e sim tudo branco, o que pode tornar bem difícil a leitura ao pesquisar por um pacote

Vamos habilitar as cores no pacman, para isso é necessário abrir o arquivo de configurações dele

```
nano /etc/pacman.conf
```

Este arquivo possui várias configurações do pacman, a maioria delas estão comentadas (um # no começo da linha), inclusive a que habilita cores

Tudo que você precisa fazer é procurar pela linha `#color` e então descomentar ela apagando o #, deixando só `color`

Após isso, é só salvar e sair `CTRL s` `CTRL x`

## Configurando
Agora que você sabe como fuciona o pacman, vamos configurar o arch

Primeiro, vamoa baixar alguns pacotes e atualizar os existentes

```
pacman -Syu sudo i3 dmenu tigervnc
```
ou, só `pacman -Syu sudo` caso você não vá usar uma interface gráfica

Nós iremos usar o i3 como interface por ocupar menos memória que as outras, justamente por ser  um gerenciador de janelas, você pode usar a interface que quiser caso seu celular seja potente, mas  recomendo priorizar o desempemho e continuar com o i3

O tigervnc você descobrirá o que é mais pra frente


### Criando usuário

Para criar um usuário chamado _outragedline_, os comandos seriam esses:

Cria o usuário
```
useradd outragedline
```

Define uma senha ao usuário
```
passwd outragedline
```

Use uma senha que você vá se lembrar, não precisa ser nada complexo, você irá usar essa senha toda hora


Após ter seu usuário criado e con senha, vamos cadastrar ele como um dos usuários do sudo

```
nano /etc/sudoers
```
Neste arquivo você irá procurar pela linha
```
root ALL=(ALL:ALL) ALL
```

E logo abaixo dela irá escrever a mesma coisa, porem com seu usuário
```
outragedline ALL=(ALL:ALL) ALL
```

Salve o arquivo e saia do nano


Após criar seu usuário, você precisa logar nele
```
su - outragedline
```

E pronto, está tudo configurado, você já está usando um archlinux da mesma forma que seria em um pc

### Interface
Agora, sobre a interface, você não consegue rodar uma interface nativamente pelo termux, então o que podemos fazer é criar um servidor VNC no termux e usar um cliente VNC para ver a interface

Como cliente VNC, eu uso VNC Viewer, disponível gratuitamente na play store

Para criar um servidor VNC no arch,primeiro defina uma senha de acesso
```
vncpasswd
```
Não precisa ser nada conplicado, qualquer coisa tipo 1234 tá bom


Após isso, você só precisa usar o comando `vncserver` e indicar a porta onde o servidor deve ser criada, por exemplo
```
vncserver :1
```

O comando acima irá criar um servidor na porta 5901, se eu tivesse indicado 2, teria sido na porta 5902, e assim por diante

Você pode configurar coisas como resolução da tela no arquivo `~/.vnc/config`, porém eu recomendo usar o padrão


No app VNC Viewr, clique no botão verde para adicionar uma conexão, e então adicione o endereço como `localhost:1`, e dê um nome para a conexão, pode ser qualuer coisa que você quiser


Depois de adicionar, conecte ao servidor, ele irá pedir a senha que você definiu para o seu vnc com `vncpasswd`, caso queira, ative a opção de lembrar senha para que não precise por toda vez

Ao conectar, provavelmente a sua imagem estará bugada e distorcida, isso é porque o VNC Viewer define a qualidade baixa como padrão, para priorizar o desempemho, você pode mudar apertando no botão `i` , que aparece junto de outras ferranenras no topo da tela

Recomendo a qualidade média

# I3WM
Ao abrir sua interface, você dará de cara com uma tela preta pedindo algumas configurações, esse é o I3WM, vocé pode só apertar enter pra configurar tudo

Para abrir um terminal, você pode usar as teclas _WIN+ENTER_, para abrir um programa, pode ser a partir de um terminal ou pelo **dmenu**, que é acessível através da combinação _WIN+d_

Recomendo que veja algum tutorial sobre I3WM, para entender como fuciona


# Conteudos recomendados

1- BASH
Para programar no celular, é interessante saber utilizar o bash, pois é o terminal que o termux utiliza

Como bom conteúdo gratuito sobre bash, recomendo o canal debxp no youtube e o livro Pequeno Manual do Programador GNU Bash
[canal debxp](https://youtube.com/c/debxplinux)
[PMPGB](https://blauaraujo.com/baixar/)


2- NEOVIM
Você certamente precisará de um editor de código, e para o celular, o neovim com certeza é o melhor

O neovim é um ediror poderoso, e eu criei um manual completo sobre ele
Nessa wiki tem uma seção com outros conteúdos sobre neovim que valem a pena ver

[manual neovim](https://github.com/outragedline/neovim-termux/wiki)
Porém esse manual é feito para o termux puro, não para o arch, mas, tudo que você precisa fazer é usar o comando abaixo para instalar o neovim desse repositório, e então seguir o tutorial


```
sudo pacman -Syu neovim nodejs-lts-gallium python3 python-pip ripgrep fd lua fzf gcc  npm tree-sitter lua-language-server

pip install wheel pynvim autopep8 flake8archlinux

git clone https://github.com/outragedline/neovim-termux ~/.config/nvim/
```


