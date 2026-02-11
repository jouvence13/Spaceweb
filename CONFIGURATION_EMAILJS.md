# Configuration EmailJS et WhatsApp

## 🚀 Fonctionnalités ajoutées

✅ **Système d'email automatique** - Les visiteurs reçoivent une confirmation automatique + vous recevez une notification
✅ **Bouton WhatsApp flottant** - En bas à droite pour contact direct

---

## 📧 Configuration EmailJS (Gratuit)

### Étape 1 : Créer un compte EmailJS

1. Allez sur [https://www.emailjs.com/](https://www.emailjs.com/)
2. Cliquez sur "Sign Up" et créez un compte gratuit
3. Connectez-vous à votre dashboard

### Étape 2 : Ajouter un service email

1. Dans le dashboard, cliquez sur "Email Services"
2. Cliquez sur "Add New Service"
3. Choisissez votre fournisseur (Gmail recommandé)
4. Suivez les instructions pour connecter votre email
5. **Notez votre SERVICE_ID** (ex: `service_abc123`)

### Étape 3 : Créer un template pour VOUS (notification)

1. Allez dans "Email Templates"
2. Cliquez sur "Create New Template"
3. Nommez-le "Portfolio Contact Notification"
4. **Template ID** : Notez-le (ex: `template_xyz789`)

**Contenu du template pour VOUS :**

```
Subject: 📩 Nouveau message de {{from_name}}

Bonjour,

Vous avez reçu un nouveau message depuis votre portfolio :

Nom: {{from_name}}
Email: {{from_email}}

Message:
{{message}}

---
Répondez rapidement à {{reply_to}}
```

**Variables à utiliser :**
- `{{from_name}}` - Nom du visiteur
- `{{from_email}}` - Email du visiteur  
- `{{message}}` - Message du visiteur
- `{{reply_to}}` - Email de réponse

### Étape 4 : Créer un template pour le VISITEUR (auto-réponse)

1. Créez un second template "Portfolio Auto Reply"
2. **Template ID** : Notez-le (ex: `template_reply123`)

**Contenu du template pour le visiteur :**

```
Subject: ✅ Message bien reçu - AKODE Jouvence

Bonjour {{from_name}},

Merci pour votre message ! Je l'ai bien reçu et je vous répondrai dans les plus brefs délais (généralement sous 24-48h).

Voici un récapitulatif de votre message :
{{message}}

À très bientôt,
Jouvence AKODE
Développeur Web
spaceweb1997@gmail.com

---
Ce message est automatique, merci de ne pas y répondre.
```

### Étape 5 : Configurer dans le code

1. Ouvrez `index.html`
2. Trouvez cette ligne (ligne ~13) :
```javascript
publicKey: "VOTRE_PUBLIC_KEY", // À remplacer
```

3. Remplacez par votre **Public Key** (trouvée dans Account > API Keys)

4. Ouvrez `script.js`
5. Trouvez ces lignes (autour de la ligne 193) :
```javascript
await emailjs.send(
  'service_nd0r86q',  // SERVICE_ID d'étape 2
  'VOTRE_TEMPLATE_ID', // TEMPLATE_ID d'étape 3
  templateParams
);
```

### Étape 6 : Envoyer DEUX emails (notification + auto-réponse)

Pour envoyer 2 emails (un pour vous, un pour le visiteur), modifiez le code dans `script.js` :

```javascript
try {
  // Email 1 : Notification pour VOUS
  await emailjs.send(
    'VOTRE_SERVICE_ID',
    'VOTRE_TEMPLATE_NOTIFICATION_ID', // Template pour vous
    {
      from_name: name,
      from_email: email,
      message: message,
      reply_to: email
    }
  );

  // Email 2 : Auto-réponse pour le VISITEUR
  await emailjs.send(
    'VOTRE_SERVICE_ID',
    'VOTRE_TEMPLATE_AUTOREPLY_ID', // Template pour le visiteur
    {
      from_name: name,
      to_email: email, // Email du visiteur
      message: message
    }
  );

  showToast("✓ Message envoyé ! Je vous réponds rapidement.");
  form.reset();
} catch (error) {
  console.error('Erreur EmailJS:', error);
  showToast("⚠ Erreur lors de l'envoi. Réessayez ou contactez-moi directement.");
}
```

**Important pour l'auto-réponse :**
- Dans le template "Auto Reply" sur EmailJS
- Configurez le champ "To Email" avec : `{{to_email}}`
- Cela enverra l'email au visiteur

---

## 💬 Configuration WhatsApp

### Étape 1 : Obtenir votre lien WhatsApp

1. Votre numéro doit être au format international
   - Exemple France : `33612345678` (sans le +)
   - Enlevez le 0 initial et ajoutez l'indicatif pays

2. Ouvrez `index.html`
3. Trouvez cette ligne (vers la ligne 425) :
```html
<a href="https://wa.me/VOTRE_NUMERO" class="whatsapp-btn"
```

4. Remplacez `VOTRE_NUMERO` par votre numéro
   - Exemple : `https://wa.me/33612345678`

### Étape 2 : Ajouter un message pré-rempli (optionnel)

```html
<a href="https://wa.me/33612345678?text=Bonjour%20Jouvence,%20je%20vous%20contacte%20depuis%20votre%20portfolio" class="whatsapp-btn"
```

---

## ✅ Test de fonctionnement

### Tester EmailJS

1. Ouvrez votre site en local ou en ligne
2. Remplissez le formulaire de contact
3. Vérifiez :
   - ✉️ Vous recevez une notification sur votre email
   - ✉️ Le visiteur reçoit une auto-réponse
   - Console du navigateur (F12) : aucune erreur

### Tester WhatsApp

1. Cliquez sur le bouton vert en bas à droite
2. Vérifiez que WhatsApp s'ouvre avec votre numéro

---

## 🎨 Personnalisation

### Changer la couleur du bouton WhatsApp

Dans `style.css`, modifiez :
```css
.whatsapp-btn {
  background: linear-gradient(135deg, #25d366, #128c7e); /* Couleur WhatsApp */
}
```

### Changer la position du bouton

```css
.whatsapp-btn {
  bottom: 2rem; /* Distance du bas */
  right: 2rem;  /* Distance de la droite */
}
```

### Désactiver l'animation pulse

Supprimez cette ligne dans `style.css` :
```css
animation: whatsapp-pulse 2s infinite;
```

---

## 🚨 Limites gratuites EmailJS

- **200 emails/mois** (gratuit)
- Si dépassé : upgrader vers plan payant ou utiliser un autre service
- Alternative : FormSubmit, Formspree, ou backend Node.js

---

## 📱 Support

Si vous avez des questions :
- Email : spaceweb1997@gmail.com
- WhatsApp : (votre numéro)

---

**Dernière mise à jour : 10 février 2026**
