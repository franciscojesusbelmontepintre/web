---
layout: post
title: Alternativa a DDNS
permalink: /notes/others/ddns-alternative/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Se trata de un Bash Script que genera un Dockerfile, el cual describe los pasos para montar un contenedor bajo Ubuntu Linux, que ejecuta un script cada minuto que comprueba cual es la IP pública actual, y la compara con 
la que se obtuvo en la iteracción anterior. Si son iguales no se hace nada, pero si son distintas se envía un email con la nueva IP pública.

El script requiere que el usuario introduzca un email de origen, un email de destino, y una contraseña de aplicación del email origen para poder configurar el SSMTP y poder enviar emails.

```bash
#!/bin/bash

read -p "Enter a target email: " targetEmail
read -p "Enter a source gmail: " sourceGmail
read -p "Enter your source gmail app password: " appPassword
#echo "You entered $targetEmail"
#echo "You entered $sourceGmail"
#echo "You entered $appPassword"

printf "FROM ubuntu:latest \
\n \
\n#Instalamos sudo, wget, ssmtp, vim y cron. Todo con opción -y para que acepte todo que sí. \
\nRUN apt-get update && apt-get -y install sudo \
\nRUN sudo apt-get -y install wget \
\nRUN sudo apt-get -y install ssmtp \
\nRUN sudo apt-get -y install vim \
\nRUN sudo apt-get -y install cron \
\n \
\n#Configuramos el ssmtp con nuestros datos. Entre otros, hay que insertar el email y contraseña del correo gmail que va a enviar avisos \
\nRUN printf \"# \\ \
\n\\\\n# Config file for sSMTP sendmail \\ \
\n\\\\n# \\ \
\n\\\\n# The person who gets all mail for userids < 1000 \\ \
\n\\\\n# Make this empty to disable rewriting. \\ \
\n\\\\nroot=$sourceGmail \\ \
\n\\\\n# The place where the mail goes. The actual machine name is required no \\ \
\n\\\\n# MX records are consulted. Commonly mailhosts are named mail.domain.com \\ \
\n\\\\nmailhub=smtp.gmail.com:587 \\ \
\n\\\\n# Where will the mail seem to come from? \\ \
\n\\\\n#rewriteDomain= \\ \
\n\\\\n# The full hostname \\ \
\n\\\\n# hostname=c833370c3aaf \\ \
\n\\\\n# Are users allowed to set their own From: address? \\ \
\n\\\\n# YES - Allow the user to specify their own From: address \\ \
\n\\\\n# NO - Use the system generated From: address \\ \
\n\\\\nFromLineOverride=YES \\ \
\n\\\\nAuthUser=$sourceGmail \\ \
\n\\\\nAuthPass=$appPassword \\ \
\n\\\\n#UseTLS=YES \\ \
\n\\\\nUseSTARTTLS=Yes\\\\n\" > /etc/ssmtp/ssmtp.conf \
\n \
\nRUN printf \"old_ip\" > /ip.txt \
\n \
\n#Script que compara la actual ip pública con la de la iteracción anterior. Si difieren, se avisa al email de hotmail. \
\nRUN printf \"#!/bin/bash \\ \
\n\\\\ncurrent_ip=\\\$(wget -qO- https://ipecho.net/plain) \\ \
\n\\\\nold_ip=\\\$(cat ip.txt) \\ \
\n\\\\necho \\\\\"current_ip:\\\\\" \\ \
\n\\\\necho \\\$current_ip \\ \
\n\\\\necho \\\\\"old_ip:\\\\\" \\ \
\n\\\\necho \\\$old_ip \\ \
\n\\\\nif [[ \\\\\"\\\$current_ip\\\\\" != \\\\\"\\\$old_ip\\\\\" ]]; then \\ \
\n\\\\n  echo \\\\\"mismatch\\\\\" \\ \
\n\\\\n  echo \\\\\"\\\$current_ip\\\\\" > ip.txt \\ \
\n\\\\n  echo \\\\\"sending email\\\\\" \\ \
\n\\\\n  echo \\\\\"\\\$current_ip\\\\\" | /usr/sbin/ssmtp $targetEmail \\ \
\n\\\\nelse \\ \
\n\\\\n  echo \\\\\"match\\\\\" \\ \
\n\\\\n  echo \\\\\"do nothing\\\\\" \\ \
\n\\\\nfi \\ \
\n\\\\necho \\\\\"-----------------------------------------------------------\\\\\"\\\\n\" > ip.sh \
\n \
\n#RUN chmod +rxw ip.sh \
\n \
\n#Cron fails silently if you forget. \
\nRUN chmod 0744 ip.sh \
\n \
\n#Cron programado para que ejecute el script cada minuto. \
\nRUN printf \"* * * * * /ip.sh  >> /var/log/cron.log 2>&1 \\ \
\n\\\\n# An empty line is required at the end of this file for a valid cron file.\" > /etc/cron.d/ip-cron \
\n \
\n# Give execution rights on the cron job \
\nRUN chmod 0644 /etc/cron.d/ip-cron \
\n \
\n# Apply cron job \
\nRUN crontab /etc/cron.d/ip-cron \
\n \
\n# Create the log file to be able to run tail \
\nRUN touch /var/log/cron.log \
\n \
\n# Run the command on container startup \
\nCMD cron && tail -f /var/log/cron.log"> /c/Users/FranPortatil/desktop/Dockerfile
``` 

El Dockerfile generado para los siguientes valores: 
- targetEmail: target@mail.com
- sourceGmail: source@gmail.com
- pass: qwerty

