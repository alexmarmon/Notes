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
// if ubuntu doesnt allow ssh as root do:
sudo cp /home/ubuntu/.ssh/authorized_keys /root/.ssh/

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
vim .ssh/authorized_keys # paste public key
chmod 775 .ssh/
chmod 664 .ssh/authorized_keys

vim /etc/ssh/sshd_config
  PermitRootLogin no
  PasswordAuthentication no
```

#### - install vim, node, mariadb
```sh
wget -O .vimrc https://raw.githubusercontent.com/FrankPetrilli/PersonalProjects/master/other/vim/vimrc

sudo apt-get update        # Fetches the list of available updates
sudo apt-get upgrade       # Strictly upgrades the current packages
sudo apt-get dist-upgrade  # Installs updates (new ones)

curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

sudo apt-get install mariadb-server
```

#### - configure mariadb user
```
CREATE USER [username] IDENTIFIED BY '[password]';
GRANT ALL ON *.* TO '[username]'@'%';                 # never grant all for application users
UPDATE mysql.user SET plugin = 'unix_socket';         # dont do for application users
FLUSH PRIVILIGES;
```

#### - forward port 80 
```
iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port [newport]
```

#### - configure letsencrypt cert
```
./certbot-auto certonly
vim /etc/haproxy/haproxy.cfg 
  frontend ssl
    bind *:443 ssl crt /etc/haproxy/all.pem ssl crt /etc/haproxy/alexmarmon-all.pem ssl crt /etc/haproxy/{domainName-all}.pem
sudo su -c "cat /etc/letsencrypt/live/{domainName}/fullchain.pem /etc/letsencrypt/live/{domainName}/privkey.pem > /etc/haproxy/{domainName-all}.pem" 
sudo service haproxy reload
```


#### - configure haproxy
```
sudo apt-get install haproxy
sudo vim /etc/haproxy/haproxy.cf
  copy and paste haproxy_starter
sudo service haproxy start
```

#### - pm2
```
sudo npm install pm2 -g
pm2 start [filename] --name="[applicationname]"
```

#### - MySQL Dump & Create
```
mysqldump -u [user] -p [database] > [name].sql
//create newDatabase in mysql
mysql -u [user] -p [newDatabase] < /Path/to/[name].sql
```

#### - Transfer file over ssh
```
scp user@server:/path/to/file /path/to/dest

readlink -f filename.sql // gets full path to file
```

#### - Fix git editor error
```
git config --global core.editor $(which vim)
```

#### - Fire up mitmproxy
```
add ~/.mitmproxy-ca-cert.pem? to keychain access and trust all
sudo pfctl -e
sudo -u nobody mitmweb --mode transparent --showhost
```

