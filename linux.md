Linux
=====

Um conjunto de dicas para referência pessoal de comando para distribuições Linux baseadas em Debian / Ubuntu.


### AGENDAR UM PROGRAMA OU COMANDO

Para agendar várias tarefas basta abrir no terminal o crontab:

  crontab -e     (para apenas listar os comandos já agendados basta usar crontab -l)

E adicionar uma instrução por linha com o seguinte formato: 

```<minuto> <hora> <dia do mês> <mês> <dia da semana> <instrução>```
ex: 

  10 20 * 1 7 echo "aviso importante!" (lembra o utilizador do terminal às 20h10 de todos os domingos do primeiro mês )

Importante:

Para usar uma aplicação no ambiente gráfico será necessário meter no início da instrução export DISPLAY=:0 && . No entanto tal não é necessário se usarmos o fcrontab em vez do crontab. No entanto o fcron não é compatível com alguns programas, tal com o Gnome Scheduler (uma GUI do crontab).


Exemplos:

Ouvir um programa de rádio às 16h30 de domingo e desligá-lo 30m depois:
  30 16 * * 7 export DISPLAY=:0 && vlc mms://rdp.oninet.pt/antena2
  0 17 * * 7 killall vlc

Mostrar um aviso a uma certa hora (meio-dia, 15h):
  0 12 * * * export DISPLAY=:0 && zenity --info --text="Angelus Domini nuntiavit Mariae" --title="Oração"
  0 15 * * * export DISPLAY=:0 && zenity --info --text="Ó Sangue e Água que brotaste do Coração de Jesus como fonte de Misericórdia insondável para nós, eu confio em Vós." --title="Oração"

Ver mais em:

* http://arco-debian.codigolivre.org.br/tutorial/crontab.html (manual português)
* http://ubuntuforums.org/showthread.php?p=1077195      (usar programas com gui)


### CORRER UMA APLICAÇÃO COM PRIVILÉGIOS DE ADMINISTRADOR

no terminal: 

  sudo <comando>

no modo gráfico: (por exemplo após pressionar ALT-F2)
  
  gksudo <comando>


### CRIAR CAIXAS DE DIÁLOGO ADHOC

Mostrar o output de outro programa:

  ls ~/ | zenity --text-info (mostra conteúdo da pasta home)
  ps -U root -u root -N | zenity --text-info (mostra processos do utilizador)


Ver mais em: http://linux.byexamples.com/archives/259/a-complete-zenity-dialog-examples-1/


### DESLIGAR O COMPUTADOR AUTOMATICAMENTE

Desligar daqui a 10 minutos:

  gksudo shutdown -h +10

Agendar um shutdown automático à meia noite todos os dias:

preimeiro permitir a que o utilizador tenha permissão para usar o comando shutdown (sem sudo):
  
  sudo chmod u+s /sbin/shutdown

depois Editar crontab e inserir no editor: (crontab -e no terminal)

  0 23 * * * export DISPLAY=:0 && zenity --info --text="Já está a ficar tarde..." --title="Aviso"
  50 23 * * * export DISPLAY=:0 && zenity --info --text="O computador irá desligar-se dentro de 10 minutos!" --title="Aviso"
  59 23 * * * export DISPLAY=:0 && zenity --info --text="O computador irá desligar-se dentro de 1 minuto!" --title="URGENTE!"
  0 0 * * * shutdown -h now

ver mais em: http://linux.about.com/od/commands/l/blcmdl8_shutdow.htm


### FECHAR UM PROGRAMA

Existem várias hipóteses:

1. kill <ID do processo>,   em que para saber o ID basta fazer pgrep <nome do processo>

2. pkill <nome do processo>

3. kill all <nome do programa>

Exemplo:

>> killall vlc - fecha todas as instâncias do leitor de audio vlc que estejam a correr 



### FORÇAR O 'MOUNT' DE UM DISCO NTFS  

criar uma directoria <dir>

  mkdir /media/<dir>

montar a particao <label> na pasta <dir>

  mount -t ntfs-3g /dev/<label> /media/<dir>

em que <label> pode ser: sda1, sda2, sdb1, sdb2, etc.  Em caso de dúvida podem-se descobrir as labels das várias unidades através do seguinte comando:

  ls /dev/disk/by-label -lah

NOTA - Pode haver perigo para o disco




### INSERIR UMA NOVA FONTE AOS REPOSITÓRIOS

basta editar o ficheiro através do comando:
sudo gedit /etc/apt/sources.list


Instalação de Programas
-----------------------

### INSTALAR LAMP NO UBUNTU 10.04

* http://tuxtweaks.com/2010/04/installing-lamp-on-ubuntu-10-04-lucid-lynx/


### INSTALAR .DEB
Se por qualquer razão o gesdotor de intalação automática de ficheiros .deb não estiver a funcionar, a maneira dos instalar manualmente no terminal é a seguinte:

  sudo dpkg -i <nome_ficheiro>.deb 



### MUDAR O TECLADO
No caso de se usar um live-cd com outro layout de teclado por defeito - normalmente o americano - uma maneira simples de alterar o teclado para o caso português basta escrever o comando seguinte: 

  setxkbmap pt


### MOSTRA PROCESSOS A CORRER

* http://www.cyberciti.biz/faq/show-all-running-processes-in-linux/


### PROCURAR UM FICHEIRO POR NOME

por exemplo procurar todas as versões de um trabalho que foi dado o nome de "trabalho v1.odt", "trabalho v2.3b.doc" na pasta /media/windows ... etc 

  find /media/windows/ -name "trabalho*"


TROCAR SISTEMA OPERATIVO A CORRER POR DEFAULT EM DUAL BOOT COM GRUB

ir a /boot/grub e editar ficheiro grub.cfg

Algures no início do ficheiro haverá uma instrução 'set default="0"' ou outro número qualquer, número este que corresponde à entrada do menu de escolha do Sistema Operativo. 

Para saber qual é o número pretendido, basta ver no final do ficheiro e contar o número de "menu entry" até ao prentedido, começando por zero.

---

NOTA: Uma maneira simples de dar a volta às permissões de edições do ficheiro é abrir o explorador de ficherios com permissões de administrador:

ALT-F2: gksudo nautilus

ir a root (sistemas de ficheiros)

abrir /boot/grub

copiar ficheiro grub.cfg para o desktop

com o botão direito ir às propriedades/permissões e permitir a leitura e escrita ao utilizador

de seguida arrastar o ficheiro alterado para a janela no nautilus da pasta /boot/grub e substituir

- - -

[voltar ao início](index.md)