```bash
FROM ubuntu:latest

#Instalamos sudo, wget, ssmtp, vim y cron. Todo con opción -y para que acepte todo que sí.
RUN apt-get update && apt-get -y install sudo
RUN sudo apt-get -y install wget
RUN sudo apt-get -y install ssmtp
RUN sudo apt-get -y install vim
RUN sudo apt-get -y install cron

#Configuramos el ssmtp con nuestros datos. Entre otros, hay que insertar el email y contraseña del correo gmail que va a enviar avisos
RUN printf "# \
\n# Config file for sSMTP sendmail \
\n# \
\n# The person who gets all mail for userids < 1000 \
\n# Make this empty to disable rewriting. \
\nroot=source@gmail.com \
\n# The place where the mail goes. The actual machine name is required no \
\n# MX records are consulted. Commonly mailhosts are named mail.domain.com \
\nmailhub=smtp.gmail.com:587 \
\n# Where will the mail seem to come from? \
\n#rewriteDomain= \
\n# The full hostname \
\n# hostname=c833370c3aaf \
\n# Are users allowed to set their own From: address? \
\n# YES - Allow the user to specify their own From: address \
\n# NO - Use the system generated From: address \
\nFromLineOverride=YES \
\nAuthUser=source@gmail.com \
\nAuthPass=qwerty \
\n#UseTLS=YES \
\nUseSTARTTLS=Yes\n" > /etc/ssmtp/ssmtp.conf

RUN printf "old_ip" > /ip.txt

#Script que compara la actual ip pública con la de la iteracción anterior. Si difieren, se avisa al email de hotmail.
RUN printf "#!/bin/bash \
\ncurrent_ip=\$(wget -qO- https://ipecho.net/plain) \
\nold_ip=\$(cat ip.txt) \
\necho \"current_ip:\" \
\necho \$current_ip \
\necho \"old_ip:\" \
\necho \$old_ip \
\nif [[ \"\$current_ip\" != \"\$old_ip\" ]]; then \
\n  echo \"mismatch\" \
\n  echo \"\$current_ip\" > ip.txt \
\n  echo \"sending email\" \
\n  echo \"\$current_ip\" | /usr/sbin/ssmtp target@mail.com \
\nelse \
\n  echo \"match\" \
\n  echo \"do nothing\" \
\nfi \
\necho \"-----------------------------------------------------------\"\n" > ip.sh

#RUN chmod +rxw ip.sh

#Cron fails silently if you forget.
RUN chmod 0744 ip.sh

#Cron programado para que ejecute el script cada minuto.
RUN printf "* * * * * /ip.sh  >> /var/log/cron.log 2>&1 \
\n# An empty line is required at the end of this file for a valid cron file." > /etc/cron.d/ip-cron

# Give execution rights on the cron job
RUN chmod 0644 /etc/cron.d/ip-cron

# Apply cron job
RUN crontab /etc/cron.d/ip-cron

# Create the log file to be able to run tail
RUN touch /var/log/cron.log
 
# Run the command on container startup
CMD cron && tail -f /var/log/cron.log
``` 

Construimos la imagen y levantamos el contenedor:

```console
C:\Users\FranPortatil\Desktop>docker build -t get_public_ip .
[+] Building 0.1s (18/18) FINISHED
 => [internal] load build definition from Dockerfile                                                               0.0s
 => => transferring dockerfile: 2.62kB                                                                             0.0s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                   0.0s
 => [ 1/14] FROM docker.io/library/ubuntu:latest                                                                   0.0s
 => CACHED [ 2/14] RUN apt-get update && apt-get -y install sudo                                                   0.0s
 => CACHED [ 3/14] RUN sudo apt-get -y install wget                                                                0.0s
 => CACHED [ 4/14] RUN sudo apt-get -y install ssmtp                                                               0.0s
 => CACHED [ 5/14] RUN sudo apt-get -y install vim                                                                 0.0s
 => CACHED [ 6/14] RUN sudo apt-get -y install cron                                                                0.0s
 => CACHED [ 7/14] RUN printf "# \n# Config file for sSMTP sendmail \n# \n# The person who gets all mail for user  0.0s
 => CACHED [ 8/14] RUN printf "old_ip" > /ip.txt                                                                   0.0s
 => CACHED [ 9/14] RUN printf "#!/bin/bash \ncurrent_ip=$(wget -qO- https://ipecho.net/plain) \nold_ip=$(cat ip.t  0.0s
 => CACHED [10/14] RUN chmod 0744 ip.sh                                                                            0.0s
 => CACHED [11/14] RUN printf "* * * * * /ip.sh  >> /var/log/cron.log 2>&1 \n# An empty line is required at the e  0.0s
 => CACHED [12/14] RUN chmod 0644 /etc/cron.d/ip-cron                                                              0.0s
 => CACHED [13/14] RUN crontab /etc/cron.d/ip-cron                                                                 0.0s
 => CACHED [14/14] RUN touch /var/log/cron.log                                                                     0.0s
 => exporting to image                                                                                             0.0s
 => => exporting layers                                                                                            0.0s
 => => writing image sha256:bac606966437d915b8063b4e738cee36fffe0ffec30f9a6c7204b10ac229ba48                       0.0s
 => => naming to docker.io/library/get_public_ip                                                                   0.0s
C:\Users\FranPortatil\Desktop>docker run -it get_public_ip
cat: ip.txt: No such file or directory
current_ip:
156.146.62.138
old_ip:

mismatch
sending email
-----------------------------------------------------------
current_ip:
156.146.62.138
old_ip:
156.146.62.138
match
do nothing
-----------------------------------------------------------
``` 