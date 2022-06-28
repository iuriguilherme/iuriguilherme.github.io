---
layout: post
title: "Instalando Python 3.9 no Ubuntu"
date: 2022-06-27 13:46:00 -0300
category: python
tags: linux python ranzinza ubuntu
---

TL;DR: Quem não quiser ler nada e estiver procurando um monte de comandos pra 
copiar e colar no terminal pula pro [final do artigo](#tldr)  

---

## Porquê (várias páginas de reclamações ranzinzas)

Ubuntu LTS 22.04 vem com Python 3.10 instalado por padrão e não tem outra 
versão de Python pelos repositórios oficiais, então não dá pra instalar 
Python 3.9 pelo `apt install`.  

No Debian stable (apt), no MacOS (homebrew), e no Windows (MS Store), a 
última versão ou versão padrão do Python é a 3.9. Estes três sistemas 
operacionais que eu acabei de citar são os mais utilizados no mundo, e 
portanto, acabam definindo o padrão do que a gente pode presumir com 
suficiente segurança de o que é que a maioria das pessoas está usando.  

Eu sou uma pessoa que tem diversos computadores e em cada um deles eu tenho um 
ambiente totalmente diferente. Eu tenho acesso a um macbook pro com python3.9 
instalado com homebrew, alguns computadores com windows 10 e o python3.9 
instalado pela microsoft store, vários servidores debian e centos com 
python3.9 instalado pelos canais oficiais, e só na máquina com ubuntu é que 
tem o python3.10 por padrão e forçado.  

Pra eu atualizar todos os meus outros sistemas pra python3.10 eu teria que 
recorrer a métodos alternativos. Então muito mais fácil padronizar só o Ubuntu 
que é o diferentão, do que decorar que configuração eu fiz em cada sistema.  

Todo tutorial ensinando a usar Python na internet presume que a pessoa lendo 
tenha só um computador com uma configuração específica e que está tudo bem 
alterar todo o estado do sistema e contar que vai continuar do jeito que se 
encontra pelos próximos anos, e que a pessoa vai passar pelo tutorial uma 
única vez e vai viver feliz para sempre.  

Mas esta nunca foi a minha realidade. Eu regularmente "formato" as máquinas, 
reinstalo sistemas, computadores quebram, param de funcionar, são roubados, 
discos falham e precisam ser reformatados, sistemas são atualizados, eu mudo 
de solução de hosting, uso máquinas virtuais, ganho mais computadores, compro 
mais computadores, testo tecnologias novas, etc. Então instalar Python do zero 
para mim é uma coisa cotidiana. Tem vezes que pra testar um script simples eu 
preciso criar o ambiente inteiro do Python, porque o computador que eu tenho à 
disposição talvez nunca tenha sido usado pra desenvolvimento, é emprestado de 
alguém, é um smartphone com android, é um computador reciclado antigo, é uma 
VPS virgem, é um Debian antigão com Python2.7, é um Windows 11, etc.  

Então as premissas quando eu vou instalar Python é que eu provavelmente nem 
tenho acesso administrativo (root/sudo/admin), e preciso conseguir instalar 
qualquer coisa como usuário comum. Isso já torna 99% dos tutoriais da internet 
e "soluções mágicas" como o homebrew e ppas alternativos inviáveis. Na minha 
opinião, o Jeito Certo (tm) de usar qualquer software é não precisar de 
permissão do administrador do sistema. Porque o usuário pode estar atrás de um 
proxy ou firewall corporativo, em uma VPS compartilhada, em um sistema 
restrito, etc. Nem todo usuário tem **sudo**, nem todo mundo consegue usar a 
Microsoft Store, nem todo mundo tem dinheiro pra ter sempre o Macbook mais 
moderno, nem todo mundo tem smartphone com root, nem todo mundo sabe instalar 
um docker ou um ppa.  

Então o jeito efetivamente simples, fácil, rápido, descomplicado, acessível 
pra todo mundo em toda e qualquer circunstância ou sistema operacional pra 
instalar Python é baixar o código fonte e compilar. Há mais de três décadas 
que eu lido com computadores e desenvolvimento de software, e essa regra não 
mudou.  

A gente chegou na era moderna onde tudo é "fácil, simples, rápido". Os 
designers de UX realmente acreditam que é mais difícil compilar um programa 
do que fazer do jeito moderno deles, que basicamente é:  

- No Windows: Usar um mecanismo de busca e ficar procurando o programa em 
vários sites com propagandas e vírus, tentar achar o botão de Download, rezar 
pra baixar a coisa certa na versão certa, desempacotar um arquivo com winrar 
versão gratuita, executar um binário sem verificar o checksum nem ver o 
código fonte, clicar em "avançar" dezenas de vezes sem saber o que está 
acontecendo, e contar com a capacidade do instalador de criar um atalho na 
área de trabalho. Tempo estimado: de 30 minutos a duas horas, dependendo da 
experiência da(o) usuária(o);  
- No MacOS: Tentar achar um dmg ou pkg nos mesmo sites, não conseguir e copiar 
e colar os comandos do homebrew no terminal até "funcionar". Tempo estimado: 
de 15 minutos a seis horas, dependendo da experiência da(o) usuária(o);  
- No Linux: Procurar em todos fóruns qual é o nome do programa no sistema 
gerenciador de pacotes, porque no gerenciador gráfico é um nome (por exemplo,
o Nautilus é "Arquivos") e se for instalar pelo terminal é outro. Tempo 
estimado: de 3 minutos a três dias, dependendo da experiência da(o) 
usuária(o).  

Em decorrência de anos de decisões revolucionárias que se provaram ruins, 
hoje em dia, ao invés das pessoas estarem se ajudando a aprender a instalar 
software pelos foruns da internet, estão dedicando tempo tentando aprender e 
ensinando outras pessoas a usar soluções novas, complicadas, difíceis, 
reinvenções da roda, jeitos novos e revolucionários que vão deixar de ser 
novos em um curto prazo, e práticas e vícios que num futuro próximo vão 
se tornar obsoletos.  

Até aí tudo bem, porque eu faço isto e também incentivo outras pessoas a 
fazerem. Porque eu gosto de criar novos caminhos neurais, experimentar o novo 
e aprender coisas novas. No entanto, quando eu preciso que um computador 
simplesmente funcione, o Jeito Certo (tm) de operar a máquina é do mesmo jeito 
que se fazia antes de eu nascer, e é o padrão que toda a solução moderna 
realmente usa por baixo dos panos, e eu poderia resumir todo este blog em três 
linhas simples, fáceis, rápidas e claras:  

```
./configure
make
make install
```

Estas três linhas são a forma mais sofisticada de instalar programas e é o 
único jeito que sempre existiu, e provavelmente vai continuar sendo a única 
forma possível de instalar software por muitos anos. Todas as outras formas 
realmente no fim das contas estão fazendo isso que estas três linhas fazem, 
mas de uma forma obscura, complexa, complicada, inacessível e 
desnecessariamente menos fácil, rápida e simples.  

---

## Alternativas

Existem mil maneiras de instalar Python e quase todas elas são ruins.  

As alternativas são usar [o ppa dos deadsnakes][deadsnakes], docker, homebrew, 
pyenv.  

Este blog é sobre usar o Jeito Certo (tm), mas eu vou mencionar algumas 
alternativas que ue já usei várias vezes e não gosto.  

Top 3 piores maneiras de instalar Python na minha opinião:  

### [Homebrew][homebrew]

Uma das razões pela qual eu uso muito mais Linux e Windows no PC é porque 
homebrew me dá ânsia de vômito. Brew só funciona se tu tiver um único usuário 
no mac, e precisa ser administrador do sistema. Muito mais fácil, simples, 
rápido, eficiente, eficaz, bonito, etc. é compilar tudo manualmente e criar 
links simbólicos para os sofwares. Porque é exatamente isso que o brew faz, 
com a diferença que ele interfere em todo sistema operacional sem pedir 
licença. Ou seja, brew só complica, dificulta, enfeia.  

### [Docker][docker]

A única razão pra usar docker é quando a aplicação precisa ser instalada no 
ambiente de produção usando docker, ou seja, nos ambientes de desenvolvimento, 
teste e homologação, vai ser necessário usar docker também. Docker não deve 
ser usado pra nada além de... usar o Docker.  

### [Pyenv][pyenv]

A propaganda do pyenv começa com "simples".  
Eu tenho um preconceito com tudo o que inclui os termos "simples", "fácil", 
"rápido", ou inclui um coração e um ponto de exclamação na documentação. 
Não tem como funcionar um software desses.  
E no caso do pyenv, o que ele faz é compilar a versão de python que tu tenta 
instalar, e tornar complexa a sintaxe de qualquer coisa que tu for fazer.  
De duas uma: ou tu reconfigura todo o teu ambiente pra usar os shims do pyenv, 
e eles documentam bem isso sem admitir que é o exato oposto de "simples"; ou 
então toda vez que tu for usar um comando tu tem que fazer por exemplo 
`pyenv exec python -m pip`, ao invés de por exemplo `pip`.  
O fluxo habitual de compilar a versão específica de python e fazer os links 
simbólicos pra expor o interpretador no ambiente, na minha opinião, é mais 
rápido, fácil e simples do que usar pyenv, e portanto, comparando com replicar 
a funcionalidade do pyenv sem o pyenv, então pyenv é complexo, difícil, 
demorado e complicado.  

### Deadsnakes (uma alternativa que não é ruim)

Usar o [ppa do deadsnakes][deadsnakes] não é um jeito ruim de instalar Python, 
e se eu fosse recomendar pra alguém que tá começando a usar Ubuntu eu diria 
que esse é o melhor jeito, mas o Jeito Certo (tm) é o infalível e o garantido, 
então eu vou focar no Jeito Certo (tm).  

---

## Jeito Certo (tm)

O jeito rápido, fácil, simples, direto ao ponto, sem necessidade de aprender 
tecnologias novas e que dá pra fazer com o menor número possível de cliques, 
comandos, teclas, é:  

### Primeiro passo

O primeiro passo é o mais difícil de todos (com exceção do segundo). Requer 
um navegador web moderno com suporte a SSL e javascript, como por exemplo, 
mas não limitado a [Surf][surf], [Chrome][chrome], [Surf][surf], 
[Firefox][firefox], [Surf][surf], [Opera][opera], [Surf][surf], 
[Safari][safari], [Surf][surf], etc.  

A parte mais fácil é conseguir fazer o navegador abrir o documento HTML 
servido em [https://www.python.org/downloads/source/][source]. Dica: com um 
mouse, clique no hyperlink deste parágrafo.  

### Segundo passo

O próximo passo é encontrar a versão do Python que a gente quer instalar. No 
caso deste blog é o Python3.9, mas talvez na hora que tu estiver lendo este 
blog, tu está realmente tentando instalar alguma versão específica do Python 
4, e nem exista Ubuntu mais. O interessante deste guia é que provavelmente vai 
continuar funcionando por muito mais tempo do que todas as outras 
alternativas, e eu previ isto em 2022.  

No dia em que este blog foi escrito, a última release do Python3.9 é a 3.9.13 
e a URL é <https://www.python.org/downloads/release/python-3913/>. Mas com 
certeza isto vai mudar a cada vez que tu estiver lendo este blog, e portanto, 
esta é a parte mais difícil de todas. Todas as últimas versões devem ou 
deveriam estar listadas na [página de download do Python][source], mas pode 
ser que mudem a URL como já aconteceu mais de uma vez no passado.  

### Terceiro passo

O interessante seria ler todas as notas da release, mas como obviamente 
ninguém nunca faz isto, os arquivos estão no final da página, na seção 
"Files".  

A gente quer o código fonte então é algum dos arquivos etiquetados como 
"source release". No dia em que este blog foi escrito e no caso do 
Python3.9.13, a URL é 
<https://www.python.org/ftp/python/3.9.13/Python-3.9.13.tar.xz>.  

Neste mesmo local também é possível encontrar o instalador msi pra Windows e o 
instalador pkg pra MacOS. Não tem os pacotes deb, rpm, etc. pras distribuições 
Linux porque cada distribuição tem o seu próprio sistema de pacotes. No caso 
do Ubuntu é o apt mas eu já expliquei no começo do blog que não funciona 
porque não tem o Python3.9 no repositório oficial da Canonical. E por esta 
razão precisamos recorrer ao método tradicional e infalível que funciona em 
todo e qualquer sistema que é o Jeito Certo (tm).  

### Quarto passo

Eu disse que pelo Jeito Certo (tm) a gente não precisa ser administrador do 
sistema pra conseguir instalar software. Eu menti. Não tem como.  

Tá vendo porque é que eu disse que todo tutorial de internet é ruim? A 
verdade inconveniente e chata é que pra usar computadores e instalar programas 
a gente tem que estudar informática, ou no mínimo estudar o funcionamento das 
ferramentas de alto nível que a gente usa. A alternativa para isto é estudar 
informática, ou no mínimo estudar o funcionamento das ferramentas de alto 
nível que a gente usa. Mas quem não gosta disto, pode estudar informática, ou 
no mínimo estudar o funcionamento das ferramentas de alto nível que a gente 
usa. Se tu não fizer nenhuma destas coisas, tu vai acabar, pra conseguir usar 
o computador, tendo que estudar informática, ou no mínimo estudar o 
funcionamento das ferramentas de alto nível que a gente usa.  

Esquece os dois últimos parágrafos, é tudo mentira. Na verdade **tem como**. 
No Ubuntu por padrão tem o **gcc** e a gente consegue compilar qualquer coisa. 
Mas aí neste caso tu vai ter que compilar TODAS as dependências do Python. 
Especialmente o OpenSSL. E não pára por aí, porque antes de compilar cada dependência do Python, a gente vai ter que compilar as dependências de cada 
dependência. Foi isso que eu tive que fazer pra conseguir instalar python 
estritamente como usuário, sem acesso administrativo em um dos servidores que 
eu uso. Isso é basicamente o dia a dia de um usuário do Gentoo há 20 anos 
atrás. Se eu fizer o passo a passo deste procedimento todo, este blog vai ter 
cerca de cinquenta mil linhas ao invés de quinhentas.  

Então este passo usando **sudo** é um atalho benevolente pra economizar o teu 
tempo e te manter fora da diversão de verdade. A diversão de verdade não é 
programar, é passar a semana inteira na tela preta do terminal pesquisando 
flag de compilador e ouvindo o barulho do processador fritando. Change my 
mind.  

Pra compilar o Python com as dependências satisfeitas a gente precisa de 
bibliotecas que os mantenedores das distribuições deliberadamente não instalam 
por padrão. No caso do Ubuntu LTS 22.04:  

```bash
iuri@tartaruga:~$ sudo apt install build-essential libbz2-dev libdb5.3-dev \
libexpat1-dev libffi-dev libgdbm-compat-dev libgdbm-dev liblzma-dev \
libncurses5-dev libncursesw5-dev libnss3-dev libreadline-dev libsqlite3-dev \
libssl-dev tk-dev uuid-dev wget zlib1g-dev
```

Estes são os pacotes que a gente precisa que o sistema tenha, e que 
provavelmente não tem, porque quase nenhum vêm instalado por padrão no Ubuntu 
22.04. Pros 99% de usuários que vão usar o Ubuntu pra jogar sudoku no gnome, 
tá mais do que bom. Mas pros 1% que vão usar o sistema pra escrever python, 
como eu por exemplo, tá ruim. E o fato do python3.10 estar instalado por 
padrão não me ajuda, porque eu tô nos 0.01% (um por cento dos um por cento) 
que precisa de outra versão.  

Dá pra compilar o Python só com o pacote **build-essential**. Mas o pacote 
mais importante de todos ali é o **libssl-dev**. Sem ele, o Python vai ser 
compilado sem suporte pra SSL, e notavelmente o **pip** não vai funcionar 
porque tudo depende de fazer requisições HTTP GET para servidores com SSL. 
E portanto, o Python instalado vai ser praticamente inutilizável, a não ser 
que tu consiga fazer tudo só com as bibliotecas padrão e nunca precise usar 
nenhum comando que dependa de ssl. O que é 100% de chance de não ser o teu 
caso.  

### Quinto passo

Não precisava ter instalado o wget no passo anterior, mas agora que tá 
instalado, a gente tem uma ferramenta pra fazer download muito mais fácil de 
usar do que o Google Chrome:  

```bash
iuri@tartaruga:~$ wget -c \
https://www.python.org/ftp/python/3.9.13/Python-3.9.13.tar.xz  
```

O **-c** do wget serve pra continuar o download (ao invés de recomeçar do 
zero) no caso dele ser interrompido porque a conexão à internet deu algum 
problema (o que é a regra no Brasil).  

O **tar** vem instalado no Ubuntu por padrão, então pra descompactar o arquivo 
é só rodar o seguinte comando:  

```bash
iuri@tartaruga:~$ tar -xf Python-3.9.13.tar.xz  
```

Obviamente também dá pra usar o file-roller com o nautilus pela interface 
gráfica do Ubuntu, eu não uso porque o meu Ubuntu na verdade é o WSL2 do 
Windows e pra usar elementos gráficos tem que rodar um servidor X no Windows. 
O que é assunto pra outro blog.  

PS: Perceberam que a gente teve que baixar o Python e descompactar o arquivo, 
como é o caso de todos os outros tutoriais que eu falei que são ruins né? Um 
dia quem sabe tu aprenda que todo mundo tá errado sobre tudo o tempo todo, 
e que é hipócrita quem disser que tá certo em alguma coisa, mas enquanto isto, 
tu pode continuar escolhendo qual é a mentira que dói menos. Eu não vou nem 
pedir desculpas porque aí eu tô admitindo que eu prefiro ser bonzinho, o que 
não é o caso.  

## Sexto passo

Agora sim a compilação propriamente dita. Se a gente não especificar o 
prefixo de instalação, o **make** vai tentar usar um diretório do sistema que 
precisa de acesso administrativo. Então a gente usa o diretório do usuário 
atual (que no bash do Ubuntu fica no atalho `${HOME}` ou `~`):  


```bash
iuri@tartaruga:~$ cd Python-3.9.13/
iuri@tartaruga:~/Python-3.9.13$ ./configure --prefix=${HOME}/.local  
```

Depois deste comando, a tela vai encher de mensagens de depuração que é todos 
os testes pra ver se vai dar certo a compilação. Como eu já disse antes, se o 
script do **configure** não conseguir achar o SSL do sistema, pode abrir aí o 
<https://duck.com/> e aprender a compilar o openssl e usar a opção 
**--with-ssl**, senão o teu futuro é procurando no stack overflow como é que 
se resolve o problema do pip não estar funcionando. E não tem nada a ver com o 
coitado do pip.  

Note que porque este comando configura o prefixo como o diretório padrão do 
usuário, na hora de instalar, o instalador do sistema vai criar os diretórios 
bin, include, lib e share, por exemplo **/home/iuri/.local/bin**. Eu sugeri 
este diretório porque é o diretório padrão por exemplo do `pip install --user` 
e de outras ferramentas a nível de usuário. Se tu não quiser isto, pode usar 
outro prefixo como por exemplo **~/usr**.  

Não é estritamente necessário, mas eu recomendo usar este comando ao invés do 
anterior:  

```bash
./configure --prefix=${HOME}/.local --enable-optimizations  
```

Este comando vai usar as otimizações do compilador, porque o que a gente está 
compilando é o CPython.  

Se tudo der certo (quase nunca dá), o próximo comando é:  

```bash
iuri@tartaruga:~/Python-3.9.13$ make  
```

Neste comando a gente sabe se a configuração estava certa e se o Python vai 
conseguir compilar no sistema. Vai dar erro neste comando principalmente se 
vossa excelência não tiver **sudo** e portanto não instalou os pacotes de 
desenvolvimento do quarto passo.  

```bash
iuri@tartaruga:~/Python-3.9.13$ make test  
```

Este comando além de servir pra gente se sentir hacker (porque só quem tem um 
terminal com "load average" escrito várias vezes na tela consegue de fato 
fazer de conta que entende de computação), vai testar um monte de coisas que 
precisam estar funcionando pro interpretador funcionar da forma esperada 
depois de instalado. Se der erros nesta fase é porque alguma coisa deu errado 
nas etapas anteriores e a gente ignorou.  

```bash
iuri@tartaruga:~/Python-3.9.13$ make altinstall  
```

O importante de usar `make altinstall` ao invés de `make install` é porque o 
primeiro vai fazer um link estático do interpretador com **python3.9** ao 
invés de **python3**. Isto é importante porque fundamentalmente o escopo deste 
blog é instalar uma versão diferente de Python.  

## Sétimo passo

Parabéns, conseguimos instalar o páitom três ponto nove no ubuntu. Pode botar 
no currículo aí.  

Agora pra ter certeza que deu certo, o próximo comando é:  

```bash
iuri@tartaruga:~$ whereis python3.9  
python3.9: /home/iuri/.local/bin/python3.9  
```

Se ao invés de um diretório o comando não mostrar nada (só o python3.9:), 
é porque no nosso ambiente o diretório **~/.local/bin** não está na variável 
**PATH** do ambiente. Isto é necessário em qualquer sistema operacional, não 
só no Ubuntu. Felizmente, editar variáveis de ambiente é muito mais fácil no 
Ubuntu do que no MacOS e no Windows.  

No arquivo **~/.profile** deve ou deveria ter alguma coisa assim:  

```bash
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
```

O jeito mais óbvio de resolver o problema anterior é copiando e colando esse 
trecho, adaptando, o arquivo **~/.profile** vai ficar assim: 

```bash
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$HOME/.local/bin:$PATH"
fi
```

E aí ou a gente reinicia o sistema ou faz um `source ~/.profile` pra fazer a 
mudança valer.  

Uma alternativa é na hora de configurar usar o `./configure --prefix=${HOME}` 
ao invés de `./configure --prefix=${HOME}/.local`, porque o diretório 
**~/bin** já está ou já deveria estar no **PATH**, e aí o Python instalado já 
deve aparecer automagicamente.  

Agora este comando vai dizer a versão específica do Python que a gente 
instalou:  

```bash
iuri@tartaruga:~$ python3.9 --version  
Python 3.9.13  
```

Os dois jeitos de garantir que a gente tá usando o pip com o python3.9 e não o 
pip do sistema (que neste caso é o do python3.10):  

```bash
iuri@tartaruga:~$ pip3.9 install --user --upgrade pip  
iuri@tartaruga:~$ python3.9 -m pip install --user --upgrade pip  
```

Não se esqueçam de clicar no sininho e se inscrever no canal.  

---

## Bibliografia

- <https://www.python.org/downloads/release/python-3913/>  
- <https://docs.python.org/3.9/using/unix.html>  
- <https://docs.python.org/3.9/using/configure.html>  
- <https://github.com/python/cpython/blob/3.9/README.rst>  

---

## TL;DR:

Pra quem não vai ler o blog e só quer copiar e colar no terminal os comandos:  

```bash
## Alterar essa URL pra usar outra versão conforme a necessidade
PYTHON_URL="https://www.python.org/ftp/python/3.9.13/Python-3.9.13.tar.xz"
echo -ne '\nif [ -d "$HOME/.local/bin" ] ; then\n    PATH="$HOME/.local/bin:$PATH"\nfi\n' 1>> ~/.profile
source ~/.profile
sudo apt install build-essential libbz2-dev libdb5.3-dev libexpat1-dev libffi-dev libgdbm-compat-dev libgdbm-dev liblzma-dev libncurses5-dev libncursesw5-dev libnss3-dev libreadline-dev libsqlite3-dev libssl-dev tk-dev uuid-dev wget zlib1g-dev
wget -c "${PYTHON_URL}" -O python.tar.xz
tar -xf python.tar.xz
cd python
./configure --prefix=~/.local --enable-optimizations
make
make test
make altinstall
pip3.9 install --user --upgrade pip
whereis python3.9
echo "Deu Certo!"
```

[chrome]: https://www.google.com/intl/pt-BR/chrome/
[deadsnakes]: https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa
[docker]: https://hub.docker.com/_/python
[firefox]: https://www.mozilla.org/pt-BR/firefox/new/
[homebrew]: https://brew.sh/index_pt-br
[opera]: https://www.opera.com/pt-br
[pyenv]: https://github.com/pyenv/pyenv
[safari]: https://www.apple.com/br/safari/
[source]: https://www.python.org/downloads/source/
[surf]: https://surf.suckless.org/
