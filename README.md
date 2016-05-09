# Notes

## Create mysql table and add sample data
```sh
create table customers (id int auto_increment primary key, first_name varchar(20), last_name varchar(20), model_ordered int, email varchar(255), phone varchar(25));
```
```sh
insert into customers (first_name, last_name, model_ordered, email, phone) values ('Chandler', 'Bing', 1, 'name@email.com', '1112223333'), ('Monica', 'Geller', 2, 'name@email.com', '4445556666');
```

## setting up linux devbox

#### - on Vultr
```sh
cat .ssh/id_rsa.pub
vim .ssh/config
  Host [name]
    Hostname [vultr ip]
    User [username]
ssh root@[vultr ip]
```
#### - create non root user, add user to sudoers group
```sh
adduser [username]
sudo adduser alexmarmon sudo
```

#### - no password sudo

```sh
export VISUAL=vim
visudo
  # Allow members of group sudo to execute any command
  %sudo   ALL=(ALL:ALL) NOPASSWD:ALL
```

#### - add ssh private key and disable password auth
```sh
mkdir .ssh
vim .ssh/authorized_keys -- paste public key
chmod 775 .ssh/
chmod 664 .ssh/authorized_keys

vim /etc/ssh/sshd_config
  PermitRootLogin no
  PasswordAuthentication no
```

#### - install vim, node, mariadb
```sh
wget -O .vimrc https://raw.githubusercontent.com/FrankPetrilli/PersonalProjects/master/other/vim/vimrc

apt-get update -y

curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

sudo apt-get install mariadb-server
```

#### - configure mariadb user
```
CREATE USER [username] IDENTIFIED BY '[password]';
GRANT ALL ON *.* TO '[username]'@'%'; // never grant all for application users
UPDATE mysql.user SET plugin = 'unix_socket'; // dont do for application users
FLUSH PRIVILIGES;
```




