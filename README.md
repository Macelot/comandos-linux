# Comandos Essenciais do Linux Debian

## Listagem e Identificação de Shells

### Como listar os shells disponíveis no sistema?
```bash
cat /etc/shells
```

### Como descobrir qual shell estou usando?
```bash
echo $SHELL
ps -p $$
```

## Gerenciamento de SSH

### Iniciar SSH
```bash
sudo service ssh start
```

### Exemplo de login via SSH
```bash
ssh aluno1@191.252.109.103
```

## Verificação de IP
```bash
curl ifconfig.me
```

## Gerenciamento de Usuários

### Listar usuários do sistema
```bash
cat /etc/passwd
```

### Listar apenas os nomes dos usuários
```bash
groups
groups aluno1
cut -d: -f1 /etc/passwd
```

### Verificar os usuários que logaram
```bash
last
last --time-format full
last --time-format full | tac
```

### Listar grupos do sistema
```bash
cut -d: -f1 /etc/group
```

### Exibir todos os grupos aos quais cada usuário pertence
```bash
for user in $(cut -d: -f1 /etc/passwd); do
    echo -n "$user: "
    groups $user
done
```

### Mostrar todos os usuários e seus grupos associados
```bash
awk -F':' '{print $1 ": " $4}' /etc/passwd | while IFS=":" read -r user group_id; do
    group_name=$(getent group "$group_id" | cut -d: -f1)
    echo "$user: $group_name"
done
```

## Comandos Essenciais

### 1. `exit` - Sair do terminal ou encerrar sessão
```bash
exit
```

### 2. `pwd` - Exibir diretório de trabalho atual
```bash
pwd
```
Saída:
```
/home/usuario/Documentos
```

### 3. `who` - Listar usuários logados no sistema
```bash
who
```

### 4. `ls` - Listar arquivos e diretórios
```bash
ls
```

### 5. `cd` - Mudar de diretório
```bash
cd pasta1
```

### 6. `cd ..` - Voltar um diretório acima
```bash
cd ..
```

### 7. `echo` - Exibir mensagens ou valores de variáveis
```bash
echo "Olá, Mundo!"
```
Saída:
```
Olá, Mundo!
```

### 8. `cat` - Exibir conteúdo de arquivos
```bash
cat arquivo1.txt
```

### 9. `nano` - Editar arquivos de texto
```bash
nano arquivo1.txt
```

### 10. `touch` - Criar arquivos vazios
```bash
touch arquivo_novo.txt
```

### 11. `chmod` - Alterar permissões de arquivos
```bash
chmod 755 arquivo1.txt
```

### 12. `mkdir` - Criar diretórios
```bash
mkdir novo_diretorio
```

## Comandos Adicionais

### Exibir processos em execução
```bash
ps aux
```

### Monitorar processos em tempo real
```bash
top
```

### Verificar uso de espaço em disco
```bash
df -h
```

### Verificar uso de memória
```bash
free -h
```

### Procurar arquivos no sistema
```bash
find /home -name "arquivo.txt"
```

### Buscar por um termo dentro de arquivos
```bash
grep "termo" arquivo.txt
```

### Encerrar um processo pelo PID
```bash
kill 1234
```

### Forçar encerramento de um processo
```bash
kill -9 1234
```

### Reiniciar o sistema
```bash
sudo reboot
ou
systemctl reboot
```
OBS. Alguns comandos não são possíveis de executar na VPS com os usuários dos alunos, pois estes usuários não fazem parte do grupo root.
OBS2. Este comando precisa de permissão, ou seja, precisa pertencer ao grupo root:
```bash
ls -l /sbin/shutdown /sbin/reboot
```
```
lrwxrwxrwx 1 root root 14 Aug 25  2024 /sbin/reboot -> /bin/systemctl
lrwxrwxrwx 1 root root 14 Aug 25  2024 /sbin/shutdown -> /bin/systemctl
```

### Desligar o sistema
```bash
sudo shutdown -h now
```

 ###  Explicação do comando `cut`

O comando `cut` é utilizado para cortar e exibir partes de linhas de arquivos de texto, com base em delimitadores especificados.

### Exemplo de uso do comando `cut`

O seguinte comando corta e exibe o primeiro campo (campo 1) de cada linha do arquivo `/etc/passwd`, utilizando `:` como delimitador.

```bash
cut -d: -f1 /etc/passwd
```

- `-d:`: Define o delimitador como `:`.
- `-f1`: Exibe o primeiro campo de cada linha (geralmente o nome de usuário no arquivo `/etc/passwd`).

### Explicação do script para exibir todos os grupos aos quais cada usuário pertence

Este script usa o comando `cut` para listar todos os usuários do sistema e, em seguida, exibe todos os grupos aos quais cada usuário pertence:

```bash
for user in $(cut -d: -f1 /etc/passwd); do
    echo -n "$user: "
    groups $user
done
```

- `cut -d: -f1 /etc/passwd`: Obtém o nome de usuário de cada linha no arquivo `/etc/passwd`.
- `groups $user`: Exibe os grupos aos quais o usuário pertence.
- `echo -n "$user: "`: Exibe o nome do usuário seguido dos grupos aos quais ele pertence.

---

