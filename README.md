# Software Development Environment Setup on Ubuntu

This README provides a step-by-step guide to set up a general software development environment on Ubuntu, including tools for web development (Apache, PHP, MySQL), Python, Node.js, Docker, and useful utilities like Visual Studio Code and terminal enhancements. The setup supports multiple programming languages and workflows.

---

## Prerequisites

- Ubuntu system (tested on 20.04 LTS and later)
- Sudo privileges
- Internet connection

---

## 1. Update System Packages

Update your package list to ensure compatibility:

```sh
sudo apt update && sudo apt upgrade -y
```

---

## 2. Install Essential Development Tools

### Install Build Essentials

Install core tools like compilers and libraries:

```sh
sudo apt install build-essential gcc g++ make -y
```

### Install Git

Set up Git for version control:

```sh
sudo apt install git -y
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify installation:

```sh
git --version
```

---

## 3. Install Apache, PHP, and MySQL

### Install Apache

```sh
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

Verify by visiting [http://localhost](http://localhost) in your browser (should display Apache default page).

### Install PHP

Install PHP with common extensions:

```sh
sudo apt install php php-mysql libapache2-mod-php php-cli php-common php-json php-mbstring php-xml php-zip -y
```

Test PHP by creating an info file:

```sh
sudo nano /var/www/html/info.php
```

Add:

```php
<?php
phpinfo();
?>
```

Save (Ctrl+X, Y, Enter) and visit [http://localhost/info.php](http://localhost/info.php).

### Install MySQL

```sh
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
```

Secure MySQL:

```sh
sudo mysql_secure_installation
```

Follow prompts to:

- Set a root password
- Remove anonymous users
- Disallow remote root login
- Remove test databases
- Reload privileges

Test MySQL:

```sh
mysql -u root -p
```

Exit with:

```sql
EXIT;
```

---

## 4. Install Python

Ensure Python and related tools are installed:

```sh
sudo apt install python3 python3-pip python3-venv -y
```

Install MySQL connector:

```sh
pip3 install mysql-connector-python
```

Verify installations:

```sh
python3 --version
pip3 --version
```

Set up a virtual environment:

```sh
python3 -m venv ~/myproject-venv
source ~/myproject-venv/bin/activate
pip install requests
deactivate
```

---

## 5. Install Node.js and npm

Install Node.js and npm:

```sh
sudo apt install nodejs npm -y
```

Install `n` to manage Node.js versions:

```sh
sudo npm install -g n
sudo n latest
```

Verify:

```sh
node --version
npm --version
```

---

## 6. Install Docker

Install Docker for containerized development:

```sh
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Add your user to the Docker group:

```sh
sudo usermod -aG docker $USER
```

**Log out and back in**, then verify:

```sh
docker --version
docker run hello-world
```

---

## 7. Install Visual Studio Code

Install VS Code via Snap:

```sh
sudo snap install --classic code
```

Launch:

```sh
code
```

**Recommended extensions:**

- Python (Microsoft)
- PHP Intelephense
- ESLint
- Docker
- GitLens

---

## 8. Install Additional Tools

### Curl and Wget

```sh
sudo apt install curl wget -y
```

### Postman

```sh
sudo snap install postman
```

### MySQL Workbench (Optional)

```sh
sudo apt install mysql-workbench -y
```

### Terminal Enhancements

Install zsh and Oh My Zsh:

```sh
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Set zsh as default:

```sh
chsh -s $(which zsh)
```

**Log out and back in.**

---

## 9. Set Up a Sample Project

### Create a Project Directory

```sh
sudo mkdir /var/www/html/myproject
sudo chown -R $USER:$USER /var/www/html/myproject
cd /var/www/html/myproject
```

### Create MySQL Database

Log in to MySQL:

```sh
mysql -u root -p
```

Run:

```sql
CREATE DATABASE myapp;
USE myapp;
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL
);
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
EXIT;
```

### Create PHP Script

Create `index.php`:

```sh
nano index.php
```

Add (replace `mypassword` with your MySQL root password):

```php
<?php
$conn = new mysqli("localhost", "root", "mypassword", "myapp");

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$result = $conn->query("SELECT * FROM users");
echo "<h1>Users List</h1>";
while ($row = $result->fetch_assoc()) {
    echo "ID: " . $row['id'] . " - Name: " . $row['name'] . " - Email: " . $row['email'] . "<br>";
}
$conn->close();
?>
```

### Create Python Script

Create `insert_data.py`:

```sh
nano insert_data.py
```

Add (replace `mypassword` with your MySQL root password):

```python
import mysql.connector

try:
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="mypassword",
        database="myapp"
    )
    cursor = connection.cursor()
    cursor.execute("INSERT INTO users (name, email) VALUES (%s, %s)", ("Jane Doe", "jane@example.com"))
    connection.commit()
    print("Data inserted successfully!")
    cursor.close()
    connection.close()
except mysql.connector.Error as err:
    print(f"Error: {err}")
```

Run:

```sh
python3 insert_data.py
```

Test the project at [http://localhost/myproject](http://localhost/myproject).

---

## 10. Set Up Node.js Example

Create a Node.js app:

```sh
mkdir node-app && cd node-app
npm init -y
npm install express
```

Create `server.js`:

```sh
nano server.js
```

Add:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
    res.send('Hello from Node.js!');
});

app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
});
```

Run:

```sh
node server.js
```

Visit [http://localhost:3000](http://localhost:3000).

---

## 11. Troubleshooting

- **Apache issues:**  
  ```sh
  sudo systemctl restart apache2
  ```

- **MySQL errors:**  
  Reset password:  
  ```sh
  sudo mysqladmin -u root password 'newpassword'
  ```

- **PHP not working:**  
  Ensure `libapache2-mod-php` is installed and restart Apache.

- **Docker permissions:**  
  Verify user is in Docker group.

- **Node.js issues:**  
  Use `sudo n stable` to switch versions.

---

## 12. Access Resources

- **Apache:** [http://localhost](http://localhost)
- **PHP Info:** [http://localhost/info.php](http://localhost/info.php)
- **Project:** [http://localhost/myproject](http://localhost/myproject)
- **Node.js:** [http://localhost:3000](http://localhost:3000)
- **MySQL CLI:**  
  ```sh
  mysql -u root -p
  ```
- **Docker:**  
  ```sh
  docker ps
  ```

---

This setup provides a versatile development environment for Ubuntu. For further assistance, consult official documentation or community forums.
