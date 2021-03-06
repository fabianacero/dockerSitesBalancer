FROM 	centos:6.6

# INSTALACION APACHE
RUN 	yum install -y httpd 
RUN 	yum install -y mod_ssl

# INSTALACION PHP 5.4
RUN 	yum install -y centos-release-SCL 
RUN	yum clean all
RUN	yum install -y php54-php-bcmath php54 php54-php-cli php54-php-gd php54-php-ldap php54-php-mbstring php54-php-xmlrpc php54-php php54-php-pdo php54-php-pear php54-php-pecl-apc php54-php-mysqlnd php54-php-process php54-php-pgsql php54-php-soap
RUN	source /opt/rh/php54/enable
RUN	rm -fr /etc/httpd/conf.d/php.conf 
RUN	sed -i -- 's/\;date\.timezone\ =/date\.timezone\ = America\/Bogota/g' /opt/rh/php54/root/etc/php.ini
RUN	sed -i -- 's/short_open_tag \= Off/short_open_tag \= On/g' /opt/rh/php54/root/etc/php.ini
RUN	sed -i -- 's/\;error_log \= syslog/error_log \= \/var\/log\/httpd\/php_error/g' /opt/rh/php54/root/etc/php.ini

# DIRECTORIO TEMPORALES Y LOGS
RUN 	mkdir -p /var/www/files/web/file \
	/var/www/files/webhw/file \
	/var/log/httpd/logAplicacion \
	/etc/httpd/ssl \
	/tmp-www/web \
	/scripts 

RUN 	chgrp apache /var \
	/var/www \
	/var/www/files \
	/var/www/files/web \
	/var/www/files/web/file \
	/var/www/files/webhw \
	/var/www/files/webhw/file \
	/var/log \ 
	/var/log/httpd \ 
	/var/log/httpd/logAplicacion \
	/tmp-www \
	/tmp-www/web \
	/scripts
	

RUN 	chmod 775 /var/ \ 
	 /var/www/ \
	 /var/www/files/ \
	 /var/www/files/web/ \
	 /var/www/files/web/file \
	 /var/www/files/webhw/ \ 
	 /var/www/files/webhw/file \
	 /var/log/ \
	 /var/log/httpd/ \
	 /var/log/httpd/logAplicacion/ \
	 /tmp-www/ \ 
	 /tmp-www/web/ \
	 /scripts
	

# ADICION DE LLAVE PUBLIC Y PRIVATE Y CERTIFICADOS
ADD 	./sites/scripts/securep.decameron.com.crt /etc/httpd/ssl/securep.decameron.com.crt
ADD 	./sites/scripts/securep.decameron.com.key /etc/httpd/ssl/securep.decameron.com.key
ADD 	./sites/scripts/securep.decameron.comInternas.crt /etc/httpd/ssl/securep.decameron.comInternas.crt
ADD 	./sites/scripts/securep.decameron.comInternas.key /etc/httpd/ssl/securep.decameron.comInternas.key
ADD 	./sites/scripts/certificate.crt /etc/pki/tls/certs/certificate.crt
ADD 	./sites/scripts/certificate.csr /etc/pki/tls/private/certificate.csr
ADD 	./sites/scripts/certificate.key /etc/pki/tls/private/certificate.key

# ARCHIVOS PARA BALANCEAR WEB-APP
ADD ./sites/scripts/createBalancer.php /scripts/createBalancer.php
ADD	./sites/configuration/balancer.conf /scripts/balancer.conf
ENV	PATH=/opt/rh/php54/root/usr/bin:/opt/rh/php54/root/usr/sbin${PATH:+:${PATH}}
ENV	MANPATH=/opt/rh/php54/root/usr/share/man:${MANPATH}
RUN php /scripts/createBalancer.php

# INCORPORAR ARCHIVOS VHOSTS
ADD ./sites/configuration/httpd-default.conf /scripts/
ADD ./sites/configuration/httpd-vhosts-*.conf /etc/httpd/conf/

RUN 	cat /scripts/httpd-default.conf >> /etc/httpd/conf/httpd.conf
RUN 	echo "" >> /etc/httpd/conf/httpd.conf
RUN 	echo "Include conf/httpd-vhost*.conf" >> /etc/httpd/conf/httpd.conf

# INSTALACION DE IDIOMAS EN SO
RUN	localedef -c -i en_GB -f UTF-8 en_GB.UTF-8 
RUN	localedef -c -i es_CO -f UTF-8 es_CO.UTF-8
RUN	localedef -c -i fr_FR -f UTF-8 fr_FR.UTF-8
RUN	localedef -c -i pt_PT -f UTF-8 pt_PT.UTF-8 

EXPOSE 80
EXPOSE 443
CMD ["/bin/bash"]
