# Envoyer des emails avec des attachements en utilisant Nodemailer et Gmail

L'envoi d'emails est souvent nécessaire dans les projets de marketing CTA ou pour envoyer des emails en masse pour des événements personnels ou professionnels.

## Créer le projet

Pour commencer, créez un nouveau répertoire pour votre projet et initialisez-le avec npm.     
Ensuite, installez la librairie Nodemailer.

```.bash
mkdir mass-emails
cd mass-emails
npm init -y
npm install nodemailer
```

### Ajouter un fichier send-emails    
Ajouter sur la racine de votre projet le fichier send-emails.js avec le contenu suivant:

```.js
const nodemailer = require('nodemailer');

// Create a transporter
const transporter = nodemailer.createTransport({
    service: 'gmail',
    port: 587,
    auth: {
      user: 'jeandaher1@gmail.com',
      pass: 'xxxxYYYYxxxxYYYY', // generate app password using https://myaccount.google.com/apppasswords
    },
    tls: {
      rejectUnauthorized: false,
    },
});

// Constructing the Email Message
const mailOptions = {
    from: 'jeandaher1@gmail.com', // Sender's email address
    to: 'jeandaher1@gmail.com', // Receiver's email address
    bcc: ['user1@gmail.com', 'user2@gmail.com'],
    subject: `C'est mon anniversaire !`,
    text: `Je fête mes 8 ans !.`,
    html: `<p>This is the <strong>HTML content</strong> of the email. <br />
Je fête mes 8 ans !<br />
Je t’invite pour m’aider à souffler ma 8ᵉ bougie, le samedi 2 mars !<br />
Si tu es d’accord, rendez-vous chez moi à partir de 14 heures ! <br />
Ensuite, il y aura un super goûter, des bonbons à volonté et même une chasse au trésor !<br />
J’espère que tu viendras !  <br />
Louise
	</p>',
    attachments: [
        {
            filename: 'louise.txt',
            content: 'Attachment content as a string or Buffer',
        },
    ],
};

// Sending the Email
transporter
    .sendMail(mailOptions)
    .then((info) => {
        console.log('Emails envoyés:', info.response);
    })
    .catch((error) => {
        console.error('Erreurs rencontrés durant l envoi:',error);
    });
```


### Générer un mot de passe pour le serveur Gmail

Pour utiliser Nodemailer avec Gmail, vous devez générer un mot de passe d'application. Vous pouvez le faire en suivant ce lien :

```.js
https://myaccount.google.com/apppasswords
```

### Personaliser l'envoi    
Remplacez ensuite l'email et le mot de passe dans le transporteur par vos propres identifiants Gmail.     
De plus, remplacez les adresses email des destinataires dans mailOptions par celles que vous souhaitez utiliser.   

### Envoyer les emails
Une fois que vous avez configuré votre fichier send-emails.js, vous pouvez l'exécuter pour envoyer les emails.

```.bash
node send-emails.js
```

Assurez-vous que votre application a bien accès à votre compte Gmail et que vous avez autorisé l'accès aux applications moins sécurisées si nécessaire.
