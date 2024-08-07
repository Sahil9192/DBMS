### SQL Installation Guide

#### MySQL Installation

**Windows:**

1. **Download MySQL Installer:**
   - Go to the [MySQL Downloads page](https://dev.mysql.com/downloads/installer/).
   - Download the MySQL Installer (choose the appropriate version for your system).

2. **Install MySQL:**
   - Run the downloaded installer.
   - Follow the setup wizard. Choose the setup type (Developer Default is a good start).
   - Configure MySQL Server by setting up the server type, port number, and root password.

3. **Verify Installation:**
   - Open a command prompt.
   - Type `mysql -u root -p` and enter the root password you set during installation.

**macOS:**

1. **Using Homebrew:**
   - Install Homebrew if you haven't already: 
     ```sh
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```
   - Install MySQL:
     ```sh
     brew install mysql
     ```

2. **Start MySQL:**
   - Start the MySQL service:
     ```sh
     brew services start mysql
     ```

3. **Verify Installation:**
   - Run MySQL:
     ```sh
     mysql -u root
     ```

**Linux (Ubuntu):**

1. **Install MySQL:**
   ```sh
   sudo apt update
   sudo apt install mysql-server
   ```

2. **Secure MySQL Installation:**
   ```sh
   sudo mysql_secure_installation
   ```

3. **Verify Installation:**
   ```sh
   sudo mysql
   ```

### NoSQL and MongoDB Installation Guide

#### MongoDB Installation

**Windows:**

1. **Download MongoDB:**
   - Go to the [MongoDB Download Center](https://www.mongodb.com/try/download/community).
   - Download the MSI package for Windows.

2. **Install MongoDB:**
   - Run the downloaded MSI package.
   - Follow the setup wizard, including the option to install MongoDB as a service.

3. **Verify Installation:**
   - Open a command prompt.
   - Type `mongo` to start the MongoDB shell.

**macOS:**

1. **Using Homebrew:**
   - Install MongoDB:
     ```sh
     brew tap mongodb/brew
     brew install mongodb-community
     ```

2. **Start MongoDB:**
   - Start the MongoDB service:
     ```sh
     brew services start mongodb/brew/mongodb-community
     ```

3. **Verify Installation:**
   - Run MongoDB shell:
     ```sh
     mongo
     ```

**Linux (Ubuntu):**

1. **Import the public key used by the package management system:**
   ```sh
   wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
   ```

2. **Create a list file for MongoDB:**
   ```sh
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
   ```

3. **Reload local package database:**
   ```sh
   sudo apt update
   ```

4. **Install MongoDB packages:**
   ```sh
   sudo apt install -y mongodb-org
   ```

5. **Start MongoDB:**
   ```sh
   sudo systemctl start mongod
   ```

6. **Verify Installation:**
   - Run MongoDB shell:
     ```sh
     mongo
     ```

With these steps, you should have both SQL (MySQL) and NoSQL (MongoDB) databases installed and ready to use on your system. If you encounter any issues or need further assistance with the installation, feel free to ask!
