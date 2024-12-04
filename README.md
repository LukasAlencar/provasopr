### Passo a Passo: Configuração e Instalação no Ambiente Linux

---

Login: developer / ifsp@123

#### **Passo 1: Testar Conexão com a Internet**

1. Verificar se a máquina está conectada à internet:
   ```bash
   ping -c 4 4.2.2.2
   ping -c 4 8.8.8.8
   ```

2. Testar resolução de DNS:
   ```bash
   ping -c 4 www.google.com
   ```

   - **Se funcionar, vá para o passo 2.**
   - **Se não funcionar:**
     - Verifique o arquivo de configuração do DNS:
       ```bash
       cat /etc/resolv.conf
       ```
     - Se o arquivo não contiver algo como `nameserver 10.0.0.1`:
       ```bash
       su -
       ```
       Senha: **ifsp@2024**
       ```bash
       > /etc/resolv.conf
       echo nameserver 10.0.0.1 > /etc/resolv.conf
       exit
       ```

---

#### **Passo 2: Instalar o Visual Studio Code**

1. Baixe o arquivo `.deb` do **VS Code** no site oficial usando o navegador.
2. No terminal:
   ```bash
   cd ~/Downloads
   sudo apt update
   sudo dpkg -i code<tab>
   ```

---

#### **Passo 3: Instalar Apache, PHP, MariaDB e PHPMyAdmin**

##### **1. Instalar o Apache**
```bash
sudo apt install -y apache2
ss -nltp
systemctl status apache2
```
- Se necessário, reinicie o serviço:
  ```bash
  sudo systemctl stop apache2
  sudo systemctl start apache2
  ```

##### **2. Instalar o PHP**
```bash
sudo apt install -y php
sudo nano /etc/php/8.2/apache2/php.ini
```

##### **3. Testar o PHP**
1. Crie um arquivo no diretório do Apache:
   ```bash
   sudo nano /var/www/html/phpinfo.php
   ```
2. Insira o seguinte conteúdo:
   ```php
   <?php
   phpinfo();
   ?>
   ```
3. Salve e abra no navegador:  
   `http://localhost/phpinfo.php`

##### **4. Instalar o MariaDB**
1. Instale o servidor:
   ```bash
   sudo apt install -y mariadb-server
   sudo mysql_secure_installation
   ```
   Siga as instruções para configurar o banco
   Enter
   Enter
   Enter
   **ifsp@2024** **ifsp@2024**
   Enter
   Enter
   Enter
   Enter

3. Criar um usuário no MariaDB:
   ```bash
   sudo mysql -p
   ```
   Senha: **ifsp@2024**
   ```sql
   CREATE USER 'developer'@'localhost' IDENTIFIED BY 'ifsp@123';
   GRANT ALL PRIVILEGES ON *.* TO 'developer'@'localhost';
   FLUSH PRIVILEGES;
   exit;
   ```
```bash
mysql -p
ifsp@123
```
##### **5. Instalar o PHPMyAdmin**
```bash
sudo apt install -y phpmyadmin
```
- Durante a instalação, escolha **Apache2** e siga as instruções.
- Acesse no navegador:  
  `http://localhost/phpmyadmin`  
  Usuário: **developer**  
  Senha: **ifsp@123**

---

#### **Passo 4: Instalar Docker e Docker Compose**

##### **1. Instalar Docker**
```bash
sudo curl -fsSL https://get.docker.com | bash
docker --version
```

##### **2. Instalar Docker Compose**
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.30.3/docker-compose-linux-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

---

#### **Passo 5: Configurar Diretório do Apache**

1. Alterar permissões:
   ```bash
   sudo chgrp -R www-data /var/www
   sudo chmod 2755 -R /var/www/html
   ```
2. Criar e configurar diretório:
   ```bash
   cd /var/www/html
   mkdir dev
   sudo gpasswd -a developer www-data
   cd ..
   sudo chmod 2775 -R /var/www/html
   cd html
   sudo mkdir dev
   sudo chown developer /var/www/html/dev
   ```
3. Testar:
   ```bash
   cd dev
   echo test > test.txt
   rm test.txt
   ```

4. Criar link simbólico:
   ```bash
   ln -s /var/www/html/dev /home/developer/projetos
   ```
5. Clonar repo:
 ```bash
  cd
  cd projetos
  git clone https://github.com/flrobson77/shinemodas.git
 ```

---

#### **Passo 6: Configurar IP Estático**

1. Edite o arquivo de interfaces de rede:
   ```bash
   sudo nano /etc/network/interfaces
   ```
   - Atualize a linha do endereço IP conforme necessário.
2. Reinicie a máquina para aplicar as configurações.
