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

### Ajouter les codes dans le fichier send-emails            
Ajouter sur la racine de votre projet le fichier **send-emails.js** avec le contenu suivant:

```.js
const nodemailer = require('nodemailer');

// Create a transporter
const transporter = nodemailer.createTransport({
    service: 'gmail',
    port: 587,
    auth: {
      user: 'jean-admin@gmail.com',
      pass: 'xxxxYYYYxxxxYYYY', // generate app password using https://myaccount.google.com/apppasswords
    },
    tls: {
      rejectUnauthorized: false,
    },
});

// Constructing the Email Message
const mailOptions = {
    from: 'jean-admin@gmail.com', // Sender's email address
    to: 'jean-admin@gmail.com', // Receiver's email address
    bcc: ['example1@gmail.com', 'example2@gmail.com'],
    subject: `C'est mon anniversaire !`,
    text: `Je fête mes 8 ans !.`,
    html: `
<p>C'est mon <strong>anniversaire</strong> !. <br />
Je fête mes 8 ans !<br />
Je t’invite pour m’aider à souffler ma 8ᵉ bougie, le samedi 2 mars !<br />
Si tu es d’accord, rendez-vous chez moi à partir de 14 heures ! <br />
Ensuite, il y aura un super goûter, des bonbons à volonté et même une chasse au trésor !<br />
J’espère que tu viendras !  <br />
Louise
</p>`,
    attachments: [
        {
            filename: 'louise.txt',
            content: 'C est mon anniversaire ...',
        },
        {
            filename: 'louise.pdf',
            path: 'c:/pdfs/louise.pdf',
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

### Completer la configuration     
Remplacez ensuite l'email et le mot de passe dans le transporteur par vos propres identifiants Gmail.     
De plus, remplacez les adresses email des destinataires dans mailOptions par celles que vous souhaitez utiliser.   

### Envoyer les emails
Une fois que vous avez configuré votre fichier send-emails.js, vous pouvez l'exécuter pour envoyer les emails.

```.bash
node send-emails.js
```

### Personaliser l'envoi
Notre ami souhaite personnaliser le contenu des e-mails en ajoutant le prénom du destinataire et une touche personnalisée. 
Il dispose d'une liste d'adresses e-mail auxquelles il souhaite envoyer ces e-mails.

Le fichier "send-emails" aura le contenu suivant :
```.js
const nodemailer = require('nodemailer');

function sendEmail(userName, userEmail) {
    return new Promise((resolve, reject) => {
        const transporter = nodemailer.createTransport({
            service: 'gmail',
            port: 587,
            auth: {
            user: 'jean-admin@gmail.com',
            pass: 'password', // generate app password using https://myaccount.google.com/apppasswords
            },
            tls: {
            rejectUnauthorized: false,
            },
        });
 
        const mailOptions = {
            from: 'jean-admin@gmail.com', // Sender's email address
            to: 'jean-admin@gmail.com', // Receiver's email address
            bcc: [`${userEmail}`],
            subject: `Salut ${userName} C'est mon anniversaire !`,
            text: `Je fête mes 8 ans !.`,
            html: `
        <p>Salut ${userName} C'est mon anniversaire ! <br />
        Salut ${userName}
        Je fête mes 8 ans !<br />
        Je t’invite pour m’aider à souffler ma 8ᵉ bougie, le samedi 2 mars !<br />
        Si tu es d’accord, rendez-vous chez moi à partir de 14 heures ! <br />
        Ensuite, il y aura un super goûter, des bonbons à volonté et même une chasse au trésor !<br />
        J’espère que tu viendras !  <br />
        Louise
        </p>`,
            attachments: [
                {
                    filename: 'louise.txt',
                    content: 'C est mon anniversaire ...',
                },
                {
                    filename: 'louise.pdf',
                    path: 'c:/pdfs/louise.pdf',
                },
            ],
        };

        // Sending the Email
        transporter
            .sendMail(mailOptions)
            .then((info) => {
                console.log('Emails envoyés:', info.response);
                return resolve(info.response);
            })
            .catch((error) => {
                console.error('Erreurs rencontrés durant l envoi:',error);
                return resolve(error);
            });        
    })
}


// Liste des utilisateurs
const userList = [
    {"name": "Jean", "email": "jean@example.com"},
    {"name": "Nathalie", "email": "nathalie@example.com"}
];

// Boucle pour envoyer des emails à tous les utilisateurs de la liste
async function sendEmails() {
    for (var i = 0; i < userList.length; i++) {
        var user = userList[i];
        try {
            await sendEmail(user.name, user.email);
        } catch (error) {
            console.error("Erreur lors de l'envoi de l'email :", error.message);
        }
    }
}

sendEmails()
    .then(() => {
        console.log("Tous les emails ont été envoyés avec succès.");
    })
    .catch((error) => {
        console.error("Une erreur s'est produite :", error.message);
    });

```
