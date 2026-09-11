---
title: "Setup data for Database"
date: "2024-01-01"
weight: 4
chapter: false
pre: "<strong>2.4. </strong>"
---

Retrieve the **Public IP address** of the EC2 instance.

![Image](/images/2-preparation/2.4-data-for-db/2.4.1.png?featherlight=false&width=90pc)

Use **MobaXterm** to connect to the instance via SSH on port 22:

- Select **Session**
- Select **SSH**
- For **Remote host**, enter the **Public IPv4 address** retrieved from the instance
- For **Specify username**, nhập **ec2-user**
- Verify port 22
- Select **Advanced SSH settings**
- Select **Use private key** and select **keypair** of instance.
- Click **OK**

![Image](/images/2-preparation/2.4-data-for-db/2.4.2.png?featherlight=false&width=90pc)

The result after SSH.

![Image](/images/2-preparation/2.4-data-for-db/2.4.3.png?featherlight=false&width=90pc)

We use Git to clone the source code. First, install Git using the following command:

```bash
sudo yum install git
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.4.png?featherlight=false&width=90pc)

Install MySQL command-line client

```bash
sudo dnf install mariadb105
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.5.png?featherlight=false&width=90pc)

Check if the installation was successful.

```bash
mysql --version
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.6.png?featherlight=false&width=90pc)

Connect to the MySQL command-line client (unencrypted)

- For the **-h** parameter, replace it with the DNS name (endpoint) of the DB instance. You can find the DNS name in the detail console of the RDS you created.
- For the **-P** parameter, replace it with the port for the DB instance (3306).
- For the **-u** parameter, replace it with the master user you set when creating the RDS.
- After running the command, enter the master user password that you set during the creation of the RDS.

```bash
mysql -h fcj-management-db-instance.cdysiiecu90g.ap-southeast-1.rds.amzonaws.com -P 3306 -u admin -p
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.7.png?featherlight=false&width=90pc)

Successfully connected to the DB instance. Proceed to check the databases within the instance using the command, which will display a list of all databases.

```sql
SHOW DATABASES;
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.8.png?featherlight=false&width=90pc)

Select the database to make changes by using the USE command; use the initial database that you created when setting up the RDS.

```sql
USE "name of database";
```

Create a table in the awsuser database using the CREATE TABLE command.

```sql
CREATE TABLE `awsfcjuser`.`user` ( `id` INT NOT NULL AUTO_INCREMENT , `first_name` VARCHAR(45) NOT NULL , `last_name` VARCHAR(45) NOT NULL , `email` VARCHAR(45) NOT NULL , `phone` VARCHAR(45) NOT NULL , `comments` TEXT NOT NULL , `status` VARCHAR(10) NOT NULL DEFAULT 'active' , PRIMARY KEY (`id`)) ENGINE = InnoDB;
```

Insert information into the table using the **INSERT INTO** command

```sql
INSERT INTO `user`
(`id`, `first_name`,  `last_name`,    `email`,                 `phone`,         `comments`, `status`) VALUES
(NULL, 'Amanda',      'Nunes',        'anunes@ufc.com',        '012345 678910', '',          'active'),
(NULL, 'Alexander',   'Volkanovski',  'avolkanovski@ufc.com',  '012345 678910', '',          'active'),
(NULL, 'Khabib',      'Nurmagomedov', 'knurmagomedov@ufc.com', '012345 678910', '',          'active'),
(NULL, 'Kamaru',      'Usman',        'kusman@ufc.com',        '012345 678910', '',          'active'),
(NULL, 'Israel',      'Adesanya',     'iadesanya@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Henry',       'Cejudo',       'hcejudo@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Valentina',   'Shevchenko',   'vshevchenko@ufc.com',   '012345 678910', '',          'active'),
(NULL, 'Tyron',       'Woodley',      'twoodley@ufc.com',      '012345 678910', '',          'active'),
(NULL, 'Rose',        'Namajunas ',   'rnamajunas@ufc.com',    '012345 678910', '',          'active'),
(NULL, 'Tony',        'Ferguson ',    'tferguson@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Jorge',       'Masvidal ',    'jmasvidal@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Nate',        'Diaz ',        'ndiaz@ufc.com',         '012345 678910', '',          'active'),
(NULL, 'Conor',       'McGregor ',    'cmcGregor@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Cris',        'Cyborg ',      'ccyborg@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Tecia',       'Torres ',      'ttorres@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Ronda',       'Rousey ',      'rrousey@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Holly',       'Holm ',        'hholm@ufc.com',         '012345 678910', '',          'active'),
(NULL, 'Joanna',      'Jedrzejczyk ', 'jjedrzejczyk@ufc.com',  '012345 678910', '',          'active');
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.9.png?featherlight=false&width=90pc)

Use the SELECT command to display the table.

```sql
SELECT * FROM user;
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.10.png?featherlight=false&width=90pc)

Use exit to leave. If you are unable to disconnect from the DB instance, use the key combination Ctrl+C.
