FROM nagios/nagios-core:latest

# Mettez à jour les packages et installez les dépendances nécessaires
RUN apt-get update \
    && apt-get install -y \
        apache2 \
        apache2-utils \
        vim \
    && rm -rf /var/lib/apt/lists/*

# Copiez votre configuration Nagios dans le conteneur
COPY ./nagios-config /usr/local/nagios/etc/

# Copiez vos plugins Nagios dans le conteneur
COPY ./nagios-plugins /usr/local/nagios/libexec/

# Exposez le port 80 pour accéder à l'interface web de Nagios
EXPOSE 80

# Démarrez Apache au lancement du conteneur
CMD ["/usr/sbin/apache2", "-D", "FOREGROUND"]

