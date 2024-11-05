### Cryptographie Symétrique
La cryptographie symétrique utilise une clé unique pour chiffrer et déchiffrer les données. Cela signifie que la même clé est partagée entre les deux parties. Elle est rapide et idéale pour des volumes de données élevés, mais elle pose des défis pour le partage sécurisé de la clé.

#### Exemple en ligne de commande (avec OpenSSL)  
![image](https://github.com/user-attachments/assets/8121792e-dcd7-4aad-9add-cb2619720086)  

Pour chiffrer un fichier en utilisant une clé symétrique :
```bash
openssl enc -aes-256-cbc -salt -in fichier.txt -out fichier_chiffre.txt -k "mot_de_passe"
```

Pour déchiffrer le fichier :
```bash
openssl enc -d -aes-256-cbc -in fichier_chiffre.txt -out fichier_dechiffre.txt -k "mot_de_passe"
```

Ici, l’algorithme de chiffrement utilisé est AES-256 en mode CBC.

### Cryptographie Asymétrique  

![images](https://github.com/user-attachments/assets/a92a12c6-d0d0-4f29-99b6-e26a536c3ddd)  


La cryptographie asymétrique utilise une paire de clés : une clé publique et une clé privée. La clé publique est utilisée pour chiffrer les données, et seule la clé privée correspondante peut les déchiffrer. Ce type de cryptographie est essentiel pour l'échange sécurisé de clés et la vérification d'identité (comme dans SSL/TLS).

#### Exemple en ligne de commande (avec OpenSSL)
Pour générer une paire de clés RSA :
```bash
openssl genpkey -algorithm RSA -out cle_privee.pem
openssl rsa -in cle_privee.pem -pubout -out cle_publique.pem
```

Pour chiffrer un fichier avec la clé publique :
```bash
openssl rsautl -encrypt -inkey cle_publique.pem -pubin -in fichier.txt -out fichier_chiffre.bin
```

Pour déchiffrer le fichier avec la clé privée :
```bash
openssl rsautl -decrypt -inkey cle_privee.pem -in fichier_chiffre.bin -out fichier_dechiffre.txt
```

### Création d'un Certificat SSL Auto-signé
Un certificat SSL auto-signé permet d'établir une connexion sécurisée sans l’intervention d’une autorité de certification (CA). C'est utile pour des serveurs de développement ou des tests internes.

#### Instructions en ligne de commande
1. **Générer une clé privée** :
   ```bash
   openssl genpkey -algorithm RSA -out cle_privee.pem -pkeyopt rsa_keygen_bits:2048
   ```

2. **Générer une demande de signature de certificat (CSR)** :
   ```bash
   openssl req -new -key cle_privee.pem -out demande.csr
   ```

3. **Générer le certificat auto-signé** :
   ```bash
   openssl x509 -req -days 365 -in demande.csr -signkey cle_privee.pem -out certificat.crt
   ```

Une fois généré, le fichier `certificat.crt` peut être utilisé pour configurer un serveur web, tel que Nginx ou Apache, avec SSL.  

Pour intégrer un certificat SSL dans Apache et sécuriser votre serveur avec HTTPS, suivez ces étapes :

1. **Placez le certificat SSL et la clé privée**  
   Assurez-vous que vous avez les fichiers suivants :
   - Le certificat SSL (généralement avec une extension `.crt` ou `.pem`)
   - La clé privée du certificat (souvent `.key`)
   - Un fichier de chaîne de certificats ou certificat intermédiaire si nécessaire (`ca.crt` ou similaire)

   Placez ces fichiers dans un dossier sécurisé sur votre serveur, par exemple : `/etc/ssl/`.

2. **Configurer Apache pour utiliser le certificat**  
   Ouvrez le fichier de configuration d'Apache pour votre site. Ce fichier est généralement situé dans `/etc/apache2/sites-available/` et s'appelle quelque chose comme `default-ssl.conf`. Sinon, vous pouvez éditer le fichier de configuration du site en question.

3. **Modifier le fichier de configuration SSL**  
   Dans le fichier, ajoutez ou modifiez les lignes suivantes :

   ```apache
   <VirtualHost *:443>
       ServerName votre_domaine.com

       SSLEngine on
       SSLCertificateFile /etc/ssl/votre_certificat.crt
       SSLCertificateKeyFile /etc/ssl/votre_cle.key
       SSLCertificateChainFile /etc/ssl/chaine_de_certificats.crt  # Si requis

       DocumentRoot /var/www/html

       # Autres configurations de votre site
   </VirtualHost>
   ```

   Remplacez `votre_domaine.com`, `votre_certificat.crt`, `votre_cle.key`, et `chaine_de_certificats.crt` par les chemins corrects vers vos fichiers.

4. **Activer le module SSL et le site sécurisé**  
   Si ce n'est pas déjà fait, activez le module SSL d'Apache et le site sécurisé avec les commandes suivantes :

   ```bash
   sudo a2enmod ssl
   sudo a2ensite default-ssl.conf
   ```

5. **Redémarrer Apache**  
   Redémarrez Apache pour appliquer les modifications :

   ```bash
   sudo systemctl restart apache2
   ```

6. **Vérification**  
   Accédez à votre site via `https://votre_domaine.com` pour vérifier que le certificat est bien installé.



#### Suite : Voir la fonction de hashage  