## 2. Permissões de Arquivo com `chmod`

O comando `chmod` é usado para alterar as permissões de arquivos e diretórios no Linux.

### Permissões Básicas

As permissões de arquivos no Linux são divididas em três categorias:
- **Usuário (Owner)**: O dono do arquivo.
- **Grupo**: O grupo ao qual o arquivo pertence.
- **Outros**: Qualquer outro usuário.

As permissões para cada categoria podem ser:
- **r** (read): Permissão de leitura.
- **w** (write): Permissão de escrita.
- **x** (execute): Permissão de execução.

### Tabela de Permissões e Valores Numéricos

| Permissão | Valor numérico | Descrição        |
|-----------|-----------------|------------------|
| ---       | 0               | Nenhuma permissão |
| --x       | 1               | Permissão de execução |
| -w-       | 2               | Permissão de escrita |
| -wx       | 3               | Permissão de escrita e execução |
| r--       | 4               | Permissão de leitura |
| r-x       | 5               | Permissão de leitura e execução |
| rw-       | 6               | Permissão de leitura e escrita |
| rwx       | 7               | Permissão de leitura, escrita e execução |

### Usando `chmod` com Números

O comando `chmod` pode ser utilizado com valores numéricos para definir as permissões. Por exemplo, para dar permissão total (leitura, escrita e execução) para o dono, e apenas leitura e execução para o grupo e outros, usamos:

```bash
chmod 755 arquivo.txt
```

- **7** (rwx): Para o usuário (dono).
- **5** (r-x): Para o grupo.
- **5** (r-x): Para outros.

### Usando `chmod` com Letras

O comando `chmod` também pode ser utilizado com letras para adicionar (+), remover (-) ou definir (=) permissões. Por exemplo:

#### Adicionando Permissão de Execução

```bash
chmod +x arquivo.txt
```
Este comando adiciona permissão de execução para o dono, grupo e outros.

#### Removendo Permissão de Escrita

```bash
chmod -w arquivo.txt
```
Este comando remove a permissão de escrita de dono, grupo e outros.

#### Definindo Permissões Específicas

```bash
chmod u+x arquivo.txt
chmod g-w arquivo.txt
chmod o=r arquivo.txt
```

- `u+x`: Adiciona permissão de execução para o usuário (dono).
- `g-w`: Remove permissão de escrita para o grupo.
- `o=r`: Define permissão de leitura para outros, removendo todas as outras permissões.


####  Alterar dono do arquivo:
```bash
chown -R alunos1:users /var/www/html/aluno1
```

#### Consultar versão do SO Debian

```bash
cat /etc/os-release
```

---

## Conclusão

O `chmod` é uma ferramenta poderosa para gerenciar permissões de arquivos e diretórios no Linux. Usando valores numéricos ou letras, você pode controlar quem pode ler, escrever ou executar seus arquivos. A tabela de permissões e exemplos demonstrados aqui são fundamentais para uma boa administração do sistema Linux.




## Instalar o SSH
```bash
su root
apt-get install openssh-server -y
```

## Verificar o status do SSH
```bash
systemctl status ssh
```

## reinicair o SSH
```bash
systemctl restart ssh
```

## Testar ssh local
```bash
ssh giovane@localhost 
```

## Depois de testado o SSH na VM, podemos configurar o redirecionamento de portas no Oracle VirtualBox
no cmd do windows ou terminal do hospedeiro 
```bash
ssh giovane@127.0.0.1 -p 22333 
```

### Comando para verificar a data de criação da VM:
```bash
stat / | grep Birth
```

### Criação de Banner para SSH
O arquivo onde o banner é definido é /etc/ssh/sshd_config.

## 1️⃣ Editar o arquivo de configuração do SSH
```bash
sudo nano /etc/ssh/sshd_config
```

# Procure pela linha:
```bash
#Banner none
```
# Altere para:
```bash
Banner /etc/issue.net
```
# Salve (CTRL + X, depois Y e ENTER).


## 2️⃣ Criar o arquivo de banner com ASCII Art
Agora, edite o arquivo /etc/issue.net com um ASCII Art 
```bash
sudo nano /etc/issue.net
```
Adicione sua arte, por exemplo, uma nuvem:
```
        .--.      
     .-(    ).   
    (___.__)__)  
     ‘ ‘ ‘ ‘ ‘  
     ‘ ‘ ‘ ‘ ‘  
```

## 3️⃣ Reiniciar o SSH
Agora, edite o arquivo /etc/issue.net com um ASCII Art 
```bash
sudo nano /etc/issue.net
```

# agora saia e entre no SSH para testar
```bash
exit
```

## Links úteis:
* Passo a passo para redirecionamento de portas [Passo a Passo VM] (https://github.com/Macelot/passo-a-passo-vm)
* Gerenciamento Linux Debian [Gerenciamento Linux] (https://github.com/Macelot/pratica-gerenciamento-linux)
* Conceitos básico do Docker [Conceitos Docker] (https://github.com/Macelot/conceitos-docker)
* Manual do Docker [Manual Docker] (https://github.com/Macelot/manual-docker)
* Recuperando a senha do root [Root sem senha] (https://github.com/Macelot/root-sem-senha)
