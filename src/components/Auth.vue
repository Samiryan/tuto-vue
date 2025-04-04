<template>
    <div class="auth-container">
      <input v-model="email" placeholder="Email" />
      <input v-model="password" type="password" placeholder="Mot de passe" />
      <button @click="inscription">S'inscrire</button>
      <button @click="connexion">Se connecter</button>
      <button @click="deconnexion">Se déconnecter</button>
    </div>
    <div>
    <Auth />
    <ToDo />
  </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import { supabase } from '../supabase';

  import { Auth } from './components/Auth.vue';
  import { ToDo } from './components/ToDo.vue';

  
export default {
    setup() {
      const email = ref('');
      const password = ref('');
  
      async function inscription() {
        const { user, error } = await supabase.auth.signUp({ email: email.value, password: password.value });
        if (error) console.error(error);
        else console.log('Utilisateur inscrit :', user);
      }
  
      async function connexion() {
        const { user, error } = await supabase.auth.signInWithPassword({ email: email.value, password: password.value });
        if (error) console.error(error);
        else console.log('Utilisateur connecté :', user);
      }
  
      async function deconnexion() {
        await supabase.auth.signOut();
        console.log('Utilisateur déconnecté');
      }
  
      return { email, password, inscription, connexion, deconnexion };
    }
    
  };
  components: { Auth, ToDo }
  
  </script>
  
  <style>
  .auth-container {
    text-align: center;
    margin: 20px;
  }
  </style>
  