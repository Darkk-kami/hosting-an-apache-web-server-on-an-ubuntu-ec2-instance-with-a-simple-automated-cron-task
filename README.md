# Documentation

## Task 1: Set Up a Linux EC2 Instance

### Launch an EC2 Instance with Ubuntu:

1. Log in to the AWS Management Console.
2. Navigate to EC2 and launch a new instance.
3. Choose an Ubuntu AMI (Amazon Machine Image) and follow the wizard to configure the instance.
4. Configure Security groups to allow ssh and http access into the ec2 instance

## Task 2: Linux and user configuration

### User Management:
SSH into your ec2 instance

- Update package lists: `sudo apt update`
- Upgrade package lists: `sudo apt upgrade`
- Create a new user: `sudo adduser apache`

- Install Apache `sudo apt install apache2`
- Change ownership of directory to the created user: `sudo chown -R apache:apache /var/www/html`
- Change permission to allow the user have appropriate read and execute permissions `sudo chmod -R 755 /var/www/html`
- Update the apache config file: `sudo vim /etc/apache2/apache2.conf`
  ```bash
  User apache
  Group apache
- Restart apache to make changes: `sudo systemctl restart apache2`

### Creating a hello world script:
- create an index.html file: `vim index.html`
  
  ```bash
  <html>
    <head>
        <title>TEST WEBSITE</title>
    </head>
    <body>
        <p>Hello World</p>
    </body>
  </html>
- replace the default index.html in the apache directory: `mv index.html /var/www/html`
  
### Bash Script:
- Create `user_report.sh`:
  
  ```bash
  #!/bin/bash
  USERS=$(getent passwd | grep --color=never -E '\b(1[0-9]{3})\b'| awk -F: '{print $1}')
  DISK=$(df -h / | awk 'NR==2 {print $4}')

  echo "the total number of users in the system are: $(echo $USERS | wc -w)"
  echo "$USERS"
  echo "Free disk space: $DISK"
- Make the script executable: `chmod +x user_report.sh`
- run script `./user_report.sh`
- n.b this script gives the total number of users in the system along with the total free availabe disk space

### Automated task with cron:
- Open cron tab to update file: `crontab -e`
- input the following: ` 0 */12 * * * sudo apt update && sudo upgrade -y`
- this task tells cron to run the command every 12hrs

### Additional Notes:
To access your website:
- login to the aws console and copy your ec2 public address: `http//:your-ip-address`
Always ensure to follow AWS best practices and security guidelines.



