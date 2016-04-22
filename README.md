# Notes

## Create mysql table and add sample data
```sh
create table customers (id int auto_increment primary key, first_name varchar(20), last_name varchar(20), model_ordered int, email varchar(255), phone varchar(25));
```
```sh
insert into customers (first_name, last_name, model_ordered, email, phone) values ('Chandler', 'Bing', 1, 'name@email.com', '1112223333'), ('Monica', 'Geller', 2, 'name@email.com', '4445556666');
```

## setting up linux devbox
#### - create non root user, add user to sudoers group

#### - no password sudo

#### - install vim, node, mariadb

