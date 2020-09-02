# Amazon Alexa Review Application

- Built with Vue.js and Firebase Realtime Database
- Styled with SCSS and Buefy + Bulma
- Deployed with Firebase Hosting: https://reviewapp-a76f1.web.app/

## Part 2: Data Analysis

- Part 2 of the challenge can be found here:
  https://www.kaggle.com/alexisdavalos/dose-part-2-code-challenge-amazon-alexa-data

## Environment Variables

```
You must set up these environment variables for the application to function correctly

VUE_APP_FIREBASE_API_KEY
VUE_APP_FIREBASE_AUTH_DOMAIN
VUE_APP_FIREBASE_DATABASE_URL
VUE_APP_FIREBASE_PROJECT_ID
VUE_APP_FIREBASE_STORAGE_BUCKET
VUE_APP_FIREBASE_SENDER_ID
VUE_APP_APP_ID

```

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```

## Firebase Deployment Setup

### Install Firebase CLI Tools:

```
npm install -g firebase-tools
```

### Run Following Sequence:

```
firebase login

firebase init
(Note: Do not overwrite index.html if you already ran `npm run build`)

firebase deploy
```

### Result:

```
✔  Deploy complete!
Hosting URL: https://example-vueapp-2222.web.app
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
