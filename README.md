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
